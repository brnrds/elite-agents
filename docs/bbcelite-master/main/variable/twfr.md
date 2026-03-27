---
title: "The TWFR variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/twfr.html"
---

[TWFL](twfl.md "Previous routine")[orange](orange.md "Next routine")
    
    
           Name: TWFR                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made character rows for the right end of a horizontal line
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-twfr)
     References: This variable is used as follows:
                 * [HLOIN](../subroutine/hloin.md) uses TWFR
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting horizontal line end caps in mode 1 (the top part
     of the split screen). This table provides a byte with pixels at the left end,
     which is used for the right end of the line.
    
     See the HLOIN routine for details.
    
    
    
    
    .TWFR
    
     EQUB %11111111
     EQUB %01110111
     EQUB %00110011
     EQUB %00010001
    

[TWFL](twfl.md "Previous routine")[orange](orange.md "Next routine")
