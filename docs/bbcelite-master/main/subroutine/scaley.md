---
title: "The SCALEY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/scaley.html"
---

[MTIN](../variable/mtin.md "Previous routine")[SCALEY2](scaley2.md "Next routine")
    
    
           Name: SCALEY                                                  [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Scale the y-coordinate in A to 0.5 * A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-scaley)
     References: This subroutine is called as follows:
                 * [TT103](tt103.md) calls SCALEY
                 * [TT14](tt14.md) calls SCALEY
                 * [TT22](tt22.md) calls SCALEY
    
    
    
    
    
    * * *
    
    
     This routine (and the related SCALEY2 and SCALEX routines) are called from
     various places in the code to scale the value in A. This code is different in
     the Apple II and BBC Master versions, and allows coordinates to be scaled
     correctly on different platforms.
    
     In the Master version, the only scaling routine that does anything is SCALEY,
     which halves the y-coordinates in the Long-range Chart (as the galaxy is half
     as tall as it is wide). The other routines are left over from the Apple II
     version, which uses them to scale the system charts to fit into its smaller
     screen size.
    
    
    
    
    .SCALEY
    
     LSR A                  \ Halve the value in A
    
                            \ Fall through into SCALEY2 to return from the
                            \ subroutine
    

[MTIN](../variable/mtin.md "Previous routine")[SCALEY2](scaley2.md "Next routine")
