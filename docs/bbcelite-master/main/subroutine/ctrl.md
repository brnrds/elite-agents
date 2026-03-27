---
title: "The CTRL subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ctrl.html"
---

[FILLKL](fillkl.md "Previous routine")[DKS5](dks5.md "Next routine")
    
    
           Name: CTRL                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard to see if CTRL is currently pressed
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-ctrl)
     Variations: See [code variations](../../related/compare/main/subroutine/ctrl.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hyp](hyp.md) calls CTRL
                 * [TT18](tt18.md) calls CTRL
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X = %10000001 (i.e. 129 or -127) if CTRL is being
                            pressed
    
                            X = 1 if CTRL is not being pressed
    
       A                    Contains the same as X
    
    
    
    
    IF _SNG47
    
    .CTRL
    
     LDA #1                 \ Set A to the internal key number for CTRL and fall
                            \ through into DKS5 to scan the keyboard
    
    ENDIF
    

[FILLKL](fillkl.md "Previous routine")[DKS5](dks5.md "Next routine")
