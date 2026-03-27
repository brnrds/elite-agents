---
title: "The MULTU subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/multu.html"
---

[MLU2](mlu2.md "Previous routine")[MU11](mu11.md "Next routine")
    
    
           Name: MULTU                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = P * Q
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-multu)
     References: This subroutine is called as follows:
                 * [GCASH](gcash.md) calls MULTU
                 * [PLS3](pls3.md) calls MULTU
                 * [TT24](tt24.md) calls MULTU
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of unsigned 8-bit numbers:
    
       (A P) = P * Q
    
    
    
    
    .MULTU
    
     LDX Q                  \ Set X = Q
    
     BEQ MU1                \ If X = Q = 0, jump to MU1 to copy X into P and A,
                            \ clear the C flag and return from the subroutine using
                            \ a tail call
    
                            \ Otherwise fall through into MU11 to set (A P) = P * X
    

[X]

Subroutine [MU1](mu1.md) (category: Maths (Arithmetic))

Copy X into P and A, and clear the C flag

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MLU2](mlu2.md "Previous routine")[MU11](mu11.md "Next routine")
