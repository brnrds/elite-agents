---
title: "The ou2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ou2.html"
---

[OUCH](ouch.md "Previous routine")[ou3](ou3.md "Next routine")
    
    
           Name: ou2                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Display "E.C.M.SYSTEM DESTROYED" as an in-flight message
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-ou2)
     Variations: See [code variations](../../related/compare/main/subroutine/ou2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [OUCH](ouch.md) calls ou2
    
    
    
    
    
    
    .ou2
    
     LDA #108               \ Set A to recursive token 108 ("E.C.M.SYSTEM")
    
     JMP MESS               \ Print recursive token A as an in-flight message,
                            \ followed by " DESTROYED", and return from the
                            \ subroutine using a tail call
    

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[OUCH](ouch.md "Previous routine")[ou3](ou3.md "Next routine")
