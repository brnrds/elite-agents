---
title: "The Main flight loop (Part 1 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_1_of_16.html"
---

[SPMASK](../variable/spmask.md "Previous routine")[Main flight loop (Part 2 of 16)](main_flight_loop_part_2_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 1 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Seed the random number generator
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Generating random numbers](https://elite.bbcelite.com/deep_dives/generating_random_numbers.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-1-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_1_of_16.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](death.md) calls via M%
                 * [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) calls via M%
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Seed the random number generator
    
    
    
    * * *
    
    
     Other entry points:
    
       M%                   The entry point for the main flight loop
    
    
    
    
    .M%
    
     LDA K%                 \ We want to seed the random number generator with a
                            \ pretty random number, so fetch the contents of K%,
                            \ which is the x_lo coordinate of the planet. This value
                            \ will be fairly unpredictable, so it's a pretty good
                            \ candidate
    
     STA RAND               \ Store the seed in the first byte of the four-byte
                            \ random number seed that's stored in RAND
    
    \LDA TRIBCT             \ These instructions are commented out in the original
    \BEQ NOMVETR            \ source
    \JMP MVTRIBS
    \
    \.NOMVETR
    

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Variable [RAND](../workspace/zp.md#rand) in workspace [ZP](../workspace/zp.md)

Four 8-bit seeds for the random number generation system implemented in the DORND routine

[SPMASK](../variable/spmask.md "Previous routine")[Main flight loop (Part 2 of 16)](main_flight_loop_part_2_of_16.md "Next routine")
