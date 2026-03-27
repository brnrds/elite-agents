---
title: "The LOD subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/lod.html"
---

[GTDRV](gtdrv.md "Previous routine")[backtonormal](backtonormal.md "Next routine")
    
    
           Name: LOD                                                     [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Load a commander file
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-lod)
     Variations: See [code variations](../../related/compare/main/subroutine/lod.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SVE](sve.md) calls LOD
                 * [GTDRV](gtdrv.md) calls via LOR
    
    
    
    
    
    * * *
    
    
     The filename should be stored at INWK, terminated with a carriage return (13).
    
    
    
    * * *
    
    
     Other entry points:
    
       LOR                  Set the C flag and return from the subroutine
    
    
    
    
    .LOD
    
     JSR rfile              \ Call rfile to load the commander file to the TAP%
                            \ staging area
    
     LDA TAP%               \ If the first byte of the loaded file has bit 7 set,
     BMI ELT2F              \ jump to ELT2F, as this is an invalid commander file
                            \
                            \ ELT2F contains a BRK instruction, which will force an
                            \ interrupt to call the address in BRKV, which will
                            \ print out the system error at ELT2F
    
     LDY #NT%               \ We have successfully loaded the commander file to the
                            \ TAP% staging area, so now we want to copy it to the
                            \ last saved commander data block at NA%+8, so we set up
                            \ a counter in Y to copy NT% bytes
    
    .copyme
    
     LDA TAP%,Y             \ Copy the Y-th byte of TAP% to the Y-th byte of NA%+8
     STA NA%+8,Y
    
     DEY                    \ Decrement the loop counter
    
     BPL copyme             \ Loop back until we have copied all NT% bytes
    
    .LOR
    
     SEC                    \ Set the C flag
    
     RTS                    \ Return from the subroutine
    
    .ELT2F
    
     LDA #9                 \ Print extended token 9 ("{cr}{all caps}ILLEGAL ELITE
     JSR DETOK              \ II FILE{sentence case}")
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     JMP SVE                \ Jump to SVE to display the disc access menu and return
                            \ from the subroutine using a tail call
    

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Label [ELT2F](lod.md#elt2f) is local to this routine

[X]

Variable [NA%](../variable/na_per_cent.md) (category: Save and load)

The data block for the last saved commander

[X]

Configuration variable [NT%](../workspace/wp.md#nt-per-cent) in workspace [WP](../workspace/wp.md)

This sets the variable NT% to the size of the current commander data block, which starts at TP and ends at SVC+3 (inclusive), i.e. with the last checksum byte

[X]

Subroutine [SVE](sve.md) (category: Save and load)

Display the disk access menu and process saving of commander files

[X]

Configuration variable [TAP%](../../all/workspaces.md#tap-per-cent) = LS% - 111

The staging area where we copy files after loading and before saving (though this isn't actually used in this version, and is left-over Commodore 64 code)

[X]

Label [copyme](lod.md#copyme) is local to this routine

[X]

Subroutine [rfile](rfile.md) (category: Save and load)

Load the commander file

[X]

Entry point [t](tt217.md#t) in subroutine [TT217](tt217.md) (category: Keyboard)

As TT217 but don't preserve Y, set it to YSAV instead

[GTDRV](gtdrv.md "Previous routine")[backtonormal](backtonormal.md "Next routine")
