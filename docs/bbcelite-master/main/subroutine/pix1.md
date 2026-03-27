---
title: "The PIX1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pix1.html"
---

[orange](../variable/orange.md "Previous routine")[PIXEL2](pixel2.md "Next routine")
    
    
           Name: PIX1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (YY+1 SYL+Y) = (A P) + (S R) and draw stardust particle
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-pix1)
     Variations: See [code variations](../../related/compare/main/subroutine/pix1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STARS1](stars1.md) calls PIX1
                 * [STARS2](stars2.md) calls PIX1
                 * [STARS6](stars6.md) calls PIX1
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       (YY+1 SYL+Y) = (A P) + (S R)
    
     and draw a stardust particle at (X1,Y1) with distance ZZ.
    
    
    
    * * *
    
    
     Arguments:
    
       (A P)                A is the angle ALPHA or BETA, P is always 0
    
       (S R)                YY(1 0) or YY(1 0) + Q * A
    
       Y                    Stardust particle number
    
       X1                   The x-coordinate offset
    
       Y1                   The y-coordinate offset
    
       ZZ                   The distance of the point, with bigger distances drawing
                            smaller points:
    
                              * ZZ < 80           Double-height four-pixel square
    
                              * 80 <= ZZ <= 143   Single-height two-pixel dash
    
                              * ZZ > 143          Single-height one-pixel dot
    
    
    
    
    .PIX1
    
     JSR ADDK               \ Set (A X) = (A P) + (S R)
    
     STA YY+1               \ Set YY+1 to A, the high byte of the result
    
     TXA                    \ Set SYL+Y to X, the low byte of the result
     STA SYL,Y
    
                            \ Fall through into PIXEL2 to draw the stardust particle
                            \ at (X1,Y1)
    

[X]

Subroutine [ADDK](addk.md) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Variable [SYL](../workspace/wp.md#syl) in workspace [WP](../workspace/wp.md)

This is where we store the y_lo coordinates for all the stardust particles

[X]

Variable [YY](../workspace/zp.md#yy) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit y-coordinate

[orange](../variable/orange.md "Previous routine")[PIXEL2](pixel2.md "Next routine")
