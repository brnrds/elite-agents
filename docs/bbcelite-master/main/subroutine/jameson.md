---
title: "The JAMESON subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/jameson.html"
---

[CHECK2](check2.md "Previous routine")[TRNME](trnme.md "Next routine")
    
    
           Name: JAMESON                                                 [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Restore the default JAMESON commander
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-jameson)
     References: This subroutine is called as follows:
                 * [BEGIN](begin.md) calls JAMESON
                 * [SVE](sve.md) calls JAMESON
    
    
    
    
    
    
    .JAMESON
    
     LDY #(NAEND%-NA2%)     \ We are going to copy the default commander at NA2%
                            \ over the top of the last saved commander at NA%, so
                            \ set a counter to copy all the bytes between NA2% and
                            \ NAEND%
    
    .JAMEL1
    
     LDA NA2%,Y             \ Copy the Y-th byte of NA2% to the Y-th byte of NA%
     STA NA%,Y
    
     DEY                    \ Decrement the loop counter
    
     BPL JAMEL1             \ Loop back until we have copied the whole commander
    
     LDY #7                 \ Set oldlong to 7, the length of the commander name
     STY oldlong            \ "JAMESON"
    
     RTS                    \ Return from the subroutine
    

[X]

Label [JAMEL1](jameson.md#jamel1) is local to this routine

[X]

Variable [NA%](../variable/na_per_cent.md) (category: Save and load)

The data block for the last saved commander

[X]

Variable [NA2%](../variable/na2_per_cent.md) (category: Save and load)

The data block for the default commander

[X]

Variable [NAEND%](../variable/na2_per_cent.md#naend-per-cent) in variable [NA2%](../variable/na2_per_cent.md)

These bytes appear to be unused

[X]

Variable [oldlong](../variable/oldlong.md) (category: Save and load)

Contains the length of the last saved commander name

[CHECK2](check2.md "Previous routine")[TRNME](trnme.md "Next routine")
