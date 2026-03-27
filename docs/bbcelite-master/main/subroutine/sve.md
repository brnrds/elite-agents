---
title: "The SVE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sve.html"
---

[DELT](delt.md "Previous routine")[thislong](../variable/thislong.md "Next routine")
    
    
           Name: SVE                                                     [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Display the disk access menu and process saving of commander files
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
                 [The competition code](https://elite.bbcelite.com/deep_dives/the_competition_code.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-sve)
     Variations: See [code variations](../../related/compare/main/subroutine/sve.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 1 of 2)](br1_part_1_of_2.md) calls SVE
                 * [DELT](delt.md) calls SVE
                 * [LOD](lod.md) calls SVE
                 * [NEWBRK](newbrk.md) calls SVE
                 * [TT102](tt102.md) calls SVE
    
    
    
    
    
    
    .SVE
    
     TSX                    \ Transfer the stack pointer to X and store it in
     STX stackpt            \ stackpt, so we can restore it in the NEWBRK routine
    
     JSR TRADEMODE2         \ Set the palette for trading screens and switch the
                            \ current colour to white
    
     LDA #1                 \ Print extended token 1, the disc access menu, which
     JSR DETOK              \ presents these options:
                            \
                            \   1. Load New Commander
                            \   2. Save Commander {commander name}
                            \   3. Catalogue
                            \   4. Delete A File
                            \   5. Default JAMESON
                            \   6. Exit
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     CMP #'1'               \ Option 1 was chosen, so jump to loading to load a new
     BEQ loading            \ commander
    
     CMP #'2'               \ Option 2 was chosen, so jump to SV1 to save the
     BEQ SV1                \ current commander
    
     CMP #'3'               \ Option 3 was chosen, so jump to feb10 to catalogue a
     BEQ feb10              \ disc
    
     CMP #'4'               \ If option 4 wasn't chosen, skip the next two
     BNE jan18              \ instructions
    
     JSR DELT               \ Option 4 was chosen, so call DELT to delete a file
    
     JMP SVE                \ Jump to SVE to display the disc access menu again and
                            \ return from the subroutine using a tail call
    
    .jan18
    
     CMP #'5'               \ If option 5 wasn't chosen, skip to feb13 to exit the
     BNE feb13              \ menu
    
     LDA #224               \ Print extended token 224 ("ARE YOU SURE?")
     JSR DETOK
    
     JSR YESNO              \ Call YESNO to wait until either "Y" or "N" is pressed
    
     BCC feb13              \ If "N" was pressed, jump to feb13
    
     JSR JAMESON            \ Otherwise "Y" was pressed, so call JAMESON to set the
                            \ last saved commander to the default "JAMESON"
                            \ commander
    
     JMP DFAULT             \ Jump to DFAULT to reset the current commander data
                            \ block to the last saved commander, returning from the
                            \ subroutine using a tail call
    
    .feb13
    
     CLC                    \ Option 5 was chosen, so clear the C flag to indicate
                            \ that nothing was loaded
    
     RTS                    \ Return from the subroutine
    
    .feb10
    
     JSR CATS               \ Call CATS to ask for a drive number (or a directory
                            \ name on the Master Compact) and catalogue that disc
                            \ or directory
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     JMP SVE                \ Jump to SVE to display the disc access menu and return
                            \ from the subroutine using a tail call
    
    .loading
    
     JSR GTNMEW             \ If we get here then option 1 (load) was chosen, so
                            \ call GTNMEW to fetch the name of the commander file
                            \ to load (including drive number and directory) into
                            \ INWK
    
    IF _SNG47
    
     JSR GTDRV              \ Get an ASCII disc drive number from the keyboard in A,
                            \ setting the C flag if an invalid drive number was
                            \ entered
    
     BCS jan2186            \ If the C flag is set, then an invalid drive number was
                            \ entered, so return from the subroutine (as DELT-1
                            \ contains an RTS)
    
     STA lodosc+6           \ Store the ASCII drive number in lodosc+6, which is the
                            \ drive character of the load filename string ":1.E."
    
    ENDIF
    
     JSR LOD                \ Call LOD to load the commander file
    
     JSR TRNME              \ Transfer the commander filename from INWK to NA%
    
     SEC                    \ Set the C flag to indicate we loaded a new commander
    
    .jan2186
    
     RTS                    \ Return from the subroutine
    
    .SV1
    
     JSR GTNMEW             \ If we get here then option 2 (save) was chosen, so
                            \ call GTNMEW to fetch the name of the commander file
                            \ to save (including drive number and directory) into
                            \ INWK
    
     JSR TRNME              \ Transfer the commander filename from INWK to NA%
    
     LSR SVC                \ Halve the save count value in SVC
    
     LDA #4                 \ Print extended token 4, which is blank (this was where
     JSR DETOK              \ the competition number was printed in older versions,
                            \ but the competition was long gone by the time of the
                            \ BBC Master version
    
     LDX #NT%               \ We now want to copy the current commander data block
                            \ from location TP to the last saved commander block at
                            \ NA%+8, so set a counter in X to copy the NT% bytes in
                            \ the commander data block
    
    .SVL1
    
     LDA TP,X               \ Copy the X-th byte of TP to the X-th byte of NA%+8
    \STA &0B00,X            \
     STA NA%+8,X            \ The STA is commented out in the original source
    
     DEX                    \ Decrement the loop counter
    
     BPL SVL1               \ Loop back until we have copied all the bytes in the
                            \ commander data block
    
    \JSR CHECK2             \ These instructions are commented out in the original
    \                       \ source
    \STA CHK3
    
     JSR CHECK              \ Call CHECK to calculate the checksum for the last
                            \ saved commander and return it in A
    
     STA CHK                \ Store the checksum in CHK, which is at the end of the
                            \ last saved commander block
    
     PHA                    \ Store the checksum on the stack
    
     ORA #%10000000         \ Set K = checksum with bit 7 set
     STA K
    
     EOR COK                \ Set K+2 = K EOR COK (the competition flags)
     STA K+2
    
     EOR CASH+2             \ Set K+1 = K+2 EOR CASH+2 (the third cash byte)
     STA K+1
    
     EOR #&5A               \ Set K+3 = K+1 EOR &5A EOR TALLY+1 (the high byte of
     EOR TALLY+1            \ the kill tally)
     STA K+3
    
     CLC                    \ Clear the C flag so the call to BPRNT does not include
                            \ a decimal point
    
     JSR TT67               \ Call TT67 twice to print two newlines
     JSR TT67
    
     PLA                    \ Restore the checksum from the stack
    
     EOR #&A9               \ Store the checksum EOR &A9 in CHK2, the penultimate
     STA CHK2               \ byte of the last saved commander block
    
                            \ We now copy the current commander data block into the
                            \ TAP% staging area, though this has no effect as we
                            \ then ignore the result (this code is left over from
                            \ the Apple II version)
    
     LDY #NT%               \ Set a counter in X to copy the NT% bytes in the
                            \ commander data block
    
    .copyme2
    
     LDA NA%+8,Y            \ Copy the X-th byte of NA% to the X-th byte of TAP%
     STA TAP%,Y
    
     DEY                    \ Decrement the loop counter
    
     BPL copyme2            \ Loop back until we have copied all the bytes in the
                            \ commander data block
    
    IF _SNG47
    
     JSR GTDRV              \ Get an ASCII disc drive number from the keyboard in A,
                            \ setting the C flag if an invalid drive number was
                            \ entered
    
     BCS SVEX9              \ If the C flag is set, then an invalid drive number was
                            \ entered, so skip the next two instructions
    
     STA savosc+6           \ Store the ASCII drive number in savosc+6, which is the
                            \ drive character of the save filename string ":1.E."
    
    ENDIF
    
     JSR wfile              \ Call wfile to save the commander file
    
    .SVEX9
    
     JSR DFAULT             \ Call DFAULT to reset the current commander data
                            \ block to the last saved commander
    
    .SVEX
    
     CLC                    \ Clear the C flag to indicate that no new commander
                            \ file was loaded
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [CASH](../workspace/wp.md#cash) in workspace [WP](../workspace/wp.md)

Our current cash pot

[X]

Subroutine [CATS](cats.md) (category: Save and load)

Ask for a disc drive number and print a catalogue of that drive

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

Subroutine [DELT](delt.md) (category: Save and load)

Catalogue a disc, ask for a filename to delete, and delete the file

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [DFAULT](dfault.md) (category: Start and end)

Reset the current commander data block to the last saved commander

[X]

Subroutine [GTDRV](gtdrv.md) (category: Save and load)

Get an ASCII disc drive number from the keyboard

[X]

Subroutine [GTNMEW](gtnmew.md) (category: Save and load)

Fetch the name of a commander file to save or load

[X]

Subroutine [JAMESON](jameson.md) (category: Save and load)

Restore the default JAMESON commander

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [LOD](lod.md) (category: Save and load)

Load a commander file

[X]

Variable [NA%](../variable/na_per_cent.md) (category: Save and load)

The data block for the last saved commander

[X]

Configuration variable [NT%](../workspace/wp.md#nt-per-cent) in workspace [WP](../workspace/wp.md)

This sets the variable NT% to the size of the current commander data block, which starts at TP and ends at SVC+3 (inclusive), i.e. with the last checksum byte

[X]

Label [SV1](sve.md#sv1) is local to this routine

[X]

Variable [SVC](../workspace/wp.md#svc) in workspace [WP](../workspace/wp.md)

The save count

[X]

Subroutine [SVE](sve.md) (category: Save and load)

Display the disk access menu and process saving of commander files

[X]

Label [SVEX9](sve.md#svex9) is local to this routine

[X]

Label [SVL1](sve.md#svl1) is local to this routine

[X]

Variable [TALLY](../workspace/wp.md#tally) in workspace [WP](../workspace/wp.md)

Our combat rank

[X]

Configuration variable [TAP%](../../all/workspaces.md#tap-per-cent) = LS% - 111

The staging area where we copy files after loading and before saving (though this isn't actually used in this version, and is left-over Commodore 64 code)

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[X]

Entry point [TRADEMODE2](trademode.md#trademode2) in subroutine [TRADEMODE](trademode.md) (category: Drawing the screen)

Set the palette for trading screens and switch the current colour to white

[X]

Subroutine [TRNME](trnme.md) (category: Save and load)

Copy the last saved commander's name from INWK to NA%

[X]

Subroutine [TT67](tt67.md) (category: Text)

Print a newline

[X]

Subroutine [YESNO](yesno.md) (category: Keyboard)

Wait until either "Y" or "N" is pressed

[X]

Label [copyme2](sve.md#copyme2) is local to this routine

[X]

Label [feb10](sve.md#feb10) is local to this routine

[X]

Label [feb13](sve.md#feb13) is local to this routine

[X]

Label [jan18](sve.md#jan18) is local to this routine

[X]

Label [jan2186](sve.md#jan2186) is local to this routine

[X]

Label [loading](sve.md#loading) is local to this routine

[X]

Variable [lodosc](../variable/lodosc.md) (category: Save and load)

The OS command string for loading a commander file

[X]

Variable [savosc](../variable/savosc.md) (category: Save and load)

The OS command string for saving a commander file

[X]

Variable [stackpt](../variable/stackpt.md) (category: Save and load)

Temporary storage for the stack pointer when jumping to the break handler at NEWBRK

[X]

Entry point [t](tt217.md#t) in subroutine [TT217](tt217.md) (category: Keyboard)

As TT217 but don't preserve Y, set it to YSAV instead

[X]

Subroutine [wfile](wfile.md) (category: Save and load)

Save the commander file

[DELT](delt.md "Previous routine")[thislong](../variable/thislong.md "Next routine")
