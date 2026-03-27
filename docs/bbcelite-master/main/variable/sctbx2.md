---
title: "The SCTBX2 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/sctbx2.html"
---

[SCTBX1](sctbx1.md "Previous routine")[wtable](wtable.md "Next routine")
    
    
           Name: SCTBX2                                                  [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Lookup table for converting a pixel x-coordinate to the byte
                 number in the pixel row that corresponds to this pixel
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-sctbx2)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     This routine is not used in this version of Elite. It is left over from the
     Apple II version.
    
    
    
    
    .SCTBX2
    
    FOR I%, 0, 255
    
     EQUB (I% + 8) DIV 7
    
    NEXT
    

[SCTBX1](sctbx1.md "Previous routine")[wtable](wtable.md "Next routine")
