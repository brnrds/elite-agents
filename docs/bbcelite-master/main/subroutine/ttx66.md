---
title: "The TTX66 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ttx66.html"
---

[CHPR](chpr.md "Previous routine")[ZES1](zes1.md "Next routine")
    
    
           Name: TTX66                                                   [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the top part of the screen and draw a border box
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-ttx66)
     Variations: See [code variations](../../related/compare/main/subroutine/ttx66.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CHPR](chpr.md) calls TTX66
                 * [cls](cls.md) calls TTX66
                 * [TT18](tt18.md) calls TTX66
                 * [TTX66K](ttx66k.md) calls TTX66
                 * [DEATH](death.md) calls via BOX
    
    
    
    
    
    * * *
    
    
     Clear the top part of the screen (the space view) and draw a border box
     along the top and sides.
    
    
    
    * * *
    
    
     Other entry points:
    
       BOX                  Just draw the border box along the top and sides
    
    
    
    
    .TTX66
    
     LDX #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STX VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDX #&40               \ Set X to point to page &40, which is the start of the
                            \ screen memory at &4000
    
    .BOL1
    
     JSR ZES1               \ Call ZES1 to zero-fill the page in X, which will clear
                            \ half a character row
    
     INX                    \ Increment X to point to the next page in screen
                            \ memory
    
     CPX #&70               \ Loop back to keep clearing character rows until we
     BNE BOL1               \ have cleared up to &7000, which is where the dashboard
                            \ starts
    
    .BOX
    
     LDX #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STX VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA COL                \ Store the current colour on the stack, so we can
     PHA                    \ restore it once we have drawn the border
    
     LDA #%00001111         \ Set COL = %00001111 to act as a four-pixel yellow
     STA COL                \ character byte (i.e. set the line colour to yellow)
    
     LDY #1                 \ Move the text cursor to row 1
     STY YC
    
     STY XC                 \ Move the text cursor to column 1
    
    IF _SNG47
    
     LDX #0                 \ Set X1 = Y1 = Y2 = 0
     STX Y1
     STX Y2
     STX X1
    
     DEX                    \ Set X2 = 255
     STX X2
    
    ELIF _COMPACT
    
     STZ Y1                 \ Set X1 = Y1 = Y2 = 0
     STZ Y2
     STZ X1
    
     LDX #255               \ Set X2 = 255
     STX X2
    
    ENDIF
    
     JSR LOINQ              \ Draw a line from (X1, Y1) to (X2, Y2), so that's from
                            \ (0, 0) to (255, 0), along the very top of the screen
    
     LDA #2                 \ Set X1 = X2 = 2
     STA X1
     STA X2
    
     JSR BOS2               \ Call BOS2 below, which will call BOS1 twice, and then
     JSR BOS2               \ call BOS2 again, so we effectively do BOS1 four times,
                            \ decrementing X1 and X2 each time before calling LOIN,
                            \ so this whole loop-within-a-loop mind-bender ends up
                            \ drawing these four lines:
                            \
                            \   (1, 0)   to (1, 191)
                            \   (0, 0)   to (0, 191)
                            \   (255, 0) to (255, 191)
                            \   (254, 0) to (254, 191)
                            \
                            \ So that's a two-pixel wide vertical border along the
                            \ left edge of the upper part of the screen, and a
                            \ two-pixel wide vertical border along the right edge
    
     LDA COL                \ Set locations &4000 and &41F8 to the correct colour,
     STA &4000              \ as otherwise the top-left and top-right corners will
     STA &41F8              \ be black (as the lines overlap at the corners, and
                            \ the EOR logic used by LOINQ will otherwise make them
                            \ black)
    
     PLA                    \ Restore the original colour that we stored above
     STA COL
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    .BOS2
    
     JSR BOS1               \ Call BOS1 below and then fall through into it, which
                            \ ends up running BOS1 twice. This is all part of the
                            \ loop-the-loop border-drawing mind-bender explained
                            \ above
    
    .BOS1
    
     STZ Y1                 \ Set Y1 = 0
    
     LDA #2*Y-1             \ Set Y2 = 2 * #Y - 1. The constant #Y is 96, the
     STA Y2                 \ y-coordinate of the mid-point of the space view, so
                            \ this sets Y2 to 191, the y-coordinate of the bottom
                            \ pixel row of the space view
    
     DEC X1                 \ Decrement X1 and X2
     DEC X2
    
     JMP LOINQ              \ Draw a line from (X1, Y1) to (X2, Y2) and return from
                            \ the subroutine using a tail call
    

[X]

Label [BOL1](ttx66.md#bol1) is local to this routine

[X]

Label [BOS1](ttx66.md#bos1) is local to this routine

[X]

Label [BOS2](ttx66.md#bos2) is local to this routine

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Entry point [LOINQ](loinq_part_1_of_7.md#loinq) in subroutine [LOINQ (Part 1 of 7)](loinq_part_1_of_7.md) (category: Drawing lines)

Draw a one-segment line from (X1, Y1) to (X2, Y2)

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](../workspace/zp.md#x2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Subroutine [ZES1](zes1.md) (category: Utility routines)

Zero-fill the page whose number is in X

[CHPR](chpr.md "Previous routine")[ZES1](zes1.md "Next routine")
