---
title: "The lodosc variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/lodosc.html"
---

[savosc](savosc.md "Previous routine")[wfile](../subroutine/wfile.md "Next routine")
    
    
           Name: lodosc                                                  [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for loading a commander file
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-lodosc)
     References: This variable is used as follows:
                 * [rfile](../subroutine/rfile.md) uses lodosc
                 * [SVE](../subroutine/sve.md) uses lodosc
    
    
    
    
    
    
    .lodosc
    
    IF _SNG47
    
     EQUS "LOAD :1.E.JAMESON  E7E"
     EQUB 13
    
    ELIF _COMPACT
    
     EQUS "LOAD JAMESON  E7E"
     EQUB 13
    
    ENDIF
    

[savosc](savosc.md "Previous routine")[wfile](../subroutine/wfile.md "Next routine")
