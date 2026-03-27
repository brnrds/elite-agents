---
title: "The Main flight loop (Part 16 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_16_of_16.html"
---

[Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md "Previous routine")[SPIN](spin.md "Next routine")
    
    
           Name: Main flight loop (Part 16 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Process laser pulsing, E.C.M. energy drain, call stardust routine
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-16-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_16_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Process laser pulsing
    
       * Process E.C.M. energy drain
    
       * Jump to the stardust routine if we are in a space view
    
       * Return from the main flight loop
    
    
    
    
    .MA23
    
     LDA LAS2               \ If the current view has no laser, jump to MA16 to skip
     BEQ MA16               \ the following
    
     LDA LASCT              \ If LASCT >= 8, jump to MA16 to skip the following, so
     CMP #8                 \ for a pulse laser with a LASCT between 8 and 10, the
     BCS MA16               \ laser stays on, but for a LASCT of 7 or less it gets
                            \ turned off and stays off until LASCT reaches zero and
                            \ the next pulse can start (if the fire button is still
                            \ being pressed)
                            \
                            \ For pulse lasers, LASCT gets set to 10 in ma1 above,
                            \ and it decrements every vertical sync (50 times a
                            \ second), so this means it pulses five times a second,
                            \ with the laser being on for the first 3/10 of each
                            \ pulse and off for the rest of the pulse
                            \
                            \ If this is a beam laser, LASCT is 0 so we always keep
                            \ going here. This means the laser doesn't pulse, but it
                            \ does get drawn and removed every cycle, in a slightly
                            \ different place each time, so the beams still flicker
                            \ around the screen
    
     JSR LASLI2             \ Redraw the existing laser lines, which has the effect
                            \ of removing them from the screen
    
     LDA #0                 \ Set LAS2 to 0 so if this is a pulse laser, it will
     STA LAS2               \ skip over the above until the next pulse (this has no
                            \ effect if this is a beam laser)
    
    .MA16
    
     LDA ECMP               \ If our E.C.M is not on, skip to MA69, otherwise keep
     BEQ MA69               \ going to drain some energy
    
     JSR DENGY              \ Call DENGY to deplete our energy banks by 1
    
     BEQ MA70               \ If we have no energy left, jump to MA70 to turn our
                            \ E.C.M. off
    
    .MA69
    
     LDA ECMA               \ If an E.C.M is going off (ours or an opponent's) then
     BEQ MA66               \ keep going, otherwise skip to MA66
    
     LDY #soecm             \ Call the NOISE routine with Y = 7 to make the sound of
     JSR NOISE              \ the E.C.M.
    
     DEC ECMA               \ Decrement the E.C.M. countdown timer, and if it has
     BNE MA66               \ reached zero, keep going, otherwise skip to MA66
    
    .MA70
    
     JSR ECMOF              \ If we get here then either we have either run out of
                            \ energy, or the E.C.M. timer has run down, so switch
                            \ off the E.C.M.
    
    .MA66
    
     LDA QQ11               \ If this is not a space view (i.e. QQ11 is non-zero)
     BNE oh                 \ then jump to oh to return from the main flight loop
                            \ (as oh is an RTS)
    
     JMP STARS              \ This is a space view, so jump to the STARS routine to
                            \ process the stardust, and return from the main flight
                            \ loop using a tail call
    
    \JMP PBFL               \ This instruction is commented out in the original
                            \ source
    

[X]

Subroutine [DENGY](dengy.md) (category: Flight)

Drain some energy from the energy banks

[X]

Variable [ECMA](../workspace/zp.md#ecma) in workspace [ZP](../workspace/zp.md)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Subroutine [ECMOF](ecmof.md) (category: Dashboard)

Switch off the E.C.M. and turn off the dashboard bulb

[X]

Variable [ECMP](../workspace/wp.md#ecmp) in workspace [WP](../workspace/wp.md)

Our E.C.M. status

[X]

Variable [LAS2](../workspace/wp.md#las2) in workspace [WP](../workspace/wp.md)

Laser power for the current laser

[X]

Variable [LASCT](../workspace/wp.md#lasct) in workspace [WP](../workspace/wp.md)

The laser pulse count for the current laser

[X]

Entry point [LASLI2](lasli.md#lasli2) in subroutine [LASLI](lasli.md) (category: Drawing lines)

Just draw the current laser lines without moving the centre point, draining energy or heating up. This has the effect of removing the lines from the screen

[X]

Label [MA16](main_flight_loop_part_16_of_16.md#ma16) is local to this routine

[X]

Label [MA66](main_flight_loop_part_16_of_16.md#ma66) is local to this routine

[X]

Label [MA69](main_flight_loop_part_16_of_16.md#ma69) is local to this routine

[X]

Label [MA70](main_flight_loop_part_16_of_16.md#ma70) is local to this routine

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Subroutine [STARS](stars.md) (category: Stardust)

The main routine for processing the stardust

[X]

Entry point [oh](spin.md#oh) in subroutine [SPIN](spin.md) (category: Universe)

Contains an RTS

[X]

Configuration variable [soecm](../../all/workspaces.md#soecm) = 7

Sound 7 = E.C.M. on

[Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md "Previous routine")[SPIN](spin.md "Next routine")
