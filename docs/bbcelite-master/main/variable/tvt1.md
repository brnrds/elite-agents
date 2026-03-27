---
title: "The TVT1 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/tvt1.html"
---

[SETINTS](../subroutine/setints.md "Previous routine")[IRQ1](../subroutine/irq1.md "Next routine")
    
    
           Name: TVT1                                                    [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Palette data for the mode 2 part of the screen (the dashboard)
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-tvt1)
     References: This variable is used as follows:
                 * [IRQ1](../subroutine/irq1.md) uses TVT1
    
    
    
    
    
    * * *
    
    
     This palette is applied in the IRQ1 routine. If we have an escape pod fitted,
     then the first byte is changed to &30, which maps logical colour 3 to actual
     colour 0 (black) instead of colour 4 (blue).
    
    
    
    
    .TVT1
    
     EQUB &34, &43
     EQUB &25, &16
     EQUB &86, &70
     EQUB &61, &52
     EQUB &C3, &B4
     EQUB &A5, &96
     EQUB &07, &F0
     EQUB &E1, &D2
    

[SETINTS](../subroutine/setints.md "Previous routine")[IRQ1](../subroutine/irq1.md "Next routine")
