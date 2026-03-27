---
title: "The DIALS (Part 4 of 4) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dials_part_4_of_4.html"
---

[DIALS (Part 3 of 4)](dials_part_3_of_4.md "Previous routine")[PZW2](pzw2.md "Next routine")
    
    
           Name: DIALS (Part 4 of 4)                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the dashboard: shields, fuel, laser & cabin temp, altitude
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-dials-part-4-of-4)
     Variations: See [code variations](../../related/compare/main/subroutine/dials_part_4_of_4.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
     LDA #&70               \ Set SC(1 0) = &7020, which is the screen address for
     STA SC+1               \ the character block containing the left end of the
     LDA #&20               \ top indicator in the left part of the dashboard, the
     STA SC                 \ one showing the forward shield
    
     LDA FSH                \ Draw the forward shield indicator using a range of
     JSR DILX               \ 0-255, and increment SC to point to the next indicator
                            \ (the aft shield)
    
     LDA ASH                \ Draw the aft shield indicator using a range of 0-255,
     JSR DILX               \ and increment SC to point to the next indicator (the
                            \ fuel level)
    
     LDA #YELLOW2           \ Set K (the colour we should show for high values) to
     STA K                  \ yellow
    
     STA K+1                \ Set K+1 (the colour we should show for low values) to
                            \ yellow, so the fuel indicator always shows in this
                            \ colour
    
     LDA QQ14               \ Draw the fuel level indicator using a range of 0-63,
     JSR DILX+2             \ and increment SC to point to the next indicator (the
                            \ cabin temperature)
    
     JSR PZW2               \ Call PZW2 to set A to the colour for dangerous values
                            \ and X to the colour for safe values, suitable for
                            \ non-striped indicators
    
     STX K+1                \ Set K+1 (the colour we should show for low values) to
                            \ X (the colour to use for safe values)
    
     STA K                  \ Set K (the colour we should show for high values) to
                            \ A (the colour to use for dangerous values)
    
                            \ The above sets the following indicators to show red
                            \ for high values and yellow/white for low values, which
                            \ we use for the cabin and laser temperature bars
    
     LDX #11                \ Set T1 to 11, the threshold at which we change the
     STX T1                 \ cabin and laser temperature indicators' colours
    
     LDA CABTMP             \ Draw the cabin temperature indicator using a range of
     JSR DILX               \ 0-255, and increment SC to point to the next indicator
                            \ (the laser temperature)
    
     LDA GNTMP              \ Draw the laser temperature indicator using a range of
     JSR DILX               \ 0-255, and increment SC to point to the next indicator
                            \ (the altitude)
    
     LDA #240               \ Set T1 to 240, the threshold at which we change the
     STA T1                 \ altitude indicator's colour. As the altitude has a
                            \ range of 0-255, pixel 16 will not be filled in, and
                            \ 240 would change the colour when moving between pixels
                            \ 15 and 16, so this effectively switches off the colour
                            \ change for the altitude indicator
    
     LDA #YELLOW2           \ Set K (the colour we should show for high values) to
     STA K                  \ yellow
    
     STA K+1                \ Set K+1 (the colour we should show for low values) to
                            \ 240, or &F0 (dashboard colour 2, yellow/white), so the
                            \ altitude indicator always shows in this colour
    
     LDA ALTIT              \ Draw the altitude indicator using a range of 0-255
     JSR DILX
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     JMP COMPAS             \ We have now drawn all the indicators, so jump to
                            \ COMPAS to draw the compass, returning from the
                            \ subroutine using a tail call
    

[X]

Variable [ALTIT](../workspace/wp.md#altit) in workspace [WP](../workspace/wp.md)

Our altitude above the surface of the planet or sun

[X]

Variable [ASH](../workspace/zp.md#ash) in workspace [ZP](../workspace/zp.md)

Aft shield status

[X]

Variable [CABTMP](../workspace/wp.md#cabtmp) in workspace [WP](../workspace/wp.md)

Cabin temperature

[X]

Subroutine [COMPAS](compas.md) (category: Dashboard)

Update the compass

[X]

Subroutine [DILX](dilx.md) (category: Dashboard)

Update a bar-based indicator on the dashboard

[X]

Entry point [DILX+2](dilx.md#dilx) in subroutine [DILX](dilx.md) (category: Dashboard)

The range of the indicator is 0-64 (for the fuel indicator)

[X]

Variable [FSH](../workspace/zp.md#fsh) in workspace [ZP](../workspace/zp.md)

Forward shield status

[X]

Variable [GNTMP](../workspace/wp.md#gntmp) in workspace [WP](../workspace/wp.md)

Laser temperature (or "gun temperature")

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PZW2](pzw2.md) (category: Dashboard)

Fetch the current dashboard colours for non-striped indicators, to support flashing

[X]

Variable [QQ14](../workspace/wp.md#qq14) in workspace [WP](../workspace/wp.md)

Our current fuel level (0-70)

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Configuration variable [YELLOW2](../../all/workspaces.md#yellow2) = %00001111

Two mode 2 pixels of colour 3 (yellow)

[DIALS (Part 3 of 4)](dials_part_3_of_4.md "Previous routine")[PZW2](pzw2.md "Next routine")
