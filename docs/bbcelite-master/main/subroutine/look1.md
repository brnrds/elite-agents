---
title: "The LOOK1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/look1.html"
---

[PLUT](plut.md "Previous routine")[SIGHT](sight.md "Next routine")
    
    
           Name: LOOK1                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Initialise the space view
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-look1)
     Variations: See [code variations](../../related/compare/main/subroutine/look1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MJP](mjp.md) calls LOOK1
                 * [TT102](tt102.md) calls LOOK1
                 * [TT110](tt110.md) calls LOOK1
                 * [WARP](warp.md) calls LOOK1
                 * [SIGHT](sight.md) calls via LO2
    
    
    
    
    
    * * *
    
    
     Initialise the space view, with the direction of view given in X. This clears
     the upper screen and draws the laser crosshairs, if the view in X has lasers
     fitted. It also wipes all the ships from the scanner, so we can recalculate
     ship positions for the new view (they get put back in the main flight loop).
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The space view to set:
    
                              * 0 = front
                              * 1 = rear
                              * 2 = left
                              * 3 = right
    
    
    
    * * *
    
    
     Other entry points:
    
       LO2                  Contains an RTS
    
    
    
    
    .LO2
    
     RTS                    \ Return from the subroutine
    
    .LQ
    
     STX VIEW               \ Set the current space view to X
    
     JSR TT66               \ Clear the top part of the screen, draw a border box,
                            \ and set the current view type in QQ11 to 0 (space
                            \ view)
    
     JSR SIGHT              \ Draw the laser crosshairs
    
     LDA BOMB               \ If our energy bomb has been set off, then BOMB will be
     BPL P%+5               \ negative, so this skips the following instruction if
                            \ our energy bomb is not going off
    
     JSR BOMBOFF            \ Our energy bomb is going off, so call BOMBOFF to draw
                            \ the zig-zag lightning bolt
    
     JMP NWSTARS            \ Set up a new stardust field and return from the
                            \ subroutine using a tail call
    
    .LOOK1
    
     LDA #0                 \ Set A = 0, the type number of a space view
    
     JSR DOVDU19            \ Switch to the mode 1 palette for the space view,
                            \ which is yellow (colour 1), red (colour 2) and cyan
                            \ (colour 3)
    
     LDY QQ11               \ If the current view is not a space view, jump up to LQ
     BNE LQ                 \ to set up a new space view
    
     CPX VIEW               \ If the current view is already of type X, jump to LO2
     BEQ LO2                \ to return from the subroutine (as LO2 contains an RTS)
    
     STX VIEW               \ Change the current space view to X
    
     JSR TT66               \ Clear the top part of the screen, draw a border box,
                            \ and set the current view type in QQ11 to 0 (space
                            \ view)
    
     JSR FLIP               \ Swap the x- and y-coordinates of all the stardust
                            \ particles and redraw the stardust field
    
     LDA BOMB               \ If our energy bomb has been set off, then BOMB will be
     BPL P%+5               \ negative, so this skips the following instruction if
                            \ our energy bomb is not going off
    
     JSR BOMBOFF            \ Our energy bomb is going off, so call BOMBOFF to draw
                            \ the zig-zag lightning bolt
    
     JSR WPSHPS             \ Wipe all the ships from the scanner and mark them all
                            \ as not being shown on-screen
    
                            \ And fall through into SIGHT to draw the laser
                            \ crosshairs
    

[X]

Variable [BOMB](../workspace/wp.md#bomb) in workspace [WP](../workspace/wp.md)

Energy bomb

[X]

Subroutine [BOMBOFF](bomboff.md) (category: Drawing lines)

Draw the zig-zag lightning bolt for the energy bomb

[X]

Subroutine [DOVDU19](dovdu19.md) (category: Drawing the screen)

Change the mode 1 palette

[X]

Subroutine [FLIP](flip.md) (category: Stardust)

Reflect the stardust particles in the screen diagonal and redraw the stardust field

[X]

Entry point [LO2](look1.md#lo2) in subroutine [LOOK1](look1.md) (category: Flight)

Contains an RTS

[X]

Label [LQ](look1.md#lq) is local to this routine

[X]

Subroutine [NWSTARS](nwstars.md) (category: Stardust)

Initialise the stardust field

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Subroutine [SIGHT](sight.md) (category: Flight)

Draw the laser crosshairs

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Variable [VIEW](../workspace/wp.md#view) in workspace [WP](../workspace/wp.md)

The number of the current space view

[X]

Subroutine [WPSHPS](wpshps.md) (category: Dashboard)

Clear the scanner, reset the ball line and sun line heaps

[PLUT](plut.md "Previous routine")[SIGHT](sight.md "Next routine")
