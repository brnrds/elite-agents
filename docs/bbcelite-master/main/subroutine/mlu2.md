---
title: "The MLU2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mlu2.html"
---

[MLU1](mlu1.md "Previous routine")[MULTU](multu.md "Next routine")
    
    
           Name: MLU2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = |A| * Q
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mlu2)
     References: This subroutine is called as follows:
                 * [STARS1](stars1.md) calls MLU2
                 * [STARS6](stars6.md) calls MLU2
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of a sign-magnitude 8-bit number P with an
     unsigned number Q:
    
       (A P) = |A| * Q
    
    
    
    
    .MLU2
    
     AND #%01111111         \ Clear the sign bit in P, so P = |A|
     STA P
    
                            \ Fall through into MULTU to calculate:
                            \
                            \   (A P) = P * Q
                            \         = |A| * Q
    

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MLU1](mlu1.md "Previous routine")[MULTU](multu.md "Next routine")
