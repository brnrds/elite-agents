---
title: "Elite H source in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_h.html"
---

[Elite G source](elite_g.md "Previous routine")[Game data](elite_data.md "Next routine")
    
    
     ELITE H FILE
    
    
    
    
     CODE_H% = P%
    
     LOAD_H% = LOAD% + P% - CODE%
    
    
    
    
           Name: MVEIT (Part 1 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Tidy the orientation vectors
      Deep dive: [Program flow of the ship-moving routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_ship-moving_routine.html)
                 [Scheduling tasks with the main loop counter](https://elite.bbcelite.com/deep_dives/scheduling_tasks_with_the_main_loop_counter.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_1_of_9.md)
     Variations: See [code variations](../related/compare/main/subroutine/mveit_part_1_of_9.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](../main/subroutine/brief.md) calls MVEIT
                 * [ESCAPE](../main/subroutine/escape.md) calls MVEIT
                 * [Main flight loop (Part 6 of 16)](../main/subroutine/main_flight_loop_part_6_of_16.md) calls MVEIT
                 * [PAS1](../main/subroutine/pas1.md) calls MVEIT
                 * [TITLE](../main/subroutine/title.md) calls MVEIT
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Tidy the orientation vectors for one of the ship slots
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The current ship/planet/sun's data block
    
       XSAV                 The slot number of the current ship/planet/sun
    
       TYPE                 The type of the current ship/planet/sun
    
    
    
    
    .MVEIT
    
     LDA INWK+31            \ If bits 5 or 7 of ship byte #31 are set, jump to MV30
     AND #%10100000         \ as the ship is either exploding or has been killed, so
     BNE MV30               \ we don't need to tidy its orientation vectors or apply
                            \ tactics
    
     LDA MCNT               \ Fetch the main loop counter
    
     EOR XSAV               \ Fetch the slot number of the ship we are moving, EOR
     AND #15                \ with the loop counter and apply mod 15 to the result.
     BNE MV3                \ The result will be zero when "counter mod 15" matches
                            \ the slot number, so this makes sure we call TIDY 12
                            \ times every 16 main loop iterations, like this:
                            \
                            \   Iteration 0, tidy the ship in slot 0
                            \   Iteration 1, tidy the ship in slot 1
                            \   Iteration 2, tidy the ship in slot 2
                            \     ...
                            \   Iteration 11, tidy the ship in slot 11
                            \   Iteration 12, do nothing
                            \   Iteration 13, do nothing
                            \   Iteration 14, do nothing
                            \   Iteration 15, do nothing
                            \   Iteration 16, tidy the ship in slot 0
                            \     ...
                            \
                            \ and so on
    
     JSR TIDY               \ Call TIDY to tidy up the orientation vectors, to
                            \ prevent the ship from getting elongated and out of
                            \ shape due to the imprecise nature of trigonometry
                            \ in assembly language
    
    
    
    
           Name: MVEIT (Part 2 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Call tactics routine, remove ship from scanner
      Deep dive: [Scheduling tasks with the main loop counter](https://elite.bbcelite.com/deep_dives/scheduling_tasks_with_the_main_loop_counter.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_2_of_9.md)
     Variations: See [code variations](../related/compare/main/subroutine/mveit_part_2_of_9.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Apply tactics to ships with AI enabled (by calling the TACTICS routine)
    
       * Remove the ship from the scanner, so we can move it
    
    
    
    
    .MV3
    
     LDX TYPE               \ If the type of the ship we are moving is positive,
     BPL P%+5               \ i.e. it is not a planet (types 128 and 130) or sun
                            \ (type 129), then skip the following instruction
    
     JMP MV40               \ This item is the planet or sun, so jump to MV40 to
                            \ move it, which ends by jumping back into this routine
                            \ at MV45 (after all the rotation, tactics and scanner
                            \ code, which we don't need to apply to planets or suns)
    
     LDA INWK+32            \ Fetch the ship's byte #32 (AI flag) into A
    
     BPL MV30               \ If bit 7 of the AI flag is clear, then skip the
                            \ following as AI is disabled and the ship has no
                            \ tactics
    
     CPX #MSL               \ If the ship is a missile, skip straight to MV26 to
     BEQ MV26               \ call the TACTICS routine, as we do this every
                            \ iteration of the main loop for missiles only
    
     LDA MCNT               \ Fetch the main loop counter
    
     EOR XSAV               \ Fetch the slot number of the ship we are moving, EOR
     AND #7                 \ with the loop counter and apply mod 8 to the result.
     BNE MV30               \ The result will be zero when "counter mod 8" matches
                            \ the slot number mod 8, so this makes sure we call
                            \ TACTICS 12 times every 8 main loop iterations, like
                            \ this:
                            \
                            \   Iteration 0, apply tactics to slots 0 and 8
                            \   Iteration 1, apply tactics to slots 1 and 9
                            \   Iteration 2, apply tactics to slots 2 and 10
                            \   Iteration 3, apply tactics to slots 3 and 11
                            \   Iteration 4, apply tactics to slot 4
                            \   Iteration 5, apply tactics to slot 5
                            \   Iteration 6, apply tactics to slot 6
                            \   Iteration 7, apply tactics to slot 7
                            \   Iteration 8, apply tactics to slots 0 and 8
                            \     ...
                            \
                            \ and so on
    
    .MV26
    
     JSR TACTICS            \ Call TACTICS to apply AI tactics to this ship
    
    .MV30
    
     JSR SCAN               \ Draw the ship on the scanner, which has the effect of
                            \ removing it, as it's already at this point and hasn't
                            \ yet moved
    
    
    
    
           Name: MVEIT (Part 3 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Move ship forward according to its speed
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_3_of_9.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Move the ship forward (along the vector pointing in the direction of
         travel) according to its speed:
    
         (x, y, z) += nosev_hi * speed / 64
    
    
    
    
     LDA INWK+27            \ Set Q = the ship's speed byte #27 * 4
     ASL A
     ASL A
     STA Q
    
     LDA INWK+10            \ Set A = |nosev_x_hi|
     AND #%01111111
    
     JSR FMLTU              \ Set R = A * Q / 256
     STA R                  \       = |nosev_x_hi| * speed / 64
    
     LDA INWK+10            \ If nosev_x_hi is positive, then:
     LDX #0                 \
     JSR MVT1-2             \   (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + R
                            \
                            \ If nosev_x_hi is negative, then:
                            \
                            \   (x_sign x_hi x_lo) = (x_sign x_hi x_lo) - R
                            \
                            \ So in effect, this does:
                            \
                            \   (x_sign x_hi x_lo) += nosev_x_hi * speed / 64
    
     LDA INWK+12            \ Set A = |nosev_y_hi|
     AND #%01111111
    
     JSR FMLTU              \ Set R = A * Q / 256
     STA R                  \       = |nosev_y_hi| * speed / 64
    
     LDA INWK+12            \ If nosev_y_hi is positive, then:
     LDX #3                 \
     JSR MVT1-2             \   (y_sign y_hi y_lo) = (y_sign y_hi y_lo) + R
                            \
                            \ If nosev_y_hi is negative, then:
                            \
                            \   (y_sign y_hi y_lo) = (y_sign y_hi y_lo) - R
                            \
                            \ So in effect, this does:
                            \
                            \   (y_sign y_hi y_lo) += nosev_y_hi * speed / 64
    
     LDA INWK+14            \ Set A = |nosev_z_hi|
     AND #%01111111
    
     JSR FMLTU              \ Set R = A * Q / 256
     STA R                  \       = |nosev_z_hi| * speed / 64
    
     LDA INWK+14            \ If nosev_y_hi is positive, then:
     LDX #6                 \
     JSR MVT1-2             \   (z_sign z_hi z_lo) = (z_sign z_hi z_lo) + R
                            \
                            \ If nosev_z_hi is negative, then:
                            \
                            \   (z_sign z_hi z_lo) = (z_sign z_hi z_lo) - R
                            \
                            \ So in effect, this does:
                            \
                            \   (z_sign z_hi z_lo) += nosev_z_hi * speed / 64
    
    
    
    
           Name: MVEIT (Part 4 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Apply acceleration to ship's speed as a one-off
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_4_of_9.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Apply acceleration to the ship's speed (if acceleration is non-zero),
         and then zero the acceleration as it's a one-off change
    
    
    
    
     LDA INWK+27            \ Set A = the ship's speed in byte #24 + the ship's
     CLC                    \ acceleration in byte #28
     ADC INWK+28
    
     BPL P%+4               \ If the result is positive, skip the following
                            \ instruction
    
     LDA #0                 \ Set A to 0 to stop the speed from going negative
    
     LDY #15                \ We now fetch byte #15 from the ship's blueprint, which
                            \ contains the ship's maximum speed, so set Y = 15 to
                            \ use as an index
    
     CMP (XX0),Y            \ If A < the ship's maximum speed, skip the following
     BCC P%+4               \ instruction
    
     LDA (XX0),Y            \ Set A to the ship's maximum speed
    
     STA INWK+27            \ We have now calculated the new ship's speed after
                            \ accelerating and keeping the speed within the ship's
                            \ limits, so store the updated speed in byte #27
    
     LDA #0                 \ We have added the ship's acceleration, so we now set
     STA INWK+28            \ it back to 0 in byte #28, as it's a one-off change
    
    
    
    
           Name: MVEIT (Part 5 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Rotate ship's location by our pitch and roll
      Deep dive: [Rotating the universe](https://elite.bbcelite.com/deep_dives/rotating_the_universe.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_5_of_9.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Rotate the ship's location in space by the amount of pitch and roll of
         our ship. See below for a deeper explanation of this routine
    
    
    
    
     LDX ALP1               \ Fetch the magnitude of the current roll into X, so
                            \ if the roll angle is alpha, X contains |alpha|
    
     LDA INWK               \ Set P = ~x_lo (i.e. with all its bits flipped) so that
     EOR #%11111111         \ we can pass x_lo to MLTU2 below)
     STA P
    
     LDA INWK+1             \ Set A = x_hi
    
     JSR MLTU2-2            \ Set (A P+1 P) = (A ~P) * X
                            \               = (x_hi x_lo) * alpha
    
     STA P+2                \ Store the high byte of the result in P+2, so we now
                            \ have:
                            \
                            \ P(2 1 0) = (x_hi x_lo) * alpha
    
     LDA ALP2+1             \ Fetch the flipped sign of the current roll angle alpha
     EOR INWK+2             \ from ALP2+1 and EOR with byte #2 (x_sign), so if the
                            \ flipped roll angle and x_sign have the same sign, A
                            \ will be positive, else it will be negative. So A will
                            \ contain the sign bit of x_sign * flipped alpha sign,
                            \ which is the opposite to the sign of the above result,
                            \ so we now have:
                            \
                            \ (A P+2 P+1) = - (x_sign x_hi x_lo) * alpha / 256
    
     LDX #3                 \ Set (A P+2 P+1) = (y_sign y_hi y_lo) + (A P+2 P+1)
     JSR MVT6               \                 = y - x * alpha / 256
    
     STA K2+3               \ Set K2(3) = A = the sign of the result
    
     LDA P+1                \ Set K2(1) = P+1, the low byte of the result
     STA K2+1
    
     EOR #%11111111         \ Set P = ~K2+1 (i.e. with all its bits flipped) so
     STA P                  \ that we can pass K2+1 to MLTU2 below)
    
     LDA P+2                \ Set K2(2) = A = P+2
     STA K2+2
    
                            \ So we now have result 1 above:
                            \
                            \ K2(3 2 1) = (A P+2 P+1)
                            \           = y - x * alpha / 256
    
     LDX BET1               \ Fetch the magnitude of the current pitch into X, so
                            \ if the pitch angle is beta, X contains |beta|
    
     JSR MLTU2-2            \ Set (A P+1 P) = (A ~P) * X
                            \               = K2(2 1) * beta
    
     STA P+2                \ Store the high byte of the result in P+2, so we now
                            \ have:
                            \
                            \ P(2 1 0) = K2(2 1) * beta
    
     LDA K2+3               \ Fetch the sign of the above result in K(3 2 1) from
     EOR BET2               \ K2+3 and EOR with BET2, the sign of the current pitch
                            \ rate, so if the pitch and K(3 2 1) have the same sign,
                            \ A will be positive, else it will be negative. So A
                            \ will contain the sign bit of K(3 2 1) * beta, which is
                            \ the same as the sign of the above result, so we now
                            \ have:
                            \
                            \ (A P+2 P+1) = K2(3 2 1) * beta / 256
    
     LDX #6                 \ Set (A P+2 P+1) = (z_sign z_hi z_lo) + (A P+2 P+1)
     JSR MVT6               \                 = z + K2 * beta / 256
    
     STA INWK+8             \ Set z_sign = A = the sign of the result
    
     LDA P+1                \ Set z_lo = P+1, the low byte of the result
     STA INWK+6
    
     EOR #%11111111         \ Set P = ~z_lo (i.e. with all its bits flipped) so that
     STA P                  \ we can pass z_lo to MLTU2 below)
    
     LDA P+2                \ Set z_hi = P+2
     STA INWK+7
    
                            \ So we now have result 2 above:
                            \
                            \ (z_sign z_hi z_lo) = (A P+2 P+1)
                            \                    = z + K2 * beta / 256
    
     JSR MLTU2              \ MLTU2 doesn't change Q, and Q was set to beta in
                            \ the previous call to MLTU2, so this call does:
                            \
                            \ (A P+1 P) = (A ~P) * Q
                            \           = (z_hi z_lo) * beta
    
     STA P+2                \ Set P+2 = A = the high byte of the result, so we
                            \ now have:
                            \
                            \ P(2 1 0) = (z_hi z_lo) * beta
    
     LDA K2+3               \ Set y_sign = K2+3
     STA INWK+5
    
     EOR BET2               \ EOR y_sign with BET2, the sign of the current pitch
     EOR INWK+8             \ rate, and z_sign. If the result is positive jump to
     BPL MV43               \ MV43, otherwise this means beta * z and y have
                            \ different signs, i.e. P(2 1) and K2(3 2 1) have
                            \ different signs, so we need to add them in order to
                            \ calculate K2(2 1) - P(2 1)
    
     LDA P+1                \ Set (y_hi y_lo) = K2(2 1) + P(2 1)
     ADC K2+1
     STA INWK+3
     LDA P+2
     ADC K2+2
     STA INWK+4
    
     JMP MV44               \ Jump to MV44 to continue the calculation
    
    .MV43
    
     LDA K2+1               \ Reversing the logic above, we need to subtract P(2 1)
     SBC P+1                \ and K2(3 2 1) to calculate K2(2 1) - P(2 1), so this
     STA INWK+3             \ sets (y_hi y_lo) = K2(2 1) - P(2 1)
     LDA K2+2
     SBC P+2
     STA INWK+4
    
     BCS MV44               \ If the above subtraction did not underflow, then
                            \ jump to MV44, otherwise we need to negate the result
    
     LDA #1                 \ Negate (y_sign y_hi y_lo) using two's complement,
     SBC INWK+3             \ first doing the low bytes:
     STA INWK+3             \
                            \ y_lo = 1 - y_lo
    
     LDA #0                 \ Then the high bytes:
     SBC INWK+4             \
     STA INWK+4             \ y_hi = 0 - y_hi
    
     LDA INWK+5             \ And finally flip the sign in y_sign
     EOR #%10000000
     STA INWK+5
    
    .MV44
    
                            \ So we now have result 3 above:
                            \
                            \ (y_sign y_hi y_lo) = K2(2 1) - P(2 1)
                            \                    = K2 - beta * z
    
     LDX ALP1               \ Fetch the magnitude of the current roll into X, so
                            \ if the roll angle is alpha, X contains |alpha|
    
     LDA INWK+3             \ Set P = ~y_lo (i.e. with all its bits flipped) so that
     EOR #&FF               \ we can pass y_lo to MLTU2 below)
     STA P
    
     LDA INWK+4             \ Set A = y_hi
    
     JSR MLTU2-2            \ Set (A P+1 P) = (A ~P) * X
                            \               = (y_hi y_lo) * alpha
    
     STA P+2                \ Store the high byte of the result in P+2, so we now
                            \ have:
                            \
                            \ P(2 1 0) = (y_hi y_lo) * alpha
    
     LDA ALP2               \ Fetch the correct sign of the current roll angle alpha
     EOR INWK+5             \ from ALP2 and EOR with byte #5 (y_sign), so if the
                            \ correct roll angle and y_sign have the same sign, A
                            \ will be positive, else it will be negative. So A will
                            \ contain the sign bit of x_sign * correct alpha sign,
                            \ which is the same as the sign of the above result,
                            \ so we now have:
                            \
                            \ (A P+2 P+1) = (y_sign y_hi y_lo) * alpha / 256
    
     LDX #0                 \ Set (A P+2 P+1) = (x_sign x_hi x_lo) + (A P+2 P+1)
     JSR MVT6               \                 = x + y * alpha / 256
    
     STA INWK+2             \ Set x_sign = A = the sign of the result
    
     LDA P+2                \ Set x_hi = P+2, the high byte of the result
     STA INWK+1
    
     LDA P+1                \ Set x_lo = P+1, the low byte of the result
     STA INWK
    
                            \ So we now have result 4 above:
                            \
                            \ x = x + alpha * y
                            \
                            \ and the rotation of (x, y, z) is done
    
    
    
    
           Name: MVEIT (Part 6 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Move the ship in space according to our speed
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_6_of_9.md)
     Variations: See [code variations](../related/compare/main/subroutine/mveit_part_6_of_9.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MV40](../main/subroutine/mv40.md) calls via MV45
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Move the ship in space according to our speed (we already moved it
         according to its own speed in part 3).
    
     We do this by subtracting our speed (i.e. the distance we travel in this
     iteration of the loop) from the other ship's z-coordinate. We subtract because
     they appear to be "moving" in the opposite direction to us, and the whole
     MVEIT routine is about moving the other ships rather than us (even though we
     are the one doing the moving).
    
    
    
    * * *
    
    
     Other entry points:
    
       MV45                 Rejoin the MVEIT routine after the rotation, tactics and
                            scanner code
    
    
    
    
    .MV45
    
     LDA DELTA              \ Set R to our speed in DELTA
     STA R
    
     LDA #%10000000         \ Set A to zeroes but with bit 7 set, so that (A R) is
                            \ a 16-bit number containing -R, or -speed
    
     LDX #6                 \ Set X to the z-axis so the call to MVT1 does this:
     JSR MVT1               \
                            \ (z_sign z_hi z_lo) = (z_sign z_hi z_lo) + (A R)
                            \                    = (z_sign z_hi z_lo) - speed
    
     LDA TYPE               \ If the ship type is not the sun (129) then skip the
     AND #%10000001         \ next instruction, otherwise return from the subroutine
     CMP #129               \ as we don't need to rotate the sun around its origin.
     BNE P%+3               \ Having both the AND and the CMP is a little odd, as
                            \ the sun is the only ship type with bits 0 and 7 set,
                            \ so the AND has no effect and could be removed
    
     RTS                    \ Return from the subroutine, as the ship we are moving
                            \ is the sun and doesn't need any of the following
    
    
    
    
           Name: MVEIT (Part 7 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Rotate ship's orientation vectors by pitch/roll
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
                 [Pitching and rolling](https://elite.bbcelite.com/deep_dives/pitching_and_rolling.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_7_of_9.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Rotate the ship's orientation vectors according to our pitch and roll
    
     As with the previous step, this is all about moving the other ships rather
     than us (even though we are the one doing the moving). So we rotate the
     current ship's orientation vectors (which defines its orientation in space),
     by the angles we are "moving" the rest of the sky through (alpha and beta, our
     roll and pitch), so the ship appears to us to be stationary while we rotate.
    
    
    
    
     LDY #9                 \ Apply our pitch and roll rotations to the current
     JSR MVS4               \ ship's nosev vector
    
     LDY #15                \ Apply our pitch and roll rotations to the current
     JSR MVS4               \ ship's roofv vector
    
     LDY #21                \ Apply our pitch and roll rotations to the current
     JSR MVS4               \ ship's sidev vector
    
    
    
    
           Name: MVEIT (Part 8 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Rotate ship about itself by its own pitch/roll
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
                 [Pitching and rolling by a fixed angle](https://elite.bbcelite.com/deep_dives/pitching_and_rolling_by_a_fixed_angle.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_8_of_9.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * If the ship we are processing is rolling or pitching itself, rotate it and
         apply damping if required
    
    
    
    
     LDA INWK+30            \ Fetch the ship's pitch counter and extract the sign
     AND #%10000000         \ into RAT2
     STA RAT2
    
     LDA INWK+30            \ Fetch the ship's pitch counter and extract the value
     AND #%01111111         \ without the sign bit into A
    
     BEQ MV8                \ If the pitch counter is 0, then jump to MV8 to skip
                            \ the following, as the ship is not pitching
    
     CMP #%01111111         \ If bits 0-6 are set in the pitch counter (i.e. the
                            \ ship's pitch is not damping down), then the C flag
                            \ will be set by this instruction
    
     SBC #0                 \ Set A = A - 0 - (1 - C), so if we are damping then we
                            \ reduce A by 1, otherwise it is unchanged
    
     ORA RAT2               \ Change bit 7 of A to the sign we saved in RAT2, so
                            \ the updated pitch counter in A retains its sign
    
     STA INWK+30            \ Store the updated pitch counter in byte #30
    
     LDX #15                \ Rotate (roofv_x, nosev_x) by a small angle (pitch)
     LDY #9
     JSR MVS5
    
     LDX #17                \ Rotate (roofv_y, nosev_y) by a small angle (pitch)
     LDY #11
     JSR MVS5
    
     LDX #19                \ Rotate (roofv_z, nosev_z) by a small angle (pitch)
     LDY #13
     JSR MVS5
    
    .MV8
    
     LDA INWK+29            \ Fetch the ship's roll counter and extract the sign
     AND #%10000000         \ into RAT2
     STA RAT2
    
     LDA INWK+29            \ Fetch the ship's roll counter and extract the value
     AND #%01111111         \ without the sign bit into A
    
     BEQ MV5                \ If the roll counter is 0, then jump to MV5 to skip the
                            \ following, as the ship is not rolling
    
     CMP #%01111111         \ If bits 0-6 are set in the roll counter (i.e. the
                            \ ship's roll is not damping down), then the C flag
                            \ will be set by this instruction
    
     SBC #0                 \ Set A = A - 0 - (1 - C), so if we are damping then we
                            \ reduce A by 1, otherwise it is unchanged
    
     ORA RAT2               \ Change bit 7 of A to the sign we saved in RAT2, so
                            \ the updated roll counter in A retains its sign
    
     STA INWK+29            \ Store the updated pitch counter in byte #29
    
     LDX #15                \ Rotate (roofv_x, sidev_x) by a small angle (roll)
     LDY #21
     JSR MVS5
    
     LDX #17                \ Rotate (roofv_y, sidev_y) by a small angle (roll)
     LDY #23
     JSR MVS5
    
     LDX #19                \ Rotate (roofv_z, sidev_z) by a small angle (roll)
     LDY #25
     JSR MVS5
    
    
    
    
           Name: MVEIT (Part 9 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Redraw on scanner, if it hasn't been destroyed
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mveit_part_9_of_9.md)
     Variations: See [code variations](../related/compare/main/subroutine/mveit_part_9_of_9.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * If the ship is exploding or being removed, hide it on the scanner
    
       * Otherwise redraw the ship on the scanner, now that it's been moved
    
    
    
    
    .MV5
    
     LDA INWK+31            \ Fetch the ship's exploding/killed state from byte #31
    
     AND #%10100000         \ If we are exploding or removing this ship then jump to
     BNE MVD1               \ MVD1 to remove it from the scanner permanently
    
     LDA INWK+31            \ Set bit 4 to keep the ship visible on the scanner
     ORA #%00010000
     STA INWK+31
    
     JMP SCAN               \ Display the ship on the scanner, returning from the
                            \ subroutine using a tail call
    
    .MVD1
    
     LDA INWK+31            \ Clear bit 4 to hide the ship on the scanner
     AND #%11101111
     STA INWK+31
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MVT1                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Calculate (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (A R)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mvt1.md)
     References: This subroutine is called as follows:
                 * [MVEIT (Part 6 of 9)](../main/subroutine/mveit_part_6_of_9.md) calls MVT1
                 * [SFS2](../main/subroutine/sfs2.md) calls MVT1
                 * [MVEIT (Part 3 of 9)](../main/subroutine/mveit_part_3_of_9.md) calls via MVT1-2
    
    
    
    
    
    * * *
    
    
     Add the signed delta (A R) to a ship's coordinate, along the axis given in X.
     Mathematically speaking, this routine translates the ship along a single axis
     by a signed delta. Taking the example of X = 0, the x-axis, it does the
     following:
    
       (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (A R)
    
     (In practice, MVT1 is only ever called directly with A = 0 or 128, otherwise
     it is always called via MVT-2, which clears A apart from the sign bit. The
     routine is written to cope with a non-zero delta_hi, so it supports a full
     16-bit delta, but it appears that delta_hi is only ever used to hold the
     sign of the delta.)
    
     The comments below assume we are adding delta to the x-axis, though the axis
     is determined by the value of X.
    
    
    
    * * *
    
    
     Arguments:
    
       (A R)                The signed delta, so A = delta_hi and R = delta_lo
    
       X                    Determines which coordinate axis of INWK to change:
    
                              * X = 0 adds the delta to (x_lo, x_hi, x_sign)
    
                              * X = 3 adds the delta to (y_lo, y_hi, y_sign)
    
                              * X = 6 adds the delta to (z_lo, z_hi, z_sign)
    
    
    
    * * *
    
    
     Other entry points:
    
       MVT1-2               Clear bits 0-6 of A before entering MVT1
    
    
    
    
     AND #%10000000         \ Clear bits 0-6 of A
    
    .MVT1
    
     ASL A                  \ Set the C flag to the sign bit of the delta, leaving
                            \ delta_hi << 1 in A
    
     STA S                  \ Set S = delta_hi << 1
                            \
                            \ This also clears bit 0 of S
    
     LDA #0                 \ Set T = just the sign bit of delta (in bit 7)
     ROR A
     STA T
    
     LSR S                  \ Set S = delta_hi >> 1
                            \       = |delta_hi|
                            \
                            \ This also clear the C flag, as we know that bit 0 of
                            \ S was clear before the LSR
    
     EOR INWK+2,X           \ If T EOR x_sign has bit 7 set, then x_sign and delta
     BMI MV10               \ have different signs, so jump to MV10
    
                            \ At this point, we know x_sign and delta have the same
                            \ sign, that sign is in T, and S contains |delta_hi|,
                            \ so now we want to do:
                            \
                            \   (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (S R)
                            \
                            \ and then set the sign of the result to the same sign
                            \ as x_sign and delta
    
     LDA R                  \ First we add the low bytes, so:
     ADC INWK,X             \
     STA INWK,X             \   x_lo = x_lo + R
    
     LDA S                  \ Then we add the high bytes:
     ADC INWK+1,X           \
     STA INWK+1,X           \   x_hi = x_hi + S
    
     LDA INWK+2,X           \ And finally we add any carry into x_sign, and if the
     ADC #0                 \ sign of x_sign and delta in T is negative, make sure
     ORA T                  \ the result is negative (by OR'ing with T)
     STA INWK+2,X
    
     RTS                    \ Return from the subroutine
    
    .MV10
    
                            \ If we get here, we know x_sign and delta have
                            \ different signs, with delta's sign in T, and
                            \ |delta_hi| in S, so now we want to do:
                            \
                            \   (x_sign x_hi x_lo) = (x_sign x_hi x_lo) - (S R)
                            \
                            \ and then set the sign of the result according to
                            \ the signs of x_sign and delta
    
     LDA INWK,X             \ First we subtract the low bytes, so:
     SEC                    \
     SBC R                  \   x_lo = x_lo - R
     STA INWK,X
    
     LDA INWK+1,X           \ Then we subtract the high bytes:
     SBC S                  \
     STA INWK+1,X           \   x_hi = x_hi - S
    
     LDA INWK+2,X           \ And finally we subtract any borrow from bits 0-6 of
     AND #%01111111         \ x_sign, and give the result the opposite sign bit to T
     SBC #0                 \ (i.e. give it the sign of the original x_sign)
     ORA #%10000000
     EOR T
     STA INWK+2,X
    
     BCS MV11               \ If the C flag is set by the above SBC, then our sum
                            \ above didn't underflow and is correct - to put it
                            \ another way, (x_sign x_hi x_lo) >= (S R) so the result
                            \ should indeed have the same sign as x_sign, so jump to
                            \ MV11 to return from the subroutine
    
                            \ Otherwise our subtraction underflowed because
                            \ (x_sign x_hi x_lo) < (S R), so we now need to flip the
                            \ subtraction around by using two's complement to this:
                            \
                            \   (S R) - (x_sign x_hi x_lo)
                            \
                            \ and then we need to give the result the same sign as
                            \ (S R), the delta, as that's the dominant figure in the
                            \ sum
    
     LDA #1                 \ First we subtract the low bytes, so:
     SBC INWK,X             \
     STA INWK,X             \   x_lo = 1 - x_lo
    
     LDA #0                 \ Then we subtract the high bytes:
     SBC INWK+1,X           \
     STA INWK+1,X           \   x_hi = 0 - x_hi
    
     LDA #0                 \ And then we subtract the sign bytes:
     SBC INWK+2,X           \
                            \   x_sign = 0 - x_sign
    
     AND #%01111111         \ Finally, we set the sign bit to the sign in T, the
     ORA T                  \ sign of the original delta, as the delta is the
     STA INWK+2,X           \ dominant figure in the sum
    
    .MV11
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MVS4                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Apply pitch and roll to an orientation vector
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
                 [Pitching and rolling](https://elite.bbcelite.com/deep_dives/pitching_and_rolling.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mvs4.md)
     References: This subroutine is called as follows:
                 * [MVEIT (Part 7 of 9)](../main/subroutine/mveit_part_7_of_9.md) calls MVS4
    
    
    
    
    
    * * *
    
    
     Apply pitch and roll angles alpha and beta to the orientation vector in Y.
    
     Specifically, this routine rotates a point (x, y, z) around the origin by
     pitch alpha and roll beta, using the small angle approximation to make the
     maths easier, and incorporating the Minsky circle algorithm to make the
     rotation more stable (though more elliptic).
    
     If that paragraph makes sense to you, then you should probably be writing
     this commentary! For the rest of us, see the associated deep dives.
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    Determines which of the INWK orientation vectors to
                            transform:
    
                              * Y = 9 rotates nosev: (nosev_x, nosev_y, nosev_z)
    
                              * Y = 15 rotates roofv: (roofv_x, roofv_y, roofv_z)
    
                              * Y = 21 rotates sidev: (sidev_x, sidev_y, sidev_z)
    
    
    
    
    .MVS4
    
     LDA ALPHA              \ Set Q = alpha (the roll angle to rotate through)
     STA Q
    
     LDX INWK+2,Y           \ Set (S R) = nosev_y
     STX R
     LDX INWK+3,Y
     STX S
    
     LDX INWK,Y             \ These instructions have no effect as MAD overwrites
     STX P                  \ X and P when called, but they set X = P = nosev_x_lo
    
     LDA INWK+1,Y           \ Set A = -nosev_x_hi
     EOR #%10000000
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
     STA INWK+3,Y           \           = alpha * -nosev_x_hi + nosev_y
     STX INWK+2,Y           \
                            \ and store (A X) in nosev_y, so this does:
                            \
                            \ nosev_y = nosev_y - alpha * nosev_x_hi
    
     STX P                  \ This instruction has no effect as MAD overwrites P,
                            \ but it sets P = nosev_y_lo
    
     LDX INWK,Y             \ Set (S R) = nosev_x
     STX R
     LDX INWK+1,Y
     STX S
    
     LDA INWK+3,Y           \ Set A = nosev_y_hi
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
     STA INWK+1,Y           \           = alpha * nosev_y_hi + nosev_x
     STX INWK,Y             \
                            \ and store (A X) in nosev_x, so this does:
                            \
                            \ nosev_x = nosev_x + alpha * nosev_y_hi
    
     STX P                  \ This instruction has no effect as MAD overwrites P,
                            \ but it sets P = nosev_x_lo
    
     LDA BETA               \ Set Q = beta (the pitch angle to rotate through)
     STA Q
    
     LDX INWK+2,Y           \ Set (S R) = nosev_y
     STX R
     LDX INWK+3,Y
     STX S
     LDX INWK+4,Y
    
     STX P                  \ This instruction has no effect as MAD overwrites P,
                            \ but it sets P = nosev_y
    
     LDA INWK+5,Y           \ Set A = -nosev_z_hi
     EOR #%10000000
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
     STA INWK+3,Y           \           = beta * -nosev_z_hi + nosev_y
     STX INWK+2,Y           \
                            \ and store (A X) in nosev_y, so this does:
                            \
                            \ nosev_y = nosev_y - beta * nosev_z_hi
    
     STX P                  \ This instruction has no effect as MAD overwrites P,
                            \ but it sets P = nosev_y_lo
    
     LDX INWK+4,Y           \ Set (S R) = nosev_z
     STX R
     LDX INWK+5,Y
     STX S
    
     LDA INWK+3,Y           \ Set A = nosev_y_hi
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
     STA INWK+5,Y           \           = beta * nosev_y_hi + nosev_z
     STX INWK+4,Y           \
                            \ and store (A X) in nosev_z, so this does:
                            \
                            \ nosev_z = nosev_z + beta * nosev_y_hi
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MVT6                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Calculate (A P+2 P+1) = (x_sign x_hi x_lo) + (A P+2 P+1)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mvt6.md)
     References: This subroutine is called as follows:
                 * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md) calls MVT6
    
    
    
    
    
    * * *
    
    
     Do the following calculation, for the coordinate given by X (so this is what
     it does for the x-coordinate):
    
       (A P+2 P+1) = (x_sign x_hi x_lo) + (A P+2 P+1)
    
     A is a sign bit and is not included in the calculation, but bits 0-6 of A are
     preserved. Bit 7 is set to the sign of the result.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The sign of P(2 1) in bit 7
    
       P(2 1)               The 16-bit value we want to add the coordinate to
    
       X                    The coordinate to add, as follows:
    
                              * If X = 0, add to (x_sign x_hi x_lo)
    
                              * If X = 3, add to (y_sign y_hi y_lo)
    
                              * If X = 6, add to (z_sign z_hi z_lo)
    
    
    
    * * *
    
    
     Returns:
    
       A                    The sign of the result (in bit 7)
    
    
    
    
    .MVT6
    
     TAY                    \ Store argument A into Y, for later use
    
     EOR INWK+2,X           \ Set A = A EOR x_sign
    
     BMI MV50               \ If the sign is negative, i.e. A and x_sign have
                            \ different signs, jump to MV50
    
                            \ The signs are the same, so we can add the two
                            \ arguments and keep the sign to get the result
    
     LDA P+1                \ First we add the low bytes:
     CLC                    \
     ADC INWK,X             \   P+1 = P+1 + x_lo
     STA P+1
    
     LDA P+2                \ And then the high bytes:
     ADC INWK+1,X           \
     STA P+2                \   P+2 = P+2 + x_hi
    
     TYA                    \ Restore the original A argument that we stored earlier
                            \ so that we keep the original sign
    
     RTS                    \ Return from the subroutine
    
    .MV50
    
     LDA INWK,X             \ First we subtract the low bytes:
     SEC                    \
     SBC P+1                \   P+1 = x_lo - P+1
     STA P+1
    
     LDA INWK+1,X           \ And then the high bytes:
     SBC P+2                \
     STA P+2                \   P+2 = x_hi - P+2
    
     BCC MV51               \ If the last subtraction underflowed, then the C flag
                            \ will be clear and x_hi < P+2, so jump to MV51 to
                            \ negate the result
    
     TYA                    \ Restore the original A argument that we stored earlier
     EOR #%10000000         \ but flip bit 7, which flips the sign. We do this
                            \ because x_hi >= P+2 so we want the result to have the
                            \ same sign as x_hi (as it's the dominant side in this
                            \ calculation). The sign of x_hi is x_sign, and x_sign
                            \ has the opposite sign to A, so we flip the sign in A
                            \ to return the correct result
    
     RTS                    \ Return from the subroutine
    
    .MV51
    
     LDA #1                 \ Our subtraction underflowed, so we negate the result
     SBC P+1                \ using two's complement, first with the low byte:
     STA P+1                \
                            \   P+1 = 1 - P+1
    
     LDA #0                 \ And then the high byte:
     SBC P+2                \
     STA P+2                \   P+2 = 0 - P+2
    
     TYA                    \ Restore the original A argument that we stored earlier
                            \ as this is the correct sign for the result. This is
                            \ because x_hi < P+2, so we want to return the same sign
                            \ as P+2, the dominant side
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MV40                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Rotate the planet or sun's location in space by the amount of
                 pitch and roll of our ship
      Deep dive: [Rotating the universe](https://elite.bbcelite.com/deep_dives/rotating_the_universe.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mv40.md)
     Variations: See [code variations](../related/compare/main/subroutine/mv40.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md) calls MV40
    
    
    
    
    
    * * *
    
    
     We implement this using the same equations as in part 5 of MVEIT, where we
     rotated the current ship's location by our pitch and roll. Specifically, the
     calculation is as follows:
    
       1. K2 = y - alpha * x
       2. z = z + beta * K2
       3. y = K2 - beta * z
       4. x = x + alpha * y
    
    
    
    
    .MV40
    
     LDA ALPHA              \ Set Q = -ALPHA, so Q contains the angle we want to
     EOR #%10000000         \ roll the planet through (i.e. in the opposite
     STA Q                  \ direction to our ship's roll angle alpha)
    
     LDA INWK               \ Set P(1 0) = (x_hi x_lo)
     STA P
     LDA INWK+1
     STA P+1
    
     LDA INWK+2             \ Set A = x_sign
    
     JSR MULT3              \ Set K(3 2 1 0) = (A P+1 P) * Q
                            \
                            \ which also means:
                            \
                            \   K(3 2 1) = (A P+1 P) * Q / 256
                            \            = x * -alpha / 256
                            \            = - alpha * x / 256
    
     LDX #3                 \ Set K(3 2 1) = (y_sign y_hi y_lo) + K(3 2 1)
     JSR MVT3               \              = y - alpha * x / 256
    
     LDA K+1                \ Set K2(2 1) = P(1 0) = K(2 1)
     STA K2+1
     STA P
    
     LDA K+2                \ Set K2+2 = K+2
     STA K2+2
    
     STA P+1                \ Set P+1 = K+2
    
     LDA BETA               \ Set Q = beta, the pitch angle of our ship
     STA Q
    
     LDA K+3                \ Set K+3 to K2+3, so now we have result 1 above:
     STA K2+3               \
                            \   K2(3 2 1) = K(3 2 1)
                            \             = y - alpha * x / 256
    
                            \ We also have:
                            \
                            \   A = K+3
                            \
                            \   P(1 0) = K(2 1)
                            \
                            \ so combined, these mean:
                            \
                            \   (A P+1 P) = K(3 2 1)
                            \             = K2(3 2 1)
    
     JSR MULT3              \ Set K(3 2 1 0) = (A P+1 P) * Q
                            \
                            \ which also means:
                            \
                            \   K(3 2 1) = (A P+1 P) * Q / 256
                            \            = K2(3 2 1) * beta / 256
                            \            = beta * K2 / 256
    
     LDX #6                 \ K(3 2 1) = (z_sign z_hi z_lo) + K(3 2 1)
     JSR MVT3               \          = z + beta * K2 / 256
    
     LDA K+1                \ Set P = K+1
     STA P
    
     STA INWK+6             \ Set z_lo = K+1
    
     LDA K+2                \ Set P+1 = K+2
     STA P+1
    
     STA INWK+7             \ Set z_hi = K+2
    
     LDA K+3                \ Set A = z_sign = K+3, so now we have:
     STA INWK+8             \
                            \   (z_sign z_hi z_lo) = K(3 2 1)
                            \                      = z + beta * K2 / 256
    
                            \ So we now have result 2 above:
                            \
                            \   z = z + beta * K2
    
     EOR #%10000000         \ Flip the sign bit of A to give A = -z_sign
    
     JSR MULT3              \ Set K(3 2 1 0) = (A P+1 P) * Q
                            \                = (-z_sign z_hi z_lo) * beta
                            \                = -z * beta
    
     LDA K+3                \ Set T to the sign bit of K(3 2 1 0), i.e. to the sign
     AND #%10000000         \ bit of -z * beta
     STA T
    
     EOR K2+3               \ If K2(3 2 1 0) has a different sign to K(3 2 1 0),
     BMI MV1                \ then EOR'ing them will produce a 1 in bit 7, so jump
                            \ to MV1 to take this into account
    
                            \ If we get here, K and K2 have the same sign, so we can
                            \ add them together to get the result we're after, and
                            \ then set the sign afterwards
    
     LDA K                  \ We now do the following sum:
     CLC                    \
     ADC K2                 \   (A y_hi y_lo -) = K(3 2 1 0) + K2(3 2 1 0)
                            \
                            \ starting with the low bytes (which we don't keep)
                            \
                            \ The CLC has no effect because MULT3 clears the C
                            \ flag, so this instruction could be removed (as it is
                            \ in the cassette version, for example)
    
     LDA K+1                \ We then do the middle bytes, which go into y_lo
     ADC K2+1
     STA INWK+3
    
     LDA K+2                \ And then the high bytes, which go into y_hi
     ADC K2+2
     STA INWK+4
    
     LDA K+3                \ And then the sign bytes into A, so overall we have the
     ADC K2+3               \ following, if we drop the low bytes from the result:
                            \
                            \   (A y_hi y_lo) = (K + K2) / 256
    
     JMP MV2                \ Jump to MV2 to skip the calculation for when K and K2
                            \ have different signs
    
    .MV1
    
     LDA K                  \ If we get here then K2 and K have different signs, so
     SEC                    \ instead of adding, we need to subtract to get the
     SBC K2                 \ result we want, like this:
                            \
                            \   (A y_hi y_lo -) = K(3 2 1 0) - K2(3 2 1 0)
                            \
                            \ starting with the low bytes (which we don't keep)
    
     LDA K+1                \ We then do the middle bytes, which go into y_lo
     SBC K2+1
     STA INWK+3
    
     LDA K+2                \ And then the high bytes, which go into y_hi
     SBC K2+2
     STA INWK+4
    
     LDA K2+3               \ Now for the sign bytes, so first we extract the sign
     AND #%01111111         \ byte from K2 without the sign bit, so P = |K2+3|
     STA P
    
     LDA K+3                \ And then we extract the sign byte from K without the
     AND #%01111111         \ sign bit, so A = |K+3|
    
     SBC P                  \ And finally we subtract the sign bytes, so P = A - P
     STA P
    
                            \ By now we have the following, if we drop the low bytes
                            \ from the result:
                            \
                            \   (A y_hi y_lo) = (K - K2) / 256
                            \
                            \ so now we just need to make sure the sign of the
                            \ result is correct
    
     BCS MV2                \ If the C flag is set, then the last subtraction above
                            \ didn't underflow and the result is correct, so jump to
                            \ MV2 as we are done with this particular stage
    
     LDA #1                 \ Otherwise the subtraction above underflowed, as K2 is
     SBC INWK+3             \ the dominant part of the subtraction, so we need to
     STA INWK+3             \ negate the result using two's complement, starting
                            \ with the low bytes:
                            \
                            \   y_lo = 1 - y_lo
    
     LDA #0                 \ And then the high bytes:
     SBC INWK+4             \
     STA INWK+4             \   y_hi = 0 - y_hi
    
     LDA #0                 \ And finally the sign bytes:
     SBC P                  \
                            \   A = 0 - P
    
     ORA #%10000000         \ We now force the sign bit to be negative, so that the
                            \ final result below gets the opposite sign to K, which
                            \ we want as K2 is the dominant part of the sum
    
    .MV2
    
     EOR T                  \ T contains the sign bit of K, so if K is negative,
                            \ this flips the sign of A
    
     STA INWK+5             \ Store A in y_sign
    
                            \ So we now have result 3 above:
                            \
                            \   y = K2 + K
                            \     = K2 - beta * z
    
     LDA ALPHA              \ Set A = alpha
     STA Q
    
     LDA INWK+3             \ Set P(1 0) = (y_hi y_lo)
     STA P
     LDA INWK+4
     STA P+1
    
     LDA INWK+5             \ Set A = y_sign
    
     JSR MULT3              \ Set K(3 2 1 0) = (A P+1 P) * Q
                            \                = (y_sign y_hi y_lo) * alpha
                            \                = y * alpha
    
     LDX #0                 \ Set K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)
     JSR MVT3               \              = x + y * alpha / 256
    
     LDA K+1                \ Set (x_sign x_hi x_lo) = K(3 2 1)
     STA INWK               \                        = x + y * alpha / 256
     LDA K+2
     STA INWK+1
     LDA K+3
     STA INWK+2
    
                            \ So we now have result 4 above:
                            \
                            \   x = x + y * alpha
    
     JMP MV45               \ We have now finished rotating the planet or sun by
                            \ our pitch and roll, so jump back into the MVEIT
                            \ routine at MV45 to apply all the other movements
    
    
    
    
           Name: PLUT                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Flip the coordinate axes for the four different views
      Deep dive: [Flipping axes between space views](https://elite.bbcelite.com/deep_dives/flipping_axes_between_space_views.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/plut.md)
     Variations: See [code variations](../related/compare/main/subroutine/plut-pu1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls PLUT
    
    
    
    
    
    * * *
    
    
     This routine flips the relevant geometric axes in INWK depending on which
     view we are looking through (front, rear, left, right).
    
    
    
    
    .PLUT
    
     LDX VIEW               \ Load the current view into X:
                            \
                            \   0 = front
                            \   1 = rear
                            \   2 = left
                            \   3 = right
    
     BEQ PU2-1              \ If the current view is the front view, return from the
                            \ subroutine (PU2-1 contains an RTS), as the geometry in
                            \ INWK is already correct
    
    .PU1
    
     DEX                    \ Decrement the view, so now:
                            \
                            \   0 = rear
                            \   1 = left
                            \   2 = right
    
     BNE PU2                \ If the current view is left or right, jump to PU2,
                            \ otherwise this is the rear view, so continue on
    
     LDA INWK+2             \ Flip the sign of x_sign
     EOR #%10000000
     STA INWK+2
    
     LDA INWK+8             \ Flip the sign of z_sign
     EOR #%10000000
     STA INWK+8
    
     LDA INWK+10            \ Flip the sign of nosev_x_hi
     EOR #%10000000
     STA INWK+10
    
     LDA INWK+14            \ Flip the sign of nosev_z_hi
     EOR #%10000000
     STA INWK+14
    
     LDA INWK+16            \ Flip the sign of roofv_x_hi
     EOR #%10000000
     STA INWK+16
    
     LDA INWK+20            \ Flip the sign of roofv_z_hi
     EOR #%10000000
     STA INWK+20
    
     LDA INWK+22            \ Flip the sign of sidev_x_hi
     EOR #%10000000
     STA INWK+22
    
     LDA INWK+26            \ Flip the sign of roofv_z_hi
     EOR #%10000000
     STA INWK+26
    
     RTS                    \ Return from the subroutine
    
    .PU2
    
                            \ We enter this with X set to the view, as follows:
                            \
                            \   1 = left
                            \   2 = right
    
     LDA #0                 \ Set RAT2 = 0 (left view) or -1 (right view)
     CPX #2
     ROR A
     STA RAT2
    
     EOR #%10000000         \ Set RAT = -1 (left view) or 0 (right view)
     STA RAT
    
     LDA INWK               \ Swap x_lo and z_lo
     LDX INWK+6
     STA INWK+6
     STX INWK
    
     LDA INWK+1             \ Swap x_hi and z_hi
     LDX INWK+7
     STA INWK+7
     STX INWK+1
    
     LDA INWK+2             \ Swap x_sign and z_sign
     EOR RAT                \ If left view, flip sign of new z_sign
     TAX                    \ If right view, flip sign of new x_sign
     LDA INWK+8
     EOR RAT2
     STA INWK+2
     STX INWK+8
    
     LDY #9                 \ Swap nosev_x_lo and nosev_z_lo
     JSR PUS1               \ Swap nosev_x_hi and nosev_z_hi
                            \ If left view, flip sign of new nosev_z_hi
                            \ If right view, flip sign of new nosev_x_hi
    
     LDY #15                \ Swap roofv_x_lo and roofv_z_lo
     JSR PUS1               \ Swap roofv_x_hi and roofv_z_hi
                            \ If left view, flip sign of new roofv_z_hi
                            \ If right view, flip sign of new roofv_x_hi
    
     LDY #21                \ Swap sidev_x_lo and sidev_z_lo
                            \ Swap sidev_x_hi and sidev_z_hi
                            \ If left view, flip sign of new sidev_z_hi
                            \ If right view, flip sign of new sidev_x_hi
    
    .PUS1
    
     LDA INWK,Y             \ Swap the low x and z bytes for the vector in Y:
     LDX INWK+4,Y           \
     STA INWK+4,Y           \   * For Y =  9 swap nosev_x_lo and nosev_z_lo
     STX INWK,Y             \   * For Y = 15 swap roofv_x_lo and roofv_z_lo
                            \   * For Y = 21 swap sidev_x_lo and sidev_z_lo
    
     LDA INWK+1,Y           \ Swap the high x and z bytes for the offset in Y:
     EOR RAT                \
     TAX                    \   * If left view, flip sign of new z-coordinate
     LDA INWK+5,Y           \   * If right view, flip sign of new x-coordinate
     EOR RAT2
     STA INWK+1,Y
     STX INWK+5,Y
    
                            \ Fall through into LOOK1 to return from the subroutine
    
    
    
    
           Name: LOOK1                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Initialise the space view
    
    
        Context: See this subroutine [on its own page](../main/subroutine/look1.md)
     Variations: See [code variations](../related/compare/main/subroutine/look1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MJP](../main/subroutine/mjp.md) calls LOOK1
                 * [TT102](../main/subroutine/tt102.md) calls LOOK1
                 * [TT110](../main/subroutine/tt110.md) calls LOOK1
                 * [WARP](../main/subroutine/warp.md) calls LOOK1
                 * [SIGHT](../main/subroutine/sight.md) calls via LO2
    
    
    
    
    
    * * *
    
    
     Initialise the space view, with the direction of view given in X. This clears
     the upper screen and draws the laser crosshairs, if the view in X has lasers
     fitted. It also wipes all the ships from the scanner, so we can recalculate
     ship positions for the new view (they get put back in the main flight loop).
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The space view to set:
    
                              * 0 = front
                              * 1 = rear
                              * 2 = left
                              * 3 = right
    
    
    
    * * *
    
    
     Other entry points:
    
       LO2                  Contains an RTS
    
    
    
    
    .LO2
    
     RTS                    \ Return from the subroutine
    
    .LQ
    
     STX VIEW               \ Set the current space view to X
    
     JSR TT66               \ Clear the top part of the screen, draw a border box,
                            \ and set the current view type in QQ11 to 0 (space
                            \ view)
    
     JSR SIGHT              \ Draw the laser crosshairs
    
     LDA BOMB               \ If our energy bomb has been set off, then BOMB will be
     BPL P%+5               \ negative, so this skips the following instruction if
                            \ our energy bomb is not going off
    
     JSR BOMBOFF            \ Our energy bomb is going off, so call BOMBOFF to draw
                            \ the zig-zag lightning bolt
    
     JMP NWSTARS            \ Set up a new stardust field and return from the
                            \ subroutine using a tail call
    
    .LOOK1
    
     LDA #0                 \ Set A = 0, the type number of a space view
    
     JSR DOVDU19            \ Switch to the mode 1 palette for the space view,
                            \ which is yellow (colour 1), red (colour 2) and cyan
                            \ (colour 3)
    
     LDY QQ11               \ If the current view is not a space view, jump up to LQ
     BNE LQ                 \ to set up a new space view
    
     CPX VIEW               \ If the current view is already of type X, jump to LO2
     BEQ LO2                \ to return from the subroutine (as LO2 contains an RTS)
    
     STX VIEW               \ Change the current space view to X
    
     JSR TT66               \ Clear the top part of the screen, draw a border box,
                            \ and set the current view type in QQ11 to 0 (space
                            \ view)
    
     JSR FLIP               \ Swap the x- and y-coordinates of all the stardust
                            \ particles and redraw the stardust field
    
     LDA BOMB               \ If our energy bomb has been set off, then BOMB will be
     BPL P%+5               \ negative, so this skips the following instruction if
                            \ our energy bomb is not going off
    
     JSR BOMBOFF            \ Our energy bomb is going off, so call BOMBOFF to draw
                            \ the zig-zag lightning bolt
    
     JSR WPSHPS             \ Wipe all the ships from the scanner and mark them all
                            \ as not being shown on-screen
    
                            \ And fall through into SIGHT to draw the laser
                            \ crosshairs
    
    
    
    
           Name: SIGHT                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Draw the laser crosshairs
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sight.md)
     Variations: See [code variations](../related/compare/main/subroutine/sight.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOOK1](../main/subroutine/look1.md) calls SIGHT
    
    
    
    
    
    
    .SIGHT
    
     LDY VIEW               \ Fetch the laser power for our new view
     LDA LASER,Y
    
     BEQ LO2                \ If it is zero (i.e. there is no laser fitted to this
                            \ view), jump to LO2 to return from the subroutine (as
                            \ LO2 contains an RTS)
    
     LDY #0                 \ Set Y to 0, to represent a pulse laser
    
     CMP #POW               \ If the laser power in A is equal to a pulse laser,
     BEQ SIG1               \ jump to SIG1 with Y = 0
    
     INY                    \ Increment Y to 1, to represent a beam laser
    
     CMP #POW+128           \ If the laser power in A is equal to a beam laser,
     BEQ SIG1               \ jump to SIG1 with Y = 1
    
     INY                    \ Increment Y to 2, to represent a military laser
    
     CMP #Armlas            \ If the laser power in A is equal to a military laser,
     BEQ SIG1               \ jump to SIG1 with Y = 2
    
     INY                    \ Increment Y to 3, to represent a mining laser
    
    .SIG1
    
     LDA sightcol,Y         \ Set the colour from the sightcol table
     STA COL
    
     LDA #128               \ Set QQ19 to the x-coordinate of the centre of the
     STA QQ19               \ screen
    
     LDA #Y-24              \ Set QQ19+1 to the y-coordinate of the centre of the
     STA QQ19+1             \ screen, minus 24 (because TT15 will add 24 to the
                            \ coordinate when it draws the crosshairs)
    
     LDA #20                \ Set QQ19+2 to size 20 for the crosshairs size
     STA QQ19+2
    
     JSR TT15               \ Call TT15 to draw crosshairs of size 20 just to the
                            \ left of the middle of the screen
    
     LDA #10                \ Set QQ19+2 to size 10 for the crosshairs size
     STA QQ19+2
    
     JMP TT15               \ Call TT15 to draw crosshairs of size 10 at the same
                            \ location, which will remove the centre part from the
                            \ laser crosshairs, leaving a gap in the middle, and
                            \ return from the subroutine using a tail call
    
    
    
    
           Name: sightcol                                                [Show more]
           Type: Variable
       Category: Drawing lines
        Summary: Colours for the crosshair sights on the different laser types
    
    
        Context: See this variable [on its own page](../main/variable/sightcol.md)
     References: This variable is used as follows:
                 * [SIGHT](../main/subroutine/sight.md) uses sightcol
    
    
    
    
    
    
    .sightcol
    
     EQUB YELLOW            \ Pulse lasers have yellow sights
    
     EQUB CYAN              \ Beam lasers have cyan sights
    
     EQUB CYAN              \ Military lasers have cyan sights
    
     EQUB YELLOW            \ Mining lasers have yellow sights
    
    
    
    
           Name: beamcol                                                 [Show more]
           Type: Variable
       Category: Drawing lines
        Summary: An unused table of laser colours
    
    
        Context: See this variable [on its own page](../main/variable/beamcol.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .beamcol
    
     EQUB WHITE             \ These bytes appear to be unused - perhaps they were
     EQUB WHITE             \ going to be used to set different colours of laser
     EQUB WHITE             \ beam for the different lasers?
     EQUB WHITE
    
    
    
    
           Name: TRIBTA                                                  [Show more]
           Type: Variable
       Category: Missions
        Summary: A table for converting the number of Trumbles in the hold into a
                 number of sprites in the range 0 to 6
    
    
        Context: See this variable [on its own page](../main/variable/tribta.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    \.TRIBTA                \ These instructions are commented out in the original
    \                       \ source
    \EQUB 0
    \EQUB 1
    \EQUB 2
    \EQUB 3
    \EQUB 4
    \EQUB 5
    \EQUB 6
    \EQUB 6
    
    
    
    
           Name: TRIBMA                                                  [Show more]
           Type: Variable
       Category: Missions
        Summary: A table for converting the number of Trumbles in the hold into a
                 sprite-enable flag to use with VIC register &15
    
    
        Context: See this variable [on its own page](../main/variable/tribma.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    \.TRIBMA                \ These instructions are commented out in the original
    \                       \ source
    \EQUB 0
    \EQUB 4
    \EQUB &C
    \EQUB &1C
    \EQUB &3C
    \EQUB &7C
    \EQUB &FC
    \EQUB &FC
    
    
    
    
           Name: TT66                                                    [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the screen and set the current view type
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt66.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt66.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](../main/subroutine/brief.md) calls TT66
                 * [DEATH](../main/subroutine/death.md) calls TT66
                 * [HALL](../main/subroutine/hall.md) calls TT66
                 * [HFS2](../main/subroutine/hfs2.md) calls TT66
                 * [LOOK1](../main/subroutine/look1.md) calls TT66
                 * [MJP](../main/subroutine/mjp.md) calls TT66
                 * [MT9](../main/subroutine/mt9.md) calls TT66
                 * [PAUSE](../main/subroutine/pause.md) calls TT66
                 * [qv](../main/subroutine/qv.md) calls TT66
                 * [TITLE](../main/subroutine/title.md) calls TT66
                 * [TRADEMODE](../main/subroutine/trademode.md) calls TT66
                 * [TT18](../main/subroutine/tt18.md) calls TT66
                 * [TT22](../main/subroutine/tt22.md) calls TT66
                 * [TT23](../main/subroutine/tt23.md) calls TT66
    
    
    
    
    
    * * *
    
    
     Clear the top part of the screen, draw a border box, and set the current
     view type in QQ11 to A.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The type of the new current view (see QQ11 for a list of
                            view types)
    
    
    
    
    .TT66
    
     STA QQ11               \ Set the current view type in QQ11 to A
    
                            \ Fall through into TTX66K to clear the screen and draw
                            \ a border box
    
    
    
    
           Name: TTX66K                                                  [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the top part of the screen, draw a border box and configure
                 the specified view
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ttx66k.md)
     Variations: See [code variations](../related/compare/main/subroutine/ttx66-ttx66k.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Clear the top part of the screen (the space view) and draw a border box
     along the top and sides.
    
    
    
    
    .TTX66K
    
     JSR TTX66              \ Call TTX66 to clear the top part of the screen and
                            \ draw a border box
    
     JSR MT2                \ Switch to Sentence Case when printing extended tokens
    
     LDA #0                 \ Reset the ball line heap pointer at LSP
     STA LSP
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch to Sentence Case
     STA QQ17
    
     STA DTW2               \ Set bit 7 of DTW2 to indicate we are not currently
                            \ printing a word
    
     JSR FLFLLS             \ Call FLFLLS to reset the LSO block
    
     LDA #0                 \ Set LAS2 = 0 to stop any laser pulsing
     STA LAS2
    
     STA DLY                \ Set the delay in DLY to 0, to indicate that we are
                            \ no longer showing an in-flight message, so any new
                            \ in-flight messages will be shown instantly
    
     STA de                 \ Clear de, the flag that appends " DESTROYED" to the
                            \ end of the next text token, so that it doesn't
    
     LDX QQ22+1             \ Fetch into X the number that's shown on-screen during
                            \ the hyperspace countdown
    
     BEQ P%+5               \ If the counter is zero then we are not counting down
                            \ to hyperspace, so skip the next instruction
    
     JSR ee3                \ Print the 8-bit number in X at text location (0, 1),
                            \ i.e. print the hyperspace countdown in the top-left
                            \ corner
    
     LDA QQ11               \ If this is not a space view, jump to tt66 to skip
     BNE tt66               \ displaying the view name
    
     LDA #11                \ Move the text cursor to row 11
     STA XC
    
     LDA #CYAN              \ Switch to colour 3, which is cyan in the space view
     STA COL
    
     LDA VIEW               \ Load the current view into A:
                            \
                            \   0 = front
                            \   1 = rear
                            \   2 = left
                            \   3 = right
    
     ORA #&60               \ OR with &60 so we get a value of &60 to &63 (96 to 99)
    
     JSR TT27               \ Print recursive token 96 to 99, which will be in the
                            \ range "FRONT" to "RIGHT"
    
     JSR TT162              \ Print a space
    
     LDA #175               \ Print recursive token 15 ("VIEW ")
     JSR TT27
    
    .tt66
    
     LDX #0                 \ Set QQ17 = 0 to switch to ALL CAPS
     STX QQ17
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TRTB%                                                   [Show more]
           Type: Variable
       Category: Keyboard
        Summary: Translation table from internal key number to ASCII
    
    
        Context: See this variable [on its own page](../main/variable/trtb_per_cent.md)
     Variations: See [code variations](../related/compare/main/variable/trantable-trtb_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [RDKEY](../main/subroutine/rdkey.md) uses TRTB%
    
    
    
    
    
    * * *
    
    
     This is a copy of the keyboard translation table from the BBC Master's MOS
     3.20 ROM. The value at offset n is the upper-case ASCII value of the key with
     internal key number n, so for example the value at offset &10 is &51, which is
     81, or ASCII "Q", so internal key number &10 is the key number of the "Q"
     key.
    
     Valid internal key numbers are Binary Coded Decimal (BCD) numbers in the range
     &10 top &79, so they're in the ranges &10 to &19, then &20 to &29, then &30 to
     &39, and so on. This means that the other locations - i.e. &1A to &1F, &2A to
     &2F and so on - aren't used by the lookup table, but the MOS doesn't let this
     space go to waste; instead, those gaps contain MOS code, which is replicated
     below as the table contains a copy of this entire block of the MOS, not just
     the table entries.
    
     This table allows code running on the parasite to convert internal key numbers
     into ASCII codes in an efficient way. Without this table we would have to do a
     lookup from the table in the I/O processor's MOS ROM, which we would have to
     access from across the Tube, and this would be a lot slower than doing a
     simple table lookup in the parasite's user RAM.
    
    
    
    
    .TRTB%
    
     EQUB &00, &40, &FE     \ MOS code
     EQUB &A0, &5F, &8C
     EQUB &43, &FE, &8E
     EQUB &4F, &FE, &EA
     EQUB &AE, &4F, &FE
     EQUB &60
    
                            \ Internal key numbers &10 to &19:
                            \
     EQUB &51, &33          \ Q             3
     EQUB &34, &35          \ 4             5
     EQUB &84, &38          \ f4            8
     EQUB &87, &2D          \ f7            -
     EQUB &5E, &8C          \ ^             Left arrow
    
     EQUB &36, &37, &BC     \ MOS code
     EQUB &00, &FC, &60
    
                            \ Internal key numbers &20 to &29:
                            \
     EQUB &80, &57          \ f0            W
     EQUB &45, &54          \ E             T
     EQUB &37, &49          \ 7             I
     EQUB &39, &30          \ 9             0
     EQUB &5F, &8E          \ _             Down arrow
    
     EQUB &38, &39, &BC     \ MOS code
     EQUB &00, &FD, &60
    
                            \ Internal key numbers &30 to &39:
                            \
     EQUB &31, &32          \ 1             2
     EQUB &44, &52          \ D             R
     EQUB &36, &55          \ 6             U
     EQUB &4F, &50          \ O             P
     EQUB &5B, &8F          \ [             Up arrow
    
     EQUB &81, &82, &0D     \ MOS code
     EQUB &4C, &20, &02
    
                            \ Internal key numbers &40 to &49:
                            \
     EQUB &01, &41          \ CAPS LOCK     A
     EQUB &58, &46          \ X             F
     EQUB &59, &4A          \ Y             J
     EQUB &4B, &40          \ K             @
     EQUB &3A, &0D          \ :             RETURN
    
     EQUB &83, &7F, &AE     \ MOS code
     EQUB &4C, &FE, &FD
    
                            \ Internal key numbers &50 to &59:
                            \
     EQUB &02, &53          \ SHIFT LOCK    S
     EQUB &43, &47          \ C             G
     EQUB &48, &4E          \ H             N
     EQUB &4C, &3B          \ L             ;
     EQUB &5D, &7F          \ ]             DELETE
    
     EQUB &85, &84, &86     \ MOS code
     EQUB &4C, &FA, &00
    
                            \ Internal key numbers &60 to &69:
                            \
     EQUB &00, &5A          \ TAB           Z
     EQUB &20, &56          \ Space         V
     EQUB &42, &4D          \ B             M
     EQUB &2C, &2E          \ ,             .
     EQUB &2F, &8B          \ /             COPY
    
     EQUB &30, &31, &33     \ MOS code
     EQUB &00, &00, &00
    
                            \ Internal key numbers &70 to &79:
                            \
     EQUB &1B, &81          \ ESCAPE        f1
     EQUB &82, &83          \ f2            f3
     EQUB &85, &86          \ f5            f6
     EQUB &88, &89          \ f8            f9
     EQUB &5C, &8D          \ \             Right arrow
    
     EQUB &34, &35, &32     \ MOS code
     EQUB &2C, &4E, &E3
    
    
    
    
           Name: IKNS                                                    [Show more]
           Type: Variable
       Category: Keyboard
        Summary: Lookup table for in-flight keyboard controls
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this variable [on its own page](../main/variable/ikns.md)
     Variations: See [code variations](../related/compare/main/variable/kytb-ikns.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [FILLKL](../main/subroutine/fillkl.md) uses IKNS
    
    
    
    
    
    * * *
    
    
     Keyboard table for in-flight controls. This table contains the internal key
     codes for the flight keys, EOR'd with &FF to invert each bit.
    
    
    
    
    .IKNS
    
     EQUB ⅅ EOR &FF       \ E         IKNS+0    KY13     E.C.M.
     EQUB &DC EOR &FF       \ T         IKNS+1    KY10     Arm missile
     EQUB &CA EOR &FF       \ U         IKNS+2    KY11     Unarm missile
     EQUB &C8 EOR &FF       \ P         IKNS+3    KY16     Cancel docking computer
     EQUB &BE EOR &FF       \ A         IKNS+4    KY7      Fire lasers
     EQUB &BD EOR &FF       \ X         IKNS+5    KY5      Pull up
     EQUB &BA EOR &FF       \ J         IKNS+6    KY14     In-system jump
     EQUB &AE EOR &FF       \ S         IKNS+7    KY6      Pitch down
     EQUB &AD EOR &FF       \ C         IKNS+8    KY15     Docking computer
     EQUB &9F EOR &FF       \ TAB       IKNS+9    KY8      Energy bomb
     EQUB &9D EOR &FF       \ Space     IKNS+10   KY2      Speed up
     EQUB &9A EOR &FF       \ M         IKNS+11   KY12     Fire missile
     EQUB &99 EOR &FF       \ <         IKNS+12   KY3      Roll left
     EQUB &98 EOR &FF       \ >         IKNS+13   KY4      Roll right
     EQUB &97 EOR &FF       \ ?         IKNS+14   KY1      Slow down
     EQUB &8F EOR &FF       \ ESCAPE    IKNS+15   KY9      Launch escape pod
    
     EQUB &F0               \ This value just has to be higher than &80 to act as a
                            \ terminator for the IKNS matching process in FILLKL
    
    
    
    
           Name: FILLKL                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard for a flight key and update the key logger
    
    
        Context: See this subroutine [on its own page](../main/subroutine/fillkl.md)
     References: This subroutine is called as follows:
                 * [RDKEY](../main/subroutine/rdkey.md) calls FILLKL
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    If a key is being pressed, X contains the internal key
                            number, otherwise it contains 0
    
    
    
    
    .FILLKL
    
     JSR ZEKTRAN            \ Call ZEKTRAN to clear the key logger, which also sets
                            \ X to 0 (so we can use X as an index for working our
                            \ way through the flight keys in Rd2 below)
    
     LDA #16                \ Start the scan with internal key number 16 ("Q")
    
     CLC                    \ Clear the C flag so we can do the additions below
    
    .Rd1
    
     LDY #%00000011         \ Set Y to %00000011, so it's ready to send to SHEILA
                            \ once interrupts have been disabled
    
     SEI                    \ Disable interrupts so we can scan the keyboard
                            \ without being hijacked
    
     STY VIA+&40            \ Set 6522 System VIA output register ORB (SHEILA &40)
                            \ to %00000011 to stop auto scan of keyboard
    
     LDY #%01111111         \ Set 6522 System VIA data direction register DDRA
     STY VIA+&43            \ (SHEILA &43) to %01111111. This sets the A registers
                            \ (IRA and ORA) so that:
                            \
                            \   * Bits 0-6 of ORA will be sent to the keyboard
                            \
                            \   * Bit 7 of IRA will be read from the keyboard
    
     STA VIA+&4F            \ Set 6522 System VIA output register ORA (SHEILA &4F)
                            \ to A, the key we want to scan for; bits 0-6 will be
                            \ sent to the keyboard, of which bits 0-3 determine the
                            \ keyboard column, and bits 4-6 the keyboard row
    
     LDY VIA+&4F            \ Read 6522 System VIA output register IRA (SHEILA &4F)
                            \ into Y; bit 7 is the only bit that will have changed.
                            \ If the key is pressed, then bit 7 will be set,
                            \ otherwise it will be clear
    
     LDA #%00001011         \ Set 6522 System VIA output register ORB (SHEILA &40)
     STA VIA+&40            \ to %00001011 to restart auto scan of keyboard
    
     CLI                    \ Allow interrupts again
    
     TYA                    \ Transfer Y into A
    
     BMI Rd2                \ If the key was pressed then Y is negative, so jump to
                            \ Rd2
    
    .Rd3
    
     ADC #1                 \ Increment A to point to the next key to scan for
    
     BPL Rd1                \ If A is positive, we still have keys to check, so loop
                            \ back to scan for the next one
    
                            \ If we get here then no keys are being pressed
    
     CLD                    \ Clear the D flag to return to binary mode (for cases
                            \ where this routine is called in BCD mode)
    
     LDA KY6                \ If KY6 ("S") is being pressed, then KY6 is &FF, and
     EOR #&FF               \ KY6 EOR &FF AND KY19 will be zero, so this stops "C"
     AND KY19               \ from being registered as being pressed if "S" is being
     STA KY19               \ pressed. This code isn't necessary on a BBC Master, as
                            \ it's from other versions of Elite where there were
                            \ issues with "ghost keys", which the Master didn't have
    
     LDA KL                 \ Fetch the last key pressed from KL
    
     TAX                    \ Copy the key value into X
    
     RTS                    \ Return from the subroutine
    
    .Rd2
    
                            \ If we get here then the key in A is being pressed. We
                            \ now work our way through the IKNS table, looking for
                            \ a match against the flight keys, using X as an index
                            \ into the table (X was initialised to 0 by the call to
                            \ ZEKTRAN above, and it keeps track of our progress
                            \ through the table between calls to Rd2)
    
     EOR #%10000000         \ The key in A is being pressed and the number of the
                            \ key is in A with bit 7 set, so flip bit 7 back to 0
    
     STA KL                 \ Store the number of the key pressed in KL
    
                            \ Now to scan the IKNS table for a possible match for
                            \ the pressed key (if we get a match we update the key
                            \ logger, as IKNS contains the flight keys that have
                            \ key logger entries. Because we are scanning the
                            \ keyboard by incrementing the key to check in A, and
                            \ the IKNS table is sorted in increasing order, we don't
                            \ need to scan the whole IKNS table each time for a
                            \ match, we can just check against the next key in the
                            \ table, as pointed to by X
    
    .Rd5
    
     CMP IKNS,X             \ If A is less than the X-th byte in IKNS, jump back to
     BCC Rd3                \ Rd3 to continue scanning for more keys, as this key
                            \ isn't in the IKNS table and doesn't have an entry in
                            \ the key logger
    
     BEQ Rd4                \ If A is equal to the X-th byte in IKNS, we have a
                            \ match, so skip the next two instructions to go to the
                            \ part where we update the key logger
    
     INX                    \ The pressed key is higher than the IKNS entry at X, so
                            \ increment X to point to the next key in the table
    
     BNE Rd5                \ And loop back to Rd5 to test against this next flight
                            \ key in IKNS (this BNE is effectively a JMP as X won't
                            \ get high enough to wrap around to zero)
    
                            \ If we get here, the pressed key has an entry in the
                            \ key logger, so now to update the logger
    
    .Rd4
    
     DEC KY17,X             \ We got a match in the IKNS table for our key at
                            \ position X, so we decrement the corresponding key
                            \ logger byte for this key at KY17+X (KY17 is the first
                            \ key in the key logger)
    
     INX                    \ Increment X so next time we check IKNS from the next
                            \ key in the table
    
     CLC                    \ Clear the C flag so we can do more additions
    
     BCC Rd3                \ Jump back to Rd3 to continue scanning for more keys
                            \ (this BCC is effectively a JMP as we just cleared the
                            \ C flag)
    
    
    
    
           Name: CTRL                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard to see if CTRL is currently pressed
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ctrl.md)
     Variations: See [code variations](../related/compare/main/subroutine/ctrl.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hyp](../main/subroutine/hyp.md) calls CTRL
                 * [TT18](../main/subroutine/tt18.md) calls CTRL
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X = %10000001 (i.e. 129 or -127) if CTRL is being
                            pressed
    
                            X = 1 if CTRL is not being pressed
    
       A                    Contains the same as X
    
    
    
    
    IF _SNG47
    
    .CTRL
    
     LDA #1                 \ Set A to the internal key number for CTRL and fall
                            \ through into DKS5 to scan the keyboard
    
    ENDIF
    
    
    
    
           Name: DKS5                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard to see if a specific key is being pressed
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dks5.md)
     Variations: See [code variations](../related/compare/main/subroutine/dks4-dks5.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT17](../main/subroutine/tt17.md) calls DKS5
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The internal number of the key to check (see page 142 of
                            the "Advanced User Guide for the BBC Micro" by Bray,
                            Dickens and Holmes for a list of internal key numbers)
    
    
    
    * * *
    
    
     Returns:
    
       A                    If the key in A is being pressed, A contains the
                            original argument A, but with bit 7 set (i.e. A + 128).
                            If the key in A is not being pressed, the value in A is
                            unchanged
    
       X                    Contains the same as A
    
    
    
    
    IF _SNG47
    
    .DKS5
    
     LDX #3                 \ Set X to 3, so it's ready to send to SHEILA once
                            \ interrupts have been disabled
    
     SEI                    \ Disable interrupts so we can scan the keyboard
                            \ without being hijacked
    
     STX VIA+&40            \ Set 6522 System VIA output register ORB (SHEILA &40)
                            \ to %00000011 to stop auto scan of keyboard
    
     LDX #%01111111         \ Set 6522 System VIA data direction register DDRA
     STX VIA+&43            \ (SHEILA &43) to %01111111. This sets the A registers
                            \ (IRA and ORA) so that:
                            \
                            \   * Bits 0-6 of ORA will be sent to the keyboard
                            \
                            \   * Bit 7 of IRA will be read from the keyboard
    
     STA VIA+&4F            \ Set 6522 System VIA output register ORA (SHEILA &4F)
                            \ to A, the key we want to scan for; bits 0-6 will be
                            \ sent to the keyboard, of which bits 0-3 determine the
                            \ keyboard column, and bits 4-6 the keyboard row
    
     LDX VIA+&4F            \ Read 6522 System VIA output register IRA (SHEILA &4F)
                            \ into X; bit 7 is the only bit that will have changed.
                            \ If the key is pressed, then bit 7 will be set,
                            \ otherwise it will be clear
    
     LDA #%00001011         \ Set 6522 System VIA output register ORB (SHEILA &40)
     STA VIA+&40            \ to %00001011 to restart auto scan of keyboard
    
     CLI                    \ Allow interrupts again
    
     TXA                    \ Transfer X into A
    
     RTS                    \ Return from the subroutine
    
    ENDIF
    
    
    
    
           Name: ZEKTRAN                                                 [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Clear the key logger
    
    
        Context: See this subroutine [on its own page](../main/subroutine/zektran.md)
     References: This subroutine is called as follows:
                 * [BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md) calls ZEKTRAN
                 * [FILLKL](../main/subroutine/fillkl.md) calls ZEKTRAN
                 * [TITLE](../main/subroutine/title.md) calls ZEKTRAN
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X is set to 0
    
    
    
    
    .ZEKTRAN
    
     LDA #0                 \ We want to zero the key logger buffer, so set A % 0
    
     LDX #17                \ We want to clear the 17 key logger locations from
                            \ KL to KY20, so set a counter in X
    
    .ZEKLOOP
    
     STA JSTY,X             \ Store 0 in the X-th byte of the key logger
    
     DEX                    \ Decrement the counter
    
     BNE ZEKLOOP            \ And loop back for the next key, until we have just
                            \ KL+1. We don't want to clear the first key logger
                            \ location at KL, as the keyboard table at KYTB starts
                            \ with offset 1, not 0, so KL is not technically part of
                            \ the key logger (it's actually used for logging keys
                            \ that don't appear in the keyboard table, and which
                            \ therefore don't use the key logger)
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: RDKEY                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard for key presses and update the key logger
    
    
        Context: See this subroutine [on its own page](../main/subroutine/rdkey.md)
     Variations: See [code variations](../related/compare/main/subroutine/rdkey.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DK4](../main/subroutine/dk4.md) calls RDKEY
                 * [PAS1](../main/subroutine/pas1.md) calls RDKEY
                 * [PAUSE2](../main/subroutine/pause2.md) calls RDKEY
                 * [TITLE](../main/subroutine/title.md) calls RDKEY
                 * [TT217](../main/subroutine/tt217.md) calls RDKEY
                 * [DOKEY](../main/subroutine/dokey.md) calls via RDKEY-1
    
    
    
    
    
    * * *
    
    
     Scan the keyboard, starting with internal key number 16 ("Q") and working
     through the set of internal key numbers, returning the resulting key press in
     ASCII. The key logger is also updated.
    
     This routine is effectively the same as OSBYTE 122, though the OSBYTE call
     preserves A, unlike this routine.
    
    
    
    * * *
    
    
     Returns:
    
       X                    If a key is being pressed, X contains the ASCII code
                            of the key pressed, otherwise it contains 0
    
       A                    Contains the same as X
    
       Y                    Y is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       RDKEY-1              Only scan the keyboard for valid BCD key numbers
    
    
    
    
     SED                    \ Set the D flag to enter decimal mode. Because
                            \ internal key numbers are all valid BCD (Binary Coded
                            \ Decimal) numbers, setting this flag ensures we only
                            \ loop through valid key numbers
    
    .RDKEY
    
     TYA                    \ Store Y on the stack so we can retrieve it later
     PHA
    
     JSR FILLKL             \ Call FILLKL to scan the keyboard, update the key
                            \ logger and return any non-logger key presses in X
    
     PLA                    \ Retrieve the value of Y we stored above
     TAY
    
     LDA TRTB%,X            \ Fetch the internal key number for the key pressed
    
     STA KL                 \ Store the key pressed in KL
    
     TAX                    \ Copy the key value into X
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: RDFIRE                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Read the fire button on either the analogue or digital joystick
    
    
        Context: See this subroutine [on its own page](../main/subroutine/rdfire.md)
     References: This subroutine is called as follows:
                 * [TITLE](../main/subroutine/title.md) calls RDFIRE
    
    
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the joystick fire button is being pressed, clear
                            if it isn't
    
    
    
    
    IF _COMPACT
    
    .RDFIRE
    
     LDA MOS                \ If MOS = 0 then this is a Master Compact, so jump to
     BEQ DFIRE              \ DFIRE to read the digital joystick rather than the
                            \ analogue joystick, as the Compact doesn't have the
                            \ latter
    
     CLC                    \ Clear the C flag
    
     LDA VIA+&40            \ Read 6522 System VIA input register IRB (SHEILA &40)
    
     AND #%00010000         \ Bit 4 of IRB (PB4) is clear if joystick 1's fire
                            \ button is pressed, otherwise it is set, so AND'ing
                            \ the value of IRB with %10000 extracts this bit
    
     BNE P%+3               \ If the joystick fire button is not being pressed,
                            \ jump skip the following to leave the C flag clear
    
     SEC                    \ Set the C flag to indicate the joystick fire button
                            \ is being pressed
    
     RTS                    \ Return from the subroutine
    
    .DFIRE
    
     LDA VIA+&60            \ Read the 6522 User VIA, which is where the Master
                            \ Compact's digital joystick is mapped to. The pins go
                            \ low when the joystick connection is made, and PB0 is
                            \ connected to the joystick fire button, so when PB0
                            \ is low, fire is being pressed
    
     EOR #%00000001         \ Flip PB0 so that it's 1 if fire is being pressed and
                            \ 0 if it isn't
    
     LSR A                  \ Shift PB0 into the C flag, so it's set if the fire
                            \ button is being pressed
    
     RTS                    \ Return from the subroutine
    
    ENDIF
    
    
    
    
           Name: RDJOY                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Read from either the analogue or digital joystick
    
    
        Context: See this subroutine [on its own page](../main/subroutine/rdjoy.md)
     References: This subroutine is called as follows:
                 * [DOKEY](../main/subroutine/dokey.md) calls RDJOY
    
    
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Clear if we just read from the analogue joystick, set if
                            we just read from the digital joystick
    
    
    
    
    IF _COMPACT
    
    .RDJOY
    
     LDA MOS                \ If MOS = 0 then this is a Master Compact, so jump to
     BEQ DIGITAL            \ DIGITAL to read the digital joystick rather than the
                            \ analogue joystick, as the Compact doesn't have the
                            \ latter
    
     CLC                    \ Clear the C flag to indicate that we are reading from
                            \ the analogue joystick
    
     LDA JOPOS              \ Fetch the high byte of the joystick X value
    
     EOR JSTE               \ The high byte A is now EOR'd with the value in
                            \ location JSTE, which contains &FF if both joystick
                            \ channels are reversed and 0 otherwise (so A now
                            \ contains the high byte but inverted, if that's what
                            \ the current settings say)
    
     ORA #1                 \ Ensure the value is at least 1
    
     STA JSTX               \ Store the resulting joystick X value in JSTX
    
     LDA JOPOS+1            \ Fetch the high byte of the joystick Y value
    
     EOR #&FF               \ This EOR is used in conjunction with the EOR JSTGY
                            \ below, as having a value of 0 in JSTGY means we have
                            \ to invert the joystick Y value, and this EOR does
                            \ that part
    
     EOR JSTE               \ The high byte A is now EOR'd with the value in
                            \ location JSTE, which contains &FF if both joystick
                            \ channels are reversed and 0 otherwise (so A now
                            \ contains the high byte but inverted, if that's what
                            \ the current settings say)
    
     EOR JSTGY              \ JSTGY will be 0 if the game is configured to reverse
                            \ the joystick Y channel, so this EOR along with the
                            \ EOR #&FF above does exactly that
    
     STA JSTY               \ Store the resulting joystick Y value in JSTY
    
     LDA VIA+&40            \ Read 6522 System VIA input register IRB (SHEILA &40)
    
     AND #%00010000         \ Bit 4 of IRB (PB4) is clear if joystick 1's fire
                            \ button is pressed, otherwise it is set, so AND'ing
                            \ the value of IRB with %10000 extracts this bit
    
     BNE P%+6               \ If the joystick fire button is not being pressed, skip
                            \ the following to return from the subroutine
    
     LDA #&FF               \ Update the key logger at KY7 to "press" the "A" (fire)
     STA KY7                \ button
    
     RTS                    \ Return from the subroutine
    
    .DIGITAL
    
     LDX #&FF               \ Set X = &FF so we can use it to "press" keys in the
                            \ key logger
    
     LDA VIA+&60            \ Read the 6522 User VIA, which is where the Master
                            \ Compact's digital joystick is mapped to. The pins go
                            \ low when the joystick connection is made, so we need
                            \ to check whether any of the following are zero:
                            \
                            \   PB0 = fire
                            \   PB1 = left
                            \   PB2 = down
                            \   PB3 = up
                            \   PB4 = right
    
     LSR A                  \ If PB0 from the User VIA is set (fire button), skip
     BCS P%+4               \ the following
    
     STX KY7                \ Update the key logger at KY7 to "press" the "A" (fire)
                            \ button
    
     LSR A                  \ If PB1 from the User VIA is set (left), skip the
     BCS P%+4               \ following
    
     STX KY3                \ Update the key logger at KY3 to "press" the "<" (roll
                            \ left) button
    
     LSR A                  \ If PB2 from the User VIA is set (down), skip the
     BCS P%+4               \ following
    
     STX KY6                \ Update the key logger at KY6 to "press" the "S" (pitch
                            \ down) button
                            \
                            \ Note that this is the opposite key press to the stick
                            \ direction, as in the default configuration, we want to
                            \ pull up when we pull the joystick back (i.e. when the
                            \ stick is down). To fix this, we flip this result below
    
     LSR A                  \ If PB3 from the User VIA is set (up), skip the
     BCS P%+4               \ following
    
     STX KY5                \ Update the key logger at KY5 to "press" the "X" (pull
                            \ up) button
                            \
                            \ Note that this is the opposite key press to the stick
                            \ direction, as in the default configuration, we want to
                            \ pitch down when we push the joystick forward (i.e.
                            \ when the stick is up). To fix this, we flip this
                            \ result below
    
     LSR A                  \ If PB4 from the User VIA is set (right), skip the
     BCS P%+4               \ following
    
     STX KY4                \ Update the key logger at KY4 to "press" the ">" (roll
                            \ right) button
    
     LDA JSTE               \ JSTE contains &FF if both joystick channels are
     BEQ DIG1               \ reversed and 0 otherwise, so skip to DIG1 if the
                            \ joystick channels are not reversed
    
     LDA KY3                \ Both the joystick channels are reversed, so swap the
     LDX KY4                \ values of KY3 and KY4 (to swap the roll)
     STX KY3
     STA KY4
    
    .DIG1
    
     LDA JSTE               \ Set A to &FF if both joystick channels are reversed
                            \ and 0 otherwise
    
     EOR JSTGY              \ JSTGY will be 0 if the game is configured to reverse
                            \ the joystick Y channel, &FF otherwise, so A will be
                            \ 0 if one of these is true:
                            \
                            \  * Both channels are normal and Y is reversed
                            \  * Both channels are reversed and Y is not
                            \
                            \ i.e. it will be 0 if the Y channel is configured to be
                            \ reversed
    
     BEQ DIG2               \ If the result in A is 0, skip the following to leave
                            \ the Y channel alone, as we already set the pitch keys
                            \ above to the opposite direction to the stick
    
                            \ If we get here, then the configuration settings are
                            \ not set to reverse the Y channel, so we now swap KY5
                            \ and KY6 around, as we set the pitch keys above to the
                            \ opposite direction to the stick
    
     LDA KY5                \ The Y channel should be reversed, so swap the values
     LDX KY6                \ of KY5 and KY6 (to swap the pitch)
     STX KY5
     STA KY6
    
    .DIG2
    
     SEC                    \ Set the C flag to indicate that we just read the
                            \ digital joystick
    
     RTS                    \ Return from the subroutine
    
    ENDIF
    
    
    
    
           Name: TT17X                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the digital joystick for movement
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt17x.md)
     References: This subroutine is called as follows:
                 * [DJOY](../main/subroutine/djoy.md) calls TT17X
                 * [SFRMIS](../main/subroutine/sfrmis.md) calls via TT17X-1
    
    
    
    
    
    * * *
    
    
     Scan the digital joystick for stick movement, and return the result as deltas
     (changes) in x- and y-coordinates as follows:
    
       * For joystick, X and Y are integers between -2 and +2 depending on how far
         the stick has moved
    
    
    
    * * *
    
    
     Returns:
    
       X                    Change in the x-coordinate according to the joystick
                            movement, as an integer (see above)
    
       Y                    Change in the y-coordinate according to the joystick
                            movement, as an integer (see above)
    
    
    
    * * *
    
    
     Other entry points:
    
       TT17X-1              Contains an RTS
    
    
    
    
    IF _COMPACT
    
    .TT17X
    
     LDA VIA+&60            \ Read the 6522 User VIA, which is where the Master
                            \ Compact's digital joystick is mapped to. The pins go
                            \ low when the joystick connection is made, so we need
                            \ to check whether any of the following are zero:
                            \
                            \   PB0 = fire
                            \   PB1 = left
                            \   PB2 = down
                            \   PB3 = up
                            \   PB4 = right
    
     LSR A                  \ Shift the PB0 bit out, as we aren't interested in the
                            \ fire button in this routine
    
     LDX #0                 \ Set the initial values for the results, X = Y = 0,
     LDY #0                 \ which we now increase or decrease appropriately
    
     LSR A                  \ If PB1 from the User VIA is set (left), skip the
     BCS P%+3               \ following
    
     DEX                    \ Set X = X - 1
    
     LSR A                  \ If PB2 from the User VIA is set (down), skip the
     BCS P%+3               \ following
    
     INY                    \ Set Y = Y + 1
                            \
                            \ Note that this is the opposite direction to the stick
                            \ direction, as moving the stick down should decrease Y.
                            \ To fix this, we flip this result below
    
     LSR A                  \ If PB3 from the User VIA is set (up), skip the
     BCS P%+3               \ following
    
     DEY                    \ Set Y = Y - 1
                            \
                            \ Note that this is the opposite direction to the stick
                            \ direction, as moving the stick up should increase Y.
                            \ To fix this, we flip this result below
    
     LSR A                  \ If PB4 from the User VIA is set (right), skip the
     BCS P%+3               \ following
    
     INX                    \ Set X = X + 1
    
     LDA JSTE               \ JSTE contains &FF if both joystick channels are
     BEQ TT171              \ reversed and 0 otherwise, so skip to TT171 if the
                            \ joystick channels are not reversed
    
     TXA                    \ The X channel needs to be reversed, so negate the
     EOR #&FF               \ value in X, using two's complement
     CLC
     ADC #1
     TAX
    
    .TT171
    
     LDA JSTE               \ Set A to &FF if both joystick channels are reversed
                            \ and 0 otherwise
    
     EOR JSTGY              \ JSTGY will be 0 if the game is configured to reverse
                            \ the joystick Y channel, &FF otherwise, so A will be
                            \ 0 if one of these is true:
                            \
                            \  * Both channels are normal and Y is reversed
                            \  * Both channels are reversed and Y is not
                            \
                            \ i.e. it will be 0 if the Y channel is configured to be
                            \ reversed
    
     BEQ TT17X-1            \ If the result in A is 0, return from the subroutine
                            \ (as TT17X-1 contain an RTS), as we already set the Y
                            \ value above to the opposite direction to the stick
    
                            \ If we get here, then the configuration settings are
                            \ not set to reverse the Y channel, so we now negate the
                            \ Y value, as we set Y above to the opposite direction
                            \ to the stick
    
     TYA                    \ The Y channel should be reversed, so negate the value
     EOR #&FF               \ in Y, using two's complement
     CLC
     ADC #1
     TAY
    
                            \ Fall through into yetanotherrts to return from the
                            \ subroutine
    
    ENDIF
    
    
    
    
           Name: yetanotherrts                                           [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Contains an RTS
    
    
        Context: See this subroutine [on its own page](../main/subroutine/yetanotherrts.md)
     References: This subroutine is called as follows:
                 * [SFRMIS](../main/subroutine/sfrmis.md) calls yetanotherrts
    
    
    
    
    
    * * *
    
    
     This routine contains an RTS so we can return from the SFRMIS subroutine with
     a branch instruction.
    
     It also contains the DEMON label, which is left over from the 6502 Second
     Processor version, where it implements the demo (there is no demo in this
     version of Elite).
    
    
    
    
    .yetanotherrts
    
    .DEMON
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: ECMOF                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Switch off the E.C.M. and turn off the dashboard bulb
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ecmof.md)
     Variations: See [code variations](../related/compare/main/subroutine/ecmof.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md) calls ECMOF
                 * [RES2](../main/subroutine/res2.md) calls ECMOF
    
    
    
    
    
    
    .ECMOF
    
    IF _SNG47
    
     LDA #0                 \ Set ECMA and ECMP to 0 to indicate that no E.C.M. is
     STA ECMA               \ currently running
     STA ECMP
    
    ELIF _COMPACT
    
     STZ ECMA               \ Set ECMA and ECMP to 0 to indicate that no E.C.M. is
     STZ ECMP               \ currently running
    
    ENDIF
    
     JMP ECBLB              \ Update the E.C.M. indicator bulb on the dashboard and
                            \ return from the subroutine using a tail call
    
    
    
    
           Name: SFRMIS                                                  [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Add an enemy missile to our local bubble of universe
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sfrmis.md)
     Variations: See [code variations](../related/compare/main/subroutine/sfrmis.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md) calls SFRMIS
    
    
    
    
    
    * * *
    
    
     An enemy has fired a missile, so add the missile to our universe if there is
     room, and if there is, make the appropriate warnings and noises.
    
    
    
    
    .SFRMIS
    
     LDX #MSL               \ Set X to the ship type of a missile, and call SFS1-2
     JSR SFS1-2             \ to add a missile to our universe that has AI (bit 7
                            \ set), is hostile (bit 6 set) and has been launched
                            \ (bit 0 clear); the target slot number is set to 31,
                            \ but this is ignored as the hostile flags means we
                            \ are the target
    
    IF _SNG47
    
     BCC yetanotherrts      \ The C flag will be set if the call to SFS1-2 was a
                            \ success, so if it's clear, jump to yetanotherrts to
                            \ return from the subroutine (as yetanotherrts contains
                            \ an RTS)
    
    ELIF _COMPACT
    
     BCC TT17X-1            \ The C flag will be set if the call to SFS1-2 was a
                            \ success, so if it's clear, jump to TT17X-1 to return
                            \ from the subroutine (as TT17X-1 contains an RTS)
    
    ENDIF
    
     LDA #120               \ Print recursive token 120 ("INCOMING MISSILE") as an
     JSR MESS               \ in-flight message
    
     LDY #solaun            \ Call the NOISE routine with Y = 8 to make the sound
     JMP NOISE              \ of the missile being launched and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: EXNO2                                                   [Show more]
           Type: Subroutine
       Category: Status
        Summary: Process us making a kill
      Deep dive: [Combat rank](https://elite.bbcelite.com/deep_dives/combat_rank.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/exno2.md)
     Variations: See [code variations](../related/compare/main/subroutine/exno2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md) calls EXNO2
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls EXNO2
                 * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md) calls EXNO2
    
    
    
    
    
    * * *
    
    
     We have killed a ship, so increase the kill tally, displaying an iconic
     message of encouragement if the kill total is a multiple of 256, and then
     make a nearby explosion sound.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The type of the ship that was killed
    
    
    
    
    .EXNO2
    
     LDA TALLYL             \ We now add the fractional kill count to our tally,
     CLC                    \ starting with the fractional bytes:
     ADC KWL%-1,X           \
     STA TALLYL             \   TALLYL = TALLYL + fractional kill count
                            \
                            \ where the fractional kill count is taken from the
                            \ KWL% table, according to the ship's type (we look up
                            \ the X-1-th value from KWL% because ship types start
                            \ at 1 rather than 0)
    
     LDA TALLY              \ And then we add the low byte of TALLY(1 0):
     ADC KWH%-1,X           \
     STA TALLY              \   TALLY = TALLY + carry + integer kill count
                            \
                            \ where the integer kill count is taken from the KWH%
                            \ table in the same way
    
     BCC davidscockup       \ If there is no carry, jump to davidscockup to skip the
                            \ following three instructions, as we have not earned
                            \ a "RIGHT ON COMMANDER!" message
    
     INC TALLY+1            \ Increment the high byte of the kill count in TALLY
    
     LDA #101               \ The kill total is a multiple of 256, so it's time
     JSR MESS               \ for a pat on the back, so print recursive token 101
                            \ ("RIGHT ON COMMANDER!") as an in-flight message
    
    .davidscockup
    
                            \ Fall through into EXNO3 to make the sound of a
                            \ ship exploding
    
    
    
    
           Name: EXNO3                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make an explosion sound
    
    
        Context: See this subroutine [on its own page](../main/subroutine/exno3.md)
     Variations: See [code variations](../related/compare/main/subroutine/exno3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 10 of 16)](../main/subroutine/main_flight_loop_part_10_of_16.md) calls EXNO3
                 * [OOPS](../main/subroutine/oops.md) calls EXNO3
                 * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md) calls EXNO3
    
    
    
    
    
    * * *
    
    
     Make the sound of death in the cold, hard vacuum of space. Apparently, in
     Elite space, everyone can hear you scream.
    
     This routine also makes the sound of a destroyed cargo canister if we don't
     get scooping right, the sound of us colliding with another ship, and the sound
     of us being hit with depleted shields. It is not a good sound to hear.
    
    
    
    
    .EXNO3
    
     LDY #soexpl            \ Call the NOISE routine with Y = 4 to make the sound of
     JMP NOISE              \ an explosion and return from the subroutine using a
                            \ tail call
    
    
    
    
           Name: EXNO                                                    [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make the sound of a laser strike on another ship or a ship
                 explosion
    
    
        Context: See this subroutine [on its own page](../main/subroutine/exno.md)
     Variations: See [code variations](../related/compare/main/subroutine/exno.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls EXNO
    
    
    
    
    
    * * *
    
    
     Make the two-part explosion sound of us making a laser strike, or of another
     ship exploding.
    
    
    
    
    .EXNO
    
     LDY #sohit             \ Call the NOISE routine with Y = 6 to make the sound of
     JMP NOISE              \ us making a hit or kill and return from the subroutine
                            \ using a tail call
    
    
    
    
           Name: COLD                                                    [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Set the standard BRKV handler for the game
    
    
        Context: See this subroutine [on its own page](../main/subroutine/cold.md)
     Variations: See [code variations](../related/compare/main/subroutine/brkbk-cold.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [S%](../main/subroutine/s_per_cent.md) calls COLD
    
    
    
    
    
    
    .COLD
    
     LDA #LO(NEWBRK)        \ Set BRKV to point to the NEWBRK routine
     STA BRKV
     LDA #HI(NEWBRK)
     STA BRKV+1
    
     LDA #LO(CHPR)          \ Set WRCHV to point to the CHPR routine
     STA WRCHV
     LDA #HI(CHPR)
     STA WRCHV+1
    
     JSR setzp              \ Call setzp to copy the top part of zero page into
                            \ the buffer at &3000
    
     JSR SETINTS            \ Call SETINTS to set various vectors, interrupts and
                            \ timers
    
     JMP SOFLUSH            \ Call SOFLUSH to reset the sound buffers and return
                            \ from the subroutine using a tail call
    
    
    
    
           Name: NMIpissoff                                              [Show more]
           Type: Subroutine
       Category: Loader
        Summary: Acknowledge NMI interrupts and ignore them
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nmipissoff.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    IF _SNG47
    
    .NMIpissoff
    
     CLI                    \ These instructions are never reached and have no
     RTI                    \ effect
    
    ENDIF
    
    
    
    
           Name: F%                                                      [Show more]
           Type: Variable
       Category: Utility routines
        Summary: Denotes the end of the main game code, from ELITE A to ELITE H
    
    
        Context: See this variable [on its own page](../main/variable/f_per_cent.md)
     Variations: See [code variations](../related/compare/main/variable/f_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DEEOR](../main/subroutine/deeor.md) uses F%
    
    
    
    
    
    
    .F%
    
     SKIP 0
    
    IF _COMPACT
    
     EQUB &F8, &F8          \ These bytes appear to be unused
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8, &F8
     EQUB &F8
    
    ENDIF
    
    
    
     Save ELTH.bin
    
    
    
    
     PRINT "ELITE H"
     PRINT "Assembled at ", ~CODE_H%
     PRINT "Ends at ", ~P%
     PRINT "Code size is ", ~(P% - CODE_H%)
     PRINT "Execute at ", ~LOAD%
     PRINT "Reload at ", ~LOAD_H%
    
     PRINT "S.ELTH ", ~CODE_H%, " ", ~P%, " ", ~LOAD%, " ", ~LOAD_H%
    \SAVE "3-assembled-output/ELTH.bin", CODE_G%, P%, LOAD%
    
    
    
     Save BCODE.unprot.bin
    
    
    
    
     PRINT "S.BCODE ", ~CODE%, " ", ~P%, " ", ~LOAD%, " ", ~LOAD%
     SAVE "3-assembled-output/BCODE.unprot.bin", CODE%, P%, LOAD%
    
     PRINT "Addresses for the scramble routines in elite-checksum.py"
     PRINT "F% = ", ~F%
     PRINT "G% = ", ~G%
     PRINT "NA2% = ", ~NA2%
    

[X]

Variable [ALP1](workspaces.md#alp1) in workspace [ZP](workspaces.md#header-zp)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Variable [ALP2](workspaces.md#alp2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Variable [ALPHA](workspaces.md#alpha) in workspace [ZP](workspaces.md#header-zp)

The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31

[X]

Configuration variable [Armlas](workspaces.md#armlas)

Military laser power

[X]

Variable [BET1](workspaces.md#bet1) in workspace [ZP](workspaces.md#header-zp)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BET2](workspaces.md#bet2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of BET2 = sign of the pitch angle in BETA

[X]

Variable [BETA](workspaces.md#beta) in workspace [ZP](workspaces.md#header-zp)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Variable [BOMB](workspaces.md#bomb) in workspace [WP](workspaces.md#header-wp)

Energy bomb

[X]

Subroutine [BOMBOFF](elite_a.md#header-bomboff) (category: Drawing lines)

Draw the zig-zag lightning bolt for the energy bomb

[X]

Configuration variable [BRKV](workspaces.md#brkv)

The break vector that we intercept to enable us to handle and display system errors

[X]

Subroutine [CHPR](elite_a.md#header-chpr) (category: Text)

Print a character at the text cursor by poking into screen memory

[X]

Variable [COL](workspaces.md#col) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](workspaces.md#cyan)

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Variable [DELTA](workspaces.md#delta) in workspace [ZP](workspaces.md#header-zp)

Our current speed, in the range 1-40

[X]

Label [DFIRE](elite_h.md#dfire) in subroutine [RDFIRE](elite_h.md#header-rdfire)

[X]

Label [DIG1](elite_h.md#dig1) in subroutine [RDJOY](elite_h.md#header-rdjoy)

[X]

Label [DIG2](elite_h.md#dig2) in subroutine [RDJOY](elite_h.md#header-rdjoy)

[X]

Label [DIGITAL](elite_h.md#digital) in subroutine [RDJOY](elite_h.md#header-rdjoy)

[X]

Variable [DLY](workspaces.md#dly) in workspace [WP](workspaces.md#header-wp)

In-flight message delay

[X]

Subroutine [DOVDU19](elite_a.md#header-dovdu19) (category: Drawing the screen)

Change the mode 1 palette

[X]

Variable [DTW2](elite_b.md#header-dtw2) (category: Text)

A flag that indicates whether we are currently printing a word

[X]

Subroutine [ECBLB](elite_a.md#header-ecblb) (category: Dashboard)

Light up the E.C.M. indicator bulb ("E") on the dashboard

[X]

Variable [ECMA](workspaces.md#ecma) in workspace [ZP](workspaces.md#header-zp)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Variable [ECMP](workspaces.md#ecmp) in workspace [WP](workspaces.md#header-wp)

Our E.C.M. status

[X]

Subroutine [FILLKL](elite_h.md#header-fillkl) (category: Keyboard)

Scan the keyboard for a flight key and update the key logger

[X]

Subroutine [FLFLLS](elite_e.md#header-flflls) (category: Drawing suns)

Reset the sun line heap

[X]

Subroutine [FLIP](elite_b.md#header-flip) (category: Stardust)

Reflect the stardust particles in the screen diagonal and redraw the stardust field

[X]

Subroutine [FMLTU](elite_c.md#header-fmltu) (category: Maths (Arithmetic))

Calculate A = A * Q / 256

[X]

Variable [IKNS](elite_h.md#header-ikns) (category: Keyboard)

Lookup table for in-flight keyboard controls

[X]

Variable [INWK](workspaces.md#inwk) in workspace [ZP](workspaces.md#header-zp)

The zero-page internal workspace for the current ship data block

[X]

Variable [JOPOS](workspaces.md#jopos) in workspace [WP](workspaces.md#header-wp)

Contains the high byte of the latest value from ADC channel 1 (the joystick X value), which gets updated regularly by the IRQ1 interrupt handler

[X]

Variable [JSTE](elite_a.md#jste) in workspace [UP](elite_a.md#header-up)

Reverse both joystick channels configuration setting

[X]

Variable [JSTGY](elite_a.md#jstgy) in workspace [UP](elite_a.md#header-up)

Reverse joystick Y-channel configuration setting

[X]

Variable [JSTX](workspaces.md#jstx) in workspace [ZP](workspaces.md#header-zp)

Our current roll rate

[X]

Variable [JSTY](workspaces.md#jsty) in workspace [ZP](workspaces.md#header-zp)

Our current pitch rate

[X]

Variable [K](workspaces.md#k) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K2](workspaces.md#k2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [KL](workspaces.md#kl) in workspace [ZP](workspaces.md#header-zp)

The following bytes implement a key logger that enables Elite to scan for concurrent key presses of the primary flight keys, plus a secondary flight key

[X]

Configuration variable [KWH%](workspaces.md#kwh-per-cent)

The address of the kill tally integer table, as set in elite-data.asm

[X]

Configuration variable [KWL%](workspaces.md#kwl-per-cent)

The address of the kill tally fraction table, as set in elite-data.asm

[X]

Variable [KY17](workspaces.md#ky17) in workspace [ZP](workspaces.md#header-zp)

"E" is being pressed (activate E.C.M.)

[X]

Variable [KY19](workspaces.md#ky19) in workspace [ZP](workspaces.md#header-zp)

"C" is being pressed (activate docking computer)

[X]

Variable [KY3](workspaces.md#ky3) in workspace [ZP](workspaces.md#header-zp)

"<" is being pressed (roll left)

[X]

Variable [KY4](workspaces.md#ky4) in workspace [ZP](workspaces.md#header-zp)

">" is being pressed (roll right)

[X]

Variable [KY5](workspaces.md#ky5) in workspace [ZP](workspaces.md#header-zp)

"X" is being pressed (pull up)

[X]

Variable [KY6](workspaces.md#ky6) in workspace [ZP](workspaces.md#header-zp)

"S" is being pressed (pitch down)

[X]

Variable [KY7](workspaces.md#ky7) in workspace [ZP](workspaces.md#header-zp)

"A" is being pressed (fire lasers)

[X]

Variable [LAS2](workspaces.md#las2) in workspace [WP](workspaces.md#header-wp)

Laser power for the current laser

[X]

Variable [LASER](workspaces.md#laser) in workspace [WP](workspaces.md#header-wp)

The specifications of the lasers fitted to each of the four space views

[X]

Entry point [LO2](elite_h.md#lo2) in subroutine [LOOK1](elite_h.md#header-look1) (category: Flight)

Contains an RTS

[X]

Label [LQ](elite_h.md#lq) in subroutine [LOOK1](elite_h.md#header-look1)

[X]

Variable [LSP](workspaces.md#lsp) in workspace [ZP](workspaces.md#header-zp)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Subroutine [MAD](elite_c.md#header-mad) (category: Maths (Arithmetic))

Calculate (A X) = Q * A + (S R)

[X]

Variable [MCNT](workspaces.md#mcnt) in workspace [ZP](workspaces.md#header-zp)

The main loop counter

[X]

Subroutine [MESS](elite_f.md#header-mess) (category: Flight)

Display an in-flight message

[X]

Subroutine [MLTU2](elite_c.md#header-mltu2) (category: Maths (Arithmetic))

Calculate (A P+1 P) = (A ~P) * Q

[X]

Entry point [MLTU2-2](elite_c.md#mltu2) in subroutine [MLTU2](elite_c.md#header-mltu2) (category: Maths (Arithmetic))

Set Q to X, so this calculates (A P+1 P) = (A ~P) * X

[X]

Variable [MOS](workspaces.md#mos) in workspace [ZP](workspaces.md#header-zp)

Determines whether we are running on a Master Compact

[X]

Configuration variable [MSL](workspaces.md#msl)

Ship type for a missile

[X]

Subroutine [MT2](elite_a.md#header-mt2) (category: Text)

Switch to Sentence Case when printing extended tokens

[X]

Subroutine [MULT3](elite_c.md#header-mult3) (category: Maths (Arithmetic))

Calculate K(3 2 1 0) = (A P+1 P) * Q

[X]

Label [MV1](elite_h.md#mv1) in subroutine [MV40](elite_h.md#header-mv40)

[X]

Label [MV10](elite_h.md#mv10) in subroutine [MVT1](elite_h.md#header-mvt1)

[X]

Label [MV11](elite_h.md#mv11) in subroutine [MVT1](elite_h.md#header-mvt1)

[X]

Label [MV2](elite_h.md#mv2) in subroutine [MV40](elite_h.md#header-mv40)

[X]

Label [MV26](elite_h.md#mv26) in subroutine [MVEIT (Part 2 of 9)](elite_h.md#header-mveit-part-2-of-9)

[X]

Label [MV3](elite_h.md#mv3) in subroutine [MVEIT (Part 2 of 9)](elite_h.md#header-mveit-part-2-of-9)

[X]

Label [MV30](elite_h.md#mv30) in subroutine [MVEIT (Part 2 of 9)](elite_h.md#header-mveit-part-2-of-9)

[X]

Subroutine [MV40](elite_h.md#header-mv40) (category: Moving)

Rotate the planet or sun's location in space by the amount of pitch and roll of our ship

[X]

Label [MV43](elite_h.md#mv43) in subroutine [MVEIT (Part 5 of 9)](elite_h.md#header-mveit-part-5-of-9)

[X]

Label [MV44](elite_h.md#mv44) in subroutine [MVEIT (Part 5 of 9)](elite_h.md#header-mveit-part-5-of-9)

[X]

Entry point [MV45](elite_h.md#mv45) in subroutine [MVEIT (Part 6 of 9)](elite_h.md#header-mveit-part-6-of-9) (category: Moving)

Rejoin the MVEIT routine after the rotation, tactics and scanner code

[X]

Label [MV5](elite_h.md#mv5) in subroutine [MVEIT (Part 9 of 9)](elite_h.md#header-mveit-part-9-of-9)

[X]

Label [MV50](elite_h.md#mv50) in subroutine [MVT6](elite_h.md#header-mvt6)

[X]

Label [MV51](elite_h.md#mv51) in subroutine [MVT6](elite_h.md#header-mvt6)

[X]

Label [MV8](elite_h.md#mv8) in subroutine [MVEIT (Part 8 of 9)](elite_h.md#header-mveit-part-8-of-9)

[X]

Label [MVD1](elite_h.md#mvd1) in subroutine [MVEIT (Part 9 of 9)](elite_h.md#header-mveit-part-9-of-9)

[X]

Subroutine [MVS4](elite_h.md#header-mvs4) (category: Moving)

Apply pitch and roll to an orientation vector

[X]

Subroutine [MVS5](elite_b.md#header-mvs5) (category: Moving)

Apply a 3.6 degree pitch or roll to an orientation vector

[X]

Subroutine [MVT1](elite_h.md#header-mvt1) (category: Moving)

Calculate (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (A R)

[X]

Entry point [MVT1-2](elite_h.md#mvt1) in subroutine [MVT1](elite_h.md#header-mvt1) (category: Moving)

Clear bits 0-6 of A before entering MVT1

[X]

Subroutine [MVT3](elite_b.md#header-mvt3) (category: Moving)

Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)

[X]

Subroutine [MVT6](elite_h.md#header-mvt6) (category: Moving)

Calculate (A P+2 P+1) = (x_sign x_hi x_lo) + (A P+2 P+1)

[X]

Subroutine [NEWBRK](elite_f.md#header-newbrk) (category: Utility routines)

The standard BRKV handler for the game

[X]

Subroutine [NOISE](elite_a.md#header-noise) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Subroutine [NWSTARS](elite_e.md#header-nwstars) (category: Stardust)

Initialise the stardust field

[X]

Variable [P](workspaces.md#p) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [POW](workspaces.md#pow)

Pulse laser power

[X]

Label [PU2](elite_h.md#pu2) in subroutine [PLUT](elite_h.md#header-plut)

[X]

[X]

Label [PUS1](elite_h.md#pus1) in subroutine [PLUT](elite_h.md#header-plut)

[X]

Variable [Q](workspaces.md#q) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [QQ11](workspaces.md#qq11) in workspace [ZP](workspaces.md#header-zp)

The type of the current view

[X]

Variable [QQ17](workspaces.md#qq17) in workspace [ZP](workspaces.md#header-zp)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Variable [QQ19](workspaces.md#qq19) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [QQ22](workspaces.md#qq22) in workspace [ZP](workspaces.md#header-zp)

The two hyperspace countdown counters

[X]

Variable [R](workspaces.md#r) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [RAT](workspaces.md#rat) in workspace [ZP](workspaces.md#header-zp)

Used to store different signs depending on the current space view, for use in calculating stardust movement

[X]

Variable [RAT2](workspaces.md#rat2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the pitch and roll signs when moving objects and stardust

[X]

Label [Rd1](elite_h.md#rd1) in subroutine [FILLKL](elite_h.md#header-fillkl)

[X]

Label [Rd2](elite_h.md#rd2) in subroutine [FILLKL](elite_h.md#header-fillkl)

[X]

Label [Rd3](elite_h.md#rd3) in subroutine [FILLKL](elite_h.md#header-fillkl)

[X]

Label [Rd4](elite_h.md#rd4) in subroutine [FILLKL](elite_h.md#header-fillkl)

[X]

Label [Rd5](elite_h.md#rd5) in subroutine [FILLKL](elite_h.md#header-fillkl)

[X]

Variable [S](workspaces.md#s) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [SCAN](elite_a.md#header-scan) (category: Dashboard)

Display the current ship on the scanner

[X]

Subroutine [SETINTS](elite_a.md#header-setints) (category: Loader)

Set the various vectors, interrupts and timers

[X]

Entry point [SFS1-2](elite_c.md#sfs1) in subroutine [SFS1](elite_c.md#header-sfs1) (category: Universe)

Used to add a missile to the local bubble that that has AI (bit 7 set), is hostile (bit 6 set) and has been launched (bit 0 clear); the target slot number is set to 31, but this is ignored as the hostile flags means we are the target

[X]

Label [SIG1](elite_h.md#sig1) in subroutine [SIGHT](elite_h.md#header-sight)

[X]

Subroutine [SIGHT](elite_h.md#header-sight) (category: Flight)

Draw the laser crosshairs

[X]

Subroutine [SOFLUSH](elite_a.md#header-soflush) (category: Sound)

Reset the sound buffers and turn off all sound channels

[X]

Variable [T](workspaces.md#t) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [TACTICS (Part 2 of 7)](elite_c.md#header-tactics-part-2-of-7) (category: Tactics)

Apply tactics: Escape pod, station, lone Thargon, safe-zone pirate

[X]

Variable [TALLY](workspaces.md#tally) in workspace [WP](workspaces.md#header-wp)

Our combat rank

[X]

Variable [TALLYL](workspaces.md#tallyl) in workspace [WP](workspaces.md#header-wp)

Combat rank fraction

[X]

Subroutine [TIDY](elite_f.md#header-tidy) (category: Maths (Geometry))

Orthonormalise the orientation vectors for a ship

[X]

Variable [TRTB%](elite_h.md#header-trtb-per-cent) (category: Keyboard)

Translation table from internal key number to ASCII

[X]

Subroutine [TT15](elite_d.md#header-tt15) (category: Drawing lines)

Draw a set of crosshairs

[X]

Subroutine [TT162](elite_d.md#header-tt162) (category: Text)

Print a space

[X]

Label [TT171](elite_h.md#tt171) in subroutine [TT17X](elite_h.md#header-tt17x)

[X]

Entry point [TT17X-1](elite_h.md#tt17x) in subroutine [TT17X](elite_h.md#header-tt17x) (category: Keyboard)

Contains an RTS

[X]

Subroutine [TT27](elite_e.md#header-tt27) (category: Text)

Print a text token

[X]

Subroutine [TT66](elite_h.md#header-tt66) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Subroutine [TTX66](elite_a.md#header-ttx66) (category: Drawing the screen)

Clear the top part of the screen and draw a border box

[X]

Variable [TYPE](workspaces.md#type) in workspace [ZP](workspaces.md#header-zp)

The current ship type

[X]

Configuration variable [VIA](workspaces.md#via)

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [VIEW](workspaces.md#view) in workspace [WP](workspaces.md#header-wp)

The number of the current space view

[X]

Configuration variable [WHITE](workspaces.md#white)

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[X]

Subroutine [WPSHPS](elite_e.md#header-wpshps) (category: Dashboard)

Clear the scanner, reset the ball line and sun line heaps

[X]

Configuration variable [WRCHV](workspaces.md#wrchv)

The WRCHV vector that we intercept with our custom text printing routine

[X]

Variable [XC](workspaces.md#xc) in workspace [ZP](workspaces.md#header-zp)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [XSAV](workspaces.md#xsav) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX0](workspaces.md#xx0) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Configuration variable [Y](workspaces.md#y)

The centre y-coordinate of the 256 x 192 space view

[X]

Configuration variable [YELLOW](workspaces.md#yellow)

Four mode 1 pixels of colour 1 (yellow)

[X]

Label [ZEKLOOP](elite_h.md#zekloop) in subroutine [ZEKTRAN](elite_h.md#header-zektran)

[X]

Subroutine [ZEKTRAN](elite_h.md#header-zektran) (category: Keyboard)

Clear the key logger

[X]

Label [davidscockup](elite_h.md#davidscockup) in subroutine [EXNO2](elite_h.md#header-exno2)

[X]

Variable [de](workspaces.md#de) in workspace [WP](workspaces.md#header-wp)

Equipment destruction flag

[X]

Subroutine [ee3](elite_d.md#header-ee3) (category: Flight)

Print the hyperspace countdown in the top-left of the screen

[X]

Subroutine [setzp](elite_a.md#header-setzp) (category: Utility routines)

Copy the top part of zero page (&0090 to &00FF) into the buffer at &3000

[X]

Variable [sightcol](elite_h.md#header-sightcol) (category: Drawing lines)

Colours for the crosshair sights on the different laser types

[X]

Configuration variable [soexpl](workspaces.md#soexpl)

Sound 4 = We died / Collision / Being hit by lasers 2

[X]

Configuration variable [sohit](workspaces.md#sohit)

Sound 6 = We made a hit/kill / Other ship exploding

[X]

Configuration variable [solaun](workspaces.md#solaun)

Sound 8 = Missile launched / Ship launch

[X]

Label [tt66](elite_h.md#tt66) in subroutine [TTX66K](elite_h.md#header-ttx66k)

[X]

Subroutine [yetanotherrts](elite_h.md#header-yetanotherrts) (category: Tactics)

Contains an RTS

[Elite G source](elite_g.md "Previous routine")[Game data](elite_data.md "Next routine")
