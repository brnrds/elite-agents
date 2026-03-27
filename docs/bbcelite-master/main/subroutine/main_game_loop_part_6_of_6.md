---
title: "The Main game loop (Part 6 of 6) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_game_loop_part_6_of_6.html"
---

[Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md "Previous routine")[TT102](tt102.md "Next routine")
    
    
           Name: Main game loop (Part 6 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Process non-flight key presses (red function keys, docked keys)
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-main-game-loop-part-6-of-6)
     Variations: See [code variations](../../related/compare/main/subroutine/main_game_loop_part_6_of_6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BAY](bay.md) calls via FRCE
                 * [TT219](tt219.md) calls via FRCE
    
    
    
    
    
    * * *
    
    
     This is the second half of the minimal game loop, which we iterate when we are
     docked. This section covers the following:
    
       * Process more key presses (red function keys, docked keys etc.)
    
     It also supports joining the main loop with a key already "pressed", so we can
     jump into the main game loop to perform a specific action. In practice, this
     is used when we enter the docking bay in BAY to display Status Mode (red key
     f8), and when we finish buying or selling cargo in BAY2 to jump to the
     Inventory (red key f9).
    
    
    
    * * *
    
    
     Other entry points:
    
       FRCE                 The entry point for the main game loop if we want to
                            jump straight to a specific screen, by pretending to
                            "press" a key, in which case A contains the internal key
                            number of the key we want to "press"
    
    
    
    
    .FRCE
    
     JSR TT102              \ Call TT102 to process the key pressed in A
    
     LDA QQ12               \ Fetch the docked flag from QQ12 into A
    
     BEQ P%+5               \ If we are docked, loop back up to MLOOP just above
     JMP MLOOP              \ to restart the main loop, but skipping all the flight
                            \ and spawning code in the top part of the main loop
    
     JMP TT100              \ Otherwise jump to TT100 to restart the main loop from
                            \ the start
    

[X]

Entry point [MLOOP](main_game_loop_part_5_of_6.md#mloop) in subroutine [Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md) (category: Main loop)

The entry point for the main game loop. This entry point comes after the call to the main flight loop and spawning routines, so it marks the start of the main game loop for when we are docked (as we don't need to call the main flight loop or spawning routines if we aren't in space)

[X]

Variable [QQ12](../workspace/zp.md#qq12) in workspace [ZP](../workspace/zp.md)

Our "docked" status

[X]

Entry point [TT100](main_game_loop_part_2_of_6.md#tt100) in subroutine [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) (category: Main loop)

The entry point for the start of the main game loop, which calls the main flight loop and the moves into the spawning routine

[X]

Subroutine [TT102](tt102.md) (category: Keyboard)

Process function key, save key, hyperspace and chart key presses and update the hyperspace counter

[Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md "Previous routine")[TT102](tt102.md "Next routine")
