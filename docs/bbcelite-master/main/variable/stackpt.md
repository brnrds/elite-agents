---
title: "The stackpt variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/stackpt.html"
---

[MAS4](../subroutine/mas4.md "Previous routine")[NEWBRK](../subroutine/newbrk.md "Next routine")
    
    
           Name: stackpt                                                 [Show more]
           Type: Variable
       Category: Save and load
        Summary: Temporary storage for the stack pointer when jumping to the break
                 handler at NEWBRK
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-stackpt)
     Variations: See [code variations](../../related/compare/main/variable/stack-stackpt.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [NEWBRK](../subroutine/newbrk.md) uses stackpt
                 * [SVE](../subroutine/sve.md) uses stackpt
    
    
    
    
    
    
    .stackpt
    
     EQUB &FF
    

[MAS4](../subroutine/mas4.md "Previous routine")[NEWBRK](../subroutine/newbrk.md "Next routine")
