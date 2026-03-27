---
title: "The MAS2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mas2.html"
---

[MAS1](mas1.md "Previous routine")[MAS3](mas3.md "Next routine")
    
    
           Name: MAS2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate a cap on the maximum distance to the planet or sun
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-mas2)
     Variations: See [code variations](../../related/compare/main/subroutine/mas2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md) calls MAS2
                 * [Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md) calls MAS2
                 * [WARP](warp.md) calls MAS2
                 * [Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md) calls via m
                 * [WARP](warp.md) calls via m
    
    
    
    
    
    * * *
    
    
     Given a value in Y that points to the start of a ship data block as an offset
     from K%, calculate the following:
    
       A = A OR x_sign OR y_sign OR z_sign
    
     and clear the sign bit of the result. The K% workspace contains the ship data
     blocks, so the offset in Y must be 0 or a multiple of NI% (as each block in
     K% contains NI% bytes).
    
     The result effectively contains a maximum cap of the three values (though it
     might not be one of the three input values - it's just guaranteed to be
     larger than all of them).
    
     If Y = 0 and A = 0, then this calculates the maximum cap of the highest byte
     containing the distance to the planet, as K%+2 = x_sign, K%+5 = y_sign and
     K%+8 = z_sign (the first slot in the K% workspace represents the planet).
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The offset from K% for the start of the ship data block
                            to use
    
    
    
    * * *
    
    
     Returns:
    
       A                    A OR K%+2+Y OR K%+5+Y OR K%+8+Y, with bit 7 cleared
    
    
    
    * * *
    
    
     Other entry points:
    
       m                    Do not include A in the calculation
    
    
    
    
    .m
    
     LDA #0                 \ Set A = 0 and fall through into MAS2 to calculate the
                            \ OR of the three bytes at K%+2+Y, K%+5+Y and K%+8+Y
    
    .MAS2
    
     ORA K%+2,Y             \ Set A = A OR x_sign OR y_sign OR z_sign
     ORA K%+5,Y
     ORA K%+8,Y
    
     AND #%01111111         \ Clear bit 7 in A
    
     RTS                    \ Return from the subroutine
    

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[MAS1](mas1.md "Previous routine")[MAS3](mas3.md "Next routine")
