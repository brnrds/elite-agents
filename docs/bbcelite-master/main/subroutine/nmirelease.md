---
title: "The NMIRELEASE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nmirelease.html"
---

[setzp](setzp.md "Previous routine")[getzp](getzp.md "Next routine")
    
    
           Name: NMIRELEASE                                              [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Release the NMI workspace (&00A0 to &00A7) so the MOS can use it
                 and store the top part of zero page in the buffer at &3000
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-nmirelease)
     References: This subroutine is called as follows:
                 * [CATS](cats.md) calls NMIRELEASE
                 * [DELT](delt.md) calls NMIRELEASE
                 * [GTDIR](gtdir.md) calls NMIRELEASE
                 * [rfile](rfile.md) calls NMIRELEASE
                 * [wfile](wfile.md) calls NMIRELEASE
    
    
    
    
    
    
    IF _COMPACT
    
    .NMIRELEASE
    
     JSR getzp+3            \ Call getzp+3 to restore the top part of zero page
                            \ from the buffer at &3000, but without first claiming
                            \ the NMI workspace (as it's already been claimed by
                            \ this point)
    
     LDA #143               \ Call OSBYTE 143 to issue a paged ROM service call of
     LDX #&B                \ type &B with Y set to the previous NMI owner's ID.
     LDY NMI                \ This releases the NMI workspace back to the original
     JMP OSBYTE             \ owner, from whom we claimed the workspace in the
                            \ NMICLAIM routine, and returns from the subroutine
                            \ using a tail call
    
    ENDIF
    

[X]

Variable [NMI](../workspace/wp.md#nmi) in workspace [WP](../workspace/wp.md)

Used to store the ID of the current owner of the NMI workspace in the NMICLAIM routine, so we can hand it back to them in the NMIRELEASE routine once we are done using it

[X]

Configuration variable [OSBYTE](../../all/workspaces.md#osbyte) = &FFF4

The address for the OSBYTE routine

[X]

Entry point [getzp+3](getzp.md#getzp) in subroutine [getzp](getzp.md) (category: Utility routines)

Restore the top part of zero page, but without first claiming the NMI workspace (Master Compact variant only)

[setzp](setzp.md "Previous routine")[getzp](getzp.md "Next routine")
