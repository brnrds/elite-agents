---
title: "The K% workspace - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/workspace/k_per_cent.html"
---

[XX3](xx3.md "Previous routine")[WP](wp.md "Next routine")
    
    
           Name: K%                                                      [Show more]
           Type: Workspace
        Address: &0400 to &05BB
       Category: Workspaces
        Summary: Ship data blocks and ship line heaps
      Deep dive: [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [The local bubble of universe](https://elite.bbcelite.com/deep_dives/the_local_bubble_of_universe.html)
    
    
        Context: See this workspace [in context in the source code](../../all/workspaces.md#header-k-per-cent)
     Variations: See [code variations](../../related/compare/main/workspace/k_per_cent.md) for this workspace in the different versions
     References: This workspace is used as follows:
                 * [ANGRY](../subroutine/angry.md) uses K%
                 * [DCS1](../subroutine/dcs1.md) uses K%
                 * [DOEXP](../subroutine/doexp.md) uses K%
                 * [Main flight loop (Part 1 of 16)](../subroutine/main_flight_loop_part_1_of_16.md) uses K%
                 * [Main flight loop (Part 9 of 16)](../subroutine/main_flight_loop_part_9_of_16.md) uses K%
                 * [Main flight loop (Part 14 of 16)](../subroutine/main_flight_loop_part_14_of_16.md) uses K%
                 * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md) uses K%
                 * [MAS2](../subroutine/mas2.md) uses K%
                 * [MAS3](../subroutine/mas3.md) uses K%
                 * [SPS3](../subroutine/sps3.md) uses K%
                 * [SPS4](../subroutine/sps4.md) uses K%
                 * [TAS4](../subroutine/tas4.md) uses K%
                 * [UNIV](../variable/univ.md) uses K%
                 * [VCSU1](../subroutine/vcsu1.md) uses K%
                 * [WARP](../subroutine/warp.md) uses K%
    
    
    
    
    
    * * *
    
    
     Contains ship data for all the ships, planets, suns and space stations in our
     local bubble of universe.
    
     The blocks are pointed to by the lookup table at location UNIV. The first 444
     bytes of the K% workspace hold ship data on up to 12 ships, with 37 (NI%)
     bytes per ship.
    
    
    
    
     ORG &0400              \ Set the assembly address to &0400
    
    .K%
    
     SKIP NOSH * NI%        \ Ship data blocks and ship line heap
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../subroutine/angry.md)
                            \   * [DCS1](../subroutine/dcs1.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [Main flight loop (Part 1 of 16)](../subroutine/main_flight_loop_part_1_of_16.md)
                            \   * [Main flight loop (Part 9 of 16)](../subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [Main flight loop (Part 14 of 16)](../subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \   * [MAS2](../subroutine/mas2.md)
                            \   * [MAS3](../subroutine/mas3.md)
                            \   * [SPS3](../subroutine/sps3.md)
                            \   * [SPS4](../subroutine/sps4.md)
                            \   * [TAS4](../subroutine/tas4.md)
                            \   * [UNIV](../variable/univ.md)
                            \   * [VCSU1](../subroutine/vcsu1.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     PRINT "K% workspace from ", ~K%, "to ", ~P%-1, "inclusive"
    

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Configuration variable [NOSH](../../all/workspaces.md#nosh) = 12

The maximum number of ships in our local bubble of universe

[XX3](xx3.md "Previous routine")[WP](wp.md "Next routine")
