---
title: "The LASNO subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/lasno.html"
---

[ELASNO](elasno.md "Previous routine")[NOISE](noise.md "Next routine")
    
    
           Name: LASNO                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make the sound of our laser firing
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-lasno)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls LASNO
    
    
    
    
    
    
    .LASNO
    
     LDY #3                 \ Call the NOISE routine with Y = 3 to make the first
     JSR NOISE              \ sound of us firing our lasers
    
     LDY #5                 \ Set Y = 5 and fall through into the NOISE routine to
                            \ make the second sound of us firing our lasers
    

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[ELASNO](elasno.md "Previous routine")[NOISE](noise.md "Next routine")
