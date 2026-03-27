---
title: "The DEATH2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/death2.html"
---

[TT170](tt170.md "Previous routine")[BR1 (Part 1 of 2)](br1_part_1_of_2.md "Next routine")
    
    
           Name: DEATH2                                                  [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Reset most of the game and restart from the title screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-death2)
     Variations: See [code variations](../../related/compare/main/subroutine/death2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](death.md) calls DEATH2
                 * [DK4](dk4.md) calls DEATH2
    
    
    
    
    
    * * *
    
    
     This routine is called following death, and when the game is quit by pressing
     ESCAPE when paused.
    
    
    
    
    .DEATH2
    
     LDX #&FF               \ Set the stack pointer to &01FF, which is the standard
     TXS                    \ location for the 6502 stack, so this instruction
                            \ effectively resets the stack
    
     JSR RES2               \ Reset a number of flight variables and workspaces
                            \ and fall through into the entry code for the game
                            \ to restart from the title screen
    

[X]

Subroutine [RES2](res2.md) (category: Start and end)

Reset a number of flight variables and workspaces

[TT170](tt170.md "Previous routine")[BR1 (Part 1 of 2)](br1_part_1_of_2.md "Next routine")
