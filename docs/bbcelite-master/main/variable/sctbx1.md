---
title: "The SCTBX1 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/sctbx1.html"
---

[alogh](alogh.md "Previous routine")[SCTBX2](sctbx2.md "Next routine")
    
    
           Name: SCTBX1                                                  [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Lookup table for converting a pixel x-coordinate to the bit number
                 within the pixel row byte that corresponds to this pixel
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-sctbx1)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     This routine is not used in this version of Elite. It is left over from the
     Apple II version.
    
    
    
    
    .SCTBX1
    
    FOR I%, 0, 255
    
     EQUB (I% + 8) MOD 7
    
    NEXT
    

[alogh](alogh.md "Previous routine")[SCTBX2](sctbx2.md "Next routine")
