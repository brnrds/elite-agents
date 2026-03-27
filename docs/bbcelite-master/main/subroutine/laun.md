---
title: "The LAUN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/laun.html"
---

[LL164](ll164.md "Previous routine")[HFS2](hfs2.md "Next routine")
    
    
           Name: LAUN                                                    [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Make the launch sound and draw the launch tunnel
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-laun)
     Variations: See [code variations](../../related/compare/main/subroutine/laun.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOENTRY](doentry.md) calls LAUN
                 * [TT110](tt110.md) calls LAUN
    
    
    
    
    
    * * *
    
    
     This is shown when launching from or docking with the space station.
    
    
    
    
    .LAUN
    
     LDY #solaun            \ Call the NOISE routine with Y = 8 to make the sound
     JSR NOISE              \ of the ship launching from the station
    
     LDA #8                 \ Set the step size for the launch tunnel rings to 8, so
                            \ there are fewer sections in the rings and they are
                            \ quite polygonal (compared to the step size of 4 used
                            \ in the much rounder hyperspace rings)
    
                            \ Fall through into HFS2 to draw the launch tunnel rings
    

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Configuration variable [solaun](../../all/workspaces.md#solaun) = 8

Sound 8 = Missile launched / Ship launch

[LL164](ll164.md "Previous routine")[HFS2](hfs2.md "Next routine")
