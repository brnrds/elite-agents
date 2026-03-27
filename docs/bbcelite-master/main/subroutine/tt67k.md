---
title: "The TT67K subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tt67k.html"
---

[cls](cls.md "Previous routine")[CHPR](chpr.md "Next routine")
    
    
           Name: TT67K                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a newline
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-tt67k)
     Variations: See [code variations](../../related/compare/main/subroutine/tt67-tt67k.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CLYNS](clyns.md) calls TT67K
                 * [STATUS](status.md) calls TT67K
    
    
    
    
    
    
    .TT67K
    
                            \ This does the same as the existing TT67 routine, which
                            \ is also present in this source, so it isn't clear why
                            \ this duplicate exists
                            \
                            \ In the original source this label is TT67, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to TT67K
    
     LDA #12                \ Set A to a carriage return character
    
                            \ Fall through into TT26 to print the newline
    

[cls](cls.md "Previous routine")[CHPR](chpr.md "Next routine")
