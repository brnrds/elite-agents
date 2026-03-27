---
title: "The GTDIR subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/gtdir.html"
---

[ZERO](zero.md "Previous routine")[CATS](cats.md "Next routine")
    
    
           Name: GTDIR                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Fetch the name of an ADFS directory on the Master Compact and
                 change to that directory
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-gtdir)
     References: This subroutine is called as follows:
                 * [CATS](cats.md) calls GTDIR
    
    
    
    
    
    
    IF _COMPACT
    
    .GTDIR
    
     LDA #2                 \ Print extended token 2 ("{cr}WHICH DRIVE?")
     JSR DETOK
    
     LDA #19                \ The call to MT26 below uses the OSWORD block at RLINE
     STA RLINE+2            \ to fetch the line, and RLINE+2 defines the maximum
                            \ line length allowed, so this changes the maximum
                            \ length to 19 (as that's the longest directory name
                            \ allowed)
    
     JSR MT26               \ Call MT26 to fetch a line of text from the keyboard
                            \ to INWK+5, with the text length in Y, so INWK now
                            \ contains the full directory name
    
     LDA #9                 \ Reset the maximum length in RLINE+2 to the original
     STA RLINE+2            \ value of 9
    
     TYA                    \ The OSWORD call returns the length of the commander's
                            \ name in Y, so transfer this to A
    
     BEQ GTDIR-1            \ If A = 0, no name was entered, so jump to DIR-1 to
                            \ return from the subroutine (as GTDIR-1 contains an
                            \ RTS)
    
                            \ We now copy the entered filename from INWK to DIRI, so
                            \ that it overwrites the filename part of the string,
                            \ i.e. the "12345678901234567890" part of
                            \ "DIR 12345678901234567890"
    
     LDX #18                \ Set up a counter in X to count from 18 to 0, so that
                            \ we copy the string starting at INWK+5 to DIRI+4
                            \ onwards (i.e. "12345678901234567890")
    
    .DIRL
    
     LDA INWK+5,X           \ Copy the X-th byte of INWK+5 to the X-th byte of
     STA DIRI+4,X           \ DIRI+4
    
     DEX                    \ Decrement the loop counter
    
     BPL DIRL               \ Loop back to DIRL to copy the next character until we
                            \ have copied the whole filename
    
     JSR NMIRELEASE         \ Release the NMI workspace (&00A0 to &00A7) so the MOS
                            \ can use it, and store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
     LDX #LO(DIRI)          \ Set (Y X) to point to DIRI ("DIR <name entered>")
     LDY #HI(DIRI)
    
     JSR OSCLI              \ Call OSCLI to run the OS command in DIRI, which
                            \ changes the disc directory to the name entered
    
     JMP getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000 and return from the subroutine
                            \ using a tail call
    
    ENDIF
    

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Variable [DIRI](../variable/diri.md) (category: Save and load)

The OS command string for changing directory on the Master Compact

[X]

Label [DIRL](gtdir.md#dirl) is local to this routine

[X]

Subroutine [GTDIR](gtdir.md) (category: Save and load)

Fetch the name of an ADFS directory on the Master Compact and change to that directory

[X]

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

Variable [RLINE](../variable/rline.md) (category: Text)

The OSWORD configuration block used to fetch a line of text from the keyboard

[X]

Subroutine [getzp](getzp.md) (category: Utility routines)

Swap zero page (&0090 to &00EF) with the buffer at &3000

[ZERO](zero.md "Previous routine")[CATS](cats.md "Next routine")
