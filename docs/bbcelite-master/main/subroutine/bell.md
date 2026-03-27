---
title: "The BELL subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bell.html"
---

[TT26](tt26.md "Previous routine")[ESCAPE](escape.md "Next routine")
    
    
           Name: BELL                                                    [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make a standard system beep
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-bell)
     Variations: See [code variations](../../related/compare/main/subroutine/bell.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DK4](dk4.md) calls BELL
                 * [DKS3](dks3.md) calls BELL
    
    
    
    
    
    * * *
    
    
     This is the standard system beep, as made by the ASCII 7 "BELL" control code.
    
    
    
    
    .BELL
    
     LDA #7                 \ Control code 7 makes a beep, so load this into A
    
     JMP CHPR               \ Call the CHPR print routine to actually make the sound
    

[X]

Subroutine [CHPR](chpr.md) (category: Text)

Print a character at the text cursor by poking into screen memory

[TT26](tt26.md "Previous routine")[ESCAPE](escape.md "Next routine")
