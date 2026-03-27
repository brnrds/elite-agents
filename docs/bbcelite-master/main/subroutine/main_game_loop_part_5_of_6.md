---
title: "The Main game loop (Part 5 of 6) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_game_loop_part_5_of_6.html"
---

[Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md "Previous routine")[Main game loop (Part 6 of 6)](main_game_loop_part_6_of_6.md "Next routine")
    
    
           Name: Main game loop (Part 5 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Cool down lasers, make calls to update the dashboard
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-main-game-loop-part-5-of-6)
     Variations: See [code variations](../../related/compare/main/subroutine/main_game_loop_part_5_of_6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) calls via MLOOP
                 * [Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md) calls via MLOOP
                 * [Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md) calls via MLOOP
                 * [Main game loop (Part 6 of 6)](main_game_loop_part_6_of_6.md) calls via MLOOP
    
    
    
    
    
    * * *
    
    
     This is the first half of the minimal game loop, which we iterate when we are
     docked. This section covers the following:
    
       * Cool down lasers
    
       * Make calls to update the dashboard
    
    
    
    * * *
    
    
     Other entry points:
    
       MLOOP                The entry point for the main game loop. This entry point
                            comes after the call to the main flight loop and
                            spawning routines, so it marks the start of the main
                            game loop for when we are docked (as we don't need to
                            call the main flight loop or spawning routines if we
                            aren't in space)
    
    
    
    
    .MLOOP
    
     LDX #&FF               \ Set the stack pointer to &01FF, which is the standard
     TXS                    \ location for the 6502 stack, so this instruction
                            \ effectively resets the stack
    
     LDX GNTMP              \ If the laser temperature in GNTMP is non-zero,
     BEQ EE20               \ decrement it (i.e. cool it down a bit)
     DEC GNTMP
    
    .EE20
    
     LDX LASCT              \ Set X to the value of LASCT, the laser pulse count
    
     BEQ NOLASCT            \ If X = 0 then jump to NOLASCT to skip reducing LASCT,
                            \ as it can't be reduced any further
    
     DEX                    \ Decrement the value of LASCT in X
    
     BEQ P%+3               \ If X = 0, skip the next instruction
    
     DEX                    \ Decrement the value of LASCT in X again
    
     STX LASCT              \ Store the decremented value of X in LASCT, so LASCT
                            \ gets reduced by 2, but not into negative territory
    
    .NOLASCT
    
    \LDA QQ11               \ These instructions are commented out in the original
    \BNE P%+5               \ source
    
     JSR DIALS              \ Call DIALS to update the dashboard
    
     LDA QQ11               \ If this is a space view, jump to plus13 to skip the
     BEQ plus13             \ following five instructions
    
     AND PATG               \ If PATG = &FF (author names are shown on start-up)
     LSR A                  \ and bit 0 of QQ11 is 1 (the current view is type 1),
     BCS plus13             \ then skip the following two instructions
    
     LDY #2                 \ Wait for 2/50 of a second (0.04 seconds), to slow the
     JSR DELAY              \ main loop down a bit
    
    .plus13
    
     JSR TT17               \ Scan the keyboard for the cursor keys or joystick,
                            \ returning the cursor's delta values in X and Y and
                            \ the key pressed in A
    

[X]

Subroutine [DELAY](delay.md) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Subroutine [DIALS (Part 1 of 4)](dials_part_1_of_4.md) (category: Dashboard)

Update the dashboard: speed indicator

[X]

Label [EE20](main_game_loop_part_5_of_6.md#ee20) is local to this routine

[X]

Variable [GNTMP](../workspace/wp.md#gntmp) in workspace [WP](../workspace/wp.md)

Laser temperature (or "gun temperature")

[X]

Variable [LASCT](../workspace/wp.md#lasct) in workspace [WP](../workspace/wp.md)

The laser pulse count for the current laser

[X]

Label [NOLASCT](main_game_loop_part_5_of_6.md#nolasct) is local to this routine

[X]

Variable [PATG](../workspace/up.md#patg) in workspace [UP](../workspace/up.md)

Configuration setting to show the author names on the start-up screen and enable manual hyperspace mis-jumps

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Subroutine [TT17](tt17.md) (category: Keyboard)

Scan the keyboard for cursor key or joystick movement

[X]

Label [plus13](main_game_loop_part_5_of_6.md#plus13) is local to this routine

[Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md "Previous routine")[Main game loop (Part 6 of 6)](main_game_loop_part_6_of_6.md "Next routine")
