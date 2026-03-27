---
title: "The Main flight loop (Part 15 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_15_of_16.html"
---

[Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md "Previous routine")[Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 15 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Perform altitude checks with the planet and sun and process fuel
                 scooping if appropriate
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Scheduling tasks with the main loop counter](https://elite.bbcelite.com/deep_dives/scheduling_tasks_with_the_main_loop_counter.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-15-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_15_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Perform an altitude check with the planet (every 32 iterations of the main
         loop, on iteration 10 of each 32)
    
       * Perform an altitude check with the sun and process fuel scooping (every
         32 iterations of the main loop, on iteration 20 of each 32)
    
    
    
    
    .MA22
    
     LDA MJ                 \ If we are in witchspace, jump down to MA23S to skip
     BNE MA23S              \ the following, as there are no planets or suns to
                            \ bump into in witchspace
    
     LDA MCNT               \ Fetch the main loop counter and calculate MCNT mod 32,
     AND #31                \ which tells us the position of this loop in each block
                            \ of 32 iterations
    
    .MA93
    
     CMP #10                \ If this is the tenth iteration in this block of 32,
     BNE MA29               \ do the following, otherwise jump to MA29 to skip the
                            \ planet altitude check and move on to the sun distance
                            \ check
    
     LDA #50                \ If our energy bank status in ENERGY is >= 50, skip
     CMP ENERGY             \ printing the following message (so the message is
     BCC P%+6               \ only shown if our energy is low)
    
     ASL A                  \ Print recursive token 100 ("ENERGY LOW{beep}") as an
     JSR MESS               \ in-flight message
    
     LDY #&FF               \ Set our altitude in ALTIT to &FF, the maximum
     STY ALTIT
    
     INY                    \ Set Y = 0
    
     JSR m                  \ Call m to calculate the maximum distance to the
                            \ planet in any of the three axes, returned in A
    
     BNE MA23               \ If A > 0 then we are a fair distance away from the
                            \ planet in at least one axis, so jump to MA23 to skip
                            \ the rest of the altitude check
    
     JSR MAS3               \ Set A = x_hi^2 + y_hi^2 + z_hi^2, so using Pythagoras
                            \ we now know that A now contains the square of the
                            \ distance between our ship (at the origin) and the
                            \ centre of the planet at (x_hi, y_hi, z_hi)
    
     BCS MA23               \ If the C flag was set by MAS3, then the result
                            \ overflowed (was greater than &FF) and we are still a
                            \ fair distance from the planet, so jump to MA23 as we
                            \ haven't crashed into the planet
    
     SBC #36                \ Subtract 37 from x_hi^2 + y_hi^2 + z_hi^2
                            \
                            \ The SBC subtracts 37 as we just passed through a BCS
                            \ so we know the C flag is clear
                            \
                            \ When we do the 3D Pythagoras calculation, we only use
                            \ the high bytes of the coordinates, so that's x_hi,
                            \ y_hi and z_hi and
                            \
                            \ The planet radius is (0 96 0), as defined in the
                            \ PLANET routine, so the high byte is 96
                            \
                            \ When we square the coordinates above and add them,
                            \ the result gets divided by 256 (otherwise the result
                            \ wouldn't fit into one byte), so if we do the same for
                            \ the planet's radius, we get:
                            \
                            \   96 * 96 / 256 = 36
                            \
                            \ So for the planet, the equivalent figure to test the
                            \ sum of the _hi bytes against is 36, so A now contains
                            \ the high byte of our altitude above the planet
                            \ surface, squared, with an extra 1 subtracted so the
                            \ test in the next instruction will ensure we crash
                            \ even if we are exactly one planet radius away
    
     BCC MA28               \ If A < 0 then jump to MA28 as we have crashed into
                            \ the planet
    
     STA R                  \ We are getting close to the planet, so we need to
     JSR LL5                \ work out how close. We know from the above that A
                            \ contains our altitude squared, so we store A in R
                            \ and call LL5 to calculate:
                            \
                            \   Q = SQRT(R Q) = SQRT(A Q)
                            \
                            \ Interestingly, Q doesn't appear to be set to 0 for
                            \ this calculation, so presumably this doesn't make a
                            \ difference
    
     LDA Q                  \ Store the result in ALTIT, our altitude
     STA ALTIT
    
     BNE MA23               \ If our altitude is non-zero then we haven't crashed,
                            \ so jump to MA23 to skip to the next section
    
    .MA28
    
     JMP DEATH              \ If we get here then we just crashed into the planet
                            \ or got too close to the sun, so jump to DEATH to start
                            \ the funeral preparations and return from the main
                            \ flight loop using a tail call
    
    .MA29
    
     CMP #15                \ If this is the 15th iteration in this block of 32,
     BNE MA33               \ do the following, otherwise jump to MA33 to skip the
                            \ docking computer manoeuvring
    
     LDA auto               \ If auto is zero, then the docking computer is not
     BEQ MA23               \ activated, so jump to MA23 to skip to the next
                            \ section
    
     LDA #123               \ Set A = 123 and jump down to MA34 to print token 123
     BNE MA34               \ ("DOCKING COMPUTERS ON") as an in-flight message
    
    .MA33
    
     CMP #20                \ If this is the 20th iteration in this block of 32,
     BNE MA23               \ do the following, otherwise jump to MA23 to skip the
                            \ sun altitude check
    
     LDA #30                \ Set CABTMP to 30, the cabin temperature in deep space
     STA CABTMP             \ (i.e. one notch on the dashboard bar)
    
     LDA SSPR               \ If we are inside the space station safe zone, jump to
     BNE MA23               \ MA23 to skip the following, as we can't have both the
                            \ sun and space station at the same time, so we clearly
                            \ can't be flying near the sun
    
     LDY #NI%               \ Set Y to NI%, which is the offset in K% for the sun's
                            \ data block, as the second block at K% is reserved for
                            \ the sun (or space station)
    
     JSR MAS2               \ Call MAS2 to calculate the largest distance to the
     BNE MA23               \ sun in any of the three axes, and if it's non-zero,
                            \ jump to MA23 to skip the following, as we are too far
                            \ from the sun for scooping or temperature changes
    
     JSR MAS3               \ Set (A ?) = x_hi^2 + y_hi^2 + z_hi^2, so using
                            \ Pythagoras we now know that A now contains the high
                            \ byte of the square of the distance between our ship
                            \ (at the origin) and the heart of the sun at coordinate
                            \ (x_hi, y_hi, z_hi)
                            \
                            \ If the calculation overflows so it doesn't fit into
                            \ one byte, then A is set to &FF and the C flag is set
    
     EOR #%11111111         \ Invert A, so A is now small if we are far from the
                            \ sun and large if we are close to the sun, in the
                            \ range 0 = far away to &FF = extremely close, ouch,
                            \ hot, hot, hot!
    
     ADC #30                \ Add the minimum cabin temperature of 30, plus the C
                            \ flag, so we get one of the following:
                            \
                            \
                            \   * If the MAS3 calculation overflowed then we are a
                            \     long way from the sun, A will be zero and the C
                            \     flag will be set, so this addition sets A = 31
                            \     and clears the C flag
                            \
                            \   * If the result of the MAS3 calculation fitted into
                            \     one byte, then A will be in the range 0 to 255 and
                            \     the C flag will be clear, so this addition has a
                            \     result in the range 0 to 285, with the higher
                            \     values overflowing the addition and setting the
                            \     C flag
                            \
                            \ So the C flag is set if the cabin temperature is too
                            \ hot to handle, and if it's clear then A contains the
                            \ cabin temperature
    
     STA CABTMP             \ Store the updated cabin temperature
    
     BCS MA28               \ If the C flag is set then jump to MA28 to die, as
                            \ our temperature is off the scale
    
     CMP #224               \ If the cabin temperature < 224 then jump to MA23 to
     BCC MA23               \ skip fuel scooping, as we aren't close enough
    
    \CMP #&F0               \ These instructions are commented out in the original
    \BCC nokilltr           \ source
    \
    \LDA #5
    \JSR SETL1
    \
    \LDA VIC+&15
    \AND #&3
    \STA VIC+&15
    \
    \LDA #4
    \JSR SETL1
    \
    \LSR TRIBBLE+1
    \ROR TRIBBLE
    \
    \.nokilltr
    
     LDA BST                \ If we don't have fuel scoops fitted, jump to BA23 to
     BEQ MA23               \ skip fuel scooping, as we can't scoop without fuel
                            \ scoops
    
     LDA DELT4+1            \ We are now successfully fuel scooping, so it's time
     LSR A                  \ to work out how much fuel we're scooping. Fetch the
                            \ high byte of DELT4, which contains our current speed
                            \ divided by 4, and halve it to get our current speed
                            \ divided by 8 (so it's now a value between 1 and 5, as
                            \ our speed is normally between 1 and 40). This gives
                            \ us the amount of fuel that's being scooped in A, so
                            \ the faster we go, the more fuel we scoop, and because
                            \ the fuel levels are stored as 10 * the fuel in light
                            \ years, that means we just scooped between 0.1 and 0.5
                            \ light years of free fuel
    
     ADC QQ14               \ Set A = A + the current fuel level * 10 (from QQ14)
    
     CMP #70                \ If A > 70 then set A = 70 (as 70 is the maximum fuel
     BCC P%+4               \ level, or 7.0 light years)
     LDA #70
    
     STA QQ14               \ Store the updated fuel level in QQ14
    
     LDA #160               \ Set A to token 160 ("FUEL SCOOPS ON")
    
    .MA34
    
     JSR MESS               \ Print the token in A as an in-flight message
    

