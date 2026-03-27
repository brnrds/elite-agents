---
title: "The MAS4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mas4.html"
---

[FAROF2](farof2.md "Previous routine")[stackpt](../variable/stackpt.md "Next routine")
    
    
           Name: MAS4                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate a cap on the maximum distance to a ship
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-mas4)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 7 of 16)](main_flight_loop_part_7_of_16.md) calls MAS4
                 * [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md) calls MAS4
                 * [TACTICS (Part 6 of 7)](tactics_part_6_of_7.md) calls MAS4
    
    
    
    
    
    * * *
    
    
     Logical OR the value in A with the high bytes of the ship's position (x_hi,
     y_hi and z_hi).
    
    
    
    * * *
    
    
     Returns:
    
       A                    A OR x_hi OR y_hi OR z_hi
    
    
    
    
    .MAS4
    
     ORA INWK+1             \ OR A with x_hi, y_hi and z_hi
     ORA INWK+4
     ORA INWK+7
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[FAROF2](farof2.md "Previous routine")[stackpt](../variable/stackpt.md "Next routine")
