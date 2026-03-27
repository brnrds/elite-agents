---
title: "The MESS1 (Loader) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/variable/mess1.html"
---

[OSB (Loader)](../subroutine/osb.md "Previous routine")[MESS2 (Loader)](mess2.md "Next routine")
    
    
           Name: MESS1                                                   [Show more]
           Type: Variable
       Category: Loader
        Summary: The OS command string for loading the BDATA binary
    
    
        Context: See this variable [in context in the source code](../../all/loader.md#header-mess1)
     References: This variable is used as follows:
                 * [Elite loader](../subroutine/elite_loader.md) uses MESS1
    
    
    
    
    
    
    .MESS1
    
     EQUS "L.BDATA FFFF1300"    \ This is short for "*LOAD BDATA FFFF1300"
     EQUB 13
    

[OSB (Loader)](../subroutine/osb.md "Previous routine")[MESS2 (Loader)](mess2.md "Next routine")
