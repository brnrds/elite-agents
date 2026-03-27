---
title: "The TRADEMODE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/trademode.html"
---

[INCYC](incyc.md "Previous routine")[TT20](tt20.md "Next routine")
    
    
           Name: TRADEMODE                                               [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the screen and set up a trading screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-trademode)
     Variations: See [code variations](../../related/compare/main/subroutine/trademode.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls TRADEMODE
                 * [STATUS](status.md) calls TRADEMODE
                 * [TT167](tt167.md) calls TRADEMODE
                 * [TT208](tt208.md) calls TRADEMODE
                 * [TT213](tt213.md) calls TRADEMODE
                 * [TT219](tt219.md) calls TRADEMODE
                 * [TT25](tt25.md) calls TRADEMODE
                 * [SVE](sve.md) calls via TRADEMODE2
    
    
    
    
    
    * * *
    
    
     Clear the top part of the screen, draw a border box, set the palette for
     trading screens, and set the current view type in QQ11 to A.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The type of the new current view (see QQ11 for a list of
                            view types)
    
    
    
    * * *
    
    
     Other entry points:
    
       TRADEMODE2           Set the palette for trading screens and switch the
                            current colour to white
    
    
    
    
    .TRADEMODE
    
     JSR TT66               \ Clear the top part of the screen, draw a border box,
                            \ and set the current view type in QQ11 to A
    
     JSR FLKB               \ Call FLKB to flush the keyboard buffer
    
    .TRADEMODE2
    
     LDA #48                \ Switch to the mode 1 palette for trading screens,
     JSR DOVDU19            \ which is yellow (colour 1), magenta (colour 2) and
                            \ white (colour 3)
    
     LDA #CYAN              \ Switch to colour 3, which is white in the trade view
     STA COL
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Subroutine [DOVDU19](dovdu19.md) (category: Drawing the screen)

Change the mode 1 palette

[X]

Subroutine [FLKB](flkb.md) (category: Keyboard)

Flush the keyboard buffer

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[INCYC](incyc.md "Previous routine")[TT20](tt20.md "Next routine")
