---
title: "The CTRLmc subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ctrlmc.html"
---

[RETURN](return.md "Previous routine")[DKS4mc](dks4mc.md "Next routine")
    
    
           Name: CTRLmc                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the Master Compact keyboard to see if CTRL is currently
                 pressed
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-ctrlmc)
     References: This subroutine is called as follows:
                 * [hyp](hyp.md) calls CTRLmc
                 * [TT18](tt18.md) calls CTRLmc
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X = %10000001 (i.e. 129 or -127) if CTRL is being
                            pressed
    
                            X = 1 if CTRL is not being pressed
    
       A                    Contains the same as X
    
    
    
    
    IF _COMPACT
    
    .CTRLmc
    
     LDA #1                 \ Set A to the internal key number for CTRL and fall
                            \ through to DKS4mc to scan the keyboard
    
    ENDIF
    

[RETURN](return.md "Previous routine")[DKS4mc](dks4mc.md "Next routine")
