---
title: "The GTHG subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/gthg.html"
---

[GVL](gvl.md "Previous routine")[MJP](mjp.md "Next routine")
    
    
           Name: GTHG                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Spawn a Thargoid ship and a Thargon companion
      Deep dive: [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-gthg)
     References: This subroutine is called as follows:
                 * [Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md) calls GTHG
                 * [MJP](mjp.md) calls GTHG
    
    
    
    
    
    
    .GTHG
    
     JSR Ze                 \ Call Ze to initialise INWK to a fairly aggressive
                            \ ship (though we increase this below)
                            \
                            \ Note that because Ze uses the value of X returned by
                            \ DORND, and X contains the value of A returned by the
                            \ previous call to DORND, this does not set the new ship
                            \ to a totally random location
    
     LDA #%11111111         \ Set the AI flag in byte #32 so that the ship has AI,
     STA INWK+32            \ an aggression level of 63 out of 63, and E.C.M.
    
     LDA #THG               \ Call NWSHP to add a new Thargoid ship to our local
     JSR NWSHP              \ bubble of universe
    
     LDA #TGL               \ Call NWSHP to add a new Thargon ship to our local
     JMP NWSHP              \ bubble of universe, and return from the subroutine
                            \ using a tail call
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Configuration variable [TGL](../../all/workspaces.md#tgl) = 30

Ship type for a Thargon

[X]

Configuration variable [THG](../../all/workspaces.md#thg) = 29

Ship type for a Thargoid

[X]

Subroutine [Ze](ze.md) (category: Universe)

Initialise the INWK workspace to a fairly aggressive ship

[GVL](gvl.md "Previous routine")[MJP](mjp.md "Next routine")
