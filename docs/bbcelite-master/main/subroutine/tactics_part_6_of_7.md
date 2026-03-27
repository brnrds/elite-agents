---
title: "The TACTICS (Part 6 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tactics_part_6_of_7.html"
---

[TACTICS (Part 5 of 7)](tactics_part_5_of_7.md "Previous routine")[TACTICS (Part 7 of 7)](tactics_part_7_of_7.md "Next routine")
    
    
           Name: TACTICS (Part 6 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Consider firing a laser at us, if aim is true
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tactics-part-6-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/tactics_part_6_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section looks at potentially firing the ship's laser at us. Specifically:
    
       * If the ship is not pointing at us, skip to the next part
    
       * If the ship is pointing at us but not accurately, fire its laser at us and
         skip to the next part
    
       * If we are in the ship's crosshairs, register some damage to our ship, slow
         down the attacking ship, make the noise of us being hit by laser fire, and
         move on to the next part to manoeuvre the attacking ship
    
    
    
    
    .TA3
    
                            \ If we get here then the ship either has plenty of
                            \ energy, or levels are low but it couldn't manage to
                            \ launch a missile, so maybe we can fire the laser?
    
     LDA #0                 \ Set A to x_hi OR y_hi OR z_hi
     JSR MAS4
    
     AND #%11100000         \ If any of the hi bytes have any of bits 5-7 set, then
     BNE TA4                \ jump to TA4 to skip the laser checks, as the ship is
                            \ too far away from us to hit us with a laser
    
     LDX CNT                \ Set X = the dot product set above in CNT. If this is
                            \ positive, this ship and our ship are facing in similar
                            \ directions, but if it's negative then we are facing
                            \ each other, so for us to be in the enemy ship's line
                            \ of fire, X needs to be negative. The value in X can
                            \ have a maximum magnitude of 36, which would mean we
                            \ were facing each other square on, so in the following
                            \ code we check X like this:
                            \
                            \   X = 0 to -31, we are not in the enemy ship's line
                            \       of fire, so they can't shoot at us
                            \
                            \   X = -32 to -34, we are in the enemy ship's line
                            \       of fire, so they can shoot at us, but they can't
                            \       hit us as we're not dead in their crosshairs
                            \
                            \   X = -35 to -36, we are bang in the middle of the
                            \       enemy ship's crosshairs, so they can not only
                            \       shoot us, they can hit us
    
    \BPL TA4                \ This instruction is commented out in the original
                            \ source
    
     CPX #160               \ If X < 160, i.e. X > -32, then we are not in the enemy
     BCC TA4                \ ship's line of fire, so jump to TA4 to skip the laser
                            \ checks
    
     LDY #19                \ Fetch the enemy ship's byte #19 from their ship's
     LDA (XX0),Y            \ blueprint into A
    
     AND #%11111000         \ Extract bits 3-7, which contain the enemy's laser
                            \ power
    
     BEQ TA4                \ If the enemy has no laser power, jump to TA4 to skip
                            \ the laser checks
    
     LDA INWK+31            \ Set bit 6 in byte #31 to denote that the ship is
     ORA #%01000000         \ firing its laser at us
     STA INWK+31
    
     CPX #163               \ If X < 163, i.e. X > -35, then we are not in the enemy
     BCC TA4                \ ship's crosshairs, so jump to TA4 to skip the laser
                            \ checks
    
    \LDY #19                \ This instruction is commented out in the original
                            \ source
    
     LDA (XX0),Y            \ Fetch the enemy ship's byte #19 from their ship's
                            \ blueprint into A
    
     LSR A                  \ Halve the enemy ship's byte #19 (which contains both
                            \ the laser power and number of missiles) to get the
                            \ amount of damage we should take
    
     JSR OOPS               \ Call OOPS to take some damage, which could do anything
                            \ from reducing the shields and energy, all the way to
                            \ losing cargo or dying (if the latter, we don't come
                            \ back from this subroutine)
    
     DEC INWK+28            \ Halve the attacking ship's acceleration in byte #28
    
     LDA ECMA               \ If an E.C.M. is currently active (either ours or an
     BNE TA9-1              \ opponent's), return from the subroutine without making
                            \ the laser-strike sound (as TA9-1 contains an RTS)
    
     JSR ELASNO             \ Call the ELASNO routine to make the sound of us
                            \ being hit by lasers
    

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [ECMA](../workspace/zp.md#ecma) in workspace [ZP](../workspace/zp.md)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Subroutine [ELASNO](elasno.md) (category: Sound)

Make the sound of us being hit

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [MAS4](mas4.md) (category: Maths (Geometry))

Calculate a cap on the maximum distance to a ship

[X]

Subroutine [OOPS](oops.md) (category: Flight)

Take some damage

[X]

Label [TA4](tactics_part_7_of_7.md#ta4) in subroutine [TACTICS (Part 7 of 7)](tactics_part_7_of_7.md)

[X]

Entry point [TA9-1](tactics_part_7_of_7.md#ta9) in subroutine [TACTICS (Part 7 of 7)](tactics_part_7_of_7.md) (category: Tactics)

Contains an RTS

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[TACTICS (Part 5 of 7)](tactics_part_5_of_7.md "Previous routine")[TACTICS (Part 7 of 7)](tactics_part_7_of_7.md "Next routine")
