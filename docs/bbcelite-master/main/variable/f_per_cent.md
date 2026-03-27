---
title: "The F% variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/f_per_cent.html"
---

[NMIpissoff](../subroutine/nmipissoff.md "Previous routine")[Dashboard image (Game data)](../../game_data/variable/dashboard_image.md "Next routine")
    
    
           Name: F%                                                      [Show more]
           Type: Variable
       Category: Utility routines
        Summary: Denotes the end of the main game code, from ELITE A to ELITE H
    
    
        Context: See this variable [in context in the source code](../../all/elite_h.md#header-f-per-cent)
     Variations: See [code variations](../../related/compare/main/variable/f_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DEEOR](../subroutine/deeor.md) uses F%
    
    
    
    
    
    
    .F%
    
     SKIP 0
    
    IF _COMPACT
    
     EQUB &F8, &F8          \ These bytes appear to be unused
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8
    
    ENDIF
    

[NMIpissoff](../subroutine/nmipissoff.md "Previous routine")[Dashboard image (Game data)](../../game_data/variable/dashboard_image.md "Next routine")
