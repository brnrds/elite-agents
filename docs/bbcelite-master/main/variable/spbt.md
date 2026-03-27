---
title: "The SPBT variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/spbt.html"
---

[SPBLB](../subroutine/spblb.md "Previous routine")[ECBT](ecbt.md "Next routine")
    
    
           Name: SPBT                                                    [Show more]
           Type: Variable
       Category: Dashboard
        Summary: The bitmap definition for the space station indicator bulb
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-spbt)
     Variations: See [code variations](../../related/compare/main/variable/spbt.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [SPBLB](../subroutine/spblb.md) uses SPBT
    
    
    
    
    
    * * *
    
    
     The bitmap definition for the space station indicator's "S" bulb that gets
     displayed on the dashboard.
    
     The bulb is four pixels wide, so it covers two mode 2 character blocks, one
     containing the left half of the "S", and the other the right half, which are
     displayed next to each other. Each pixel is in mode 2 colour 7 (%1111), which
     is white.
    
    
    
    
    .SPBT
    
                            \ Left half of the "S" bulb
                            \
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %10101010         \ x .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %00000000         \ . .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
    
                            \ Right half of the "S" bulb
                            \
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %00000000         \ . .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %01010101         \ . x
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
    
                            \ Combined "S" bulb
                            \
                            \ x x x x
                            \ x x x x
                            \ x . . .
                            \ x x x x
                            \ x x x x
                            \ . . . x
                            \ x x x x
                            \ x x x x
    

[SPBLB](../subroutine/spblb.md "Previous routine")[ECBT](ecbt.md "Next routine")
