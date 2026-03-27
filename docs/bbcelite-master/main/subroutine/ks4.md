---
title: "The KS4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ks4.html"
---

[KS1](ks1.md "Previous routine")[KS2](ks2.md "Next routine")
    
    
           Name: KS4                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Remove the space station and replace it with the sun
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-ks4)
     Variations: See [code variations](../../related/compare/main/subroutine/ks4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KILLSHP](killshp.md) calls KS4
    
    
    
    
    
    
    .KS4
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     JSR FLFLLS             \ Reset the LSO block, returns with A = 0
    
     STA FRIN+1             \ Set the second slot in the FRIN table to 0, which
                            \ sets this slot to empty, so when we call NWSHP below
                            \ the new sun that gets created will go into FRIN+1
    
     STA SSPR               \ Set the "space station present" flag to 0, as we are
                            \ no longer in the space station's safe zone
    
     JSR SPBLB              \ Call SPBLB to redraw the space station bulb, which
                            \ will erase it from the dashboard
    
     LDA #6                 \ Set the sun's y_sign to 6
     STA INWK+5
    
     LDA #129               \ Set A = 129, the ship type for the sun
    
     JMP NWSHP              \ Call NWSHP to set up the sun's data block and add it
                            \ to FRIN, where it will get put in the second slot as
                            \ we just cleared out the second slot, and the first
                            \ slot is already taken by the planet
    

[X]

Subroutine [FLFLLS](flflls.md) (category: Drawing suns)

Reset the sun line heap

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Subroutine [SPBLB](spblb.md) (category: Dashboard)

Light up the space station indicator ("S") on the dashboard

[X]

Variable [SSPR](../workspace/wp.md#sspr) in workspace [WP](../workspace/wp.md)

"Space station present" flag

[X]

Subroutine [ZINF](zinf.md) (category: Universe)

Reset the INWK workspace and orientation vectors

[KS1](ks1.md "Previous routine")[KS2](ks2.md "Next routine")
