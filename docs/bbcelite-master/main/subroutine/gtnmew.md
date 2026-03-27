---
title: "The GTNMEW subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/gtnmew.html"
---

[TR1](tr1.md "Previous routine")[MT26](mt26.md "Next routine")
    
    
           Name: GTNMEW                                                  [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Fetch the name of a commander file to save or load
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-gtnmew)
     Variations: See [code variations](../../related/compare/main/subroutine/gtnme-gtnmew.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SVE](sve.md) calls GTNMEW
    
    
    
    
    
    * * *
    
    
     Get the commander's name for loading or saving a commander file. The name is
     stored in the INWK workspace and is terminated by a return character (13).
    
     If ESCAPE is pressed or a blank name is entered, then the name stored is set
     to the name from the last saved commander block.
    
    
    
    * * *
    
    
     Returns:
    
       INWK                 The full filename, including drive and directory, in
                            the form ":0.E.JAMESON", for example, terminated by a
                            return character (13)
    
    
    
    
    .GTNMEW
    
    .GTNME
    
     LDX #4                 \ First we want to copy the drive and directory part of
                            \ the commander file from NA%-5, so set a counter in X
                            \ for 5 bytes, as the string is of the form ":0.E."
    
    .GTL3
    
     LDA NA%-5,X            \ Copy the X-th byte from NA%-5 to INWK
     STA INWK,X
    
     DEX                    \ Decrement the loop counter
    
     BPL GTL3               \ Loop back until the whole drive and directory string
                            \ has been copied to INWK to INWK+4
    
     LDA #7                 \ The call to MT26 below uses the OSWORD block at RLINE
     STA RLINE+2            \ to fetch the line, and RLINE+2 defines the maximum
                            \ line length allowed, so this changes the maximum
                            \ length to 7 (as that's the longest commander name
                            \ allowed)
    
     LDA #8                 \ Print extended token 8 ("{single cap}COMMANDER'S
     JSR DETOK              \ NAME? ")
    
     JSR MT26               \ Call MT26 to fetch a line of text from the keyboard
                            \ to INWK+5, with the text length in Y, so INWK now
                            \ contains the full pathname of the file, as in
                            \ ":0.E.JAMESON", for example
    
     LDA #9                 \ Reset the maximum length in RLINE+2 to the original
     STA RLINE+2            \ value of 9
    
     TYA                    \ The OSWORD call returns the length of the commander's
                            \ name in Y, so transfer this to A
    
     BEQ TR1                \ If A = 0, no name was entered, so jump to TR1 to copy
                            \ the last saved commander's name from NA% to INWK
                            \ and return from the subroutine there
    
     STY thislong           \ Store the length of the length of the commander's that
                            \ was entered in thislong
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Label [GTL3](gtnmew.md#gtl3) is local to this routine

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [MT26](mt26.md) (category: Text)

Fetch a line of text from the keyboard

[X]

Variable [NA%](../variable/na_per_cent.md) (category: Save and load)

The data block for the last saved commander

[X]

Variable [RLINE](../variable/rline.md) (category: Text)

The OSWORD configuration block used to fetch a line of text from the keyboard

[X]

Subroutine [TR1](tr1.md) (category: Save and load)

Copy the last saved commander's name from NA% to INWK

[X]

Variable [thislong](../variable/thislong.md) (category: Save and load)

Contains the length of the most recently entered commander name

[TR1](tr1.md "Previous routine")[MT26](mt26.md "Next routine")
