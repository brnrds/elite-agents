---
title: "The plf subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/plf.html"
---

[csh](csh.md "Previous routine")[TT68](tt68.md "Next routine")
    
    
           Name: plf                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token followed by a newline
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-plf)
     References: This subroutine is called as follows:
                 * [fwl](fwl.md) calls plf
                 * [plf2](plf2.md) calls plf
                 * [STATUS](status.md) calls plf
                 * [TITLE](title.md) calls plf
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .plf
    
     JSR TT27               \ Print the text token in A
    
     JMP TT67               \ Jump to TT67 to print a newline and return from the
                            \ subroutine using a tail call
    

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[X]

Subroutine [TT67](tt67.md) (category: Text)

Print a newline

[csh](csh.md "Previous routine")[TT68](tt68.md "Next routine")
