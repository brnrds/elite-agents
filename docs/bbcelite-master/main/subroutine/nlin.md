---
title: "The NLIN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nlin.html"
---

[NLIN4](nlin4.md "Previous routine")[NLIN2](nlin2.md "Next routine")
    
    
           Name: NLIN                                                    [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a horizontal line at pixel row 23 to box in a title
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-nlin)
     Variations: See [code variations](../../related/compare/main/subroutine/nlin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT22](tt22.md) calls NLIN
                 * [TT22](tt22.md) calls via NLIN5
    
    
    
    
    
    * * *
    
    
     Draw a horizontal line at pixel row 23 and move the text cursor down one
     line.
    
    
    
    * * *
    
    
     Other entry points:
    
       NLIN5                Move the text cursor down one line before drawing the
                            line
    
    
    
    
    .NLIN
    
     LDA #23                \ Set A = 23 so NLIN2 below draws a horizontal line at
                            \ pixel row 23
    
    .NLIN5
    
     INC YC                 \ Move the text cursor down one line
    
                            \ Fall through into NLIN2 to draw the horizontal line
                            \ at row 23
    

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[NLIN4](nlin4.md "Previous routine")[NLIN2](nlin2.md "Next routine")
