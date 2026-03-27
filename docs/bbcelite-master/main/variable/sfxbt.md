---
title: "The SFXBT variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/sfxbt.html"
---

[SFXPR](sfxpr.md "Previous routine")[SFXFQ](sfxfq.md "Next routine")
    
    
           Name: SFXBT                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound data block 2
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-sfxbt)
     References: This variable is used as follows:
                 * [NOISE](../subroutine/noise.md) uses SFXBT
    
    
    
    
    
    
    .SFXBT
    
     EQUB &40, &10, &01
     EQUB &FC, &F3, &19
     EQUB &F9, &7C, &F1
     EQUB &FA, &FE, &FE
    

[SFXPR](sfxpr.md "Previous routine")[SFXFQ](sfxfq.md "Next routine")
