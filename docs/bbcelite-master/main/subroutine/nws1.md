---
title: "The NwS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nws1.html"
---

[NWSHP](nwshp.md "Previous routine")[ABORT](abort.md "Next routine")
    
    
           Name: NwS1                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Flip the sign and double an INWK byte
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-nws1)
     References: This subroutine is called as follows:
                 * [NWSPS](nwsps.md) calls NwS1
    
    
    
    
    
    * * *
    
    
     Flip the sign of the INWK byte at offset X, and increment X by 2. This is
     used by the space station creation routine at NWSPS.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The offset of the INWK byte to be flipped
    
    
    
    * * *
    
    
     Returns:
    
       X                    X is incremented by 2
    
    
    
    
    .NwS1
    
     LDA INWK,X             \ Load the X-th byte of INWK into A and flip bit 7,
     EOR #%10000000         \ storing the result back in the X-th byte of INWK
     STA INWK,X
    
     INX                    \ Add 2 to X
     INX
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[NWSHP](nwshp.md "Previous routine")[ABORT](abort.md "Next routine")
