---
title: "The BOOP subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/boop.html"
---

[DELAY](delay.md "Previous routine")[BEEP](beep.md "Next routine")
    
    
           Name: BOOP                                                    [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make a long, low beep
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-boop)
     References: This subroutine is called as follows:
                 * [HME2](hme2.md) calls BOOP
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls BOOP
                 * [WARP](warp.md) calls BOOP
    
    
    
    
    
    
    .BOOP
    
     LDY #soboop            \ Call the NOISE routine with Y = 0 to make a long, low
     BRA NOISE              \ beep, returning from the subroutine using a tail call
    

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Configuration variable [soboop](../../all/workspaces.md#soboop) = 0

Sound 0 = Long, low beep

[DELAY](delay.md "Previous routine")[BEEP](beep.md "Next routine")
