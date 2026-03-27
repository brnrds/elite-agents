---
title: "The mes9 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mes9.html"
---

[MESS](mess.md "Previous routine")[OUCH](ouch.md "Next routine")
    
    
           Name: mes9                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Print a text token, possibly followed by " DESTROYED"
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-mes9)
     Variations: See [code variations](../../related/compare/main/subroutine/mes9.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [me1](me1.md) calls mes9
    
    
    
    
    
    * * *
    
    
     Print a text token, followed by " DESTROYED" if the destruction flag is set
     (for when a piece of equipment is destroyed).
    
    
    
    
    .mes9
    
     JSR TT27               \ Call TT27 to print the text token in A
    
     LSR de                 \ If bit 0 of variable de is clear, return from the
     BCC out                \ subroutine (as out contains an RTS)
    
     LDA #253               \ Print recursive token 93 (" DESTROYED") and return
     JMP TT27               \ from the subroutine using a tail call
    

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[X]

Variable [de](../workspace/wp.md#de) in workspace [WP](../workspace/wp.md)

Equipment destruction flag

[X]

Entry point [out](tt217.md#out) in subroutine [TT217](tt217.md) (category: Keyboard)

Contains an RTS

[MESS](mess.md "Previous routine")[OUCH](ouch.md "Next routine")
