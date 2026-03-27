---
title: "The VEC variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/vec.html"
---

[TVT3](tvt3.md "Previous routine")[WSCAN](../subroutine/wscan.md "Next routine")
    
    
           Name: VEC                                                     [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: The original value of the IRQ1 vector
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-vec)
     Variations: See [code variations](../../related/compare/main/variable/vec.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [SETINTS](../subroutine/setints.md) uses VEC
    
    
    
    
    
    
    .VEC
    
     EQUW &8888             \ This gets set to the value of the original IRQ1 vector
                            \ by the STARTUP routine
    

[TVT3](tvt3.md "Previous routine")[WSCAN](../subroutine/wscan.md "Next routine")
