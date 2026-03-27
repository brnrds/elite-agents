---
title: "The ZINF subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/zinf.html"
---

[RES2](res2.md "Previous routine")[msblob](msblob.md "Next routine")
    
    
           Name: ZINF                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Reset the INWK workspace and orientation vectors
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-zinf)
     References: This subroutine is called as follows:
                 * [BRIEF](brief.md) calls ZINF
                 * [DOKEY](dokey.md) calls ZINF
                 * [FRS1](frs1.md) calls ZINF
                 * [HAS1](has1.md) calls ZINF
                 * [KS4](ks4.md) calls ZINF
                 * [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) calls ZINF
                 * [SOLAR](solar.md) calls ZINF
                 * [TITLE](title.md) calls ZINF
                 * [Ze](ze.md) calls ZINF
    
    
    
    
    
    * * *
    
    
     Zero-fill the INWK ship workspace and reset the orientation vectors, with
     nosev pointing out of the screen, towards us.
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is set to &FF
    
    
    
    
    .ZINF
    
     LDY #NI%-1             \ There are NI% bytes in the INWK workspace, so set a
                            \ counter in Y so we can loop through them
    
     LDA #0                 \ Set A to 0 so we can zero-fill the workspace
    
    .ZI1
    
     STA INWK,Y             \ Zero the Y-th byte of the INWK workspace
    
     DEY                    \ Decrement the loop counter
    
     BPL ZI1                \ Loop back for the next byte, ending when we have
                            \ zero-filled the last byte at INWK, which leaves Y
                            \ with a value of &FF
    
                            \ Finally, we reset the orientation vectors as follows:
                            \
                            \   sidev = (1,  0,  0)
                            \   roofv = (0,  1,  0)
                            \   nosev = (0,  0, -1)
                            \
                            \ 96 * 256 (&6000) represents 1 in the orientation
                            \ vectors, while -96 * 256 (&E000) represents -1. We
                            \ already set the vectors to zero above, so we just
                            \ need to set up the high bytes of the diagonal values
                            \ and we're done. The negative nosev makes the ship
                            \ point towards us, as the z-axis points into the screen
    
     LDA #96                \ Set A to represent a 1 (in vector terms)
    
     STA INWK+18            \ Set byte #18 = roofv_y_hi = 96 = 1
    
     STA INWK+22            \ Set byte #22 = sidev_x_hi = 96 = 1
    
     ORA #%10000000         \ Flip the sign of A to represent a -1
    
     STA INWK+14            \ Set byte #14 = nosev_z_hi = -96 = -1
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Label [ZI1](zinf.md#zi1) is local to this routine

[RES2](res2.md "Previous routine")[msblob](msblob.md "Next routine")
