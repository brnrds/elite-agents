---
title: "The DELAY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/delay.html"
---

[WSCAN](wscan.md "Previous routine")[BOOP](boop.md "Next routine")
    
    
           Name: DELAY                                                   [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Wait for a specified time, in 1/50s of a second
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-delay)
     Variations: See [code variations](../../related/compare/main/subroutine/delay.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIS](bris.md) calls DELAY
                 * [DK4](dk4.md) calls DELAY
                 * [DKS3](dks3.md) calls DELAY
                 * [dn2](dn2.md) calls DELAY
                 * [DOENTRY](doentry.md) calls DELAY
                 * [Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md) calls DELAY
                 * [MT26](mt26.md) calls DELAY
                 * [TT217](tt217.md) calls DELAY
    
    
    
    
    
    * * *
    
    
     Wait for the number of vertical syncs given in Y, so this effectively waits
     for Y/50 of a second (as the vertical sync occurs 50 times a second).
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The number of vertical sync events to wait for
    
    
    
    
    .DELAY
    
     JSR WSCAN              \ Call WSCAN to wait for the vertical sync, so the whole
                            \ screen gets drawn
    
     DEY                    \ Decrement the counter in Y
    
     BNE DELAY              \ If Y isn't yet at zero, jump back to DELAY to wait
                            \ for another vertical sync
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [DELAY](delay.md) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Subroutine [WSCAN](wscan.md) (category: Drawing the screen)

Implement the #wscn command (wait for the vertical sync)

[WSCAN](wscan.md "Previous routine")[BOOP](boop.md "Next routine")
