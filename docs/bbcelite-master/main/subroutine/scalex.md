---
title: "The SCALEX subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/scalex.html"
---

[SCALEY2](scaley2.md "Previous routine")[DVLOIN](dvloin.md "Next routine")
    
    
           Name: SCALEX                                                  [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Scale the x-coordinate in A (leave it unchanged)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-scalex)
     References: This subroutine is called as follows:
                 * [TT103](tt103.md) calls SCALEX
                 * [TT14](tt14.md) calls SCALEX
                 * [TT22](tt22.md) calls SCALEX
    
    
    
    
    
    * * *
    
    
     This routine (and the related SCALEY and SCALEY2 routines) are called from
     various places in the code to scale the value in A. This code is different in
     the Apple II and BBC Master versions, and allows coordinates to be scaled
     correctly on different platforms.
    
     In the Master version, the only scaling routine that does anything is SCALEY,
     which halves the y-coordinates in the Long-range Chart (as the galaxy is half
     as tall as it is wide). The other routines are left over from the Apple II
     version, which uses them to scale the system charts to fit into its smaller
     screen size.
    
    
    
    
    .SCALEX
    
     RTS                    \ Return from the subroutine
    

[SCALEY2](scaley2.md "Previous routine")[DVLOIN](dvloin.md "Next routine")
