---
title: "The SCALEY2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/scaley2.html"
---

[SCALEY](scaley.md "Previous routine")[SCALEX](scalex.md "Next routine")
    
    
           Name: SCALEY2                                                 [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Scale the y-coordinate in A (leave it unchanged)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-scaley2)
     References: This subroutine is called as follows:
                 * [TT105](tt105.md) calls SCALEY2
                 * [TT14](tt14.md) calls SCALEY2
                 * [TT23](tt23.md) calls SCALEY2
    
    
    
    
    
    * * *
    
    
     This routine (and the related SCALEY and SCALEX routines) are called from
     various places in the code to scale the value in A. This code is different in
     the Apple II and BBC Master versions, and allows coordinates to be scaled
     correctly on different platforms.
    
     The original source contains the comment "SCALE Scans by 3/4 to fit in".
    
     In the Master version, the only scaling routine that does anything is SCALEY,
     which halves the y-coordinates in the Long-range Chart (as the galaxy is half
     as tall as it is wide). The other routines are left over from the Apple II
     version, which uses them to scale the system charts to fit into its smaller
     screen size.
    
    
    
    
    .SCALEY2
    
     RTS                    \ Return from the subroutine
    

[SCALEY](scaley.md "Previous routine")[SCALEX](scalex.md "Next routine")
