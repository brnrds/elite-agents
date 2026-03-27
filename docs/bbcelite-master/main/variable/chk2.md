---
title: "The CHK2 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/chk2.html"
---

[NA%](na_per_cent.md "Previous routine")[CHK](chk.md "Next routine")
    
    
           Name: CHK2                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: Second checksum byte for the saved commander data file
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
                 [The competition code](https://elite.bbcelite.com/deep_dives/the_competition_code.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-chk2)
     Variations: See [code variations](../../related/compare/main/variable/chk2.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DFAULT](../subroutine/dfault.md) uses CHK2
                 * [SVE](../subroutine/sve.md) uses CHK2
    
    
    
    
    
    * * *
    
    
     Second commander checksum byte. If the default commander is changed, a new
     checksum will be calculated and inserted by the elite-checksum.py script.
    
     The offset of this byte within a saved commander file is also shown (it's at
     byte #74).
    
    
    
    
    .CHK2
    
     EQUB 0                 \ Placeholder for the checksum in byte #74
    

[NA%](na_per_cent.md "Previous routine")[CHK](chk.md "Next routine")
