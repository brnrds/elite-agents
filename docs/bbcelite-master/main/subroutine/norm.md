---
title: "The NORM subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/norm.html"
---

[TAS2](tas2.md "Previous routine")[WARP](warp.md "Next routine")
    
    
           Name: NORM                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Normalise the three-coordinate vector in XX15
      Deep dive: [Tidying orthonormal vectors](https://elite.bbcelite.com/deep_dives/tidying_orthonormal_vectors.html)
                 [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-norm)
     References: This subroutine is called as follows:
                 * [TIDY](tidy.md) calls NORM
    
    
    
    
    
    * * *
    
    
     We do this by dividing each of the three coordinates by the length of the
     vector, which we can calculate using Pythagoras. Once normalised, 96 (&60) is
     used to represent a value of 1, and 96 with bit 7 set (&E0) is used to
     represent -1. This enables us to represent fractional values of less than 1
     using integers.
    
    
    
    * * *
    
    
     Arguments:
    
       XX15                 The vector to normalise, with:
    
                              * The x-coordinate in XX15
    
                              * The y-coordinate in XX15+1
    
                              * The z-coordinate in XX15+2
    
    
    
    * * *
    
    
     Returns:
    
       XX15                 The normalised vector
    
       Q                    The length of the original XX15 vector
    
    
    
    * * *
    
    
     Other entry points:
    
       NO1                  Contains an RTS
    
    
    
    
    .NORM
    
     LDA XX15               \ Fetch the x-coordinate into A
    
     JSR SQUA               \ Set (A P) = A * A = x^2
    
     STA R                  \ Set (R Q) = (A P) = x^2
     LDA P
     STA Q
    
     LDA XX15+1             \ Fetch the y-coordinate into A
    
     JSR SQUA               \ Set (A P) = A * A = y^2
    
     STA T                  \ Set (T P) = (A P) = y^2
    
     LDA P                  \ Set (R Q) = (R Q) + (T P) = x^2 + y^2
     ADC Q                  \
     STA Q                  \ First, doing the low bytes, Q = Q + P
    
     LDA T                  \ And then the high bytes, R = R + T
     ADC R
     STA R
    
     LDA XX15+2             \ Fetch the z-coordinate into A
    
     JSR SQUA               \ Set (A P) = A * A = z^2
    
     STA T                  \ Set (T P) = (A P) = z^2
    
     LDA P                  \ Set (R Q) = (R Q) + (T P) = x^2 + y^2 + z^2
     ADC Q                  \
     STA Q                  \ First, doing the low bytes, Q = Q + P
    
     LDA T                  \ And then the high bytes, R = R + T
     ADC R
     STA R
    
     JSR LL5                \ We now have the following:
                            \
                            \ (R Q) = x^2 + y^2 + z^2
                            \
                            \ so we can call LL5 to use Pythagoras to get:
                            \
                            \ Q = SQRT(R Q)
                            \   = SQRT(x^2 + y^2 + z^2)
                            \
                            \ So Q now contains the length of the vector (x, y, z),
                            \ and we can normalise the vector by dividing each of
                            \ the coordinates by this value, which we do by calling
                            \ routine TIS2. TIS2 returns the divided figure, using
                            \ 96 to represent 1 and 96 with bit 7 set for -1
    
     LDA XX15               \ Call TIS2 to divide the x-coordinate in XX15 by Q,
     JSR TIS2               \ with 1 being represented by 96
     STA XX15
    
     LDA XX15+1             \ Call TIS2 to divide the y-coordinate in XX15+1 by Q,
     JSR TIS2               \ with 1 being represented by 96
     STA XX15+1
    
     LDA XX15+2             \ Call TIS2 to divide the z-coordinate in XX15+2 by Q,
     JSR TIS2               \ with 1 being represented by 96
     STA XX15+2
    
    .NO1
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [LL5](ll5.md) (category: Maths (Arithmetic))

Calculate Q = SQRT(R Q)

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

Subroutine [SQUA](squa.md) (category: Maths (Arithmetic))

Clear bit 7 of A and calculate (A P) = A * A

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [TIS2](tis2.md) (category: Maths (Arithmetic))

Calculate A = A / Q

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[TAS2](tas2.md "Previous routine")[WARP](warp.md "Next routine")
