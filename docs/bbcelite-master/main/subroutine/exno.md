---
title: "The EXNO subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/exno.html"
---

[EXNO3](exno3.md "Previous routine")[COLD](cold.md "Next routine")
    
    
           Name: EXNO                                                    [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make the sound of a laser strike on another ship or a ship
                 explosion
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-exno)
     Variations: See [code variations](../../related/compare/main/subroutine/exno.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls EXNO
    
    
    
    
    
    * * *
    
    
     Make the two-part explosion sound of us making a laser strike, or of another
     ship exploding.
    
    
    
    
    .EXNO
    
     LDY #sohit             \ Call the NOISE routine with Y = 6 to make the sound of
     JMP NOISE              \ us making a hit or kill and return from the subroutine
                            \ using a tail call
    

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Configuration variable [sohit](../../all/workspaces.md#sohit) = 6

Sound 6 = We made a hit/kill / Other ship exploding

[EXNO3](exno3.md "Previous routine")[COLD](cold.md "Next routine")
