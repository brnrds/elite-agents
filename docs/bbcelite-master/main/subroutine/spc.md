---
title: "The spc subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/spc.html"
---

[TT70](tt70.md "Previous routine")[TT25](tt25.md "Next routine")
    
    
           Name: spc                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token followed by a space
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-spc)
     References: This subroutine is called as follows:
                 * [dn](dn.md) calls spc
                 * [EQSHP](eqshp.md) calls spc
                 * [qv](qv.md) calls spc
                 * [STATUS](status.md) calls spc
                 * [TT25](tt25.md) calls spc
    
    
    
    
    
    * * *
    
    
     Print a text token (i.e. a character, control code, two-letter token or
     recursive token) followed by a space.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .spc
    
     JSR TT27               \ Print the text token in A
    
     JMP TT162              \ Print a space and return from the subroutine using a
                            \ tail call
    

[X]

Subroutine [TT162](tt162.md) (category: Text)

Print a space

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[TT70](tt70.md "Previous routine")[TT25](tt25.md "Next routine")
