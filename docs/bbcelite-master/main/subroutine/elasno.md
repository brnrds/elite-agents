---
title: "The ELASNO subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/elasno.html"
---

[SOFLUSH](soflush.md "Previous routine")[LASNO](lasno.md "Next routine")
    
    
           Name: ELASNO                                                  [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make the sound of us being hit
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-elasno)
     References: This subroutine is called as follows:
                 * [TACTICS (Part 6 of 7)](tactics_part_6_of_7.md) calls ELASNO
    
    
    
    
    
    
    .ELASNO
    
     LDY #9                 \ Call the NOISE routine with Y = 9 to make the first
     JSR NOISE              \ sound of us being hit
    
     LDY #5                 \ Call the NOISE routine with Y = 5 to make the second
     BRA NOISE              \ sound of us being hit, returning from the subroutine
                            \ using a tail call
    

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[SOFLUSH](soflush.md "Previous routine")[LASNO](lasno.md "Next routine")
