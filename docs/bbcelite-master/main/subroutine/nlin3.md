---
title: "The NLIN3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nlin3.html"
---

[FLKB](flkb.md "Previous routine")[NLIN4](nlin4.md "Next routine")
    
    
           Name: NLIN3                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Print a title and draw a horizontal line at row 19 to box it in
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-nlin3)
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls NLIN3
                 * [STATUS](status.md) calls NLIN3
                 * [TT167](tt167.md) calls NLIN3
                 * [TT208](tt208.md) calls NLIN3
                 * [TT23](tt23.md) calls NLIN3
                 * [TT25](tt25.md) calls NLIN3
    
    
    
    
    
    * * *
    
    
     This routine print a text token at the cursor position and draws a horizontal
     line at pixel row 19. It is used for the Status Mode screen, the Short-range
     Chart, the Market Price screen and the Equip Ship screen.
    
    
    
    
    .NLIN3
    
     JSR TT27               \ Print the text token in A
    
                            \ Fall through into NLIN4 to draw a horizontal line at
                            \ pixel row 19
    

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[FLKB](flkb.md "Previous routine")[NLIN4](nlin4.md "Next routine")
