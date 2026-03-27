---
title: "The DIALS (Part 1 of 4) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dials_part_1_of_4.html"
---

[CLYNS](clyns.md "Previous routine")[DIALS (Part 2 of 4)](dials_part_2_of_4.md "Next routine")
    
    
           Name: DIALS (Part 1 of 4)                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the dashboard: speed indicator
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-dials-part-1-of-4)
     Variations: See [code variations](../../related/compare/main/subroutine/dials_part_1_of_4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md) calls DIALS
    
    
    
    
    
    * * *
    
    
     This routine updates the dashboard. First we draw all the indicators in the
     right part of the dashboard, from top (speed) to bottom (energy banks), and
     then we move on to the left part, again drawing from top (forward shield) to
     bottom (altitude).
    
     This first section starts us off with the speedometer in the top right.
    
    
    
    
    .DIALS
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA #1                 \ Set location &DDEB to 1. This location is in HAZEL,
     STA &DDEB              \ which contains the filing system RAM space, though
                            \ I'm not sure what effect this has
    
     LDA #&A0               \ Set SC(1 0) = &71A0, which is the screen address for
     STA SC                 \ the character block containing the left end of the
     LDA #&71               \ top indicator in the right part of the dashboard, the
     STA SC+1               \ one showing our speed
    
     JSR PZW2               \ Call PZW2 to set A to the colour for dangerous values
                            \ and X to the colour for safe values, suitable for
                            \ non-striped indicators
    
     STX K+1                \ Set K+1 (the colour we should show for low values) to
                            \ X (the colour to use for safe values)
    
     STA K                  \ Set K (the colour we should show for high values) to
                            \ A (the colour to use for dangerous values)
    
                            \ The above sets the following indicators to show red
                            \ for high values and yellow/white for low values
    
     LDA #14                \ Set T1 to 14, the threshold at which we change the
     STA T1                 \ indicator's colour
    
     LDA DELTA              \ Fetch our ship's speed into A, in the range 0-40
    
    \LSR A                  \ Draw the speed indicator using a range of 0-31, and
     JSR DIL-1              \ increment SC to point to the next indicator (the roll
                            \ indicator). The LSR is commented out as it isn't
                            \ required with a call to DIL-1, so perhaps this was
                            \ originally a call to DIL that got optimised
    

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Entry point [DIL-1](dilx.md#dil) in subroutine [DILX](dilx.md) (category: Dashboard)

The range of the indicator is 0-32 (for the speed indicator)

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PZW2](pzw2.md) (category: Dashboard)

Fetch the current dashboard colours for non-striped indicators, to support flashing

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[CLYNS](clyns.md "Previous routine")[DIALS (Part 2 of 4)](dials_part_2_of_4.md "Next routine")
