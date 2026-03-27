---
title: "The HANGER subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hanger.html"
---

[HANGFLAG](../variable/hangflag.md "Previous routine")[HAS2](has2.md "Next routine")
    
    
           Name: HANGER                                                  [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Display the ship hangar
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-hanger)
     Variations: See [code variations](../../related/compare/main/subroutine/hanger.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HALL](hall.md) calls HANGER
                 * [HAL3](hal3.md) calls via HA3
                 * [HAS2](has2.md) calls via HA3
                 * [HAS3](has3.md) calls via HA3
    
    
    
    
    
    * * *
    
    
     This routine is called after the ships in the hangar have been drawn, so all
     it has to do is draw the hangar's background.
    
     The hangar background is made up of two parts:
    
       * The hangar floor consists of 11 screen-wide horizontal lines, which start
         out quite spaced out near the bottom of the screen, and bunch ever closer
         together as the eye moves up towards the horizon, where they merge to give
         a sense of perspective
    
       * The back wall of the hangar consists of 15 equally spaced vertical lines
         that join the horizon to the top of the screen
    
     The ships in the hangar have already been drawn by this point, so the lines
     are drawn so they don't overlap anything that's already there, which makes
     them look like they are behind and below the ships. This is achieved by
     drawing the lines in from the screen edges until they bump into something
     already on-screen. For the horizontal lines, when there are multiple ships in
     the hangar, this also means drawing lines between the ships, as well as in
     from each side.
    
    
    
    * * *
    
    
     Other entry points:
    
       HA3                  Contains an RTS
    
    
    
    
    .HANGER
    
                            \ We start by drawing the floor
    
     LDX #2                 \ We start with a loop using a counter in T that goes
                            \ from 2 to 12, one for each of the 11 horizontal lines
                            \ in the floor, so set the initial value in X
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
    .HAL1
    
     STX T                  \ Store the loop counter in T
    
     LDA #130               \ Set A = 130
    
     STX Q                  \ Set Q to the value of the loop counter
    
     JSR DVID4K             \ Calculate the following:
                            \
                            \   (P R) = 256 * A / Q
                            \         = 256 * 130 / Q
                            \
                            \ so P = 130 / Q, and as the counter Q goes from 2 to
                            \ 12, P goes 65, 43, 32 ... 13, 11, 10, with the
                            \ difference between two consecutive numbers getting
                            \ smaller as P gets smaller
                            \
                            \ We can use this value as a y-coordinate to draw a set
                            \ of horizontal lines, spaced out near the bottom of the
                            \ screen (high value of P, high y-coordinate, lower down
                            \ the screen) and bunching up towards the horizon (low
                            \ value of P, low y-coordinate, higher up the screen)
    
     LDA P                  \ Set Y = #Y + P
     CLC                    \
     ADC #Y                 \ where #Y is the y-coordinate of the centre of the
     TAY                    \ screen, so Y is now the horizontal pixel row of the
                            \ line we want to draw to display the hangar floor
    
     LDA ylookup,Y          \ Look up the page number of the character row that
     STA SC+1               \ contains the pixel with the y-coordinate in Y, and
                            \ store it in the high byte of SC(1 0) at SC+1
    
     STA R                  \ Also store the page number in R
    
     LDA P                  \ Set the low byte of SC(1 0) to the y-coordinate mod 8,
     AND #7                 \ which determines the pixel row in the character block
     STA SC                 \ we need to draw in (as each character row is 8 pixels
                            \ high), so SC(1 0) now points to the address of the
                            \ start of the horizontal line we want to draw
    
     LDY #0                 \ Set Y = 0 so the call to HAS2 starts drawing the line
                            \ in the first byte of the screen row, at the left edge
                            \ of the screen
    
     JSR HAS2               \ Draw a horizontal line from the left edge of the
                            \ screen, going right until we bump into something
                            \ already on-screen, at which point stop drawing
    
     LDY R                  \ Fetch the page number of the line from R, increment it
     INY                    \ so it points to the right half of the character row
     STY SC+1               \ (as each row takes up 2 pages), and store it in the
                            \ high byte of SC(1 0) at SC+1
    
     LDA #%01000000         \ Now to draw the same line but from the right edge of
                            \ the screen, so set a pixel mask in A to check the
                            \ second pixel of the last byte, so we skip the
                            \ two-pixel screen border at the right edge of the
                            \ screen
    
     LDY #248               \ Set Y = 248 so the call to HAS3 starts drawing the
                            \ line in the last byte of the screen row, at the right
                            \ edge of the screen
    
     JSR HAS3               \ Draw a horizontal line from the right edge of the
                            \ screen, going left until we bump into something
                            \ already on-screen, at which point stop drawing
    
     LDY HANGFLAG           \ Fetch the value of HANGFLAG, which gets set to 0 in
                            \ the HALL routine above if there is only one ship
    
     BEQ HA2                \ If HANGFLAG is zero, jump to HA2 to skip the following
                            \ as there is only one ship in the hangar
    
                            \ If we get here then there are multiple ships in the
                            \ hangar, so we also need to draw the horizontal line in
                            \ the gap between the ships
    
     LDY #0                 \ First we draw the line from the centre of the screen
                            \ to the right. SC(1 0) points to the start address of
                            \ the second half of the screen row, so we set Y to 0 so
                            \ the call to HAL3 starts drawing from the first
                            \ character in that second half
    
     LDA #%10001000         \ We want to start drawing from the first pixel, so we
                            \ set a mask in A to the first pixel in the four-pixel
                            \ byte
    
     JSR HAL3               \ Call HAL3, which draws a line from the halfway point
                            \ across the right half of the screen, going right until
                            \ we bump into something already on-screen, at which
                            \ point it stops drawing
    
     DEC SC+1               \ Decrement the high byte of SC(1 0) in SC+1 to point to
                            \ the previous page (i.e. the left half of this screen
                            \ row)
    
     LDY #248               \ We now draw the line from the centre of the screen
                            \ to the left. SC(1 0) points to the start address of
                            \ the first half of the screen row, so we set Y to 248
                            \ so the call to HAS3 starts drawing from the last
                            \ character in that first half
    
     LDA #%00010000         \ We want to start drawing from the last pixel, so we
                            \ set a mask in A to the last pixel in the four-pixel
                            \ byte
    
     JSR HAS3               \ Call HAS3, which draws a line from the halfway point
                            \ across the left half of the screen, going left until
                            \ we bump into something already on-screen, at which
                            \ point it stops drawing
    
    .HA2
    
                            \ We have finished threading our horizontal line behind
                            \ the ships already on-screen, so now for the next line
    
     LDX T                  \ Fetch the loop counter from T and increment it
     INX
    
     CPX #13                \ If the loop counter is less than 13 (i.e. 2 to 12)
     BCC HAL1               \ then loop back to HAL1 to draw the next line
    
                            \ The floor is done, so now we move on to the back wall
    
     LDA #60                \ Set S = 60, so we run the following 60 times (though I
     STA S                  \ have no idea why it's 60 times, when it should be 15,
                            \ as this has the effect of drawing each vertical line
                            \ four times, each time starting one character row lower
                            \ on-screen)
    
     LDA #16                \ We want to draw 15 vertical lines, one every 16 pixels
                            \ across the screen, with the first at x-coordinate 16,
                            \ so set this in A to act as the x-coordinate of each
                            \ line as we work our way through them from left to
                            \ right, incrementing by 16 for each new line
    
     LDX #&40               \ Set X = &40, the high byte of the start of screen
     STX R                  \ memory (the screen starts at location &4000) and the
                            \ page number of the first screen row
    
    .HAL6
    
     LDX R                  \ Set the high byte of SC(1 0) to R
     STX SC+1
    
     STA T                  \ Store A in T so we can retrieve it later
    
     AND #%11111100         \ A contains the x-coordinate of the line to draw, and
     STA SC                 \ each character block is 4 pixels wide, so setting the
                            \ low byte of SC(1 0) to A mod 4 points SC(1 0) to the
                            \ correct character block on the top screen row for this
                            \ x-coordinate
    
     LDX #%10001000         \ Set a mask in X to the first pixel in the four-pixel
                            \ byte
    
     LDY #1                 \ We are going to start drawing the line from the second
                            \ pixel from the top (to avoid drawing on the one-pixel
                            \ border), so set Y to 1 to point to the second row in
                            \ the first character block
    
    .HAL7
    
     TXA                    \ Copy the pixel mask to A
    
     AND (SC),Y             \ If the pixel we want to draw is non-zero (using A as a
     BNE HA6                \ mask), then this means it already contains something,
                            \ so jump to HA6 to stop drawing this line
    
     TXA                    \ Copy the pixel mask to A again
    
     AND #RED               \ Apply the pixel mask in A to a four-pixel block of
                            \ red pixels, so we now know which bits to set in screen
                            \ memory
    
     ORA (SC),Y             \ OR the byte with the current contents of screen
                            \ memory, so the pixel we want is set to red (because
                            \ we know the bits are already 0 from the above test)
    
     STA (SC),Y             \ Store the updated pixel in screen memory
    
     INY                    \ Increment Y to point to the next row in the character
                            \ block, i.e. the next pixel down
    
     CPY #8                 \ Loop back to HAL7 to draw this next pixel until we
     BNE HAL7               \ have drawn all 8 in the character block
    
     INC SC+1               \ There are two pages of memory for each character row,
     INC SC+1               \ so we increment the high byte of SC(1 0) twice to
                            \ point to the same character but in the next row down
    
     LDY #0                 \ Set Y = 0 to point to the first row in this character
                            \ block
    
     BEQ HAL7               \ Loop back up to HAL7 to keep drawing the line (this
                            \ BEQ is effectively a JMP as Y is always zero)
    
    .HA6
    
     LDA T                  \ Fetch the x-coordinate of the line we just drew from T
     CLC                    \ into A, and add 16 so that A contains the x-coordinate
     ADC #16                \ of the next line to draw
    
     BCC P%+4               \ If the addition overflowed, increment the page number
     INC R                  \ in R to point to the second half of the screen row
    
     DEC S                  \ Decrement the loop counter in S
    
     BNE HAL6               \ Loop back to HAL6 until we have run through the loop
                            \ 60 times, by which point we are most definitely done
    
    IF _SNG47
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine (this instruction is not
                            \ needed as we could just fall through into the RTS at
                            \ HA3 below)
    
    ELIF _COMPACT
    
     JMP away               \ Jump to away to switch main memory back into
                            \ &3000-&7FFF and return from the subroutine
    
    ENDIF
    
    .HA3
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [DVID4K](dvid4k.md) (category: Maths (Arithmetic))

