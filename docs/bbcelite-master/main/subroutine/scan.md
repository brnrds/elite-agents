---
title: "The SCAN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/scan.html"
---

[DKS4mc](dks4mc.md "Previous routine")[LOIN](loin.md "Next routine")
    
    
           Name: SCAN                                                    [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Display the current ship on the scanner
      Deep dive: [The 3D scanner](https://elite.bbcelite.com/deep_dives/the_3d_scanner.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-scan)
     Variations: See [code variations](../../related/compare/main/subroutine/scan.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ESCAPE](escape.md) calls SCAN
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls SCAN
                 * [MVEIT (Part 2 of 9)](mveit_part_2_of_9.md) calls SCAN
                 * [MVEIT (Part 9 of 9)](mveit_part_9_of_9.md) calls SCAN
                 * [WPSHPS](wpshps.md) calls SCAN
    
    
    
    
    
    * * *
    
    
     This is used both to display a ship on the scanner, and to erase it again.
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The ship's data block
    
    
    
    
    .SCR1
    
     RTS                    \ Return from the subroutine
    
    .SCAN
    
     LDA INWK+31            \ Fetch the ship's scanner flag from byte #31
    
     AND #%00010000         \ If bit 4 is clear then the ship should not be shown
     BEQ SCR1               \ on the scanner, so return from the subroutine (as SCR1
                            \ contains an RTS)
    
     LDX TYPE               \ Fetch the ship's type from TYPE into X
    
     BMI SCR1               \ If this is the planet or the sun, then the type will
                            \ have bit 7 set and we don't want to display it on the
                            \ scanner, so return from the subroutine (as SCR1
                            \ contains an RTS)
    
     LDA scacol,X           \ Set A to the scanner colour for this ship type from
                            \ the X-th entry in the scacol table
    
     STA COL                \ Store the scanner colour in COL so it can be used to
                            \ draw this ship in the correct colour
    
     LDA INWK+1             \ If any of x_hi, y_hi and z_hi have a 1 in bit 6 or 7,
     ORA INWK+4             \ then the ship is too far away to be shown on the
     ORA INWK+7             \ scanner, so return from the subroutine (as SCR1
     AND #%11000000         \ contains an RTS)
     BNE SCR1
    
                            \ If we get here, we know x_hi, y_hi and z_hi are all
                            \ 63 (%00111111) or less
    
                            \ Now, we convert the x_hi coordinate of the ship into
                            \ the screen x-coordinate of the dot on the scanner,
                            \ using the following:
                            \
                            \   X1 = 123 + (x_sign x_hi)
    
     LDA INWK+1             \ Set A = x_hi
    
     CLC                    \ Clear the C flag so we can do addition below
    
     LDX INWK+2             \ Set X = x_sign
    
     BPL SC2                \ If x_sign is positive, skip the following
    
     EOR #%11111111         \ x_sign is negative, so flip the bits in A and add 1
     ADC #1                 \ to make it a negative number (bit 7 will now be set
                            \ as we confirmed above that bits 6 and 7 are clear). So
                            \ this gives A the sign of x_sign and gives it a value
                            \ range of -63 (%11000001) to 0
    
     CLC                    \ Clear the C flag so we can do addition below
    
    .SC2
    
     ADC #125               \ Set X1 = 125 + (x_sign x_hi)
     AND #%11111110         \
     STA X1                 \ and if the result is odd, subtract 1 to make it even
    
     TAX                    \ Set X = X1 - 2
     DEX                    \
     DEX                    \ So X contains the x-coordinate that's two pixels to
                            \ left of the top of the stick, so we can draw the dot
                            \ at the end of the stick by simply drawing a dot at
                            \ x-coordinate X at the correct end of the stick
    
                            \ Next, we convert the z_hi coordinate of the ship into
                            \ the y-coordinate of the base of the ship's stick,
                            \ like this:
                            \
                            \   SC = 220 - (z_sign z_hi) / 4
                            \
                            \ though the following code actually does it like this:
                            \
                            \   SC = 255 - (35 + z_hi / 4)
    
     LDA INWK+7             \ Set A = z_hi / 4
     LSR A                  \
     LSR A                  \ So A is in the range 0-15
    
     CLC                    \ Clear the C flag for the addition below
    
     LDY INWK+8             \ Set Y = z_sign
    
     BPL SC3                \ If z_sign is positive, skip the following
    
     EOR #%11111111         \ z_sign is negative, so flip the bits in A and set the
     SEC                    \ C flag. As above, this makes A negative, this time
                            \ with a range of -16 (%11110000) to -1 (%11111111). And
                            \ as we are about to do an ADC, the SEC effectively adds
                            \ another 1 to that value, giving a range of -15 to 0
    
    .SC3
    
     ADC #35                \ Set A = 35 + A to give a number in the range 20 to 50
    
     EOR #%11111111         \ Flip all the bits and store in Y2, so Y2 is in the
     STA Y2                 \ range 205 to 235, with a higher z_hi giving a lower Y2
    
                            \ Now for the stick height, which we calculate using the
                            \ following:
                            \
                            \ A = - (y_sign y_hi) / 2
    
     LDA INWK+4             \ Set A = y_hi / 2
     LSR A
    
     CLC                    \ Clear the C flag
    
     LDY INWK+5             \ Set Y = y_sign
    
     BMI SCD6               \ If y_sign is negative, skip the following, as we
                            \ already have a positive value in A
    
     EOR #%11111111         \ y_sign is positive, so flip the bits in A and set the
     SEC                    \ C flag. This makes A negative, and as we are about to
                            \ do an ADC below, the SEC effectively adds another 1 to
                            \ that value to implement two's complement negation, so
                            \ we don't need to add another 1 here
    
    .SCD6
    
                            \ We now have all the information we need to draw this
                            \ ship on the scanner, namely:
                            \
                            \   X1 = the screen x-coordinate of the ship's dot
                            \
                            \   SC = the screen y-coordinate of the base of the
                            \        stick
                            \
                            \   A = the screen height of the ship's stick, with the
                            \       correct sign for adding to the base of the stick
                            \       to get the dot's y-coordinate
                            \
                            \ First, though, we have to make sure the dot is inside
                            \ the dashboard, by moving it if necessary
    
     ADC Y2                 \ Set A = Y2 + A, so A now contains the y-coordinate of
                            \ the end of the stick, plus the length of the stick, to
                            \ give us the screen y-coordinate of the dot
    
     BPL ld246              \ If the result has bit 0 clear, then the result has
                            \ overflowed and is bigger than 256, so jump to ld246 to
                            \ set A to the maximum allowed value of 246 (this
                            \ instruction isn't required as we test both the maximum
                            \ and minimum below, but it might save a few cycles)
    
     CMP #194               \ If A >= 194, skip the following instruction, as 194 is
     BCS P%+4               \ the minimum allowed value of A
    
     LDA #194               \ A < 194, so set A to 194, the minimum allowed value
                            \ for the y-coordinate of our ship's dot
    
     CMP #247               \ If A < 247, skip the following instruction, as 246 is
     BCC P%+4               \ the maximum allowed value of A
    
    .ld246
    
     LDA #246               \ A >= 247, so set A to 246, the maximum allowed value
                            \ for the y-coordinate of our ship's dot
    
     LDY #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     JSR CPIXK              \ Call CPIXK to draw a single-height dash at the
                            \ y-coordinate in A, to draw the ship dot, and return
                            \ the dash's right pixel byte in R, which we use below
    
     LDA Y1                 \ Fetch the y-coordinate back into A, which was stored
                            \ in Y1 by the call to CPIX2
    
     SEC                    \ Set A = A - Y2 to get the stick length, by reversing
     SBC Y2                 \ the ADC Y2 we did above. This clears the C flag if the
                            \ result is negative (i.e. the stick length is negative)
                            \ and sets it if the result is positive (i.e. the stick
                            \ length is negative)
    
                            \ So now we have the following:
                            \
                            \   X1 = the screen x-coordinate of the ship's dot,
                            \        clipped to fit into the dashboard
                            \
                            \   Y1 = the screen y-coordinate of the ship's dot,
                            \        clipped to fit into the dashboard
                            \
                            \   SC = the screen y-coordinate of the base of the
                            \        stick
                            \
                            \   A = the screen height of the ship's stick, with the
                            \       correct sign for adding to the base of the stick
                            \       to get the dot's y-coordinate
                            \
                            \   C = 0 if A is negative, 1 if A is positive
                            \
                            \ and we can get on with drawing the dot and stick
    
     BEQ VLO5               \ If the stick height is zero, then there is no stick to
                            \ draw, so return from the subroutine (as VLO5 contains
                            \ an RTS)
    
     BCC VLO1               \ If the C flag is clear then the stick height in A is
                            \ negative, so jump down to RTS+1
    
     TAX                    \ Copy the (positive) stick height into X
    
     INX                    \ Increment the (positive) stick height in X
    
     JMP VLO4               \ Jump into the middle of the VLOL1 loop, skipping the
                            \ drawing of first pixel in the stick
    
    .VLOL1
    
     LDA R                  \ The call to CPIX2 above saved the dash's right pixel
                            \ byte in R, so we load this into A (so the stick comes
                            \ out of the right side of the dot)
    
     EOR (SC),Y             \ Draw the bottom row of the double-height dot using the
     STA (SC),Y             \ same byte as the top row, plotted using EOR logic
    
    .VLO4
    
                            \ If we get here then the stick length is positive (so
                            \ the dot is below the ellipse and the stick is above
                            \ the dot, and we need to draw the stick upwards from
                            \ the dot)
    
     DEY                    \ We want to draw the stick upwards, so decrement the
                            \ pixel row in Y
    
     BPL VLO3               \ If Y is still positive then it correctly points at the
                            \ line above, so jump to VLO3 to skip the following
    
     LDA SC+1               \ Subtract 2 from the high byte of the screen address to
     SBC #2                 \ move to the character block above
     STA SC+1
    
     LDY #7                 \ We just decremented Y up through the top of the
                            \ character block, so we need to move it to the last row
                            \ in the character above, so set Y to 7, the number of
                            \ the last row
    
    .VLO3
    
     DEX                    \ Decrement the (positive) stick height in X
    
     BNE VLOL1              \ If we still have more stick to draw, jump up to VLOL1
                            \ to draw the next pixel
    
    .VLO5
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    .VLO1
    
                            \ If we get here then the stick length is negative (so
                            \ the dot is above the ellipse and the stick is below
                            \ the dot, and we need to draw the stick downwards from
                            \ the dot)
    
     LDA Y2                 \ Set A = Y2 - Y1 to get the positive stick height
     SEC
     SBC Y1
    
     TAX                    \ Copy the (positive) stick height into X
    
     INX                    \ Increment the (positive) stick height in X
    
     JMP VLO6               \ Jump into the middle of the VLOL2 loop, skipping the
                            \ drawing of first pixel in the stick
    
    .VLOL2
    
     LDA R                  \ The call to CPIX2 above saved the dash's right pixel
                            \ byte in R, so we load this into A (so the stick comes
                            \ out of the right side of the dot)
    
     EOR (SC),Y             \ Draw the bottom row of the double-height dot using the
     STA (SC),Y             \ same byte as the top row, plotted using EOR logic
    
    .VLO6
    
     INY                    \ We want to draw the stick itself, heading downwards,
                            \ so increment the pixel row in Y
    
     CPY #8                 \ If the row number in Y is less than 8, then it
     BNE VLO7               \ correctly points at the next line down, so jump to
                            \ VLO7 to skip the following
    
     LDA SC+1               \ We just incremented Y down through the bottom of the
     ADC #1                 \ character block, so increment the high byte of the
     STA SC+1               \ screen address to move to the character block above
    
     LDY #0                 \ We need to move to the first row in the character
                            \ below, so set Y to 0, the number of the first row
    
    .VLO7
    
     DEX                    \ Decrement the (positive) stick height in X
    
     BNE VLOL2              \ If we still have more stick to draw, jump up to VLOL2
                            \ to draw the next pixel
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [CPIXK](cpixk.md) (category: Drawing pixels)

Draw a single-height dash on the dashboard

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Label [SC2](scan.md#sc2) is local to this routine

[X]

Label [SC3](scan.md#sc3) is local to this routine

[X]

Label [SCD6](scan.md#scd6) is local to this routine

[X]

Label [SCR1](scan.md#scr1) is local to this routine

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Label [VLO1](scan.md#vlo1) is local to this routine

[X]

Label [VLO3](scan.md#vlo3) is local to this routine

[X]

Label [VLO4](scan.md#vlo4) is local to this routine

[X]

Label [VLO5](scan.md#vlo5) is local to this routine

[X]

Label [VLO6](scan.md#vlo6) is local to this routine

[X]

Label [VLO7](scan.md#vlo7) is local to this routine

[X]

Label [VLOL1](scan.md#vlol1) is local to this routine

[X]

Label [VLOL2](scan.md#vlol2) is local to this routine

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Label [ld246](scan.md#ld246) is local to this routine

[X]

Variable [scacol](../variable/scacol.md) (category: Drawing ships)

Ship colours on the scanner

[DKS4mc](dks4mc.md "Previous routine")[LOIN](loin.md "Next routine")
