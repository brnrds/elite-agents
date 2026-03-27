---
title: "The TR1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tr1.html"
---

[TRNME](trnme.md "Previous routine")[GTNMEW](gtnmew.md "Next routine")
    
    
           Name: TR1                                                     [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Copy the last saved commander's name from NA% to INWK
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-tr1)
     Variations: See [code variations](../../related/compare/main/subroutine/tr1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [GTNMEW](gtnmew.md) calls TR1
    
    
    
    
    
    
    .TR1
    
     LDX #7                 \ The commander's name can contain a maximum of 7
                            \ characters, and is terminated by a carriage return,
                            \ so set up a counter in X to copy 8 characters
    
    .GTL2
    
     LDA NA%,X              \ Copy the X-th byte of NA% to the X-th byte of INWK+5
     STA INWK+5,X
    
     DEX                    \ Decrement the loop counter
    
     BPL GTL2               \ Loop back until we have copied all 8 bytes
    
     RTS                    \ Return from the subroutine
    

[X]

Label [GTL2](tr1.md#gtl2) is local to this routine

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [NA%](../variable/na_per_cent.md) (category: Save and load)

The data block for the last saved commander

[TRNME](trnme.md "Previous routine")[GTNMEW](gtnmew.md "Next routine")
