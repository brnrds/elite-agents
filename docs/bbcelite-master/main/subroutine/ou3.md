---
title: "The ou3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ou3.html"
---

[ou2](ou2.md "Previous routine")[ITEM](../macro/item.md "Next routine")
    
    
           Name: ou3                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Display "FUEL SCOOPS DESTROYED" as an in-flight message
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-ou3)
     Variations: See [code variations](../../related/compare/main/subroutine/ou3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [OUCH](ouch.md) calls ou3
    
    
    
    
    
    
    .ou3
    
     LDA #111               \ Set A to recursive token 111 ("FUEL SCOOPS")
    
     JMP MESS               \ Print recursive token A as an in-flight message,
                            \ followed by " DESTROYED", and return from the
                            \ subroutine using a tail call
    

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[ou2](ou2.md "Previous routine")[ITEM](../macro/item.md "Next routine")
