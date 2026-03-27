---
title: "The OSB (Loader) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/subroutine/osb.html"
---

[ROOT (Loader)](root.md "Previous routine")[MESS1 (Loader)](../variable/mess1.md "Next routine")
    
    
           Name: OSB                                                     [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: A convenience routine for calling OSBYTE with Y = 0
    
    
        Context: See this subroutine [in context in the source code](../../all/loader.md#header-osb)
     References: This subroutine is called as follows:
                 * [Elite loader](elite_loader.md) calls OSB
    
    
    
    
    
    
    .OSB
    
     LDY #0                 \ Call OSBYTE with Y = 0, returning from the subroutine
     JMP OSBYTE             \ using a tail call (so we can call OSB to call OSBYTE
                            \ for when we know we want Y set to 0)
    

[X]

Configuration variable [OSBYTE](../../all/loader.md#osbyte) = &FFF4

The address for the OSBYTE routine

[ROOT (Loader)](root.md "Previous routine")[MESS1 (Loader)](../variable/mess1.md "Next routine")
