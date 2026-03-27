---
title: "The TWFL variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/twfl.html"
---

[HLOIN](../subroutine/hloin.md "Previous routine")[TWFR](twfr.md "Next routine")
    
    
           Name: TWFL                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made character rows for the left end of a horizontal line
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-twfl)
     References: This variable is used as follows:
                 * [HLOIN](../subroutine/hloin.md) uses TWFL
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting horizontal line end caps in mode 1 (the top part
     of the split screen). This table provides a byte with pixels at the left end,
     which is used for the right end of the line.
    
     See the HLOIN routine for details.
    
    
    
    
    .TWFL
    
     EQUB %10001000
     EQUB %11001100
     EQUB %11101110
     EQUB %11111111
    

[HLOIN](../subroutine/hloin.md "Previous routine")[TWFR](twfr.md "Next routine")
