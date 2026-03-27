---
title: "The Main flight loop (Part 7 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_7_of_16.html"
---

[Main flight loop (Part 6 of 16)](main_flight_loop_part_6_of_16.md "Previous routine")[Main flight loop (Part 8 of 16)](main_flight_loop_part_8_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 7 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Check whether we are docking, scooping or
                 colliding with it
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-7-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_7_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Check how close we are to this ship and work out if we are docking,
           scooping or colliding with it
    
    
    
    
     LDA INWK+31            \ Fetch the status of this ship from bits 5 (is ship
     AND #%10100000         \ exploding?) and bit 7 (has ship been killed?) from
                            \ ship byte #31 into A
    
     JSR MAS4               \ Or this value with x_hi, y_hi and z_hi
    
     BNE MA65               \ If this value is non-zero, then either the ship is
                            \ far away (i.e. has a non-zero high byte in at least
                            \ one of the three axes), or it is already exploding,
                            \ or has been flagged as being killed - in which case
                            \ jump to MA65 to skip the following, as we can't dock
                            \ scoop or collide with it
    
     LDA INWK               \ Set A = (x_lo OR y_lo OR z_lo), and if bit 7 of the
     ORA INWK+3             \ result is set, the ship is still a fair distance
     ORA INWK+6             \ away (further than 127 in at least one axis), so jump
     BMI MA65               \ to MA65 to skip the following, as it's too far away to
                            \ dock, scoop or collide with
    
     LDX TYPE               \ If the current ship type is negative then it's either
     BMI MA65               \ a planet or a sun, so jump down to MA65 to skip the
                            \ following, as we can't dock with it or scoop it
    
     CPX #SST               \ If this ship is the space station, jump to ISDK to
     BEQ ISDK               \ check whether we are docking with it
    
     AND #%11000000         \ If bit 6 of (x_lo OR y_lo OR z_lo) is set, then the
     BNE MA65               \ ship is still a reasonable distance away (further than
                            \ 63 in at least one axis), so jump to MA65 to skip the
                            \ following, as it's too far away to dock, scoop or
                            \ collide with
    
     CPX #MSL               \ If this ship is a missile, jump down to MA65 to skip
     BEQ MA65               \ the following, as we can't scoop or dock with a
                            \ missile, and it has its own dedicated collision
                            \ checks in the TACTICS routine
    
     LDA BST                \ If we have fuel scoops fitted then BST will be &FF,
                            \ otherwise it will be 0
    
     AND INWK+5             \ Ship byte #5 contains the y_sign of this ship, so a
                            \ negative value here means the canister is below us,
                            \ which means the result of the AND will be negative if
                            \ the canister is below us and we have a fuel scoop
                            \ fitted
    
     BPL MA58               \ If the result is positive, then we either have no
                            \ scoop or the canister is above us, and in both cases
                            \ this means we can't scoop the item, so jump to MA58
                            \ to process a collision
    

[X]

Variable [BST](../workspace/wp.md#bst) in workspace [WP](../workspace/wp.md)

Fuel scoops (BST stands for "barrel status")

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Label [ISDK](main_flight_loop_part_9_of_16.md#isdk) in subroutine [Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md)

[X]

Label [MA58](main_flight_loop_part_10_of_16.md#ma58) in subroutine [Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md)

[X]

Label [MA65](main_flight_loop_part_8_of_16.md#ma65) in subroutine [Main flight loop (Part 8 of 16)](main_flight_loop_part_8_of_16.md)

[X]

Subroutine [MAS4](mas4.md) (category: Maths (Geometry))

Calculate a cap on the maximum distance to a ship

[X]

Configuration variable [MSL](../../all/workspaces.md#msl) = 1

Ship type for a missile

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[Main flight loop (Part 6 of 16)](main_flight_loop_part_6_of_16.md "Previous routine")[Main flight loop (Part 8 of 16)](main_flight_loop_part_8_of_16.md "Next routine")
