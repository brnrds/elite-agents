---
title: "The MU1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mu1.html"
---

[SQUA2](squa2.md "Previous routine")[MLU1](mlu1.md "Next routine")
    
    
           Name: MU1                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Copy X into P and A, and clear the C flag
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mu1)
     References: This subroutine is called as follows:
                 * [MULTU](multu.md) calls MU1
    
    
    
    
    
    * * *
    
    
     Used to return a 0 result quickly from MULTU below.
    
    
    
    
    .MU1
    
     CLC                    \ Clear the C flag
    
     STX P                  \ Copy X into P and A
     TXA
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[SQUA2](squa2.md "Previous routine")[MLU1](mlu1.md "Next routine")
