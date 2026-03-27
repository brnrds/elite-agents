---
title: "The TRIBMA variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/tribma.html"
---

[TRIBTA](tribta.md "Previous routine")[TT66](../subroutine/tt66.md "Next routine")
    
    
           Name: TRIBMA                                                  [Show more]
           Type: Variable
       Category: Missions
        Summary: A table for converting the number of Trumbles in the hold into a
                 sprite-enable flag to use with VIC register &15
    
    
        Context: See this variable [in context in the source code](../../all/elite_h.md#header-tribma)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    \.TRIBMA                \ These instructions are commented out in the original
    \                       \ source
    \EQUB 0
    \EQUB 4
    \EQUB &C
    \EQUB &1C
    \EQUB &3C
    \EQUB &7C
    \EQUB &FC
    \EQUB &FC
    

[TRIBTA](tribta.md "Previous routine")[TT66](../subroutine/tt66.md "Next routine")
