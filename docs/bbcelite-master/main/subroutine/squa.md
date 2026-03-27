---
title: "The SQUA subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/squa.html"
---

[MU6](mu6.md "Previous routine")[SQUA2](squa2.md "Next routine")
    
    
           Name: SQUA                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Clear bit 7 of A and calculate (A P) = A * A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-squa)
     References: This subroutine is called as follows:
                 * [NORM](norm.md) calls SQUA
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of unsigned 8-bit numbers, after first
     clearing bit 7 of A:
    
       (A P) = A * A
    
    
    
    
    .SQUA
    
     AND #%01111111         \ Clear bit 7 of A and fall through into SQUA2 to set
                            \ (A P) = A * A
    

[MU6](mu6.md "Previous routine")[SQUA2](squa2.md "Next routine")
