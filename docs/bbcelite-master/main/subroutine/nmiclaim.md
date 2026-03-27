---
title: "The NMICLAIM subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nmiclaim.html"
---

[getzp](getzp.md "Previous routine")[ylookup](../variable/ylookup.md "Next routine")
    
    
           Name: NMICLAIM                                                [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Claim the NMI workspace (&00A0 to &00A7) back from the MOS so the
                 game can use it once again
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-nmiclaim)
     References: This subroutine is called as follows:
                 * [getzp](getzp.md) calls NMICLAIM
                 * [setzp](setzp.md) calls NMICLAIM
    
    
    
    
    
    
    IF _COMPACT
    
    .NMICLAIM
    
     LDA #143               \ Call OSBYTE 143 to issue a paged ROM service call of
     LDX #&C                \ type &C with argument &FF, which is the "NMI claim"
     LDY #&FF               \ service call that asks the current user of the NMI
     JSR OSBYTE             \ space to clear it out
    
     STY NMI                \ Save the returned value of Y in NMI, as it contains
                            \ the filing system ID of the previous claimant of the
                            \ NMI, which we need to restore once we have finished
                            \ using the NMI workspace
    
     RTS                    \ Return from the subroutine
    
    ENDIF
    

[X]

Variable [NMI](../workspace/wp.md#nmi) in workspace [WP](../workspace/wp.md)

Used to store the ID of the current owner of the NMI workspace in the NMICLAIM routine, so we can hand it back to them in the NMIRELEASE routine once we are done using it

[X]

Configuration variable [OSBYTE](../../all/workspaces.md#osbyte) = &FFF4

The address for the OSBYTE routine

[getzp](getzp.md "Previous routine")[ylookup](../variable/ylookup.md "Next routine")
