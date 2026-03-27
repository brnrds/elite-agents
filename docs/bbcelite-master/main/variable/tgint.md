---
title: "The TGINT variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/tgint.html"
---

[UP](../workspace/up.md "Previous routine")[S%](../subroutine/s_per_cent.md "Next routine")
    
    
           Name: TGINT                                                   [Show more]
           Type: Variable
       Category: Keyboard
        Summary: The keys used to toggle configuration settings when the game is
                 paused
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-tgint)
     References: This variable is used as follows:
                 * [DKS3](../subroutine/dks3.md) uses TGINT
    
    
    
    
    
    
    .TGINT
    
     EQUB 1                 \ The configuration keys in the same order as their
     EQUS "AXFYJKUT"        \ configuration bytes (starting from DAMP). The 1 is
                            \ for CAPS LOCK, and although "U" and "T" still toggle
                            \ the relevant configuration bytes, those values are not
                            \ used, so those keys have no effect
    

[UP](../workspace/up.md "Previous routine")[S%](../subroutine/s_per_cent.md "Next routine")
