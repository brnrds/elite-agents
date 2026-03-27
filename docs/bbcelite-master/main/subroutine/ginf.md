---
title: "The GINF subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ginf.html"
---

[PAUSE2](pause2.md "Previous routine")[ping](ping.md "Next routine")
    
    
           Name: GINF                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Fetch the address of a ship's data block into INF
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-ginf)
     References: This subroutine is called as follows:
                 * [FRMIS](frmis.md) calls GINF
                 * [Main flight loop (Part 4 of 16)](main_flight_loop_part_4_of_16.md) calls GINF
                 * [NWSHP](nwshp.md) calls GINF
                 * [WPSHPS](wpshps.md) calls GINF
    
    
    
    
    
    * * *
    
    
     Get the address of the data block for ship slot X and store it in INF. This
     address is fetched from the UNIV table, which stores the addresses of the 13
     ship data blocks in workspace K%.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The ship slot number for which we want the data block
                            address
    
    
    
    
    .GINF
    
     TXA                    \ Set Y = X * 2
     ASL A
     TAY
    
     LDA UNIV,Y             \ Get the high byte of the address of the X-th ship
     STA INF                \ from UNIV and store it in INF
    
     LDA UNIV+1,Y           \ Get the low byte of the address of the X-th ship
     STA INF+1              \ from UNIV and store it in INF
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [UNIV](../variable/univ.md) (category: Universe)

Table of pointers to the local universe's ship data blocks

[PAUSE2](pause2.md "Previous routine")[ping](ping.md "Next routine")
