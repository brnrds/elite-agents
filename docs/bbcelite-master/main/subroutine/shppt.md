---
title: "The SHPPT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/shppt.html"
---

[KTRAN](../variable/ktran.md "Previous routine")[LL5](ll5.md "Next routine")
    
    
           Name: SHPPT                                                   [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw a distant ship as a point rather than a full wireframe
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-shppt)
     Variations: See [code variations](../../related/compare/main/subroutine/shppt.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL9 (Part 2 of 12)](ll9_part_2_of_12.md) calls SHPPT
    
    
    
    
    
    
    .SHPPT
    
     JSR PROJ               \ Project the ship onto the screen, returning:
                            \
                            \   * K3(1 0) = the screen x-coordinate
                            \   * K4(1 0) = the screen y-coordinate
                            \   * A = K4+1
    
     ORA K3+1               \ If either of the high bytes of the screen coordinates
     BNE nono               \ are non-zero, jump to nono as the ship is off-screen
    
     LDA K4                 \ Set A = the y-coordinate of the dot
    
     CMP #Y*2-2             \ If the y-coordinate is bigger than the y-coordinate of
     BCS nono               \ the bottom of the screen, jump to nono as the ship's
                            \ dot is off the bottom of the space view
    
     JSR Shpt               \ Call Shpt to draw a horizontal four-pixel dash for the
                            \ first row of the dot (i.e. a four-pixel dash)
    
     LDA K4                 \ Set A = y-coordinate of dot + 1 (so this is the second
     CLC                    \ row of the two-pixel high dot)
     ADC #1
    
     JSR Shpt               \ Call Shpt to draw a horizontal four-pixel dash for the
                            \ second row of the dot (i.e. a four-pixel dash)
    
     LDA #%00001000         \ Set bit 3 of the ship's byte #31 to record that we
     ORA XX1+31             \ have now drawn something on-screen for this ship
     STA XX1+31
    
     JMP LSCLR              \ Jump to LSCLR to draw any remaining lines that are
                            \ still in the ship line heap and return from the
                            \ subroutine using a tail call
    
    .nono
    
     LDA #%11110111         \ Clear bit 3 of the ship's byte #31 to record that
     AND XX1+31             \ nothing is being drawn on-screen for this ship
     STA XX1+31
    
     JMP LSCLR              \ Jump to LSCLR to draw any remaining lines that are
                            \ still in the ship line heap and return from the
                            \ subroutine using a tail call
    
    .Shpt
    
                            \ This routine draws a horizontal four-pixel dash, for
                            \ either the top or the bottom of the ship's dot
    
     STA Y1                 \ Store A in both y-coordinates, as this is a horizontal
     STA Y2                 \ dash at y-coordinate A
    
     LDA K3                 \ Set A = screen x-coordinate of the ship dot
    
     STA X1                 \ Store the x-coordinate of the ship dot in X1, as this
                            \ is where the dash starts
    
     CLC                    \ Set A = screen x-coordinate of the ship dot + 3
     ADC #3
    
     BCC P%+4               \ If the addition overflowed, set A = 255, the
     LDA #255               \ x-coordinate of the right edge of the screen
    
     STA X2                 \ Store the x-coordinate of the ship dot in X1, as this
                            \ is where the dash starts
    
     JMP LSPUT              \ Draw this edge using flicker-free animation, by first
                            \ drawing the ship's new line and then erasing the
                            \ corresponding old line from the screen, and return
                            \ from the subroutine using a tail call
    

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [K4](../workspace/zp.md#k4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Entry point [LSCLR](ll9_part_12_of_12.md#lsclr) in subroutine [LL9 (Part 12 of 12)](ll9_part_12_of_12.md) (category: Drawing ships)

Draw any remaining lines from the old ship that are still in the ship line heap

[X]

Subroutine [LSPUT](lsput.md) (category: Drawing lines)

Draw a ship line using flicker-free animation

[X]

Subroutine [PROJ](proj.md) (category: Maths (Geometry))

Project the current ship or planet onto the screen

[X]

Label [Shpt](shppt.md#shpt) is local to this routine

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](../workspace/zp.md#x2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XX1](../workspace/zp.md#xx1) in workspace [ZP](../workspace/zp.md)

This is an alias for INWK that is used in the main ship-drawing routine at LL9

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

Label [nono](shppt.md#nono) is local to this routine

[KTRAN](../variable/ktran.md "Previous routine")[LL5](ll5.md "Next routine")
