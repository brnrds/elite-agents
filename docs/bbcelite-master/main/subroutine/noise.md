---
title: "The NOISE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/noise.html"
---

[LASNO](lasno.md "Previous routine")[SOINT](soint.md "Next routine")
    
    
           Name: NOISE                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make the sound whose number is in Y by populating the sound buffer
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-noise)
     Variations: See [code variations](../../related/compare/main/subroutine/noise.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BEEP](beep.md) calls NOISE
                 * [BOMBEFF2](bombeff2.md) calls NOISE
                 * [BOOP](boop.md) calls NOISE
                 * [DEATH](death.md) calls NOISE
                 * [ELASNO](elasno.md) calls NOISE
                 * [EXNO](exno.md) calls NOISE
                 * [EXNO3](exno3.md) calls NOISE
                 * [FRMIS](frmis.md) calls NOISE
                 * [LASNO](lasno.md) calls NOISE
                 * [LAUN](laun.md) calls NOISE
                 * [LL164](ll164.md) calls NOISE
                 * [Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md) calls NOISE
                 * [SFRMIS](sfrmis.md) calls NOISE
    
    
    
    
    
    * * *
    
    
     The following sounds can be made by this routine. Two-part noises are made by
     consecutive calls to this routine with different values of Y. The routine
     doesn't make any sounds itself; instead, it populates the sound buffer at
     SOFLG with the relevant sound data, and the interrupt handler at IRQ1 calls
     the SOINT routine to process the data in the sound buffer and send it to the
     76489 sound chip.
    
     This routine can make the following sounds depending on the value of Y:
    
       0       Long, low beep
       1       Short, high beep
       3, 5    Lasers fired by us
       4       We died / Collision / Our depleted shields being hit by lasers
       6       We made a hit or kill / Energy bomb / Other ship exploding
       7       E.C.M. on
       8       Missile launched / Ship launched from station
       9, 5    We're being hit by lasers
       10, 11  Hyperspace drive engaged
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The number of the sound to be made from the above table
    
    
    
    
    .NOISE
    
                            \ This routine appears to set up the contents of the
                            \ SOFLG sound buffer, so the SOINT routine can then send
                            \ the results to the 76489 sound chip. How this all
                            \ works is a bit of a mystery, so this part needs more
                            \ investigation
    
     LDA DNOIZ              \ If DNOIZ is non-zero, then sound is disabled, so
     BNE SOUR1              \ return from the subroutine (as SOUR1 contains an RTS)
    
     LDA SFXBT,Y            \ Fetch SFXBT+Y and shift bit 0 into the C flag
     LSR A
    
     CLV                    \ Clear the V flag
    
     LDX #0                 \ If bit 0 of SFXBT+Y is set, set X = 0 and jump to
     BCS SOUS4              \ SOUS4
    
     INX                    \ Increment X to 1
    
     LDA SOPR+1             \ If SOPR+1 < SOPR+2, set X = 1 and jump to SOUS4
     CMP SOPR+2
     BCC SOUS4
    
     INX                    \ SOPR+1 >= SOPR+2, so increment X to 2
    
    \JSR SOUS4              \ These instructions are commented out in the original
    \                       \ source
    \BCC SOUR1
    \
    \DEX
    \
    \BIT SOUR1
    \
    \LDA SFXPR,Y
    \AND #&10
    \BEQ SOUS9
    \
    \RTS
    \
    \fall into SOUS4 since
    \this facility not
    \needed
    
    .SOUS4
    
                            \ By the time we get here, X is set as follows:
                            \
                            \   * X = 0 if bit 0 of SFXBT+Y is set
                            \   * X = 1 if SOPR+1 < SOPR+2
                            \   * X = 2 if SOPR+1 >= SOPR+2
    
     LDA SFXPR,Y            \ Set A = SFXPR+Y
    
    .SOUS9
    
     CMP SOPR,X             \ If SFXPR+Y < SOPR+X, return from the subroutine
     BCC SOUR1              \ (as SOUR1 contains an RTS)
    
     SEI                    \ Disable interrupts while we update the sound buffer
    
                            \ If we get here then SFXPR+Y >= SOPR+X
    
     STA SOPR,X             \ SOPR+X = A = SFXPR+Y
    
     LSR A                  \ Store bits 1-3 of SFXPR+Y in bits 0-2 of SOVOL+X
     AND #%00000111
     STA SOVOL,X
    
     LDA SFXVC,Y            \ Store SFXVC+Y in SOVCH+X
     STA SOVCH,X
    
     LDA SFXBT,Y            \ Store SFXBT+Y in SOCNT+X
     STA SOCNT,X
    
     AND #%00001111         \ Store bits 1-3 of SFXBT+Y in bits 0-2 of SOFRCH+X
     LSR A
     STA SOFRCH,X
    
     LDA SFXFQ,Y            \ Set A = SFXFQ+Y
    
     BVC P%+3               \ If the V flag is set, double the value in A
     ASL A
    
     STA SOFRQ,X            \ Store A in SOFRQ+X
    
     LDA #%10000000         \ Set bit 7 of SOFLG+X
     STA SOFLG,X
    
     CLI                    \ Enable interrupts again
    
     SEC                    \ Set the C flag
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [DNOIZ](../workspace/up.md#dnoiz) in workspace [UP](../workspace/up.md)

Sound on/off configuration setting

[X]

Variable [SFXBT](../variable/sfxbt.md) (category: Sound)

Sound data block 2

[X]

Variable [SFXFQ](../variable/sfxfq.md) (category: Sound)

Sound data block 3

[X]

Variable [SFXPR](../variable/sfxpr.md) (category: Sound)

Sound data block 1

[X]

Variable [SFXVC](../variable/sfxvc.md) (category: Sound)

Sound data block 4

[X]

Variable [SOCNT](../workspace/sound_variables.md#socnt) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for sound effect counters

[X]

Variable [SOFLG](../workspace/sound_variables.md#soflg) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for sound effect flags

[X]

Variable [SOFRCH](../workspace/sound_variables.md#sofrch) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for frequency change values

[X]

Variable [SOFRQ](../workspace/sound_variables.md#sofrq) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for sound effect frequencies

[X]

Variable [SOPR](../workspace/sound_variables.md#sopr) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for sound effect priorities

[X]

Entry point [SOUR1](sous1.md#sour1) in subroutine [SOUS1](sous1.md) (category: Sound)

Contains an RTS

[X]

Label [SOUS4](noise.md#sous4) is local to this routine

[X]

Variable [SOVCH](../workspace/sound_variables.md#sovch) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for the volume change rate

[X]

Variable [SOVOL](../workspace/sound_variables.md#sovol) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for volume levels

[LASNO](lasno.md "Previous routine")[SOINT](soint.md "Next routine")
