---
title: "The TT16a subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tt16a.html"
---

[TT161](tt161.md "Previous routine")[TT163](tt163.md "Next routine")
    
    
           Name: TT16a                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print "g" (for grams)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-tt16a)
     References: This subroutine is called as follows:
                 * [TT152](tt152.md) calls TT16a
    
    
    
    
    
    
    .TT16a
    
     LDA #'g'               \ Load a "g" character into A
    
     JMP TT26               \ Print the character, using TT216 so that it doesn't
                            \ change the character case, and return from the
                            \ subroutine using a tail call
    

[X]

Subroutine [TT26](tt26.md) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[TT161](tt161.md "Previous routine")[TT163](tt163.md "Next routine")
