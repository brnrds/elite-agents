---
title: "The Main flight loop (Part 5 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_5_of_16.html"
---

[Main flight loop (Part 4 of 16)](main_flight_loop_part_4_of_16.md "Previous routine")[Main flight loop (Part 6 of 16)](main_flight_loop_part_6_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 5 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: If an energy bomb has been set off,
                 potentially kill this ship
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-5-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_5_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * If an energy bomb has been set off and this ship can be killed, kill it
           and increase the kill tally
    
    
    
    
     LDA BOMB               \ If we set off our energy bomb (see MA24 above), then
     BPL MA21               \ BOMB is now negative, so this skips to MA21 if our
                            \ energy bomb is not going off
    
     CPY #2*SST             \ If the ship in Y is the space station, jump to BA21
     BEQ MA21               \ as energy bombs are useless against space stations
    
     CPY #2*THG             \ If the ship in Y is a Thargoid, jump to BA21 as energy
     BEQ MA21               \ bombs have no effect against Thargoids
    
     CPY #2*CON             \ If the ship in Y is the Constrictor, jump to BA21
     BCS MA21               \ as energy bombs are useless against the Constrictor
                            \ (the Constrictor is the target of mission 1, and it
                            \ would be too easy if it could just be blown out of
                            \ the sky with a single key press)
    
     LDA INWK+31            \ If the ship we are checking has bit 5 set in its ship
     AND #%00100000         \ byte #31, then it is already exploding, so jump to
     BNE MA21               \ BA21 as ships can't explode more than once
    
     ASL INWK+31            \ The energy bomb is killing this ship, so set bit 7 of
     SEC                    \ the ship byte #31 to indicate that it has now been
     ROR INWK+31            \ killed
    
     LDX TYPE               \ Set X to the type of the ship that was killed so the
                            \ following call to EXNO2 can award us the correct
                            \ number of fractional kill points
    
     JSR EXNO2              \ Call EXNO2 to process the fact that we have killed a
                            \ ship (so increase the kill tally, make an explosion
                            \ sound and possibly display "RIGHT ON COMMANDER!")
    

[X]

Variable [BOMB](../workspace/wp.md#bomb) in workspace [WP](../workspace/wp.md)

Energy bomb

[X]

Configuration variable [CON](../../all/workspaces.md#con) = 31

Ship type for a Constrictor

[X]

Subroutine [EXNO2](exno2.md) (category: Status)

Process us making a kill

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Label [MA21](main_flight_loop_part_6_of_16.md#ma21) in subroutine [Main flight loop (Part 6 of 16)](main_flight_loop_part_6_of_16.md)

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[X]

Configuration variable [THG](../../all/workspaces.md#thg) = 29

Ship type for a Thargoid

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[Main flight loop (Part 4 of 16)](main_flight_loop_part_4_of_16.md "Previous routine")[Main flight loop (Part 6 of 16)](main_flight_loop_part_6_of_16.md "Next routine")
