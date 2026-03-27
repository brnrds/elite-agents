---
title: "The NWSTARS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nwstars.html"
---

[SOLAR](solar.md "Previous routine")[nWq](nwq.md "Next routine")
    
    
           Name: NWSTARS                                                 [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Initialise the stardust field
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-nwstars)
     Variations: See [code variations](../../related/compare/main/subroutine/nwstars.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOOK1](look1.md) calls NWSTARS
    
    
    
    
    
    * * *
    
    
     This routine is called when the space view is initialised in routine LOOK1.
    
    
    
    
    .NWSTARS
    
     LDA QQ11               \ If this is not a space view, jump to WPSHPS to skip
    \ORA MJ                 \ the initialisation of the SX, SY and SZ tables. The OR
     BNE WPSHPS             \ instruction is commented out in the original source,
                            \ but it would have the effect of also skipping the
                            \ initialisation if we had mis-jumped into witchspace
    

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Subroutine [WPSHPS](wpshps.md) (category: Dashboard)

Clear the scanner, reset the ball line and sun line heaps

[SOLAR](solar.md "Previous routine")[nWq](nwq.md "Next routine")