[X]

Variable [ALTIT](../workspace/wp.md#altit) in workspace [WP](../workspace/wp.md)

Our altitude above the surface of the planet or sun

[X]

Variable [BST](../workspace/wp.md#bst) in workspace [WP](../workspace/wp.md)

Fuel scoops (BST stands for "barrel status")

[X]

Variable [CABTMP](../workspace/wp.md#cabtmp) in workspace [WP](../workspace/wp.md)

Cabin temperature

[X]

Subroutine [DEATH](death.md) (category: Start and end)

Display the death screen

[X]

Variable [DELT4](../workspace/zp.md#delt4) in workspace [ZP](../workspace/zp.md)

Our current speed * 64 as a 16-bit value

[X]

Variable [ENERGY](../workspace/zp.md#energy) in workspace [ZP](../workspace/zp.md)

Energy bank status

[X]

Subroutine [LL5](ll5.md) (category: Maths (Arithmetic))

Calculate Q = SQRT(R Q)

[X]

Label [MA23](main_flight_loop_part_16_of_16.md#ma23) in subroutine [Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md)

[X]

Label [MA23S](main_flight_loop_part_14_of_16.md#ma23s) in subroutine [Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md)

[X]

Label [MA28](main_flight_loop_part_15_of_16.md#ma28) is local to this routine

[X]

Label [MA29](main_flight_loop_part_15_of_16.md#ma29) is local to this routine

[X]

Label [MA33](main_flight_loop_part_15_of_16.md#ma33) is local to this routine

[X]

Label [MA34](main_flight_loop_part_15_of_16.md#ma34) is local to this routine

[X]

Subroutine [MAS2](mas2.md) (category: Maths (Geometry))

Calculate a cap on the maximum distance to the planet or sun

[X]

Subroutine [MAS3](mas3.md) (category: Maths (Arithmetic))

Calculate A = x_hi^2 + y_hi^2 + z_hi^2 in the K% block

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[X]

Variable [MJ](../workspace/wp.md#mj) in workspace [WP](../workspace/wp.md)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [QQ14](../workspace/wp.md#qq14) in workspace [WP](../workspace/wp.md)

Our current fuel level (0-70)

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SSPR](../workspace/wp.md#sspr) in workspace [WP](../workspace/wp.md)

"Space station present" flag

[X]

Variable [auto](../workspace/wp.md#auto) in workspace [WP](../workspace/wp.md)

Docking computer activation status

[X]

Entry point [m](mas2.md#m) in subroutine [MAS2](mas2.md) (category: Maths (Geometry))

Do not include A in the calculation

[Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md "Previous routine")[Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md "Next routine")
