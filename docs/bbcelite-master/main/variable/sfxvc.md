---
title: "The SFXVC variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/sfxvc.html"
---

[SFXFQ](sfxfq.md "Previous routine")[SETINTS](../subroutine/setints.md "Next routine")
    
    
           Name: SFXVC                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound data block 4
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-sfxvc)
     References: This variable is used as follows:
                 * [NOISE](../subroutine/noise.md) uses SFXVC
    
    
    
    
    
    
    .SFXVC
    
     EQUB &FF, &FF, &00
     EQUB &03, &1F, &01
     EQUB &07, &07, &0F
     EQUB &03, &0F, &0F
    

[SFXFQ](sfxfq.md "Previous routine")[SETINTS](../subroutine/setints.md "Next routine")
