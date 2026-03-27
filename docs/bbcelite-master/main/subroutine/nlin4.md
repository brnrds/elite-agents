---
title: "The NLIN4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nlin4.html"
---

[NLIN3](nlin3.md "Previous routine")[NLIN](nlin.md "Next routine")
    
    
           Name: NLIN4                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a horizontal line at pixel row 19 to box in a title
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-nlin4)
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls NLIN4
                 * [TT213](tt213.md) calls NLIN4
    
    
    
    
    
    * * *
    
    
     This routine is used on the Inventory screen to draw a horizontal line at
     pixel row 19 to box in the title.
    
    
    
    
    .NLIN4
    
     LDA #19                \ Jump to NLIN2 to draw a horizontal line at pixel row
     BNE NLIN2              \ 19, returning from the subroutine with using a tail
                            \ call (this BNE is effectively a JMP as A will never
                            \ be zero)
    

[X]

Subroutine [NLIN2](nlin2.md) (category: Drawing lines)

Draw a screen-wide horizontal line at the pixel row in A

[NLIN3](nlin3.md "Previous routine")[NLIN](nlin.md "Next routine")
