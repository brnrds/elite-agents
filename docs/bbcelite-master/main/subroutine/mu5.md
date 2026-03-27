---
title: "The MU5 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mu5.html"
---

[STARS2](stars2.md "Previous routine")[MULT3](mult3.md "Next routine")
    
    
           Name: MU5                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Set K(3 2 1 0) = (A A A A) and clear the C flag
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mu5)
     References: This subroutine is called as follows:
                 * [MULT3](mult3.md) calls MU5
    
    
    
    
    
    * * *
    
    
     In practice this is only called via a BEQ following an AND instruction, in
     which case A = 0, so this routine effectively does this:
    
       K(3 2 1 0) = 0
    
    
    
    
    .MU5
    
     STA K                  \ Set K(3 2 1 0) to (A A A A)
     STA K+1
     STA K+2
     STA K+3
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[STARS2](stars2.md "Previous routine")[MULT3](mult3.md "Next routine")
