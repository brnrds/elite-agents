---
title: "The Main game loop (Part 1 of 6) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_game_loop_part_1_of_6.html"
---

[DORND](dornd.md "Previous routine")[Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md "Next routine")
    
    
           Name: Main game loop (Part 1 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Spawn a trader (a Cobra Mk III, Python, Boa or Anaconda)
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-main-game-loop-part-1-of-6)
     Variations: See [code variations](../../related/compare/main/subroutine/main_game_loop_part_1_of_6.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This is part of the main game loop. This is where the core loop of the game
     lives, and it's in two parts. The shorter loop (just parts 5 and 6) is
     iterated when we are docked, while the entire loop from part 1 to 6 iterates
     if we are in space.
    
     This section covers the following:
    
       * Spawn a trader, i.e. a Cobra Mk III, Python, Boa or Anaconda, with a 50%
         chance of it having an E.C.M., a 50% chance of it docking, a random
         aggression level, a speed between 16 and 31, and a gentle clockwise roll
    
     We call this from within the main loop.
    
    
    
    
    .MTT4
    
     JSR DORND              \ Set A and X to random numbers
    
     LSR A                  \ Clear bit 7 of our random number in A and set the C
                            \ flag to bit 0 of A, which is random
    
     STA INWK+32            \ Store this in the ship's AI flag, so this ship does
                            \ not have AI
    
     STA INWK+29            \ Store A in the ship's roll counter, giving it a
                            \ clockwise roll (as bit 7 is clear), and a 1 in 127
                            \ chance of it having no damping
    
     ROL INWK+31            \ This instruction would appear to set bit 0 of the
                            \ ship's missile count randomly (as the C flag was set),
                            \ giving the ship either no missiles or one missile
                            \
                            \ However, INWK+31 is overwritten in the call to the
                            \ NWSHP routine below, where it is set to the number of
                            \ missiles from the ship blueprint, and the value of the
                            \ C flag is not used, so this instruction actually has
                            \ no effect
                            \
                            \ Interestingly, the original source code for the NWSPS
                            \ routine also has an instruction that sets INWK+31 and
                            \ which gets overwritten when it falls through into
                            \ NWSHP, but in this case the instruction is commented
                            \ out in the source. Perhaps the original version of
                            \ NWSHP didn't set the missile count and instead relied
                            \ on the calling code to set it, and when the authors
                            \ changed it, they commented out the INWK+31 instruction
                            \ in NWSPS and forgot about this one. Who knows?
    
     AND #31                \ Set the ship speed to our random number, set to a
     ORA #16                \ minimum of 16 and a maximum of 31
     STA INWK+27
    
     JSR DORND              \ Set A and X to random numbers, plus the C flag
    
     BMI nodo               \ If A is negative (50% chance), jump to nodo to skip
                            \ the following
    
                            \ If we get here then we are going to spawn a ship that
                            \ is minding its own business and trying to dock
    
     LDA INWK+32            \ Set bits 6 and 7 of A, so the ship has AI (bit 7) and
     ORA #%11000000         \ an aggression level of at least 32 out of 63 (this
     STA INWK+32            \ makes the ship more likely to turn towards its target,
                            \ which in this case is the space station, as we are
                            \ about to set the ship flags so it is docking)
    
     LDX #%00010000         \ Set bit 4 of the ship's NEWB flags, to indicate that
     STX NEWB               \ this ship is docking
    
    .nodo
    
     AND #2                 \ This reduces A to a random value of either 0 or 2
    
     ADC #CYL               \ Set A = A + C + #CYL
                            \
                            \ where A is 0 or 2 and C is 0 or 1, so this gives us a
                            \ ship type from the following: Cobra Mk III, Python,
                            \ Boa or Anaconda
    
     CMP #HER               \ If A is now the ship type of a rock hermit, jump to
     BEQ TT100              \ TT100 to skip the following instruction
    
     JSR NWSHP              \ Add a new ship of type A to the local bubble and fall
                            \ through into the main game loop again
    

[X]

Configuration variable [CYL](../../all/workspaces.md#cyl) = 11

Ship type for a Cobra Mk III

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Configuration variable [HER](../../all/workspaces.md#her) = 15

Ship type for a rock hermit (asteroid)

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Entry point [TT100](main_game_loop_part_2_of_6.md#tt100) in subroutine [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) (category: Main loop)

The entry point for the start of the main game loop, which calls the main flight loop and the moves into the spawning routine

[X]

Label [nodo](main_game_loop_part_1_of_6.md#nodo) is local to this routine

[DORND](dornd.md "Previous routine")[Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md "Next routine")
