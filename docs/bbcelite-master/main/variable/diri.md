---
title: "The DIRI variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/diri.html"
---

[DELI](deli.md "Previous routine")[savosc](savosc.md "Next routine")
    
    
           Name: DIRI                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for changing directory on the Master Compact
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-diri)
     References: This variable is used as follows:
                 * [GTDIR](../subroutine/gtdir.md) uses DIRI
    
    
    
    
    
    
    IF _COMPACT
    
    .DIRI
    
     EQUS "DIR 12345678901234567890"
     EQUB 13
    
    ENDIF
    

[DELI](deli.md "Previous routine")[savosc](savosc.md "Next routine")
