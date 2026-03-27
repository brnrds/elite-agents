---
title: "The S% subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/s_per_cent.html"
---

[TGINT](../variable/tgint.md "Previous routine")[DEEOR](deeor.md "Next routine")
    
    
           Name: S%                                                      [Show more]
           Type: Subroutine
       Category: Loader
        Summary: Move code, set up break handler and start the game
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-s-per-cent)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
     RTS                    \ This byte appears to be unused, but it might be a
                            \ hangover from the cassette version, where this byte is
                            \ used for a checksum
    
    .S%
    
     CLD                    \ Clear the D flag to make sure we are in binary mode
    
     JSR DEEOR              \ Call DEEOR to unscramble the main code
    
     JSR COLD               \ Call COLD to set up the break handler
    
    \JSR Checksum           \ This instruction is commented out in the original
                            \ source
    
     JMP BEGIN              \ Jump to BEGIN to start the game
    

[X]

Subroutine [BEGIN](begin.md) (category: Loader)

Initialise the configuration variables and start the game

[X]

Subroutine [COLD](cold.md) (category: Save and load)

Set the standard BRKV handler for the game

[X]

Subroutine [DEEOR](deeor.md) (category: Utility routines)

Unscramble the main code

[TGINT](../variable/tgint.md "Previous routine")[DEEOR](deeor.md "Next routine")
