---
title: "The DELT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/delt.html"
---

[CATS](cats.md "Previous routine")[SVE](sve.md "Next routine")
    
    
           Name: DELT                                                    [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Catalogue a disc, ask for a filename to delete, and delete the
                 file
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-delt)
     Variations: See [code variations](../../related/compare/main/subroutine/delt.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SVE](sve.md) calls DELT
                 * [CATS](cats.md) calls via DELT-1
    
    
    
    
    
    * * *
    
    
     This routine asks for a disc drive number, and if it is a valid number (0-3)
     it displays a catalogue of the disc in that drive. It then asks for a filename
     to delete, updates the OS command at DELI so that when that command is run, it
     deletes the correct file, and then it does the deletion.
    
    
    
    * * *
    
    
     Other entry points:
    
       DELT-1               Contains an RTS
    
    
    
    
    .DELT
    
     JSR CATS               \ Call CATS to ask for a drive number (or a directory
                            \ name on the Master Compact) and catalogue that disc
                            \ or directory
    
     BCS SVE                \ If the C flag is set then an invalid drive number was
                            \ entered as part of the catalogue process, so jump to
                            \ SVE to display the disc access menu
    
    IF _SNG47
    
     LDA CTLI+4             \ The call to CATS above put the drive number into
     STA DELI+8             \ CTLI+4, so copy the drive number into DELI+8 so that
                            \ the drive number in the "DELETE :1.1234567" string
                            \ gets updated (i.e. the number after the colon)
    
    ENDIF
    
     LDA #8                 \ Print extended token 8 ("{single cap}COMMANDER'S
     JSR DETOK              \ NAME? ")
    
     JSR MT26               \ Call MT26 to fetch a line of text from the keyboard
                            \ to INWK+5, with the text length in Y
    
     TYA                    \ If no text was entered (Y = 0) then jump to SVE to
     BEQ SVE                \ display the disc access menu
    
    IF _SNG47
    
                            \ We now copy the entered filename from INWK to DELI, so
                            \ that it overwrites the filename part of the string,
                            \ i.e. the "E.1234567" part of "DELETE :1.1234567"
    
     LDX #9                 \ Set up a counter in X to count from 9 to 1, so that we
                            \ copy the string starting at INWK+4+1 (i.e. INWK+5) to
                            \ DELI+9+1 (i.e. DELI+10 onwards, or "1.1234567")
                            \
                            \ Note that this is a bug - X should be set to 8, as a
                            \ value of 9 overwrites the first character of the
                            \ "SAVE" command in savosc
                            \
                            \ This means that if you delete a file, it breaks the
                            \ save command, so you can't save a commander file if
                            \ you have previously deleted a file
    
    ELIF _COMPACT
    
                            \ We now copy the entered filename from INWK to DELI, so
                            \ that it overwrites the filename part of the string,
                            \ i.e. the "1234567890" part of "DELETE 1234567890"
    
     LDX #8                 \ Set up a counter in X to count from 8 to 0, so that we
                            \ copy the string starting at INWK+5+0 (i.e. INWK+5) to
                            \ DELI+7+0 (i.e. DELI+7 onwards, or "1234567890")
    
    ENDIF
    
    .DELL1
    
    IF _SNG47
    
     LDA INWK+4,X           \ Copy the X-th byte of INWK+4 to the X-th byte of
     STA DELI+9,X           \ DELI+9
    
     DEX                    \ Decrement the loop counter
    
     BNE DELL1              \ Loop back to DELL1 to copy the next character until we
                            \ have copied the whole filename
    
     JSR getzp              \ Call getzp to store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ELIF _COMPACT
    
     LDA INWK+5,X           \ Copy the X-th byte of INWK+5 to the X-th byte of
     STA DELI+7,X           \ DELI+7
    
     DEX                    \ Decrement the loop counter
    
     BPL DELL1              \ Loop back to DELL1 to copy the next character until we
                            \ have copied the whole filename
    
     JSR NMIRELEASE         \ Release the NMI workspace (&00A0 to &00A7) so the MOS
                            \ can use it, and store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ENDIF
    
     LDX #LO(DELI)          \ Set (Y X) to point to the OS command at DELI, which
     LDY #HI(DELI)          \ contains the DFS command for deleting this file
    
     JSR OSCLI              \ Call OSCLI to execute the OS command at (Y X), which
                            \ deletes the file
    
     JSR getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000
    
     JMP SVE                \ Jump to SVE to display the disc access menu and return
                            \ from the subroutine using a tail call
    

[X]

Subroutine [CATS](cats.md) (category: Save and load)

Ask for a disc drive number and print a catalogue of that drive

[X]

Variable [CTLI](../variable/ctli.md) (category: Save and load)

The OS command string for cataloguing a disc

[X]

Variable [DELI](../variable/deli.md) (category: Save and load)

The OS command string for deleting a file

[X]

Label [DELL1](delt.md#dell1) is local to this routine

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [MT26](mt26.md) (category: Text)

Fetch a line of text from the keyboard

[X]

Subroutine [NMIRELEASE](nmirelease.md) (category: Utility routines)

Release the NMI workspace (&00A0 to &00A7) so the MOS can use it and store the top part of zero page in the buffer at &3000

[X]

Configuration variable [OSCLI](../../all/workspaces.md#oscli) = &FFF7

The address for the OSCLI routine

[X]

Subroutine [SVE](sve.md) (category: Save and load)

Display the disk access menu and process saving of commander files

[X]

Subroutine [getzp](getzp.md) (category: Utility routines)

Swap zero page (&0090 to &00EF) with the buffer at &3000

[CATS](cats.md "Previous routine")[SVE](sve.md "Next routine")
