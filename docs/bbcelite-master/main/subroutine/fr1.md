---
title: "The FR1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/fr1.html"
---

[ANGRY](angry.md "Previous routine")[SESCP](sescp.md "Next routine")
    
    
           Name: FR1                                                     [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Display the "missile jammed" message
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-fr1)
     References: This subroutine is called as follows:
                 * [FRMIS](frmis.md) calls FR1
    
    
    
    
    
    * * *
    
    
     This is shown if there isn't room in the local bubble of universe for a new
     missile.
    
    
    
    * * *
    
    
     Other entry points:
    
       FR1-2                Clear the C flag and return from the subroutine
    
    
    
    
    .FR1
    
     LDA #201               \ Print recursive token 41 ("MISSILE JAMMED") as an
     JMP MESS               \ in-flight message and return from the subroutine using
                            \ a tail call
    

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[ANGRY](angry.md "Previous routine")[SESCP](sescp.md "Next routine")
