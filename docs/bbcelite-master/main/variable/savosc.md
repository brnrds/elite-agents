---
title: "The savosc variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/savosc.html"
---

[DIRI](diri.md "Previous routine")[lodosc](lodosc.md "Next routine")
    
    
           Name: savosc                                                  [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for saving a commander file
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-savosc)
     References: This variable is used as follows:
                 * [SVE](../subroutine/sve.md) uses savosc
                 * [wfile](../subroutine/wfile.md) uses savosc
    
    
    
    
    
    
    .savosc
    
    IF _SNG47
    
     EQUS "SAVE :1.E.JAMESON  E7E +100 0 0"
     EQUB 13
    
    ELIF _COMPACT
    
     EQUS "SAVE JAMESON  E7E +100 0 0"
     EQUB 13
    
    ENDIF
    

[DIRI](diri.md "Previous routine")[lodosc](lodosc.md "Next routine")
