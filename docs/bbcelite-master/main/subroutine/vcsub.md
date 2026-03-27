---
title: "The VCSUB subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/vcsub.html"
---

[VCSU1](vcsu1.md "Previous routine")[TAS1](tas1.md "Next routine")
    
    
           Name: VCSUB                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate vector K3(8 0) = [x y z] - coordinates in (A V)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-vcsub)
     References: This subroutine is called as follows:
                 * [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md) calls VCSUB
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate in (A V)
    
       K3(5 4 3) = (y_sign y_hi z_lo) - y-coordinate in (A V)
    
       K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate in (A V)
    
     where the first coordinate is from the ship data block in INWK, and the second
     coordinate is from the ship data block pointed to by (A V).
    
    
    
    
    .VCSUB
    
     STA V+1                \ Set the low byte of V(1 0) to A, so now V(1 0) = (A V)
    
     LDY #2                 \ K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate in data
     JSR TAS1               \ block at V(1 0)
    
     LDY #5                 \ K3(5 4 3) = (y_sign y_hi z_lo) - y-coordinate of data
     JSR TAS1               \ block at V(1 0)
    
     LDY #8                 \ Fall through into TAS1 to calculate the final result:
                            \
                            \ K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate of data
                            \ block at V(1 0)
    

[X]

Subroutine [TAS1](tas1.md) (category: Maths (Arithmetic))

Calculate K3 = (x_sign x_hi x_lo) - V(1 0)

[X]

Variable [V](../workspace/zp.md#v) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing an address pointer

[VCSU1](vcsu1.md "Previous routine")[TAS1](tas1.md "Next routine")
