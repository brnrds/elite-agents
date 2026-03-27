---
title: "The CHECK subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/check.html"
---

[TITLE](title.md "Previous routine")[CHECK2](check2.md "Next routine")
    
    
           Name: CHECK                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Calculate the checksum for the last saved commander data block
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-check)
     Variations: See [code variations](../../related/compare/main/subroutine/check.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DFAULT](dfault.md) calls CHECK
                 * [SVE](sve.md) calls CHECK
    
    
    
    
    
    * * *
    
    
     The checksum for the last saved commander data block is saved as part of the
     commander file, in two places (CHK AND CHK2), to protect against file
     tampering. This routine calculates the checksum and returns it in A.
    
     This algorithm is also implemented in elite-checksum.py.
    
    
    
    * * *
    
    
     Returns:
    
       A                    The checksum for the last saved commander data block
    
    
    
    
    .CHECK
    
     LDX #NT%-3             \ Set X to the size of the commander data block, less
                            \ 3 (as there are two checksum bytes and the save count)
    
     CLC                    \ Clear the C flag so we can do addition without the
                            \ C flag affecting the result
    
     TXA                    \ Seed the checksum calculation by setting A to the
                            \ size of the commander data block, less 2
    
                            \ We now loop through the commander data block,
                            \ starting at the end and looping down to the start
                            \ (so at the start of this loop, the X-th byte is the
                            \ last byte of the commander data block, i.e. the save
                            \ count)
    
    .QUL2
    
     ADC NA%+7,X            \ Add the X-1-th byte of the data block to A, plus the
                            \ C flag
    
     EOR NA%+8,X            \ EOR A with the X-th byte of the data block
    
     DEX                    \ Decrement the loop counter
    
     BNE QUL2               \ Loop back for the next byte in the calculation, until
                            \ we have added byte #0 and EOR'd with byte #1 of the
                            \ data block
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [NA%](../variable/na_per_cent.md) (category: Save and load)

The data block for the last saved commander

[X]

Configuration variable [NT%](../workspace/wp.md#nt-per-cent) in workspace [WP](../workspace/wp.md)

This sets the variable NT% to the size of the current commander data block, which starts at TP and ends at SVC+3 (inclusive), i.e. with the last checksum byte

[X]

Label [QUL2](check.md#qul2) is local to this routine

[TITLE](title.md "Previous routine")[CHECK2](check2.md "Next routine")
