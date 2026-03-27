---
title: "The VSCAN variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/vscan.html"
---

[IRQ1](../subroutine/irq1.md "Previous routine")[DOVDU19](../subroutine/dovdu19.md "Next routine")
    
    
           Name: VSCAN                                                   [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Defines the split position in the split-screen mode
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-vscan)
     References: This variable is used as follows:
                 * [IRQ1](../subroutine/irq1.md) uses VSCAN
                 * [SETINTS](../subroutine/setints.md) uses VSCAN
    
    
    
    
    
    
    .VSCAN
    
     EQUB 57                \ Defines the split position in the split-screen mode
    
     EQUB 30                \ The line scan counter in DL gets reset to this value
                            \ at each vertical sync, before decrementing with each
                            \ line scan
    

[IRQ1](../subroutine/irq1.md "Previous routine")[DOVDU19](../subroutine/dovdu19.md "Next routine")
