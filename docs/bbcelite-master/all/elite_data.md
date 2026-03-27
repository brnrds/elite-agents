---
title: "Game data in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_data.html"
---

[Elite H source](elite_h.md "Previous routine")[Ship blueprints](elite_ships.md "Next routine")
    
    
     BBC MASTER ELITE GAME DATA SOURCE
    
     BBC Master Elite was written by Ian Bell and David Braben and is copyright
     Acornsoft 1986
    
     The code in this file has been reconstructed from a disassembly of the version
     released on Ian Bell's personal website at <http://www.elitehomepage.org/>
    
     The commentary is copyright Mark Moxon, and any misunderstandings or mistakes
     in the documentation are entirely my fault
    
     The terminology and notations used in this commentary are explained at
     <https://elite.bbcelite.com/terminology>
    
     The deep dive articles referred to in this commentary can be found at
     <https://elite.bbcelite.com/deep_dives>
    
    
    
    * * *
    
    
     This source file contains the game data for BBC Master Elite, including the
     ship blueprints and game text.
    
    
    
    * * *
    
    
     This source file produces the following binary file:
    
       * BDATA.bin
    
     after reading in the following file:
    
       * P.DIALS2P.bin
    
    
    
    
     INCLUDE "1-source-files/main-sources/elite-build-options.asm"
    
     CPU 1                  \ Switch to 65SC12 assembly, as this code runs on a
                            \ BBC Master
    
     _SNG47                 = (_VARIANT = 1)
     _COMPACT               = (_VARIANT = 2)
    
     GUARD &C000            \ Guard against assembling over MOS memory
    
    
    
     Configuration variables
    
    
    
    
     CODE% = &7000          \ The address where the code will be run
    
     LOAD% = &1300          \ The address where the code will be loaded
    
     RE = &23               \ The obfuscation byte used to hide the recursive tokens
                            \ table from crackers viewing the binary code
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHAR](../game_data/macro/char.md)
                            \   * [CONT](../game_data/macro/cont.md)
                            \   * [RTOK](../game_data/macro/rtok.md)
                            \   * [TWOK](../game_data/macro/twok.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     VE = &57               \ The obfuscation byte used to hide the extended tokens
                            \ table from crackers viewing the binary code
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ECHR](../game_data/macro/echr.md)
                            \   * [EJMP](../game_data/macro/ejmp.md)
                            \   * [ERND](../game_data/macro/ernd.md)
                            \   * [ETOK](../game_data/macro/etok.md)
                            \   * [ETWO](../game_data/macro/etwo.md)
                            \   * [RUTOK](../game_data/variable/rutok.md)
                            \   * [TKN1](../game_data/variable/tkn1.md)
                            \   * [TOKN](../game_data/macro/tokn.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    
    
     ELITE GAME DATA SOURCE
    
    
    
    
     ORG CODE%              \ Set the assembly address to CODE%
    
    
    
    
           Name: Dashboard image                                         [Show more]
           Type: Variable
       Category: Loader
        Summary: The binary for the dashboard image
    
    
        Context: See this variable [on its own page](../game_data/variable/dashboard_image.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     The data file contains the dashboard binary, which gets moved into screen
     memory by the loader:
    
       * P.DIALS2P.bin contains the dashboard, which gets moved to screen address
         &7000, which is the starting point of the eight-colour mode 2 portion at
         the bottom of the split screen
    
    
    
    
    .DIALS
    
     INCBIN "1-source-files/images/P.DIALS2P.bin"
    
     SKIP 256               \ These bytes appear to be unused, but they get moved to
                            \ &7E00-&7EFF along with the dashboard
    
    
    

[Elite H source](elite_h.md "Previous routine")[Ship blueprints](elite_ships.md "Next routine")
