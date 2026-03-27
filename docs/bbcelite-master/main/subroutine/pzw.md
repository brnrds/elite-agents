---
title: "The PZW subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pzw.html"
---

[PZW2](pzw2.md "Previous routine")[DILX](dilx.md "Next routine")
    
    
           Name: PZW                                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Fetch the current dashboard colours, to support flashing
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-pzw)
     Variations: See [code variations](../../related/compare/main/subroutine/pzw.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DIALS (Part 3 of 4)](dials_part_3_of_4.md) calls PZW
    
    
    
    
    
    * * *
    
    
     Set A and X to the colours we should use for indicators showing dangerous and
     safe values respectively. This enables us to implement flashing indicators,
     which is one of the game's configurable options.
    
     If flashing is enabled, the colour returned in A (dangerous values) will be
     red for 8 iterations of the main loop, and green for the next 8, before
     going back to red. If we always use PZW to decide which colours we should use
     when updating indicators, flashing colours will be automatically taken care of
     for us.
    
     The values returned are #GREEN2 for green and #RED2 for red. These are mode 2
     bytes that contain 2 pixels, with the colour of each pixel given in four bits.
    
    
    
    * * *
    
    
     Returns:
    
       A                    The colour to use for indicators with dangerous values
    
       X                    The colour to use for indicators with safe values
    
    
    
    
    .PZW
    
     LDX #STRIPE            \ Set X to the dashboard stripe colour, which is stripe
                            \ 5-1 (magenta/red)
    
     LDA MCNT               \ A will be non-zero for 8 out of every 16 main loop
     AND #%00001000         \ counts, when bit 4 is set, so this is what we use to
                            \ flash the "danger" colour
    
     AND FLH                \ A will be zeroed if flashing colours are disabled
    
     BEQ P%+5               \ If A is zero, skip the next two instructions
    
     LDA #GREEN2            \ Otherwise flashing colours are enabled and it's the
     RTS                    \ main loop iteration where we flash them, so set A to
                            \ dashboard colour 2 (green) and return from the
                            \ subroutine
    
     LDA #RED2              \ Set A to dashboard colour 1 (red)
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [FLH](../workspace/up.md#flh) in workspace [UP](../workspace/up.md)

Flashing console bars configuration setting

[X]

Configuration variable [GREEN2](../../all/workspaces.md#green2) = %00001100

Two mode 2 pixels of colour 2 (green)

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Configuration variable [RED2](../../all/workspaces.md#red2) = %00000011

Two mode 2 pixels of colour 1 (red)

[X]

Configuration variable [STRIPE](../../all/workspaces.md#stripe) = %00100011

Two mode 2 pixels of colour 5, 1 (magenta/red)

[PZW2](pzw2.md "Previous routine")[DILX](dilx.md "Next routine")
