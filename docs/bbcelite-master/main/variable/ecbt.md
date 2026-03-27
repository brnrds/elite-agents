---
title: "The ECBT variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/ecbt.html"
---

[SPBT](spbt.md "Previous routine")[MSBAR](../subroutine/msbar.md "Next routine")
    
    
           Name: ECBT                                                    [Show more]
           Type: Variable
       Category: Dashboard
        Summary: The character bitmap for the E.C.M. indicator bulb
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-ecbt)
     Variations: See [code variations](../../related/compare/main/variable/ecbt.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [ECBLB](../subroutine/ecblb.md) uses ECBT
    
    
    
    
    
    * * *
    
    
     The character bitmap for the E.C.M. indicator's "E" bulb that gets displayed
     on the dashboard.
    
     The bulb is four pixels wide, so it covers two mode 2 character blocks, one
     containing the left half of the "E", and the other the right half, which are
     displayed next to each other. Each pixel is in mode 2 colour 7 (%1111), which
     is white.
    
    
    
    
    .ECBT
    
                            \ Left half of the "E" bulb
                            \
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %10101010         \ x .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %10101010         \ x .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
    
                            \ Right half of the "E" bulb
                            \
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %00000000         \ . .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %00000000         \ . .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
    
                            \ Combined "E" bulb
                            \
                            \ x x x x
                            \ x x x x
                            \ x . . .
                            \ x x x x
                            \ x x x x
                            \ x . . .
                            \ x x x x
                            \ x x x x
    

[SPBT](spbt.md "Previous routine")[MSBAR](../subroutine/msbar.md "Next routine")
