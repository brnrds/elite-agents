---
title: "The SFXFQ variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/sfxfq.html"
---

[SFXBT](sfxbt.md "Previous routine")[SFXVC](sfxvc.md "Next routine")
    
    
           Name: SFXFQ                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound data block 3
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-sfxfq)
     References: This variable is used as follows:
                 * [NOISE](../subroutine/noise.md) uses SFXFQ
    
    
    
    
    
    
    .SFXFQ
    
     EQUB &F0, &20, &10
     EQUB &30, &03, &01
     EQUB &08, &80, &16
     EQUB &38, &00, &80
    

[SFXBT](sfxbt.md "Previous routine")[SFXVC](sfxvc.md "Next routine")
