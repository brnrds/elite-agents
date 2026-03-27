---
title: "The Main flight loop (Part 10 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_10_of_16.html"
---

[Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md "Previous routine")[Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 10 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Remove if scooped, or process collisions
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-10-of-16)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Remove scooped item after both successful and failed scooping attempts
    
         * Process collisions
    
    
    
    
    .MA59
    
                            \ If we get here then scooping failed
    
     JSR EXNO3              \ Make the sound of the cargo canister being destroyed
                            \ and fall through into MA60 to remove the canister
                            \ from our local bubble
    
    .MA60
    
                            \ If we get here then scooping was successful
    
     ASL INWK+31            \ Set bit 7 of the scooped or destroyed item, to denote
     SEC                    \ that it has been killed and should be removed from
     ROR INWK+31            \ the local bubble
    
    .MA61
    
     BNE MA26               \ Jump to MA26 to skip over the collision routines and
                            \ to move on to missile targeting (this BNE is
                            \ effectively a JMP as A will never be zero)
    
    .MA67
    
                            \ If we get here then we have collided with something,
                            \ but not fatally
    
     LDA #1                 \ Set the speed in DELTA to 1 (i.e. a sudden stop)
     STA DELTA
    
     LDA #5                 \ Set the amount of damage in A to 5 (a small dent) and
     BNE MA63               \ jump down to MA63 to process the damage (this BNE is
                            \ effectively a JMP as A will never be zero)
    
    .MA58
    
                            \ If we get here, we have collided with something in a
                            \ potentially fatal way
    
     ASL INWK+31            \ Set bit 7 of the ship we just collided with, to
     SEC                    \ denote that it has been killed and should be removed
     ROR INWK+31            \ from the local bubble
    
     LDA INWK+35            \ Load A with the energy level of the ship we just hit
    
     SEC                    \ Set the amount of damage in A to 128 + A / 2, so
     ROR A                  \ this is quite a big dent, and colliding with higher
                            \ energy ships will cause more damage
    
    .MA63
    
     JSR OOPS               \ The amount of damage is in A, so call OOPS to reduce
                            \ our shields, and if the shields are gone, there's a
                            \ chance of cargo loss or even death
    
     JSR EXNO3              \ Make the sound of colliding with the other ship and
                            \ fall through into MA26 to try targeting a missile
    

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Subroutine [EXNO3](exno3.md) (category: Sound)

Make an explosion sound

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Label [MA26](main_flight_loop_part_11_of_16.md#ma26) in subroutine [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md)

[X]

Label [MA63](main_flight_loop_part_10_of_16.md#ma63) is local to this routine

[X]

Subroutine [OOPS](oops.md) (category: Flight)

Take some damage

[Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md "Previous routine")[Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md "Next routine")
