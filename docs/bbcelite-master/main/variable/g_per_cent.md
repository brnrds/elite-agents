---
title: "The G% variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/g_per_cent.html"
---

[DEEORS](../subroutine/deeors.md "Previous routine")[DOENTRY](../subroutine/doentry.md "Next routine")
    
    
           Name: G%                                                      [Show more]
           Type: Variable
       Category: Utility routines
        Summary: Denotes the start of the main game code, from ELITE A to ELITE H
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-g-per-cent)
     References: This variable is used as follows:
                 * [DEEOR](../subroutine/deeor.md) uses G%
    
    
    
    
    
    
    .G%
    
     SKIP 0                 \ The game code is scrambled from here to F% (or, as the
                            \ original source code puts it, "mutilated")
    

[DEEORS](../subroutine/deeors.md "Previous routine")[DOENTRY](../subroutine/doentry.md "Next routine")
