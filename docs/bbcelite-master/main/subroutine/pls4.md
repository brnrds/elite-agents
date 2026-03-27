---
title: "The PLS4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pls4.html"
---

[PLS3](pls3.md "Previous routine")[PLS5](pls5.md "Next routine")
    
    
           Name: PLS4                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Calculate CNT2 = arctan(P / A) / 4
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-pls4)
     References: This subroutine is called as follows:
                 * [PL9 (Part 2 of 3)](pl9_part_2_of_3.md) calls PLS4
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       CNT2 = arctan(P / A) / 4
    
     and do the following if nosev_z_hi >= 0:
    
       CNT2 = CNT2 + 32
    
     which is the equivalent of adding 180 degrees to the result (or PI radians),
     as there are 64 segments in a full circle.
    
     This routine is called with the following arguments when calculating the
     equator and meridian for planets:
    
       * A = roofv_z_hi, P = -nosev_z_hi
    
       * A = sidev_z_hi, P = -nosev_z_hi
    
     So it calculates the angle between the planet's orientation vectors, in the
     z-axis.
    
    
    
    
    .PLS4
    
     STA Q                  \ Set Q = A
    
     JSR ARCTAN             \ Call ARCTAN to calculate:
                            \
                            \   A = arctan(P / Q)
                            \       arctan(P / A)
                            \
                            \ The result in A will be in the range 0 to 128, which
                            \ represents an angle of 0 to 180 degrees (or 0 to PI
                            \ radians)
    
     LDX INWK+14            \ If nosev_z_hi is negative, skip the following
     BMI P%+4               \ instruction to leave the angle in A as a positive
                            \ integer in the range 0 to 128 (so when we calculate
                            \ CNT2 below, it will be in the right half of the
                            \ anti-clockwise arc that we describe when drawing
                            \ circles, i.e. from 6 o'clock, through 3 o'clock and
                            \ on to 12 o'clock)
    
     EOR #%10000000         \ If we get here then nosev_z_hi is positive, so flip
                            \ bit 7 of the angle in A, which is the same as adding
                            \ 128 to give a result in the range 129 to 256 (i.e. 129
                            \ to 0), or 180 to 360 degrees (so when we calculate
                            \ CNT2 below, it will be in the left half of the
                            \ anti-clockwise arc that we describe when drawing
                            \ circles, i.e. from 12 o'clock, through 9 o'clock and
                            \ on to 6 o'clock)
    
     LSR A                  \ Set CNT2 = A / 4
     LSR A
     STA CNT2
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [ARCTAN](arctan.md) (category: Maths (Geometry))

Calculate A = arctan(P / Q)

[X]

Variable [CNT2](../workspace/zp.md#cnt2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[PLS3](pls3.md "Previous routine")[PLS5](pls5.md "Next routine")
