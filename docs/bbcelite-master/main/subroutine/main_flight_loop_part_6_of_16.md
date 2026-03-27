---
title: "The Main flight loop (Part 6 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_6_of_16.html"
---

[Main flight loop (Part 5 of 16)](main_flight_loop_part_5_of_16.md "Previous routine")[Main flight loop (Part 7 of 16)](main_flight_loop_part_7_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 6 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Move the ship in space and copy the updated
                 INWK data block back to K%
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Program flow of the ship-moving routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_ship-moving_routine.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-6-of-16)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Move the ship in space
    
         * Copy the updated ship's data block from INWK back to K%
    
    
    
    
    .MA21
    
     JSR MVEIT              \ Call MVEIT to move the ship we are processing in space
    
                            \ Now that we are done processing this ship, we need to
                            \ copy the ship data back from INWK to the correct place
                            \ in the K% workspace. We already set INF in part 4 to
                            \ point to the ship's data block in K%, so we can simply
                            \ do the reverse of the copy we did before, this time
                            \ copying from INWK to INF
    
     LDY #NI%-1             \ Set a counter in Y so we can loop through the NI%
                            \ bytes in the ship data block
    
    .MAL3
    
     LDA INWK,Y             \ Load the Y-th byte of INWK and store it in the Y-th
     STA (INF),Y            \ byte of INF
    
     DEY                    \ Decrement the loop counter
    
     BPL MAL3               \ Loop back for the next byte, until we have copied the
                            \ last byte from INWK back to INF
    

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Label [MAL3](main_flight_loop_part_6_of_16.md#mal3) is local to this routine

[X]

Subroutine [MVEIT (Part 1 of 9)](mveit_part_1_of_9.md) (category: Moving)

Move current ship: Tidy the orientation vectors

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[Main flight loop (Part 5 of 16)](main_flight_loop_part_5_of_16.md "Previous routine")[Main flight loop (Part 7 of 16)](main_flight_loop_part_7_of_16.md "Next routine")
