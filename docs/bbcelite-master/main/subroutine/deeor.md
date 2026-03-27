---
title: "The DEEOR subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/deeor.html"
---

[S%](s_per_cent.md "Previous routine")[DEEORS](deeors.md "Next routine")
    
    
           Name: DEEOR                                                   [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Unscramble the main code
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-deeor)
     References: This subroutine is called as follows:
                 * [S%](s_per_cent.md) calls DEEOR
    
    
    
    
    
    * * *
    
    
     The main game code and data are encrypted. This routine decrypts the game code
     in two parts: the main game code between G% and F%, and the game data between
     XX21 and the end of the game data at &B1FF.
    
     In the BeebAsm version, the encryption is done by elite-checksum.py, but in
     the original this would have been done by the BBC BASIC build scripts.
    
    
    
    
    .DEEOR
    
     LDA #LO(G%-1)          \ Set FRIN(1 0) = G%-1 as the low address of the
     STA FRIN               \ decryption block, so we decrypt from the start of the
     LDA #HI(G%-1)          \ DOENTRY routine
     STA FRIN+1
    
     LDA #HI(F%-1)          \ Set (A Y) to F% as the high address of the decryption
     LDY #LO(F%-1)          \ block, so we decrypt to the end of the main game code
                            \ at F%
    
     LDX #KEY1              \ Set X = KEY1 as the decryption seed (the value used to
                            \ encrypt the code, which is done in elite-checksum.py)
    
    IF _REMOVE_CHECKSUMS
    
     NOP                    \ If we have disabled checksums, skip the call to DEEORS
     NOP                    \ and return from the subroutine to skip the second call
     RTS                    \ below
    
    ELSE
    
     JSR DEEORS             \ Call DEEORS to decrypt between DOENTRY and F%
    
    ENDIF
    
     LDA #LO(XX21-1)        \ Set FRIN(1 0) = XX21-1 as the low address of the
     STA FRIN               \ decryption block
     LDA #HI(XX21-1)
     STA FRIN+1
    
     LDA #&B1               \ Set (A Y) = &B1FF as the high address of the
     LDY #&FF               \ decryption block
    
     LDX #KEY2              \ Set X = KEY2 as the decryption seed (the value used to
                            \ encrypt the code, which is done in elite-checksum.py)
    
                            \ Fall through into DEEORS to decrypt between XX21 and
                            \ &B1FF
    

[X]

Subroutine [DEEORS](deeors.md) (category: Utility routines)

Decrypt a multi-page block of memory

[X]

Variable [F%](../variable/f_per_cent.md) (category: Utility routines)

Denotes the end of the main game code, from ELITE A to ELITE H

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Variable [G%](../variable/g_per_cent.md) (category: Utility routines)

Denotes the start of the main game code, from ELITE A to ELITE H

[X]

Configuration variable [KEY1](../../all/workspaces.md#key1) = &19

The seed for encrypting the game code from DOENTRY to F%

[X]

Configuration variable [KEY2](../../all/workspaces.md#key2) = &62

The seed for encrypting the game data from XX21 to &BBFF

[X]

Configuration variable [XX21](../../all/workspaces.md#xx21) = &8000

The address of the ship blueprints lookup table, as set in elite-data.asm

[S%](s_per_cent.md "Previous routine")[DEEORS](deeors.md "Next routine")
