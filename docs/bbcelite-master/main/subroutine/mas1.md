---
title: "The MAS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mas1.html"
---

[STARS6](stars6.md "Previous routine")[MAS2](mas2.md "Next routine")
    
    
           Name: MAS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Add an orientation vector coordinate to an INWK coordinate
      Deep dive: [The space station safe zone](https://elite.bbcelite.com/deep_dives/the_space_station_safe_zone.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-mas1)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md) calls MAS1
    
    
    
    
    
    * * *
    
    
     Add a doubled nosev vector coordinate, e.g. (nosev_y_hi nosev_y_lo) * 2, to
     an INWK coordinate, e.g. (x_sign x_hi x_lo), storing the result in the INWK
     coordinate. The axes used in each side of the addition are specified by the
     arguments X and Y.
    
     In the comments below, we document the routine as if we are doing the
     following, i.e. if X = 0 and Y = 11:
    
       (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (nosev_y_hi nosev_y_lo) * 2
    
     as that way the variable names in the comments contain "x" and "y" to match
     the registers that specify the vector axis to use.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The coordinate to add, as follows:
    
                              * If X = 0, add (x_sign x_hi x_lo)
                              * If X = 3, add (y_sign y_hi y_lo)
                              * If X = 6, add (z_sign z_hi z_lo)
    
       Y                    The vector to add, as follows:
    
                              * If Y = 9,  add (nosev_x_hi nosev_x_lo)
                              * If Y = 11, add (nosev_y_hi nosev_y_lo)
                              * If Y = 13, add (nosev_z_hi nosev_z_lo)
    
    
    
    * * *
    
    
     Returns:
    
       A                    The highest byte of the result with the sign cleared
                            (e.g. |x_sign| when X = 0, etc.)
    
    
    
    * * *
    
    
     Other entry points:
    
       MA9                  Contains an RTS
    
    
    
    
    .MAS1
    
     LDA INWK,Y             \ Set K(2 1) = (nosev_y_hi nosev_y_lo) * 2
     ASL A
     STA K+1
     LDA INWK+1,Y
     ROL A
     STA K+2
    
     LDA #0                 \ Set K+3 bit 7 to the C flag, so the sign bit of the
     ROR A                  \ above result goes into K+3
     STA K+3
    
     JSR MVT3               \ Add (x_sign x_hi x_lo) to K(3 2 1)
    
     STA INWK+2,X           \ Store the sign of the result in x_sign
    
     LDY K+1                \ Store K(2 1) in (x_hi x_lo)
     STY INWK,X
     LDY K+2
     STY INWK+1,X
    
     AND #%01111111         \ Set A to the sign byte with the sign cleared,
                            \ i.e. |x_sign| when X = 0
    
    .MA9
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [MVT3](mvt3.md) (category: Moving)

Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)

[STARS6](stars6.md "Previous routine")[MAS2](mas2.md "Next routine")
