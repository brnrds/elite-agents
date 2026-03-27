---
title: "The THERE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/there.html"
---

[KILLSHP](killshp.md "Previous routine")[NUMBOR](numbor.md "Next routine")
    
    
           Name: THERE                                                   [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Check whether we are in the Constrictor's system in mission 1
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-there)
     References: This subroutine is called as follows:
                 * [Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md) calls THERE
    
    
    
    
    
    * * *
    
    
     The stolen Constrictor is the target of mission 1. We finally track it down to
     the Orarra system in the second galaxy, which is at galactic coordinates
     (144, 33). This routine checks whether we are in this system and sets the C
     flag accordingly.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if we are in the Constrictor system, otherwise clear
    
    
    
    
    .THERE
    
     LDX GCNT               \ Set X = GCNT - 1
     DEX
    
     BNE THEX               \ If X is non-zero (i.e. GCNT is not 1, so we are not in
                            \ the second galaxy), then jump to THEX
    
     LDA QQ0                \ Set A = the current system's galactic x-coordinate
    
     CMP #144               \ If A <> 144 then jump to THEX
     BNE THEX
    
     LDA QQ1                \ Set A = the current system's galactic y-coordinate
    
     CMP #33                \ If A = 33 then set the C flag
    
     BEQ THEX+1             \ If A = 33 then jump to THEX+1, so we return from the
                            \ subroutine with the C flag set (otherwise we clear the
                            \ C flag with the next instruction)
    
    .THEX
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [GCNT](../workspace/wp.md#gcnt) in workspace [WP](../workspace/wp.md)

The number of the current galaxy (0-7)

[X]

Variable [QQ0](../workspace/wp.md#qq0) in workspace [WP](../workspace/wp.md)

The current system's galactic x-coordinate (0-256)

[X]

Variable [QQ1](../workspace/wp.md#qq1) in workspace [WP](../workspace/wp.md)

The current system's galactic y-coordinate (0-256)

[X]

Label [THEX](there.md#thex) is local to this routine

[X]

[KILLSHP](killshp.md "Previous routine")[NUMBOR](numbor.md "Next routine")
