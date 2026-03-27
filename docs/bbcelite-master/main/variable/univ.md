---
title: "The UNIV variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/univ.html"
---

[scacol](scacol.md "Previous routine")[FLKB](../subroutine/flkb.md "Next routine")
    
    
           Name: UNIV                                                    [Show more]
           Type: Variable
       Category: Universe
        Summary: Table of pointers to the local universe's ship data blocks
      Deep dive: [The local bubble of universe](https://elite.bbcelite.com/deep_dives/the_local_bubble_of_universe.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_b.md#header-univ)
     References: This variable is used as follows:
                 * [GINF](../subroutine/ginf.md) uses UNIV
                 * [KILLSHP](../subroutine/killshp.md) uses UNIV
                 * [KS2](../subroutine/ks2.md) uses UNIV
                 * [TACTICS (Part 1 of 7)](../subroutine/tactics_part_1_of_7.md) uses UNIV
    
    
    
    
    
    
    .UNIV
    
     FOR I%, 0, NOSH
    
      EQUW K% + I% * NI%    \ Address of block no. I%, of size NI%, in workspace K%
    
     NEXT
    

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[scacol](scacol.md "Previous routine")[FLKB](../subroutine/flkb.md "Next routine")
