---
title: "The SPS4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sps4.html"
---

[SPS2](sps2.md "Previous routine")[SP1](sp1.md "Next routine")
    
    
           Name: SPS4                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the vector to the space station
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-sps4)
     References: This subroutine is called as follows:
                 * [SP1](sp1.md) calls SPS4
    
    
    
    
    
    * * *
    
    
     Calculate the vector between our ship and the space station and store it in
     XX15.
    
    
    
    
    .SPS4
    
     LDX #8                 \ First we need to copy the space station's coordinates
                            \ into K3, so set a counter to copy the first 9 bytes
                            \ (the three-byte x, y and z coordinates) from the
                            \ station's data block at K% + NI% into K3
    
    .SPL1
    
     LDA K%+NI%,X           \ Copy the X-th byte from the station's data block at
     STA K3,X               \ K% + NI% to the X-th byte of K3
    
     DEX                    \ Decrement the loop counter
    
     BPL SPL1               \ Loop back to SPL1 until we have copied all 9 bytes
    
     JMP TAS2               \ Call TAS2 to build XX15 from K3, returning from the
                            \ subroutine using a tail call
    

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Label [SPL1](sps4.md#spl1) is local to this routine

[X]

Subroutine [TAS2](tas2.md) (category: Maths (Geometry))

Normalise the three-coordinate vector in K3

[SPS2](sps2.md "Previous routine")[SP1](sp1.md "Next routine")
