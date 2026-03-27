---
title: "The SHIFT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/shift.html"
---

[ylookup](../variable/ylookup.md "Previous routine")[RETURN](return.md "Next routine")
    
    
           Name: SHIFT                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard to see if SHIFT is currently pressed
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-shift)
     References: This subroutine is called as follows:
                 * [MT26](mt26.md) calls SHIFT
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X = %10000000 if SHIFT is being pressed
    
                            X = 0 if SHIFT is not being pressed
    
       A                    Contains the same as X
    
    
    
    
    IF _COMPACT
    
    .SHIFT
    
     LDA #0                 \ Set A to the internal key number for SHIFT and fall
                            \ through to DKS4mc to scan the keyboard
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &49, or BIT &49A9, which does nothing apart
                            \ from affect the flags
    
    ENDIF
    

[ylookup](../variable/ylookup.md "Previous routine")[RETURN](return.md "Next routine")
