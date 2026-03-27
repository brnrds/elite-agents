---
title: "The MVEIT (Part 6 of 9) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mveit_part_6_of_9.html"
---

[MVEIT (Part 5 of 9)](mveit_part_5_of_9.md "Previous routine")[MVEIT (Part 7 of 9)](mveit_part_7_of_9.md "Next routine")
    
    
           Name: MVEIT (Part 6 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Move the ship in space according to our speed
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-mveit-part-6-of-9)
     Variations: See [code variations](../../related/compare/main/subroutine/mveit_part_6_of_9.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MV40](mv40.md) calls via MV45
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Move the ship in space according to our speed (we already moved it
         according to its own speed in part 3).
    
     We do this by subtracting our speed (i.e. the distance we travel in this
     iteration of the loop) from the other ship's z-coordinate. We subtract because
     they appear to be "moving" in the opposite direction to us, and the whole
     MVEIT routine is about moving the other ships rather than us (even though we
     are the one doing the moving).
    
    
    
    * * *
    
    
     Other entry points:
    
       MV45                 Rejoin the MVEIT routine after the rotation, tactics and
                            scanner code
    
    
    
    
    .MV45
    
     LDA DELTA              \ Set R to our speed in DELTA
     STA R
    
     LDA #%10000000         \ Set A to zeroes but with bit 7 set, so that (A R) is
                            \ a 16-bit number containing -R, or -speed
    
     LDX #6                 \ Set X to the z-axis so the call to MVT1 does this:
     JSR MVT1               \
                            \ (z_sign z_hi z_lo) = (z_sign z_hi z_lo) + (A R)
                            \                    = (z_sign z_hi z_lo) - speed
    
     LDA TYPE               \ If the ship type is not the sun (129) then skip the
     AND #%10000001         \ next instruction, otherwise return from the subroutine
     CMP #129               \ as we don't need to rotate the sun around its origin.
     BNE P%+3               \ Having both the AND and the CMP is a little odd, as
                            \ the sun is the only ship type with bits 0 and 7 set,
                            \ so the AND has no effect and could be removed
    
     RTS                    \ Return from the subroutine, as the ship we are moving
                            \ is the sun and doesn't need any of the following
    

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Subroutine [MVT1](mvt1.md) (category: Moving)

Calculate (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (A R)

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[MVEIT (Part 5 of 9)](mveit_part_5_of_9.md "Previous routine")[MVEIT (Part 7 of 9)](mveit_part_7_of_9.md "Next routine")
