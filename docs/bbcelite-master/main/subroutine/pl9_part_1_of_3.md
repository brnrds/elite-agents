---
title: "The PL9 (Part 1 of 3) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pl9_part_1_of_3.html"
---

[PLANET](planet.md "Previous routine")[PL9 (Part 2 of 3)](pl9_part_2_of_3.md "Next routine")
    
    
           Name: PL9 (Part 1 of 3)                                       [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw the planet, with either an equator and meridian, or a crater
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-pl9-part-1-of-3)
     Variations: See [code variations](../../related/compare/main/subroutine/pl9_part_1_of_3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLANET](planet.md) calls PL9
    
    
    
    
    
    * * *
    
    
     Draw the planet with radius K at pixel coordinate (K3, K4), and with either an
     equator and meridian, or a crater.
    
    
    
    * * *
    
    
     Arguments:
    
       K(1 0)               The planet's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the planet
    
       K4(1 0)              Pixel y-coordinate of the centre of the planet
    
       INWK                 The planet's ship data block
    
    
    
    
    .PL9
    
     JSR WPLS2              \ Call WPLS2 to remove the planet from the screen
    
     JSR CIRCLE             \ Call CIRCLE to draw the planet's new circle
    
     BCS PL20               \ If the call to CIRCLE returned with the C flag set,
                            \ then the circle does not fit on-screen, so jump to
                            \ PL20 to return from the subroutine
    
     LDA K+1                \ If K+1 is zero, jump to PL25 as K(1 0) < 256, so the
     BEQ PL25               \ planet fits on the screen and we can draw meridians or
                            \ craters
    
    .PL20
    
     RTS                    \ The planet doesn't fit on-screen, so return from the
                            \ subroutine
    
    .PL25
    
     LDA TYPE               \ If the planet type is 128 then it has an equator and
     CMP #128               \ a meridian, so this jumps to PL26 if this is not a
     BNE PL26               \ planet with an equator - in other words, if it is a
                            \ planet with a crater
    
                            \ Otherwise this is a planet with an equator and
                            \ meridian, so fall through into the following to draw
                            \ them
    

[X]

Subroutine [CIRCLE](circle.md) (category: Drawing circles)

Draw a circle for the planet

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [PL20](pl9_part_1_of_3.md#pl20) is local to this routine

[X]

Label [PL25](pl9_part_1_of_3.md#pl25) is local to this routine

[X]

Label [PL26](pl9_part_3_of_3.md#pl26) in subroutine [PL9 (Part 3 of 3)](pl9_part_3_of_3.md)

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Subroutine [WPLS2](wpls2.md) (category: Drawing planets)

Remove the planet from the screen

[PLANET](planet.md "Previous routine")[PL9 (Part 2 of 3)](pl9_part_2_of_3.md "Next routine")
