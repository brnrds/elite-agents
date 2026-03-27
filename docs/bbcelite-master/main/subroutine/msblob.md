---
title: "The msblob subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/msblob.html"
---

[ZINF](zinf.md "Previous routine")[me2](me2.md "Next routine")
    
    
           Name: msblob                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Display the dashboard's missile indicators in green
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-msblob)
     Variations: See [code variations](../../related/compare/main/subroutine/msblob.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 2 of 2)](br1_part_2_of_2.md) calls msblob
                 * [EQSHP](eqshp.md) calls msblob
                 * [SOS1](sos1.md) calls msblob
    
    
    
    
    
    * * *
    
    
     Display the dashboard's missile indicators, with all the missiles reset to
     green (i.e. not armed or locked).
    
    
    
    
    .msblob
    
     LDX #4                 \ Set up a loop counter in X to count through all four
                            \ missile indicators
    
    .ss
    
     CPX NOMSL              \ If the counter is equal to the number of missiles,
     BEQ SAL8               \ jump down to SAL8 to draw the remaining missiles, as
                            \ the rest of them are present and should be drawn in
                            \ green
    
     LDY #0                 \ Draw the missile indicator at position X in black
     JSR MSBAR
    
     DEX                    \ Decrement the counter to point to the next missile
    
     BNE ss                 \ Loop back to ss if we still have missiles to draw
    
     RTS                    \ Return from the subroutine
    
    .SAL8
    
     LDY #GREEN2            \ Draw the missile indicator at position X in green
     JSR MSBAR
    
     DEX                    \ Decrement the counter to point to the next missile
    
     BNE SAL8               \ Loop back to SAL8 if we still have missiles to draw
    
     RTS                    \ Return from the subroutine
    

[X]

Configuration variable [GREEN2](../../all/workspaces.md#green2) = %00001100

Two mode 2 pixels of colour 2 (green)

[X]

Subroutine [MSBAR](msbar.md) (category: Dashboard)

Draw a specific indicator in the dashboard's missile bar

[X]

Variable [NOMSL](../workspace/wp.md#nomsl) in workspace [WP](../workspace/wp.md)

The number of missiles we have fitted (0-4)

[X]

Label [SAL8](msblob.md#sal8) is local to this routine

[X]

Label [ss](msblob.md#ss) is local to this routine

[ZINF](zinf.md "Previous routine")[me2](me2.md "Next routine")
