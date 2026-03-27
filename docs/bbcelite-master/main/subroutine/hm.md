---
title: "The hm subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hm.html"
---

[qv](qv.md "Previous routine")[refund](refund.md "Next routine")
    
    
           Name: hm                                                      [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Select the closest system and redraw the chart crosshairs
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-hm)
     Variations: See [code variations](../../related/compare/main/subroutine/hm.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hyp](hyp.md) calls hm
                 * [TT102](tt102.md) calls hm
    
    
    
    
    
    * * *
    
    
     Set the system closest to galactic coordinates (QQ9, QQ10) as the selected
     system, redraw the crosshairs on the chart accordingly (if they are being
     shown), and if this is not the space view, clear the bottom three text rows of
     the screen.
    
    
    
    
    .hm
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will erase the crosshairs currently there
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will draw the crosshairs at our current home
                            \ system
    
     JMP CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ move the text cursor to the first cleared row, and
                            \ return from the subroutine using a tail call
    

[X]

Subroutine [CLYNS](clyns.md) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Subroutine [TT103](tt103.md) (category: Charts)

Draw a small set of crosshairs on a chart

[X]

Subroutine [TT111](tt111.md) (category: Universe)

Set the current system to the nearest system to a point

[qv](qv.md "Previous routine")[refund](refund.md "Next routine")
