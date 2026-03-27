---
title: "The BOMBPOS variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/bombpos.html"
---

[BOMBON](../subroutine/bombon.md "Previous routine")[BOMBTBX](bombtbx.md "Next routine")
    
    
           Name: BOMBPOS                                                 [Show more]
           Type: Variable
       Category: Drawing lines
        Summary: A set of x-coordinates that are used as the basis for the energy
                 bomb's zig-zag lightning bolt
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-bombpos)
     References: This variable is used as follows:
                 * [BOMBON](../subroutine/bombon.md) uses BOMBPOS
    
    
    
    
    
    
    .BOMBPOS
    
     EQUB 224
     EQUB 224
     EQUB 192
     EQUB 160
     EQUB 128
     EQUB 96
     EQUB 64
     EQUB 32
     EQUB 0
     EQUB 0
    

[BOMBON](../subroutine/bombon.md "Previous routine")[BOMBTBX](bombtbx.md "Next routine")
