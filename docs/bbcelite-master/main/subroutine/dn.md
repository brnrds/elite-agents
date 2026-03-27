---
title: "The dn subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dn.html"
---

[EQSHP](eqshp.md "Previous routine")[dn2](dn2.md "Next routine")
    
    
           Name: dn                                                      [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print the amount of money we have left in the cash pot, then make
                 a short, high beep and delay for 1 second
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-dn)
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls dn
                 * [TT219](tt219.md) calls dn
    
    
    
    
    
    
    .dn
    
     JSR TT162              \ Print a space
    
     LDA #119               \ Print recursive token 119 ("CASH:{cash} CR{crlf}")
     JSR spc                \ followed by a space
    
                            \ Fall through into dn2 to make a beep and delay for
                            \ 1 second before returning from the subroutine
    

[X]

Subroutine [TT162](tt162.md) (category: Text)

Print a space

[X]

Subroutine [spc](spc.md) (category: Text)

Print a text token followed by a space

[EQSHP](eqshp.md "Previous routine")[dn2](dn2.md "Next routine")
