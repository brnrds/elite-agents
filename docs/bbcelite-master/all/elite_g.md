---
title: "Elite G source in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_g.html"
---

[Elite F source](elite_f.md "Previous routine")[Elite H source](elite_h.md "Next routine")
    
    
     ELITE G FILE
    
    
    
    
     CODE_G% = P%
    
     LOAD_G% = LOAD% + P% - CODE%
    
    
    
    
           Name: SHPPT                                                   [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw a distant ship as a point rather than a full wireframe
    
    
        Context: See this subroutine [on its own page](../main/subroutine/shppt.md)
     Variations: See [code variations](../related/compare/main/subroutine/shppt.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md) calls SHPPT
    
    
    
    
    
    
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
    
    
    
    
           Name: LL5                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate Q = SQRT(R Q)
      Deep dive: [Calculating square roots](https://elite.bbcelite.com/deep_dives/calculating_square_roots.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll5.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll5.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HAS1](../main/subroutine/has1.md) calls LL5
                 * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md) calls LL5
                 * [NORM](../main/subroutine/norm.md) calls LL5
                 * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md) calls LL5
                 * [TT111](../main/subroutine/tt111.md) calls LL5
    
    
    
    
    
    * * *
    
    
     Calculate the following square root:
    
       Q = SQRT(R Q)
    
    
    
    
    .LL5
    
     LDY R                  \ Set (Y S) = (R Q)
     LDA Q
     STA S
    
                            \ So now to calculate Q = SQRT(Y S)
    
     LDX #0                 \ Set X = 0, to hold the remainder
    
     STX Q                  \ Set Q = 0, to hold the result
    
     LDA #8                 \ Set T = 8, to use as a loop counter
     STA T
    
    .LL6
    
     CPX Q                  \ If X < Q, jump to LL7
     BCC LL7
    
     BNE P%+6               \ If X > Q, skip the next two instructions
    
     CPY #64                \ If Y < 64, jump to LL7 with the C flag clear,
     BCC LL7                \ otherwise fall through into LL8 with the C flag set
    
     TYA                    \ Set Y = Y - 64
     SBC #64                \
     TAY                    \ This subtraction will work as we know C is set from
                            \ the BCC above, and the result will not underflow as we
                            \ already checked that Y >= 64, so the C flag is also
                            \ set for the next subtraction
    
     TXA                    \ Set X = X - Q
     SBC Q
     TAX
    
    .LL7
    
     ROL Q                  \ Shift the result in Q to the left, shifting the C flag
                            \ into bit 0 and bit 7 into the C flag
    
     ASL S                  \ Shift the dividend in (Y S) to the left, inserting
     TYA                    \ bit 7 from above into bit 0
     ROL A
     TAY
    
     TXA                    \ Shift the remainder in X to the left
     ROL A
     TAX
    
     ASL S                  \ Shift the dividend in (Y S) to the left
     TYA
     ROL A
     TAY
    
     TXA                    \ Shift the remainder in X to the left
     ROL A
     TAX
    
     DEC T                  \ Decrement the loop counter
    
     BNE LL6                \ Loop back to LL6 until we have done 8 loops
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL28                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate R = 256 * A / Q
      Deep dive: [Multiplication and division using logarithms](https://elite.bbcelite.com/deep_dives/multiplication_and_division_using_logarithms.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll28.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll28.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ARCTAN](../main/subroutine/arctan.md) calls LL28
                 * [LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md) calls LL28
                 * [LL61](../main/subroutine/ll61.md) calls LL28
                 * [LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md) calls LL28
                 * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md) calls LL28
    
    
    
    
    
    * * *
    
    
     Calculate the following, where A < Q:
    
       R = 256 * A / Q
    
     This is a sister routine to LL61, which does the division when A >= Q.
    
     If A >= Q then 255 is returned and the C flag is set to indicate an overflow
     (the C flag is clear if the division was a success).
    
     The result is returned in one byte as the result of the division multiplied
     by 256, so we can return fractional results using integers.
    
     This routine uses the same logarithm algorithm that's documented in FMLTU,
     except it subtracts the logarithm values, to do a division instead of a
     multiplication.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the answer is too big for one byte, clear if the
                            division was a success
    
    
    
    * * *
    
    
     Other entry points:
    
       LL28+4               Skips the A >= Q check and always returns with C flag
                            cleared, so this can be called if we know the division
                            will work
    
       LL31                 Skips the A >= Q check and does not set the R counter,
                            so this can be used for jumping straight into the
                            division loop if R is already set to 254 and we know the
                            division will work
    
    
    
    
    .LL28
    
     CMP Q                  \ If A >= Q, then the answer will not fit in one byte,
     BCS LL2                \ so jump to LL2 to return 255
    
     STA widget             \ Store A in widget, so now widget = argument A
    
     TAX                    \ Transfer A into X, so now X = argument A
    
     BEQ LLfix              \ If A = 0, jump to LLfix to return a result of 0, as
                            \ 0 * Q / 256 is always 0
    
                            \ We now want to calculate log(A) - log(Q), first adding
                            \ the low bytes (from the logL table), and then the high
                            \ bytes (from the log table)
    
     LDA logL,X             \ Set A = low byte of log(X)
                            \       = low byte of log(A) (as we set X to A above)
    
     LDX Q                  \ Set X = Q
    
     SEC                    \ Set A = A - low byte of log(Q)
     SBC logL,X             \       = low byte of log(A) - low byte of log(Q)
    
     LDX widget             \ Set A = high byte of log(A) - high byte of log(Q)
     LDA log,X
     LDX Q
     SBC log,X
    
     BCS LL2                \ If the subtraction fitted into one byte and didn't
                            \ underflow, then log(A) - log(Q) < 256, so we jump to
                            \ LL2 return a result of 255
    
     TAX                    \ Otherwise we return the A-th entry from the antilog
     LDA alogh,X            \ table
    
    .LLfix
    
     STA R                  \ Set the result in R to the value of A
    
     RTS                    \ Return from the subroutine
    
    \.LL28                  \ These instructions are commented out in the original
    \CMP Q                  \ source
    
     BCS LL2                \ If the subtraction fitted into one byte and didn't
                            \ underflow, then log(A) - log(Q) < 256, so we jump to
                            \ LL2 to return a result of 255
    
     LDX #254               \ Otherwise set the result in R to 254
     STX R
    
    .LL31
    
     ASL A                  \ Shift A to the left
    
     BCS LL29               \ If bit 7 of A was set, then jump straight to the
                            \ subtraction
    
     CMP Q                  \ If A < Q, skip the following subtraction
     BCC P%+4
    
     SBC Q                  \ A >= Q, so set A = A - Q
    
     ROL R                  \ Rotate the counter in R to the left, and catch the
                            \ result bit into bit 0 (which will be a 0 if we didn't
                            \ do the subtraction, or 1 if we did)
    
     BCS LL31               \ If we still have set bits in R, loop back to LL31 to
                            \ do the next iteration of 7
    
     RTS                    \ R left with remainder of division
    
    .LL29
    
     SBC Q                  \ A >= Q, so set A = A - Q
    
     SEC                    \ Set the C flag to rotate into the result in R
    
     ROL R                  \ Rotate the counter in R to the left, and catch the
                            \ result bit into bit 0 (which will be a 0 if we didn't
                            \ do the subtraction, or 1 if we did)
    
     BCS LL31               \ If we still have set bits in R, loop back to LL31 to
                            \ do the next iteration of 7
    
     LDA R                  \ Set A to the remainder in R
    
     RTS                    \ Return from the subroutine with R containing the
                            \ remainder of the division
    
    .LL2
    
     LDA #255               \ The division is very close to 1, so return the closest
     STA R                  \ possible answer to 256, i.e. R = 255
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL38                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (S A) = (S R) + (A Q)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll38.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll38.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL51](../main/subroutine/ll51.md) calls LL38
                 * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md) calls LL38
    
    
    
    
    
    * * *
    
    
     Calculate the following between sign-magnitude numbers:
    
       (S A) = (S R) + (A Q)
    
     where the sign bytes only contain the sign bits, not magnitudes.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the addition overflowed, clear otherwise
    
    
    
    
    .LL38
    
     EOR S                  \ If the sign of A * S is negative, skip to LL35, as
     BMI LL39               \ A and S have different signs so we need to subtract
    
     LDA Q                  \ Otherwise set A = R + Q, which is the result we need,
     CLC                    \ as S already contains the correct sign
     ADC R
    
     RTS                    \ Return from the subroutine
    
    .LL39
    
     LDA R                  \ Set A = R - Q
     SEC
     SBC Q
    
     BCC P%+4               \ If the subtraction underflowed, skip the next two
                            \ instructions so we can negate the result
    
     CLC                    \ Otherwise the result is correct, and S contains the
                            \ correct sign of the result as R is the dominant side
                            \ of the subtraction, so clear the C flag
    
     RTS                    \ And return from the subroutine
    
                            \ If we get here we need to negate both the result and
                            \ the sign in S, as both are the wrong sign
    
    .LL40
    
     PHA                    \ Store the result of the subtraction on the stack
    
     LDA S                  \ Flip the sign of S
     EOR #%10000000
     STA S
    
     PLA                    \ Restore the subtraction result into A
    
     EOR #%11111111         \ Negate the result in A using two's complement, i.e.
     ADC #1                 \ set A = ~A + 1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL51                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the dot product of XX15 and XX16
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll51.md)
     References: This subroutine is called as follows:
                 * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md) calls LL51
                 * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md) calls LL51
    
    
    
    
    
    * * *
    
    
     Calculate the following dot products:
    
       XX12(1 0) = XX15(5 0) . XX16(5 0)
       XX12(3 2) = XX15(5 0) . XX16(11 6)
       XX12(5 4) = XX15(5 0) . XX16(12 17)
    
     storing the results as sign-magnitude numbers in XX12 through XX12+5.
    
     When called from part 5 of LL9, XX12 contains the vector [x y z] to the ship
     we're drawing, and XX16 contains the orientation vectors, so it returns:
    
       [ x ]   [ sidev_x ]         [ x ]   [ roofv_x ]         [ x ]   [ nosev_x ]
       [ y ] . [ sidev_y ]         [ y ] . [ roofv_y ]         [ y ] . [ nosev_y ]
       [ z ]   [ sidev_z ]         [ z ]   [ roofv_z ]         [ z ]   [ nosev_z ]
    
     When called from part 6 of LL9, XX12 contains the vector [x y z] of the vertex
     we're analysing, and XX16 contains the transposed orientation vectors with
     each of them containing the x, y and z elements of the original vectors, so it
    
    
    * * *
    
    
     Returns:
    
       [ x ]   [ sidev_x ]         [ x ]   [ sidev_y ]         [ x ]   [ sidev_z ]
       [ y ] . [ roofv_x ]         [ y ] . [ roofv_y ]         [ y ] . [ roofv_z ]
       [ z ]   [ nosev_x ]         [ z ]   [ nosev_y ]         [ z ]   [ nosev_z ]
    
    
    
    * * *
    
    
     Arguments:
    
       XX15(1 0)            The ship (or vertex)'s x-coordinate as (x_sign x_lo)
    
       XX15(3 2)            The ship (or vertex)'s y-coordinate as (y_sign y_lo)
    
       XX15(5 4)            The ship (or vertex)'s z-coordinate as (z_sign z_lo)
    
       XX16 to XX16+5       The scaled sidev (or _x) vector, with:
    
                              * x, y, z magnitudes in XX16, XX16+2, XX16+4
    
                              * x, y, z signs in XX16+1, XX16+3, XX16+5
    
       XX16+6 to XX16+11    The scaled roofv (or _y) vector, with:
    
                              * x, y, z magnitudes in XX16+6, XX16+8, XX16+10
    
                              * x, y, z signs in XX16+7, XX16+9, XX16+11
    
       XX16+12 to XX16+17   The scaled nosev (or _z) vector, with:
    
                              * x, y, z magnitudes in XX16+12, XX16+14, XX16+16
    
                              * x, y, z signs in XX16+13, XX16+15, XX16+17
    
    
    
    * * *
    
    
     Returns:
    
       XX12(1 0)            The dot product of [x y z] vector with the sidev (or _x)
                            vector, with the sign in XX12+1 and magnitude in XX12
    
       XX12(3 2)            The dot product of [x y z] vector with the roofv (or _y)
                            vector, with the sign in XX12+3 and magnitude in XX12+2
    
       XX12(5 4)            The dot product of [x y z] vector with the nosev (or _z)
                            vector, with the sign in XX12+5 and magnitude in XX12+4
    
    
    
    
    .LL51
    
     LDX #0                 \ Set X = 0, which will contain the offset of the vector
                            \ to use in the calculation, increasing by 6 for each
                            \ new vector
    
     LDY #0                 \ Set Y = 0, which will contain the offset of the
                            \ result bytes in XX12, increasing by 2 for each new
                            \ result
    
    .ll51
    
     LDA XX15               \ Set Q = x_lo
     STA Q
    
     LDA XX16,X             \ Set A = |sidev_x|
    
     JSR FMLTU              \ Set T = A * Q / 256
     STA T                  \       = |sidev_x| * x_lo / 256
    
     LDA XX15+1             \ Set S to the sign of x_sign * sidev_x
     EOR XX16+1,X
     STA S
    
     LDA XX15+2             \ Set Q = y_lo
     STA Q
    
     LDA XX16+2,X           \ Set A = |sidev_y|
    
     JSR FMLTU              \ Set Q = A * Q / 256
     STA Q                  \       = |sidev_y| * y_lo / 256
    
     LDA T                  \ Set R = T
     STA R                  \       = |sidev_x| * x_lo / 256
    
     LDA XX15+3             \ Set A to the sign of y_sign * sidev_y
     EOR XX16+3,X
    
     JSR LL38               \ Set (S T) = (S R) + (A Q)
     STA T                  \           = |sidev_x| * x_lo + |sidev_y| * y_lo
    
     LDA XX15+4             \ Set Q = z_lo
     STA Q
    
     LDA XX16+4,X           \ Set A = |sidev_z|
    
     JSR FMLTU              \ Set Q = A * Q / 256
     STA Q                  \       = |sidev_z| * z_lo / 256
    
     LDA T                  \ Set R = T
     STA R                  \       = |sidev_x| * x_lo + |sidev_y| * y_lo
    
     LDA XX15+5             \ Set A to the sign of z_sign * sidev_z
     EOR XX16+5,X
    
     JSR LL38               \ Set (S A) = (S R) + (A Q)
                            \           = |sidev_x| * x_lo + |sidev_y| * y_lo
                            \             + |sidev_z| * z_lo
    
     STA XX12,Y             \ Store the result in XX12+Y(1 0)
     LDA S
     STA XX12+1,Y
    
     INY                    \ Set Y = Y + 2
     INY
    
     TXA                    \ Set X = X + 6
     CLC
     ADC #6
     TAX
    
     CMP #17                \ If X < 17, loop back to ll51 for the next vector
     BCC ll51
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL9 (Part 1 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Check if ship is exploding, check if ship is in front
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_1_of_12.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll9_part_1_of_12.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](../main/subroutine/brief.md) calls LL9
                 * [ESCAPE](../main/subroutine/escape.md) calls LL9
                 * [HAS1](../main/subroutine/has1.md) calls LL9
                 * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md) calls LL9
                 * [PAS1](../main/subroutine/pas1.md) calls LL9
                 * [PAUSE](../main/subroutine/pause.md) calls LL9
                 * [TITLE](../main/subroutine/title.md) calls LL9
    
    
    
    
    
    * * *
    
    
     This routine draws the current ship on the screen. This part checks to see if
     the ship is exploding, or if it should start exploding, and if it does it sets
     things up accordingly.
    
     In this code, XX1 is used to point to the current ship's data block at INWK
     (the two labels are interchangeable).
    
    
    
    * * *
    
    
     Arguments:
    
       XX1                  XX1 shares its location with INWK, which contains the
                            zero-page copy of the data block for this ship from the
                            K% workspace
    
       INF                  The address of the data block for this ship in workspace
                            K%
    
       XX19(1 0)            XX19(1 0) shares its location with INWK(34 33), which
                            contains the ship line heap address pointer
    
       XX0                  The address of the blueprint for this ship
    
    
    
    * * *
    
    
     Other entry points:
    
       EE51                 Remove the current ship from the screen, called from
                            SHPPT before drawing the ship as a point
    
    
    
    
    .LL25
    
     JMP PLANET             \ Jump to the PLANET routine, returning from the
                            \ subroutine using a tail call
    
    .LL9
    
     LDX TYPE               \ If the ship type is negative then this indicates a
     BMI LL25               \ planet or sun, so jump to PLANET via LL25 above
    
     LDA shpcol,X           \ Set A to the ship colour for this type, from the X-th
                            \ entry in the shpcol table
    
     STA COL                \ Switch to this colour
    
     LDA #31                \ Set XX4 = 31 to store the ship's distance for later
     STA XX4                \ comparison with the visibility distance. We will
                            \ update this value below with the actual ship's
                            \ distance if it turns out to be visible on-screen
    
                            \ We now set things up for flicker-free ship plotting,
                            \ by setting the following:
                            \
                            \   LSNUM = offset to the first coordinate in the ship's
                            \           line heap
                            \
                            \   LSNUM2 = the number of bytes in the heap for the
                            \            ship that's currently on-screen (or 0 if
                            \            there is no ship currently on-screen)
    
     LDY #1                 \ Set LSNUM = 1, the offset of the first set of line
     STY LSNUM              \ coordinates in the ship line heap
    
     DEY                    \ Decrement Y to 0
    
     LDA #%00001000         \ If bit 3 of the ship's byte #31 is set, then the ship
     BIT INWK+31            \ is currently being drawn on-screen, so skip the
     BNE P%+5               \ following two instructions
    
     LDA #0                 \ The ship is not being drawn on screen, so set A = 0
                            \ so that LSNUM2 gets set to 0 below (as there are no
                            \ existing coordinates on the ship line heap for this
                            \ ship)
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &B1 &BD, or BIT &BDB1 which does nothing apart
                            \ from affect the flags
    
     LDA (XX19),Y           \ Set LSNUM2 to the first byte of the ship's line heap,
     STA LSNUM2             \ which contains the number of bytes in the heap
    
     LDA NEWB               \ If bit 7 of the ship's NEWB flags is set, then the
     BMI EE51               \ ship has been scooped or has docked, so jump down to
                            \ EE51 to redraw its wireframe, to remove it from the
                            \ screen
    
     LDA #%00100000         \ If bit 5 of the ship's byte #31 is set, then the ship
     BIT XX1+31             \ is currently exploding, so jump down to EE28
     BNE EE28
    
     BPL EE28               \ If bit 7 of the ship's byte #31 is clear then the ship
                            \ has not just been killed, so jump down to EE28
    
                            \ Otherwise bit 5 is clear and bit 7 is set, so the ship
                            \ is not yet exploding but it has been killed, so we
                            \ need to start an explosion
    
     ORA XX1+31             \ Clear bits 6 and 7 of the ship's byte #31, to stop the
     AND #%00111111         \ ship from firing its laser and to mark it as no longer
     STA XX1+31             \ having just been killed
    
     LDA #0                 \ Set the ship's acceleration in byte #31 to 0, updating
     LDY #28                \ the byte in the workspace K% data block so we don't
     STA (INF),Y            \ have to copy it back from INWK later
    
     LDY #30                \ Set the ship's pitch counter in byte #30 to 0, to stop
     STA (INF),Y            \ the ship from pitching
    
     JSR EE51               \ Call EE51 to remove the ship from the screen
    
                            \ We now need to set up a new explosion cloud. We
                            \ initialise it with a size of 18 (which gets increased
                            \ by 4 every time the cloud gets redrawn), and the
                            \ explosion count (i.e. the number of particles in the
                            \ explosion), which go into bytes 1 and 2 of the ship
                            \ line heap. See DOEXP for more details of explosion
                            \ clouds
    
     LDY #1                 \ Set byte #1 of the ship line heap to 18, the initial
     LDA #18                \ size of the explosion cloud
     STA (XX19),Y
    
     LDY #7                 \ Fetch byte #7 from the ship's blueprint, which
     LDA (XX0),Y            \ determines the explosion count (i.e. the number of
     LDY #2                 \ vertices used as origins for explosion clouds), and
     STA (XX19),Y           \ store it in byte #2 of the ship line heap
    
                            \ The following loop sets bytes 3-6 of the of the ship
                            \ line heap to random numbers
    
    .EE55
    
     INY                    \ Increment Y (so the loop starts at 3)
    
     JSR DORND              \ Set A and X to random numbers
    
     STA (XX19),Y           \ Store A in the Y-th byte of the ship line heap
    
     CPY #6                 \ Loop back until we have randomised the 6th byte
     BNE EE55
    
    .EE28
    
     LDA XX1+8              \ Set A = z_sign
    
    .EE49
    
     BPL LL10               \ If A is positive, i.e. the ship is in front of us,
                            \ jump down to LL10
    
    .LL14
    
                            \ The following removes the ship from the screen by
                            \ redrawing it (or, if it is exploding, by redrawing the
                            \ explosion cloud). We call it when the ship is no
                            \ longer on-screen, is too far away to be fully drawn,
                            \ and so on
    
     LDA XX1+31             \ If bit 5 of the ship's byte #31 is clear, then the
     AND #%00100000         \ ship is not currently exploding, so jump down to EE51
     BEQ EE51               \ to redraw its wireframe
    
     LDA XX1+31             \ The ship is exploding, so clear bit 3 of the ship's
     AND #%11110111         \ byte #31 to denote that the ship is no longer being
     STA XX1+31             \ drawn on-screen
    
     JMP DOEXP              \ Jump to DOEXP to display the explosion cloud, which
                            \ will remove it from the screen, returning from the
                            \ subroutine using a tail call
    
    .EE51
    
     LDA #%00001000         \ If bit 3 of the ship's byte #31 is clear, then there
     BIT XX1+31             \ is already nothing being shown for this ship, so
     BEQ LL10-1             \ return from the subroutine (as LL10-1 contains an RTS)
    
     EOR XX1+31             \ Otherwise flip bit 3 of byte #31 and store it (which
     STA XX1+31             \ clears bit 3 as we know it was set before the EOR), so
                            \ this sets this ship as no longer being drawn on-screen
    
     JMP LSCLR              \ Jump to LSCLR to draw the ship, which removes it from
                            \ the screen, returning from the subroutine using a
                            \ tail call
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL9 (Part 2 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Check if ship is in field of view, close enough to draw
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_2_of_12.md)
     References: This subroutine is called as follows:
                 * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md) calls via LL10-1
    
    
    
    
    
    * * *
    
    
     This part checks whether the ship is in our field of view, and whether it is
     close enough to be fully drawn (if not, we jump to SHPPT to draw it as a dot).
    
    
    
    * * *
    
    
     Other entry points:
    
       LL10-1               Contains an RTS
    
    
    
    
    .LL10
    
     LDA XX1+7              \ Set A = z_hi
    
     CMP #192               \ If A >= 192 then the ship is a long way away, so jump
     BCS LL14               \ to LL14 to remove the ship from the screen
    
     LDA XX1                \ If x_lo >= z_lo, set the C flag, otherwise clear it
     CMP XX1+6
    
     LDA XX1+1              \ Set A = x_hi - z_hi using the carry from the low
     SBC XX1+7              \ bytes, which sets the C flag as if we had done a full
                            \ two-byte subtraction (x_hi x_lo) - (z_hi z_lo)
    
     BCS LL14               \ If the C flag is set then x >= z, so the ship is
                            \ further to the side than it is in front of us, so it's
                            \ outside our viewing angle of 45 degrees, and we jump
                            \ to LL14 to remove it from the screen
    
     LDA XX1+3              \ If y_lo >= z_lo, set the C flag, otherwise clear it
     CMP XX1+6
    
     LDA XX1+4              \ Set A = y_hi - z_hi using the carry from the low
     SBC XX1+7              \ bytes, which sets the C flag as if we had done a full
                            \ two-byte subtraction (y_hi y_lo) - (z_hi z_lo)
    
     BCS LL14               \ If the C flag is set then y >= z, so the ship is
                            \ further above us than it is in front of us, so it's
                            \ outside our viewing angle of 45 degrees, and we jump
                            \ to LL14 to remove it from the screen
    
     LDY #6                 \ Fetch byte #6 from the ship's blueprint into X, which
     LDA (XX0),Y            \ is the number * 4 of the vertex used for the ship's
     TAX                    \ laser
    
     LDA #255               \ Set bytes X and X+1 of the XX3 heap to 255. We're
     STA XX3,X              \ going to use XX3 to store the screen coordinates of
     STA XX3+1,X            \ all the visible vertices of this ship, so setting the
                            \ laser vertex to 255 means that if we don't update this
                            \ vertex with its screen coordinates in parts 6 and 7,
                            \ this vertex's entry in the XX3 heap will still be 255,
                            \ which we can check in part 9 to see if the laser
                            \ vertex is visible (and therefore whether we should
                            \ draw laser lines if the ship is firing on us)
    
     LDA XX1+6              \ Set (A T) = (z_hi z_lo)
     STA T
     LDA XX1+7
    
     LSR A                  \ Set (A T) = (A T) / 8
     ROR T
     LSR A
     ROR T
     LSR A
     ROR T
    
     LSR A                  \ If A >> 4 is non-zero, i.e. z_hi >= 16, jump to LL13
     BNE LL13               \ as the ship is possibly far away enough to be shown as
                            \ a dot
    
     LDA T                  \ Otherwise the C flag contains the previous bit 0 of A,
     ROR A                  \ which could have been set, so rotate A right four
     LSR A                  \ times so it's in the form %000xxxxx, i.e. z_hi reduced
     LSR A                  \ to a maximum value of 31
     LSR A
    
     STA XX4                \ Store A in XX4, which is now the distance of the ship
                            \ we can use for visibility testing
    
     BPL LL17               \ Jump down to LL17 (this BPL is effectively a JMP as we
                            \ know bit 7 of A is definitely clear)
    
    .LL13
    
                            \ If we get here then the ship is possibly far enough
                            \ away to be shown as a dot
    
     LDY #13                \ Fetch byte #13 from the ship's blueprint, which gives
     LDA (XX0),Y            \ the ship's visibility distance, beyond which we show
                            \ the ship as a dot
    
     CMP XX1+7              \ If z_hi <= the visibility distance, skip to LL17 to
     BCS LL17               \ draw the ship fully, rather than as a dot, as it is
                            \ closer than the visibility distance
    
     LDA #%00100000         \ If bit 5 of the ship's byte #31 is set, then the
     AND XX1+31             \ ship is currently exploding, so skip to LL17 to draw
     BNE LL17               \ the ship's explosion cloud
    
     JMP SHPPT              \ Otherwise jump to SHPPT to draw the ship as a dot,
                            \ returning from the subroutine using a tail call
    
    
    
    
           Name: LL9 (Part 3 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Set up orientation vector, ship coordinate variables
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_3_of_12.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part sets up the following variable blocks:
    
       * XX16 contains the orientation vectors, divided to normalise them
    
       * XX18 contains the ship's x, y and z coordinates in space
    
    
    
    
    .LL17
    
     LDX #5                 \ First we copy the three orientation vectors into XX16,
                            \ so set up a counter in X for the 6 bytes in each
                            \ vector
    
    .LL15
    
     LDA XX1+21,X           \ Copy the X-th byte of sidev to the X-th byte of XX16
     STA XX16,X
    
     LDA XX1+15,X           \ Copy the X-th byte of roofv to XX16+6 to the X-th byte
     STA XX16+6,X           \ of XX16+6
    
     LDA XX1+9,X            \ Copy the X-th byte of nosev to XX16+12 to the X-th
     STA XX16+12,X          \ byte of XX16+12
    
     DEX                    \ Decrement the counter
    
     BPL LL15               \ Loop back to copy the next byte of each vector, until
                            \ we have the following:
                            \
                            \   * XX16(1 0) = sidev_x
                            \   * XX16(3 2) = sidev_y
                            \   * XX16(5 4) = sidev_z
                            \
                            \   * XX16(7 6) = roofv_x
                            \   * XX16(9 8) = roofv_y
                            \   * XX16(11 10) = roofv_z
                            \
                            \   * XX16(13 12) = nosev_x
                            \   * XX16(15 14) = nosev_y
                            \   * XX16(17 16) = nosev_z
    
     LDA #197               \ Set Q = 197
     STA Q
    
     LDY #16                \ Set Y to be a counter that counts down by 2 each time,
                            \ starting with 16, then 14, 12 and so on. We use this
                            \ to work through each of the coordinates in each of the
                            \ orientation vectors
    
    .LL21
    
     LDA XX16,Y             \ Set A = the low byte of the vector coordinate, e.g.
                            \ nosev_z_lo when Y = 16
    
     ASL A                  \ Shift bit 7 into the C flag
    
     LDA XX16+1,Y           \ Set A = the high byte of the vector coordinate, e.g.
                            \ nosev_z_hi when Y = 16
    
     ROL A                  \ Rotate A left, incorporating the C flag, so A now
                            \ contains the original high byte, doubled, and without
                            \ a sign bit, e.g. A = |nosev_z_hi| * 2
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
                            \
                            \ so, for nosev, this would be:
                            \
                            \   R = 256 * |nosev_z_hi| * 2 / 197
                            \     = 2.6 * |nosev_z_hi|
    
     LDX R                  \ Store R in the low byte's location, so we can keep the
     STX XX16,Y             \ old, unscaled high byte intact for the sign
    
     DEY                    \ Decrement the loop counter twice
     DEY
    
     BPL LL21               \ Loop back for the next vector coordinate until we have
                            \ divided them all
    
                            \ By this point, the vectors have been turned into
                            \ scaled magnitudes, so we have the following:
                            \
                            \   * XX16   = scaled |sidev_x|
                            \   * XX16+2 = scaled |sidev_y|
                            \   * XX16+4 = scaled |sidev_z|
                            \
                            \   * XX16+6  = scaled |roofv_x|
                            \   * XX16+8  = scaled |roofv_y|
                            \   * XX16+10 = scaled |roofv_z|
                            \
                            \   * XX16+12 = scaled |nosev_x|
                            \   * XX16+14 = scaled |nosev_y|
                            \   * XX16+16 = scaled |nosev_z|
    
     LDX #8                 \ Next we copy the ship's coordinates into XX18, so set
                            \ up a counter in X for 9 bytes
    
    .ll91
    
     LDA XX1,X              \ Copy the X-th byte from XX1 to XX18
     STA XX18,X
    
     DEX                    \ Decrement the loop counter
    
     BPL ll91               \ Loop back for the next byte until we have copied all
                            \ three coordinates
    
                            \ So we now have the following:
                            \
                            \   * XX18(2 1 0) = (x_sign x_hi x_lo)
                            \
                            \   * XX18(5 4 3) = (y_sign y_hi y_lo)
                            \
                            \   * XX18(8 7 6) = (z_sign z_hi z_lo)
    
     LDA #255               \ Set the 15th byte of XX2 to 255, so that face 15 is
     STA XX2+15             \ always visible. No ship definitions actually have this
                            \ number of faces, but this allows us to force a vertex
                            \ to always be visible by associating it with face 15
                            \ (see the ship blueprints for the Cobra Mk III at
                            \ SHIP_COBRA_MK_3 and the asteroid at SHIP_ASTEROID for
                            \ examples of vertices that are associated with face 15)
    
     LDY #12                \ Set Y = 12 to point to the ship blueprint byte #12,
    
     LDA XX1+31             \ If bit 5 of the ship's byte #31 is clear, then the
     AND #%00100000         \ ship is not currently exploding, so jump down to EE29
     BEQ EE29               \ to skip the following
    
                            \ Otherwise we fall through to set up the visibility
                            \ block for an exploding ship
    
    
    
    
           Name: LL9 (Part 4 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Set visibility for exploding ship (all faces visible)
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_4_of_12.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part sets up the visibility block in XX2 for a ship that is exploding.
    
     The XX2 block consists of one byte for each face in the ship's blueprint,
     which holds the visibility of that face. Because the ship is exploding, we
     want to set all the faces to be visible. A value of 255 in the visibility
     table means the face is visible, so the following code sets each face to 255
     and then skips over the face visibility calculations that we would apply to a
     non-exploding ship.
    
    
    
    
     LDA (XX0),Y            \ Fetch byte #12 of the ship's blueprint, which contains
                            \ the number of faces * 4
    
     LSR A                  \ Set X = A / 4
     LSR A                  \       = the number of faces
     TAX
    
     LDA #255               \ Set A = 255
    
    .EE30
    
     STA XX2,X              \ Set the X-th byte of XX2 to 255
    
     DEX                    \ Decrement the loop counter
    
     BPL EE30               \ Loop back for the next byte until there is one byte
                            \ set to 255 for each face
    
     INX                    \ Set XX4 = 0 for the distance value we use to test
     STX XX4                \ for visibility, so we always shows everything
    
    .LL41
    
     JMP LL42               \ Jump to LL42 to skip the face visibility calculations
                            \ as we don't need to do them now we've set up the XX2
                            \ block for the explosion
    
    
    
    
           Name: LL9 (Part 5 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Calculate the visibility of each of the ship's faces
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
                 [Back-face culling](https://elite.bbcelite.com/deep_dives/back-face_culling.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_5_of_12.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .EE29
    
     LDA (XX0),Y            \ We set Y to 12 above before jumping down to EE29, so
                            \ this fetches byte #12 of the ship's blueprint, which
                            \ contains the number of faces * 4
    
     BEQ LL41               \ If there are no faces in this ship, jump to LL42 (via
                            \ LL41) to skip the face visibility calculations
    
     STA XX20               \ Set A = the number of faces * 4
    
     LDY #18                \ Fetch byte #18 of the ship's blueprint, which contains
     LDA (XX0),Y            \ the factor by which we scale the face normals, into X
     TAX
    
     LDA XX18+7             \ Set A = z_hi
    
    .LL90
    
     TAY                    \ Set Y = z_hi
    
     BEQ LL91               \ If z_hi = 0 then jump to LL91
    
                            \ The following is a loop that jumps back to LL90+3,
                            \ i.e. here. LL90 is only used for this loop, so it's a
                            \ bit of a strange use of the label here
    
     INX                    \ Increment the scale factor in X
    
     LSR XX18+4             \ Divide (y_hi y_lo) by 2
     ROR XX18+3
    
     LSR XX18+1             \ Divide (x_hi x_lo) by 2
     ROR XX18
    
     LSR A                  \ Divide (z_hi z_lo) by 2 (as A contains z_hi)
     ROR XX18+6
    
     TAY                    \ Set Y = z_hi
    
     BNE LL90+3             \ If Y is non-zero, loop back to LL90+3 to divide the
                            \ three coordinates until z_hi is 0
    
    .LL91
    
                            \ By this point z_hi is 0 and X contains the number of
                            \ right shifts we had to do, plus the scale factor from
                            \ the blueprint
    
     STX XX17               \ Store the updated scale factor in XX17
    
     LDA XX18+8             \ Set XX15+5 = z_sign
     STA XX15+5
    
     LDA XX18               \ Set XX15(1 0) = (x_sign x_lo)
     STA XX15
     LDA XX18+2
     STA XX15+1
    
     LDA XX18+3             \ Set XX15(3 2) = (y_sign y_lo)
     STA XX15+2
     LDA XX18+5
     STA XX15+3
    
     LDA XX18+6             \ Set XX15+4 = z_lo, so now XX15(5 4) = (z_sign z_lo)
     STA XX15+4
    
     JSR LL51               \ Call LL51 to set XX12 to the dot products of XX15 and
                            \ XX16, which we'll call dot_sidev, dot_roofv and
                            \ dot_nosev:
                            \
                            \   XX12(1 0) = [x y z] . sidev
                            \             = (dot_sidev_sign dot_sidev_lo)
                            \             = dot_sidev
                            \
                            \   XX12(3 2) = [x y z] . roofv
                            \             = (dot_roofv_sign dot_roofv_lo)
                            \             = dot_roofv
                            \
                            \   XX12(5 4) = [x y z] . nosev
                            \             = (dot_nosev_sign dot_nosev_lo)
                            \             = dot_nosev
    
     LDA XX12               \ Set XX18(2 0) = dot_sidev
     STA XX18
     LDA XX12+1
     STA XX18+2
    
     LDA XX12+2             \ Set XX18(5 3) = dot_roofv
     STA XX18+3
     LDA XX12+3
     STA XX18+5
    
     LDA XX12+4             \ Set XX18(8 6) = dot_nosev
     STA XX18+6
     LDA XX12+5
     STA XX18+8
    
     LDY #4                 \ Fetch byte #4 of the ship's blueprint, which contains
     LDA (XX0),Y            \ the low byte of the offset to the faces data
    
     CLC                    \ Set V = low byte faces offset + XX0
     ADC XX0
     STA V
    
     LDY #17                \ Fetch byte #17 of the ship's blueprint, which contains
     LDA (XX0),Y            \ the high byte of the offset to the faces data
    
     ADC XX0+1              \ Set V+1 = high byte faces offset + XX0+1
     STA V+1                \
                            \ So V(1 0) now points to the start of the faces data
                            \ for this ship
    
     LDY #0                 \ We're now going to loop through all the faces for this
                            \ ship, so set a counter in Y, starting from 0, which we
                            \ will increment by 4 each loop to step through the
                            \ four bytes of data for each face
    
    .LL86
    
     LDA (V),Y              \ Fetch byte #0 for this face into A, so:
                            \
                            \   A = %xyz vvvvv, where:
                            \
                            \     * Bits 0-4 = visibility distance, beyond which the
                            \       face is always shown
                            \
                            \     * Bits 7-5 = the sign bits of normal_x, normal_y
                            \       and normal_z
    
     STA XX12+1             \ Store byte #0 in XX12+1, so XX12+1 now has the sign of
                            \ normal_x
    
     AND #%00011111         \ Extract bits 0-4 to give the visibility distance
    
     CMP XX4                \ If XX4 <= the visibility distance, where XX4 contains
     BCS LL87               \ the ship's z-distance reduced to 0-31 (which we set in
                            \ part 2), skip to LL87 as this face is close enough
                            \ that we have to test its visibility using the face
                            \ normals
    
                            \ Otherwise this face is within range and is therefore
                            \ always shown
    
     TYA                    \ Set X = Y / 4
     LSR A                  \       = the number of this face * 4 /4
     LSR A                  \       = the number of this face
     TAX
    
     LDA #255               \ Set the X-th byte of XX2 to 255 to denote that this
     STA XX2,X              \ face is visible
    
     TYA                    \ Set Y = Y + 4 to point to the next face
     ADC #4
     TAY
    
     JMP LL88               \ Jump down to LL88 to skip the following, as we don't
                            \ need to test the face normals
    
    .LL87
    
     LDA XX12+1             \ Fetch byte #0 for this face into A
    
     ASL A                  \ Shift A left and store it, so XX12+3 now has the sign
     STA XX12+3             \ of normal_y
    
     ASL A                  \ Shift A left and store it, so XX12+5 now has the sign
     STA XX12+5             \ of normal_z
    
     INY                    \ Increment Y to point to byte #1
    
     LDA (V),Y              \ Fetch byte #1 for this face and store in XX12, so
     STA XX12               \ XX12 = normal_x
    
     INY                    \ Increment Y to point to byte #2
    
     LDA (V),Y              \ Fetch byte #2 for this face and store in XX12+2, so
     STA XX12+2             \ XX12+2 = normal_y
    
     INY                    \ Increment Y to point to byte #3
    
     LDA (V),Y              \ Fetch byte #3 for this face and store in XX12+4, so
     STA XX12+4             \ XX12+4 = normal_z
    
                            \ So we now have:
                            \
                            \   XX12(1 0) = (normal_x_sign normal_x)
                            \
                            \   XX12(3 2) = (normal_y_sign normal_y)
                            \
                            \   XX12(5 4) = (normal_z_sign normal_z)
    
     LDX XX17               \ If XX17 < 4 then jump to LL92, otherwise we stored a
     CPX #4                 \ larger scale factor above
     BCC LL92
    
    .LL143
    
     LDA XX18               \ Set XX15(1 0) = XX18(2 0)
     STA XX15               \               = dot_sidev
     LDA XX18+2
     STA XX15+1
    
     LDA XX18+3             \ Set XX15(3 2) = XX18(5 3)
     STA XX15+2             \               = dot_roofv
     LDA XX18+5
     STA XX15+3
    
     LDA XX18+6             \ Set XX15(5 4) = XX18(8 6)
     STA XX15+4             \               = dot_nosev
     LDA XX18+8
     STA XX15+5
    
     JMP LL89               \ Jump down to LL89
    
    .ovflw
    
                            \ If we get here then the addition below overflowed, so
                            \ we halve the dot products and normal vector
    
     LSR XX18               \ Divide dot_sidev_lo by 2, so dot_sidev = dot_sidev / 2
    
     LSR XX18+6             \ Divide dot_nosev_lo by 2, so dot_nosev = dot_nosev / 2
    
     LSR XX18+3             \ Divide dot_roofv_lo by 2, so dot_roofv = dot_roofv / 2
    
     LDX #1                 \ Set X = 1 so when we fall through into LL92, we divide
                            \ the normal vector by 2 as well
    
    .LL92
    
                            \ We jump here from above with the scale factor in X,
                            \ and now we apply it by scaling the normal vector down
                            \ by a factor of 2^X (i.e. divide by 2^X)
    
     LDA XX12               \ Set XX15 = normal_x
     STA XX15
    
     LDA XX12+2             \ Set XX15+2 = normal_y
     STA XX15+2
    
     LDA XX12+4             \ Set A = normal_z
    
    .LL93
    
     DEX                    \ Decrement the scale factor in X
    
     BMI LL94               \ If X was 0 before the decrement, there is no scaling
                            \ to do, so jump to LL94 to exit the loop
    
     LSR XX15               \ Set XX15 = XX15 / 2
                            \          = normal_x / 2
    
     LSR XX15+2             \ Set XX15+2 = XX15+2 / 2
                            \            = normal_y / 2
    
     LSR A                  \ Set A = A / 2
                            \       = normal_z / 2
    
     DEX                    \ Decrement the scale factor in X
    
     BPL LL93+3             \ If we have more scaling to do, loop back up to the
                            \ first LSR above until the normal vector is scaled down
    
    .LL94
    
     STA R                  \ Set R = normal_z
    
     LDA XX12+5             \ Set S = normal_z_sign
     STA S
    
     LDA XX18+6             \ Set Q = dot_nosev_lo
     STA Q
    
     LDA XX18+8             \ Set A = dot_nosev_sign
    
     JSR LL38               \ Set (S A) = (S R) + (A Q)
                            \           = normal_z + dot_nosev
                            \
                            \ setting the sign of the result in S
    
     BCS ovflw              \ If the addition overflowed, jump up to ovflw to divide
                            \ both the normal vector and dot products by 2 and try
                            \ again
    
     STA XX15+4             \ Set XX15(5 4) = (S A)
     LDA S                  \               = normal_z + dot_nosev
     STA XX15+5
    
     LDA XX15               \ Set R = normal_x
     STA R
    
     LDA XX12+1             \ Set S = normal_x_sign
     STA S
    
     LDA XX18               \ Set Q = dot_sidev_lo
     STA Q
    
     LDA XX18+2             \ Set A = dot_sidev_sign
    
     JSR LL38               \ Set (S A) = (S R) + (A Q)
                            \           = normal_x + dot_sidev
                            \
                            \ setting the sign of the result in S
    
     BCS ovflw              \ If the addition overflowed, jump up to ovflw to divide
                            \ both the normal vector and dot products by 2 and try
                            \ again
    
     STA XX15               \ Set XX15(1 0) = (S A)
     LDA S                  \               = normal_x + dot_sidev
     STA XX15+1
    
     LDA XX15+2             \ Set R = normal_y
     STA R
    
     LDA XX12+3             \ Set S = normal_y_sign
     STA S
    
     LDA XX18+3             \ Set Q = dot_roofv_lo
     STA Q
    
     LDA XX18+5             \ Set A = dot_roofv_sign
    
     JSR LL38               \ Set (S A) = (S R) + (A Q)
                            \           = normal_y + dot_roofv
    
     BCS ovflw              \ If the addition overflowed, jump up to ovflw to divide
                            \ both the normal vector and dot products by 2 and try
                            \ again
    
     STA XX15+2             \ Set XX15(3 2) = (S A)
     LDA S                  \               = normal_y + dot_roofv
     STA XX15+3
    
    .LL89
    
                            \ When we get here, we have set up the following:
                            \
                            \   XX15(1 0) = normal_x + dot_sidev
                            \             = normal_x + [x y z] . sidev
                            \
                            \   XX15(3 2) = normal_y + dot_roofv
                            \             = normal_y + [x y z] . roofv
                            \
                            \   XX15(5 4) = normal_z + dot_nosev
                            \             = normal_z + [x y z] . nosev
                            \
                            \ and:
                            \
                            \   XX12(1 0) = (normal_x_sign normal_x)
                            \
                            \   XX12(3 2) = (normal_y_sign normal_y)
                            \
                            \   XX12(5 4) = (normal_z_sign normal_z)
                            \
                            \ We now calculate the dot product XX12 . XX15 to tell
                            \ us whether or not this face is visible
    
     LDA XX12               \ Set Q = XX12
     STA Q
    
     LDA XX15               \ Set A = XX15
    
     JSR FMLTU              \ Set T = A * Q / 256
     STA T                  \       = XX15 * XX12 / 256
    
     LDA XX12+1             \ Set S = sign of XX15(1 0) * XX12(1 0), so:
     EOR XX15+1             \
     STA S                  \   (S T) = XX15(1 0) * XX12(1 0) / 256
    
     LDA XX12+2             \ Set Q = XX12+2
     STA Q
    
     LDA XX15+2             \ Set A = XX15+2
    
     JSR FMLTU              \ Set Q = A * Q
     STA Q                  \       = XX15+2 * XX12+2 / 256
    
     LDA T                  \ Set T = R, so now:
     STA R                  \
                            \   (S R) = XX15(1 0) * XX12(1 0) / 256
    
     LDA XX12+3             \ Set A = sign of XX15+3 * XX12+3, so:
     EOR XX15+3             \
                            \   (A Q) = XX15(3 2) * XX12(3 2) / 256
    
     JSR LL38               \ Set (S T) = (S R) + (A Q)
     STA T                  \           =   XX15(1 0) * XX12(1 0) / 256
                            \             + XX15(3 2) * XX12(3 2) / 256
    
     LDA XX12+4             \ Set Q = XX12+4
     STA Q
    
     LDA XX15+4             \ Set A = XX15+4
    
     JSR FMLTU              \ Set Q = A * Q
     STA Q                  \       = XX15+4 * XX12+4 / 256
    
     LDA T                  \ Set T = R, so now:
     STA R                  \
                            \   (S R) =   XX15(1 0) * XX12(1 0) / 256
                            \           + XX15(3 2) * XX12(3 2) / 256
    
     LDA XX15+5             \ Set A = sign of XX15+5 * XX12+5, so:
     EOR XX12+5             \
                            \   (A Q) = XX15(5 4) * XX12(5 4) / 256
    
     JSR LL38               \ Set (S A) = (S R) + (A Q)
                            \           =   XX15(1 0) * XX12(1 0) / 256
                            \             + XX15(3 2) * XX12(3 2) / 256
                            \             + XX15(5 4) * XX12(5 4) / 256
    
     PHA                    \ Push the result A onto the stack, so the stack now
                            \ contains the dot product XX12 . XX15
    
     TYA                    \ Set X = Y / 4
     LSR A                  \       = the number of this face * 4 /4
     LSR A                  \       = the number of this face
     TAX
    
     PLA                    \ Pull the dot product off the stack into A
    
     BIT S                  \ If bit 7 of S is set, i.e. the dot product is
     BMI P%+4               \ negative, then this face is visible as its normal is
                            \ pointing towards us, so skip the following instruction
    
     LDA #0                 \ Otherwise the face is not visible, so set A = 0 so we
                            \ can store this to mean "not visible"
    
     STA XX2,X              \ Store the face's visibility in the X-th byte of XX2
    
     INY                    \ Above we incremented Y to point to byte #3, so this
                            \ increments Y to point to byte #4, i.e. byte #0 of the
                            \ next face
    
    .LL88
    
     CPY XX20               \ If Y >= XX20, the number of faces * 4, jump down to
     BCS LL42               \ LL42 to move on to the
    
     JMP LL86               \ Otherwise loop back to LL86 to work out the visibility
                            \ of the next face
    
    
    
    
           Name: LL9 (Part 6 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Calculate the visibility of each of the ship's vertices
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
                 [Calculating vertex coordinates](https://elite.bbcelite.com/deep_dives/calculating_vertex_coordinates.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_6_of_12.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section calculates the visibility of each of the ship's vertices, and for
     those that are visible, it starts the process of calculating the screen
     coordinates of each vertex
    
    
    
    
    .LL42
    
                            \ The first task is to set up the inverse matrix, ready
                            \ for us to send to the dot product routine at LL51.
                            \ Back up in part 3, we set up the following variables:
                            \
                            \   * XX16(1 0) = sidev_x
                            \   * XX16(3 2) = sidev_y
                            \   * XX16(5 4) = sidev_z
                            \
                            \   * XX16(7 6) = roofv_x
                            \   * XX16(9 8) = roofv_y
                            \   * XX16(11 10) = roofv_z
                            \
                            \   * XX16(13 12) = nosev_x
                            \   * XX16(15 14) = nosev_y
                            \   * XX16(17 16) = nosev_z
                            \
                            \ and we then scaled the vectors to give the following:
                            \
                            \   * XX16   = scaled |sidev_x|
                            \   * XX16+2 = scaled |sidev_y|
                            \   * XX16+4 = scaled |sidev_z|
                            \
                            \   * XX16+6  = scaled |roofv_x|
                            \   * XX16+8  = scaled |roofv_y|
                            \   * XX16+10 = scaled |roofv_z|
                            \
                            \   * XX16+12 = scaled |nosev_x|
                            \   * XX16+14 = scaled |nosev_y|
                            \   * XX16+16 = scaled |nosev_z|
                            \
                            \ We now need to rearrange these locations so they
                            \ effectively transpose the matrix into its inverse
    
     LDY XX16+2             \ Set XX16+2 = XX16+6 = scaled |roofv_x|
     LDX XX16+3             \ Set XX16+3 = XX16+7 = roofv_x_hi
     LDA XX16+6             \ Set XX16+6 = XX16+2 = scaled |sidev_y|
     STA XX16+2             \ Set XX16+7 = XX16+3 = sidev_y_hi
     LDA XX16+7
     STA XX16+3
     STY XX16+6
     STX XX16+7
    
     LDY XX16+4             \ Set XX16+4 = XX16+12 = scaled |nosev_x|
     LDX XX16+5             \ Set XX16+5 = XX16+13 = nosev_x_hi
     LDA XX16+12            \ Set XX16+12 = XX16+4 = scaled |sidev_z|
     STA XX16+4             \ Set XX16+13 = XX16+5 = sidev_z_hi
     LDA XX16+13
     STA XX16+5
     STY XX16+12
     STX XX16+13
    
     LDY XX16+10            \ Set XX16+10 = XX16+14 = scaled |nosev_y|
     LDX XX16+11            \ Set XX16+11 = XX16+15 = nosev_y_hi
     LDA XX16+14            \ Set XX16+14 = XX16+10 = scaled |roofv_z|
     STA XX16+10            \ Set XX16+15 = XX16+11 = roofv_z
     LDA XX16+15
     STA XX16+11
     STY XX16+14
     STX XX16+15
    
                            \ So now we have the following sign-magnitude variables
                            \ containing parts of the scaled orientation vectors:
                            \
                            \   XX16(1 0)   = scaled sidev_x
                            \   XX16(3 2)   = scaled roofv_x
                            \   XX16(5 4)   = scaled nosev_x
                            \
                            \   XX16(7 6)   = scaled sidev_y
                            \   XX16(9 8)   = scaled roofv_y
                            \   XX16(11 10) = scaled nosev_y
                            \
                            \   XX16(13 12) = scaled sidev_z
                            \   XX16(15 14) = scaled roofv_z
                            \   XX16(17 16) = scaled nosev_z
                            \
                            \ which is what we want, as the various vectors are now
                            \ arranged so we can use LL51 to multiply by the
                            \ transpose (i.e. the inverse of the matrix)
    
     LDY #8                 \ Fetch byte #8 of the ship's blueprint, which is the
     LDA (XX0),Y            \ number of vertices * 8, and store it in XX20
     STA XX20
    
                            \ We now set V(1 0) = XX0(1 0) + 20, so V(1 0) points
                            \ to byte #20 of the ship's blueprint, which is always
                            \ where the vertex data starts (i.e. just after the 20
                            \ byte block that define the ship's characteristics)
    
     LDA XX0                \ We start with the low bytes
     CLC
     ADC #20
     STA V
    
     LDA XX0+1              \ And then do the high bytes
     ADC #0
     STA V+1
    
     LDY #0                 \ We are about to step through all the vertices, using
                            \ Y as a counter. There are six data bytes for each
                            \ vertex, so we will increment Y by 6 for each iteration
                            \ so it can act as an offset from V(1 0) to the current
                            \ vertex's data
    
     STY CNT                \ Set CNT = 0, which we will use as a pointer to the
                            \ heap at XX3, starting it at zero so the heap starts
                            \ out empty
    
    .LL48
    
     STY XX17               \ Set XX17 = Y, so XX17 now contains the offset of the
                            \ current vertex's data
    
     LDA (V),Y              \ Fetch byte #0 for this vertex into XX15, so:
     STA XX15               \
                            \   XX15 = magnitude of the vertex's x-coordinate
    
     INY                    \ Increment Y to point to byte #1
    
     LDA (V),Y              \ Fetch byte #1 for this vertex into XX15+2, so:
     STA XX15+2             \
                            \   XX15+2 = magnitude of the vertex's y-coordinate
    
     INY                    \ Increment Y to point to byte #2
    
     LDA (V),Y              \ Fetch byte #2 for this vertex into XX15+4, so:
     STA XX15+4             \
                            \   XX15+4 = magnitude of the vertex's z-coordinate
    
     INY                    \ Increment Y to point to byte #3
    
     LDA (V),Y              \ Fetch byte #3 for this vertex into T, so:
     STA T                  \
                            \   T = %xyz vvvvv, where:
                            \
                            \     * Bits 0-4 = visibility distance, beyond which the
                            \                  vertex is not shown
                            \
                            \     * Bits 7-5 = the sign bits of x, y and z
    
     AND #%00011111         \ Extract bits 0-4 to get the visibility distance
    
     CMP XX4                \ If XX4 > the visibility distance, where XX4 contains
     BCC LL49-3             \ the ship's z-distance reduced to 0-31 (which we set in
                            \ part 2), then this vertex is too far away to be
                            \ visible, so jump down to LL50 (via the JMP instruction
                            \ in LL49-3) to move on to the next vertex
    
     INY                    \ Increment Y to point to byte #4
    
     LDA (V),Y              \ Fetch byte #4 for this vertex into P, so:
     STA P                  \
                            \  P = %ffff ffff, where:
                            \
                            \    * Bits 0-3 = the number of face 1
                            \
                            \    * Bits 4-7 = the number of face 2
    
     AND #%00001111         \ Extract the number of face 1 into X
     TAX
    
     LDA XX2,X              \ If XX2+X is non-zero then we decided in part 5 that
     BNE LL49               \ face 1 is visible, so jump to LL49
    
     LDA P                  \ Fetch byte #4 for this vertex into A
    
     LSR A                  \ Shift right four times to extract the number of face 2
     LSR A                  \ from bits 4-7 into X
     LSR A
     LSR A
     TAX
    
     LDA XX2,X              \ If XX2+X is non-zero then we decided in part 5 that
     BNE LL49               \ face 2 is visible, so jump to LL49
    
     INY                    \ Increment Y to point to byte #5
    
     LDA (V),Y              \ Fetch byte #5 for this vertex into P, so:
     STA P                  \
                            \  P = %ffff ffff, where:
                            \
                            \    * Bits 0-3 = the number of face 3
                            \
                            \    * Bits 4-7 = the number of face 4
    
     AND #%00001111         \ Extract the number of face 1 into X
     TAX
    
     LDA XX2,X              \ If XX2+X is non-zero then we decided in part 5 that
     BNE LL49               \ face 3 is visible, so jump to LL49
    
     LDA P                  \ Fetch byte #5 for this vertex into A
    
     LSR A                  \ Shift right four times to extract the number of face 4
     LSR A                  \ from bits 4-7 into X
     LSR A
     LSR A
     TAX
    
     LDA XX2,X              \ If XX2+X is non-zero then we decided in part 5 that
     BNE LL49               \ face 4 is visible, so jump to LL49
    
     JMP LL50               \ If we get here then none of the four faces associated
                            \ with this vertex are visible, so this vertex is also
                            \ not visible, so jump to LL50 to move on to the next
                            \ vertex
    
    .LL49
    
     LDA T                  \ Fetch byte #5 for this vertex into A and store it, so
     STA XX15+1             \ XX15+1 now has the sign of the vertex's x-coordinate
    
     ASL A                  \ Shift A left and store it, so XX15+3 now has the sign
     STA XX15+3             \ of the vertex's y-coordinate
    
     ASL A                  \ Shift A left and store it, so XX15+5 now has the sign
     STA XX15+5             \ of the vertex's z-coordinate
    
                            \ By this point we have the following:
                            \
                            \   XX15(1 0) = vertex x-coordinate
                            \   XX15(3 2) = vertex y-coordinate
                            \   XX15(5 4) = vertex z-coordinate
                            \
                            \   XX16(1 0)   = scaled sidev_x
                            \   XX16(3 2)   = scaled roofv_x
                            \   XX16(5 4)   = scaled nosev_x
                            \
                            \   XX16(7 6)   = scaled sidev_y
                            \   XX16(9 8)   = scaled roofv_y
                            \   XX16(11 10) = scaled nosev_y
                            \
                            \   XX16(13 12) = scaled sidev_z
                            \   XX16(15 14) = scaled roofv_z
                            \   XX16(17 16) = scaled nosev_z
    
     JSR LL51               \ Call LL51 to set XX12 to the dot products of XX15 and
                            \ XX16, as follows:
                            \
                            \   XX12(1 0) = [ x y z ] . [ sidev_x roofv_x nosev_x ]
                            \
                            \   XX12(3 2) = [ x y z ] . [ sidev_y roofv_y nosev_y ]
                            \
                            \   XX12(5 4) = [ x y z ] . [ sidev_z roofv_z nosev_z ]
                            \
                            \ XX12 contains the vector from the ship's centre to
                            \ the vertex, transformed from the orientation vector
                            \ space to the universe orientated around our ship. So
                            \ we can refer to this vector below, let's call it
                            \ vertv, so:
                            \
                            \   vertv_x = [ x y z ] . [ sidev_x roofv_x nosev_x ]
                            \
                            \   vertv_y = [ x y z ] . [ sidev_y roofv_y nosev_y ]
                            \
                            \   vertv_z = [ x y z ] . [ sidev_z roofv_z nosev_z ]
                            \
                            \ To finish the calculation, we now want to calculate:
                            \
                            \   vertv + [ x y z ]
                            \
                            \ So let's start with the vertv_x + x
    
     LDA XX1+2              \ Set A = x_sign of the ship's location
    
     STA XX15+2             \ Set XX15+2 = x_sign
    
     EOR XX12+1             \ If the sign of x_sign * the sign of vertv_x is
     BMI LL52               \ negative (i.e. they have different signs), skip to
                            \ LL52
    
     CLC                    \ Set XX15(2 1 0) = XX1(2 1 0) + XX12(1 0)
     LDA XX12               \                 = (x_sign x_hi x_lo) + vertv_x
     ADC XX1                \
     STA XX15               \ Starting with the low bytes
    
     LDA XX1+1              \ And then doing the high bytes (we can add 0 here as
     ADC #0                 \ we know the sign byte of vertv_x is 0)
     STA XX15+1
    
     JMP LL53               \ We've added the x-coordinates, so jump to LL53 to do
                            \ the y-coordinates
    
    .LL52
    
                            \ If we get here then x_sign and vertv_x have different
                            \ signs, so we need to subtract them to get the result
    
     LDA XX1                \ Set XX15(2 1 0) = XX1(2 1 0) - XX12(1 0)
     SEC                    \                 = (x_sign x_hi x_lo) - vertv_x
     SBC XX12               \
     STA XX15               \ Starting with the low bytes
    
     LDA XX1+1              \ And then doing the high bytes (we can subtract 0 here
     SBC #0                 \ as we know the sign byte of vertv_x is 0)
     STA XX15+1
    
     BCS LL53               \ If the subtraction didn't underflow, then the sign of
                            \ the result is the same sign as x_sign, and that's what
                            \ we want, so we can jump down to LL53 to do the
                            \ y-coordinates
    
     EOR #%11111111         \ Otherwise we need to negate the result using two's
     STA XX15+1             \ complement, so first we flip the bits of the high byte
    
     LDA #1                 \ And then subtract the low byte from 1
     SBC XX15
     STA XX15
    
     BCC P%+4               \ If the above subtraction underflowed then we need to
     INC XX15+1             \ bump the high byte of the result up by 1
    
     LDA XX15+2             \ And now we flip the sign of the result to get the
     EOR #%10000000         \ correct result
     STA XX15+2
    
    .LL53
    
                            \ Now for the y-coordinates, vertv_y + y
    
     LDA XX1+5              \ Set A = y_sign of the ship's location
    
     STA XX15+5             \ Set XX15+5 = y_sign
    
     EOR XX12+3             \ If the sign of y_sign * the sign of vertv_y is
     BMI LL54               \ negative (i.e. they have different signs), skip to
                            \ LL54
    
     CLC                    \ Set XX15(5 4 3) = XX1(5 4 3) + XX12(3 2)
     LDA XX12+2             \                 = (y_sign y_hi y_lo) + vertv_y
     ADC XX1+3              \
     STA XX15+3             \ Starting with the low bytes
    
     LDA XX1+4              \ And then doing the high bytes (we can add 0 here as
     ADC #0                 \ we know the sign byte of vertv_y is 0)
     STA XX15+4
    
     JMP LL55               \ We've added the y-coordinates, so jump to LL55 to do
                            \ the z-coordinates
    
    .LL54
    
                            \ If we get here then y_sign and vertv_y have different
                            \ signs, so we need to subtract them to get the result
    
     LDA XX1+3              \ Set XX15(5 4 3) = XX1(5 4 3) - XX12(3 2)
     SEC                    \                 = (y_sign y_hi y_lo) - vertv_y
     SBC XX12+2             \
     STA XX15+3             \ Starting with the low bytes
    
     LDA XX1+4              \ And then doing the high bytes (we can subtract 0 here
     SBC #0                 \ as we know the sign byte of vertv_z is 0)
     STA XX15+4
    
     BCS LL55               \ If the subtraction didn't underflow, then the sign of
                            \ the result is the same sign as y_sign, and that's what
                            \ we want, so we can jump down to LL55 to do the
                            \ z-coordinates
    
     EOR #%11111111         \ Otherwise we need to negate the result using two's
     STA XX15+4             \ complement, so first we flip the bits of the high byte
    
     LDA XX15+3             \ And then flip the bits of the low byte and add 1
     EOR #%11111111
     ADC #1
     STA XX15+3
    
     LDA XX15+5             \ And now we flip the sign of the result to get the
     EOR #%10000000         \ correct result
     STA XX15+5
    
     BCC LL55               \ If the above subtraction underflowed then we need to
     INC XX15+4             \ bump the high byte of the result up by 1
    
    .LL55
    
                            \ Now for the z-coordinates, vertv_z + z
    
     LDA XX12+5             \ If vertv_z_hi is negative, jump down to LL56
     BMI LL56
    
     LDA XX12+4             \ Set (U T) = XX1(7 6) + XX12(5 4)
     CLC                    \           = (z_hi z_lo) + vertv_z
     ADC XX1+6              \
     STA T                  \ Starting with the low bytes
    
     LDA XX1+7              \ And then doing the high bytes (we can add 0 here as
     ADC #0                 \ we know the sign byte of vertv_y is 0)
     STA U
    
     JMP LL57               \ We've added the z-coordinates, so jump to LL57
    
                            \ The adding process is continued in part 7, after a
                            \ couple of subroutines that we don't need quite yet
    
    
    
    
           Name: LL61                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (U R) = 256 * A / Q
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll61.md)
     References: This subroutine is called as follows:
                 * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md) calls LL61
    
    
    
    
    
    * * *
    
    
     Calculate the following, where A >= Q:
    
       (U R) = 256 * A / Q
    
     This is a sister routine to LL28, which does the division when A < Q.
    
    
    
    
    .LL61
    
     LDX Q                  \ If Q = 0, jump down to LL84 to return a division
     BEQ LL84               \ error
    
                            \ The LL28 routine returns A / Q, but only if A < Q. In
                            \ our case A >= Q, but we still want to use the LL28
                            \ routine, so we halve A until it's less than Q, call
                            \ the division routine, and then double A by the same
                            \ number of times
    
     LDX #0                 \ Set X = 0 to count the number of times we halve A
    
    .LL63
    
     LSR A                  \ Halve A by shifting right
    
     INX                    \ Increment X
    
     CMP Q                  \ If A >= Q, loop back to LL63 to halve it again
     BCS LL63
    
     STX S                  \ Otherwise store the number of times we halved A in S
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
                            \
                            \ which we can do now as A < Q
    
     LDX S                  \ Otherwise restore the number of times we halved A
                            \ above into X
    
     LDA R                  \ Set A = our division result
    
    .LL64
    
     ASL A                  \ Double (U A) by shifting left
     ROL U
    
     BMI LL84               \ If bit 7 of U is set, the doubling has overflowed, so
                            \ jump to LL84 to return a division error
    
     DEX                    \ Decrement X
    
     BNE LL64               \ If X is not yet zero then we haven't done as many
                            \ doublings as we did halvings earlier, so loop back for
                            \ another doubling
    
     STA R                  \ Store the low byte of the division result in R
    
     RTS                    \ Return from the subroutine
    
    .LL84
    
     LDA #50                \ If we get here then either we tried to divide by 0, or
     STA R                  \ the result overflowed, so we set U and R to 50
     STA U
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL62                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate 128 - (U R)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll62.md)
     References: This subroutine is called as follows:
                 * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md) calls LL62
    
    
    
    
    
    * * *
    
    
     Calculate the following for a positive sign-magnitude number (U R):
    
       128 - (U R)
    
     and then store the result, low byte then high byte, on the end of the heap at
     XX3, where X points to the first free byte on the heap. Return by jumping down
     to LL66.
    
    
    
    * * *
    
    
     Returns:
    
       X                    X is incremented by 1
    
    
    
    
    .LL62
    
     LDA #128               \ Calculate 128 - (U R), starting with the low bytes
     SEC
     SBC R
    
     STA XX3,X              \ Store the low byte of the result in the X-th byte of
                            \ the heap at XX3
    
     INX                    \ Increment the heap pointer in X to point to the next
                            \ byte
    
     LDA #0                 \ And then subtract the high bytes
     SBC U
    
     STA XX3,X              \ Store the low byte of the result in the X-th byte of
                            \ the heap at XX3
    
     JMP LL66               \ Jump down to LL66
    
    
    
    
           Name: LL9 (Part 7 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Calculate the visibility of each of the ship's vertices
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
                 [Calculating vertex coordinates](https://elite.bbcelite.com/deep_dives/calculating_vertex_coordinates.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_7_of_12.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section continues the coordinate adding from part 6 by finishing off the
     calculation that we started above:
    
                          [ sidev_x roofv_x nosev_x ]   [ x ]   [ x ]
       vector to vertex = [ sidev_y roofv_y nosev_y ] . [ y ] + [ y ]
                          [ sidev_z roofv_z nosev_z ]   [ z ]   [ z ]
    
     The gets stored as follows, in sign-magnitude values with the magnitudes
     fitting into the low bytes:
    
       XX15(2 0)           [ x y z ] . [ sidev_x roofv_x nosev_x ] + [ x y z ]
    
       XX15(5 3)           [ x y z ] . [ sidev_y roofv_y nosev_y ] + [ x y z ]
    
       (U T)               [ x y z ] . [ sidev_z roofv_z nosev_z ] + [ x y z ]
    
     Finally, because this vector is from our ship to the vertex, and we are at the
     origin, this vector is the same as the coordinates of the vertex. In other
     words, we have just worked out:
    
       XX15(2 0)           x-coordinate of the current vertex
    
       XX15(5 3)           y-coordinate of the current vertex
    
       (U T)               z-coordinate of the current vertex
    
    
    
    
    .LL56
    
     LDA XX1+6              \ Set (U T) = XX1(7 6) - XX12(5 4)
     SEC                    \           = (z_hi z_lo) - vertv_z
     SBC XX12+4             \
     STA T                  \ Starting with the low bytes
    
     LDA XX1+7              \ And then doing the high bytes (we can subtract 0 here
     SBC #0                 \ as we know the sign byte of vertv_z is 0)
     STA U
    
     BCC LL140              \ If the subtraction just underflowed, skip to LL140 to
                            \ set (U T) to the minimum value of 4
    
     BNE LL57               \ If U is non-zero, jump down to LL57
    
     LDA T                  \ If T >= 4, jump down to LL57
     CMP #4
     BCS LL57
    
    .LL140
    
     LDA #0                 \ If we get here then either (U T) < 4 or the
     STA U                  \ subtraction underflowed, so set (U T) = 4
     LDA #4
     STA T
    
    .LL57
    
                            \ By this point we have our results, so now to scale
                            \ the 16-bit results down into 8-bit values
    
     LDA U                  \ If the high bytes of the result are all zero, we are
     ORA XX15+1             \ done, so jump down to LL60 for the next stage
     ORA XX15+4
     BEQ LL60
    
     LSR XX15+1             \ Shift XX15(1 0) to the right
     ROR XX15
    
     LSR XX15+4             \ Shift XX15(4 3) to the right
     ROR XX15+3
    
     LSR U                  \ Shift (U T) to the right
     ROR T
    
     JMP LL57               \ Jump back to LL57 to see if we can shift the result
                            \ any more
    
    
    
    
           Name: LL9 (Part 8 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Calculate the screen coordinates of visible vertices
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_8_of_12.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll9_part_8_of_12.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL62](../main/subroutine/ll62.md) calls via LL66
    
    
    
    
    
    * * *
    
    
     This section projects the coordinate of the vertex into screen coordinates and
     stores them on the XX3 heap. By the end of this part, the XX3 heap contains
     four bytes containing the 16-bit screen coordinates of the current vertex, in
     the order: x_lo, x_hi, y_lo, y_hi.
    
     When we reach here, we are looping through the vertices, and we've just worked
     out the coordinates of the vertex in our normal coordinate system, as follows
    
       XX15(2 0)           (x_sign x_lo) = x-coordinate of the current vertex
    
       XX15(5 3)           (y_sign y_lo) = y-coordinate of the current vertex
    
       (U T)               (z_sign z_lo) = z-coordinate of the current vertex
    
     Note that U is always zero when we get to this point, as the vertex is always
     in front of us (so it has a positive z-coordinate, into the screen).
    
    
    
    * * *
    
    
     Other entry points:
    
       LL70+1               Contains an RTS (as the first byte of an LDA
                            instruction)
    
       LL66                 A re-entry point into the ship-drawing routine, used by
                            the LL62 routine to store 128 - (U R) on the XX3 heap
    
    
    
    
    .LL60
    
     LDA T                  \ Set Q = z_lo
     STA Q
    
     LDA XX15               \ Set A = x_lo
    
     CMP Q                  \ If x_lo < z_lo jump to LL69
     BCC LL69
    
     JSR LL61               \ Call LL61 to calculate:
                            \
                            \   (U R) = 256 * A / Q
                            \         = 256 * x / z
                            \
                            \ which we can do as x >= z
    
     JMP LL69+3             \ Jump over the next instruction to skip the division
                            \ for x_lo < z_lo
    
    .LL69
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
                            \     = 256 * x / z
                            \
                            \ Because x < z, the result fits into one byte, and we
                            \ also know that U = 0, so (U R) also contains the
                            \ result
    
                            \ At this point we have:
                            \
                            \   (U R) = x / z
                            \
                            \ so (U R) contains the vertex's x-coordinate projected
                            \ on screen
                            \
                            \ The next task is to convert (U R) to a pixel screen
                            \ coordinate and stick it on the XX3 heap.
                            \
                            \ We start with the x-coordinate. To convert the
                            \ x-coordinate to a screen pixel we add 128, the
                            \ x-coordinate of the centre of the screen, because the
                            \ projected value is relative to an origin at the centre
                            \ of the screen, but the origin of the screen pixels is
                            \ at the top-left of the screen
    
     LDX CNT                \ Fetch the pointer to the end of the XX3 heap from CNT
                            \ into X
    
     LDA XX15+2             \ If x_sign is negative, jump up to LL62, which will
     BMI LL62               \ store 128 - (U R) on the XX3 heap and return by
                            \ jumping down to LL66 below
    
     LDA R                  \ Calculate 128 + (U R), starting with the low bytes
     CLC
     ADC #128
    
     STA XX3,X              \ Store the low byte of the result in the X-th byte of
                            \ the heap at XX3
    
     INX                    \ Increment the heap pointer in X to point to the next
                            \ byte
    
     LDA U                  \ And then add the high bytes
     ADC #0
    
     STA XX3,X              \ Store the high byte of the result in the X-th byte of
                            \ the heap at XX3
    
    .LL66
    
                            \ We've just stored the screen x-coordinate of the
                            \ vertex on the XX3 heap, so now for the y-coordinate
    
     TXA                    \ Store the heap pointer in X on the stack (at this
     PHA                    \ it points to the last entry on the heap, not the first
                            \ free byte)
    
     LDA #0                 \ Set U = 0
     STA U
    
     LDA T                  \ Set Q = z_lo
     STA Q
    
     LDA XX15+3             \ Set A = y_lo
    
     CMP Q                  \ If y_lo < z_lo jump to LL67
     BCC LL67
    
     JSR LL61               \ Call LL61 to calculate:
                            \
                            \   (U R) = 256 * A / Q
                            \         = 256 * y / z
                            \
                            \ which we can do as y >= z
    
     JMP LL68               \ Jump to LL68 to skip the division for y_lo < z_lo
    
    .LL70
    
                            \ This gets called from below when y_sign is negative
    
     LDA #Y                 \ Calculate #Y + (U R), starting with the low bytes
     CLC
     ADC R
    
     STA XX3,X              \ Store the low byte of the result in the X-th byte of
                            \ the heap at XX3
    
     INX                    \ Increment the heap pointer in X to point to the next
                            \ byte
    
     LDA #0                 \ And then add the high bytes
     ADC U
    
     STA XX3,X              \ Store the high byte of the result in the X-th byte of
                            \ the heap at XX3
    
     JMP LL50               \ Jump to LL50 to move on to the next vertex
    
    .LL67
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
                            \     = 256 * y / z
                            \
                            \ Because y < z, the result fits into one byte, and we
                            \ also know that U = 0, so (U R) also contains the
                            \ result
    
    .LL68
    
                            \ At this point we have:
                            \
                            \   (U R) = y / z
                            \
                            \ so (U R) contains the vertex's y-coordinate projected
                            \ on screen
                            \
                            \ We now want to convert this to a screen y-coordinate
                            \ and stick it on the XX3 heap, much like we did with
                            \ the x-coordinate above. Again, we convert the
                            \ coordinate by adding or subtracting the y-coordinate
                            \ of the centre of the screen, which is in the constant
                            \ #Y, but this time we do the opposite, as a positive
                            \ projected y-coordinate, i.e. up the space y-axis and
                            \ up the screen, converts to a low y-coordinate, which
                            \ is the opposite way round to the x-coordinates
    
     PLA                    \ Restore the heap pointer from the stack into X
     TAX
    
     INX                    \ When we stored the heap pointer, it pointed to the
                            \ last entry on the heap, not the first free byte, so we
                            \ increment it so it does point to the next free byte
    
     LDA XX15+5             \ If y_sign is negative, jump up to LL70, which will
     BMI LL70               \ store #Y + (U R) on the XX3 heap and return by jumping
                            \ down to LL50 below
    
     LDA #Y                 \ Calculate #Y - (U R), starting with the low bytes
     SEC
     SBC R
    
     STA XX3,X              \ Store the low byte of the result in the X-th byte of
                            \ the heap at XX3
    
     INX                    \ Increment the heap pointer in X to point to the next
                            \ byte
    
     LDA #0                 \ And then subtract the high bytes
     SBC U
    
     STA XX3,X              \ Store the high byte of the result in the X-th byte of
                            \ the heap at XX3
    
    .LL50
    
                            \ By the time we get here, the XX3 heap contains four
                            \ bytes containing the screen coordinates of the current
                            \ vertex, in the order: x_lo, x_hi, y_lo, y_hi
    
     CLC                    \ Set CNT = CNT + 4, so the heap pointer points to the
     LDA CNT                \ next free byte on the heap
     ADC #4
     STA CNT
    
     LDA XX17               \ Set A to the offset of the current vertex's data,
                            \ which we set in part 6
    
     ADC #6                 \ Set Y = A + 6, so Y now points to the data for the
     TAY                    \ next vertex
    
     BCS LL72               \ If the addition just overflowed, meaning we just tried
                            \ to access vertex #43, jump to LL72, as the maximum
                            \ number of vertices allowed is 42
    
     CMP XX20               \ If Y >= number of vertices * 6 (which we stored in
     BCS LL72               \ XX20 in part 6), jump to LL72, as we have processed
                            \ all the vertices for this ship
    
     JMP LL48               \ Loop back to LL48 in part 6 to calculate visibility
                            \ and screen coordinates for the next vertex
    
    
    
    
           Name: LL9 (Part 9 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Draw laser beams if the ship is firing its laser at us
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_9_of_12.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll9_part_9_of_12.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part sets things up so we can loop through the edges in the next part. It
     also adds a line to the ship line heap, if the ship is firing at us.
    
     When we get here, the heap at XX3 contains all the visible vertex screen
     coordinates.
    
    
    
    
    .LL72
    
     LDA XX1+31             \ If bit 5 of the ship's byte #31 is clear, then the
     AND #%00100000         \ ship is not currently exploding, so jump down to EE31
     BEQ EE31
    
     LDA XX1+31             \ The ship is exploding, so set bit 3 of the ship's byte
     ORA #%00001000         \ #31 to denote that we are drawing something on-screen
     STA XX1+31             \ for this ship
    
     JMP DOEXP              \ Jump to DOEXP to display the explosion cloud,
                            \ returning from the subroutine using a tail call
    
    .EE31
    
     LDY #9                 \ Fetch byte #9 of the ship's blueprint, which is the
     LDA (XX0),Y            \ number of edges, and store it in XX20
     STA XX20
    
     LDA #%00001000         \ Set bit 3 of A so the next instruction sets bit 3 of
                            \ the ship's byte #31 to denote that we are drawing
                            \ something on-screen for this ship
    
     ORA XX1+31             \ Apply bit 3 of A to the ship's byte #31, so if there
     STA XX1+31             \ was no ship already on screen, the bit is clear,
                            \ otherwise it is set
    
     LDY #0                 \ Set XX17 = 0, which we are going to use as a counter
    \STY LSNUM              \ for stepping through the ship's edges
     STY XX17               \
                            \ The STY is commented out in the original source
    
     BIT XX1+31             \ If bit 6 of the ship's byte #31 is clear, then the
     BVC LL170              \ ship is not firing its lasers, so jump to LL170 to
                            \ skip the drawing of laser lines
    
                            \ The ship is firing its laser at us, so we need to draw
                            \ the laser lines
    
     LDA XX1+31             \ Clear bit 6 of the ship's byte #31 so the ship doesn't
     AND #%10111111         \ keep firing endlessly
     STA XX1+31
    
     LDY #6                 \ Fetch byte #6 of the ship's blueprint, which is the
     LDA (XX0),Y            \ number * 4 of the vertex where the ship has its lasers
    
     TAY                    \ Put the vertex number into Y, where it can act as an
                            \ index into list of vertex screen coordinates we added
                            \ to the XX3 heap
    
     LDX XX3,Y              \ Fetch the x_lo coordinate of the laser vertex from the
     STX XX15               \ XX3 heap into XX15
    
     INX                    \ If X = 255 then the laser vertex is not visible, as
     BEQ LL170              \ the value we stored in part 2 wasn't overwritten by
                            \ the vertex calculation in part 6 and 7, so jump to
                            \ LL170 to skip drawing the laser lines
    
                            \ We now build a laser beam from the ship's laser vertex
                            \ towards our ship, as follows:
                            \
                            \   XX15(1 0) = laser vertex x-coordinate
                            \
                            \   XX15(3 2) = laser vertex y-coordinate
                            \
                            \   XX15(5 4) = x-coordinate of the end of the beam
                            \
                            \   XX12(1 0) = y-coordinate of the end of the beam
                            \
                            \ The end of the laser beam will be positioned to look
                            \ good, rather than being directly aimed at us, as
                            \ otherwise we would only see a flashing point of light
                            \ as they unleashed their attack
    
     LDX XX3+1,Y            \ Fetch the x_hi coordinate of the laser vertex from the
     STX XX15+1             \ XX3 heap into XX15+1
    
     INX                    \ If X = 255 then the laser vertex is not visible, as
     BEQ LL170              \ the value we stored in part 2 wasn't overwritten by
                            \ a vertex calculation in part 6 and 7, so jump to LL170
                            \ to skip drawing the laser beam
    
     LDX XX3+2,Y            \ Fetch the y_lo coordinate of the laser vertex from the
     STX XX15+2             \ XX3 heap into XX15+2
    
     LDX XX3+3,Y            \ Fetch the y_hi coordinate of the laser vertex from the
     STX XX15+3             \ XX3 heap into XX15+3
    
     LDA #0                 \ Set XX15(5 4) = 0, so their laser beam fires to the
     STA XX15+4             \ left edge of the screen
     STA XX15+5
    
     STA XX12+1             \ Set XX12(1 0) = the ship's z_lo coordinate, which will
     LDA XX1+6              \ effectively make the vertical position of the end of
     STA XX12               \ the laser beam move around as the ship moves in space
    
     LDA XX1+2              \ If the ship's x_sign is positive, skip the next
     BPL P%+4               \ instruction
    
     DEC XX15+4             \ The ship's x_sign is negative (i.e. it's on the left
                            \ side of the screen), so switch the laser beam so it
                            \ goes to the right edge of the screen by decrementing
                            \ XX15(5 4) to 255
    
     JSR CLIP               \ Call CLIP to see if the laser beam needs to be
                            \ clipped to fit on-screen, returning the clipped line's
                            \ end-points in (X1, Y1) and (X2, Y2)
    
     BCS LL170              \ If the C flag is set then the line is not visible on
                            \ screen, so jump to LL170 so we don't store this line
                            \ in the ship line heap
    
     JSR LSPUT              \ Draw the laser line using flicker-free animation, by
                            \ first drawing the new laser line and then erasing the
                            \ corresponding old line from the screen
    
    
    
    
           Name: LL9 (Part 10 of 12)                                     [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Calculate the visibility of each of the ship's edges
                 and draw the visible ones using flicker-free animation
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_10_of_12.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll9_part_10_of_12.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part calculates which edges are visible - in other words, which lines we
     should draw - and clips them to fit on the screen.
    
     Visible edges are drawn using flicker-free animation, which erases the
     corresponding edge from the on-screen ship at the same time.
    
     When we get here, the heap at XX3 contains all the visible vertex screen
     coordinates.
    
    
    
    
    .LL170
    
     LDY #3                 \ Fetch byte #3 of the ship's blueprint, which contains
     CLC                    \ the low byte of the offset to the edges data
     LDA (XX0),Y
    
     ADC XX0                \ Set V = low byte edges offset + XX0
     STA V
    
     LDY #16                \ Fetch byte #16 of the ship's blueprint, which contains
     LDA (XX0),Y            \ the high byte of the offset to the edges data
    
     ADC XX0+1              \ Set V+1 = high byte edges offset + XX0+1
     STA V+1                \
                            \ So V(1 0) now points to the start of the edges data
                            \ for this ship
    
     LDY #5                 \ Fetch byte #5 of the ship's blueprint, which contains
     LDA (XX0),Y            \ the maximum heap size for plotting the ship (which is
     STA CNT                \ 1 + 4 * the maximum number of visible edges) and store
                            \ it in CNT
    
    .LL75
    
     LDY #0                 \ Set Y = 0 so we start with byte #0
    
     LDA (V),Y              \ Fetch byte #0 for this edge, which contains the
                            \ visibility distance for this edge, beyond which the
                            \ edge is not shown
    
     CMP XX4                \ If XX4 > the visibility distance, where XX4 contains
     BCC LL78               \ the ship's z-distance reduced to 0-31 (which we set in
                            \ part 2), then this edge is too far away to be visible,
                            \ so jump down to LL78 to move on to the next edge
    
     INY                    \ Increment Y to point to byte #1
    
     LDA (V),Y              \ Fetch byte #1 for this edge into A, so:
                            \
                            \   A = %ffff ffff, where:
                            \
                            \     * Bits 0-3 = the number of face 1
                            \
                            \     * Bits 4-7 = the number of face 2
    
     STA P                  \ Store byte #1 into P
    
     AND #%00001111         \ Extract the number of face 1 into X
     TAX
    
     LDA XX2,X              \ If XX2+X is non-zero then we decided in part 5 that
     BNE LL79               \ face 1 is visible, so jump to LL79
    
     LDA P                  \ Fetch byte #1 for this edge into A
    
     LSR A                  \ Shift right four times to extract the number of face 2
     LSR A                  \ from bits 4-7 into X
     LSR A
     LSR A
     TAX
    
     LDA XX2,X              \ If XX2+X is zero then we decided in part 5 that
     BEQ LL78               \ face 2 is hidden, so jump to LL78
    
    .LL79
    
                            \ We now build the screen line for this edge, as
                            \ follows:
                            \
                            \   XX15(1 0) = start x-coordinate
                            \
                            \   XX15(3 2) = start y-coordinate
                            \
                            \   XX15(5 4) = end x-coordinate
                            \
                            \   XX12(1 0) = end y-coordinate
                            \
                            \ We can then pass this to the line clipping routine
                            \ before storing the resulting line in the ship line
                            \ heap
    
     INY                    \ Increment Y to point to byte #2
    
     LDA (V),Y              \ Fetch byte #2 for this edge into X, which contains
     TAX                    \ the number of the vertex at the start of the edge
                            \
                            \ Byte #2 contains the vertex number multiplied by 4,
                            \ so we can use it as an index into the heap at XX3 to
                            \ fetch the vertex's screen coordinates, which are
                            \ stored as four bytes containing two 16-bit numbers
    
     LDA XX3,X              \ Fetch the x_lo coordinate of the edge's start vertex
     STA XX15               \ from the XX3 heap into XX15
    
     LDA XX3+1,X            \ Fetch the x_hi coordinate of the edge's start vertex
     STA XX15+1             \ from the XX3 heap into XX15+1
    
     LDA XX3+2,X            \ Fetch the y_lo coordinate of the edge's start vertex
     STA XX15+2             \ from the XX3 heap into XX15+2
    
     LDA XX3+3,X            \ Fetch the y_hi coordinate of the edge's start vertex
     STA XX15+3             \ from the XX3 heap into XX15+3
    
     INY                    \ Increment Y to point to byte #3
    
     LDA (V),Y              \ Fetch byte #3 for this edge into X, which contains
     TAX                    \ the number of the vertex at the end of the edge
    
     LDA XX3,X              \ Fetch the x_lo coordinate of the edge's end vertex
     STA XX15+4             \ from the XX3 heap into XX15+4
    
     LDA XX3+2,X            \ Fetch the y_lo coordinate of the edge's end vertex
     STA XX12               \ from the XX3 heap into XX12
    
     LDA XX3+3,X            \ Fetch the y_hi coordinate of the edge's end vertex
     STA XX12+1             \ from the XX3 heap into XX12+1
    
     LDA XX3+1,X            \ Fetch the x_hi coordinate of the edge's end vertex
     STA XX15+5             \ from the XX3 heap into XX15+5
    
     JSR CLIP2              \ Call CLIP2 to see if the new line segment needs to be
                            \ clipped to fit on-screen, returning the clipped line's
                            \ end-points in (X1, Y1) and (X2, Y2)
    
     BCS LL78               \ If the C flag is set then the line is not visible on
                            \ screen, so jump to LL78 so we don't store this line
                            \ in the ship line heap
    
     JSR LSPUT              \ Draw this edge using flicker-free animation, by first
                            \ drawing the ship's new line and then erasing the
                            \ corresponding old line from the screen
    
    
    
    
           Name: LL9 (Part 11 of 12)                                     [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Loop back for the next edge
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_11_of_12.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll9_part_11_of_12.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       LL81+2               Draw the contents of the ship line heap, used to draw
                            the ship as a dot from SHPPT
    
    
    
    
    .LL78
    
     LDA LSNUM              \ If LSNUM >= CNT, skip to LL81 so we don't loop back
     CMP CNT                \ for the next edge (CNT was set to the maximum heap
     BCS LL81               \ size for this ship in part 10, so this checks whether
                            \ we have just run out of space in the ship line heap,
                            \ and stops drawing edges if we have)
    
     LDA V                  \ Increment V by 4 so V(1 0) points to the data for the
     CLC                    \ next edge
     ADC #4
     STA V
    
     BCC P%+4               \ If the above addition didn't overflow, skip the
                            \ following instruction
    
     INC V+1                \ Otherwise increment the high byte of V(1 0), as we
                            \ just moved the V(1 0) pointer past a page boundary
    
     INC XX17               \ Increment the edge counter to point to the next edge
    
     LDY XX17               \ If Y < XX20, which contains the number of edges in
     CPY XX20               \ the blueprint, loop back to LL75 to process the next
     BCC LL75               \ edge
    
    .LL81
    
     JMP LSCLR              \ Jump down to part 12 below to draw any remaining lines
                            \ from the old ship that are still in the ship line heap
    
    
    
    
           Name: LL118                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Move a point along a line until it is on-screen
      Deep dive: [Line-clipping](https://elite.bbcelite.com/deep_dives/line-clipping.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll118.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll118.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md) calls LL118
    
    
    
    
    
    * * *
    
    
     Given a point (x1, y1), a gradient and a direction of slope, move the point
     along the line until it is on-screen, so this effectively clips the (x1, y1)
     end of a line to be on the screen.
    
    
    
    * * *
    
    
     Arguments:
    
       XX15(1 0)            x1 as a 16-bit coordinate (x1_hi x1_lo)
    
       XX15(3 2)            y1 as a 16-bit coordinate (y1_hi y1_lo)
    
       XX12+2               The line's gradient * 256 (so 1.0 = 256)
    
       XX12+3               The direction of slope:
    
                              * Positive (bit 7 clear) = top left to bottom right
    
                              * Negative (bit 7 set) = top right to bottom left
    
       T                    The gradient of slope:
    
                              * 0 if it's a shallow slope
    
                              * &FF if it's a steep slope
    
    
    
    * * *
    
    
     Returns:
    
       XX15                 x1 as an 8-bit coordinate
    
       XX15+2               y1 as an 8-bit coordinate
    
    
    
    * * *
    
    
     Other entry points:
    
       LL118-1              Contains an RTS
    
    
    
    
    .LL118
    
     LDA XX15+1             \ If x1_hi is positive, jump down to LL119 to skip the
     BPL LL119              \ following
    
     STA S                  \ Otherwise x1_hi is negative, i.e. off the left of the
                            \ screen, so set S = x1_hi
    
     JSR LL120              \ Call LL120 to calculate:
                            \
                            \   (Y X) = (S x1_lo) * XX12+2      if T = 0
                            \         = x1 * gradient
                            \
                            \   (Y X) = (S x1_lo) / XX12+2      if T <> 0
                            \         = x1 / gradient
                            \
                            \ with the sign of (Y X) set to the opposite of the
                            \ line's direction of slope
    
     TXA                    \ Set y1 = y1 + (Y X)
     CLC                    \
     ADC XX15+2             \ starting with the low bytes
     STA XX15+2
    
     TYA                    \ And then adding the high bytes
     ADC XX15+3
     STA XX15+3
    
     LDA #0                 \ Set x1 = 0
     STA XX15
     STA XX15+1
    
     TAX                    \ Set X = 0 so the next instruction becomes a JMP
    
    .LL119
    
     BEQ LL134              \ If x1_hi = 0 then jump down to LL134 to skip the
                            \ following, as the x-coordinate is already on-screen
                            \ (as 0 <= (x_hi x_lo) <= 255)
    
     STA S                  \ Otherwise x1_hi is positive, i.e. x1 >= 256 and off
     DEC S                  \ the right side of the screen, so set S = x1_hi - 1
    
     JSR LL120              \ Call LL120 to calculate:
                            \
                            \   (Y X) = (S x1_lo) * XX12+2      if T = 0
                            \         = (x1 - 256) * gradient
                            \
                            \   (Y X) = (S x1_lo) / XX12+2      if T <> 0
                            \         = (x1 - 256) / gradient
                            \
                            \ with the sign of (Y X) set to the opposite of the
                            \ line's direction of slope
    
     TXA                    \ Set y1 = y1 + (Y X)
     CLC                    \
     ADC XX15+2             \ starting with the low bytes
     STA XX15+2
    
     TYA                    \ And then adding the high bytes
     ADC XX15+3
     STA XX15+3
    
     LDX #255               \ Set x1 = 255
     STX XX15
     INX
     STX XX15+1
    
    .LL134
    
                            \ We have moved the point so the x-coordinate is on
                            \ screen (i.e. in the range 0-255), so now for the
                            \ y-coordinate
    
     LDA XX15+3             \ If y1_hi is positive, jump down to LL119 to skip
     BPL LL135              \ the following
    
     STA S                  \ Otherwise y1_hi is negative, i.e. off the top of the
                            \ screen, so set S = y1_hi
    
     LDA XX15+2             \ Set R = y1_lo
     STA R
    
     JSR LL123              \ Call LL123 to calculate:
                            \
                            \   (Y X) = (S R) / XX12+2      if T = 0
                            \         = y1 / gradient
                            \
                            \   (Y X) = (S R) * XX12+2      if T <> 0
                            \         = y1 * gradient
                            \
                            \ with the sign of (Y X) set to the opposite of the
                            \ line's direction of slope
    
     TXA                    \ Set x1 = x1 + (Y X)
     CLC                    \
     ADC XX15               \ starting with the low bytes
     STA XX15
    
     TYA                    \ And then adding the high bytes
     ADC XX15+1
     STA XX15+1
    
     LDA #0                 \ Set y1 = 0
     STA XX15+2
     STA XX15+3
    
    .LL135
    
    \BNE LL139              \ This instruction is commented out in the original
                            \ source
    
     LDA XX15+2             \ Set (S R) = (y1_hi y1_lo) - screen height
     SEC                    \
     SBC #Y*2               \ starting with the low bytes
     STA R
    
     LDA XX15+3             \ And then subtracting the high bytes
     SBC #0
     STA S
    
     BCC LL136              \ If the subtraction underflowed, i.e. if y1 < screen
                            \ height, then y1 is already on-screen, so jump to LL136
                            \ to return from the subroutine, as we are done
    
    .LL139
    
                            \ If we get here then y1 >= screen height, i.e. off the
                            \ bottom of the screen
    
     JSR LL123              \ Call LL123 to calculate:
                            \
                            \   (Y X) = (S R) / XX12+2      if T = 0
                            \         = (y1 - screen height) / gradient
                            \
                            \   (Y X) = (S R) * XX12+2      if T <> 0
                            \         = (y1 - screen height) * gradient
                            \
                            \ with the sign of (Y X) set to the opposite of the
                            \ line's direction of slope
    
     TXA                    \ Set x1 = x1 + (Y X)
     CLC                    \
     ADC XX15               \ starting with the low bytes
     STA XX15
    
     TYA                    \ And then adding the high bytes
     ADC XX15+1
     STA XX15+1
    
     LDA #Y*2-1             \ Set y1 = 2 * #Y - 1. The constant #Y is 96, the
     STA XX15+2             \ y-coordinate of the mid-point of the space view, so
     LDA #0                 \ this sets Y2 to 191, the y-coordinate of the bottom
     STA XX15+3             \ pixel row of the space view
    
    .LL136
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL120                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (Y X) = (S x1_lo) * XX12+2 or (S x1_lo) / XX12+2
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll120.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll120.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL118](../main/subroutine/ll118.md) calls LL120
                 * [LL123](../main/subroutine/ll123.md) calls via LL122
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       * If T = 0, this is a shallow slope, so calculate (Y X) = (S x1_lo) * XX12+2
    
       * If T <> 0, this is a steep slope, so calculate (Y X) = (S x1_lo) / XX12+2
    
     giving (Y X) the opposite sign to the slope direction in XX12+3.
    
    
    
    * * *
    
    
     Arguments:
    
       T                    The gradient of slope:
    
                              * 0 if it's a shallow slope
    
                              * &FF if it's a steep slope
    
    
    
    * * *
    
    
     Other entry points:
    
       LL122                Calculate (Y X) = (S R) * Q and set the sign to the
                            opposite of the top byte on the stack
    
    
    
    
    .LL120
    
     LDA XX15               \ Set R = x1_lo
     STA R
    
    \.LL120                 \ This label is commented out in the original source
    
     JSR LL129              \ Call LL129 to do the following:
                            \
                            \   Q = XX12+2
                            \     = line gradient
                            \
                            \   A = S EOR XX12+3
                            \     = S EOR slope direction
                            \
                            \   (S R) = |S R|
                            \
                            \ So A contains the sign of S * slope direction
    
     PHA                    \ Store A on the stack so we can use it later
    
     LDX T                  \ If T is non-zero, then it's a steep slope, so jump
     BNE LL121              \ down to LL121 to calculate this instead:
                            \
                            \   (Y X) = (S R) / Q
    
    .LL122
    
                            \ The following calculates:
                            \
                            \   (Y X) = (S R) * Q
                            \
                            \ using the same shift-and-add algorithm that's
                            \ documented in MULT1
    
     LDA #0                 \ Set A = 0
    
     TAX                    \ Set (Y X) = 0 so we can start building the answer here
     TAY
    
     LSR S                  \ Shift (S R) to the right, so we extract bit 0 of (S R)
     ROR R                  \ into the C flag
    
     ASL Q                  \ Shift Q to the left, catching bit 7 in the C flag
    
     BCC LL126              \ If C (i.e. the next bit from Q) is clear, do not do
                            \ the addition for this bit of Q, and instead skip to
                            \ LL126 to just do the shifts
    
    .LL125
    
     TXA                    \ Set (Y X) = (Y X) + (S R)
     CLC                    \
     ADC R                  \ starting with the low bytes
     TAX
    
     TYA                    \ And then doing the high bytes
     ADC S
     TAY
    
    .LL126
    
     LSR S                  \ Shift (S R) to the right
     ROR R
    
     ASL Q                  \ Shift Q to the left, catching bit 7 in the C flag
    
     BCS LL125              \ If C (i.e. the next bit from Q) is set, loop back to
                            \ LL125 to do the addition for this bit of Q
    
     BNE LL126              \ If Q has not yet run out of set bits, loop back to
                            \ LL126 to do the "shift" part of shift-and-add until
                            \ we have done additions for all the set bits in Q, to
                            \ give us our multiplication result
    
     PLA                    \ Restore A, which we calculated above, from the stack
    
     BPL LL133              \ If A is positive jump to LL133 to negate (Y X) and
                            \ return from the subroutine using a tail call
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL123                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (Y X) = (S R) / XX12+2 or (S R) * XX12+2
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll123.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll123.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL118](../main/subroutine/ll118.md) calls LL123
                 * [LL120](../main/subroutine/ll120.md) calls via LL121
                 * [LL120](../main/subroutine/ll120.md) calls via LL133
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       * If T = 0, this is a shallow slope, so calculate (Y X) = (S R) / XX12+2
    
       * If T <> 0, this is a steep slope, so calculate (Y X) = (S R) * XX12+2
    
     giving (Y X) the opposite sign to the slope direction in XX12+3.
    
    
    
    * * *
    
    
     Arguments:
    
       XX12+2               The line's gradient * 256 (so 1.0 = 256)
    
       XX12+3               The direction of slope:
    
                              * Bit 7 clear means top left to bottom right
    
                              * Bit 7 set means top right to bottom left
    
       T                    The gradient of slope:
    
                              * 0 if it's a shallow slope
    
                              * &FF if it's a steep slope
    
    
    
    * * *
    
    
     Other entry points:
    
       LL121                Calculate (Y X) = (S R) / Q and set the sign to the
                            opposite of the top byte on the stack
    
       LL133                Negate (Y X) and return from the subroutine
    
       LL128                Contains an RTS
    
    
    
    
    .LL123
    
     JSR LL129              \ Call LL129 to do the following:
                            \
                            \   Q = XX12+2
                            \     = line gradient
                            \
                            \   A = S EOR XX12+3
                            \     = S EOR slope direction
                            \
                            \   (S R) = |S R|
                            \
                            \ So A contains the sign of S * slope direction
    
     PHA                    \ Store A on the stack so we can use it later
    
     LDX T                  \ If T is non-zero, then it's a steep slope, so jump up
     BNE LL122              \ to LL122 to calculate this instead:
                            \
                            \   (Y X) = (S R) * Q
    
    .LL121
    
                            \ The following calculates:
                            \
                            \   (Y X) = (S R) / Q
                            \
                            \ using the same shift-and-subtract algorithm that's
                            \ documented in TIS2
    
     LDA #%11111111         \ Set Y = %11111111
     TAY
    
     ASL A                  \ Set X = %11111110
     TAX
    
                            \ This sets (Y X) = %1111111111111110, so we can rotate
                            \ through 15 loop iterations, getting a 1 each time, and
                            \ then getting a 0 on the 16th iteration... and we can
                            \ also use it to catch our result bits into bit 0 each
                            \ time
    
    .LL130
    
     ASL R                  \ Shift (S R) to the left
     ROL S
    
     LDA S                  \ Set A = S
    
     BCS LL131              \ If bit 7 of S was set, then jump straight to the
                            \ subtraction
    
     CMP Q                  \ If A < Q (i.e. S < Q), skip the following subtractions
     BCC LL132
    
    .LL131
    
     SBC Q                  \ A >= Q (i.e. S >= Q) so set:
     STA S                  \
                            \   S = (A R) - Q
                            \     = (S R) - Q
                            \
                            \ starting with the low bytes (we know the C flag is
                            \ set so the subtraction will be correct)
    
     LDA R                  \ And then doing the high bytes
     SBC #0
     STA R
    
     SEC                    \ Set the C flag to rotate into the result in (Y X)
    
    .LL132
    
     TXA                    \ Rotate the counter in (Y X) to the left, and catch the
     ROL A                  \ result bit into bit 0 (which will be a 0 if we didn't
     TAX                    \ do the subtraction, or 1 if we did)
     TYA
     ROL A
     TAY
    
     BCS LL130              \ If we still have set bits in (Y X), loop back to LL130
                            \ to do the next iteration of 15, until we have done the
                            \ whole division
    
     PLA                    \ Restore A, which we calculated above, from the stack
    
     BMI LL128              \ If A is negative jump to LL128 to return from the
                            \ subroutine with (Y X) as is
    
    .LL133
    
     TXA                    \ Otherwise negate (Y X) using two's complement by first
     EOR #%11111111         \ setting the low byte to ~X + 1
    \CLC                    \
     ADC #1                 \ The CLC instruction is commented out in the original
     TAX                    \ source. It would have no effect as we know the C flag
                            \ is clear from when we passed through the BCS above
    
     TYA                    \ Then set the high byte to ~Y + C
     EOR #%11111111
     ADC #0
     TAY
    
    .LL128
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL129                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate Q = XX12+2, A = S EOR XX12+3 and (S R) = |S R|
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll129.md)
     References: This subroutine is called as follows:
                 * [LL120](../main/subroutine/ll120.md) calls LL129
                 * [LL123](../main/subroutine/ll123.md) calls LL129
    
    
    
    
    
    * * *
    
    
     Do the following, in this order:
    
       Q = XX12+2
    
       A = S EOR XX12+3
    
       (S R) = |S R|
    
     This sets up the variables required above to calculate (S R) / XX12+2 and give
     the result the opposite sign to XX12+3.
    
    
    
    
    .LL129
    
     LDX XX12+2             \ Set Q = XX12+2
     STX Q
    
     LDA S                  \ If S is positive, jump to LL127
     BPL LL127
    
     LDA #0                 \ Otherwise set R = -R
     SEC
     SBC R
     STA R
    
     LDA S                  \ Push S onto the stack
     PHA
    
     EOR #%11111111         \ Set S = ~S + 1 + C
     ADC #0
     STA S
    
     PLA                    \ Pull the original, negative S from the stack into A
    
    .LL127
    
     EOR XX12+3             \ Set A = original argument S EOR'd with XX12+3
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL145 (Part 1 of 4)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Clip line: Work out which end-points are on-screen, if any
      Deep dive: [Line-clipping](https://elite.bbcelite.com/deep_dives/line-clipping.html)
                 [Extended screen coordinates](https://elite.bbcelite.com/deep_dives/extended_screen_coordinates.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll145_part_1_of_4.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll145_part_1_of_4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BLINE](../main/subroutine/bline.md) calls LL145
                 * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md) calls via CLIP
                 * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md) calls via CLIP2
    
    
    
    
    
    * * *
    
    
     This routine clips the line from (x1, y1) to (x2, y2) so it fits on-screen, or
     returns an error if it can't be clipped to fit. The arguments are 16-bit
     coordinates, and the clipped line is returned using 8-bit screen coordinates.
    
     This part sets XX13 to reflect which of the two points are on-screen and
     off-screen.
    
    
    
    * * *
    
    
     Arguments:
    
       XX15(1 0)            x1 as a 16-bit coordinate (x1_hi x1_lo)
    
       XX15(3 2)            y1 as a 16-bit coordinate (y1_hi y1_lo)
    
       XX15(5 4)            x2 as a 16-bit coordinate (x2_hi x2_lo)
    
       XX12(1 0)            y2 as a 16-bit coordinate (y2_hi y2_lo)
    
    
    
    * * *
    
    
     Returns:
    
       (X1, Y1)             Screen coordinate of the start of the clipped line
    
       (X2, Y2)             Screen coordinate of the end of the clipped line
    
       C flag               Clear if the clipped line fits on-screen, set if it
                            doesn't
    
       XX13                 The state of the original coordinates on-screen:
    
                              * 0   = (x2, y2) on-screen
    
                              * 95  = (x1, y1) on-screen,  (x2, y2) off-screen
    
                              * 191 = (x1, y1) off-screen, (x2, y2) off-screen
    
                            So XX13 is non-zero if the end of the line was clipped,
                            meaning the next line sent to BLINE can't join onto the
                            end but has to start a new segment
    
       SWAP                 The swap status of the returned coordinates:
    
                              * &FF if we swapped the values of (x1, y1) and
                                (x2, y2) as part of the clipping process
    
                              * 0 if the coordinates are still in the same order
    
       Y                    Y is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       CLIP                 Another name for LL145
    
       CLIP2                Don't initialise the values in SWAP or A
    
    
    
    
    .LL145
    
    .CLIP
    
     LDA #0                 \ Set SWAP = 0
     STA SWAP
    
     LDA XX15+5             \ Set A = x2_hi
    
    .CLIP2
    
     LDX #Y*2-1             \ Set X = #Y * 2 - 1. The constant #Y is 96, the
                            \ y-coordinate of the mid-point of the space view, so
                            \ this sets Y2 to 191, the y-coordinate of the bottom
                            \ pixel row of the space view
    
     ORA XX12+1             \ If one or both of x2_hi and y2_hi are non-zero, jump
     BNE LL107              \ to LL107 to skip the following, leaving X at 191
    
     CPX XX12               \ If y2_lo > the y-coordinate of the bottom of screen
     BCC LL107              \ then (x2, y2) is off the bottom of the screen, so skip
                            \ the following instruction, leaving X at 191
    
     LDX #0                 \ Set X = 0
    
    .LL107
    
     STX XX13               \ Set XX13 = X, so we have:
                            \
                            \   * XX13 = 0 if x2_hi = y2_hi = 0, y2_lo is on-screen
                            \
                            \   * XX13 = 191 if x2_hi or y2_hi are non-zero or y2_lo
                            \            is off the bottom of the screen
                            \
                            \ In other words, XX13 is 191 if (x2, y2) is off-screen,
                            \ otherwise it is 0
    
     LDA XX15+1             \ If one or both of x1_hi and y1_hi are non-zero, jump
     ORA XX15+3             \ to LL83
     BNE LL83
    
     LDA #Y*2-1             \ If y1_lo > the y-coordinate of the bottom of screen
     CMP XX15+2             \ then (x1, y1) is off the bottom of the screen, so jump
     BCC LL83               \ to LL83
    
                            \ If we get here, (x1, y1) is on-screen
    
     LDA XX13               \ If XX13 is non-zero, i.e. (x2, y2) is off-screen, jump
     BNE LL108              \ to LL108 to halve it before continuing at LL83
    
                            \ If we get here, the high bytes are all zero, which
                            \ means the x-coordinates are < 256 and therefore fit on
                            \ screen, and neither coordinate is off the bottom of
                            \ the screen. That means both coordinates are already on
                            \ screen, so we don't need to do any clipping, all we
                            \ need to do is move the low bytes into (X1, Y1) and
                            \ X2, Y2) and return
    
    .LL146
    
                            \ If we get here then we have clipped our line to the
                            \ screen edge (if we had to clip it at all), so we move
                            \ the low bytes from (x1, y1) and (x2, y2) into (X1, Y1)
                            \ and (X2, Y2), remembering that they share locations
                            \ with XX15:
                            \
                            \   X1 = XX15
                            \   Y1 = XX15+1
                            \   X2 = XX15+2
                            \   Y2 = XX15+3
                            \
                            \ X1 already contains x1_lo, so now we do the rest
    
     LDA XX15+2             \ Set Y1 (aka XX15+1) = y1_lo
     STA XX15+1
    
     LDA XX15+4             \ Set X2 (aka XX15+2) = x2_lo
     STA XX15+2
    
     LDA XX12               \ Set Y2 (aka XX15+3) = y2_lo
     STA XX15+3
    
     CLC                    \ Clear the C flag as the clipped line fits on-screen
    
     RTS                    \ Return from the subroutine
    
    .LL109
    
     SEC                    \ Set the C flag to indicate the clipped line does not
                            \ fit on-screen
    
     RTS                    \ Return from the subroutine
    
    .LL108
    
     LSR XX13               \ If we get here then (x2, y2) is off-screen and XX13 is
                            \ 191, so shift XX13 right to halve it to 95
    
    
    
    
           Name: LL145 (Part 2 of 4)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Clip line: Work out if any part of the line is on-screen
      Deep dive: [Line-clipping](https://elite.bbcelite.com/deep_dives/line-clipping.html)
                 [Extended screen coordinates](https://elite.bbcelite.com/deep_dives/extended_screen_coordinates.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll145_part_2_of_4.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part does a number of tests to see if the line is on or off the screen.
    
     If we get here then at least one of (x1, y1) and (x2, y2) is off-screen, with
     XX13 set as follows:
    
       * 0   = (x1, y1) off-screen, (x2, y2) on-screen
    
       * 95  = (x1, y1) on-screen,  (x2, y2) off-screen
    
       * 191 = (x1, y1) off-screen, (x2, y2) off-screen
    
     where "off-screen" is defined as having a non-zero high byte in one of the
     coordinates, or in the case of y-coordinates, having a low byte > 191, the
     y-coordinate of the bottom of the space view.
    
    
    
    
    .LL83
    
     LDA XX13               \ If XX13 < 128 then only one of the points is on-screen
     BPL LL115              \ so jump down to LL115 to skip the checks of whether
                            \ both points are in the strips to the right or bottom
                            \ of the screen
    
                            \ If we get here, both points are off-screen
    
     LDA XX15+1             \ If both x1_hi and x2_hi have bit 7 set, jump to LL109
     AND XX15+5             \ to return from the subroutine with the C flag set, as
     BMI LL109              \ the entire line is above the top of the screen
    
     LDA XX15+3             \ If both y1_hi and y2_hi have bit 7 set, jump to LL109
     AND XX12+1             \ to return from the subroutine with the C flag set, as
     BMI LL109              \ the entire line is to the left of the screen
    
     LDX XX15+1             \ Set A = X = x1_hi - 1
     DEX
     TXA
    
     LDX XX15+5             \ Set XX12+2 = x2_hi - 1
     DEX
     STX XX12+2
    
     ORA XX12+2             \ If neither (x1_hi - 1) or (x2_hi - 1) have bit 7 set,
     BPL LL109              \ jump to LL109 to return from the subroutine with the C
                            \ flag set, as the line doesn't fit on-screen
    
     LDA XX15+2             \ If y1_lo < y-coordinate of screen bottom, clear the C
     CMP #Y*2               \ flag, otherwise set it
    
     LDA XX15+3             \ Set XX12+2 = y1_hi - (1 - C), so:
     SBC #0                 \
     STA XX12+2             \  * Set XX12+2 = y1_hi - 1 if y1_lo is on-screen
                            \  * Set XX12+2 = y1_hi     otherwise
                            \
                            \ We do this subtraction because we are only interested
                            \ in trying to move the points up by a screen if that
                            \ might move the point into the space view portion of
                            \ the screen, i.e. if y1_lo is on-screen
    
     LDA XX12               \ If y2_lo < y-coordinate of screen bottom, clear the C
     CMP #Y*2               \ flag, otherwise set it
    
     LDA XX12+1             \ Set XX12+2 = y2_hi - (1 - C), so:
     SBC #0                 \
                            \  * Set XX12+1 = y2_hi - 1 if y2_lo is on-screen
                            \  * Set XX12+1 = y2_hi     otherwise
                            \
                            \ We do this subtraction because we are only interested
                            \ in trying to move the points up by a screen if that
                            \ might move the point into the space view portion of
                            \ the screen, i.e. if y1_lo is on-screen
    
     ORA XX12+2             \ If neither XX12+1 or XX12+2 have bit 7 set, jump to
     BPL LL109              \ LL109 to return from the subroutine with the C flag
                            \ set, as the line doesn't fit on-screen
    
    
    
    
           Name: LL145 (Part 3 of 4)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Clip line: Calculate the line's gradient
      Deep dive: [Line-clipping](https://elite.bbcelite.com/deep_dives/line-clipping.html)
                 [Extended screen coordinates](https://elite.bbcelite.com/deep_dives/extended_screen_coordinates.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll145_part_3_of_4.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .LL115
    
     TYA                    \ Store Y on the stack so we can preserve it through the
     PHA                    \ call to this subroutine
    
     LDA XX15+4             \ Set XX12+2 = x2_lo - x1_lo
     SEC
     SBC XX15
     STA XX12+2
    
     LDA XX15+5             \ Set XX12+3 = x2_hi - x1_hi
     SBC XX15+1
     STA XX12+3
    
     LDA XX12               \ Set XX12+4 = y2_lo - y1_lo
     SEC
     SBC XX15+2
     STA XX12+4
    
     LDA XX12+1             \ Set XX12+5 = y2_hi - y1_hi
     SBC XX15+3
     STA XX12+5
    
                            \ So we now have:
                            \
                            \   delta_x in XX12(3 2)
                            \   delta_y in XX12(5 4)
                            \
                            \ where the delta is (x1, y1) - (x2, y2))
    
     EOR XX12+3             \ Set S = the sign of delta_x * the sign of delta_y, so
     STA S                  \ if bit 7 of S is set, the deltas have different signs
    
     LDA XX12+5             \ If delta_y_hi is positive, jump down to LL110 to skip
     BPL LL110              \ the following
    
     LDA #0                 \ Otherwise flip the sign of delta_y to make it
     SEC                    \ positive, starting with the low bytes
     SBC XX12+4
     STA XX12+4
    
     LDA #0                 \ And then doing the high bytes, so now:
     SBC XX12+5             \
     STA XX12+5             \   XX12(5 4) = |delta_y|
    
    .LL110
    
     LDA XX12+3             \ If delta_x_hi is positive, jump down to LL111 to skip
     BPL LL111              \ the following
    
     SEC                    \ Otherwise flip the sign of delta_x to make it
     LDA #0                 \ positive, starting with the low bytes
     SBC XX12+2
     STA XX12+2
    
     LDA #0                 \ And then doing the high bytes, so now:
     SBC XX12+3             \
                            \   (A XX12+2) = |delta_x|
    
    .LL111
    
                            \ We now keep halving |delta_x| and |delta_y| until
                            \ both of them have zero in their high bytes
    
     TAX                    \ If |delta_x_hi| is non-zero, skip the following
     BNE LL112
    
     LDX XX12+5             \ If |delta_y_hi| = 0, jump down to LL113 (as both
     BEQ LL113              \ |delta_x_hi| and |delta_y_hi| are 0)
    
    .LL112
    
     LSR A                  \ Halve the value of delta_x in (A XX12+2)
     ROR XX12+2
    
     LSR XX12+5             \ Halve the value of delta_y XX12(5 4)
     ROR XX12+4
    
     JMP LL111              \ Loop back to LL111
    
    .LL113
    
                            \ By now, the high bytes of both |delta_x| and |delta_y|
                            \ are zero
    
     STX T                  \ We know that X = 0 as that's what we tested with a BEQ
                            \ above, so this sets T = 0
    
     LDA XX12+2             \ If delta_x_lo < delta_y_lo, so our line is more
     CMP XX12+4             \ vertical than horizontal, jump to LL114
     BCC LL114
    
                            \ If we get here then our line is more horizontal than
                            \ vertical, so it is a shallow slope
    
     STA Q                  \ Set Q = delta_x_lo
    
     LDA XX12+4             \ Set A = delta_y_lo
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
                            \     = 256 * delta_y_lo / delta_x_lo
    
     JMP LL116              \ Jump to LL116, as we now have the line's gradient in R
    
    .LL114
    
                            \ If we get here then our line is more vertical than
                            \ horizontal, so it is a steep slope
    
     LDA XX12+4             \ Set Q = delta_y_lo
     STA Q
     LDA XX12+2             \ Set A = delta_x_lo
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
                            \     = 256 * delta_x_lo / delta_y_lo
    
     DEC T                  \ T was set to 0 above, so this sets T = &FF when our
                            \ line is steep
    
    
    
    
           Name: LL145 (Part 4 of 4)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Clip line: Call the routine in LL188 to do the actual clipping
      Deep dive: [Line-clipping](https://elite.bbcelite.com/deep_dives/line-clipping.html)
                 [Extended screen coordinates](https://elite.bbcelite.com/deep_dives/extended_screen_coordinates.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll145_part_4_of_4.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part sets things up to call the routine in LL188, which does the actual
     clipping.
    
     If we get here, then R has been set to the gradient of the line (x1, y1) to
     (x2, y2), with T indicating the gradient of slope:
    
       * 0   = shallow slope (more horizontal than vertical)
    
       * &FF = steep slope (more vertical than horizontal)
    
     and XX13 has been set as follows:
    
       * 0   = (x1, y1) off-screen, (x2, y2) on-screen
    
       * 95  = (x1, y1) on-screen,  (x2, y2) off-screen
    
       * 191 = (x1, y1) off-screen, (x2, y2) off-screen
    
    
    
    
    .LL116
    
     LDA R                  \ Store the gradient in XX12+2
     STA XX12+2
    
     LDA S                  \ Store the type of slope in XX12+3, bit 7 clear means
     STA XX12+3             \ top left to bottom right, bit 7 set means top right to
                            \ bottom left
    
     LDA XX13               \ If XX13 = 0, skip the following instruction
     BEQ LL138
    
     BPL LLX117             \ If XX13 is positive, it must be 95. This means
                            \ (x1, y1) is on-screen but (x2, y2) isn't, so we jump
                            \ to LLX117 to swap the (x1, y1) and (x2, y2)
                            \ coordinates around before doing the actual clipping,
                            \ because we need to clip (x2, y2) but the clipping
                            \ routine at LL118 only clips (x1, y1)
    
    .LL138
    
                            \ If we get here, XX13 = 0 or 191, so (x1, y1) is
                            \ off-screen and needs clipping
    
     JSR LL118              \ Call LL118 to move (x1, y1) along the line onto the
                            \ screen, i.e. clip the line at the (x1, y1) end
    
     LDA XX13               \ If XX13 = 0, i.e. (x2, y2) is on-screen, jump down to
     BPL LL124              \ LL124 to return with a successfully clipped line
    
    .LL117
    
                            \ If we get here, XX13 = 191 (both coordinates are
                            \ off-screen)
    
     LDA XX15+1             \ If either of x1_hi or y1_hi are non-zero, jump to
     ORA XX15+3             \ LL137 to return from the subroutine with the C flag
     BNE LL137              \ set, as the line doesn't fit on-screen
    
     LDA XX15+2             \ If y1_lo > y-coordinate of the bottom of the screen
     CMP #Y*2               \ jump to LL137 to return from the subroutine with the
     BCS LL137              \ C flag set, as the line doesn't fit on-screen
    
    .LLX117
    
                            \ If we get here, XX13 = 95 or 191, and in both cases
                            \ (x2, y2) is off-screen, so we now need to swap the
                            \ (x1, y1) and (x2, y2) coordinates around before doing
                            \ the actual clipping, because we need to clip (x2, y2)
                            \ but the clipping routine at LL118 only clips (x1, y1)
    
     LDX XX15               \ Swap x1_lo = x2_lo
     LDA XX15+4
     STA XX15
     STX XX15+4
    
     LDA XX15+5             \ Swap x2_lo = x1_lo
     LDX XX15+1
     STX XX15+5
     STA XX15+1
    
     LDX XX15+2             \ Swap y1_lo = y2_lo
     LDA XX12
     STA XX15+2
     STX XX12
    
     LDA XX12+1             \ Swap y2_lo = y1_lo
     LDX XX15+3
     STX XX12+1
     STA XX15+3
    
     JSR LL118              \ Call LL118 to move (x1, y1) along the line onto the
                            \ screen, i.e. clip the line at the (x1, y1) end
    
     DEC SWAP               \ Set SWAP = &FF to indicate that we just clipped the
                            \ line at the (x2, y2) end by swapping the coordinates
                            \ (the DEC does this as we set SWAP to 0 at the start of
                            \ this subroutine)
    
    .LL124
    
     PLA                    \ Restore Y from the stack so it gets preserved through
     TAY                    \ the call to this subroutine
    
     JMP LL146              \ Jump up to LL146 to move the low bytes of (x1, y1) and
                            \ (x2, y2) into (X1, Y1) and (X2, Y2), and return from
                            \ the subroutine with a successfully clipped line
    
    .LL137
    
     PLA                    \ Restore Y from the stack so it gets preserved through
     TAY                    \ the call to this subroutine
    
     SEC                    \ Set the C flag to indicate the clipped line does not
                            \ fit on-screen
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LL9 (Part 12 of 12)                                     [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Draw all the visible edges from the ship line heap
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll9_part_12_of_12.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll9_part_12_of_12.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md) calls via LSCLR
                 * [LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md) calls via LSCLR
                 * [SHPPT](../main/subroutine/shppt.md) calls via LSCLR
                 * [LSPUT](../main/subroutine/lsput.md) calls via LSC3
    
    
    
    
    
    * * *
    
    
     This part draws any remaining lines from the old ship that are still in the
     ship line heap.
    
    
    
    * * *
    
    
     Other entry points:
    
       LSCLR                Draw any remaining lines from the old ship that are
                            still in the ship line heap
    
       LSC3                 Contains an RTS
    
    
    
    
    .LSCLR
    
     LDY LSNUM              \ Set Y to the offset in the line heap LSNUM
    
    .LSC1
    
     CPY LSNUM2             \ If Y >= LSNUM2, jump to LSC2 to return from the ship
     BCS LSC2               \ drawing routine, because the index in Y is greater
                            \ than the size of the existing ship line heap, which
                            \ means we have already erased all the old ship's lines
                            \ when drawing the new ship
    
                            \ If we get here then Y < LSNUM2, which means Y is
                            \ pointing to an on-screen line from the old ship that
                            \ we need to erase
    
     LDA (XX19),Y           \ Fetch the X1 line coordinate from the heap and store
     INY                    \ it in XX15, incrementing the heap pointer
     STA XX15
    
     LDA (XX19),Y           \ Fetch the Y1 line coordinate from the heap and store
     INY                    \ it in XX15+1, incrementing the heap pointer
     STA XX15+1
    
     LDA (XX19),Y           \ Fetch the X2 line coordinate from the heap and store
     INY                    \ it in XX15+2, incrementing the heap pointer
     STA XX15+2
    
     LDA (XX19),Y           \ Fetch the Y2 line coordinate from the heap and store
     INY                    \ it in XX15+3, incrementing the heap pointer
     STA XX15+3
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2) to erase it from
                            \ the screen
    
     JMP LSC1               \ Loop back to LSC1 to draw (i.e. erase) the next line
                            \ from the heap
    
    .LSC2
    
     LDA LSNUM              \ Store LSNUM in the first byte of the ship line heap
     LDY #0
     STA (XX19),Y
    
    .LSC3
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LSPUT                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a ship line using flicker-free animation
    
    
        Context: See this subroutine [on its own page](../main/subroutine/lsput.md)
     References: This subroutine is called as follows:
                 * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md) calls LSPUT
                 * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md) calls LSPUT
                 * [SHPPT](../main/subroutine/shppt.md) calls LSPUT
    
    
    
    
    
    * * *
    
    
     This routine implements flicker-free ship animation by erasing and redrawing
     each individual line in the ship, rather than the approach in the other
     Acornsoft versions of the game, which erase the entire existing ship before
     drawing the new one.
    
     Here's the new approach in this routine:
    
       * Draw the new line
    
       * Fetch the corresponding existing line (in position LSNUM) from the heap
    
       * Store the new line in the heap at this position, replacing the old one
    
       * If the existing line we just took from the heap is on-screen, erase it
    
    
    
    * * *
    
    
     Arguments:
    
       LSNUM                The offset within the line heap where we add the new
                            line's coordinates
    
       X1                   The screen x-coordinate of the start of the line to add
                            to the ship line heap
    
       Y1                   The screen y-coordinate of the start of the line to add
                            to the ship line heap
    
       X2                   The screen x-coordinate of the end of the line to add
                            to the ship line heap
    
       Y2                   The screen y-coordinate of the end of the line to add
                            to the ship line heap
    
       XX19(1 0)            XX19(1 0) shares its location with INWK(34 33), which
                            contains the ship line heap address pointer
    
    
    
    * * *
    
    
     Returns:
    
       LSNUM                The offset of the next line in the line heap
    
    
    
    
    .LSPUT
    
     LDY LSNUM              \ Set Y = LSNUM, to get the offset within the ship line
                            \ heap where we want to insert our new line
    
     CPY LSNUM2             \ Compare LSNUM and LSNUM2 and store the flags on the
     PHP                    \ stack so we can retrieve them later
    
     LDX #3                 \ We now want to copy the line coordinates (X1, Y1) and
                            \ (X2, Y2) to XX12...XX12+3, so set a counter to copy
                            \ 4 bytes
    
    .LSC4
    
     LDA X1,X               \ Copy the X-th byte of X1/Y1/X2/Y2 to the X-th byte of
     STA XX12,X             \ XX12
    
     DEX                    \ Decrement the loop counter
    
     BPL LSC4               \ Loop back until we have copied all four bytes
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2)
    
     LDA (XX19),Y           \ Set X1 to the Y-th coordinate on the ship line heap,
     STA X1                 \ i.e. the one we are replacing in the heap
    
     LDA XX12               \ Replace it with the X1 coordinate in XX12
     STA (XX19),Y
    
     INY                    \ Increment the index to point to the Y1 coordinate
    
     LDA (XX19),Y           \ Set Y1 to the Y-th coordinate on the ship line heap,
     STA Y1                 \ i.e. the one we are replacing in the heap
    
     LDA XX12+1             \ Replace it with the Y1 coordinate in XX12+1
     STA (XX19),Y
    
     INY                    \ Increment the index to point to the X2 coordinate
    
     LDA (XX19),Y           \ Set X2 to the Y-th coordinate on the ship line heap,
     STA X2                 \ i.e. the one we are replacing in the heap
    
     LDA XX12+2             \ Replace it with the X2 coordinate in XX12+2
     STA (XX19),Y
    
     INY                    \ Increment the index to point to the Y2 coordinate
    
     LDA (XX19),Y           \ Set Y2 to the Y-th coordinate on the ship line heap,
     STA Y2                 \ i.e. the one we are replacing in the heap
    
     LDA XX12+3             \ Replace it with the Y2 coordinate in XX12+3
     STA (XX19),Y
    
     INY                    \ Increment the index to point to the next coordinate
     STY LSNUM              \ and store the updated index in LSNUM
    
     PLP                    \ Restore the result of the comparison above, so if the
     BCS LSC3               \ original value of LSNUM >= LSNUM2, then we have
                            \ already redrawn all the lines from the old ship's line
                            \ heap, so return from the subroutine (as LSC3 contains
                            \ an RTS)
    
     JMP LOIN               \ Otherwise there are still more lines to erase from the
                            \ old ship on-screen, so the coordinates in (X1, Y1) and
                            \ (X2, Y2) that we just pulled from the ship line heap
                            \ point to a line that is still on-screen, so call LOIN
                            \ to draw this line and erase it from the screen,
                            \ returning from the subroutine using a tail call
    
    
    
     Save ELTG.bin
    
    
    
    
     PRINT "ELITE G"
     PRINT "Assembled at ", ~CODE_G%
     PRINT "Ends at ", ~P%
     PRINT "Code size is ", ~(P% - CODE_G%)
     PRINT "Execute at ", ~LOAD%
     PRINT "Reload at ", ~LOAD_G%
    
     PRINT "S.ELTG ", ~CODE_G%, " ", ~P%, " ", ~LOAD%, " ", ~LOAD_G%
    \SAVE "3-assembled-output/ELTG.bin", CODE_G%, P%, LOAD%
    
    
    

[X]

Entry point [CLIP](elite_g.md#clip) in subroutine [LL145 (Part 1 of 4)](elite_g.md#header-ll145-part-1-of-4) (category: Drawing lines)

Another name for LL145

[X]

Entry point [CLIP2](elite_g.md#clip2) in subroutine [LL145 (Part 1 of 4)](elite_g.md#header-ll145-part-1-of-4) (category: Drawing lines)

Don't initialise the values in SWAP or A

[X]

Variable [CNT](workspaces.md#cnt) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [COL](workspaces.md#col) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [DOEXP](elite_e.md#header-doexp) (category: Drawing ships)

Draw an exploding ship

[X]

Subroutine [DORND](elite_f.md#header-dornd) (category: Maths (Arithmetic))

Generate random numbers

[X]

Label [EE28](elite_g.md#ee28) in subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12)

[X]

Label [EE29](elite_g.md#ee29) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Label [EE30](elite_g.md#ee30) in subroutine [LL9 (Part 4 of 12)](elite_g.md#header-ll9-part-4-of-12)

[X]

Label [EE31](elite_g.md#ee31) in subroutine [LL9 (Part 9 of 12)](elite_g.md#header-ll9-part-9-of-12)

[X]

Entry point [EE51](elite_g.md#ee51) in subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12) (category: Drawing ships)

Remove the current ship from the screen, called from SHPPT before drawing the ship as a point

[X]

Label [EE55](elite_g.md#ee55) in subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12)

[X]

Subroutine [FMLTU](elite_c.md#header-fmltu) (category: Maths (Arithmetic))

Calculate A = A * Q / 256

[X]

Variable [INF](workspaces.md#inf) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](workspaces.md#inwk) in workspace [ZP](workspaces.md#header-zp)

The zero-page internal workspace for the current ship data block

[X]

Variable [K3](workspaces.md#k3) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K4](workspaces.md#k4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Label [LL10](elite_g.md#ll10) in subroutine [LL9 (Part 2 of 12)](elite_g.md#header-ll9-part-2-of-12)

[X]

Entry point [LL10-1](elite_g.md#ll10) in subroutine [LL9 (Part 2 of 12)](elite_g.md#header-ll9-part-2-of-12) (category: Drawing ships)

Contains an RTS

[X]

Label [LL107](elite_g.md#ll107) in subroutine [LL145 (Part 1 of 4)](elite_g.md#header-ll145-part-1-of-4)

[X]

Label [LL108](elite_g.md#ll108) in subroutine [LL145 (Part 1 of 4)](elite_g.md#header-ll145-part-1-of-4)

[X]

Label [LL109](elite_g.md#ll109) in subroutine [LL145 (Part 1 of 4)](elite_g.md#header-ll145-part-1-of-4)

[X]

Label [LL110](elite_g.md#ll110) in subroutine [LL145 (Part 3 of 4)](elite_g.md#header-ll145-part-3-of-4)

[X]

Label [LL111](elite_g.md#ll111) in subroutine [LL145 (Part 3 of 4)](elite_g.md#header-ll145-part-3-of-4)

[X]

Label [LL112](elite_g.md#ll112) in subroutine [LL145 (Part 3 of 4)](elite_g.md#header-ll145-part-3-of-4)

[X]

Label [LL113](elite_g.md#ll113) in subroutine [LL145 (Part 3 of 4)](elite_g.md#header-ll145-part-3-of-4)

[X]

Label [LL114](elite_g.md#ll114) in subroutine [LL145 (Part 3 of 4)](elite_g.md#header-ll145-part-3-of-4)

[X]

Label [LL115](elite_g.md#ll115) in subroutine [LL145 (Part 3 of 4)](elite_g.md#header-ll145-part-3-of-4)

[X]

Label [LL116](elite_g.md#ll116) in subroutine [LL145 (Part 4 of 4)](elite_g.md#header-ll145-part-4-of-4)

[X]

Subroutine [LL118](elite_g.md#header-ll118) (category: Drawing lines)

Move a point along a line until it is on-screen

[X]

Label [LL119](elite_g.md#ll119) in subroutine [LL118](elite_g.md#header-ll118)

[X]

Subroutine [LL120](elite_g.md#header-ll120) (category: Maths (Arithmetic))

Calculate (Y X) = (S x1_lo) * XX12+2 or (S x1_lo) / XX12+2

[X]

Entry point [LL121](elite_g.md#ll121) in subroutine [LL123](elite_g.md#header-ll123) (category: Maths (Arithmetic))

Calculate (Y X) = (S R) / Q and set the sign to the opposite of the top byte on the stack

[X]

Entry point [LL122](elite_g.md#ll122) in subroutine [LL120](elite_g.md#header-ll120) (category: Maths (Arithmetic))

Calculate (Y X) = (S R) * Q and set the sign to the opposite of the top byte on the stack

[X]

Subroutine [LL123](elite_g.md#header-ll123) (category: Maths (Arithmetic))

Calculate (Y X) = (S R) / XX12+2 or (S R) * XX12+2

[X]

Label [LL124](elite_g.md#ll124) in subroutine [LL145 (Part 4 of 4)](elite_g.md#header-ll145-part-4-of-4)

[X]

Label [LL125](elite_g.md#ll125) in subroutine [LL120](elite_g.md#header-ll120)

[X]

Label [LL126](elite_g.md#ll126) in subroutine [LL120](elite_g.md#header-ll120)

[X]

Label [LL127](elite_g.md#ll127) in subroutine [LL129](elite_g.md#header-ll129)

[X]

Entry point [LL128](elite_g.md#ll128) in subroutine [LL123](elite_g.md#header-ll123) (category: Maths (Arithmetic))

Contains an RTS

[X]

Subroutine [LL129](elite_g.md#header-ll129) (category: Maths (Arithmetic))

Calculate Q = XX12+2, A = S EOR XX12+3 and (S R) = |S R|

[X]

Label [LL13](elite_g.md#ll13) in subroutine [LL9 (Part 2 of 12)](elite_g.md#header-ll9-part-2-of-12)

[X]

Label [LL130](elite_g.md#ll130) in subroutine [LL123](elite_g.md#header-ll123)

[X]

Label [LL131](elite_g.md#ll131) in subroutine [LL123](elite_g.md#header-ll123)

[X]

Label [LL132](elite_g.md#ll132) in subroutine [LL123](elite_g.md#header-ll123)

[X]

Entry point [LL133](elite_g.md#ll133) in subroutine [LL123](elite_g.md#header-ll123) (category: Maths (Arithmetic))

Negate (Y X) and return from the subroutine

[X]

Label [LL134](elite_g.md#ll134) in subroutine [LL118](elite_g.md#header-ll118)

[X]

Label [LL135](elite_g.md#ll135) in subroutine [LL118](elite_g.md#header-ll118)

[X]

Label [LL136](elite_g.md#ll136) in subroutine [LL118](elite_g.md#header-ll118)

[X]

Label [LL137](elite_g.md#ll137) in subroutine [LL145 (Part 4 of 4)](elite_g.md#header-ll145-part-4-of-4)

[X]

Label [LL138](elite_g.md#ll138) in subroutine [LL145 (Part 4 of 4)](elite_g.md#header-ll145-part-4-of-4)

[X]

Label [LL14](elite_g.md#ll14) in subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12)

[X]

Label [LL140](elite_g.md#ll140) in subroutine [LL9 (Part 7 of 12)](elite_g.md#header-ll9-part-7-of-12)

[X]

Label [LL146](elite_g.md#ll146) in subroutine [LL145 (Part 1 of 4)](elite_g.md#header-ll145-part-1-of-4)

[X]

Label [LL15](elite_g.md#ll15) in subroutine [LL9 (Part 3 of 12)](elite_g.md#header-ll9-part-3-of-12)

[X]

Label [LL17](elite_g.md#ll17) in subroutine [LL9 (Part 3 of 12)](elite_g.md#header-ll9-part-3-of-12)

[X]

Label [LL170](elite_g.md#ll170) in subroutine [LL9 (Part 10 of 12)](elite_g.md#header-ll9-part-10-of-12)

[X]

Label [LL2](elite_g.md#ll2) in subroutine [LL28](elite_g.md#header-ll28)

[X]

Label [LL21](elite_g.md#ll21) in subroutine [LL9 (Part 3 of 12)](elite_g.md#header-ll9-part-3-of-12)

[X]

Label [LL25](elite_g.md#ll25) in subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12)

[X]

Subroutine [LL28](elite_g.md#header-ll28) (category: Maths (Arithmetic))

Calculate R = 256 * A / Q

[X]

Label [LL29](elite_g.md#ll29) in subroutine [LL28](elite_g.md#header-ll28)

[X]

Entry point [LL31](elite_g.md#ll31) in subroutine [LL28](elite_g.md#header-ll28) (category: Maths (Arithmetic))

Skips the A >= Q check and does not set the R counter, so this can be used for jumping straight into the division loop if R is already set to 254 and we know the division will work

[X]

Subroutine [LL38](elite_g.md#header-ll38) (category: Maths (Arithmetic))

Calculate (S A) = (S R) + (A Q)

[X]

Label [LL39](elite_g.md#ll39) in subroutine [LL38](elite_g.md#header-ll38)

[X]

Label [LL41](elite_g.md#ll41) in subroutine [LL9 (Part 4 of 12)](elite_g.md#header-ll9-part-4-of-12)

[X]

Label [LL42](elite_g.md#ll42) in subroutine [LL9 (Part 6 of 12)](elite_g.md#header-ll9-part-6-of-12)

[X]

Label [LL48](elite_g.md#ll48) in subroutine [LL9 (Part 6 of 12)](elite_g.md#header-ll9-part-6-of-12)

[X]

Label [LL49](elite_g.md#ll49) in subroutine [LL9 (Part 6 of 12)](elite_g.md#header-ll9-part-6-of-12)

[X]

[X]

Label [LL50](elite_g.md#ll50) in subroutine [LL9 (Part 8 of 12)](elite_g.md#header-ll9-part-8-of-12)

[X]

Subroutine [LL51](elite_g.md#header-ll51) (category: Maths (Geometry))

Calculate the dot product of XX15 and XX16

[X]

Label [LL52](elite_g.md#ll52) in subroutine [LL9 (Part 6 of 12)](elite_g.md#header-ll9-part-6-of-12)

[X]

Label [LL53](elite_g.md#ll53) in subroutine [LL9 (Part 6 of 12)](elite_g.md#header-ll9-part-6-of-12)

[X]

Label [LL54](elite_g.md#ll54) in subroutine [LL9 (Part 6 of 12)](elite_g.md#header-ll9-part-6-of-12)

[X]

Label [LL55](elite_g.md#ll55) in subroutine [LL9 (Part 6 of 12)](elite_g.md#header-ll9-part-6-of-12)

[X]

Label [LL56](elite_g.md#ll56) in subroutine [LL9 (Part 7 of 12)](elite_g.md#header-ll9-part-7-of-12)

[X]

Label [LL57](elite_g.md#ll57) in subroutine [LL9 (Part 7 of 12)](elite_g.md#header-ll9-part-7-of-12)

[X]

Label [LL6](elite_g.md#ll6) in subroutine [LL5](elite_g.md#header-ll5)

[X]

Label [LL60](elite_g.md#ll60) in subroutine [LL9 (Part 8 of 12)](elite_g.md#header-ll9-part-8-of-12)

[X]

Subroutine [LL61](elite_g.md#header-ll61) (category: Maths (Arithmetic))

Calculate (U R) = 256 * A / Q

[X]

Subroutine [LL62](elite_g.md#header-ll62) (category: Maths (Arithmetic))

Calculate 128 - (U R)

[X]

Label [LL63](elite_g.md#ll63) in subroutine [LL61](elite_g.md#header-ll61)

[X]

Label [LL64](elite_g.md#ll64) in subroutine [LL61](elite_g.md#header-ll61)

[X]

Entry point [LL66](elite_g.md#ll66) in subroutine [LL9 (Part 8 of 12)](elite_g.md#header-ll9-part-8-of-12) (category: Drawing ships)

A re-entry point into the ship-drawing routine, used by the LL62 routine to store 128 - (U R) on the XX3 heap

[X]

Label [LL67](elite_g.md#ll67) in subroutine [LL9 (Part 8 of 12)](elite_g.md#header-ll9-part-8-of-12)

[X]

Label [LL68](elite_g.md#ll68) in subroutine [LL9 (Part 8 of 12)](elite_g.md#header-ll9-part-8-of-12)

[X]

Label [LL69](elite_g.md#ll69) in subroutine [LL9 (Part 8 of 12)](elite_g.md#header-ll9-part-8-of-12)

[X]

[X]

Label [LL7](elite_g.md#ll7) in subroutine [LL5](elite_g.md#header-ll5)

[X]

Label [LL70](elite_g.md#ll70) in subroutine [LL9 (Part 8 of 12)](elite_g.md#header-ll9-part-8-of-12)

[X]

Label [LL72](elite_g.md#ll72) in subroutine [LL9 (Part 9 of 12)](elite_g.md#header-ll9-part-9-of-12)

[X]

Label [LL75](elite_g.md#ll75) in subroutine [LL9 (Part 10 of 12)](elite_g.md#header-ll9-part-10-of-12)

[X]

Label [LL78](elite_g.md#ll78) in subroutine [LL9 (Part 11 of 12)](elite_g.md#header-ll9-part-11-of-12)

[X]

Label [LL79](elite_g.md#ll79) in subroutine [LL9 (Part 10 of 12)](elite_g.md#header-ll9-part-10-of-12)

[X]

Label [LL81](elite_g.md#ll81) in subroutine [LL9 (Part 11 of 12)](elite_g.md#header-ll9-part-11-of-12)

[X]

Label [LL83](elite_g.md#ll83) in subroutine [LL145 (Part 2 of 4)](elite_g.md#header-ll145-part-2-of-4)

[X]

Label [LL84](elite_g.md#ll84) in subroutine [LL61](elite_g.md#header-ll61)

[X]

Label [LL86](elite_g.md#ll86) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Label [LL87](elite_g.md#ll87) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Label [LL88](elite_g.md#ll88) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Label [LL89](elite_g.md#ll89) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Label [LL90](elite_g.md#ll90) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

[X]

Label [LL91](elite_g.md#ll91) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Label [LL92](elite_g.md#ll92) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Label [LL93](elite_g.md#ll93) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

[X]

Label [LL94](elite_g.md#ll94) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Label [LLX117](elite_g.md#llx117) in subroutine [LL145 (Part 4 of 4)](elite_g.md#header-ll145-part-4-of-4)

[X]

Label [LLfix](elite_g.md#llfix) in subroutine [LL28](elite_g.md#header-ll28)

[X]

Subroutine [LOIN](elite_a.md#header-loin) (category: Drawing lines)

Draw a one-segment line

[X]

Label [LSC1](elite_g.md#lsc1) in subroutine [LL9 (Part 12 of 12)](elite_g.md#header-ll9-part-12-of-12)

[X]

Label [LSC2](elite_g.md#lsc2) in subroutine [LL9 (Part 12 of 12)](elite_g.md#header-ll9-part-12-of-12)

[X]

Entry point [LSC3](elite_g.md#lsc3) in subroutine [LL9 (Part 12 of 12)](elite_g.md#header-ll9-part-12-of-12) (category: Drawing ships)

Contains an RTS

[X]

Label [LSC4](elite_g.md#lsc4) in subroutine [LSPUT](elite_g.md#header-lsput)

[X]

Entry point [LSCLR](elite_g.md#lsclr) in subroutine [LL9 (Part 12 of 12)](elite_g.md#header-ll9-part-12-of-12) (category: Drawing ships)

Draw any remaining lines from the old ship that are still in the ship line heap

[X]

Variable [LSNUM](workspaces.md#lsnum) in workspace [ZP](workspaces.md#header-zp)

The pointer to the current position in the ship line heap as we work our way through the new ship's edges (and the corresponding old ship's edges) when drawing the ship in the main ship-drawing routine at LL9

[X]

Variable [LSNUM2](workspaces.md#lsnum2) in workspace [ZP](workspaces.md#header-zp)

The size of the existing ship line heap for the ship we are drawing in LL9, i.e. the number of lines in the old ship that is currently shown on-screen and which we need to erase

[X]

Subroutine [LSPUT](elite_g.md#header-lsput) (category: Drawing lines)

Draw a ship line using flicker-free animation

[X]

Variable [NEWB](workspaces.md#newb) in workspace [ZP](workspaces.md#header-zp)

The ship's "new byte flags" (or NEWB flags)

[X]

Variable [P](workspaces.md#p) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [PLANET](elite_e.md#header-planet) (category: Drawing planets)

Draw the planet or sun

[X]

Subroutine [PROJ](elite_e.md#header-proj) (category: Maths (Geometry))

Project the current ship or planet onto the screen

[X]

Variable [Q](workspaces.md#q) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [R](workspaces.md#r) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [S](workspaces.md#s) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [SHPPT](elite_g.md#header-shppt) (category: Drawing ships)

Draw a distant ship as a point rather than a full wireframe

[X]

Variable [SWAP](workspaces.md#swap) in workspace [WP](workspaces.md#header-wp)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[X]

Label [Shpt](elite_g.md#shpt) in subroutine [SHPPT](elite_g.md#header-shppt)

[X]

Variable [T](workspaces.md#t) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [TYPE](workspaces.md#type) in workspace [ZP](workspaces.md#header-zp)

The current ship type

[X]

Variable [U](workspaces.md#u) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [V](workspaces.md#v) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing an address pointer

[X]

Variable [X1](workspaces.md#x1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](workspaces.md#x2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XX0](workspaces.md#xx0) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX1](workspaces.md#xx1) in workspace [ZP](workspaces.md#header-zp)

This is an alias for INWK that is used in the main ship-drawing routine at LL9

[X]

Variable [XX12](workspaces.md#xx12) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX13](workspaces.md#xx13) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used in the line-drawing routines

[X]

Variable [XX15](workspaces.md#xx15) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Variable [XX16](workspaces.md#xx16) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX17](workspaces.md#xx17) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in BPRNT to store the number of characters to print, and as the edge counter in the main ship-drawing routine

[X]

Variable [XX18](workspaces.md#xx18) in workspace [ZP](workspaces.md#header-zp)

Temporary storage used to store coordinates in the LL9 ship-drawing routine

[X]

Variable [XX19](workspaces.md#xx19) in workspace [ZP](workspaces.md#header-zp)

XX19(1 0) shares its location with INWK(34 33), which contains the address of the ship line heap

[X]

Variable [XX2](workspaces.md#xx2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the visibility of the ship's faces during the ship-drawing routine at LL9

[X]

Variable [XX20](workspaces.md#xx20) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Workspace [XX3](workspaces.md#header-xx3) (category: Workspaces)

Temporary storage space for complex calculations

[X]

Variable [XX4](workspaces.md#xx4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [Y](workspaces.md#y)

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Y1](workspaces.md#y1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](workspaces.md#y2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [alogh](elite_a.md#header-alogh) (category: Maths (Arithmetic))

Binary antilogarithm table

[X]

Label [ll51](elite_g.md#ll51) in subroutine [LL51](elite_g.md#header-ll51)

[X]

Label [ll91](elite_g.md#ll91) in subroutine [LL9 (Part 3 of 12)](elite_g.md#header-ll9-part-3-of-12)

[X]

Variable [log](elite_a.md#header-log) (category: Maths (Arithmetic))

Binary logarithm table (high byte)

[X]

Variable [logL](elite_a.md#header-logl) (category: Maths (Arithmetic))

Binary logarithm table (low byte)

[X]

Label [nono](elite_g.md#nono) in subroutine [SHPPT](elite_g.md#header-shppt)

[X]

Label [ovflw](elite_g.md#ovflw) in subroutine [LL9 (Part 5 of 12)](elite_g.md#header-ll9-part-5-of-12)

[X]

Variable [shpcol](elite_a.md#header-shpcol) (category: Drawing ships)

Ship colours

[X]

Variable [widget](workspaces.md#widget) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the original argument in A in the logarithmic FMLTU and LL28 routines

[Elite F source](elite_f.md "Previous routine")[Elite H source](elite_h.md "Next routine")
