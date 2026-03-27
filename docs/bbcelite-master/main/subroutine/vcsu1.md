---
title: "The VCSU1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/vcsu1.html"
---

[DOCKIT](dockit.md "Previous routine")[VCSUB](vcsub.md "Next routine")
    
    
           Name: VCSU1                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate vector K3(8 0) = [x y z] - coordinates of the sun or
                 space station
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-vcsu1)
     References: This subroutine is called as follows:
                 * [DOCKIT](dockit.md) calls VCSU1
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate of the sun or space station
    
       K3(5 4 3) = (y_sign y_hi z_lo) - y-coordinate of the sun or space station
    
       K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate of the sun or space station
    
     where the first coordinate is from the ship data block in INWK, and the second
     coordinate is from the sun or space station's ship data block which they
     share.
    
    
    
    
    .VCSU1
    
     LDA #LO(K%+NI%)        \ Set the low byte of V(1 0) to point to the coordinates
     STA V                  \ of the sun or space station
    
     LDA #HI(K%+NI%)        \ Set A to the high byte of the address of the
                            \ coordinates of the sun or space station
    
                            \ Fall through into VCSUB to calculate:
                            \
                            \   K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate of sun
                            \               or space station
                            \
                            \   K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate of sun
                            \               or space station
                            \
                            \   K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate of sun
                            \               or space station
    

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Variable [V](../workspace/zp.md#v) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing an address pointer

[DOCKIT](dockit.md "Previous routine")[VCSUB](vcsub.md "Next routine")
