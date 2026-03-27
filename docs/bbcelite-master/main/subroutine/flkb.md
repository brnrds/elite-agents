---
title: "The FLKB subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/flkb.html"
---

[UNIV](../variable/univ.md "Previous routine")[NLIN3](nlin3.md "Next routine")
    
    
           Name: FLKB                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Flush the keyboard buffer
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-flkb)
     Variations: See [code variations](../../related/compare/main/subroutine/flkb.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MT26](mt26.md) calls FLKB
                 * [TRADEMODE](trademode.md) calls FLKB
    
    
    
    
    
    * * *
    
    
     This routine does nothing in the BBC Master version of Elite. It does have a
     function in the disc and 6502SP versions, so the authors presumably just
     cleared out the FLKB routine for the Master version, rather than unplumbing it
     from the code.
    
    
    
    
    .FLKB
    
     RTS                    \ Return from the subroutine
    

[UNIV](../variable/univ.md "Previous routine")[NLIN3](nlin3.md "Next routine")
