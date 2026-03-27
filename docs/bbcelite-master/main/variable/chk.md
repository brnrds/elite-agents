---
title: "The CHK variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/chk.html"
---

[CHK2](chk2.md "Previous routine")[S1%](s1_per_cent.md "Next routine")
    
    
           Name: CHK                                                     [Show more]
           Type: Variable
       Category: Save and load
        Summary: First checksum byte for the saved commander data file
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
                 [The competition code](https://elite.bbcelite.com/deep_dives/the_competition_code.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-chk)
     Variations: See [code variations](../../related/compare/main/variable/chk.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DFAULT](../subroutine/dfault.md) uses CHK
                 * [SVE](../subroutine/sve.md) uses CHK
    
    
    
    
    
    * * *
    
    
     Commander checksum byte. If the default commander is changed, a new checksum
     will be calculated and inserted by the elite-checksum.py script.
    
     The offset of this byte within a saved commander file is also shown (it's at
     byte #75).
    
    
    
    
    .CHK
    
     EQUB 0                 \ Placeholder for the checksum in byte #75
    
    \.CHK3                  \ These instructions are commented out in the original
    \                       \ source
    \EQUB 0
    
     SKIP 12                \ These bytes appear to be unused, though the first byte
                            \ in this block is included in the commander file (it
                            \ has no effect, as it's the third checksum byte from
                            \ the Commodore 64 version, which isn't used in the
                            \ Master version)
    

[CHK2](chk2.md "Previous routine")[S1%](s1_per_cent.md "Next routine")
