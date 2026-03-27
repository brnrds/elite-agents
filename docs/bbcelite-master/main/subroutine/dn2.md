---
title: "The dn2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dn2.html"
---

[dn](dn.md "Previous routine")[eq](eq.md "Next routine")
    
    
           Name: dn2                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Make a short, high beep and delay for 0.5 seconds
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-dn2)
     Variations: See [code variations](../../related/compare/main/subroutine/dn2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls dn2
                 * [NWDAV4](nwdav4.md) calls dn2
                 * [TT210](tt210.md) calls dn2
                 * [TT219](tt219.md) calls dn2
    
    
    
    
    
    
    .dn2
    
     JSR BEEP               \ Call the BEEP subroutine to make a short, high beep
    
     LDY #25                \ Wait for 25/50 of a second (0.5 second) and return
     JMP DELAY              \ from the subroutine using a tail call
    

[X]

Subroutine [BEEP](beep.md) (category: Sound)

Make a short, high beep

[X]

Subroutine [DELAY](delay.md) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[dn](dn.md "Previous routine")[eq](eq.md "Next routine")
