---
title: "The DELI variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/deli.html"
---

[CTLI](ctli.md "Previous routine")[DIRI](diri.md "Next routine")
    
    
           Name: DELI                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for deleting a file
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-deli)
     Variations: See [code variations](../../related/compare/main/variable/deli.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DELT](../subroutine/delt.md) uses DELI
    
    
    
    
    
    
    .DELI
    
    IF _SNG47
    
     EQUS "DELETE :1.1234567"
     EQUB 13
    
    ELIF _COMPACT
    
     EQUS "DELETE 1234567890"
     EQUB 13
    
    ENDIF
    

[CTLI](ctli.md "Previous routine")[DIRI](diri.md "Next routine")
