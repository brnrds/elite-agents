---
title: "The Main flight loop (Part 2 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_2_of_16.html"
---

[Main flight loop (Part 1 of 16)](main_flight_loop_part_1_of_16.md "Previous routine")[Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 2 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Calculate the alpha and beta angles from the current pitch and
                 roll of our ship
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Pitching and rolling](https://elite.bbcelite.com/deep_dives/pitching_and_rolling.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-2-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_2_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Calculate the alpha and beta angles from the current pitch and roll
    
     Here we take the current rate of pitch and roll, as set by the joystick or
     keyboard, and convert them into alpha and beta angles that we can use in the
     matrix functions to rotate space around our ship. The alpha angle covers
     roll, while the beta angle covers pitch (there is no yaw in this version of
     Elite). The angles are in radians, which allows us to use the small angle
     approximation when moving objects in the sky (see the MVEIT routine for more
     on this). Also, the signs of the two angles are stored separately, in both
     the sign and the flipped sign, as this makes calculations easier.
    
    
    
    
     LDX JSTX               \ Set X to the current rate of roll in JSTX
    
     JSR cntr               \ Apply keyboard damping twice (if enabled) so the roll
     JSR cntr               \ rate in X creeps towards the centre by 2
    
                            \ The roll rate in JSTX increases if we press ">" (and
                            \ the RL indicator on the dashboard goes to the right)
                            \
                            \ This rolls our ship to the right (clockwise), but we
                            \ actually implement this by rolling everything else
                            \ to the left (anti-clockwise), so a positive roll rate
                            \ in JSTX translates to a negative roll angle alpha
    
     TXA                    \ Set A and Y to the roll rate but with the sign bit
     EOR #%10000000         \ flipped (i.e. set them to the sign we want for alpha)
     TAY
    
     AND #%10000000         \ Extract the flipped sign of the roll rate and store
     STA ALP2               \ in ALP2 (so ALP2 contains the sign of the roll angle
                            \ alpha)
    
     STX JSTX               \ Update JSTX with the damped value that's still in X
    
     EOR #%10000000         \ Extract the correct sign of the roll rate and store
     STA ALP2+1             \ in ALP2+1 (so ALP2+1 contains the flipped sign of the
                            \ roll angle alpha)
    
     TYA                    \ Set A to the roll rate but with the sign bit flipped
    
     BPL P%+7               \ If the value of A is positive, skip the following
                            \ three instructions
    
     EOR #%11111111         \ A is negative, so change the sign of A using two's
     CLC                    \ complement so that A is now positive and contains
     ADC #1                 \ the absolute value of the roll rate, i.e. |JSTX|
    
     LSR A                  \ Divide the (positive) roll rate in A by 4
     LSR A
    
     CMP #8                 \ If A >= 8, skip the following instruction
     BCS P%+3
    
     LSR A                  \ A < 8, so halve A again
    
     STA ALP1               \ Store A in ALP1, so we now have:
                            \
                            \   ALP1 = |JSTX| / 8    if |JSTX| < 32
                            \
                            \   ALP1 = |JSTX| / 4    if |JSTX| >= 32
                            \
                            \ This means that at lower roll rates, the roll angle is
                            \ reduced closer to zero than at higher roll rates,
                            \ which gives us finer control over the ship's roll at
                            \ lower roll rates
                            \
                            \ Because JSTX is in the range -127 to +127, ALP1 is
                            \ in the range 0 to 31
    
     ORA ALP2               \ Store A in ALPHA, but with the sign set to ALP2 (so
     STA ALPHA              \ ALPHA has a different sign to the actual roll rate)
    
     LDX JSTY               \ Set X to the current rate of pitch in JSTY
    
     JSR cntr               \ Apply keyboard damping so the pitch rate in X creeps
                            \ towards the centre by 1
    
     TXA                    \ Set A and Y to the pitch rate but with the sign bit
     EOR #%10000000         \ flipped
     TAY
    
     AND #%10000000         \ Extract the flipped sign of the pitch rate into A
    
     STX JSTY               \ Update JSTY with the damped value that's still in X
    
     STA BET2+1             \ Store the flipped sign of the pitch rate in BET2+1
    
     EOR #%10000000         \ Extract the correct sign of the pitch rate and store
     STA BET2               \ it in BET2
    
     TYA                    \ Set A to the pitch rate but with the sign bit flipped
    
     BPL P%+4               \ If the value of A is positive, skip the following
                            \ instruction
    
     EOR #%11111111         \ A is negative, so flip the bits
    
     ADC #4                 \ Add 4 to the (positive) pitch rate, so the maximum
                            \ value is now up to 131 (rather than 127)
    
     LSR A                  \ Divide the (positive) pitch rate in A by 16
     LSR A
     LSR A
     LSR A
    
     CMP #3                 \ If A >= 3, skip the following instruction
     BCS P%+3
    
     LSR A                  \ A < 3, so halve A again
    
     STA BET1               \ Store A in BET1, so we now have:
                            \
                            \   BET1 = |JSTY| / 32    if |JSTY| < 48
                            \
                            \   BET1 = |JSTY| / 16    if |JSTY| >= 48
                            \
                            \ This means that at lower pitch rates, the pitch angle
                            \ is reduced closer to zero than at higher pitch rates,
                            \ which gives us finer control over the ship's pitch at
                            \ lower pitch rates
                            \
                            \ Because JSTY is in the range -131 to +131, BET1 is in
                            \ the range 0 to 8
    
     ORA BET2               \ Store A in BETA, but with the sign set to BET2 (so
     STA BETA               \ BETA has the same sign as the actual pitch rate)
    
     LDA BSTK               \ If BSTK = 0 then the Bitstik is not configured, so
     BEQ BS2                \ jump to BS2 to skip the following
    
     LDA JOPOS+2            \ Fetch the Bitstik rotation value (high byte) from
                            \ JOPOS+2, which is constantly updated with the high
                            \ byte of ADC channel 3 by the interrupt handler IRQ1
    
     LSR A                  \ Divide A by 4
     LSR A
    
     CMP #40                \ If A < 40, skip the following instruction
     BCC P%+4
    
     LDA #40                \ Set A = 40, which ensures a maximum speed of 40
    
     STA DELTA              \ Update our speed in DELTA
    
     BNE MA4                \ If the speed we just set is non-zero, then jump to MA4
                            \ to skip the following, as we don't need to check the
                            \ keyboard for speed keys, otherwise do check the
                            \ keyboard (so Bitstik users can still use the keyboard
                            \ for speed adjustments if they twist the stick to zero)
    

[X]

Variable [ALP1](../workspace/zp.md#alp1) in workspace [ZP](../workspace/zp.md)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Variable [ALP2](../workspace/zp.md#alp2) in workspace [ZP](../workspace/zp.md)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Variable [ALPHA](../workspace/zp.md#alpha) in workspace [ZP](../workspace/zp.md)

The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31

[X]

Variable [BET1](../workspace/zp.md#bet1) in workspace [ZP](../workspace/zp.md)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BET2](../workspace/zp.md#bet2) in workspace [ZP](../workspace/zp.md)

Bit 7 of BET2 = sign of the pitch angle in BETA

[X]

Variable [BETA](../workspace/zp.md#beta) in workspace [ZP](../workspace/zp.md)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Label [BS2](main_flight_loop_part_3_of_16.md#bs2) in subroutine [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md)

[X]

Variable [BSTK](../workspace/up.md#bstk) in workspace [UP](../workspace/up.md)

Bitstik configuration setting

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Variable [JOPOS](../workspace/wp.md#jopos) in workspace [WP](../workspace/wp.md)

Contains the high byte of the latest value from ADC channel 1 (the joystick X value), which gets updated regularly by the IRQ1 interrupt handler

[X]

Variable [JSTX](../workspace/zp.md#jstx) in workspace [ZP](../workspace/zp.md)

Our current roll rate

[X]

Variable [JSTY](../workspace/zp.md#jsty) in workspace [ZP](../workspace/zp.md)

Our current pitch rate

[X]

Label [MA4](main_flight_loop_part_3_of_16.md#ma4) in subroutine [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md)

[X]

Subroutine [cntr](cntr.md) (category: Dashboard)

Apply damping to the pitch or roll dashboard indicator

[Main flight loop (Part 1 of 16)](main_flight_loop_part_1_of_16.md "Previous routine")[Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md "Next routine")
