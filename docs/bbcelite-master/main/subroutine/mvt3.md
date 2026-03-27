---
title: "The MVT3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mvt3.html"
---

[plf2](plf2.md "Previous routine")[MVS5](mvs5.md "Next routine")
    
    
           Name: MVT3                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-mvt3)
     References: This subroutine is called as follows:
                 * [MAS1](mas1.md) calls MVT3
                 * [MV40](mv40.md) calls MVT3
                 * [TAS1](tas1.md) calls MVT3
    
    
    
    
    
    * * *
    
    
     Add an INWK position coordinate - i.e. x, y or z - to K(3 2 1), like this:
    
       K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)
    
     The INWK coordinate to add to K(3 2 1) is specified by X.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The coordinate to add to K(3 2 1), as follows:
    
                              * If X = 0, add (x_sign x_hi x_lo)
    
                              * If X = 3, add (y_sign y_hi y_lo)
    
                              * If X = 6, add (z_sign z_hi z_lo)
    
    
    
    * * *
    
    
     Returns:
    
       A                    Contains a copy of the high byte of the result, K+3
    
       X                    X is preserved
    
    
    
    
    .MVT3
    
     LDA K+3                \ Set S = K+3
     STA S
    
     AND #%10000000         \ Set T = sign bit of K(3 2 1)
     STA T
    
     EOR INWK+2,X           \ If x_sign has a different sign to K(3 2 1), jump to
     BMI MV13               \ MV13 to process the addition as a subtraction
    
     LDA K+1                \ Set K(3 2 1) = K(3 2 1) + (x_sign x_hi x_lo)
     CLC                    \ starting with the low bytes
     ADC INWK,X
     STA K+1
    
     LDA K+2                \ Then the middle bytes
     ADC INWK+1,X
     STA K+2
    
     LDA K+3                \ And finally the high bytes
     ADC INWK+2,X
    
     AND #%01111111         \ Setting the sign bit of K+3 to T, the original sign
     ORA T                  \ of K(3 2 1)
     STA K+3
    
     RTS                    \ Return from the subroutine
    
    .MV13
    
     LDA S                  \ Set S = |K+3| (i.e. K+3 with the sign bit cleared)
     AND #%01111111
     STA S
    
     LDA INWK,X             \ Set K(3 2 1) = (x_sign x_hi x_lo) - K(3 2 1)
     SEC                    \ starting with the low bytes
     SBC K+1
     STA K+1
    
     LDA INWK+1,X           \ Then the middle bytes
     SBC K+2
     STA K+2
    
     LDA INWK+2,X           \ And finally the high bytes, doing A = |x_sign| - |K+3|
     AND #%01111111         \ and setting the C flag for testing below
     SBC S
    
     ORA #%10000000         \ Set the sign bit of K+3 to the opposite sign of T,
     EOR T                  \ i.e. the opposite sign to the original K(3 2 1)
     STA K+3
    
     BCS MV14               \ If the C flag is set, i.e. |x_sign| >= |K+3|, then
                            \ the sign of K(3 2 1). In this case, we want the
                            \ result to have the same sign as the largest argument,
                            \ which is (x_sign x_hi x_lo), which we know has the
                            \ opposite sign to K(3 2 1), and that's what we just set
                            \ the sign of K(3 2 1) to... so we can jump to MV14 to
                            \ return from the subroutine
    
     LDA #1                 \ We need to swap the sign of the result in K(3 2 1),
     SBC K+1                \ which we do by calculating 0 - K(3 2 1), which we can
     STA K+1                \ do with 1 - C - K(3 2 1), as we know the C flag is
                            \ clear. We start with the low bytes
    
     LDA #0                 \ Then the middle bytes
     SBC K+2
     STA K+2
    
     LDA #0                 \ And finally the high bytes
     SBC K+3
    
     AND #%01111111         \ Set the sign bit of K+3 to the same sign as T,
     ORA T                  \ i.e. the same sign as the original K(3 2 1), as
     STA K+3                \ that's the largest argument
    
    .MV14
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [MV13](mvt3.md#mv13) is local to this routine

[X]

Label [MV14](mvt3.md#mv14) is local to this routine

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[plf2](plf2.md "Previous routine")[MVS5](mvs5.md "Next routine")
