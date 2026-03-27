---
title: "The Main game loop (Part 3 of 6) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_game_loop_part_3_of_6.html"
---

[Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md "Previous routine")[Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md "Next routine")
    
    
           Name: Main game loop (Part 3 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Potentially spawn a cop, particularly if we've been bad
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-main-game-loop-part-3-of-6)
     Variations: See [code variations](../../related/compare/main/subroutine/main_game_loop_part_3_of_6.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section covers the following:
    
       * Potentially spawn a cop (in a Viper), very rarely if we have been good,
         more often if have been naughty, and very often if we have been properly
         bad
    
       * Very rarely, consider spawning a Thargoid, or vanishingly rarely, a Cougar
    
    
    
    
    .MTT1
    
     LDA SSPR               \ If we are outside the space station's safe zone, skip
     BEQ P%+5               \ the following instruction
    
    .MLOOPS
    
     JMP MLOOP              \ Jump to MLOOP to skip the following
    
     JSR BAD                \ Call BAD to work out how much illegal contraband we
                            \ are carrying in our hold (A is up to 40 for a
                            \ standard hold crammed with contraband, up to 70 for
                            \ an extended cargo hold full of narcotics and slaves)
    
     ASL A                  \ Double A to a maximum of 80 or 140
    
     LDX MANY+COPS          \ If there are no cops in the local bubble, skip the
     BEQ P%+5               \ next instruction
    
     ORA FIST               \ There are cops in the vicinity and we've got a hold
                            \ full of jail time, so OR the value in A with FIST to
                            \ get a new value that is at least as high as both
                            \ values, to reflect the fact that they have almost
                            \ certainly scanned our ship
    
     STA T                  \ Store our badness level in T
    
     JSR Ze                 \ Call Ze to initialise INWK to a fairly aggressive
                            \ ship, and set A and X to random values
                            \
                            \ Note that because Ze uses the value of X returned by
                            \ DORND, and X contains the value of A returned by the
                            \ previous call to DORND, this does not set the new ship
                            \ to a totally random location
    
     CMP #136               \ If the random number in A = 136 (0.4% chance), jump
     BEQ fothg              \ to fothg in part 4 to spawn either a Thargoid or, very
                            \ rarely, a Cougar
    
     CMP T                  \ If the random value in A >= our badness level, which
     BCS P%+7               \ will be the case unless we have been really, really
                            \ bad, then skip the following two instructions (so
                            \ if we are really bad, there's a higher chance of
                            \ spawning a cop, otherwise we got away with it, for
                            \ now)
    
     LDA #COPS              \ Add a new police ship to the local bubble
     JSR NWSHP
    
     LDA MANY+COPS          \ If we now have at least one cop in the local bubble,
     BNE MLOOPS             \ jump down to MLOOPS to stop spawning, otherwise fall
                            \ through into the next part to look at spawning
                            \ something else
    

[X]

Subroutine [BAD](bad.md) (category: Status)

Calculate how bad we have been

[X]

Configuration variable [COPS](../../all/workspaces.md#cops) = 16

Ship type for a Viper

[X]

Variable [FIST](../workspace/wp.md#fist) in workspace [WP](../workspace/wp.md)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Variable [MANY](../workspace/wp.md#many) in workspace [WP](../workspace/wp.md)

The number of ships of each type in the local bubble of universe

[X]

Entry point [MLOOP](main_game_loop_part_5_of_6.md#mloop) in subroutine [Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md) (category: Main loop)

The entry point for the main game loop. This entry point comes after the call to the main flight loop and spawning routines, so it marks the start of the main game loop for when we are docked (as we don't need to call the main flight loop or spawning routines if we aren't in space)

[X]

Label [MLOOPS](main_game_loop_part_3_of_6.md#mloops) is local to this routine

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Variable [SSPR](../workspace/wp.md#sspr) in workspace [WP](../workspace/wp.md)

"Space station present" flag

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [Ze](ze.md) (category: Universe)

Initialise the INWK workspace to a fairly aggressive ship

[X]

Label [fothg](main_game_loop_part_4_of_6.md#fothg) in subroutine [Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md)

[Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md "Previous routine")[Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md "Next routine")
