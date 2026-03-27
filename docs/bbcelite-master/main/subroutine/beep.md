---
title: "The BEEP subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/beep.html"
---

[BOOP](boop.md "Previous routine")[SOFH](../variable/sofh.md "Next routine")
    
    
           Name: BEEP                                                    [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make a short, high beep
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-beep)
     Variations: See [code variations](../../related/compare/main/subroutine/beep.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CHPR](chpr.md) calls BEEP
                 * [DK4](dk4.md) calls BEEP
                 * [dn2](dn2.md) calls BEEP
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls BEEP
    
    
    
    
    
    
    .BEEP
    
     LDY #sobeep            \ Call the NOISE routine with Y = 1 to make a short,
     BRA NOISE              \ high beep, returning from the subroutine using a tail
                            \ call
    

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Configuration variable [sobeep](../../all/workspaces.md#sobeep) = 1

Sound 1 = Short, high beep

[BOOP](boop.md "Previous routine")[SOFH](../variable/sofh.md "Next routine")
