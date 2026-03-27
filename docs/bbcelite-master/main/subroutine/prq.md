---
title: "The prq subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/prq.html"
---

[TT147](tt147.md "Previous routine")[TT151](tt151.md "Next routine")
    
    
           Name: prq                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token followed by a question mark
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-prq)
     References: This subroutine is called as follows:
                 * [eq](eq.md) calls prq
                 * [EQSHP](eqshp.md) calls prq
                 * [NWDAV4](nwdav4.md) calls prq
                 * [qv](qv.md) calls prq
                 * [TT219](tt219.md) calls prq
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    * * *
    
    
     Other entry points:
    
       prq+3                Print a question mark
    
    
    
    
    .prq
    
     JSR TT27               \ Print the text token in A
    
     LDA #'?'               \ Print a question mark and return from the
     JMP TT27               \ subroutine using a tail call
    

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[TT147](tt147.md "Previous routine")[TT151](tt151.md "Next routine")
