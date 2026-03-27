---
title: "The SFXPR variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/sfxpr.html"
---

[Sound variables](../workspace/sound_variables.md "Previous routine")[SFXBT](sfxbt.md "Next routine")
    
    
           Name: SFXPR                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound data block 1
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-sfxpr)
     References: This variable is used as follows:
                 * [NOISE](../subroutine/noise.md) uses SFXPR
    
    
    
    
    
    
    .SFXPR
    
     EQUB &4B, &5B, &3F
     EQUB &EB, &FF, &09
     EQUB &FF, &8B, &CF
     EQUB &E7, &FF, &EF
    

[Sound variables](../workspace/sound_variables.md "Previous routine")[SFXBT](sfxbt.md "Next routine")
