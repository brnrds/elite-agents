---
title: "The cls subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/cls.html"
---

[DVID4K](dvid4k.md "Previous routine")[TT67K](tt67k.md "Next routine")
    
    
           Name: cls                                                     [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the top part of the screen and draw a border box
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-cls)
     References: This subroutine is called as follows:
                 * [CHPR](chpr.md) calls cls
    
    
    
    
    
    
    .cls
    
     JSR TTX66              \ Call TTX66 to clear the top part of the screen and
                            \ draw a border box
    
     JMP RR4                \ Jump to RR4 to restore X and Y from the stack and A
                            \ from K3, and return from the subroutine using a tail
                            \ call
    

[X]

Entry point [RR4](chpr.md#rr4) in subroutine [CHPR](chpr.md) (category: Text)

Restore the registers and return from the subroutine

[X]

Subroutine [TTX66](ttx66.md) (category: Drawing the screen)

Clear the top part of the screen and draw a border box

[DVID4K](dvid4k.md "Previous routine")[TT67K](tt67k.md "Next routine")
