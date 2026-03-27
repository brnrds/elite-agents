---
title: "The SOFH variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/sofh.html"
---

[BEEP](../subroutine/beep.md "Previous routine")[SOOFF](sooff.md "Next routine")
    
    
           Name: SOFH                                                    [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound chip data mask for choosing a tone channel in the range 0-2
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-sofh)
     References: This variable is used as follows:
                 * [SOINT](../subroutine/soint.md) uses SOFH
    
    
    
    
    
    
    .SOFH
    
     EQUB %11000000         \ Mask for a latch byte for channel 2
     EQUB %10100000         \ Mask for a latch byte for channel 1
     EQUB %10000000         \ Mask for a latch byte for channel 0
    

[BEEP](../subroutine/beep.md "Previous routine")[SOOFF](sooff.md "Next routine")
