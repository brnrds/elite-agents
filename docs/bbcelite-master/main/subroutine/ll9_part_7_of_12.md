---
title: "The LL9 (Part 7 of 12) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll9_part_7_of_12.html"
---

[LL62](ll62.md "Previous routine")[LL9 (Part 8 of 12)](ll9_part_8_of_12.md "Next routine")
    
    
           Name: LL9 (Part 7 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Calculate the visibility of each of the ship's vertices
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
                 [Calculating vertex coordinates](https://elite.bbcelite.com/deep_dives/calculating_vertex_coordinates.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll9-part-7-of-12)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section continues the coordinate adding from part 6 by finishing off the
     calculation that we started above:
    
                          [ sidev_x roofv_x nosev_x ]   [ x ]   [ x ]
       vector to vertex = [ sidev_y roofv_y nosev_y ] . [ y ] + [ y ]
                          [ sidev_z roofv_z nosev_z ]   [ z ]   [ z ]
    
     The gets stored as follows, in sign-magnitude values with the magnitudes
     fitting into the low bytes:
    
       XX15(2 0)           [ x y z ] . [ sidev_x roofv_x nosev_x ] + [ x y z ]
    
       XX15(5 3)           [ x y z ] . [ sidev_y roofv_y nosev_y ] + [ x y z ]
    
       (U T)               [ x y z ] . [ sidev_z roofv_z nosev_z ] + [ x y z ]
    
     Finally, because this vector is from our ship to the vertex, and we are at the
     origin, this vector is the same as the coordinates of the vertex. In other
     words, we have just worked out:
    
       XX15(2 0)           x-coordinate of the current vertex
    
       XX15(5 3)           y-coordinate of the current vertex
    
       (U T)               z-coordinate of the current vertex
    
    
    
    
    .LL56
    
     LDA XX1+6              \ Set (U T) = XX1(7 6) - XX12(5 4)
     SEC                    \           = (z_hi z_lo) - vertv_z
     SBC XX12+4             \
     STA T                  \ Starting with the low bytes
    
     LDA XX1+7              \ And then doing the high bytes (we can subtract 0 here
     SBC #0                 \ as we know the sign byte of vertv_z is 0)
     STA U
    
     BCC LL140              \ If the subtraction just underflowed, skip to LL140 to
                            \ set (U T) to the minimum value of 4
    
     BNE LL57               \ If U is non-zero, jump down to LL57
    
     LDA T                  \ If T >= 4, jump down to LL57
     CMP #4
     BCS LL57
    
    .LL140
    
     LDA #0                 \ If we get here then either (U T) < 4 or the
     STA U                  \ subtraction underflowed, so set (U T) = 4
     LDA #4
     STA T
    
    .LL57
    
                            \ By this point we have our results, so now to scale
                            \ the 16-bit results down into 8-bit values
    
     LDA U                  \ If the high bytes of the result are all zero, we are
     ORA XX15+1             \ done, so jump down to LL60 for the next stage
     ORA XX15+4
     BEQ LL60
    
     LSR XX15+1             \ Shift XX15(1 0) to the right
     ROR XX15
    
     LSR XX15+4             \ Shift XX15(4 3) to the right
     ROR XX15+3
    
     LSR U                  \ Shift (U T) to the right
     ROR T
    
     JMP LL57               \ Jump back to LL57 to see if we can shift the result
                            \ any more
    

[X]

Label [LL140](ll9_part_7_of_12.md#ll140) is local to this routine

[X]

Label [LL57](ll9_part_7_of_12.md#ll57) is local to this routine

[X]

Label [LL60](ll9_part_8_of_12.md#ll60) in subroutine [LL9 (Part 8 of 12)](ll9_part_8_of_12.md)

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [U](../workspace/zp.md#u) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX1](../workspace/zp.md#xx1) in workspace [ZP](../workspace/zp.md)

This is an alias for INWK that is used in the main ship-drawing routine at LL9

[X]

Variable [XX12](../workspace/zp.md#xx12) in workspace [ZP](../workspace/zp.md)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[LL62](ll62.md "Previous routine")[LL9 (Part 8 of 12)](ll9_part_8_of_12.md "Next routine")
