---
title: "The ZERO subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/zero.html"
---

[OTHERFILEPR](otherfilepr.md "Previous routine")[GTDIR](gtdir.md "Next routine")
    
    
           Name: ZERO                                                    [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Reset the local bubble of universe and ship status
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-zero)
     Variations: See [code variations](../../related/compare/main/subroutine/zero.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [RES2](res2.md) calls ZERO
                 * [RESET](reset.md) calls ZERO
    
    
    
    
    
    * * *
    
    
     This resets the following workspaces to zero:
    
       * UP workspace variables from FRIN to de, which include the ship slots for
         the local bubble of universe, and various flight and ship status variables
    
    
    
    
    .ZERO
    
     LDX #(de-FRIN)         \ We're going to zero the UP workspace variables from
                            \ FRIN to de, so set a counter in X for the correct
                            \ number of bytes
    
     LDA #0                 \ Set A = 0 so we can zero the variables
    
    .ZEL2
    
     STA FRIN,X             \ Zero the X-th byte of FRIN to de
    
     DEX                    \ Decrement the loop counter
    
     BPL ZEL2               \ Loop back to zero the next variable until we have done
                            \ them all
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Label [ZEL2](zero.md#zel2) is local to this routine

[X]

Variable [de](../workspace/wp.md#de) in workspace [WP](../workspace/wp.md)

Equipment destruction flag

[OTHERFILEPR](otherfilepr.md "Previous routine")[GTDIR](gtdir.md "Next routine")
