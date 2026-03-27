---
title: "The STARS6 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/stars6.html"
---

[STARS1](stars1.md "Previous routine")[MAS1](mas1.md "Next routine")
    
    
           Name: STARS6                                                  [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Process the stardust for the rear view
      Deep dive: [Stardust in the front view](https://elite.bbcelite.com/deep_dives/stardust_in_the_front_view.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-stars6)
     Variations: See [code variations](../../related/compare/main/subroutine/stars6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STARS](stars.md) calls STARS6
    
    
    
    
    
    * * *
    
    
     This routine is very similar to STARS1, which processes stardust for the front
     view. The main difference is that the direction of travel is reversed, so the
     signs in the calculations are different, as well as the order of the first
     batch of calculations.
    
     When a stardust particle falls away into the far distance, it is removed from
     the screen and its memory is recycled as a new particle, positioned randomly
     along one of the four edges of the screen.
    
     These are the calculations referred to in the commentary:
    
       1. q = 64 * speed / z_hi
       2. z = z - speed * 64
       3. y = y + |y_hi| * q
       4. x = x + |x_hi| * q
    
       5. y = y + alpha * x / 256
       6. x = x - alpha * y / 256
    
       7. x = x + 2 * (beta * y / 256) ^ 2
       8. y = y - beta * 256
    
     For more information see the associated deep dive.
    
    
    
    
    .STARS6
    
     LDY NOSTM              \ Set Y to the current number of stardust particles, so
                            \ we can use it as a counter through all the stardust
    
    .STL6
    
     JSR DV42               \ Call DV42 to set the following:
                            \
                            \   (P R) = 256 * DELTA / z_hi
                            \         = 256 * speed / z_hi
                            \
                            \ The maximum value returned is P = 2 and R = 128 (see
                            \ DV42 for an explanation)
    
     LDA R                  \ Set A = R, so now:
                            \
                            \   (P A) = 256 * speed / z_hi
    
     LSR P                  \ Rotate (P A) right by 2 places, which sets P = 0 (as P
     ROR A                  \ has a maximum value of 2) and leaves:
     LSR P                  \
     ROR A                  \   A = 64 * speed / z_hi
    
     ORA #1                 \ Make sure A is at least 1, and store it in Q, so we
     STA Q                  \ now have result 1 above:
                            \
                            \   Q = 64 * speed / z_hi
    
     LDA SX,Y               \ Set X1 = A = x_hi
     STA X1                 \
                            \ So X1 contains the original value of x_hi, which we
                            \ use below to remove the existing particle
    
     JSR MLU2               \ Set (A P) = |x_hi| * Q
    
                            \ We now calculate:
                            \
                            \   XX(1 0) = x - (A P)
    
     STA XX+1               \ First we do the low bytes:
     LDA SXL,Y              \
     SBC P                  \   XX(1 0) = x_lo - (A P)
     STA XX
    
     LDA X1                 \ And then we do the high bytes:
     SBC XX+1               \
     STA XX+1               \   XX(1 0) = (x_hi 0) - XX(1 0)
                            \
                            \ so we get our result:
                            \
                            \   XX(1 0) = x - (A P)
                            \           = x - |x_hi| * Q
                            \
                            \ which is result 2 above, and we also have:
    
     JSR MLU1               \ Call MLU1 to set:
                            \
                            \   Y1 = y_hi
                            \
                            \   (A P) = |y_hi| * Q
                            \
                            \ So Y1 contains the original value of y_hi, which we
                            \ use below to remove the existing particle
    
                            \ We now calculate:
                            \
                            \   (S R) = YY(1 0) = y - (A P)
    
     STA YY+1               \ First we do the low bytes with:
     LDA SYL,Y              \
     SBC P                  \   YY+1 = A
     STA YY                 \   R = YY = y_lo - P
     STA R                  \
                            \ so we get this:
                            \
                            \   (? R) = YY(1 0) = y_lo - (A P)
    
     LDA Y1                 \ And then we do the high bytes with:
     SBC YY+1               \
     STA YY+1               \   S = YY+1 = y_hi - YY+1
     STA S                  \
                            \ so we get our result:
                            \
                            \   (S R) = YY(1 0) = (y_hi y_lo) - (A P)
                            \                   = y - |y_hi| * Q
                            \
                            \ which is result 3 above, and (S R) is set to the new
                            \ value of y
    
     LDA SZL,Y              \ We now calculate the following:
     ADC DELT4              \
     STA SZL,Y              \  (z_hi z_lo) = (z_hi z_lo) + DELT4(1 0)
                            \
                            \ starting with the low bytes
    
     LDA SZ,Y               \ And then we do the high bytes
     STA ZZ                 \
     ADC DELT4+1            \ We also set ZZ to the original value of z_hi, which we
     STA SZ,Y               \ use below to remove the existing particle
                            \
                            \ So now we have result 4 above:
                            \
                            \   z = z + DELT4(1 0)
                            \     = z + speed * 64
    
     LDA XX+1               \ EOR x with the correct sign of the roll angle alpha,
     EOR ALP2               \ so A has the opposite sign to the roll angle alpha
    
     JSR MLS1               \ Call MLS1 to calculate:
                            \
                            \   (A P) = A * ALP1
                            \         = (-x / 256) * alpha
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = (-x / 256) * alpha + y
                            \         = y - alpha * x / 256
    
     STA YY+1               \ Set YY(1 0) = (A X) to give:
     STX YY                 \
                            \   YY(1 0) = y - alpha * x / 256
                            \
                            \ which is result 5 above, and we also have:
                            \
                            \   A = YY+1 = y - alpha * x / 256
                            \
                            \ i.e. A is the new value of y, divided by 256
    
     EOR ALP2+1             \ EOR with the flipped sign of the roll angle alpha, so
                            \ A has the opposite sign to the flipped roll angle
                            \ alpha, i.e. it gets the same sign as alpha
    
     JSR MLS2               \ Call MLS2 to calculate:
                            \
                            \   (S R) = XX(1 0)
                            \         = x
                            \
                            \   (A P) = A * ALP1
                            \         = y / 256 * alpha
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = y / 256 * alpha + x
    
     STA XX+1               \ Set XX(1 0) = (A X), which gives us result 6 above:
     STX XX                 \
                            \   x = x + alpha * y / 256
    
     LDA YY+1               \ Set A to y_hi and set it to the flipped sign of beta
     EOR BET2+1
    
     LDX BET1               \ Fetch the pitch magnitude into X
    
     JSR MULTS-2            \ Call MULTS-2 to calculate:
                            \
                            \   (A P) = X * A
                            \         = beta * y_hi
    
     STA Q                  \ Store the high byte of the result in Q, so:
                            \
                            \   Q = beta * y_hi / 256
    
     LDA XX+1               \ Set S = x_hi
     STA S
    
     EOR #%10000000         \ Flip the sign of A, so A now contains -x
    
     JSR MUT1               \ Call MUT1 to calculate:
                            \
                            \   R = XX = x_lo
                            \
                            \   (A P) = Q * A
                            \         = (beta * y_hi / 256) * (-beta * y_hi / 256)
                            \         = (-beta * y / 256) ^ 2
    
     ASL P                  \ Double (A P), store the top byte in A and set the C
     ROL A                  \ flag to bit 7 of the original A, so this does:
     STA T                  \
                            \   (T P) = (A P) << 1
                            \         = 2 * (-beta * y / 256) ^ 2
    
     LDA #0                 \ Set bit 7 in A to the sign bit from the A in the
     ROR A                  \ calculation above and apply it to T, so we now have:
     ORA T                  \
                            \   (A P) = -2 * (beta * y / 256) ^ 2
                            \
                            \ with the doubling retaining the sign of (A P)
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = -2 * (beta * y / 256) ^ 2 + x
    
     STA XX+1               \ Store the high byte A in XX+1
    
     TXA                    \ Store the low byte X in x_lo
     STA SXL,Y
    
                            \ So (XX+1 x_lo) now contains:
                            \
                            \   x = x - 2 * (beta * y / 256) ^ 2
                            \
                            \ which is result 7 above
    
     LDA YY                 \ Set (S R) = YY(1 0) = y
     STA R
     LDA YY+1
     STA S
    
    \EOR #128               \ These instructions are commented out in the original
    \JSR MAD                \ source
    \STA S
    \STX R
    
     LDA #0                 \ Set P = 0
     STA P
    
     LDA BETA               \ Set A = beta, so (A P) = (beta 0) = beta * 256
    
     JSR PIX1               \ Call PIX1 to calculate the following:
                            \
                            \   (YY+1 y_lo) = (A P) + (S R)
                            \               = beta * 256 + y
                            \
                            \ i.e. y = y + beta * 256, which is result 8 above
                            \
                            \ PIX1 also draws a particle at (X1, Y1) with distance
                            \ ZZ, which will remove the old stardust particle, as we
                            \ set X1, Y1 and ZZ to the original values for this
                            \ particle during the calculations above
    
                            \ We now have our newly moved stardust particle at
                            \ x-coordinate (XX+1 x_lo) and y-coordinate (YY+1 y_lo)
                            \ and distance z_hi, so we draw it if it's still on
                            \ screen, otherwise we recycle it as a new bit of
                            \ stardust and draw that
    
     LDA XX+1               \ Set X1 and x_hi to the high byte of XX in XX+1, so
     STA X1                 \ the new x-coordinate is in (x_hi x_lo) and the high
     STA SX,Y               \ byte is in X1
    
     LDA YY+1               \ Set Y1 and y_hi to the high byte of YY in YY+1, so
     STA SY,Y               \ the new x-coordinate is in (y_hi y_lo) and the high
     STA Y1                 \ byte is in Y1
    
     AND #%01111111         \ If |y_hi| >= 110 then jump to KILL6 to recycle this
     CMP #110               \ particle, as it's gone off the top or bottom of the
     BCS KILL6              \ screen, and rejoin at STC6 with the new particle
    
     LDA SZ,Y               \ If z_hi >= 160 then jump to KILL6 to recycle this
     CMP #160               \ particle, as it's so far away that it's too far to
     BCS KILL6              \ see, and rejoin at STC1 with the new particle
    
     STA ZZ                 \ Set ZZ to the z-coordinate in z_hi
    
    .STC6
    
     JSR PIXEL2             \ Draw a stardust particle at (X1,Y1) with distance ZZ,
                            \ i.e. draw the newly moved particle at (x_hi, y_hi)
                            \ with distance z_hi
    
     DEY                    \ Decrement the loop counter to point to the next
                            \ stardust particle
    
     BEQ ST3                \ If we have just done the last particle, skip the next
                            \ instruction to return from the subroutine
    
     JMP STL6               \ We have more stardust to process, so jump back up to
                            \ STL6 for the next particle
    
    .ST3
    
     RTS                    \ Return from the subroutine
    
    .KILL6
    
     JSR DORND              \ Set A and X to random numbers
    
     AND #%01111111         \ Clear the sign bit of A to get |A|
    
     ADC #10                \ Make sure A is at least 10 and store it in z_hi and
     STA SZ,Y               \ ZZ, so the new particle starts close to us
     STA ZZ
    
     LSR A                  \ Divide A by 2 and randomly set the C flag
    
     BCS ST4                \ Jump to ST4 half the time
    
     LSR A                  \ Randomly set the C flag again
    
     LDA #252               \ Set A to either +126 or -126 (252 >> 1) depending on
     ROR A                  \ the C flag, as this is a sign-magnitude number with
                            \ the C flag rotated into its sign bit
    
     STA X1                 \ Set x_hi and X1 to A, so this particle starts on
     STA SX,Y               \ either the left or right edge of the screen
    
     JSR DORND              \ Set A and X to random numbers
    
     STA Y1                 \ Set y_hi and Y1 to random numbers, so the particle
     STA SY,Y               \ starts anywhere along either the left or right edge
    
     JMP STC6               \ Jump up to STC6 to draw this new particle
    
    .ST4
    
     JSR DORND              \ Set A and X to random numbers
    
     STA X1                 \ Set x_hi and X1 to random numbers, so the particle
     STA SX,Y               \ starts anywhere along the x-axis
    
     LSR A                  \ Randomly set the C flag
    
     LDA #230               \ Set A to either +115 or -115 (230 >> 1) depending on
     ROR A                  \ the C flag, as this is a sign-magnitude number with
                            \ the C flag rotated into its sign bit
    
     STA Y1                 \ Set y_hi and Y1 to A, so the particle starts anywhere
     STA SY,Y               \ along either the top or bottom edge of the screen
    
     BNE STC6               \ Jump up to STC6 to draw this new particle (this BNE is
                            \ effectively a JMP as A will never be zero)
    

[X]

Subroutine [ADD](add.md) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Variable [ALP2](../workspace/zp.md#alp2) in workspace [ZP](../workspace/zp.md)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Variable [BET1](../workspace/zp.md#bet1) in workspace [ZP](../workspace/zp.md)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BET2](../workspace/zp.md#bet2) in workspace [ZP](../workspace/zp.md)

Bit 7 of BET2 = sign of the pitch angle in BETA

[X]

Variable [BETA](../workspace/zp.md#beta) in workspace [ZP](../workspace/zp.md)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Variable [DELT4](../workspace/zp.md#delt4) in workspace [ZP](../workspace/zp.md)

Our current speed * 64 as a 16-bit value

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [DV42](dv42.md) (category: Maths (Arithmetic))

Calculate (P R) = 256 * DELTA / z_hi

[X]

Label [KILL6](stars6.md#kill6) is local to this routine

[X]

Subroutine [MLS1](mls1.md) (category: Maths (Arithmetic))

Calculate (A P) = ALP1 * A

[X]

Subroutine [MLS2](mls2.md) (category: Maths (Arithmetic))

Calculate (S R) = XX(1 0) and (A P) = A * ALP1

[X]

Subroutine [MLU1](mlu1.md) (category: Maths (Arithmetic))

Calculate Y1 = y_hi and (A P) = |y_hi| * Q for Y-th stardust

[X]

Subroutine [MLU2](mlu2.md) (category: Maths (Arithmetic))

Calculate (A P) = |A| * Q

[X]

Entry point [MULTS-2](mls1.md#mults) in subroutine [MLS1](mls1.md) (category: Maths (Arithmetic))

Calculate (A P) = X * A

[X]

Subroutine [MUT1](mut1.md) (category: Maths (Arithmetic))

Calculate R = XX and (A P) = Q * A

[X]

Variable [NOSTM](../workspace/zp.md#nostm) in workspace [ZP](../workspace/zp.md)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PIX1](pix1.md) (category: Maths (Arithmetic))

Calculate (YY+1 SYL+Y) = (A P) + (S R) and draw stardust particle

[X]

Subroutine [PIXEL2](pixel2.md) (category: Drawing pixels)

Draw a stardust particle relative to the screen centre

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [ST3](stars6.md#st3) is local to this routine

[X]

Label [ST4](stars6.md#st4) is local to this routine

[X]

Label [STC6](stars6.md#stc6) is local to this routine

[X]

Label [STL6](stars6.md#stl6) is local to this routine

[X]

Variable [SX](../workspace/wp.md#sx) in workspace [WP](../workspace/wp.md)

This is where we store the x_hi coordinates for all the stardust particles

[X]

Variable [SXL](../workspace/wp.md#sxl) in workspace [WP](../workspace/wp.md)

This is where we store the x_lo coordinates for all the stardust particles

[X]

Variable [SY](../workspace/wp.md#sy) in workspace [WP](../workspace/wp.md)

This is where we store the y_hi coordinates for all the stardust particles

[X]

Variable [SYL](../workspace/wp.md#syl) in workspace [WP](../workspace/wp.md)

This is where we store the y_lo coordinates for all the stardust particles

[X]

Variable [SZ](../workspace/wp.md#sz) in workspace [WP](../workspace/wp.md)

This is where we store the z_hi coordinates for all the stardust particles

[X]

Variable [SZL](../workspace/wp.md#szl) in workspace [WP](../workspace/wp.md)

This is where we store the z_lo coordinates for all the stardust particles

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XX](../workspace/zp.md#xx) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit x-coordinate

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [YY](../workspace/zp.md#yy) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit y-coordinate

[X]

Variable [ZZ](../workspace/zp.md#zz) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for distance values

[STARS1](stars1.md "Previous routine")[MAS1](mas1.md "Next routine")
