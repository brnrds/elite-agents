---
title: "The MESS2 (Loader) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/variable/mess2.html"
---

[MESS1 (Loader)](mess1.md "Previous routine")[MESS3 (Loader)](mess3.md "Next routine")
    
    
           Name: MESS2                                                   [Show more]
           Type: Variable
       Category: Loader
        Summary: The OS command string for loading the main game code binary
    
    
        Context: See this variable [in context in the source code](../../all/loader.md#header-mess2)
     References: This variable is used as follows:
                 * [Elite loader](../subroutine/elite_loader.md) uses MESS2
    
    
    
    
    
    
    .MESS2
    
    IF _SNG47
    
     EQUS "L.BCODE FFFF1300"    \ This is short for "*LOAD BCODE FFFF1300"
     EQUB 13
    
    ELIF _COMPACT
    
     EQUS "L.ELITE FFFF1300"    \ This is short for "*LOAD ELITE FFFF1300"
     EQUB 13
    
    ENDIF
    

[MESS1 (Loader)](mess1.md "Previous routine")[MESS3 (Loader)](mess3.md "Next routine")
