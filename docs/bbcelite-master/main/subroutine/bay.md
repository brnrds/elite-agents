---
title: "The BAY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bay.html"
---

[BR1 (Part 2 of 2)](br1_part_2_of_2.md "Previous routine")[DFAULT](dfault.md "Next routine")
    
    
           Name: BAY                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Go to the docking bay (i.e. show the Status Mode screen)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-bay)
     Variations: See [code variations](../../related/compare/main/subroutine/bay.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRP](brp.md) calls BAY
                 * [DOENTRY](doentry.md) calls BAY
                 * [EQSHP](eqshp.md) calls BAY
                 * [TT102](tt102.md) calls BAY
    
    
    
    
    
    * * *
    
    
     We end up here after the start-up process (load commander etc.), as well as
     after a successful save, an escape pod launch, a successful docking, the end
     of a cargo sell, and various errors (such as not having enough cash, entering
     too many items when buying, trying to fit an item to your ship when you
     already have it, running out of cargo space, and so on).
    
    
    
    
    .BAY
    
     LDA #&FF               \ Set QQ12 = &FF (the docked flag) to indicate that we
     STA QQ12               \ are docked
    
     LDA #f8                \ Jump into the main loop at FRCE, setting the key
     JMP FRCE               \ that's "pressed" to red key f8 (so we show the Status
                            \ Mode screen)
    

[X]

Entry point [FRCE](main_game_loop_part_6_of_6.md#frce) in subroutine [Main game loop (Part 6 of 6)](main_game_loop_part_6_of_6.md) (category: Main loop)

The entry point for the main game loop if we want to jump straight to a specific screen, by pretending to "press" a key, in which case A contains the internal key number of the key we want to "press"

[X]

Variable [QQ12](../workspace/zp.md#qq12) in workspace [ZP](../workspace/zp.md)

Our "docked" status

[X]

Configuration variable [f8](../../all/workspaces.md#f8) = &88

Internal key number for red key f8 (Status Mode)

[BR1 (Part 2 of 2)](br1_part_2_of_2.md "Previous routine")[DFAULT](dfault.md "Next routine")
