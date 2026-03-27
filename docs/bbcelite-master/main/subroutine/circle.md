---
title: "The CIRCLE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/circle.html"
---

[SUN (Part 4 of 4)](sun_part_4_of_4.md "Previous routine")[CIRCLE2](circle2.md "Next routine")
    
    
           Name: CIRCLE                                                  [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw a circle for the planet
      Deep dive: [Drawing circles](https://elite.bbcelite.com/deep_dives/drawing_circles.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-circle)
     Variations: See [code variations](../../related/compare/main/subroutine/circle.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PL9 (Part 1 of 3)](pl9_part_1_of_3.md) calls CIRCLE
    
    
    
    
    
    * * *
    
    
     Draw a circle with the centre at (K3, K4) and radius K. Used to draw the
     planet's main outline.
    
    
    
    * * *
    
    
     Arguments:
    
       K                    The planet's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the planet
    
       K4(1 0)              Pixel y-coordinate of the centre of the planet
    
    
    
    
    .CIRCLE
    
     JSR CHKON              \ Call CHKON to check whether the circle fits on-screen
    
     BCS RTS2               \ If CHKON set the C flag then the circle does not fit
                            \ on-screen, so return from the subroutine (as RTS2
                            \ contains an RTS)
    
     LDA #0                 \ Set LSX2 = 0 to indicate that the ball line heap is
     STA LSX2               \ not empty, as we are about to fill it
    
     LDX K                  \ Set X = K = radius
    
     LDA #8                 \ Set A = 8
    
     CPX #8                 \ If the radius < 8, skip to PL89
     BCC PL89
    
     LSR A                  \ Halve A so A = 4
    
     CPX #60                \ If the radius < 60, skip to PL89
     BCC PL89
    
     LSR A                  \ Halve A so A = 2
    
    .PL89
    
     STA STP                \ Set STP = A. STP is the step size for the circle, so
                            \ the above sets a smaller step size for bigger circles
    
                            \ Fall through into CIRCLE2 to draw the circle with the
                            \ correct step size
    

[X]

Subroutine [CHKON](chkon.md) (category: Drawing circles)

Check whether any part of a circle appears on the extended screen

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [LSX2](../workspace/wp.md#lsx2) in workspace [WP](../workspace/wp.md)

The ball line heap for storing x-coordinates

[X]

Label [PL89](circle.md#pl89) is local to this routine

[X]

Entry point [RTS2](sun_part_4_of_4.md#rts2) in subroutine [SUN (Part 4 of 4)](sun_part_4_of_4.md) (category: Drawing suns)

Contains an RTS

[X]

Variable [STP](../workspace/zp.md#stp) in workspace [ZP](../workspace/zp.md)

The step size for drawing circles

[SUN (Part 4 of 4)](sun_part_4_of_4.md "Previous routine")[CIRCLE2](circle2.md "Next routine")
