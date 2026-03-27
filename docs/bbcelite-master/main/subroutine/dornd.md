---
title: "The DORND subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dornd.html"
---

[Ze](ze.md "Previous routine")[Main game loop (Part 1 of 6)](main_game_loop_part_1_of_6.md "Next routine")
    
    
           Name: DORND                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Generate random numbers
      Deep dive: [Generating random numbers](https://elite.bbcelite.com/deep_dives/generating_random_numbers.html)
                 [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-dornd)
     References: This subroutine is called as follows:
                 * [BOMBON](bombon.md) calls DORND
                 * [DEATH](death.md) calls DORND
                 * [DETOK2](detok2.md) calls DORND
                 * [GVL](gvl.md) calls DORND
                 * [HALL](hall.md) calls DORND
                 * [HAS1](has1.md) calls DORND
                 * [LASLI](lasli.md) calls DORND
                 * [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) calls DORND
                 * [Main flight loop (Part 8 of 16)](main_flight_loop_part_8_of_16.md) calls DORND
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls DORND
                 * [Main game loop (Part 1 of 6)](main_game_loop_part_1_of_6.md) calls DORND
                 * [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) calls DORND
                 * [Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md) calls DORND
                 * [MT18](mt18.md) calls DORND
                 * [nWq](nwq.md) calls DORND
                 * [OUCH](ouch.md) calls DORND
                 * [SFS1](sfs1.md) calls DORND
                 * [SOLAR](solar.md) calls DORND
                 * [SPIN](spin.md) calls DORND
                 * [STARS1](stars1.md) calls DORND
                 * [STARS2](stars2.md) calls DORND
                 * [STARS6](stars6.md) calls DORND
                 * [SUN (Part 3 of 4)](sun_part_3_of_4.md) calls DORND
                 * [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md) calls DORND
                 * [TACTICS (Part 2 of 7)](tactics_part_2_of_7.md) calls DORND
                 * [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md) calls DORND
                 * [TACTICS (Part 4 of 7)](tactics_part_4_of_7.md) calls DORND
                 * [TACTICS (Part 5 of 7)](tactics_part_5_of_7.md) calls DORND
                 * [TACTICS (Part 7 of 7)](tactics_part_7_of_7.md) calls DORND
                 * [TT18](tt18.md) calls DORND
                 * [TT210](tt210.md) calls DORND
                 * [Ze](ze.md) calls DORND
    
    
    
    
    
    * * *
    
    
     Set A and X to random numbers (though note that X is set to the random number
     that was returned in A the last time DORND was called).
    
     The C and V flags are also set randomly.
    
     If we want to generate a repeatable sequence of random numbers, when
     generating explosion clouds, for example, then we call DORND2 to ensure that
     the value of the C flag on entry doesn't affect the outcome, as otherwise we
     might not get the same sequence of numbers if the C flag changes.
    
    
    
    * * *
    
    
     Other entry points:
    
       DORND2               Make sure the C flag doesn't affect the outcome
    
    
    
    
    .DORND2
    
     CLC                    \ Clear the C flag so the value of the C flag on entry
                            \ doesn't affect the outcome
    
    .DORND
    
     LDA RAND               \ Calculate the next two values f2 and f3 in the feeder
     ROL A                  \ sequence:
     TAX                    \
     ADC RAND+2             \   * f2 = (f1 << 1) mod 256 + C flag on entry
     STA RAND               \   * f3 = f0 + f2 + (1 if bit 7 of f1 is set)
     STX RAND+2             \   * C flag is set according to the f3 calculation
    
     LDA RAND+1             \ Calculate the next value m2 in the main sequence:
     TAX                    \
     ADC RAND+3             \   * A = m2 = m0 + m1 + C flag from feeder calculation
     STA RAND+1             \   * X = m1
     STX RAND+3             \   * C and V flags set according to the m2 calculation
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [RAND](../workspace/zp.md#rand) in workspace [ZP](../workspace/zp.md)

Four 8-bit seeds for the random number generation system implemented in the DORND routine

[Ze](ze.md "Previous routine")[Main game loop (Part 1 of 6)](main_game_loop_part_1_of_6.md "Next routine")
