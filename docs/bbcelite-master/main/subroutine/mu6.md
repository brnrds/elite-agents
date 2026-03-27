---
title: "The MU6 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mu6.html"
---

[MLS1](mls1.md "Previous routine")[SQUA](squa.md "Next routine")
    
    
           Name: MU6                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Set P(1 0) = (A A)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mu6)
     References: This subroutine is called as follows:
                 * [MLS1](mls1.md) calls MU6
    
    
    
    
    
    * * *
    
    
     In practice this is only called via a BEQ following an AND instruction, in
     which case A = 0, so this routine effectively does this:
    
       P(1 0) = 0
    
    
    
    
    .MU6
    
     STA P+1                \ Set P(1 0) = (A A)
     STA P
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MLS1](mls1.md "Previous routine")[SQUA](squa.md "Next routine")
