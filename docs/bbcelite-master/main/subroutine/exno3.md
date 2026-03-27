---
title: "The EXNO3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/exno3.html"
---

[EXNO2](exno2.md "Previous routine")[EXNO](exno.md "Next routine")
    
    
           Name: EXNO3                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make an explosion sound
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-exno3)
     Variations: See [code variations](../../related/compare/main/subroutine/exno3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md) calls EXNO3
                 * [OOPS](oops.md) calls EXNO3
                 * [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md) calls EXNO3
    
    
    
    
    
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
    

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Configuration variable [soexpl](../../all/workspaces.md#soexpl) = 4

Sound 4 = We died / Collision / Being hit by lasers 2

[EXNO2](exno2.md "Previous routine")[EXNO](exno.md "Next routine")
