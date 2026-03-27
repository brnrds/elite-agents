---
title: "The TRNME subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/trnme.html"
---

[JAMESON](jameson.md "Previous routine")[TR1](tr1.md "Next routine")
    
    
           Name: TRNME                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Copy the last saved commander's name from INWK to NA%
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-trnme)
     Variations: See [code variations](../../related/compare/main/subroutine/trnme.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SVE](sve.md) calls TRNME
    
    
    
    
    
    
    .TRNME
    
     LDX #7                 \ The commander's name can contain a maximum of 7
                            \ characters, and is terminated by a carriage return,
                            \ so set up a counter in X to copy 8 characters
    
     LDA thislong           \ Copy the length of the commander's name from thislong
     STA oldlong            \ to oldlong (though this is never used, so this
                            \ doesn't have any effect)
    
    .GTL1
    
     LDA INWK+5,X           \ Copy the X-th byte of INWK+5 to the X-th byte of NA%
     STA NA%,X
    
     DEX                    \ Decrement the loop counter
    
     BPL GTL1               \ Loop back until we have copied all 8 bytes
    
                            \ Fall through into TR1 to copy the name back from NA%
                            \ to INWK. This isn't necessary as the name is already
                            \ there, but it does save one byte, as we don't need an
                            \ RTS here
    

[X]

Label [GTL1](trnme.md#gtl1) is local to this routine

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [NA%](../variable/na_per_cent.md) (category: Save and load)

The data block for the last saved commander

[X]

Variable [oldlong](../variable/oldlong.md) (category: Save and load)

Contains the length of the last saved commander name

[X]

Variable [thislong](../variable/thislong.md) (category: Save and load)

Contains the length of the most recently entered commander name

[JAMESON](jameson.md "Previous routine")[TR1](tr1.md "Next routine")
