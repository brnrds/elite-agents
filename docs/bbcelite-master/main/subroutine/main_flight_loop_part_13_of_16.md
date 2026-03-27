---
title: "The Main flight loop (Part 13 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_13_of_16.html"
---

[Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md "Previous routine")[Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 13 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Show energy bomb effect, charge shields and energy banks
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Scheduling tasks with the main loop counter](https://elite.bbcelite.com/deep_dives/scheduling_tasks_with_the_main_loop_counter.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-13-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_13_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Show energy bomb effect (if applicable)
    
       * Charge shields and energy banks (every 7 iterations of the main loop)
    
    
    
    
    .MA18
    
     LDA BOMB               \ If we set off our energy bomb (see MA24 above), then
     BPL MA77               \ BOMB is now negative, so this skips to MA21 if our
                            \ energy bomb is not going off
    
     JSR BOMBEFF2           \ Call BOMBEFF2 to erase the energy bomb zig-zag
                            \ lightning bolt that we drew in part 3, make the sound
                            \ of the energy bomb going off, draw a new lightning
                            \ bolt, and repeat the process four times so the bolt
                            \ flashes
    
     ASL BOMB               \ We set off our energy bomb, so rotate BOMB to the
                            \ left by one place. BOMB was rotated left once already
                            \ during this iteration of the main loop, back at MA24,
                            \ so if this is the first pass it will already be
                            \ %11111110, and this will shift it to %11111100 - so
                            \ if we set off an energy bomb, it stays activated
                            \ (BOMB > 0) for four iterations of the main loop
    
     BMI MA77               \ If the result has bit 7 set, skip the following
                            \ instruction as the bomb is still going off
    
     JSR BOMBOFF            \ Our energy bomb has finished going off, so call
                            \ BOMBOFF to draw the zig-zag lightning bolt, which
                            \ erases it from the screen
    
    .MA77
    
     LDA MCNT               \ Fetch the main loop counter and calculate MCNT mod 8,
     AND #7                 \ jumping to MA22 if it is non-zero (so the following
     BNE MA22               \ code only runs every 8 iterations of the main loop)
    
     LDX ENERGY             \ Fetch our ship's energy levels and skip to b if bit 7
     BPL b                  \ is not set, i.e. only charge the shields from the
                            \ energy banks if they are at more than 50% charge
    
     LDX ASH                \ Call SHD to recharge our aft shield and update the
     JSR SHD                \ shield status in ASH
     STX ASH
    
     LDX FSH                \ Call SHD to recharge our forward shield and update
     JSR SHD                \ the shield status in FSH
     STX FSH
    
    .b
    
     SEC                    \ Set A = ENERGY + ENGY + 1, so our ship's energy
     LDA ENGY               \ level goes up by 2 if we have an energy unit fitted,
     ADC ENERGY             \ otherwise it goes up by 1
    
     BCS paen1              \ If the value of A did not overflow (the maximum
     STA ENERGY             \ energy level is &FF), then store A in ENERGY
    
    .paen1
    

[X]

Variable [ASH](../workspace/zp.md#ash) in workspace [ZP](../workspace/zp.md)

Aft shield status

[X]

Variable [BOMB](../workspace/wp.md#bomb) in workspace [WP](../workspace/wp.md)

Energy bomb

[X]

Subroutine [BOMBEFF2](bombeff2.md) (category: Drawing lines)

Erase the energy bomb zig-zag lightning bolt, make the sound of the energy bomb going off, draw a new bolt and repeat four times

[X]

Subroutine [BOMBOFF](bomboff.md) (category: Drawing lines)

Draw the zig-zag lightning bolt for the energy bomb

[X]

Variable [ENERGY](../workspace/zp.md#energy) in workspace [ZP](../workspace/zp.md)

Energy bank status

[X]

Variable [ENGY](../workspace/wp.md#engy) in workspace [WP](../workspace/wp.md)

Energy unit

[X]

Variable [FSH](../workspace/zp.md#fsh) in workspace [ZP](../workspace/zp.md)

Forward shield status

[X]

Label [MA22](main_flight_loop_part_15_of_16.md#ma22) in subroutine [Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md)

[X]

Label [MA77](main_flight_loop_part_13_of_16.md#ma77) is local to this routine

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Subroutine [SHD](shd.md) (category: Flight)

Charge a shield and drain some energy from the energy banks

[X]

Label [b](main_flight_loop_part_13_of_16.md#b) is local to this routine

[X]

Label [paen1](main_flight_loop_part_13_of_16.md#paen1) is local to this routine

[Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md "Previous routine")[Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md "Next routine")