Calculate (P R) = 256 * A / Q

[X]

Label [HA2](hanger.md#ha2) is local to this routine

[X]

Label [HA6](hanger.md#ha6) is local to this routine

[X]

Label [HAL1](hanger.md#hal1) is local to this routine

[X]

Subroutine [HAL3](hal3.md) (category: Ship hangar)

Draw a hangar background line from left to right, stopping when it bumps into existing on-screen content

[X]

Label [HAL6](hanger.md#hal6) is local to this routine

[X]

Label [HAL7](hanger.md#hal7) is local to this routine

[X]

Variable [HANGFLAG](../variable/hangflag.md) (category: Ship hangar)

The number of ships being displayed in the ship hangar

[X]

Subroutine [HAS2](has2.md) (category: Ship hangar)

Draw a hangar background line from left to right

[X]

Subroutine [HAS3](has3.md) (category: Ship hangar)

Draw a hangar background line from right to left

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Configuration variable [RED](../../all/workspaces.md#red) = %11110000

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[X]

Entry point [away](spblb.md#away) in subroutine [SPBLB](spblb.md) (category: Dashboard)

Switch main memory back into &3000-&7FFF and return from the subroutine

[X]

Variable [ylookup](../variable/ylookup.md) (category: Drawing pixels)

Lookup table for converting pixel y-coordinate to page number of screen address

[HANGFLAG](../variable/hangflag.md "Previous routine")[HAS2](has2.md "Next routine")
