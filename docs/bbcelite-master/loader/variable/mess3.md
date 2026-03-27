---
title: "The MESS3 (Loader) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/variable/mess3.html"
---

[MESS2 (Loader)](mess2.md "Previous routine")[ZP](../../main/workspace/zp.md "Next routine")
    
    
           Name: MESS3                                                   [Show more]
           Type: Variable
       Category: Loader
        Summary: The OS command string for changing the disc directory to E
    
    
        Context: See this variable [in context in the source code](../../all/loader.md#header-mess3)
     References: This variable is used as follows:
                 * [Elite loader](../subroutine/elite_loader.md) uses MESS3
    
    
    
    
    
    
    .MESS3
    
     EQUS "DIR E"
     EQUB 13
    

[MESS2 (Loader)](mess2.md "Previous routine")[ZP](../../main/workspace/zp.md "Next routine")
