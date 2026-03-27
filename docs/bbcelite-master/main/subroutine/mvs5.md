---
title: "The MVS5 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mvs5.html"
---

[MVT3](mvt3.md "Previous routine")[TENS](../variable/tens.md "Next routine")
    
    
           Name: MVS5                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Apply a 3.6 degree pitch or roll to an orientation vector
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
                 [Pitching and rolling by a fixed angle](https://elite.bbcelite.com/deep_dives/pitching_and_rolling_by_a_fixed_angle.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-mvs5)
     References: This subroutine is called as follows:
                 * [HAS1](has1.md) calls MVS5
                 * [MVEIT (Part 8 of 9)](mveit_part_8_of_9.md) calls MVS5
    
    
    
    
    
    * * *
    
    
     Pitch or roll a ship by a small, fixed amount (1/16 radians, or 3.6 degrees),
     in a specified direction, by rotating the orientation vectors. The vectors to
     rotate are given in X and Y, and the direction of the rotation is given in
     RAT2. The calculation is as follows:
    
       * If the direction is positive:
    
         X = X * (1 - 1/512) + Y / 16
         Y = Y * (1 - 1/512) - X / 16
    
       * If the direction is negative:
    
         X = X * (1 - 1/512) - Y / 16
         Y = Y * (1 - 1/512) + X / 16
    
     So if X = 15 (roofv_x), Y = 21 (sidev_x) and RAT2 is positive, it does this:
    
       roofv_x = roofv_x * (1 - 1/512)  + sidev_x / 16
       sidev_x = sidev_x * (1 - 1/512)  - roofv_x / 16
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The first vector to rotate:
    
                              * If X = 15, rotate roofv_x
    
                              * If X = 17, rotate roofv_y
    
                              * If X = 19, rotate roofv_z
    
                              * If X = 21, rotate sidev_x
    
                              * If X = 23, rotate sidev_y
    
                              * If X = 25, rotate sidev_z
    
       Y                    The second vector to rotate:
    
                              * If Y = 9,  rotate nosev_x
    
                              * If Y = 11, rotate nosev_y
    
                              * If Y = 13, rotate nosev_z
    
                              * If Y = 21, rotate sidev_x
    
                              * If Y = 23, rotate sidev_y
    
                              * If Y = 25, rotate sidev_z
    
       RAT2                 The direction of the pitch or roll to perform, positive
                            or negative (i.e. the sign of the roll or pitch counter
                            in bit 7)
    
    
    
    
    .MVS5
    
     LDA INWK+1,X           \ Fetch roofv_x_hi, clear the sign bit, divide by 2 and
     AND #%01111111         \ store in T, so:
     LSR A                  \
     STA T                  \ T = |roofv_x_hi| / 2
                            \   = |roofv_x| / 512
                            \
                            \ The above is true because:
                            \
                            \ |roofv_x| = |roofv_x_hi| * 256 + roofv_x_lo
                            \
                            \ so:
                            \
                            \ |roofv_x| / 512 = |roofv_x_hi| * 256 / 512
                            \                    + roofv_x_lo / 512
                            \                  = |roofv_x_hi| / 2
    
     LDA INWK,X             \ Now we do the following subtraction:
     SEC                    \
     SBC T                  \ (S R) = (roofv_x_hi roofv_x_lo) - |roofv_x| / 512
     STA R                  \       = (1 - 1/512) * roofv_x
                            \
                            \ by doing the low bytes first
    
     LDA INWK+1,X           \ And then the high bytes (the high byte of the right
     SBC #0                 \ side of the subtraction being 0)
     STA S
    
     LDA INWK,Y             \ Set P = nosev_x_lo
     STA P
    
     LDA INWK+1,Y           \ Fetch the sign of nosev_x_hi (bit 7) and store in T
     AND #%10000000
     STA T
    
     LDA INWK+1,Y           \ Fetch nosev_x_hi into A and clear the sign bit, so
     AND #%01111111         \ A = |nosev_x_hi|
    
     LSR A                  \ Set (A P) = (A P) / 16
     ROR P                  \           = |nosev_x_hi nosev_x_lo| / 16
     LSR A                  \           = |nosev_x| / 16
     ROR P
     LSR A
     ROR P
     LSR A
     ROR P
    
     ORA T                  \ Set the sign of A to the sign in T (i.e. the sign of
                            \ the original nosev_x), so now:
                            \
                            \ (A P) = nosev_x / 16
    
     EOR RAT2               \ Give it the sign as if we multiplied by the direction
                            \ by the pitch or roll direction
    
     STX Q                  \ Store the value of X so it can be restored after the
                            \ call to ADD
    
     JSR ADD                \ (A X) = (A P) + (S R)
                            \       = +/-nosev_x / 16 + (1 - 1/512) * roofv_x
    
     STA K+1                \ Set K(1 0) = (1 - 1/512) * roofv_x +/- nosev_x / 16
     STX K
    
     LDX Q                  \ Restore the value of X from before the call to ADD
    
     LDA INWK+1,Y           \ Fetch nosev_x_hi, clear the sign bit, divide by 2 and
     AND #%01111111         \ store in T, so:
     LSR A                  \
     STA T                  \ T = |nosev_x_hi| / 2
                            \   = |nosev_x| / 512
    
     LDA INWK,Y             \ Now we do the following subtraction:
     SEC                    \
     SBC T                  \ (S R) = (nosev_x_hi nosev_x_lo) - |nosev_x| / 512
     STA R                  \       = (1 - 1/512) * nosev_x
                            \
                            \ by doing the low bytes first
    
     LDA INWK+1,Y           \ And then the high bytes (the high byte of the right
     SBC #0                 \ side of the subtraction being 0)
     STA S
    
     LDA INWK,X             \ Set P = roofv_x_lo
     STA P
    
     LDA INWK+1,X           \ Fetch the sign of roofv_x_hi (bit 7) and store in T
     AND #%10000000
     STA T
    
     LDA INWK+1,X           \ Fetch roofv_x_hi into A and clear the sign bit, so
     AND #%01111111         \ A = |roofv_x_hi|
    
     LSR A                  \ Set (A P) = (A P) / 16
     ROR P                  \           = |roofv_x_hi roofv_x_lo| / 16
     LSR A                  \           = |roofv_x| / 16
     ROR P
     LSR A
     ROR P
     LSR A
     ROR P
    
     ORA T                  \ Set the sign of A to the opposite sign to T (i.e. the
     EOR #%10000000         \ sign of the original -roofv_x), so now:
                            \
                            \ (A P) = -roofv_x / 16
    
     EOR RAT2               \ Give it the sign as if we multiplied by the direction
                            \ by the pitch or roll direction
    
     STX Q                  \ Store the value of X so it can be restored after the
                            \ call to ADD
    
     JSR ADD                \ (A X) = (A P) + (S R)
                            \       = -/+roofv_x / 16 + (1 - 1/512) * nosev_x
    
     STA INWK+1,Y           \ Set nosev_x = (1-1/512) * nosev_x -/+ roofv_x / 16
     STX INWK,Y
    
     LDX Q                  \ Restore the value of X from before the call to ADD
    
     LDA K                  \ Set roofv_x = K(1 0)
     STA INWK,X             \             = (1-1/512) * roofv_x +/- nosev_x / 16
     LDA K+1
     STA INWK+1,X
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [ADD](add.md) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

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

Variable [RAT2](../workspace/zp.md#rat2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the pitch and roll signs when moving objects and stardust

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MVT3](mvt3.md "Previous routine")[TENS](../variable/tens.md "Next routine")
