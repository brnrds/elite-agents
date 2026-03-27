---
title: "The PDESC subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pdesc.html"
---

[LASLI](lasli.md "Previous routine")[BRIEF2](brief2.md "Next routine")
    
    
           Name: PDESC                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Print the system's extended description or a mission 1 directive
      Deep dive: [Extended system descriptions](https://elite.bbcelite.com/deep_dives/extended_system_descriptions.html)
                 [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-pdesc)
     Variations: See [code variations](../../related/compare/main/subroutine/pdesc.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT25](tt25.md) calls PDESC
    
    
    
    
    
    * * *
    
    
     This prints a specific system's extended description. This is called the "pink
     volcanoes string" in a comment in the original source, and the "goat soup"
     recipe by Ian Bell on his website (where he also refers to the species string
     as the "pink felines" string).
    
     For some special systems, when you are docked at them, the procedurally
     generated extended description is overridden and a text token from the RUTOK
     table is shown instead. If mission 1 is in progress, then a number of systems
     along the route of that mission's story will show custom mission-related
     directives in place of that system's normal "goat soup" phrase.
    
    
    
    * * *
    
    
     Arguments:
    
       ZZ                   The system number (0-255)
    
    
    
    
    .PDESC
    
     LDA QQ8                \ If either byte in QQ18(1 0) is non-zero, meaning that
     ORA QQ8+1              \ the distance from the current system to the selected
     BNE PD1                \ is non-zero, jump to PD1 to show the standard "goat
                            \ soup" description
    
     LDA QQ12               \ If QQ12 does not have bit 7 set, which means we are
     BPL PD1                \ not docked, jump to PD1 to show the standard "goat
                            \ soup" description
    
                            \ If we get here, then the current system is the same as
                            \ the selected system and we are docked, so now to check
                            \ whether there is a special override token for this
                            \ system
    
     LDY #NRU%              \ Set Y as a loop counter as we work our way through the
                            \ system numbers in RUPLA, starting at NRU% (which is
                            \ the number of entries in RUPLA, 26) and working our
                            \ way down to 1
    
    .PDL1
    
     LDA RUPLA-1,Y          \ Fetch the Y-th byte from RUPLA-1 into A (we use
                            \ RUPLA-1 because Y is looping from 26 to 1)
    
     CMP ZZ                 \ If A doesn't match the system whose description we
     BNE PD2                \ are printing (in ZZ), jump to PD2 to keep looping
                            \ through the system numbers in RUPLA
    
                            \ If we get here we have found a match for this system
                            \ number in RUPLA
    
     LDA RUGAL-1,Y          \ Fetch the Y-th byte from RUGAL-1 into A
    
     AND #%01111111         \ Extract bits 0-6 of A
    
     CMP GCNT               \ If the result does not equal the current galaxy
     BNE PD2                \ number, jump to PD2 to keep looping through the system
                            \ numbers in RUPLA
    
     LDA RUGAL-1,Y          \ Fetch the Y-th byte from RUGAL-1 into A, once again
    
     BMI PD3                \ If bit 7 is set, jump to PD3 to print the extended
                            \ token in A from the second table in RUTOK
    
     LDA TP                 \ Fetch bit 0 of TP into the C flag, and skip to PD1 if
     LSR A                  \ it is clear (i.e. if mission 1 is not in progress) to
     BCC PD1                \ print the "goat soup" extended description
    
                            \ If we get here then mission 1 is in progress, so we
                            \ print out the corresponding token from RUTOK
    
     JSR MT14               \ Call MT14 to switch to justified text
    
     LDA #1                 \ Set A = 1 so that extended token 1 (an empty string)
                            \ gets printed below instead of token 176, followed by
                            \ the Y-th token in RUTOK
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &B0, or BIT &B0A9, which does nothing apart
                            \ from affect the flags
    
    .PD3
    
     LDA #176               \ Print extended token 176 ("{lower case}{justify}
     JSR DETOK2             \ {single cap}")
    
     TYA                    \ Print the extended token in Y from the second table
     JSR DETOK3             \ in RUTOK
    
     LDA #177               \ Set A = 177 so when we jump to PD4 in the next
                            \ instruction, we print token 177 (".{cr}{left align}")
    
     BNE PD4                \ Jump to PD4 to print the extended token in A and
                            \ return from the subroutine using a tail call
    
    .PD2
    
     DEY                    \ Decrement the byte counter in Y
    
     BNE PDL1               \ Loop back to check the next byte in RUPLA until we
                            \ either find a match for the system in ZZ, or we fall
                            \ through into the "goat soup" extended description
                            \ routine
    
    .PD1
    
                            \ We now print the "goat soup" extended description
    
     LDX #3                 \ We now want to seed the random number generator with
                            \ the s1 and s2 16-bit seeds from the current system, so
                            \ we get the same extended description for each system
                            \ every time we call PDESC, so set a counter in X for
                            \ copying 4 bytes
    
    .PDL1K                  \ This label is a duplicate of the label above
                            \
                            \ In the original source this label is PDL1, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to PDL1K
    
     LDA QQ15+2,X           \ Copy QQ15+2 to QQ15+5 (s1 and s2) to RAND to RAND+3
     STA RAND,X
    
     DEX                    \ Decrement the loop counter
    
     BPL PDL1K              \ Loop back to PDL1K until we have copied all
    
     LDA #5                 \ Set A = 5, so we print extended token 5 in the next
                            \ instruction ("{lower case}{justify}{single cap}[86-90]
                            \ IS [140-144].{cr}{left align}"
    
    .PD4
    
     JMP DETOK              \ Print the extended token given in A, and return from
                            \ the subroutine using a tail call
    

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [DETOK2](detok2.md) (category: Text)

Print an extended text token (1-255)

[X]

Subroutine [DETOK3](detok3.md) (category: Text)

Print an extended recursive token from the RUTOK token table

[X]

Variable [GCNT](../workspace/wp.md#gcnt) in workspace [WP](../workspace/wp.md)

The number of the current galaxy (0-7)

[X]

Subroutine [MT14](mt14.md) (category: Text)

Switch to justified text when printing extended tokens

[X]

Configuration variable [NRU%](../../all/workspaces.md#nru-per-cent) = 0

The number of planetary systems with extended system description overrides in the RUTOK table

[X]

Label [PD1](pdesc.md#pd1) is local to this routine

[X]

Label [PD2](pdesc.md#pd2) is local to this routine

[X]

Label [PD3](pdesc.md#pd3) is local to this routine

[X]

Label [PD4](pdesc.md#pd4) is local to this routine

[X]

Label [PDL1](pdesc.md#pdl1) is local to this routine

[X]

Label [PDL1K](pdesc.md#pdl1k) is local to this routine

[X]

Variable [QQ12](../workspace/zp.md#qq12) in workspace [ZP](../workspace/zp.md)

Our "docked" status

[X]

Variable [QQ15](../workspace/zp.md#qq15) in workspace [ZP](../workspace/zp.md)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ8](../workspace/zp.md#qq8) in workspace [ZP](../workspace/zp.md)

The distance from the current system to the selected system in light years * 10, stored as a 16-bit number

[X]

Variable [RAND](../workspace/zp.md#rand) in workspace [ZP](../workspace/zp.md)

Four 8-bit seeds for the random number generation system implemented in the DORND routine

[X]

Configuration variable [RUGAL](../../all/workspaces.md#rugal) = &AF62

The address of the extended system description galaxy number table, as set in elite-data.asm

[X]

Configuration variable [RUPLA](../../all/workspaces.md#rupla) = &AF48

The address of the extended system description system number table, as set in elite-data.asm

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[X]

Variable [ZZ](../workspace/zp.md#zz) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for distance values

[LASLI](lasli.md "Previous routine")[BRIEF2](brief2.md "Next routine")
