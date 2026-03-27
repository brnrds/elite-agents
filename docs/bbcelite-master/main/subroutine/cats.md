---
title: "The CATS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/cats.html"
---

[GTDIR](gtdir.md "Previous routine")[DELT](delt.md "Next routine")
    
    
           Name: CATS                                                    [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Ask for a disc drive number and print a catalogue of that drive
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-cats)
     Variations: See [code variations](../../related/compare/main/subroutine/cats.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DELT](delt.md) calls CATS
                 * [SVE](sve.md) calls CATS
    
    
    
    
    
    * * *
    
    
     This routine asks for a disc drive number, and if it is a valid number (0-3)
     it displays a catalogue of the disc in that drive. It also updates the OS
     command at CTLI so that when that command is run, it catalogues the correct
     drive.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Clear if a valid drive number was entered (0-3), set
                            otherwise
    
    
    
    
    .CATS
    
    IF _SNG47
    
     JSR GTDRV              \ Get an ASCII disc drive number from the keyboard in A,
                            \ setting the C flag if an invalid drive number was
                            \ entered
    
     BCS DELT-1             \ If the C flag is set, then an invalid drive number was
                            \ entered, so return from the subroutine (as DELT-1
                            \ contains an RTS)
    
     STA CTLI+4             \ Store the drive number in the fifth byte of the
                            \ command string at CTLI, so it overwrites the "1" in
                            \ "CAT 1" with the drive number to catalogue
    
     STA DTW7               \ Store the drive number in DTW7, so printing extended
                            \ token 4 will show the correct drive number (as token 4
                            \ contains the {drive number} jump code, which calls
                            \ MT16 to print the character in DTW7)
    
    ELIF _COMPACT
    
     JSR GTDIR              \ Get a directory name from the keyboard and change to
                            \ that directory
    
    ENDIF
    
     LDA #3                 \ Print extended token 3, which clears the screen and
     JSR DETOK              \ prints the boxed-out title "DRIVE {drive number}
                            \ CATALOGUE"
    
     LDA #1                 \ Set the CATF flag to 1, so that the TT26 routine will
     STA CATF               \ print out the disc catalogue correctly
    
     STA XC                 \ Move the text cursor to column 1
    
    IF _SNG47
    
     JSR getzp              \ Call getzp to store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ELIF _COMPACT
    
     JSR NMIRELEASE         \ Release the NMI workspace (&00A0 to &00A7) so the MOS
                            \ can use it, and store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ENDIF
    
     LDX #LO(CTLI)          \ Set (Y X) to point to the OS command at CTLI, which
     LDY #HI(CTLI)          \ contains a dot and the drive number, which is the
                            \ DFS command for cataloguing that drive (*. being short
                            \ for *CAT)
    
     JSR OSCLI              \ Call OSCLI to execute the OS command at (Y X), which
                            \ catalogues the disc
    
     JSR getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000
    
     STZ CATF               \ Set the CATF flag to 0, so the TT26 routine reverts to
                            \ standard formatting
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [CATF](../workspace/up.md#catf) in workspace [UP](../workspace/up.md)

The disc catalogue flag

[X]

Variable [CTLI](../variable/ctli.md) (category: Save and load)

The OS command string for cataloguing a disc

[X]

Entry point [DELT-1](delt.md#delt) in subroutine [DELT](delt.md) (category: Save and load)

Contains an RTS

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Configuration variable [DTW7](mt16.md#dtw7) in subroutine [MT16](mt16.md)

Point DTW7 to the second byte of the instruction above so that modifying DTW7 changes the value loaded into A

[X]

Subroutine [GTDIR](gtdir.md) (category: Save and load)

Fetch the name of an ADFS directory on the Master Compact and change to that directory

[X]

Subroutine [GTDRV](gtdrv.md) (category: Save and load)

Get an ASCII disc drive number from the keyboard

[X]

Subroutine [NMIRELEASE](nmirelease.md) (category: Utility routines)

Release the NMI workspace (&00A0 to &00A7) so the MOS can use it and store the top part of zero page in the buffer at &3000

[X]

Configuration variable [OSCLI](../../all/workspaces.md#oscli) = &FFF7

The address for the OSCLI routine

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Subroutine [getzp](getzp.md) (category: Utility routines)

Swap zero page (&0090 to &00EF) with the buffer at &3000

[GTDIR](gtdir.md "Previous routine")[DELT](delt.md "Next routine")
