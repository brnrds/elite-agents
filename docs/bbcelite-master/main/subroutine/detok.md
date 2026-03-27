---
title: "The DETOK subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/detok.html"
---

[DETOK3](detok3.md "Previous routine")[DETOK2](detok2.md "Next routine")
    
    
           Name: DETOK                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print an extended recursive token from the TKN1 token table
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-detok)
     References: This subroutine is called as follows:
                 * [BRIS](bris.md) calls DETOK
                 * [BRP](brp.md) calls DETOK
                 * [CATS](cats.md) calls DETOK
                 * [DELT](delt.md) calls DETOK
                 * [DETOK2](detok2.md) calls DETOK
                 * [dockEd](docked.md) calls DETOK
                 * [FILEPR](filepr.md) calls DETOK
                 * [GTDIR](gtdir.md) calls DETOK
                 * [GTDRV](gtdrv.md) calls DETOK
                 * [GTNMEW](gtnmew.md) calls DETOK
                 * [HME2](hme2.md) calls DETOK
                 * [LOD](lod.md) calls DETOK
                 * [MT17](mt17.md) calls DETOK
                 * [MT28](mt28.md) calls DETOK
                 * [OTHERFILEPR](otherfilepr.md) calls DETOK
                 * [PDESC](pdesc.md) calls DETOK
                 * [STATUS](status.md) calls DETOK
                 * [SVE](sve.md) calls DETOK
                 * [TITLE](title.md) calls DETOK
                 * [TT210](tt210.md) calls DETOK
                 * [TT214](tt214.md) calls DETOK
                 * [DETOK3](detok3.md) calls via DTEN
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The recursive token to be printed, in the range 1-255
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       Y                    Y is preserved
    
       V(1 0)               V(1 0) is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       DTEN                 Print recursive token number X from the token table
                            pointed to by (A V), used to print tokens from the RUTOK
                            table via calls to DETOK3
    
    
    
    
    .DETOK
    
     PHA                    \ Store A on the stack, so we can retrieve it later
    
     TAX                    \ Copy the token number from A into X
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     LDA #LO(TKN1)          \ Set V to the low byte of TKN1
     STA V
    
     LDA #HI(TKN1)          \ Set A to the high byte of TKN1, so when we fall
                            \ through into DTEN, V(1 0) gets set to the address of
                            \ the TKN1 token table
    
    .DTEN
    
     STA V+1                \ Set the high byte of V(1 0) to A, so V(1 0) now points
                            \ to the start of the token table to use
    
     LDY #0                 \ First, we need to work our way through the table until
                            \ we get to the token that we want to print. Tokens are
                            \ delimited by #VE, and VE EOR VE = 0, so we work our
                            \ way through the table in, counting #VE delimiters
                            \ until we have passed X of them, at which point we jump
                            \ down to DTL2 to do the actual printing. So first, we
                            \ set a counter Y to point to the character offset as we
                            \ scan through the table
    
    .DTL1
    
     LDA (V),Y              \ Load the character at offset Y in the token table,
                            \ which is the next character from the token table
    
     EOR #VE                \ Tokens are stored in memory having been EOR'd with
                            \ #VE, so we repeat the EOR to get the actual character
                            \ in this token
    
     BNE DT1                \ If the result is non-zero, then this is a character
                            \ in a token rather than the delimiter (which is #VE),
                            \ so jump to DT1
    
     DEX                    \ We have just scanned the end of a token, so decrement
                            \ X, which contains the token number we are looking for
    
     BEQ DTL2               \ If X has now reached zero then we have found the token
                            \ we are looking for, so jump down to DTL2 to print it
    
    .DT1
    
     INY                    \ Otherwise this isn't the token we are looking for, so
                            \ increment the character pointer
    
     BNE DTL1               \ If Y hasn't just wrapped around to 0, loop back to
                            \ DTL1 to process the next character
    
     INC V+1                \ We have just crossed into a new page, so increment
                            \ V+1 so that V points to the start of the new page
    
     BNE DTL1               \ Jump back to DTL1 to process the next character (this
                            \ BNE is effectively a JMP as V+1 won't reach zero
                            \ before we reach the end of the token table)
    
    .DTL2
    
     INY                    \ We just detected the delimiter byte before the token
                            \ that we want to print, so increment the character
                            \ pointer to point to the first character of the token,
                            \ rather than the delimiter
    
     BNE P%+4               \ If Y hasn't just wrapped around to 0, skip the next
                            \ instruction
    
     INC V+1                \ We have just crossed into a new page, so increment
                            \ V+1 so that V points to the start of the new page
    
     LDA (V),Y              \ Load the character at offset Y in the token table,
                            \ which is the next character from the token we want to
                            \ print
    
     EOR #VE                \ Tokens are stored in memory having been EOR'd with
                            \ #VE, so we repeat the EOR to get the actual character
                            \ in this token
    
     BEQ DTEX               \ If the result is zero, then this is the delimiter at
                            \ the end of the token to print (which is #VE), so jump
                            \ to DTEX to return from the subroutine, as we are done
                            \ printing
    
     JSR DETOK2             \ Otherwise call DETOK2 to print this part of the token
    
     JMP DTL2               \ Jump back to DTL2 to process the next character
    
    .DTEX
    
     PLA                    \ Restore V(1 0) from the stack, so it is preserved
     STA V+1                \ through calls to this routine
     PLA
     STA V
    
     PLA                    \ Restore Y from the stack, so it is preserved through
     TAY                    \ calls to this routine
    
     PLA                    \ Restore A from the stack, so it is preserved through
                            \ calls to this routine
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [DETOK2](detok2.md) (category: Text)

Print an extended text token (1-255)

[X]

Label [DT1](detok.md#dt1) is local to this routine

[X]

Label [DTEX](detok.md#dtex) is local to this routine

[X]

Label [DTL1](detok.md#dtl1) is local to this routine

[X]

Label [DTL2](detok.md#dtl2) is local to this routine

[X]

Configuration variable [TKN1](../../all/workspaces.md#tkn1) = &A400

The address of the extended token table, as set in elite-data.asm

[X]

Variable [V](../workspace/zp.md#v) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing an address pointer

[X]

Configuration variable [VE](../../all/workspaces.md#ve) = &57

The obfuscation byte used to hide the extended tokens table from crackers viewing the binary code

[DETOK3](detok3.md "Previous routine")[DETOK2](detok2.md "Next routine")
