---
title: "The DFAULT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dfault.html"
---

[BAY](bay.md "Previous routine")[TITLE](title.md "Next routine")
    
    
           Name: DFAULT                                                  [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Reset the current commander data block to the last saved commander
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-dfault)
     Variations: See [code variations](../../related/compare/main/subroutine/dfault-qu5.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 1 of 2)](br1_part_1_of_2.md) calls DFAULT
                 * [SVE](sve.md) calls DFAULT
    
    
    
    
    
    
    .DFAULT
    
     LDX #NT%+8             \ The size of the last saved commander data block is NT%
                            \ bytes, and it is preceded by the 8 bytes of the
                            \ commander name (seven characters plus a carriage
                            \ return). The commander data block at NAME is followed
                            \ by the commander data block, so we need to copy the
                            \ name and data from the "last saved" buffer at NA% to
                            \ the current commander workspace at NAME. So we set up
                            \ a counter in X for the NT% + 8 bytes that we want to
                            \ copy
    
    .QUL1
    
     LDA NA%-1,X            \ Copy the X-th byte of NA%-1 to the X-th byte of
     STA NAME-1,X           \ NAME-1 (the -1 is because X is counting down from
                            \ NT% + 8 to 1)
    
     DEX                    \ Decrement the loop counter
    
     BNE QUL1               \ Loop back for the next byte of the commander data
                            \ block
    
     STX QQ11               \ X is 0 by the end of the above loop, so this sets QQ11
                            \ to 0, which means we will be showing a view without a
                            \ boxed title at the top (i.e. we're going to use the
                            \ screen layout of a space view in the following)
    
                            \ If the commander check below fails, we keep jumping
                            \ back to here to crash the game with an infinite loop
    
    .doitagain
    
     JSR CHECK              \ Call the CHECK subroutine to calculate the checksum
                            \ for the current commander block at NA%+8 and put it
                            \ in A
    
     CMP CHK                \ Test the calculated checksum against CHK
    
    IF _REMOVE_CHECKSUMS
    
     NOP                    \ If we have disabled checksums, then ignore the result
     NOP                    \ of the comparison and fall through into the next part
    
    ELSE
    
     BNE doitagain          \ If the calculated checksum does not match CHK, then
                            \ loop back to repeat the check - in other words, we
                            \ enter an infinite loop here, as the checksum routine
                            \ will keep returning the same incorrect value
    
    ENDIF
    
                            \ The checksum CHK is correct, so now we check whether
                            \ CHK2 = CHK EOR A9, and if this check fails, bit 7 of
                            \ the competition flags at COK gets set, to indicate
                            \ to Acornsoft via the competition code that there has
                            \ been some hacking going on with this competition entry
    
     EOR #&A9               \ X = checksum EOR &A9
     TAX
    
     LDA COK                \ Set A to the competition flags in COK
    
     CPX CHK2               \ If X = CHK2, then skip the next instruction
     BEQ tZ
    
     ORA #%10000000         \ Set bit 7 of A to indicate this commander file has
                            \ been tampered with
    
    .tZ
    
     ORA #%00001000         \ Set bit 3 of A to denote that this is the Master
                            \ version (which is the same flag as the Apple II
                            \ version)
    
     STA COK                \ Store the updated competition flags in COK
    
    \JSR CHECK2             \ These instructions are commented out in the original
    \                       \ source
    \CMP CHK3
    \BNE doitagain
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [CHECK](check.md) (category: Save and load)

Calculate the checksum for the last saved commander data block

[X]

Variable [CHK](../variable/chk.md) (category: Save and load)

First checksum byte for the saved commander data file

[X]

Variable [CHK2](../variable/chk2.md) (category: Save and load)

Second checksum byte for the saved commander data file

[X]

Variable [COK](../workspace/wp.md#cok) in workspace [WP](../workspace/wp.md)

Flags used to generate the competition code

[X]

Variable [NA%](../variable/na_per_cent.md) (category: Save and load)

The data block for the last saved commander

[X]

Variable [NAME](../workspace/wp.md#name) in workspace [WP](../workspace/wp.md)

The current commander name

[X]

Configuration variable [NT%](../workspace/wp.md#nt-per-cent) in workspace [WP](../workspace/wp.md)

This sets the variable NT% to the size of the current commander data block, which starts at TP and ends at SVC+3 (inclusive), i.e. with the last checksum byte

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Label [QUL1](dfault.md#qul1) is local to this routine

[X]

Label [doitagain](dfault.md#doitagain) is local to this routine

[X]

Label [tZ](dfault.md#tz) is local to this routine

[BAY](bay.md "Previous routine")[TITLE](title.md "Next routine")
