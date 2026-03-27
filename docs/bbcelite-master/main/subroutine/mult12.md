---
title: "The MULT12 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mult12.html"
---

[MULT1](mult1.md "Previous routine")[TAS3](tas3.md "Next routine")
    
    
           Name: MULT12                                                  [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (S R) = Q * A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mult12)
     References: This subroutine is called as follows:
                 * [TAS3](tas3.md) calls MULT12
                 * [TAS4](tas4.md) calls MULT12
                 * [TIDY](tidy.md) calls MULT12
                 * [TIS3](tis3.md) calls MULT12
    
    
    
    
    
    * * *
    
    
     Calculate:
    
       (S R) = Q * A
    
    
    
    
    .MULT12
    
     JSR MULT1              \ Set (A P) = Q * A
    
     STA S                  \ Set (S R) = (A P)
     LDA P                  \           = Q * A
     STA R
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [MULT1](mult1.md) (category: Maths (Arithmetic))

Calculate (A P) = Q * A

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MULT1](mult1.md "Previous routine")[TAS3](tas3.md "Next routine")
