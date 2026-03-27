---
title: "Elite A source in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_a.html"
---

[Workspaces and configuration](workspaces.md "Previous routine")[Elite B source](elite_b.md "Next routine")
    
    
     ELITE A FILE
    
    
    
    
     ORG CODE%              \ Set the assembly address to CODE%
    
     LOAD_A% = LOAD%
    
    
    
    
           Name: TVT3                                                    [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Palette data for the mode 1 part of the screen (the top part)
    
    
        Context: See this variable [on its own page](../main/variable/tvt3.md)
     Variations: See [code variations](../related/compare/main/variable/tvt3.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [IRQ1](../main/subroutine/irq1.md) uses TVT3
    
    
    
    
    
    * * *
    
    
     The following table contains four different mode 1 palettes, each of which
     sets a four-colour palette for the top part of the screen. Mode 1 supports
     four colours on-screen and in Elite colour 0 is always set to black, so each
     of the palettes in this table defines the three other colours (1 to 3).
    
     There is some consistency between the palettes:
    
       * Colour 0 is always black
       * Colour 1 (#YELLOW) is always yellow
       * Colour 2 (#RED) is normally red-like (i.e. red or magenta)
                  ... except in the title screen palette, when it is white
       * Colour 3 (#CYAN) is always cyan-like (i.e. white or cyan)
    
     The configuration variables of #YELLOW, #RED and #CYAN are a bit misleading,
     but if you think of them in terms of hue rather than specific colours, they
     work reasonably well (outside of the title screen palette, anyway).
    
     The palettes are set in the IRQ1 handler that implements the split screen
     mode, and can be changed by calling the SETVDU19 routine to set the offset to
     the new palette in this table.
    
     This table must start on a page boundary (i.e. an address that ends in two
     zeroes in hexadecimal). In the release version of the game TVT3 is at &2C00.
     This is so the SETVDU19 routine can switch palettes properly, as it does this
     by overwriting the low byte of the palette data address with a new offset, so
     the low byte for first palette's address must be 0.
    
     Palette data is given as a set of bytes, with each byte mapping a logical
     colour to a physical one. In each byte, the logical colour is given in bits
     4-7 and the physical colour in bits 0-3. See page 379 of the "Advanced User
     Guide for the BBC Micro" by Bray, Dickens and Holmes for details of how
     palette mapping works, as in modes 1 and 2 we have to do multiple palette
     commands to change the colours correctly, and the physical colour value is
     EOR'd with 7, just to make things even more confusing.
    
    
    
    
    .TVT3
    
     EQUB &00, &34          \ 1 = yellow, 2 = red, 3 = cyan (space view)
     EQUB &24, &17          \
     EQUB &74, &64          \ Set with a call to SETVDU19 with A = 0, after which:
     EQUB &57, &47          \
     EQUB &B1, &A1          \   #YELLOW = yellow
     EQUB &96, &86          \   #RED    = red
     EQUB &F1, &E1          \   #CYAN   = cyan
     EQUB &D6, &C6          \   #GREEN  = cyan/yellow stripe
                            \   #WHITE  = cyan/red stripe
    
     EQUB &00, &34          \ 1 = yellow, 2 = red, 3 = white (chart view)
     EQUB &24, &17          \
     EQUB &74, &64          \ Set with a call to SETVDU19 with A = 16, after which:
     EQUB &57, &47          \
     EQUB &B0, &A0          \   #YELLOW = yellow
     EQUB &96, &86          \   #RED    = red
     EQUB &F0, &E0          \   #CYAN   = white
     EQUB &D6, &C6          \   #GREEN  = white/yellow stripe
                            \   #WHITE  = white/red stripe
    
     EQUB &00, &34          \ 1 = yellow, 2 = white, 3 = cyan (title screen)
     EQUB &24, &17          \
     EQUB &74, &64          \ Set with a call to SETVDU19 with A = 32, after which:
     EQUB &57, &47          \
     EQUB &B1, &A1          \   #YELLOW = yellow
     EQUB &90, &80          \   #RED    = white
     EQUB &F1, &E1          \   #CYAN   = cyan
     EQUB &D0, &C0          \   #GREEN  = cyan/yellow stripe
                            \   #WHITE  = cyan/white stripe
    
     EQUB &00, &34          \ 1 = yellow, 2 = magenta, 3 = white (trade view)
     EQUB &24, &17          \
     EQUB &74, &64          \ Set with a call to SETVDU19 with A = 48, after which:
     EQUB &57, &47          \
     EQUB &B0, &A0          \   #YELLOW = yellow
     EQUB &92, &82          \   #RED    = magenta
     EQUB &F0, &E0          \   #CYAN   = white
     EQUB &D2, &C2          \   #GREEN  = white/yellow stripe
                            \   #WHITE  = white/magenta stripe
    
    
    
    
           Name: VEC                                                     [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: The original value of the IRQ1 vector
    
    
        Context: See this variable [on its own page](../main/variable/vec.md)
     Variations: See [code variations](../related/compare/main/variable/vec.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [SETINTS](../main/subroutine/setints.md) uses VEC
    
    
    
    
    
    
    .VEC
    
     EQUW &8888             \ This gets set to the value of the original IRQ1 vector
                            \ by the STARTUP routine
    
    
    
    
           Name: WSCAN                                                   [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Implement the #wscn command (wait for the vertical sync)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/wscan.md)
     Variations: See [code variations](../related/compare/main/subroutine/wscan.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DELAY](../main/subroutine/delay.md) calls WSCAN
                 * [DK4](../main/subroutine/dk4.md) calls WSCAN
                 * [TT16](../main/subroutine/tt16.md) calls WSCAN
    
    
    
    
    
    * * *
    
    
     Wait for vertical sync to occur on the video system - in other words, wait
     for the screen to start its refresh cycle, which it does 50 times a second
     (50Hz).
    
    
    
    
    .WSCAN
    
     STZ DL                 \ Set DL to 0
    
    .DELL1K                 \ This label is a duplicate of a label in the DELT
                            \ routine
                            \
                            \ In the original source this label is DELL1, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to DELL1K
    
     LDA DL                 \ Loop round these two instructions until DL is no
     BEQ DELL1K             \ longer 0 (DL gets set to 30 in the LINSCN routine,
                            \ which is run when vertical sync has occurred on the
                            \ video system, so DL will change to a non-zero value
                            \ at the start of each screen refresh)
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DELAY                                                   [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Wait for a specified time, in 1/50s of a second
    
    
        Context: See this subroutine [on its own page](../main/subroutine/delay.md)
     Variations: See [code variations](../related/compare/main/subroutine/delay.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIS](../main/subroutine/bris.md) calls DELAY
                 * [DK4](../main/subroutine/dk4.md) calls DELAY
                 * [DKS3](../main/subroutine/dks3.md) calls DELAY
                 * [dn2](../main/subroutine/dn2.md) calls DELAY
                 * [DOENTRY](../main/subroutine/doentry.md) calls DELAY
                 * [Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md) calls DELAY
                 * [MT26](../main/subroutine/mt26.md) calls DELAY
                 * [TT217](../main/subroutine/tt217.md) calls DELAY
    
    
    
    
    
    * * *
    
    
     Wait for the number of vertical syncs given in Y, so this effectively waits
     for Y/50 of a second (as the vertical sync occurs 50 times a second).
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The number of vertical sync events to wait for
    
    
    
    
    .DELAY
    
     JSR WSCAN              \ Call WSCAN to wait for the vertical sync, so the whole
                            \ screen gets drawn
    
     DEY                    \ Decrement the counter in Y
    
     BNE DELAY              \ If Y isn't yet at zero, jump back to DELAY to wait
                            \ for another vertical sync
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: BOOP                                                    [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make a long, low beep
    
    
        Context: See this subroutine [on its own page](../main/subroutine/boop.md)
     References: This subroutine is called as follows:
                 * [HME2](../main/subroutine/hme2.md) calls BOOP
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls BOOP
                 * [WARP](../main/subroutine/warp.md) calls BOOP
    
    
    
    
    
    
    .BOOP
    
     LDY #soboop            \ Call the NOISE routine with Y = 0 to make a long, low
     BRA NOISE              \ beep, returning from the subroutine using a tail call
    
    
    
    
           Name: BEEP                                                    [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make a short, high beep
    
    
        Context: See this subroutine [on its own page](../main/subroutine/beep.md)
     Variations: See [code variations](../related/compare/main/subroutine/beep.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CHPR](../main/subroutine/chpr.md) calls BEEP
                 * [DK4](../main/subroutine/dk4.md) calls BEEP
                 * [dn2](../main/subroutine/dn2.md) calls BEEP
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls BEEP
    
    
    
    
    
    
    .BEEP
    
     LDY #sobeep            \ Call the NOISE routine with Y = 1 to make a short,
     BRA NOISE              \ high beep, returning from the subroutine using a tail
                            \ call
    
    
    
    
           Name: SOFH                                                    [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound chip data mask for choosing a tone channel in the range 0-2
    
    
        Context: See this variable [on its own page](../main/variable/sofh.md)
     References: This variable is used as follows:
                 * [SOINT](../main/subroutine/soint.md) uses SOFH
    
    
    
    
    
    
    .SOFH
    
     EQUB %11000000         \ Mask for a latch byte for channel 2
     EQUB %10100000         \ Mask for a latch byte for channel 1
     EQUB %10000000         \ Mask for a latch byte for channel 0
    
    
    
    
           Name: SOOFF                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound chip data to turn the volume down on all channels and to act
                 as a mask for choosing a tone channel in the range 0-2
    
    
        Context: See this variable [on its own page](../main/variable/sooff.md)
     References: This variable is used as follows:
                 * [SOFLUSH](../main/subroutine/soflush.md) uses SOOFF
                 * [SOINT](../main/subroutine/soint.md) uses SOOFF
    
    
    
    
    
    
    .SOOFF
    
     EQUB %11111111         \            %1         %11               %1     %1111
                            \ Latch byte (1), channel 3, volume latch (1), data 15
    
     EQUB %10111111         \            %1         %01               %1     %1111
                            \ Latch byte (1), channel 1, volume latch (1), data 15
    
     EQUB %10011111         \            %1         %00               %1     %1111
                            \ Latch byte (1), channel 0, volume latch (1), data 15
    
     EQUB %11011111         \            %1         %10               %1     %1111
                            \ Latch byte (1), channel 2, volume latch (1), data 15
    
     EQUB %11101111         \            %1         %11             %0       %1111
                            \ Latch byte (1), channel 3,   tone latch (0), data 15
    
    
    
    
           Name: SOUS1                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Write sound data directly to the 76489 sound chip
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sous1.md)
     References: This subroutine is called as follows:
                 * [SOFLUSH](../main/subroutine/soflush.md) calls SOUS1
                 * [SOINT](../main/subroutine/soint.md) calls SOUS1
                 * [NOISE](../main/subroutine/noise.md) calls via SOUR1
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The sound byte to send to the 76489 sound chip
    
    
    
    * * *
    
    
     Other entry points:
    
       SOUR1                Contains an RTS
    
    
    
    
    .SOUS1
    
     LDX #%11111111         \ Set 6522 System VIA data direction register DDRA
     STX VIA+&43            \ (SHEILA &43) to %11111111. This sets the ORA register
                            \ so that bits 0-7 of ORA will be sent to the 76489
                            \ sound chip
    
     STA VIA+&4F            \ Set 6522 System VIA output register ORA (SHEILA &4F)
                            \ to A, the sound data we want to send
    
     LDA #%00000000         \ Activate the sound chip by clearing bit 3 of the
     STA VIA+&40            \ 6522 System VIA output register ORB (SHEILA &40)
    
     PHA                    \ These instructions don't do anything apart from
     PLA                    \ keeping the sound chip activated for at least 8us,
     PHA                    \ which we need to do in order for the data to make
     PLA                    \ it to the chip
    
     LDA #%00001000         \ Deactivate the sound chip by setting bit 3 of the
     STA VIA+&40            \ 6522 System VIA output register ORB (SHEILA &40)
    
    .SOUR1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SOFLUSH                                                 [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Reset the sound buffers and turn off all sound channels
    
    
        Context: See this subroutine [on its own page](../main/subroutine/soflush.md)
     References: This subroutine is called as follows:
                 * [COLD](../main/subroutine/cold.md) calls SOFLUSH
    
    
    
    
    
    
    .SOFLUSH
    
     LDY #3                 \ We need to zero the first 3 bytes of the sound buffer
                            \ at SOFLG, so set a counter in Y
    
     LDA #0                 \ Set A to 0 so we can zero the sound buffer
    
    .SOUL2
    
     STA SOFLG-1,Y          \ Zero the Y-1th byte of SOFLG
    
     DEY                    \ Decrement the loop counter
    
     BNE SOUL2              \ Loop back to zero the next byte until we have done all
                            \ three from SOFLG+2 down to SOFLG
    
     SEI                    \ Disable interrupts while we update the sound chip
    
                            \ At this point Y = 0, which we can now use as a loop
                            \ counter to loop through the 5 bytes in SOOFF and send
                            \ them directly to the 76489 sound chip to set the
                            \ volume levels on all 4 sound channels to 15 (silent)
                            \ and the noise register on channel 3 to %111
    
    .SOUL1
    
     LDA SOOFF,Y            \ Fetch the Y-th byte of SOOFF
    
     JSR SOUS1              \ Write the value in A directly to the 76489 sound chip
    
     INY                    \ Increment the loop counter
    
     CPY #5                 \ Loop back until we have sent all 5 bytes to the sound
     BNE SOUL1              \ chip
    
     CLI                    \ Enable interrupts again
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: ELASNO                                                  [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make the sound of us being hit
    
    
        Context: See this subroutine [on its own page](../main/subroutine/elasno.md)
     References: This subroutine is called as follows:
                 * [TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md) calls ELASNO
    
    
    
    
    
    
    .ELASNO
    
     LDY #9                 \ Call the NOISE routine with Y = 9 to make the first
     JSR NOISE              \ sound of us being hit
    
     LDY #5                 \ Call the NOISE routine with Y = 5 to make the second
     BRA NOISE              \ sound of us being hit, returning from the subroutine
                            \ using a tail call
    
    
    
    
           Name: LASNO                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make the sound of our laser firing
    
    
        Context: See this subroutine [on its own page](../main/subroutine/lasno.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls LASNO
    
    
    
    
    
    
    .LASNO
    
     LDY #3                 \ Call the NOISE routine with Y = 3 to make the first
     JSR NOISE              \ sound of us firing our lasers
    
     LDY #5                 \ Set Y = 5 and fall through into the NOISE routine to
                            \ make the second sound of us firing our lasers
    
    
    
    
           Name: NOISE                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make the sound whose number is in Y by populating the sound buffer
    
    
        Context: See this subroutine [on its own page](../main/subroutine/noise.md)
     Variations: See [code variations](../related/compare/main/subroutine/noise.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BEEP](../main/subroutine/beep.md) calls NOISE
                 * [BOMBEFF2](../main/subroutine/bombeff2.md) calls NOISE
                 * [BOOP](../main/subroutine/boop.md) calls NOISE
                 * [DEATH](../main/subroutine/death.md) calls NOISE
                 * [ELASNO](../main/subroutine/elasno.md) calls NOISE
                 * [EXNO](../main/subroutine/exno.md) calls NOISE
                 * [EXNO3](../main/subroutine/exno3.md) calls NOISE
                 * [FRMIS](../main/subroutine/frmis.md) calls NOISE
                 * [LASNO](../main/subroutine/lasno.md) calls NOISE
                 * [LAUN](../main/subroutine/laun.md) calls NOISE
                 * [LL164](../main/subroutine/ll164.md) calls NOISE
                 * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md) calls NOISE
                 * [SFRMIS](../main/subroutine/sfrmis.md) calls NOISE
    
    
    
    
    
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
    
    
    
    
           Name: SOINT                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Process the contents of the sound buffer and send it to the sound
                 chip
    
    
        Context: See this subroutine [on its own page](../main/subroutine/soint.md)
     References: This subroutine is called as follows:
                 * [IRQ1](../main/subroutine/irq1.md) calls SOINT
    
    
    
    
    
    
    .SOINT
    
                            \ This routine is called from the IRQ1 interrupt handler
                            \ and appears to process the contents of the SOFLG sound
                            \ buffer, sending the results to the 76489 sound chip.
                            \ What it's actually doing, though, is a bit of a
                            \ mystery, so this part needs more investigation
    
     LDY #2                 \ We want to loop through the three tone channels, so
                            \ set a counter in Y to iterate through the channels
    
    .SOUL8
    
     LDA SOFLG,Y            \ If the Y-th byte of SOFLG is zero, there is no data
     BEQ SOUL3              \ buffered for this channel, so jump to SOUL3 to move
                            \ onto the next one
    
     BMI SOUL4              \ If bit 7 of the Y-th byte of SOFLG is set, jump to
                            \ SOUL4
    
     LDA SOFRCH,Y           \ If SOFRCH+Y = 0, jump to SOUL5
     BEQ SOUL5
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &00, or BIT &00A9, which does nothing apart
                            \ from affect the flags
    
    .SOUL4
    
     LDA #0                 \ Set A = 0
    
     CLC                    \ Clear the C flag for the additions below
    
     CLD                    \ Clear the D flag to ensure we are in binary mode
    
     ADC SOFRQ,Y            \ Set SOFRQ+Y = SOFRQ+Y + A
     STA SOFRQ,Y
    
     PHA                    \ Store A on the stack
    
     ASL A                  \ Set A = (A * 4) mod 16
     ASL A
     AND #%00001111
    
     ORA SOFH,Y             \ Set the channel to 0, 1, 2 for Y = 2, 1, 0
    
     JSR SOUS1              \ Write the value in A directly to the 76489 sound chip
    
     PLA                    \ Retrieve A from the stack
    
     LSR A                  \ Set A = A / 4
     LSR A
    
     JSR SOUS1              \ Write the value in A directly to the 76489 sound chip
    
    .SOUL5
    
     TYA                    \ Copy Y into X
     TAX
    
     LDA SOFLG,Y            \ If bit 7 of the Y-th byte of SOFLG is set, jump to
     BMI SOUL6              \ SOUL6
    
     DEC SOCNT,X            \ Decrement SOCNT+X
    
     BEQ SOKILL             \ If the value is zero, skip to SOKILL
    
     LDA SOCNT,X            \ If SOCNT+X AND SOVCH+X is non-zero, skip to SOUL3
     AND SOVCH,X
     BNE SOUL3
    
     DEC SOVOL,X            \ Decrement SOVOL+X
    
     BNE SOU1               \ If the value is non-zero, skip to SOU1
    
    .SOKILL
    
     LDA #0                 \ Set SOFLG+Y = 0
     STA SOFLG,Y
    
     STA SOPR,Y             \ Set SOPR+Y = 0
    
     BEQ SOU3               \ Jump to SOU3 (this BEQ is effectively a JMP as A is
                            \ always zero)
    
    .SOUL6
    
     LSR SOFLG,X            \ Halve the value in SOFLG+X
    
    .SOU1
    
     LDA SOVOL,Y            \ Set A = SOVOL+Y + VOL
     CLC                    \
     ADC VOL                \ where VOL is the current volume setting (0-7)
    
    .SOU3
    
     EOR SOOFF,Y            \ EOR A with the Y-th byte of SOOFF
    
     JSR SOUS1              \ Write the value in A directly to the 76489 sound chip
    
    .SOUL3
    
     DEY                    \ Decrement the loop counter
    
     BPL SOUL8              \ Loop back to SOUL8 until we have done all three
                            \ channels
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: Sound variables                                         [Show more]
           Type: Workspace
        Address: &144D to &1491
       Category: Sound
        Summary: The sound buffer where the data to be sent to the sound chip is
                 processed
    
    
        Context: See this workspace [on its own page](../main/workspace/sound_variables.md)
     References: No direct references to this workspace in this source file
    
    
    
    
    
    
    .SOFLG
    
     EQUB 0                 \ Sound buffer for sound effect flags
     EQUB 0                 \
     EQUB 0                 \ SOFLG,Y contains the following:
                            \
                            \   * Bits 0-5: sound effect number + 1 of the sound
                            \               currently being made on voice Y
                            \
                            \   * Bit 7 is set if this is a new sound being made,
                            \     rather than one that is in progress
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../main/subroutine/noise.md)
                            \   * [SOFLUSH](../main/subroutine/soflush.md)
                            \   * [SOINT](../main/subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOCNT
    
     EQUB 0                 \ Sound buffer for sound effect counters
     EQUB 0                 \
     EQUB 0                 \ SOCNT,Y contains the counter of the sound currently
                            \ being made on voice Y
                            \
                            \ The counter decrements each frame, and when it reaches
                            \ zero, the sound effect has finished
                            \
                            \ These values come from the SFXCNT table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../main/subroutine/noise.md)
                            \   * [SOINT](../main/subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOVOL
    
     EQUB 0                 \ Sound buffer for volume levels
     EQUB 0                 \
     EQUB 0                 \ SOVOL,Y contains the volume of the sound currently
                            \ being made on voice Y
                            \
                            \ These values come from bits 1-3 of the SFXPR table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../main/subroutine/noise.md)
                            \   * [SOINT](../main/subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOVCH
    
     EQUB 0                 \ Sound buffer for the volume change rate
     EQUB 0                 \
     EQUB 0                 \ SOVCH,Y contains the volume change rate of the sound
                            \ currently being made on voice Y
                            \
                            \ The sound's volume gets reduced by one every SOVCH,Y
                            \ frames
                            \
                            \ These values come from the SFXVCH table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../main/subroutine/noise.md)
                            \   * [SOINT](../main/subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOPR
    
     EQUB 0                 \ Sound buffer for sound effect priorities
     EQUB 0                 \
     EQUB 0                 \ SOPR,Y contains the priority of the sound currently
                            \ being made on voice Y
                            \
                            \ These values come from the SFXPR table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../main/subroutine/noise.md)
                            \   * [SOINT](../main/subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOFRCH
    
     EQUB 0                 \ Sound buffer for frequency change values
     EQUB 0                 \
     EQUB 0                 \ SOFRCH,Y contains the frequency change to be applied
                            \ to the sound currently being made on voice Y in each
                            \ frame
                            \
                            \ These values come from the SFXFRCH table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../main/subroutine/noise.md)
                            \   * [SOINT](../main/subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOFRQ
    
     EQUB 0                 \ Sound buffer for sound effect frequencies
     EQUB 0                 \
     EQUB 0                 \ SOFRQ,Y contains the frequency of the sound currently
                            \ being made on voice Y
                            \
                            \ These values come from the SFXFQ table, and have the
                            \ frequency change from the SFXFRCH table applied in
                            \ each frame
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../main/subroutine/noise.md)
                            \   * [SOINT](../main/subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    
    
    
           Name: SFXPR                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound data block 1
    
    
        Context: See this variable [on its own page](../main/variable/sfxpr.md)
     References: This variable is used as follows:
                 * [NOISE](../main/subroutine/noise.md) uses SFXPR
    
    
    
    
    
    
    .SFXPR
    
     EQUB &4B, &5B, &3F
     EQUB &EB, &FF, &09
     EQUB &FF, &8B, &CF
     EQUB &E7, &FF, &EF
    
    
    
    
           Name: SFXBT                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound data block 2
    
    
        Context: See this variable [on its own page](../main/variable/sfxbt.md)
     References: This variable is used as follows:
                 * [NOISE](../main/subroutine/noise.md) uses SFXBT
    
    
    
    
    
    
    .SFXBT
    
     EQUB &40, &10, &01
     EQUB &FC, &F3, &19
     EQUB &F9, &7C, &F1
     EQUB &FA, &FE, &FE
    
    
    
    
           Name: SFXFQ                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound data block 3
    
    
        Context: See this variable [on its own page](../main/variable/sfxfq.md)
     References: This variable is used as follows:
                 * [NOISE](../main/subroutine/noise.md) uses SFXFQ
    
    
    
    
    
    
    .SFXFQ
    
     EQUB &F0, &20, &10
     EQUB &30, &03, &01
     EQUB &08, &80, &16
     EQUB &38, &00, &80
    
    
    
    
           Name: SFXVC                                                   [Show more]
           Type: Variable
       Category: Sound
        Summary: Sound data block 4
    
    
        Context: See this variable [on its own page](../main/variable/sfxvc.md)
     References: This variable is used as follows:
                 * [NOISE](../main/subroutine/noise.md) uses SFXVC
    
    
    
    
    
    
    .SFXVC
    
     EQUB &FF, &FF, &00
     EQUB &03, &1F, &01
     EQUB &07, &07, &0F
     EQUB &03, &0F, &0F
    
    
    
    
           Name: SETINTS                                                 [Show more]
           Type: Subroutine
       Category: Loader
        Summary: Set the various vectors, interrupts and timers
    
    
        Context: See this subroutine [on its own page](../main/subroutine/setints.md)
     Variations: See [code variations](../related/compare/main/subroutine/startup-setints.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [COLD](../main/subroutine/cold.md) calls SETINTS
    
    
    
    
    
    
    .SETINTS
    
     SEI                    \ Disable interrupts
    
     LDA #%00111001         \ Set 6522 System VIA interrupt enable register IER
     STA VIA+&4E            \ (SHEILA &4E) bits 0 and 3-5 (i.e. disable the Timer1,
                            \ CB1, CB2 and CA2 interrupts from the System VIA)
    
     LDA #%01111111         \ Set 6522 User VIA interrupt enable register IER
     STA &FE6E              \ (SHEILA &6E) bits 0-7 (i.e. disable all hardware
                            \ interrupts from the User VIA)
    
     LDA IRQ1V              \ Store the current IRQ1V vector in VEC, so VEC(1 0) now
     STA VEC                \ contains the original address of the IRQ1 handler
     LDA IRQ1V+1
     STA VEC+1
    
     LDA #LO(IRQ1)          \ Set the IRQ1V vector to IRQ1, so IRQ1 is now the
     STA IRQ1V              \ interrupt handler
     LDA #HI(IRQ1)
     STA IRQ1V+1
    
     LDA VSCAN              \ Set 6522 System VIA T1C-L timer 1 high-order counter
     STA VIA+&45            \ (SHEILA &45) to the contents of VSCAN (57) to start
                            \ the T1 counter counting down from 14592 at a rate of
                            \ 1 MHz
    
     CLI                    \ Enable interrupts again
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TVT1                                                    [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Palette data for the mode 2 part of the screen (the dashboard)
    
    
        Context: See this variable [on its own page](../main/variable/tvt1.md)
     References: This variable is used as follows:
                 * [IRQ1](../main/subroutine/irq1.md) uses TVT1
    
    
    
    
    
    * * *
    
    
     This palette is applied in the IRQ1 routine. If we have an escape pod fitted,
     then the first byte is changed to &30, which maps logical colour 3 to actual
     colour 0 (black) instead of colour 4 (blue).
    
    
    
    
    .TVT1
    
     EQUB &34, &43
     EQUB &25, &16
     EQUB &86, &70
     EQUB &61, &52
     EQUB &C3, &B4
     EQUB &A5, &96
     EQUB &07, &F0
     EQUB &E1, &D2
    
    
    
    
           Name: IRQ1                                                    [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: The main screen-mode interrupt handler (IRQ1V points here)
      Deep dive: [The split-screen mode in BBC Micro Elite](https://elite.bbcelite.com/deep_dives/the_split-screen_mode.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/irq1.md)
     Variations: See [code variations](../related/compare/main/subroutine/irq1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SETINTS](../main/subroutine/setints.md) calls IRQ1
    
    
    
    
    
    * * *
    
    
     The main interrupt handler, which implements Elite's split-screen mode.
    
     IRQ1V is set to point to IRQ1 by the loading process.
    
    
    
    
    .IRQ1
    
     PHY                    \ Store Y on the stack
    
     LDY #15                \ Set Y as a counter for 16 bytes, to use when setting
                            \ the dashboard palette below
    
     LDA #%00000010         \ Read the 6522 System VIA status byte bit 1 (SHEILA
     BIT VIA+&4D            \ &4D), which is set if vertical sync has occurred on
                            \ the video system
    
     BNE LINSCN             \ If we are on the vertical sync pulse, jump to LINSCN
                            \ to set up the timers to enable us to switch the
                            \ screen mode between the space view and dashboard
    
    \BVC jvec               \ This instruction is commented out in the original
                            \ source
    
     LDA #%00010100         \ Set the Video ULA control register (SHEILA &20) to
     STA VIA+&20            \ %00010100, which is the same as switching to mode 2,
                            \ (i.e. the bottom part of the screen) but with no
                            \ cursor
    
     LDA ESCP               \ Set A = ESCP, which is &FF if we have an escape pod
                            \ fitted, or 0 if we don't
    
     AND #4                 \ Set A = 4 if we have an escape pod fitted, or 0 if we
                            \ don't
    
     EOR #&34               \ Set A = &30 if we have an escape pod fitted, or &34 if
                            \ we don't
    
     STA &FE21              \ Store A in SHEILA &21 to map colour 3 (#YELLOW2) to
                            \ white if we have an escape pod fitted, or yellow if we
                            \ don't, so the outline colour of the dashboard changes
                            \ from yellow to white if we have an escape pod fitted
    
                            \ The following loop copies bytes #15 to #1 from TVT1 to
                            \ SHEILA &21, but not byte #0, as we just did that
                            \ colour mapping
    
    .VNT2
    
     LDA TVT1,Y             \ Copy the Y-th palette byte from TVT1 to SHEILA &21
     STA &FE21              \ to map logical to actual colours for the bottom part
                            \ of the screen (i.e. the dashboard)
    
     DEY                    \ Decrement the palette byte counter
    
     BNE VNT2               \ Loop back to VNT2 until we have copied all the palette
                            \ bytes bar the first one
    
    IF _COMPACT
    
     LDA MOS                \ If MOS = 0 then this is a Master Compact, so jump to
     BEQ jvec               \ jvec to skip reading the ADC channels (as the Compact
                            \ has a digital joystick rather than an analogue one)
    
    ENDIF
    
     LDA VIA+&18            \ Fetch the ADC channel number into Y from bits 0-1 of
    \BMI JONO               \ the ADC status byte at SHEILA &18
     AND #%00000011         \
     TAY                    \ The BMI is commented out in the original source
    
     LDA VIA+&19            \ Fetch the high byte of the value on this ADC channel
                            \ at SHEILA &19 to read the relevant joystick position
    
     STA JOPOS,Y            \ Store this value in the appropriate JOPOS byte
    
     INY                    \ Increment the channel number
    
     TYA                    \ If the new channel number in A < 3, skip the next two
     CMP #3                 \ instructions
     BCC P%+4
    
     LDA #0                 \ Set the ADC status byte at SHEILA &18 to 0 to reset
     STA VIA+&18            \ the data latch
    
    .jvec
    
     PLY                    \ Restore Y from the stack
    
     LDA VIA+&44            \ Read 6522 System VIA T1C-L timer 1 low-order counter
                            \ (SHEILA &44)
    
     LDA &FC                \ Restore the value of A from before the call to the
                            \ interrupt handler (the MOS stores the value of A in
                            \ location &FC before calling the interrupt handler)
    
     RTI                    \ Return from interrupts, so this interrupt is not
                            \ passed on to the next interrupt handler, but instead
                            \ the interrupt terminates here
    
    .LINSCN
    
     LDA VIA+&41            \ Read 6522 System VIA input register IRA (SHEILA &41)
    
     LDA &FC                \ Fetch the value of A from before the call to the
                            \ interrupt handler (the MOS stores the value of A in
                            \ location &FC before calling the interrupt handler)
    
     PHA                    \ Store the original value of A on the stack
    
     LDA VSCAN+1            \ Set the line scan counter to the value of VSCAN+1
     STA DL                 \ (which contains 30 by default and doesn't change), so
                            \ routines like WSCAN can set DL to 0 and then wait for
                            \ it to change to this value to catch the vertical sync
    
     STA VIA+&44            \ Set 6522 System VIA T1C-L timer 1 low-order counter
                            \ (SHEILA &44) to 30
    
     LDA VSCAN              \ Set 6522 System VIA T1C-L timer 1 high-order counter
     STA VIA+&45            \ (SHEILA &45) to the contents of VSCAN (57) to start
                            \ the T1 counter counting down from 14622 at a rate of
                            \ 1 MHz
    
     LDA HFX                \ If the hyperspace effect flag in HFX is non-zero, then
     BNE j2vec              \ jump up to j2vec to pass control to the next interrupt
                            \ handler, instead of switching the palette to mode 1.
                            \ This will have the effect of blurring and colouring
                            \ the top screen in a mode 2 palette, making the
                            \ hyperspace rings turn multicoloured when we do a
                            \ hyperspace jump. This effect is triggered by the
                            \ parasite issuing a #DOHFX 1 command in routine LL164
                            \ and is disabled again by a #DOHFX 0 command
    
     LDA #%00011000         \ Set the Video ULA control register (SHEILA &20) to
     STA VIA+&20            \ %00011000, which is the same as switching to mode 1
                            \ (i.e. the top part of the screen) but with no cursor
    
    .VNT3
    
                            \ The following instruction gets modified in-place by
                            \ the #SETVDU19 <offset> command, which changes the
                            \ value of TVT3+1 (i.e. the low byte of the address in
                            \ the LDA instruction). This changes the palette block
                            \ that gets copied to SHEILA &21, so a #SETVDU19 32
                            \ command applies the third palette from TVT3 in this
                            \ loop, for example
    
     LDA TVT3,Y             \ Copy the Y-th palette byte from TVT3 to SHEILA &21
     STA VIA+&21            \ to map logical to actual colours for the bottom part
                            \ of the screen (i.e. the dashboard)
    
     DEY                    \ Decrement the palette byte counter
    
     BNE VNT3               \ Loop back to VNT3 until we have copied all the
                            \ palette bytes
    
    \LDA svn                \ These instructions are commented out in the original
    \BMI jvecOS             \ source
    
    .j2vec
    
     PHX                    \ Call SOINT to send the current sound data to the
     JSR SOINT              \ 76489 sound chip, stashing X on the stack so it gets
     PLX                    \ preserved across the call
    
     PLA                    \ Restore A from the stack
    
     PLY                    \ Restore Y from the stack
    
     RTI                    \ Return from interrupts, so this interrupt is not
                            \ passed on to the next interrupt handler, but instead
                            \ the interrupt terminates here
    
    
    
    
           Name: VSCAN                                                   [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Defines the split position in the split-screen mode
    
    
        Context: See this variable [on its own page](../main/variable/vscan.md)
     References: This variable is used as follows:
                 * [IRQ1](../main/subroutine/irq1.md) uses VSCAN
                 * [SETINTS](../main/subroutine/setints.md) uses VSCAN
    
    
    
    
    
    
    .VSCAN
    
     EQUB 57                \ Defines the split position in the split-screen mode
    
     EQUB 30                \ The line scan counter in DL gets reset to this value
                            \ at each vertical sync, before decrementing with each
                            \ line scan
    
    
    
           Name: DOVDU19                                                 [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Change the mode 1 palette
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dovdu19.md)
     Variations: See [code variations](../related/compare/main/subroutine/setvdu19-dovdu19.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HALL](../main/subroutine/hall.md) calls DOVDU19
                 * [LOOK1](../main/subroutine/look1.md) calls DOVDU19
                 * [TITLE](../main/subroutine/title.md) calls DOVDU19
                 * [TRADEMODE](../main/subroutine/trademode.md) calls DOVDU19
                 * [TT22](../main/subroutine/tt22.md) calls DOVDU19
                 * [TT23](../main/subroutine/tt23.md) calls DOVDU19
    
    
    
    
    
    * * *
    
    
     This routine updates the VNT3+1 location in the IRQ1 handler to change the
     palette that's applied to the top part of the screen (the four-colour mode 1
     part). The parameter is the offset within the TVT3 palette block of the
     desired palette.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The offset within the TVT3 table of palettes:
    
                              * 0 = Yellow, red, cyan palette (space view)
    
                              * 16 = Yellow, red, white palette (charts)
    
                              * 32 = Yellow, white, cyan palette (title screen)
    
                              * 48 = Yellow, magenta, white palette (trading)
    
    
    
    
    .DOVDU19
    
     STA VNT3+1             \ Store the new colour in VNT3+1, in the IRQ1 routine,
                            \ which modifies which TVT3 palette block gets applied
                            \ to the mode 1 part of the screen
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: setzp                                                   [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Copy the top part of zero page (&0090 to &00FF) into the buffer at
                 &3000
    
    
        Context: See this subroutine [on its own page](../main/subroutine/setzp.md)
     References: This subroutine is called as follows:
                 * [COLD](../main/subroutine/cold.md) calls setzp
    
    
    
    
    
    
    .setzp
    
    IF _COMPACT
    
     JSR NMICLAIM           \ Claim the NMI workspace (&00A0 to &00A7) from the MOS
                            \ so the game can use it
    
    ENDIF
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDX #&90               \ We want to save zero page from &0090 and up, so set an
                            \ index in X, starting from &90
    
    .sz1
    
     LDA ZP,X               \ Copy the X-th byte of ZP to the X-th byte of &3000
     STA &3000,X
    
     INX                    \ Increment the loop counter
    
     BNE sz1                \ Loop back until we have copied the last byte of zero
                            \ page
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: NMIRELEASE                                              [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Release the NMI workspace (&00A0 to &00A7) so the MOS can use it
                 and store the top part of zero page in the buffer at &3000
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nmirelease.md)
     References: This subroutine is called as follows:
                 * [CATS](../main/subroutine/cats.md) calls NMIRELEASE
                 * [DELT](../main/subroutine/delt.md) calls NMIRELEASE
                 * [GTDIR](../main/subroutine/gtdir.md) calls NMIRELEASE
                 * [rfile](../main/subroutine/rfile.md) calls NMIRELEASE
                 * [wfile](../main/subroutine/wfile.md) calls NMIRELEASE
    
    
    
    
    
    
    IF _COMPACT
    
    .NMIRELEASE
    
     JSR getzp+3            \ Call getzp+3 to restore the top part of zero page
                            \ from the buffer at &3000, but without first claiming
                            \ the NMI workspace (as it's already been claimed by
                            \ this point)
    
     LDA #143               \ Call OSBYTE 143 to issue a paged ROM service call of
     LDX #&B                \ type &B with Y set to the previous NMI owner's ID.
     LDY NMI                \ This releases the NMI workspace back to the original
     JMP OSBYTE             \ owner, from whom we claimed the workspace in the
                            \ NMICLAIM routine, and returns from the subroutine
                            \ using a tail call
    
    ENDIF
    
    
    
    
           Name: getzp                                                   [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Swap zero page (&0090 to &00EF) with the buffer at &3000
    
    
        Context: See this subroutine [on its own page](../main/subroutine/getzp.md)
     References: This subroutine is called as follows:
                 * [CATS](../main/subroutine/cats.md) calls getzp
                 * [DELT](../main/subroutine/delt.md) calls getzp
                 * [GTDIR](../main/subroutine/gtdir.md) calls getzp
                 * [NEWBRK](../main/subroutine/newbrk.md) calls getzp
                 * [rfile](../main/subroutine/rfile.md) calls getzp
                 * [wfile](../main/subroutine/wfile.md) calls getzp
                 * [NMIRELEASE](../main/subroutine/nmirelease.md) calls via getzp+3
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       getzp+3              Restore the top part of zero page, but without first
                            claiming the NMI workspace (Master Compact variant only)
    
    
    
    
    .getzp
    
    IF _COMPACT
    
     JSR NMICLAIM           \ Claim the NMI workspace (&00A0 to &00A7) from the MOS
                            \ so the game can use it
    
    ENDIF
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDX #&90               \ We want to swap zero page from &0090 and up, so set an
                            \ index in X, starting from &90
    
    .sz2
    
     LDA ZP,X               \ Swap the X-th byte of ZP with the X-th byte of &3000
     LDY &3000,X
     STY ZP,X
     STA &3000,X
    
     INX                    \ Increment the loop counter
    
     CPX #&F0               \ Loop back until we have swapped up to location &00EF
     BNE sz2
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDA #6                 \ Set bits 0-3 of the ROM Select latch at SHEILA &30 to
     STA VIA+&30            \ 6, to switch sideways ROM bank 6 into &8000-&BFFF in
                            \ main memory (we already confirmed that this bank
                            \ contains RAM rather than ROM in the loader)
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: NMICLAIM                                                [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Claim the NMI workspace (&00A0 to &00A7) back from the MOS so the
                 game can use it once again
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nmiclaim.md)
     References: This subroutine is called as follows:
                 * [getzp](../main/subroutine/getzp.md) calls NMICLAIM
                 * [setzp](../main/subroutine/setzp.md) calls NMICLAIM
    
    
    
    
    
    
    IF _COMPACT
    
    .NMICLAIM
    
     LDA #143               \ Call OSBYTE 143 to issue a paged ROM service call of
     LDX #&C                \ type &C with argument &FF, which is the "NMI claim"
     LDY #&FF               \ service call that asks the current user of the NMI
     JSR OSBYTE             \ space to clear it out
    
     STY NMI                \ Save the returned value of Y in NMI, as it contains
                            \ the filing system ID of the previous claimant of the
                            \ NMI, which we need to restore once we have finished
                            \ using the NMI workspace
    
     RTS                    \ Return from the subroutine
    
    ENDIF
    
    
    
    
           Name: ylookup                                                 [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Lookup table for converting pixel y-coordinate to page number of
                 screen address
    
    
        Context: See this variable [on its own page](../main/variable/ylookup.md)
     References: This variable is used as follows:
                 * [CPIXK](../main/subroutine/cpixk.md) uses ylookup
                 * [HANGER](../main/subroutine/hanger.md) uses ylookup
                 * [HLOIN](../main/subroutine/hloin.md) uses ylookup
                 * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md) uses ylookup
                 * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md) uses ylookup
                 * [PIXEL](../main/subroutine/pixel.md) uses ylookup
    
    
    
    
    
    * * *
    
    
     Elite's screen mode is based on mode 1, so it allocates two pages of screen
     memory to each character row (where a character row is 8 pixels high). This
     table enables us to convert a pixel y-coordinate in the range 0-247 into the
     page number for the start of the character row containing that coordinate.
    
     Screen memory is from &4000 to &7DFF, so the lookup works like this:
    
       Y =   0 to  7,  lookup value = &40 (so row 1 is from &4000 to &41FF)
       Y =   8 to 15,  lookup value = &42 (so row 2 is from &4200 to &43FF)
       Y =  16 to 23,  lookup value = &44 (so row 3 is from &4400 to &45FF)
       Y =  24 to 31,  lookup value = &46 (so row 4 is from &4600 to &47FF)
    
       ...
    
       Y = 232 to 239, lookup value = &7A (so row 31 is from &7A00 to &7BFF)
       Y = 240 to 247, lookup value = &7C (so row 32 is from &7C00 to &7DFF)
    
     There is also a lookup value for y-coordinates from 248 to 255, but that's off
     the end of the screen, as the special Elite screen mode only has 31 character
     rows.
    
    
    
    
    .ylookup
    
     FOR I%, 0, 255
    
      EQUB &40 + ((I% DIV 8) * 2)
    
     NEXT
    
    
    
    
           Name: SHIFT                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard to see if SHIFT is currently pressed
    
    
        Context: See this subroutine [on its own page](../main/subroutine/shift.md)
     References: This subroutine is called as follows:
                 * [MT26](../main/subroutine/mt26.md) calls SHIFT
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X = %10000000 if SHIFT is being pressed
    
                            X = 0 if SHIFT is not being pressed
    
       A                    Contains the same as X
    
    
    
    
    IF _COMPACT
    
    .SHIFT
    
     LDA #0                 \ Set A to the internal key number for SHIFT and fall
                            \ through to DKS4mc to scan the keyboard
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &49, or BIT &49A9, which does nothing apart
                            \ from affect the flags
    
    ENDIF
    
    
    
    
           Name: RETURN                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard to see if RETURN is currently pressed
    
    
        Context: See this subroutine [on its own page](../main/subroutine/return.md)
     References: This subroutine is called as follows:
                 * [CHPR](../main/subroutine/chpr.md) calls RETURN
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X = %11001001 if RETURN is being pressed
    
                            X = %01001001 if RETURN is not being pressed
    
       A                    Contains the same as X
    
    
    
    
    IF _COMPACT
    
    .RETURN
    
     LDA #&49               \ Set A to the internal key number for RETURN and fall
                            \ through to DKS4mc to scan the keyboard
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &01, or BIT &01A9, which does nothing apart
                            \ from affect the flags
    
    ENDIF
    
    
    
    
           Name: CTRLmc                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the Master Compact keyboard to see if CTRL is currently
                 pressed
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ctrlmc.md)
     References: This subroutine is called as follows:
                 * [hyp](../main/subroutine/hyp.md) calls CTRLmc
                 * [TT18](../main/subroutine/tt18.md) calls CTRLmc
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X = %10000001 (i.e. 129 or -127) if CTRL is being
                            pressed
    
                            X = 1 if CTRL is not being pressed
    
       A                    Contains the same as X
    
    
    
    
    IF _COMPACT
    
    .CTRLmc
    
     LDA #1                 \ Set A to the internal key number for CTRL and fall
                            \ through to DKS4mc to scan the keyboard
    
    ENDIF
    
    
    
    
           Name: DKS4mc                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the Master Compact keyboard to see if a specific key is being
                 pressed
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dks4mc.md)
     References: This subroutine is called as follows:
                 * [TT17](../main/subroutine/tt17.md) calls DKS4mc
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The internal number of the key to check (see page 142 of
                            the "Advanced User Guide for the BBC Micro" by Bray,
                            Dickens and Holmes for a list of internal key numbers)
    
    
    
    * * *
    
    
     Returns:
    
       A                    If the key in A is being pressed, A contains the
                            original argument A, but with bit 7 set (i.e. A + 128).
                            If the key in A is not being pressed, the value in A is
                            unchanged
    
       X                    Contains the same as A
    
    
    
    
    IF _COMPACT
    
    .DKS4mc
    
     LDX #3                 \ Set X to 3, so it's ready to send to SHEILA once
                            \ interrupts have been disabled
    
     SEI                    \ Disable interrupts so we can scan the keyboard
                            \ without being hijacked
    
     STX VIA+&40            \ Set 6522 System VIA output register ORB (SHEILA &40)
                            \ to %00000011 to stop auto scan of keyboard
    
     LDX #%01111111         \ Set 6522 System VIA data direction register DDRA
     STX VIA+&43            \ (SHEILA &43) to %01111111. This sets the A registers
                            \ (IRA and ORA) so that:
                            \
                            \   * Bits 0-6 of ORA will be sent to the keyboard
                            \
                            \   * Bit 7 of IRA will be read from the keyboard
    
     STA VIA+&4F            \ Set 6522 System VIA output register ORA (SHEILA &4F)
                            \ to A, the key we want to scan for; bits 0-6 will be
                            \ sent to the keyboard, of which bits 0-3 determine the
                            \ keyboard column, and bits 4-6 the keyboard row
    
     LDX VIA+&4F            \ Read 6522 System VIA output register IRA (SHEILA &4F)
                            \ into X; bit 7 is the only bit that will have changed.
                            \ If the key is pressed, then bit 7 will be set,
                            \ otherwise it will be clear
    
     LDA #%00001011         \ Set 6522 System VIA output register ORB (SHEILA &40)
     STA VIA+&40            \ to %00001011 to restart auto scan of keyboard
    
     CLI                    \ Allow interrupts again
    
     TXA                    \ Transfer X into A
    
    ENDIF
    
    
    
    
           Name: SCAN                                                    [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Display the current ship on the scanner
      Deep dive: [The 3D scanner](https://elite.bbcelite.com/deep_dives/the_3d_scanner.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/scan.md)
     Variations: See [code variations](../related/compare/main/subroutine/scan.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ESCAPE](../main/subroutine/escape.md) calls SCAN
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls SCAN
                 * [MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md) calls SCAN
                 * [MVEIT (Part 9 of 9)](../main/subroutine/mveit_part_9_of_9.md) calls SCAN
                 * [WPSHPS](../main/subroutine/wpshps.md) calls SCAN
    
    
    
    
    
    * * *
    
    
     This is used both to display a ship on the scanner, and to erase it again.
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The ship's data block
    
    
    
    
    .SCR1
    
     RTS                    \ Return from the subroutine
    
    .SCAN
    
     LDA INWK+31            \ Fetch the ship's scanner flag from byte #31
    
     AND #%00010000         \ If bit 4 is clear then the ship should not be shown
     BEQ SCR1               \ on the scanner, so return from the subroutine (as SCR1
                            \ contains an RTS)
    
     LDX TYPE               \ Fetch the ship's type from TYPE into X
    
     BMI SCR1               \ If this is the planet or the sun, then the type will
                            \ have bit 7 set and we don't want to display it on the
                            \ scanner, so return from the subroutine (as SCR1
                            \ contains an RTS)
    
     LDA scacol,X           \ Set A to the scanner colour for this ship type from
                            \ the X-th entry in the scacol table
    
     STA COL                \ Store the scanner colour in COL so it can be used to
                            \ draw this ship in the correct colour
    
     LDA INWK+1             \ If any of x_hi, y_hi and z_hi have a 1 in bit 6 or 7,
     ORA INWK+4             \ then the ship is too far away to be shown on the
     ORA INWK+7             \ scanner, so return from the subroutine (as SCR1
     AND #%11000000         \ contains an RTS)
     BNE SCR1
    
                            \ If we get here, we know x_hi, y_hi and z_hi are all
                            \ 63 (%00111111) or less
    
                            \ Now, we convert the x_hi coordinate of the ship into
                            \ the screen x-coordinate of the dot on the scanner,
                            \ using the following:
                            \
                            \   X1 = 123 + (x_sign x_hi)
    
     LDA INWK+1             \ Set A = x_hi
    
     CLC                    \ Clear the C flag so we can do addition below
    
     LDX INWK+2             \ Set X = x_sign
    
     BPL SC2                \ If x_sign is positive, skip the following
    
     EOR #%11111111         \ x_sign is negative, so flip the bits in A and add 1
     ADC #1                 \ to make it a negative number (bit 7 will now be set
                            \ as we confirmed above that bits 6 and 7 are clear). So
                            \ this gives A the sign of x_sign and gives it a value
                            \ range of -63 (%11000001) to 0
    
     CLC                    \ Clear the C flag so we can do addition below
    
    .SC2
    
     ADC #125               \ Set X1 = 125 + (x_sign x_hi)
     AND #%11111110         \
     STA X1                 \ and if the result is odd, subtract 1 to make it even
    
     TAX                    \ Set X = X1 - 2
     DEX                    \
     DEX                    \ So X contains the x-coordinate that's two pixels to
                            \ left of the top of the stick, so we can draw the dot
                            \ at the end of the stick by simply drawing a dot at
                            \ x-coordinate X at the correct end of the stick
    
                            \ Next, we convert the z_hi coordinate of the ship into
                            \ the y-coordinate of the base of the ship's stick,
                            \ like this:
                            \
                            \   SC = 220 - (z_sign z_hi) / 4
                            \
                            \ though the following code actually does it like this:
                            \
                            \   SC = 255 - (35 + z_hi / 4)
    
     LDA INWK+7             \ Set A = z_hi / 4
     LSR A                  \
     LSR A                  \ So A is in the range 0-15
    
     CLC                    \ Clear the C flag for the addition below
    
     LDY INWK+8             \ Set Y = z_sign
    
     BPL SC3                \ If z_sign is positive, skip the following
    
     EOR #%11111111         \ z_sign is negative, so flip the bits in A and set the
     SEC                    \ C flag. As above, this makes A negative, this time
                            \ with a range of -16 (%11110000) to -1 (%11111111). And
                            \ as we are about to do an ADC, the SEC effectively adds
                            \ another 1 to that value, giving a range of -15 to 0
    
    .SC3
    
     ADC #35                \ Set A = 35 + A to give a number in the range 20 to 50
    
     EOR #%11111111         \ Flip all the bits and store in Y2, so Y2 is in the
     STA Y2                 \ range 205 to 235, with a higher z_hi giving a lower Y2
    
                            \ Now for the stick height, which we calculate using the
                            \ following:
                            \
                            \ A = - (y_sign y_hi) / 2
    
     LDA INWK+4             \ Set A = y_hi / 2
     LSR A
    
     CLC                    \ Clear the C flag
    
     LDY INWK+5             \ Set Y = y_sign
    
     BMI SCD6               \ If y_sign is negative, skip the following, as we
                            \ already have a positive value in A
    
     EOR #%11111111         \ y_sign is positive, so flip the bits in A and set the
     SEC                    \ C flag. This makes A negative, and as we are about to
                            \ do an ADC below, the SEC effectively adds another 1 to
                            \ that value to implement two's complement negation, so
                            \ we don't need to add another 1 here
    
    .SCD6
    
                            \ We now have all the information we need to draw this
                            \ ship on the scanner, namely:
                            \
                            \   X1 = the screen x-coordinate of the ship's dot
                            \
                            \   SC = the screen y-coordinate of the base of the
                            \        stick
                            \
                            \   A = the screen height of the ship's stick, with the
                            \       correct sign for adding to the base of the stick
                            \       to get the dot's y-coordinate
                            \
                            \ First, though, we have to make sure the dot is inside
                            \ the dashboard, by moving it if necessary
    
     ADC Y2                 \ Set A = Y2 + A, so A now contains the y-coordinate of
                            \ the end of the stick, plus the length of the stick, to
                            \ give us the screen y-coordinate of the dot
    
     BPL ld246              \ If the result has bit 0 clear, then the result has
                            \ overflowed and is bigger than 256, so jump to ld246 to
                            \ set A to the maximum allowed value of 246 (this
                            \ instruction isn't required as we test both the maximum
                            \ and minimum below, but it might save a few cycles)
    
     CMP #194               \ If A >= 194, skip the following instruction, as 194 is
     BCS P%+4               \ the minimum allowed value of A
    
     LDA #194               \ A < 194, so set A to 194, the minimum allowed value
                            \ for the y-coordinate of our ship's dot
    
     CMP #247               \ If A < 247, skip the following instruction, as 246 is
     BCC P%+4               \ the maximum allowed value of A
    
    .ld246
    
     LDA #246               \ A >= 247, so set A to 246, the maximum allowed value
                            \ for the y-coordinate of our ship's dot
    
     LDY #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     JSR CPIXK              \ Call CPIXK to draw a single-height dash at the
                            \ y-coordinate in A, to draw the ship dot, and return
                            \ the dash's right pixel byte in R, which we use below
    
     LDA Y1                 \ Fetch the y-coordinate back into A, which was stored
                            \ in Y1 by the call to CPIX2
    
     SEC                    \ Set A = A - Y2 to get the stick length, by reversing
     SBC Y2                 \ the ADC Y2 we did above. This clears the C flag if the
                            \ result is negative (i.e. the stick length is negative)
                            \ and sets it if the result is positive (i.e. the stick
                            \ length is negative)
    
                            \ So now we have the following:
                            \
                            \   X1 = the screen x-coordinate of the ship's dot,
                            \        clipped to fit into the dashboard
                            \
                            \   Y1 = the screen y-coordinate of the ship's dot,
                            \        clipped to fit into the dashboard
                            \
                            \   SC = the screen y-coordinate of the base of the
                            \        stick
                            \
                            \   A = the screen height of the ship's stick, with the
                            \       correct sign for adding to the base of the stick
                            \       to get the dot's y-coordinate
                            \
                            \   C = 0 if A is negative, 1 if A is positive
                            \
                            \ and we can get on with drawing the dot and stick
    
     BEQ VLO5               \ If the stick height is zero, then there is no stick to
                            \ draw, so return from the subroutine (as VLO5 contains
                            \ an RTS)
    
     BCC VLO1               \ If the C flag is clear then the stick height in A is
                            \ negative, so jump down to RTS+1
    
     TAX                    \ Copy the (positive) stick height into X
    
     INX                    \ Increment the (positive) stick height in X
    
     JMP VLO4               \ Jump into the middle of the VLOL1 loop, skipping the
                            \ drawing of first pixel in the stick
    
    .VLOL1
    
     LDA R                  \ The call to CPIX2 above saved the dash's right pixel
                            \ byte in R, so we load this into A (so the stick comes
                            \ out of the right side of the dot)
    
     EOR (SC),Y             \ Draw the bottom row of the double-height dot using the
     STA (SC),Y             \ same byte as the top row, plotted using EOR logic
    
    .VLO4
    
                            \ If we get here then the stick length is positive (so
                            \ the dot is below the ellipse and the stick is above
                            \ the dot, and we need to draw the stick upwards from
                            \ the dot)
    
     DEY                    \ We want to draw the stick upwards, so decrement the
                            \ pixel row in Y
    
     BPL VLO3               \ If Y is still positive then it correctly points at the
                            \ line above, so jump to VLO3 to skip the following
    
     LDA SC+1               \ Subtract 2 from the high byte of the screen address to
     SBC #2                 \ move to the character block above
     STA SC+1
    
     LDY #7                 \ We just decremented Y up through the top of the
                            \ character block, so we need to move it to the last row
                            \ in the character above, so set Y to 7, the number of
                            \ the last row
    
    .VLO3
    
     DEX                    \ Decrement the (positive) stick height in X
    
     BNE VLOL1              \ If we still have more stick to draw, jump up to VLOL1
                            \ to draw the next pixel
    
    .VLO5
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    .VLO1
    
                            \ If we get here then the stick length is negative (so
                            \ the dot is above the ellipse and the stick is below
                            \ the dot, and we need to draw the stick downwards from
                            \ the dot)
    
     LDA Y2                 \ Set A = Y2 - Y1 to get the positive stick height
     SEC
     SBC Y1
    
     TAX                    \ Copy the (positive) stick height into X
    
     INX                    \ Increment the (positive) stick height in X
    
     JMP VLO6               \ Jump into the middle of the VLOL2 loop, skipping the
                            \ drawing of first pixel in the stick
    
    .VLOL2
    
     LDA R                  \ The call to CPIX2 above saved the dash's right pixel
                            \ byte in R, so we load this into A (so the stick comes
                            \ out of the right side of the dot)
    
     EOR (SC),Y             \ Draw the bottom row of the double-height dot using the
     STA (SC),Y             \ same byte as the top row, plotted using EOR logic
    
    .VLO6
    
     INY                    \ We want to draw the stick itself, heading downwards,
                            \ so increment the pixel row in Y
    
     CPY #8                 \ If the row number in Y is less than 8, then it
     BNE VLO7               \ correctly points at the next line down, so jump to
                            \ VLO7 to skip the following
    
     LDA SC+1               \ We just incremented Y down through the bottom of the
     ADC #1                 \ character block, so increment the high byte of the
     STA SC+1               \ screen address to move to the character block above
    
     LDY #0                 \ We need to move to the first row in the character
                            \ below, so set Y to 0, the number of the first row
    
    .VLO7
    
     DEX                    \ Decrement the (positive) stick height in X
    
     BNE VLOL2              \ If we still have more stick to draw, jump up to VLOL2
                            \ to draw the next pixel
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LOIN                                                    [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a one-segment line
    
    
        Context: See this subroutine [on its own page](../main/subroutine/loin.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll30-loin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BLINE](../main/subroutine/bline.md) calls LOIN
                 * [BOMBOFF](../main/subroutine/bomboff.md) calls LOIN
                 * [DVLOIN](../main/subroutine/dvloin.md) calls LOIN
                 * [LASLI](../main/subroutine/lasli.md) calls LOIN
                 * [LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md) calls LOIN
                 * [LSPUT](../main/subroutine/lsput.md) calls LOIN
                 * [TT15](../main/subroutine/tt15.md) calls LOIN
                 * [WPLS2](../main/subroutine/wpls2.md) calls LOIN
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       X1                   The screen x-coordinate of the start of the line
    
       Y1                   The screen y-coordinate of the start of the line
    
       X2                   The screen x-coordinate of the end of the line
    
       Y2                   The screen y-coordinate of the end of the line
    
    
    
    
    .LOIN
    
     STY YSAV               \ Store Y in YSAV so we can retrieve it below
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     JSR LOINQ              \ Draw a line from (X1, Y1) to (X2, Y2)
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY YSAV               \ Retrieve the value of Y we stored above
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TWOS                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made single-pixel character row bytes for mode 1
    
    
        Context: See this variable [on its own page](../main/variable/twos.md)
     Variations: See [code variations](../related/compare/loader/variable/twos.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md) uses TWOS
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting one-pixel points in mode 1 (the top part of the
     split screen).
    
    
    
    
    .TWOS
    
     EQUB %10001000
     EQUB %01000100
     EQUB %00100010
     EQUB %00010001
    
    
    
    
           Name: TWOS2                                                   [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made double-pixel character row bytes for mode 1
    
    
        Context: See this variable [on its own page](../main/variable/twos2.md)
     References: This variable is used as follows:
                 * [PIXEL](../main/subroutine/pixel.md) uses TWOS2
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting two-pixel dashes in mode 1 (the top part of the
     split screen).
    
    
    
    
    .TWOS2
    
     EQUB %11001100
     EQUB %01100110
     EQUB %00110011
     EQUB %00110011
    
    
    
    
           Name: CTWOS                                                   [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made single-pixel character row bytes for mode 2
    
    
        Context: See this variable [on its own page](../main/variable/ctwos.md)
     References: This variable is used as follows:
                 * [CPIXK](../main/subroutine/cpixk.md) uses CTWOS
                 * [DIL2](../main/subroutine/dil2.md) uses CTWOS
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting one-pixel points in mode 2 (the bottom part of
     the split screen).
    
     In mode 2, each character row is one byte, which is two pixels. Rows 0 and 1
     of the table contain a character row byte with just the left pixel plotted,
     while rows 2 and 3 contain a character row byte with just the right pixel
     plotted.
    
     In other words, looking up row X will return a character row byte with pixel
     X/2 plotted (if the pixels are numbered 0 and 1).
    
     There are two extra rows to support the use of CTWOS+2,X indexing in the CPIX2
     routine. The extra rows are repeats of the first two rows, and save us from
     having to work out whether CTWOS+2+X needs to be wrapped around when drawing a
     two-pixel dash that crosses from one character block into another. See CPIX2
     for more details.
    
    
    
    
    .CTWOS
    
     EQUB %10101010
     EQUB %10101010
     EQUB %01010101
     EQUB %01010101
     EQUB %10101010
     EQUB %10101010
    
    
    
    
           Name: LOINQ (Part 1 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a line: Calculate the line gradient in the form of deltas
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/loinq_part_1_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/loin_part_1_of_7-loinq_part_1_of_7.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOIN](../main/subroutine/loin.md) calls LOINQ
                 * [TTX66](../main/subroutine/ttx66.md) calls LOINQ
                 * [LOIN](../main/subroutine/loin.md) calls via LOINQ
                 * [TTX66](../main/subroutine/ttx66.md) calls via LOINQ
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     This stage calculates the line deltas.
    
    
    
    * * *
    
    
     Arguments:
    
       X1                   The screen x-coordinate of the start of the line
    
       Y1                   The screen y-coordinate of the start of the line
    
       X2                   The screen x-coordinate of the end of the line
    
       Y2                   The screen y-coordinate of the end of the line
    
    
    
    * * *
    
    
     Other entry points:
    
       LOINQ                Draw a one-segment line from (X1, Y1) to (X2, Y2)
    
    
    
    
    .HLOIN22
    
     JMP HLOIN3             \ This instruction doesn't appear to be used anywhere
    
                            \ In the BBC Micro cassette and disc versions of Elite,
                            \ LL30 and LOIN are synonyms for the same routine,
                            \ presumably because the two developers each had their
                            \ own line routines to start with, and then chose one of
                            \ them for the final game
                            \
                            \ In the BBC Master version, there are two different
                            \ routines: LOINQ draws a one-segment line, while LOIN
                            \ draws individual segments of multi-segment lines (the
                            \ distinction being that we switch to screen memory at
                            \ the start of LOINQ and back out again after drawing
                            \ the line, while LOIN just draws the line)
    
    .LOINQ
    
     LDA #128               \ Set S = 128, which is the starting point for the
     STA S                  \ slope error (representing half a pixel)
    
     ASL A                  \ Set SWAP = 0, as %10000000 << 1 = 0
     STA SWAP
    
     LDA X2                 \ Set A = X2 - X1
     SBC X1                 \       = delta_x
                            \
                            \ This subtraction works as the ASL A above sets the C
                            \ flag
    
     BCS LI1                \ If X2 > X1 then A is already positive and we can skip
                            \ the next three instructions
    
     EOR #%11111111         \ Negate the result in A by flipping all the bits and
     ADC #1                 \ adding 1, i.e. using two's complement to make it
                            \ positive
    
     SEC                    \ Set the C flag, ready for the subtraction below
    
    .LI1
    
     STA P                  \ Store A in P, so P = |X2 - X1|, or |delta_x|
    
     LDA Y2                 \ Set A = Y2 - Y1
     SBC Y1                 \       = delta_y
                            \
                            \ This subtraction works as we either set the C flag
                            \ above, or we skipped that SEC instruction with a BCS
    
    \BEQ HLOIN22            \ This instruction is commented out in the original
                            \ source
    
     BCS LI2                \ If Y2 > Y1 then A is already positive and we can skip
                            \ the next two instructions
    
     EOR #%11111111         \ Negate the result in A by flipping all the bits and
     ADC #1                 \ adding 1, i.e. using two's complement to make it
                            \ positive
    
    .LI2
    
     STA Q                  \ Store A in Q, so Q = |Y2 - Y1|, or |delta_y|
    
     CMP P                  \ If Q < P, jump to STPX to step along the x-axis, as
     BCC STPX               \ the line is closer to being horizontal than vertical
    
     JMP STPY               \ Otherwise Q >= P so jump to STPY to step along the
                            \ y-axis, as the line is closer to being vertical than
                            \ horizontal
    
    
    
    
           Name: LOINQ (Part 2 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a line: Line has a shallow gradient, step right along x-axis
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/loinq_part_2_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/loin_part_2_of_7-loinq_part_2_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * |delta_y| < |delta_x|
    
       * The line is closer to being horizontal than vertical
    
       * We are going to step right along the x-axis
    
       * We potentially swap coordinates to make sure X1 < X2
    
    
    
    
    .STPX
    
     LDX X1                 \ Set X = X1
    
     CPX X2                 \ If X1 < X2, jump down to LI3, as the coordinates are
     BCC LI3                \ already in the order that we want
    
     DEC SWAP               \ Otherwise decrement SWAP from 0 to &FF, to denote that
                            \ we are swapping the coordinates around
    
     LDA X2                 \ Swap the values of X1 and X2
     STA X1
     STX X2
    
     TAX                    \ Set X = X1
    
     LDA Y2                 \ Swap the values of Y1 and Y2
     LDY Y1
     STA Y1
     STY Y2
    
    .LI3
    
                            \ By this point we know the line is horizontal-ish and
                            \ X1 < X2, so we're going from left to right as we go
                            \ from X1 to X2
    
     LDY Y1                 \ Look up the page number of the character row that
     LDA ylookup,Y          \ contains the pixel with the y-coordinate in Y1, and
     STA SC+1               \ store it in SC+1, so the high byte of SC is set
                            \ correctly for drawing our line
    
     LDA Y1                 \ Set Y = Y1 mod 8, which is the pixel row within the
     AND #7                 \ character block at which we want to draw the start of
     TAY                    \ our line (as each character block has 8 rows)
    
     TXA                    \ Set A = 2 * bits 2-6 of X1
     AND #%11111100         \
     ASL A                  \ and shift bit 7 of X1 into the C flag
    
     STA SC                 \ Store this value in SC, so SC(1 0) now contains the
                            \ screen address of the far left end (x-coordinate = 0)
                            \ of the horizontal pixel row that we want to draw the
                            \ start of our line on
    
     BCC P%+4               \ If bit 7 of X1 was set, so X1 > 127, increment the
     INC SC+1               \ high byte of SC(1 0) to point to the second page on
                            \ this screen row, as this page contains the right half
                            \ of the row
    
     TXA                    \ Set R = X1 mod 4, which is the horizontal pixel number
     AND #3                 \ within the character block where the line starts (as
     STA R                  \ each pixel line in the character block is 4 pixels
                            \ wide)
    
                            \ The following section calculates:
                            \
                            \   Q = Q / P
                            \     = |delta_y| / |delta_x|
                            \
                            \ using the log tables at logL and log to calculate:
                            \
                            \   A = log(Q) - log(P)
                            \     = log(|delta_y|) - log(|delta_x|)
                            \
                            \ by first subtracting the low bytes of the logarithms
                            \ from the table at LogL, and then subtracting the high
                            \ bytes from the table at log, before applying the
                            \ antilog to get the result of the division and putting
                            \ it in Q
    
     LDX Q                  \ Set X = |delta_y|
    
     BEQ LIlog7             \ If |delta_y| = 0, jump to LIlog7 to return 0 as the
                            \ result of the division
    
     LDA logL,X             \ Set A = log(Q) - log(P)
     LDX P                  \       = log(|delta_y|) - log(|delta_x|)
     SEC                    \
     SBC logL,X             \ by first subtracting the low bytes of log(Q) - log(P)
    
     LDX Q                  \ And then subtracting the high bytes of log(Q) - log(P)
     LDA log,X              \ so now A contains the high byte of log(Q) - log(P)
     LDX P
     SBC log,X
    
     BCS LIlog5             \ If the subtraction fitted into one byte and didn't
                            \ underflow, then log(Q) - log(P) < 256, so we jump to
                            \ LIlog5 to return a result of 255
    
     TAX                    \ Otherwise we set A to the A-th entry from the antilog
     LDA alogh,X            \ table so the result of the division is now in A
    
     JMP LIlog6             \ Jump to LIlog6 to return the result
    
    .LIlog5
    
     LDA #255               \ The division is very close to 1, so set A to the
     BNE LIlog6             \ closest possible answer to 256, i.e. 255, and jump to
                            \ LIlog6 to return the result (this BNE is effectively a
                            \ JMP as A is never zero)
    
    .LIlog7
    
     LDA #0                 \ The numerator in the division is 0, so set A to 0
    
    .LIlog6
    
     STA Q                  \ Store the result of the division in Q, so we have:
                            \
                            \   Q = |delta_y| / |delta_x|
    
     LDX P                  \ Set X = P
                            \       = |delta_x|
    
     BEQ LIEXS              \ If |delta_x| = 0, return from the subroutine, as LIEXS
                            \ contains a BEQ LIEX instruction, and LIEX contains an
                            \ RTS
    
     INX                    \ Set X = P + 1
                            \       = |delta_x| + 1
                            \
                            \ We add 1 so we can skip the first pixel plot if the
                            \ line is being drawn with swapped coordinates
    
     LDA Y2                 \ If Y2 < Y1 then skip the following instruction
     CMP Y1
     BCC P%+5
    
     JMP DOWN               \ Y2 >= Y1, so jump to DOWN, as we need to draw the line
                            \ to the right and down
    
    
    
    
           Name: LOINQ (Part 3 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a shallow line going right and up or left and down
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/loinq_part_3_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/loin_part_3_of_7-loinq_part_3_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * The line is going right and up (no swap) or left and down (swap)
    
       * X1 < X2 and Y1 > Y2
    
       * Draw from (X1, Y1) at bottom left to (X2, Y2) at top right, omitting the
         first pixel
    
     This routine looks complex, but that's because the loop that's used in the
     BBC Micro cassette and disc versions has been unrolled to speed it up. The
     algorithm is unchanged, it's just a lot longer.
    
    
    
    
     LDA #%10001000         \ Modify the value in the LDA instruction at LI100 below
     AND COL                \ to contain a pixel mask for the first pixel in the
     STA LI100+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%01000100         \ Modify the value in the LDA instruction at LI110 below
     AND COL                \ to contain a pixel mask for the second pixel in the
     STA LI110+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%00100010         \ Modify the value in the LDA instruction at LI120 below
     AND COL                \ to contain a pixel mask for the third pixel in the
     STA LI120+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%00010001         \ Modify the value in the LDA instruction at LI130 below
     AND COL                \ to contain a pixel mask for the fourth pixel in the
     STA LI130+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
                            \ We now work our way along the line from left to right,
                            \ using X as a decreasing counter, and at each count we
                            \ plot a single pixel using the pixel mask in R
    
     LDA SWAP               \ If SWAP = 0 then we didn't swap the coordinates above,
     BEQ LI190              \ so jump down to LI190 to plot the first pixel
    
                            \ If we get here then we want to omit the first pixel
    
     LDA R                  \ Fetch the pixel byte from R, which we set in part 2 to
                            \ the horizontal pixel number within the character block
                            \ where the line starts (so it's 0, 1, 2 or 3)
    
     BEQ LI100+6            \ If R = 0, jump to LI100+6 to start plotting from the
                            \ second pixel in this byte (LI100+6 points to the DEX
                            \ instruction after the EOR/STA instructions, so the
                            \ pixel doesn't get plotted but we join at the right
                            \ point to decrement X correctly to plot the next three)
    
     CMP #2                 \ If R < 2 (i.e. R = 1), jump to LI110+6 to skip the
     BCC LI110+6            \ first two pixels but plot the next two
    
     CLC                    \ Clear the C flag so it doesn't affect the additions
                            \ below
    
     BEQ LI120+6            \ If R = 2, jump to LI120+6 to skip the first three
                            \ pixels but plot the last one
    
     BNE LI130+6            \ If we get here then R must be 3, so jump to LI130+6 to
                            \ skip plotting any of the pixels, but making sure we
                            \ join the routine just after the plotting instructions
                            \ (this BNE is effectively a JMP as we just passed
                            \ through a BEQ)
    
    .LI190
    
     DEX                    \ Decrement the counter in X because we're about to plot
                            \ the first pixel
    
     LDA R                  \ Fetch the pixel byte from R, which we set in part 2 to
                            \ the horizontal pixel number within the character block
                            \ where the line starts (so it's 0, 1, 2 or 3)
    
     BEQ LI100              \ If R = 0, jump to LI100 to start plotting from the
                            \ first pixel in this byte
    
     CMP #2                 \ If R < 2 (i.e. R = 1), jump to LI110 to start plotting
     BCC LI110              \ from the second pixel in this byte
    
     CLC                    \ Clear the C flag so it doesn't affect the additions
                            \ below
    
     BEQ LI120              \ If R = 2, jump to LI120 to start plotting from the
                            \ third pixel in this byte
    
     JMP LI130              \ If we get here then R must be 3, so jump to LI130 to
                            \ start plotting from the fourth pixel in this byte
    
    .LI100
    
     LDA #%10001000         \ Set a mask in A to the first pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
    .LIEXS
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI110              \ If the addition didn't overflow, jump to LI110
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     DEY                    \ decrement Y to move to the pixel line above
    
     BMI LI101              \ If Y is negative we need to move up into the character
                            \ block above, so jump to LI101 to decrement the screen
                            \ address accordingly (jumping back to LI110 afterwards)
    
    .LI110
    
     LDA #%01000100         \ Set a mask in A to the second pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI120              \ If the addition didn't overflow, jump to LI120
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     DEY                    \ decrement Y to move to the pixel line above
    
     BMI LI111              \ If Y is negative we need to move up into the character
                            \ block above, so jump to LI111 to decrement the screen
                            \ address accordingly (jumping back to LI120 afterwards)
    
    .LI120
    
     LDA #%00100010         \ Set a mask in A to the third pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI130              \ If the addition didn't overflow, jump to LI130
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     DEY                    \ decrement Y to move to the pixel line above
    
     BMI LI121              \ If Y is negative we need to move up into the character
                            \ block above, so jump to LI121 to decrement the screen
                            \ address accordingly (jumping back to LI130 afterwards)
    
    .LI130
    
     LDA #%00010001         \ Set a mask in A to the fourth pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI140              \ If the addition didn't overflow, jump to LI140
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     DEY                    \ decrement Y to move to the pixel line above
    
     BMI LI131              \ If Y is negative we need to move up into the character
                            \ block above, so jump to LI131 to decrement the screen
                            \ address accordingly (jumping back to LI140 afterwards)
    
    .LI140
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #8                 \ character along to the right
     STA SC
    
     BCC LI100              \ If the addition didn't overflow, jump back to LI100
                            \ to plot the next pixel
    
     INC SC+1               \ Otherwise the low byte of SC(1 0) just overflowed, so
                            \ increment the high byte SC+1 as we just crossed over
                            \ into the right half of the screen
    
     CLC                    \ Clear the C flag to avoid breaking any arithmetic
    
     BCC LI100              \ Jump back to LI100 to plot the next pixel
    
    .LI101
    
     DEC SC+1               \ If we get here then we need to move up into the
     DEC SC+1               \ character block above, so we decrement the high byte
     LDY #7                 \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the last line in
                            \ that character block
    
     BPL LI110              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI111
    
     DEC SC+1               \ If we get here then we need to move up into the
     DEC SC+1               \ character block above, so we decrement the high byte
     LDY #7                 \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the last line in
                            \ that character block
    
     BPL LI120              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI121
    
     DEC SC+1               \ If we get here then we need to move up into the
     DEC SC+1               \ character block above, so we decrement the high byte
     LDY #7                 \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the last line in
                            \ that character block
    
     BPL LI130              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI131
    
     DEC SC+1               \ If we get here then we need to move up into the
     DEC SC+1               \ character block above, so we decrement the high byte
     LDY #7                 \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the last line in
                            \ that character block
    
     BPL LI140              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LIEX
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LOINQ (Part 4 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a shallow line going right and down or left and up
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/loinq_part_4_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/loin_part_4_of_7-loinq_part_4_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * The line is going right and down (no swap) or left and up (swap)
    
       * X1 < X2 and Y1 <= Y2
    
       * Draw from (X1, Y1) at top left to (X2, Y2) at bottom right, omitting the
         first pixel
    
     This routine looks complex, but that's because the loop that's used in the
     BBC Micro cassette and disc versions has been unrolled to speed it up. The
     algorithm is unchanged, it's just a lot longer.
    
    
    
    
    .DOWN
    
     LDA #%10001000         \ Modify the value in the LDA instruction at LI200 below
     AND COL                \ to contain a pixel mask for the first pixel in the
     STA LI200+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%01000100         \ Modify the value in the LDA instruction at LI210 below
     AND COL                \ to contain a pixel mask for the second pixel in the
     STA LI210+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%00100010         \ Modify the value in the LDA instruction at LI220 below
     AND COL                \ to contain a pixel mask for the third pixel in the
     STA LI220+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%00010001         \ Modify the value in the LDA instruction at LI230 below
     AND COL                \ to contain a pixel mask for the fourth pixel in the
     STA LI230+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA SC                 \ Set SC(1 0) = SC(1 0) - 248
     SBC #248
     STA SC
     LDA SC+1
     SBC #0
     STA SC+1
    
     TYA                    \ Set bits 3-7 of Y, which contains the pixel row within
     EOR #%11111000         \ the character, and is therefore in the range 0-7, so
     TAY                    \ this does Y = 248 + Y
                            \
                            \ We therefore have the following:
                            \
                            \   SC(1 0) + Y = SC(1 0) - 248 + 248 + Y
                            \               = SC(1 0) + Y
                            \
                            \ so the screen location we poke hasn't changed, but Y
                            \ is now a larger number and SC is smaller. This means
                            \ we can increment Y to move down a line, as per usual,
                            \ but we can test for when it reaches the bottom of the
                            \ character block with a simple BEQ rather than checking
                            \ whether it's reached 8, so this appears to be a code
                            \ optimisation
                            \
                            \ If it helps, you can think of Y as being a negative
                            \ number that we are incrementing towards zero as we
                            \ move along the line - we just need to alter the value
                            \ of SC so that SC(1 0) + Y points to the right address
    
                            \ We now work our way along the line from left to right,
                            \ using X as a decreasing counter, and at each count we
                            \ plot a single pixel using the pixel mask in R
    
     LDA SWAP               \ If SWAP = 0 then we didn't swap the coordinates above,
     BEQ LI191              \ so jump down to LI191 to plot the first pixel
    
                            \ If we get here then we want to omit the first pixel
    
     LDA R                  \ Fetch the pixel byte from R, which we set in part 2 to
                            \ the horizontal pixel number within the character block
                            \ where the line starts (so it's 0, 1, 2 or 3)
    
     BEQ LI200+6            \ If R = 0, jump to LI200+6 to start plotting from the
                            \ second pixel in this byte (LI200+6 points to the DEX
                            \ instruction after the EOR/STA instructions, so the
                            \ pixel doesn't get plotted but we join at the right
                            \ point to decrement X correctly to plot the next three)
    
     CMP #2                 \ If R < 2 (i.e. R = 1), jump to LI210+6 to skip the
     BCC LI210+6            \ first two pixels but plot the next two
    
     CLC                    \ Clear the C flag so it doesn't affect the additions
                            \ below
    
     BEQ LI220+6            \ If R = 2, jump to LI220+6 to skip the first three
                            \ pixels but plot the last one
    
     BNE LI230+6            \ If we get here then R must be 3, so jump to LI230+6 to
                            \ skip plotting any of the pixels, but making sure we
                            \ join the routine just after the plotting instructions
                            \ (this BNE is effectively a JMP as we just passed
                            \ through a BEQ)
    
    .LI191
    
     DEX                    \ Decrement the counter in X because we're about to plot
                            \ the first pixel
    
     LDA R                  \ Fetch the pixel byte from R, which we set in part 2 to
                            \ the horizontal pixel number within the character block
                            \ where the line starts (so it's 0, 1, 2 or 3)
    
     BEQ LI200              \ If R = 0, jump to LI200 to start plotting from the
                            \ first pixel in this byte
    
     CMP #2                 \ If R < 2 (i.e. R = 1), jump to LI210 to start plotting
     BCC LI210              \ from the second pixel in this byte
    
     CLC                    \ Clear the C flag so it doesn't affect the additions
                            \ below
    
     BEQ LI220              \ If R = 2, jump to LI220 to start plotting from the
                            \ third pixel in this byte
    
     BNE LI230              \ If we get here then R must be 3, so jump to LI130 to
                            \ start plotting from the fourth pixel in this byte
                            \ (this BNE is effectively a JMP as we just passed
                            \ through a BEQ)
    
    .LI200
    
     LDA #%10001000         \ Set a mask in A to the first pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI210              \ If the addition didn't overflow, jump to LI210
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     INY                    \ increment Y to move to the pixel line below
    
     BEQ LI201              \ If Y is zero we need to move down into the character
                            \ block below, so jump to LI201 to increment the screen
                            \ address accordingly (jumping back to LI210 afterwards)
    
    .LI210
    
     LDA #%01000100         \ Set a mask in A to the second pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI220              \ If the addition didn't overflow, jump to LI220
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     INY                    \ increment Y to move to the pixel line below
    
     BEQ LI211              \ If Y is zero we need to move down into the character
                            \ block below, so jump to LI211 to increment the screen
                            \ address accordingly (jumping back to LI220 afterwards)
    
    .LI220
    
     LDA #%00100010         \ Set a mask in A to the third pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX2              \ If we have just reached the right end of the line,
                            \ jump to LIEX2 to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI230              \ If the addition didn't overflow, jump to LI230
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     INY                    \ increment Y to move to the pixel line below
    
     BEQ LI221              \ If Y is zero we need to move down into the character
                            \ block below, so jump to LI221 to increment the screen
                            \ address accordingly (jumping back to LI230 afterwards)
    
    .LI230
    
     LDA #%00010001         \ Set a mask in A to the fourth pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI240              \ If the addition didn't overflow, jump to LI240
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     INY                    \ increment Y to move to the pixel line below
    
     BEQ LI231              \ If Y is zero we need to move down into the character
                            \ block below, so jump to LI231 to increment the screen
                            \ address accordingly (jumping back to LI240 afterwards)
    
    .LI240
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX2              \ If we have just reached the right end of the line,
                            \ jump to LIEX2 to return from the subroutine
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #8                 \ character along to the right
     STA SC
    
     BCC LI200              \ If the addition didn't overflow, jump back to LI200
                            \ to plot the next pixel
    
     INC SC+1               \ Otherwise the low byte of SC(1 0) just overflowed, so
                            \ increment the high byte SC+1 as we just crossed over
                            \ into the right half of the screen
    
     CLC                    \ Clear the C flag to avoid breaking any arithmetic
    
     BCC LI200              \ Jump back to LI200 to plot the next pixel
    
    .LI201
    
     INC SC+1               \ If we get here then we need to move down into the
     INC SC+1               \ character block below, so we increment the high byte
     LDY #248               \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the first line in that
                            \ character block (as we subtracted 248 from SC above)
    
     BNE LI210              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI211
    
     INC SC+1               \ If we get here then we need to move down into the
     INC SC+1               \ character block below, so we increment the high byte
     LDY #248               \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the first line in that
                            \ character block (as we subtracted 248 from SC above)
    
     BNE LI220              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI221
    
     INC SC+1               \ If we get here then we need to move down into the
     INC SC+1               \ character block below, so we increment the high byte
     LDY #248               \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the first line in that
                            \ character block (as we subtracted 248 from SC above)
    
     BNE LI230              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI231
    
     INC SC+1               \ If we get here then we need to move down into the
     INC SC+1               \ character block below, so we increment the high byte
     LDY #248               \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the first line in that
                            \ character block (as we subtracted 248 from SC above)
    
     BNE LI240              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LIEX2
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LOINQ (Part 5 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a line: Line has a steep gradient, step up along y-axis
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/loinq_part_5_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/loin_part_5_of_7-loinq_part_5_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * |delta_y| >= |delta_x|
    
       * The line is closer to being vertical than horizontal
    
       * We are going to step up along the y-axis
    
       * We potentially swap coordinates to make sure Y1 >= Y2
    
    
    
    
    .STPY
    
     LDY Y1                 \ Set A = Y = Y1
     TYA
    
     LDX X1                 \ Set X = X1
    
     CPY Y2                 \ If Y1 >= Y2, jump down to LI15, as the coordinates are
     BCS LI15               \ already in the order that we want
    
     DEC SWAP               \ Otherwise decrement SWAP from 0 to &FF, to denote that
                            \ we are swapping the coordinates around
    
     LDA X2                 \ Swap the values of X1 and X2
     STA X1
     STX X2
    
     TAX                    \ Set X = X1
    
     LDA Y2                 \ Swap the values of Y1 and Y2
     STA Y1
     STY Y2
    
     TAY                    \ Set Y = A = Y1
    
    .LI15
    
                            \ By this point we know the line is vertical-ish and
                            \ Y1 >= Y2, so we're going from top to bottom as we go
                            \ from Y1 to Y2
    
     LDA ylookup,Y          \ Look up the page number of the character row that
     STA SC+1               \ contains the pixel with the y-coordinate in Y1, and
                            \ store it in the high byte of SC(1 0) at SC+1, so the
                            \ high byte of SC is set correctly for drawing our line
    
     TXA                    \ Set A = 2 * bits 2-6 of X1
     AND #%11111100         \
     ASL A                  \ and shift bit 7 of X1 into the C flag
    
     STA SC                 \ Store this value in SC, so SC(1 0) now contains the
                            \ screen address of the far left end (x-coordinate = 0)
                            \ of the horizontal pixel row that we want to draw the
                            \ start of our line on
    
     BCC P%+4               \ If bit 7 of X1 was set, so X1 > 127, increment the
     INC SC+1               \ high byte of SC(1 0) to point to the second page on
                            \ this screen row, as this page contains the right half
                            \ of the row
    
     TXA                    \ Set X = X1 mod 4, which is the horizontal pixel number
     AND #3                 \ within the character block where the line starts (as
     TAX                    \ each pixel line in the character block is 4 pixels
                            \ wide)
    
     LDA TWOS,X             \ Fetch a one-pixel byte from TWOS where pixel X is set,
     STA R                  \ and store it in R
    
                            \ The following section calculates:
                            \
                            \   P = P / Q
                            \     = |delta_x| / |delta_y|
                            \
                            \ using the log tables at logL and log to calculate:
                            \
                            \   A = log(P) - log(Q)
                            \     = log(|delta_x|) - log(|delta_y|)
                            \
                            \ by first subtracting the low bytes of the logarithms
                            \ from the table at LogL, and then subtracting the high
                            \ bytes from the table at log, before applying the
                            \ antilog to get the result of the division and putting
                            \ it in P
    
     LDX P                  \ Set X = |delta_x|
    
     BEQ LIfudge            \ If |delta_x| = 0, jump to LIfudge to return 0 as the
                            \ result of the division
    
     LDA logL,X             \ Set A = log(P) - log(Q)
     LDX Q                  \       = log(|delta_x|) - log(|delta_y|)
     SEC                    \
     SBC logL,X             \ by first subtracting the low bytes of log(P) - log(Q)
    
     LDX P                  \ And then subtracting the high bytes of log(P) - log(Q)
     LDA log,X              \ so now A contains the high byte of log(P) - log(Q)
     LDX Q
     SBC log,X
    
     BCS LIlog3             \ If the subtraction fitted into one byte and didn't
                            \ underflow, then log(P) - log(Q) < 256, so we jump to
                            \ LIlog3 to return a result of 255
    
     TAX                    \ Otherwise we set A to the A-th entry from the antilog
     LDA alogh,X            \ table so the result of the division is now in A
    
     JMP LIlog2             \ Jump to LIlog2 to return the result
    
    .LIlog3
    
     LDA #255               \ The division is very close to 1, so set A to the
                            \ closest possible answer to 256, i.e. 255
    
    .LIlog2
    
     STA P                  \ Store the result of the division in P, so we have:
                            \
                            \   P = |delta_x| / |delta_y|
    
    .LIfudge
    
     LDX Q                  \ Set X = Q
                            \       = |delta_y|
    
     BEQ LIEX7              \ If |delta_y| = 0, jump down to LIEX7 to return from
                            \ the subroutine
    
     INX                    \ Set X = Q + 1
                            \       = |delta_y| + 1
                            \
                            \ We add 1 so we can skip the first pixel plot if the
                            \ line is being drawn with swapped coordinates
    
     LDA X2                 \ Set A = X2 - X1
     SEC
     SBC X1
    
     BCS P%+6               \ If X2 >= X1 then skip the following two instructions
    
     JMP LFT                \ If X2 < X1 then jump to LFT, as we need to draw the
                            \ line to the left and down
    
    .LIEX7
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LOINQ (Part 6 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a steep line going up and left or down and right
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/loinq_part_6_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/loin_part_6_of_7-loinq_part_6_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * The line is going up and left (no swap) or down and right (swap)
    
       * X1 < X2 and Y1 >= Y2
    
       * Draw from (X1, Y1) at top left to (X2, Y2) at bottom right, omitting the
         first pixel
    
     This routine looks complex, but that's because the loop that's used in the
     BBC Micro cassette and disc versions has been unrolled to speed it up. The
     algorithm is unchanged, it's just a lot longer.
    
    
    
    
     LDA SWAP               \ If SWAP = 0 then we didn't swap the coordinates above,
     BEQ LI290              \ so jump down to LI290 to plot the first pixel
    
     TYA                    \ Fetch bits 0-2 of the y-coordinate, so Y contains the
     AND #7                 \ y-coordinate mod 8
     TAY
    
     BNE P%+5               \ If Y = 0, jump to LI307+8 to start plotting from the
     JMP LI307+8            \ pixel above the top row of this character block
                            \ (LI307+8 points to the DEX instruction after the
                            \ EOR/STA instructions, so the pixel at row 0 doesn't
                            \ get plotted but we join at the right point to
                            \ decrement X and Y correctly to continue plotting from
                            \ the character row above)
    
     CPY #2                 \ If Y < 2 (i.e. Y = 1), jump to LI306+8 to start
     BCS P%+5               \ plotting from row 0 of this character block, missing
     JMP LI306+8            \ out row 1
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 2, jump to LI305+8 to start plotting from row
     JMP LI305+8            \ 1 of this character block, missing out row 2
    
     CPY #4                 \ If Y < 4 (i.e. Y = 3), jump to LI304+8 to start
     BCS P%+5               \ plotting from row 2 of this character block, missing
     JMP LI304+8            \ out row 3
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 4, jump to LI303+8 to start plotting from row
     JMP LI303+8            \ 3 of this character block, missing out row 4
    
     CPY #6                 \ If Y < 6 (i.e. Y = 5), jump to LI302+8 to start
     BCS P%+5               \ plotting from row 4 of this character block, missing
     JMP LI302+8            \ out row 5
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ P%+5               \ If Y <> 6 (i.e. Y = 7), jump to LI300+8 to start
     JMP LI300+8            \ plotting from row 6 of this character block, missing
                            \ out row 7
    
     JMP LI301+8            \ Otherwise Y = 6, so jump to LI301+8 to start plotting
                            \ from row 5 of this character block, missing out row 6
    
    .LI290
    
     DEX                    \ Decrement the counter in X because we're about to plot
                            \ the first pixel
    
     TYA                    \ Fetch bits 0-2 of the y-coordinate, so Y contains the
     AND #7                 \ y-coordinate mod 8
     TAY
    
     BNE P%+5               \ If Y = 0, jump to LI307 to start plotting from row 0
     JMP LI307              \ of this character block
    
     CPY #2                 \ If Y < 2 (i.e. Y = 1), jump to LI306 to start plotting
     BCS P%+5               \ from row 1 of this character block
     JMP LI306
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 2, jump to LI305 to start plotting from row 2
     JMP LI305              \ of this character block
    
     CPY #4                 \ If Y < 4 (i.e. Y = 3), jump to LI304 (via LI304S) to
     BCC LI304S             \ start plotting from row 3 of this character block
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ LI303S             \ If Y = 4, jump to LI303 (via LI303S) to start plotting
                            \ from row 4 of this character block
    
     CPY #6                 \ If Y < 6 (i.e. Y = 5), jump to LI302 (via LI302S) to
     BCC LI302S             \ start plotting from row 5 of this character block
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ LI301S             \ If Y = 6, jump to LI301 (via LI301S) to start plotting
                            \ from row 6 of this character block
    
     JMP LI300              \ Otherwise Y = 7, so jump to LI300 to start plotting
                            \ from row 7 of this character block
    
    .LI310
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI300, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI301              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI301 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI301              \ If the addition didn't overflow, jump to LI301 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI301S
    
     BCC LI301              \ Jump to LI301 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI311
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI301, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI302              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI302 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI302              \ If the addition didn't overflow, jump to LI302 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI302S
    
     BCC LI302              \ Jump to LI302 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI312
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI302, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI303              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI303 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI303              \ If the addition didn't overflow, jump to LI303 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI303S
    
     BCC LI303              \ Jump to LI303 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI313
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI303, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI304              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI304 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI304              \ If the addition didn't overflow, jump to LI304 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI304S
    
     BCC LI304              \ Jump to LI304 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LIEX3
    
     RTS                    \ Return from the subroutine
    
    .LI300
    
                            \ Plot a pixel on row 7 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX3              \ If we have just reached the right end of the line,
                            \ jump to LIEX3 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI310              \ If the addition overflowed, jump to LI310 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI301 below
    
    .LI301
    
                            \ Plot a pixel on row 6 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX3              \ If we have just reached the right end of the line,
                            \ jump to LIEX3 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI311              \ If the addition overflowed, jump to LI311 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI302 below
    
    .LI302
    
                            \ Plot a pixel on row 5 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX3              \ If we have just reached the right end of the line,
                            \ jump to LIEX3 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI312              \ If the addition overflowed, jump to LI312 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI303 below
    
    .LI303
    
                            \ Plot a pixel on row 4 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX3              \ If we have just reached the right end of the line,
                            \ jump to LIEX3 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI313              \ If the addition overflowed, jump to LI313 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI304 below
    
    .LI304
    
                            \ Plot a pixel on row 3 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX4              \ If we have just reached the right end of the line,
                            \ jump to LIEX4 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI314              \ If the addition overflowed, jump to LI314 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI305 below
    
    .LI305
    
                            \ Plot a pixel on row 2 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX4              \ If we have just reached the right end of the line,
                            \ jump to LIEX4 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI315              \ If the addition overflowed, jump to LI315 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI306 below
    
    .LI306
    
                            \ Plot a pixel on row 1 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX4              \ If we have just reached the right end of the line,
                            \ jump to LIEX4 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI316              \ If the addition overflowed, jump to LI316 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI307 below
    
    .LI307
    
                            \ Plot a pixel on row 0 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX4              \ If we have just reached the right end of the line,
                            \ jump to LIEX4 to return from the subroutine
    
     DEC SC+1               \ We just reached the top of the character block, so
     DEC SC+1               \ decrement the high byte in SC(1 0) twice to point to
     LDY #7                 \ the screen row above (as there are two pages per
                            \ screen row) and set Y to point to the last row in the
                            \ new character block
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS P%+5               \ If the addition didn't overflow, jump to LI300 to
     JMP LI300              \ continue plotting in the next character block along
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI307 above, so shift the
                            \ single pixel in R to the right, so the next pixel we
                            \ plot will be at the next x-coordinate
    
     BCS P%+5               \ If the pixel didn't fall out of the right end of R
     JMP LI300              \ into the C flag, then jump to LI400 to continue
                            \ plotting in the next character block along
    
     LDA #%10001000         \ Otherwise we need to move over to the next character
     STA R                  \ along, so set a mask in R to the first pixel in the
                            \ four-pixel byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ took the above BCS, so the ADC adds 8)
    
     BCS P%+5               \ If the addition didn't overflow, ump to LI300 to
     JMP LI300              \ continue plotting in the next character block along
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     JMP LI300              \ Jump to LI300 to continue plotting in the next
                            \ character block along
    
    .LIEX4
    
     RTS                    \ Return from the subroutine
    
    .LI314
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI304, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI305              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI305 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI305              \ If the addition didn't overflow, jump to LI305 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI305              \ Jump to LI305 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI315
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI305, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI306              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI306 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI306              \ If the addition didn't overflow, jump to LI306 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI306              \ Jump to LI306 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI316
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI306, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI307              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI307 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI307              \ If the addition didn't overflow, jump to LI307 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI307              \ Jump to LI307 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    
    
    
           Name: LOINQ (Part 7 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a steep line going up and right or down and left
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/loinq_part_7_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/loin_part_7_of_7-loinq_part_7_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * The line is going up and right (no swap) or down and left (swap)
    
       * X1 >= X2 and Y1 >= Y2
    
       * Draw from (X1, Y1) at bottom left to (X2, Y2) at top right, omitting the
         first pixel
    
     This routine looks complex, but that's because the loop that's used in the
     BBC Micro cassette and disc versions has been unrolled to speed it up. The
     algorithm is unchanged, it's just a lot longer.
    
    
    
    
    .LFT
    
     LDA SWAP               \ If SWAP = 0 then we didn't swap the coordinates above,
     BEQ LI291              \ so jump down to LI291 to plot the first pixel
    
     TYA                    \ Fetch bits 0-2 of the y-coordinate, so Y contains the
     AND #7                 \ y-coordinate mod 8
     TAY                    \
                            \ So Y is the pixel row within the character block where
                            \ we want to start drawing
    
     BNE P%+5               \ If Y = 0, jump to LI407+8 to start plotting from the
     JMP LI407+8            \ pixel above the top row of this character block
                            \ (LI407+8 points to the DEX instruction after the
                            \ EOR/STA instructions, so the pixel at row 0 doesn't
                            \ get plotted but we join at the right point to
                            \ decrement X and Y correctly to continue plotting from
                            \ the character row above)
    
     CPY #2                 \ If Y < 2 (i.e. Y = 1), jump to LI406+8 to start
     BCS P%+5               \ plotting from row 0 of this character block, missing
     JMP LI406+8            \ out row 1
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 2, jump to LI405+8 to start plotting from row
     JMP LI405+8            \ 1 of this character block, missing out row 2
    
     CPY #4                 \ If Y < 4 (i.e. Y = 3), jump to LI404+8 to start
     BCS P%+5               \ plotting from row 2 of this character block, missing
     JMP LI404+8            \ out row 3
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 4, jump to LI403+8 to start plotting from row
     JMP LI403+8            \ 3 of this character block, missing out row 4
    
     CPY #6                 \ If Y < 6 (i.e. Y = 5), jump to LI402+8 to start
     BCS P%+5               \ plotting from row 4 of this character block, missing
     JMP LI402+8            \ out row 5
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ P%+5               \ If Y <> 6 (i.e. Y = 7), jump to LI400+8 to start
     JMP LI400+8            \ plotting from row 6 of this character block, missing
                            \ out row 7
    
     JMP LI401+8            \ Otherwise Y = 6, so jump to LI401+8 to start plotting
                            \ from row 5 of this character block, missing out row 6
    
    .LI291
    
     DEX                    \ Decrement the counter in X because we're about to plot
                            \ the first pixel
    
     TYA                    \ Fetch bits 0-2 of the y-coordinate, so Y contains the
     AND #7                 \ y-coordinate mod 8
     TAY                    \
                            \ So Y is the pixel row within the character block where
                            \ we want to start drawing
    
     BNE P%+5               \ If Y = 0, jump to LI407 to start plotting from row 0
     JMP LI407              \ of this character block
    
     CPY #2                 \ If Y < 2 (i.e. Y = 1), jump to LI406 to start plotting
     BCS P%+5               \ from row 1 of this character block
     JMP LI406
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 2, jump to LI405 to start plotting from row 2
     JMP LI405              \ of this character block
    
     CPY #4                 \ If Y < 4 (i.e. Y = 3), jump to LI404 (via LI404S) to
     BCC LI404S             \ start plotting from row 3 of this character block
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ LI403S             \ If Y = 4, jump to LI403 (via LI403S) to start plotting
                            \ from row 4 of this character block
    
     CPY #6                 \ If Y < 6 (i.e. Y = 5), jump to LI402 (via LI402S) to
     BCC LI402S             \ start plotting from row 5 of this character block
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ LI401S             \ If Y = 6, jump to LI401 (via LI401S) to start plotting
                            \ from row 6 of this character block
    
     JMP LI400              \ Otherwise Y = 7, so jump to LI400 to start plotting
                            \ from row 7 of this character block
    
    .LI410
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI400, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI401              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI401 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI401S
    
     BCC LI401              \ Jump to LI401 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI411
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI410, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI402              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI402 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI402S
    
     BCC LI402              \ Jump to LI402 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI412
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI420, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI403              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI403 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI403S
    
     BCC LI403              \ Jump to LI403 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI413
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI430, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI404              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI404 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI404S
    
     BCC LI404              \ Jump to LI404 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LIEX5
    
     RTS                    \ Return from the subroutine
    
    .LI400
    
                            \ Plot a pixel on row 7 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX5              \ If we have just reached the right end of the line,
                            \ jump to LIEX5 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI410              \ If the addition overflowed, jump to LI410 to move to
                            \ the pixel in the row above, which returns us to LI401
                            \ below
    
    .LI401
    
                            \ Plot a pixel on row 6 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX5              \ If we have just reached the right end of the line,
                            \ jump to LIEX5 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI411              \ If the addition overflowed, jump to LI411 to move to
                            \ the pixel in the row above, which returns us to LI402
                            \ below
    
    .LI402
    
                            \ Plot a pixel on row 5 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX5              \ If we have just reached the right end of the line,
                            \ jump to LIEX5 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI412              \ If the addition overflowed, jump to LI412 to move to
                            \ the pixel in the row above, which returns us to LI403
                            \ below
    
    .LI403
    
                            \ Plot a pixel on row 4 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX5              \ If we have just reached the right end of the line,
                            \ jump to LIEX5 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI413              \ If the addition overflowed, jump to LI413 to move to
                            \ the pixel in the row above, which returns us to LI404
                            \ below
    
    .LI404
    
                            \ Plot a pixel on row 3 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX6              \ If we have just reached the right end of the line,
                            \ jump to LIEX6 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI414              \ If the addition overflowed, jump to LI414 to move to
                            \ the pixel in the row above, which returns us to LI405
                            \ below
    
    .LI405
    
                            \ Plot a pixel on row 2 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX6              \ If we have just reached the right end of the line,
                            \ jump to LIEX6 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI415              \ If the addition overflowed, jump to LI415 to move to
                            \ the pixel in the row above, which returns us to LI406
                            \ below
    
    .LI406
    
                            \ Plot a pixel on row 1 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX6              \ If we have just reached the right end of the line,
                            \ jump to LIEX6 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI416              \ If the addition overflowed, jump to LI416 to move to
                            \ the pixel in the row above, which returns us to LI407
                            \ below
    
    .LI407
    
                            \ Plot a pixel on row 0 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX6              \ If we have just reached the right end of the line,
                            \ jump to LIEX6 to return from the subroutine
    
     DEC SC+1               \ We just reached the top of the character block, so
     DEC SC+1               \ decrement the high byte in SC(1 0) twice to point to
     LDY #7                 \ the screen row above (as there are two pages per
                            \ screen row) and set Y to point to the last row in the
                            \ new character block
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS P%+5               \ If the addition didn't overflow, jump to LI400 to
     JMP LI400              \ continue plotting from row 7 of the new character
                            \ block
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI407 above, so shift the
                            \ single pixel in R to the left, so the next pixel we
                            \ plot will be at the previous x-coordinate
    
     BCS P%+5               \ If the pixel didn't fall out of the left end of R
     JMP LI400              \ into the C flag, then jump to LI400 to continue
                            \ plotting from row 7 of the new character block
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     JMP LI400              \ Jump to LI400 to continue plotting from row 7 of the
                            \ new character block
    
    .LIEX6
    
     RTS                    \ Return from the subroutine
    
    .LI414
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI440, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI405              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI405 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI405              \ Jump to LI405 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI415
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI450, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI406              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI406 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI406              \ Jump to LI406 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI416
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI460, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI407              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI407 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     JMP LI407              \ Jump to LI407 to rejoin the pixel plotting routine
    
    
    
    
           Name: HLOIN                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a horizontal line from (X1, Y1) to (X2, Y1)
      Deep dive: [Drawing colour pixels on the BBC Micro](https://elite.bbcelite.com/deep_dives/drawing_colour_pixels_in_mode_5.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hloin.md)
     Variations: See [code variations](../related/compare/main/subroutine/hloin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HLOIN2](../main/subroutine/hloin2.md) calls HLOIN
                 * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md) calls HLOIN
                 * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md) calls via HLOIN3
                 * [NLIN2](../main/subroutine/nlin2.md) calls via HLOIN3
                 * [TT15](../main/subroutine/tt15.md) calls via HLOIN3
    
    
    
    
    
    * * *
    
    
     This routine draws a horizontal orange line in the space view.
    
     We do not draw a pixel at the right end of the line.
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       HLOIN3               Draw a line from (X, Y1) to (X2, Y1) in the colour given
                            in A
    
    
    
    
    .HLOIN
    
     LDA Y1                 \ Set A = Y1, the pixel y-coordinate of the line
    
     AND #3                 \ Set A to the correct order of red/yellow pixels to
     TAX                    \ make this line an orange colour (by using bits 0-1 of
     LDA orange,X           \ the pixel y-coordinate as the index into the orange
                            \ lookup table)
    
     STA COL                \ Store the correct orange colour in COL
    
    .HLOIN3
    
     STY YSAV               \ Store Y into YSAV, so we can preserve it across the
                            \ call to this subroutine
    
     LDY #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDX X1                 \ Set X = X1
    
     CPX X2                 \ If X1 = X2 then the start and end points are the same,
     BEQ HL6                \ so return from the subroutine (as HL6 contains an RTS)
    
     BCC HL5                \ If X1 < X2, jump to HL5 to skip the following code, as
                            \ (X1, Y1) is already the left point
    
     LDA X2                 \ Swap the values of X1 and X2, so we know that (X1, Y1)
     STA X1                 \ is on the left and (X2, Y1) is on the right
     STX X2
    
     TAX                    \ Set X = X1
    
    .HL5
    
     DEC X2                 \ Decrement X2 so we do not draw a pixel at the end
                            \ point
    
     LDY Y1                 \ Look up the page number of the character row that
     LDA ylookup,Y          \ contains the pixel with the y-coordinate in Y1, and
     STA SC+1               \ store it in SC+1, so the high byte of SC is set
                            \ correctly for drawing our line
    
     TYA                    \ Set A = Y1 mod 8, which is the pixel row within the
     AND #7                 \ character block at which we want to draw our line (as
                            \ each character block has 8 rows)
    
     STA SC                 \ Store this value in SC, so SC(1 0) now contains the
                            \ screen address of the far left end (x-coordinate = 0)
                            \ of the horizontal pixel row that we want to draw our
                            \ horizontal line on
    
     TXA                    \ Set Y = 2 * bits 2-6 of X1
     AND #%11111100         \
     ASL A                  \ and shift bit 7 of X1 into the C flag
     TAY
    
     BCC P%+4               \ If bit 7 of X1 was set, so X1 > 127, increment the
     INC SC+1               \ high byte of SC(1 0) to point to the second page on
                            \ this screen row, as this page contains the right half
                            \ of the row
    
    .HL1
    
     TXA                    \ Set T = bits 2-7 of X1, which will contain the
     AND #%11111100         \ character number of the start of the line * 4
     STA T
    
     LDA X2                 \ Set A = bits 2-7 of X2, which will contain the
     AND #%11111100         \ character number of the end of the line * 4
    
     SEC                    \ Set A = A - T, which will contain the number of
     SBC T                  \ character blocks we need to fill - 1 * 4
    
     BEQ HL2                \ If A = 0 then the start and end character blocks are
                            \ the same, so the whole line fits within one block, so
                            \ jump down to HL2 to draw the line
    
                            \ Otherwise the line spans multiple characters, so we
                            \ start with the left character, then do any characters
                            \ in the middle, and finish with the right character
    
     LSR A                  \ Set R = A / 4, so R now contains the number of
     LSR A                  \ character blocks we need to fill - 1
     STA R
    
     LDA X1                 \ Set X = X1 mod 4, which is the horizontal pixel number
     AND #3                 \ within the character block where the line starts (as
     TAX                    \ each pixel line in the character block is 4 pixels
                            \ wide)
    
     LDA TWFR,X             \ Fetch a ready-made byte with X pixels filled in at the
                            \ right end of the byte (so the filled pixels start at
                            \ point X and go all the way to the end of the byte),
                            \ which is the shape we want for the left end of the
                            \ line
    
     AND COL                \ Apply the pixel mask in A to the four-pixel block of
                            \ coloured pixels in COL, so we now know which bits to
                            \ set in screen memory to paint the relevant pixels in
                            \ the required colour
    
     EOR (SC),Y             \ Store this into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen,
                            \ so we have now drawn the line's left cap
    
     TYA                    \ Set Y = Y + 8 so (SC),Y points to the next character
     ADC #8                 \ block along, on the same pixel row as before
     TAY
    
     BCS HL7                \ If the above addition overflowed, then we have just
                            \ crossed over from the left half of the screen into the
                            \ right half, so call HL7 to increment the high byte in
                            \ SC+1 so that SC(1 0) points to the page in screen
                            \ memory for the right half of the screen row. HL7 also
                            \ clears the C flag and jumps back to HL8, so this acts
                            \ like a conditional JSR instruction
    
    .HL8
    
     LDX R                  \ Fetch the number of character blocks we need to fill
                            \ from R
    
     DEX                    \ Decrement the number of character blocks in X
    
     BEQ HL3                \ If X = 0 then we only have the last block to do (i.e.
                            \ the right cap), so jump down to HL3 to draw it
    
     CLC                    \ Otherwise clear the C flag so we can do some additions
                            \ while we draw the character blocks with full-width
                            \ lines in them
    
    .HLL1
    
     LDA COL                \ Store a full-width four-pixel horizontal line of
     EOR (SC),Y             \ colour COL in SC(1 0) so that it draws the line
     STA (SC),Y             \ on-screen, using EOR logic so it merges with whatever
                            \ is already on-screen
    
     TYA                    \ Set Y = Y + 8 so (SC),Y points to the next character
     ADC #8                 \ block along, on the same pixel row as before
     TAY
    
     BCS HL9                \ If the above addition overflowed, then we have just
                            \ crossed over from the left half of the screen into the
                            \ right half, so call HL9 to increment the high byte in
                            \ SC+1 so that SC(1 0) points to the page in screen
                            \ memory for the right half of the screen row. HL9 also
                            \ clears the C flag and jumps back to HL10, so this acts
                            \ like a conditional JSR instruction
    
    .HL10
    
     DEX                    \ Decrement the number of character blocks in X
    
     BNE HLL1               \ Loop back to draw more full-width lines, if we have
                            \ any more to draw
    
    .HL3
    
     LDA X2                 \ Now to draw the last character block at the right end
     AND #3                 \ of the line, so set X = X2 mod 3, which is the
     TAX                    \ horizontal pixel number where the line ends
    
     LDA TWFL,X             \ Fetch a ready-made byte with X pixels filled in at the
                            \ left end of the byte (so the filled pixels start at
                            \ the left edge and go up to point X), which is the
                            \ shape we want for the right end of the line
    
     AND COL                \ Apply the pixel mask in A to the four-pixel block of
                            \ coloured pixels in COL, so we now know which bits to
                            \ set in screen memory to paint the relevant pixels in
                            \ the required colour
    
     EOR (SC),Y             \ Store this into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen,
                            \ so we have now drawn the line's right cap
    
    .HL6
    
     LDY #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY YSAV               \ Restore Y from YSAV, so that it's preserved
    
     RTS                    \ Return from the subroutine
    
    .HL2
    
                            \ If we get here then the entire horizontal line fits
                            \ into one character block
    
     LDA X1                 \ Set X = X1 mod 4, which is the horizontal pixel number
     AND #3                 \ within the character block where the line starts (as
     TAX                    \ each pixel line in the character block is 4 pixels
                            \ wide)
    
     LDA TWFR,X             \ Fetch a ready-made byte with X pixels filled in at the
     STA T                  \ right end of the byte (so the filled pixels start at
                            \ point X and go all the way to the end of the byte)
    
     LDA X2                 \ Set X = X2 mod 4, which is the horizontal pixel number
     AND #3                 \ where the line ends
     TAX
    
     LDA TWFL,X             \ Fetch a ready-made byte with X pixels filled in at the
                            \ left end of the byte (so the filled pixels start at
                            \ the left edge and go up to point X)
    
     AND T                  \ We now have two bytes, one (T) containing pixels from
                            \ the starting point X1 onwards, and the other (A)
                            \ containing pixels up to the end point at X2, so we can
                            \ get the actual line we want to draw by AND'ing them
                            \ together. For example, if we want to draw a line from
                            \
                            \   T       = %00111111
                            \   A       = %11111100
                            \   T AND A = %00111100
                            \
                            \ So we can stick T AND A in screen memory to get the
                            \ line we want, which is what we do here by setting
                            \ A = A AND T
    
     AND COL                \ Apply the pixel mask in A to the four-pixel block of
                            \ coloured pixels in COL, so we now know which bits to
                            \ set in screen memory to paint the relevant pixels in
                            \ the required colour
    
     EOR (SC),Y             \ Store our horizontal line byte into screen memory at
     STA (SC),Y             \ SC(1 0), using EOR logic so it merges with whatever is
                            \ already on-screen
    
     LDY #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY YSAV               \ Restore Y from YSAV, so that it's preserved
    
     RTS                    \ Return from the subroutine
    
    .HL7
    
     INC SC+1               \ We have just crossed over from the left half of the
                            \ screen into the right half, so increment the high byte
                            \ in SC+1 so that SC(1 0) points to the page in screen
                            \ memory for the right half of the screen row
    
     CLC                    \ Clear the C flag (as HL7 is called with the C flag
                            \ set, which this instruction reverts)
    
     JMP HL8                \ Jump back to HL8, just after the instruction that
                            \ called HL7
    
    .HL9
    
     INC SC+1               \ We have just crossed over from the left half of the
                            \ screen into the right half, so increment the high byte
                            \ in SC+1 so that SC(1 0) points to the page in screen
                            \ memory for the right half of the screen row
    
     CLC                    \ Clear the C flag (as HL9 is called with the C flag
                            \ set, which this instruction reverts)
    
     JMP HL10               \ Jump back to HL10, just after the instruction that
                            \ called HL9
    
    
    
    
           Name: TWFL                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made character rows for the left end of a horizontal line
    
    
        Context: See this variable [on its own page](../main/variable/twfl.md)
     References: This variable is used as follows:
                 * [HLOIN](../main/subroutine/hloin.md) uses TWFL
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting horizontal line end caps in mode 1 (the top part
     of the split screen). This table provides a byte with pixels at the left end,
     which is used for the right end of the line.
    
     See the HLOIN routine for details.
    
    
    
    
    .TWFL
    
     EQUB %10001000
     EQUB %11001100
     EQUB %11101110
     EQUB %11111111
    
    
    
    
           Name: TWFR                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made character rows for the right end of a horizontal line
    
    
        Context: See this variable [on its own page](../main/variable/twfr.md)
     References: This variable is used as follows:
                 * [HLOIN](../main/subroutine/hloin.md) uses TWFR
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting horizontal line end caps in mode 1 (the top part
     of the split screen). This table provides a byte with pixels at the left end,
     which is used for the right end of the line.
    
     See the HLOIN routine for details.
    
    
    
    
    .TWFR
    
     EQUB %11111111
     EQUB %01110111
     EQUB %00110011
     EQUB %00010001
    
    
    
    
           Name: orange                                                  [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Lookup table for two-pixel mode 1 orange pixels for the sun
    
    
        Context: See this variable [on its own page](../main/variable/orange.md)
     References: This variable is used as follows:
                 * [HLOIN](../main/subroutine/hloin.md) uses orange
    
    
    
    
    
    * * *
    
    
     Blocks of orange (as used when drawing the sun) have alternate red and yellow
     pixels in a cross-hatch pattern. The cross-hatch pattern is made up of offset
     rows that are 2 pixels high, and it is made up of red and yellow rectangles,
     each of which is 2 pixels high and 1 pixel wide. The result looks like this:
    
       ...ryryryryryryryry...
       ...ryryryryryryryry...
       ...yryryryryryryryr...
       ...yryryryryryryryr...
       ...ryryryryryryryry...
       ...ryryryryryryryry...
    
     and so on, repeating every four pixel rows.
    
     This is implemented with the following lookup table, where bits 0-1 of the
     pixel y-coordinate are used as the index, to fetch the correct pattern to use.
    
     Rows with y-coordinates ending in %00 or %01 fetch the red/yellow pattern from
     the table, while rows with y-coordinates ending in %10 or %11 fetch the
     yellow/red pattern, so the pattern repeats every four pixel rows.
    
    
    
    
    .orange
    
     EQUB %10100101         \ Four mode 1 pixels of colour 2, 1, 2, 1 (red/yellow)
     EQUB %10100101
     EQUB %01011010         \ Four mode 1 pixels of colour 1, 2, 1, 2 (yellow/red)
     EQUB %01011010
    
    
    
    
           Name: PIX1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (YY+1 SYL+Y) = (A P) + (S R) and draw stardust particle
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pix1.md)
     Variations: See [code variations](../related/compare/main/subroutine/pix1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STARS1](../main/subroutine/stars1.md) calls PIX1
                 * [STARS2](../main/subroutine/stars2.md) calls PIX1
                 * [STARS6](../main/subroutine/stars6.md) calls PIX1
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       (YY+1 SYL+Y) = (A P) + (S R)
    
     and draw a stardust particle at (X1,Y1) with distance ZZ.
    
    
    
    * * *
    
    
     Arguments:
    
       (A P)                A is the angle ALPHA or BETA, P is always 0
    
       (S R)                YY(1 0) or YY(1 0) + Q * A
    
       Y                    Stardust particle number
    
       X1                   The x-coordinate offset
    
       Y1                   The y-coordinate offset
    
       ZZ                   The distance of the point, with bigger distances drawing
                            smaller points:
    
                              * ZZ < 80           Double-height four-pixel square
    
                              * 80 <= ZZ <= 143   Single-height two-pixel dash
    
                              * ZZ > 143          Single-height one-pixel dot
    
    
    
    
    .PIX1
    
     JSR ADDK               \ Set (A X) = (A P) + (S R)
    
     STA YY+1               \ Set YY+1 to A, the high byte of the result
    
     TXA                    \ Set SYL+Y to X, the low byte of the result
     STA SYL,Y
    
                            \ Fall through into PIXEL2 to draw the stardust particle
                            \ at (X1,Y1)
    
    
    
    
           Name: PIXEL2                                                  [Show more]
           Type: Subroutine
       Category: Drawing pixels
        Summary: Draw a stardust particle relative to the screen centre
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pixel2.md)
     Variations: See [code variations](../related/compare/main/subroutine/pixel2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [FLIP](../main/subroutine/flip.md) calls PIXEL2
                 * [nWq](../main/subroutine/nwq.md) calls PIXEL2
                 * [STARS1](../main/subroutine/stars1.md) calls PIXEL2
                 * [STARS2](../main/subroutine/stars2.md) calls PIXEL2
                 * [STARS6](../main/subroutine/stars6.md) calls PIXEL2
    
    
    
    
    
    * * *
    
    
     Draw a point (X1, Y1) from the middle of the screen with a size determined by
     a distance value. Used to draw stardust particles.
    
    
    
    * * *
    
    
     Arguments:
    
       X1                   The x-coordinate offset
    
       Y1                   The y-coordinate offset (positive means up the screen
                            from the centre, negative means down the screen)
    
       ZZ                   The distance of the point, with bigger distances drawing
                            smaller points:
    
                              * ZZ < 80           Double-height four-pixel square
    
                              * 80 <= ZZ <= 143   Single-height two-pixel dash
    
                              * ZZ > 143          Single-height one-pixel dot
    
    
    
    
    .PIXEL2
    
     LDA X1                 \ Fetch the x-coordinate offset into A
    
     BPL PX21               \ If the x-coordinate offset is positive, jump to PX21
                            \ to skip the following negation
    
     EOR #%01111111         \ The x-coordinate offset is negative, so flip all the
     CLC                    \ bits apart from the sign bit and add 1, to convert it
     ADC #1                 \ from a sign-magnitude number to a signed number
    
    .PX21
    
     EOR #%10000000         \ Set X = X1 + 128
     TAX                    \
                            \ So X is now the offset converted to an x-coordinate,
                            \ centred on x-coordinate 128
    
     LDA Y1                 \ Fetch the y-coordinate offset into A and clear the
     AND #%01111111         \ sign bit, so A = |Y1|
    
     CMP #Y                 \ If |Y1| >= #Y then it's off the screen (as #Y is half
     BCS PXR1               \ the screen height), so return from the subroutine (as
                            \ PXR1 contains an RTS)
    
     LDA Y1                 \ Fetch the y-coordinate offset into A
    
     BPL PX22               \ If the y-coordinate offset is positive, jump to PX22
                            \ to skip the following negation
    
     EOR #%01111111         \ The y-coordinate offset is negative, so flip all the
     ADC #1                 \ bits apart from the sign bit and add 1, to convert it
                            \ from a sign-magnitude number to a signed number
    
    .PX22
    
     STA T                  \ Set A = #Y + 1 - Y1
     LDA #Y+1               \
     SBC T                  \ So if Y1 is positive we display the point up from the
                            \ centre at y-coordinate 97, while a negative Y1 means
                            \ down from the centre
    
                            \ Fall through into PIXEL to draw the stardust at the
                            \ screen coordinates in (X, A)
    
    
    
    
           Name: PIXEL                                                   [Show more]
           Type: Subroutine
       Category: Drawing pixels
        Summary: Draw a one-pixel dot, two-pixel dash or four-pixel square
      Deep dive: [Drawing colour pixels on the BBC Micro](https://elite.bbcelite.com/deep_dives/drawing_colour_pixels_in_mode_5.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pixel.md)
     Variations: See [code variations](../related/compare/main/subroutine/pixel.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOEXP](../main/subroutine/doexp.md) calls PIXEL
                 * [TT22](../main/subroutine/tt22.md) calls PIXEL
                 * [PIXEL2](../main/subroutine/pixel2.md) calls via PXR1
    
    
    
    
    
    * * *
    
    
     Draw a point at screen coordinate (X, A) with the point size determined by the
     distance in ZZ. This applies to the top part of the screen (the four-colour
     mode 5 portion).
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The screen x-coordinate of the point to draw
    
       A                    The screen y-coordinate of the point to draw
    
       ZZ                   The distance of the point, with bigger distances drawing
                            smaller points:
    
                              * ZZ < 80           Double-height four-pixel square
    
                              * 80 <= ZZ <= 143   Single-height two-pixel dash
    
                              * ZZ > 143          Single-height one-pixel dot
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       PXR1                 Contains an RTS
    
    
    
    
    .PIXEL
    
     STY T1                 \ Store Y in T1 so we can restore it at the end of the
                            \ subroutine
    
     LDY #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     TAY                    \ Copy the screen y-coordinate from A into Y
    
     LDA ylookup,Y          \ Look up the page number of the character row that
     STA SC+1               \ contains the pixel with the y-coordinate in Y, and
                            \ store it in the high byte of SC(1 0) at SC+1
    
     TXA                    \ Each character block contains 8 pixel rows, so to get
     AND #%11111100         \ the address of the first byte in the character block
     ASL A                  \ that we need to draw into, as an offset from the start
                            \ of the row, we clear bits 0-1 and shift left to double
                            \ it (as each character row contains two pages of bytes,
                            \ or 512 bytes, which cover 256 pixels). This also
                            \ shifts bit 7 of the x-coordinate into the C flag
    
     STA SC                 \ Store the address of the character block in the low
                            \ byte of SC(1 0), so now SC(1 0) points to the
                            \ character block we need to draw into
    
     BCC P%+4               \ If the C flag is clear then skip the next instruction
    
     INC SC+1               \ The C flag is set, which means bit 7 of X1 was set
                            \ before the ASL above, so the x-coordinate is in the
                            \ right half of the screen (i.e. in the range 128-255).
                            \ Each row takes up two pages in memory, so the right
                            \ half is in the second page but SC+1 contains the value
                            \ we looked up from ylookup, which is the page number of
                            \ the first memory page for the row... so we need to
                            \ increment SC+1 to point to the correct page
    
     TYA                    \ Set Y = Y mod 8, which is the pixel row within the
     AND #7                 \ character block at which we want to draw the pixel
     TAY                    \ (as each character block has 8 rows)
    
     TXA                    \ Copy bits 0-1 of the x-coordinate to bits 0-1 of X,
     AND #%00000011         \ which will now be in the range 0-3, and will contain
     TAX                    \ the two pixels to show in the character row
    
     LDA ZZ                 \ Set A to the pixel's distance in ZZ
    
     CMP #80                \ If the pixel's ZZ distance is < 80, then the dot is
     BCC PX2                \ pretty close, so jump to PX2 to draw a four-pixel
                            \ square
    
     LDA TWOS2,X            \ Fetch a mode 1 two-pixel byte with the pixels set as
     AND COL                \ in X, and AND with the colour byte we fetched into COL
                            \ so that pixel takes on the colour we want to draw
                            \ (i.e. A is acting as a mask on the colour byte)
    
     EOR (SC),Y             \ Draw the pixel on-screen using EOR logic, so we can
     STA (SC),Y             \ remove it later without ruining the background that's
                            \ already on-screen
    
     LDY #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY T1                 \ Restore Y from T1, so Y is preserved by the routine
    
    .PXR1
    
     RTS                    \ Return from the subroutine
    
    .PX2
    
                            \ If we get here, we need to plot a four-pixel square in
                            \ in the correct colour for this pixel's distance
    
     LDA TWOS2,X            \ Fetch a mode 1 two-pixel byte with the pixels set as
     AND COL                \ in X, and AND with the colour byte we fetched into COL
                            \ so that pixel takes on the colour we want to draw
                            \ (i.e. A is acting as a mask on the colour byte)
    
     EOR (SC),Y             \ Draw the pixel on-screen using EOR logic, so we can
     STA (SC),Y             \ remove it later without ruining the background that's
                            \ already on-screen
    
     DEY                    \ Reduce Y by 1 to point to the pixel row above the one
     BPL P%+4               \ we just plotted, and if it is still positive, skip the
                            \ next instruction
    
     LDY #1                 \ Reducing Y by 1 made it negative, which means Y was
                            \ 0 before we did the DEY above, so set Y to 1 to point
                            \ to the pixel row after the one we just plotted
    
                            \ We now draw our second dash
    
     LDA TWOS2,X            \ Fetch a mode 1 two-pixel byte with the pixels set as
     AND COL                \ in X, and AND with the colour byte we fetched into COL
                            \ so that pixel takes on the colour we want to draw
                            \ (i.e. A is acting as a mask on the colour byte)
    
     EOR (SC),Y             \ Draw the pixel on-screen using EOR logic, so we can
     STA (SC),Y             \ remove it later without ruining the background that's
                            \ already on-screen
    
     LDY #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY T1                 \ Restore Y from T1, so Y is preserved by the routine
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PXCL                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: A four-colour mode 1 pixel byte that represents a dot's distance
    
    
        Context: See this variable [on its own page](../main/variable/pxcl.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     The following table contains colour bytes for two-pixel mode 1 pixels, with
     the index into the table representing distance. Closer pixels are at the top,
     so the closest pixels are cyan/red, then yellow, then red, then red/yellow,
     then yellow.
    
     That said, this table is only used with odd distance values, as set in the
     parasite's PIXEL3 routine, so in practice the four distances are yellow, red,
     red/yellow, yellow.
    
    
    
    
    .PXCL
    
     EQUB WHITE             \ Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)
     EQUB %00001111         \ Four mode 1 pixels of colour 1 (yellow)
     EQUB %00001111         \ Four mode 1 pixels of colour 1 (yellow)
     EQUB %11110000         \ Four mode 1 pixels of colour 2 (red)
     EQUB %11110000         \ Four mode 1 pixels of colour 2 (red)
     EQUB %10100101         \ Four mode 1 pixels of colour 2, 1, 2, 1 (red/yellow)
     EQUB %10100101         \ Four mode 1 pixels of colour 2, 1, 2, 1 (red/yellow)
     EQUB %00001111         \ Four mode 1 pixels of colour 1, 1, 1, 1 (yellow)
    
    
    
    
           Name: DOT                                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Draw a dash on the compass
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dot.md)
     Variations: See [code variations](../related/compare/main/subroutine/dot.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [COMPAS](../main/subroutine/compas.md) calls DOT
                 * [SP2](../main/subroutine/sp2.md) calls DOT
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       COMX                 The screen pixel x-coordinate of the dash
    
       COMY                 The screen pixel y-coordinate of the dash
    
       COMC                 The colour and thickness of the dash:
    
                              * &F0 = a double-height dash in yellow/white, for when
                                the object in the compass is in front of us
    
                              * &FF = a single-height dash in green/cyan, for when
                                the object in the compass is behind us
    
    
    
    
    .DOT
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA COMX               \ Set X1 = COMX, the x-coordinate of the dash
     STA X1
    
     LDX COMC               \ Set COL = COMC, the mode 2 colour byte for the dash
     STX COL
    
     LDA COMY               \ Set Y1 = COMY, the y-coordinate of the dash
    
     CPX #YELLOW2           \ If the colour in X is yellow, then the planet/station
     BNE P%+8               \ is behind us, so skip the following three instructions
                            \ so we only draw a single-height dash
    
     JSR CPIXK              \ Call CPIXK to draw a single-height dash, i.e. the top
                            \ row of a double-height dash
    
     LDA Y1                 \ Fetch the y-coordinate of the row we just drew and
     DEC A                  \ decrement it, ready to draw the bottom row
    
    .DOT2
    
     JSR CPIXK              \ Call CPIXK to draw a single-height dash
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: CPIXK                                                   [Show more]
           Type: Subroutine
       Category: Drawing pixels
        Summary: Draw a single-height dash on the dashboard
    
    
        Context: See this subroutine [on its own page](../main/subroutine/cpixk.md)
     Variations: See [code variations](../related/compare/main/subroutine/cpix2-cpixk.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOT](../main/subroutine/dot.md) calls CPIXK
                 * [SCAN](../main/subroutine/scan.md) calls CPIXK
    
    
    
    
    
    * * *
    
    
     Draw a single-height mode 2 dash (1 pixel high, 2 pixels wide).
    
    
    
    * * *
    
    
     Arguments:
    
       X1                   The screen pixel x-coordinate of the dash
    
       A                    The screen pixel y-coordinate of the dash
    
       COL                  The colour of the dash as a mode 2 character row byte
    
    
    
    * * *
    
    
     Returns:
    
       R                    The dash's right pixel byte
    
    
    
    
    .CPIXK
    
     STA Y1                 \ Store the y-coordinate in Y1
    
     TAY                    \ Store the y-coordinate in Y
    
     LDA ylookup,Y          \ Look up the page number of the character row that
     STA SC+1               \ contains the pixel with the y-coordinate in Y, and
                            \ store it in the high byte of SC(1 0) at SC+1
    
     LDA X1                 \ Each character block contains 8 pixel rows, so to get
     AND #%11111100         \ the address of the first byte in the character block
     ASL A                  \ that we need to draw into, as an offset from the start
                            \ of the row, we clear bits 0-1 and shift left to double
                            \ it (as each character row contains two pages of bytes,
                            \ or 512 bytes, which cover 256 pixels). This also
                            \ shifts bit 7 of X1 into the C flag
    
     STA SC                 \ Store the address of the character block in the low
                            \ byte of SC(1 0), so now SC(1 0) points to the
                            \ character block we need to draw into
    
     BCC P%+5               \ If the C flag is clear then skip the next two
                            \ instructions
    
     INC SC+1               \ The C flag is set, which means bit 7 of X1 was set
                            \ before the ASL above, so the x-coordinate is in the
                            \ right half of the screen (i.e. in the range 128-255).
                            \ Each row takes up two pages in memory, so the right
                            \ half is in the second page but SC+1 contains the value
                            \ we looked up from ylookup, which is the page number of
                            \ the first memory page for the row... so we need to
                            \ increment SC+1 to point to the correct page
    
     CLC                    \ Clear the C flag
    
     TYA                    \ Set Y to the y-coordinate mod 8, which will be the
     AND #7                 \ number of the pixel row we need to draw within the
     TAY                    \ character block
    
     LDA X1                 \ Copy bit 1 of X1 to bit 1 of X. X will now be either
     AND #%00000010         \ 0 or 2, and will be double the pixel number in the
     TAX                    \ character row for the left pixel in the dash (so 0
                            \ means the left pixel in the two-pixel character row,
                            \ while 2 means the right pixel)
    
     LDA CTWOS,X            \ Fetch a mode 2 one-pixel byte with the pixel position
     AND COL                \ at X/2, and AND with the colour byte so that pixel
                            \ takes on the colour we want to draw (i.e. A is acting
                            \ as a mask on the colour byte)
    
     EOR (SC),Y             \ Draw the pixel on-screen using EOR logic, so we can
     STA (SC),Y             \ remove it later without ruining the background that's
                            \ already on-screen
    
     LDA CTWOS+2,X          \ Fetch a mode 2 one-pixel byte with the pixel position
                            \ at (X+1)/2, so we can draw the right pixel of the dash
    
     BPL CP1                \ The CTWOS table has 2 extra rows at the end of it that
                            \ repeat the first values, %10101010, so if we have not
                            \ fetched that value, then the right pixel of the dash
                            \ is in the same character block as the left pixel, so
                            \ jump to CP1 to draw it
    
     LDA SC                 \ Otherwise the left pixel we drew was at the last
     ADC #8                 \ position of four in this character block, so we add
     STA SC                 \ 8 to the screen address to move onto the next block
                            \ along (as there are 8 bytes in a character block).
                            \ The C flag was cleared above, so this ADC is correct
    
     BCC P%+4               \ If the addition we just did overflowed, then increment
     INC SC+1               \ the high byte of SC(1 0), as this means we just moved
                            \ into the right half of the screen row
    
     LDA CTWOS+2,X          \ Re-fetch the mode 2 one-pixel byte, as we just
                            \ overwrote A (the byte will still be the fifth or sixth
                            \ byte from the table, which is correct as we want to
                            \ draw the leftmost pixel in the next character along as
                            \ the dash's right pixel)
    
    .CP1
    
     AND COL                \ Apply the colour mask to the pixel byte, as above
    
     STA R                  \ Store the dash's right pixel byte in R
    
     EOR (SC),Y             \ Draw the dash's right pixel according to the mask in
     STA (SC),Y             \ A, with the colour in COL, using EOR logic, just as
                            \ above
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: ECBLB2                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Start up the E.C.M. (light up the indicator, start the countdown
                 and make the E.C.M. sound)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ecblb2.md)
     Variations: See [code variations](../related/compare/main/subroutine/ecblb2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls ECBLB2
                 * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md) calls ECBLB2
    
    
    
    
    
    
    .ECBLB2
    
     LDA #32                \ Set the E.C.M. countdown timer in ECMA to 32
     STA ECMA
    
                            \ Fall through into ECBLB to light up the E.C.M. bulb
    
    
    
    
           Name: ECBLB                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Light up the E.C.M. indicator bulb ("E") on the dashboard
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ecblb.md)
     Variations: See [code variations](../related/compare/main/subroutine/ecblb.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ECMOF](../main/subroutine/ecmof.md) calls ECBLB
    
    
    
    
    
    
    .ECBLB
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA #8*14              \ The E.C.M. bulb is in character block number 14 with
     STA SC                 \ each character taking 8 bytes, so this sets the low
                            \ byte of the screen address of the character block we
                            \ want to draw to
    
     LDA #&7A               \ Set the high byte of SC(1 0) to &7A, as the bulbs are
     STA SC+1               \ both in the character row from &7A00 to &7BFF, and the
                            \ E.C.M. bulb is in the left half, which is from &7A00
                            \ to &7AFF
    
     LDY #15                \ Now to poke the bulb bitmap into screen memory, and
                            \ there are two character blocks' worth, each with eight
                            \ lines of one byte, so set a counter in Y for 16 bytes
    
    .BULL1
    
     LDA ECBT,Y             \ Fetch the Y-th byte of the bulb bitmap
    
     EOR (SC),Y             \ EOR the byte with the current contents of screen
                            \ memory, so drawing the bulb when it is already
                            \ on-screen will erase it
    
     STA (SC),Y             \ Store the Y-th byte of the bulb bitmap in screen
                            \ memory
    
     DEY                    \ Decrement the loop counter
    
     BPL BULL1              \ Loop back to poke the next byte until we have done
                            \ all 16 bytes across two character blocks
    
     BMI away               \ Jump to away to switch main memory back into
                            \ &3000-&7FFF and return from the subroutine (this BMI
                            \ is effectively a JMP as we just passed through the BPL
                            \ above)
    
    
    
    
           Name: SPBLB                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Light up the space station indicator ("S") on the dashboard
    
    
        Context: See this subroutine [on its own page](../main/subroutine/spblb.md)
     Variations: See [code variations](../related/compare/main/subroutine/spblb-dobulb.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KS4](../main/subroutine/ks4.md) calls SPBLB
                 * [NWSPS](../main/subroutine/nwsps.md) calls SPBLB
                 * [RES2](../main/subroutine/res2.md) calls SPBLB
                 * [ECBLB](../main/subroutine/ecblb.md) calls via away
                 * [HANGER](../main/subroutine/hanger.md) calls via away
                 * [MSBAR](../main/subroutine/msbar.md) calls via away
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       away                 Switch main memory back into &3000-&7FFF and return from
                            the subroutine
    
    
    
    
    .SPBLB
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA #16*8              \ The space station bulb is in character block number 48
     STA SC                 \ (counting from the left edge of the screen), with the
                            \ first half of the row in one page, and the second half
                            \ in another. We want to set the screen address to point
                            \ to the second part of the row, as the bulb is in that
                            \ half, so that's character block number 16 within that
                            \ second half (as the first half takes up 32 character
                            \ blocks, so given that each character block takes up 8
                            \ bytes, this sets the low byte of the screen address
                            \ of the character block we want to draw to
    
     LDA #&7B               \ Set the high byte of SC(1 0) to &7B, as the bulbs are
     STA SC+1               \ both in the character row from &7A00 to &7BFF, and the
                            \ space station bulb is in the right half, which is from
                            \ &7B00 to &7BFF
    
     LDY #15                \ Now to poke the bulb bitmap into screen memory, and
                            \ there are two character blocks' worth, each with eight
                            \ lines of one byte, so set a counter in Y for 16 bytes
    
    .BULL2
    
     LDA SPBT,Y             \ Fetch the Y-th byte of the bulb bitmap
    
     EOR (SC),Y             \ EOR the byte with the current contents of screen
                            \ memory, so drawing the bulb when it is already
                            \ on-screen will erase it
    
     STA (SC),Y             \ Store the Y-th byte of the bulb bitmap in screen
                            \ memory
    
     DEY                    \ Decrement the loop counter
    
     BPL BULL2              \ Loop back to poke the next byte until we have done
                            \ all 16 bytes across two character blocks
    
    .away
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SPBT                                                    [Show more]
           Type: Variable
       Category: Dashboard
        Summary: The bitmap definition for the space station indicator bulb
    
    
        Context: See this variable [on its own page](../main/variable/spbt.md)
     Variations: See [code variations](../related/compare/main/variable/spbt.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [SPBLB](../main/subroutine/spblb.md) uses SPBT
    
    
    
    
    
    * * *
    
    
     The bitmap definition for the space station indicator's "S" bulb that gets
     displayed on the dashboard.
    
     The bulb is four pixels wide, so it covers two mode 2 character blocks, one
     containing the left half of the "S", and the other the right half, which are
     displayed next to each other. Each pixel is in mode 2 colour 7 (%1111), which
     is white.
    
    
    
    
    .SPBT
    
                            \ Left half of the "S" bulb
                            \
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %10101010         \ x .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %00000000         \ . .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
    
                            \ Right half of the "S" bulb
                            \
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %00000000         \ . .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %01010101         \ . x
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
    
                            \ Combined "S" bulb
                            \
                            \ x x x x
                            \ x x x x
                            \ x . . .
                            \ x x x x
                            \ x x x x
                            \ . . . x
                            \ x x x x
                            \ x x x x
    
    
    
    
           Name: ECBT                                                    [Show more]
           Type: Variable
       Category: Dashboard
        Summary: The character bitmap for the E.C.M. indicator bulb
    
    
        Context: See this variable [on its own page](../main/variable/ecbt.md)
     Variations: See [code variations](../related/compare/main/variable/ecbt.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [ECBLB](../main/subroutine/ecblb.md) uses ECBT
    
    
    
    
    
    * * *
    
    
     The character bitmap for the E.C.M. indicator's "E" bulb that gets displayed
     on the dashboard.
    
     The bulb is four pixels wide, so it covers two mode 2 character blocks, one
     containing the left half of the "E", and the other the right half, which are
     displayed next to each other. Each pixel is in mode 2 colour 7 (%1111), which
     is white.
    
    
    
    
    .ECBT
    
                            \ Left half of the "E" bulb
                            \
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %10101010         \ x .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %10101010         \ x .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
    
                            \ Right half of the "E" bulb
                            \
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %00000000         \ . .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
     EQUB %00000000         \ . .
     EQUB %11111111         \ x x
     EQUB %11111111         \ x x
    
                            \ Combined "E" bulb
                            \
                            \ x x x x
                            \ x x x x
                            \ x . . .
                            \ x x x x
                            \ x x x x
                            \ x . . .
                            \ x x x x
                            \ x x x x
    
    
    
    
           Name: MSBAR                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Draw a specific indicator in the dashboard's missile bar
    
    
        Context: See this subroutine [on its own page](../main/subroutine/msbar.md)
     Variations: See [code variations](../related/compare/main/subroutine/msbar.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ABORT2](../main/subroutine/abort2.md) calls MSBAR
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls MSBAR
                 * [msblob](../main/subroutine/msblob.md) calls MSBAR
    
    
    
    
    
    * * *
    
    
     Each indicator is a rectangle that's 3 pixels wide and 5 pixels high. If the
     indicator is set to black, this effectively removes a missile.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The number of the missile indicator to update (counting
                            from right to left, so indicator NOMSL is the leftmost
                            indicator)
    
       Y                    The colour of the missile indicator:
    
                              * &00 = black (no missile)
    
                              * #RED2 = red (armed and locked)
    
                              * #YELLOW2 = yellow/white (armed)
    
                              * #GREEN2 = green (disarmed)
    
    
    
    * * *
    
    
     Returns:
    
       X                    X is preserved
    
       Y                    Y is set to 0
    
    
    
    
    .MSBAR
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     TXA                    \ Store the value of X on the stack so we can preserve
     PHA                    \ it across the call to this subroutine
    
     ASL A                  \ Set T = A * 16
     ASL A
     ASL A
     ASL A
     STA T
    
     LDA #97                \ Set SC = 97 - T
     SBC T                  \        = 96 + 1 - (X * 16)
     STA SC
    
                            \ So the low byte of SC(1 0) contains the row address
                            \ for the rightmost missile indicator, made up as
                            \ follows:
                            \
                            \   * 96 (character block 14, as byte #14 * 8 = 96), the
                            \     character block of the rightmost missile
                            \
                            \   * 1 (so we start drawing on the second row of the
                            \     character block)
                            \
                            \   * Move left one character (8 bytes) for each count
                            \     of X, so when X = 0 we are drawing the rightmost
                            \     missile, for X = 1 we hop to the left by one
                            \     character, and so on
    
     LDA #&7C               \ Set the high byte of SC(1 0) to &7C, the character row
     STA SCH                \ that contains the missile indicators (i.e. the bottom
                            \ row of the screen)
    
     TYA                    \ Set A to the correct colour, which is a two-pixel wide
                            \ mode 2 character row byte in the specified colour
    
     LDY #5                 \ We now want to draw this line five times to do the
                            \ left two pixels of the indicator, so set a counter in
                            \ Y
    
    .MBL1
    
     STA (SC),Y             \ Draw the three-pixel row, and as we do not use EOR
                            \ logic, this will overwrite anything that is already
                            \ there (so drawing a black missile will delete what's
                            \ there)
    
     DEY                    \ Decrement the counter for the next row
    
     BNE MBL1               \ Loop back to MBL1 if have more rows to draw
    
     PHA                    \ Store the value of A on the stack so we can retrieve
                            \ it after the following addition
    
     LDA SC                 \ Set SC = SC + 8
     CLC                    \
     ADC #8                 \ so SC(1 0) now points to the next character block on
     STA SC                 \ the row (for the right half of the indicator)
    
     PLA                    \ Retrieve A from the stack
    
     AND #%10101010         \ Mask the character row to plot just the first pixel
                            \ in the next character block, as we already did a
                            \ two-pixel wide band in the previous character block,
                            \ so we need to plot just one more pixel, width-wise
    
     LDY #5                 \ We now want to draw this line five times, so set a
                            \ counter in Y
    
    .MBL2
    
     STA (SC),Y             \ Draw the one-pixel row, and as we do not use EOR
                            \ logic, this will overwrite anything that is already
                            \ there (so drawing a black missile will delete what's
                            \ there)
    
     DEY                    \ Decrement the counter for the next row
    
     BNE MBL2               \ Loop back to MBL2 if have more rows to draw
    
     PLX                    \ Restore X from the stack, so that it's preserved
    
    IF _SNG47
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    ELIF _COMPACT
    
     JMP away               \ Jump to away to switch main memory back into
                            \ &3000-&7FFF and return from the subroutine
    
    ENDIF
    
    
    
    
           Name: HANGFLAG                                                [Show more]
           Type: Variable
       Category: Ship hangar
        Summary: The number of ships being displayed in the ship hangar
    
    
        Context: See this variable [on its own page](../main/variable/hangflag.md)
     References: This variable is used as follows:
                 * [HALL](../main/subroutine/hall.md) uses HANGFLAG
                 * [HANGER](../main/subroutine/hanger.md) uses HANGFLAG
    
    
    
    
    
    
    .HANGFLAG
    
     EQUB 0
    
    
    
    
           Name: HANGER                                                  [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Display the ship hangar
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hanger.md)
     Variations: See [code variations](../related/compare/main/subroutine/hanger.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HALL](../main/subroutine/hall.md) calls HANGER
                 * [HAL3](../main/subroutine/hal3.md) calls via HA3
                 * [HAS2](../main/subroutine/has2.md) calls via HA3
                 * [HAS3](../main/subroutine/has3.md) calls via HA3
    
    
    
    
    
    * * *
    
    
     This routine is called after the ships in the hangar have been drawn, so all
     it has to do is draw the hangar's background.
    
     The hangar background is made up of two parts:
    
       * The hangar floor consists of 11 screen-wide horizontal lines, which start
         out quite spaced out near the bottom of the screen, and bunch ever closer
         together as the eye moves up towards the horizon, where they merge to give
         a sense of perspective
    
       * The back wall of the hangar consists of 15 equally spaced vertical lines
         that join the horizon to the top of the screen
    
     The ships in the hangar have already been drawn by this point, so the lines
     are drawn so they don't overlap anything that's already there, which makes
     them look like they are behind and below the ships. This is achieved by
     drawing the lines in from the screen edges until they bump into something
     already on-screen. For the horizontal lines, when there are multiple ships in
     the hangar, this also means drawing lines between the ships, as well as in
     from each side.
    
    
    
    * * *
    
    
     Other entry points:
    
       HA3                  Contains an RTS
    
    
    
    
    .HANGER
    
                            \ We start by drawing the floor
    
     LDX #2                 \ We start with a loop using a counter in T that goes
                            \ from 2 to 12, one for each of the 11 horizontal lines
                            \ in the floor, so set the initial value in X
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
    .HAL1
    
     STX T                  \ Store the loop counter in T
    
     LDA #130               \ Set A = 130
    
     STX Q                  \ Set Q to the value of the loop counter
    
     JSR DVID4K             \ Calculate the following:
                            \
                            \   (P R) = 256 * A / Q
                            \         = 256 * 130 / Q
                            \
                            \ so P = 130 / Q, and as the counter Q goes from 2 to
                            \ 12, P goes 65, 43, 32 ... 13, 11, 10, with the
                            \ difference between two consecutive numbers getting
                            \ smaller as P gets smaller
                            \
                            \ We can use this value as a y-coordinate to draw a set
                            \ of horizontal lines, spaced out near the bottom of the
                            \ screen (high value of P, high y-coordinate, lower down
                            \ the screen) and bunching up towards the horizon (low
                            \ value of P, low y-coordinate, higher up the screen)
    
     LDA P                  \ Set Y = #Y + P
     CLC                    \
     ADC #Y                 \ where #Y is the y-coordinate of the centre of the
     TAY                    \ screen, so Y is now the horizontal pixel row of the
                            \ line we want to draw to display the hangar floor
    
     LDA ylookup,Y          \ Look up the page number of the character row that
     STA SC+1               \ contains the pixel with the y-coordinate in Y, and
                            \ store it in the high byte of SC(1 0) at SC+1
    
     STA R                  \ Also store the page number in R
    
     LDA P                  \ Set the low byte of SC(1 0) to the y-coordinate mod 8,
     AND #7                 \ which determines the pixel row in the character block
     STA SC                 \ we need to draw in (as each character row is 8 pixels
                            \ high), so SC(1 0) now points to the address of the
                            \ start of the horizontal line we want to draw
    
     LDY #0                 \ Set Y = 0 so the call to HAS2 starts drawing the line
                            \ in the first byte of the screen row, at the left edge
                            \ of the screen
    
     JSR HAS2               \ Draw a horizontal line from the left edge of the
                            \ screen, going right until we bump into something
                            \ already on-screen, at which point stop drawing
    
     LDY R                  \ Fetch the page number of the line from R, increment it
     INY                    \ so it points to the right half of the character row
     STY SC+1               \ (as each row takes up 2 pages), and store it in the
                            \ high byte of SC(1 0) at SC+1
    
     LDA #%01000000         \ Now to draw the same line but from the right edge of
                            \ the screen, so set a pixel mask in A to check the
                            \ second pixel of the last byte, so we skip the
                            \ two-pixel screen border at the right edge of the
                            \ screen
    
     LDY #248               \ Set Y = 248 so the call to HAS3 starts drawing the
                            \ line in the last byte of the screen row, at the right
                            \ edge of the screen
    
     JSR HAS3               \ Draw a horizontal line from the right edge of the
                            \ screen, going left until we bump into something
                            \ already on-screen, at which point stop drawing
    
     LDY HANGFLAG           \ Fetch the value of HANGFLAG, which gets set to 0 in
                            \ the HALL routine above if there is only one ship
    
     BEQ HA2                \ If HANGFLAG is zero, jump to HA2 to skip the following
                            \ as there is only one ship in the hangar
    
                            \ If we get here then there are multiple ships in the
                            \ hangar, so we also need to draw the horizontal line in
                            \ the gap between the ships
    
     LDY #0                 \ First we draw the line from the centre of the screen
                            \ to the right. SC(1 0) points to the start address of
                            \ the second half of the screen row, so we set Y to 0 so
                            \ the call to HAL3 starts drawing from the first
                            \ character in that second half
    
     LDA #%10001000         \ We want to start drawing from the first pixel, so we
                            \ set a mask in A to the first pixel in the four-pixel
                            \ byte
    
     JSR HAL3               \ Call HAL3, which draws a line from the halfway point
                            \ across the right half of the screen, going right until
                            \ we bump into something already on-screen, at which
                            \ point it stops drawing
    
     DEC SC+1               \ Decrement the high byte of SC(1 0) in SC+1 to point to
                            \ the previous page (i.e. the left half of this screen
                            \ row)
    
     LDY #248               \ We now draw the line from the centre of the screen
                            \ to the left. SC(1 0) points to the start address of
                            \ the first half of the screen row, so we set Y to 248
                            \ so the call to HAS3 starts drawing from the last
                            \ character in that first half
    
     LDA #%00010000         \ We want to start drawing from the last pixel, so we
                            \ set a mask in A to the last pixel in the four-pixel
                            \ byte
    
     JSR HAS3               \ Call HAS3, which draws a line from the halfway point
                            \ across the left half of the screen, going left until
                            \ we bump into something already on-screen, at which
                            \ point it stops drawing
    
    .HA2
    
                            \ We have finished threading our horizontal line behind
                            \ the ships already on-screen, so now for the next line
    
     LDX T                  \ Fetch the loop counter from T and increment it
     INX
    
     CPX #13                \ If the loop counter is less than 13 (i.e. 2 to 12)
     BCC HAL1               \ then loop back to HAL1 to draw the next line
    
                            \ The floor is done, so now we move on to the back wall
    
     LDA #60                \ Set S = 60, so we run the following 60 times (though I
     STA S                  \ have no idea why it's 60 times, when it should be 15,
                            \ as this has the effect of drawing each vertical line
                            \ four times, each time starting one character row lower
                            \ on-screen)
    
     LDA #16                \ We want to draw 15 vertical lines, one every 16 pixels
                            \ across the screen, with the first at x-coordinate 16,
                            \ so set this in A to act as the x-coordinate of each
                            \ line as we work our way through them from left to
                            \ right, incrementing by 16 for each new line
    
     LDX #&40               \ Set X = &40, the high byte of the start of screen
     STX R                  \ memory (the screen starts at location &4000) and the
                            \ page number of the first screen row
    
    .HAL6
    
     LDX R                  \ Set the high byte of SC(1 0) to R
     STX SC+1
    
     STA T                  \ Store A in T so we can retrieve it later
    
     AND #%11111100         \ A contains the x-coordinate of the line to draw, and
     STA SC                 \ each character block is 4 pixels wide, so setting the
                            \ low byte of SC(1 0) to A mod 4 points SC(1 0) to the
                            \ correct character block on the top screen row for this
                            \ x-coordinate
    
     LDX #%10001000         \ Set a mask in X to the first pixel in the four-pixel
                            \ byte
    
     LDY #1                 \ We are going to start drawing the line from the second
                            \ pixel from the top (to avoid drawing on the one-pixel
                            \ border), so set Y to 1 to point to the second row in
                            \ the first character block
    
    .HAL7
    
     TXA                    \ Copy the pixel mask to A
    
     AND (SC),Y             \ If the pixel we want to draw is non-zero (using A as a
     BNE HA6                \ mask), then this means it already contains something,
                            \ so jump to HA6 to stop drawing this line
    
     TXA                    \ Copy the pixel mask to A again
    
     AND #RED               \ Apply the pixel mask in A to a four-pixel block of
                            \ red pixels, so we now know which bits to set in screen
                            \ memory
    
     ORA (SC),Y             \ OR the byte with the current contents of screen
                            \ memory, so the pixel we want is set to red (because
                            \ we know the bits are already 0 from the above test)
    
     STA (SC),Y             \ Store the updated pixel in screen memory
    
     INY                    \ Increment Y to point to the next row in the character
                            \ block, i.e. the next pixel down
    
     CPY #8                 \ Loop back to HAL7 to draw this next pixel until we
     BNE HAL7               \ have drawn all 8 in the character block
    
     INC SC+1               \ There are two pages of memory for each character row,
     INC SC+1               \ so we increment the high byte of SC(1 0) twice to
                            \ point to the same character but in the next row down
    
     LDY #0                 \ Set Y = 0 to point to the first row in this character
                            \ block
    
     BEQ HAL7               \ Loop back up to HAL7 to keep drawing the line (this
                            \ BEQ is effectively a JMP as Y is always zero)
    
    .HA6
    
     LDA T                  \ Fetch the x-coordinate of the line we just drew from T
     CLC                    \ into A, and add 16 so that A contains the x-coordinate
     ADC #16                \ of the next line to draw
    
     BCC P%+4               \ If the addition overflowed, increment the page number
     INC R                  \ in R to point to the second half of the screen row
    
     DEC S                  \ Decrement the loop counter in S
    
     BNE HAL6               \ Loop back to HAL6 until we have run through the loop
                            \ 60 times, by which point we are most definitely done
    
    IF _SNG47
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine (this instruction is not
                            \ needed as we could just fall through into the RTS at
                            \ HA3 below)
    
    ELIF _COMPACT
    
     JMP away               \ Jump to away to switch main memory back into
                            \ &3000-&7FFF and return from the subroutine
    
    ENDIF
    
    .HA3
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: HAS2                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw a hangar background line from left to right
    
    
        Context: See this subroutine [on its own page](../main/subroutine/has2.md)
     Variations: See [code variations](../related/compare/main/subroutine/has2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HANGER](../main/subroutine/hanger.md) calls HAS2
    
    
    
    
    
    * * *
    
    
     This routine draws a line to the right, starting with the third pixel of the
     pixel row at screen address SC(1 0), and aborting if we bump into something
     that's already on-screen.
    
     HAL2 draws from the left edge of the screen to the halfway point, and then
     HAL3 takes over to draw from the halfway point across the right half of the
     screen.
    
    
    
    
    .HAS2
    
     LDA #%00100010         \ Set A to the pixel pattern for a mode 1 character row
                            \ byte with the third pixel set, so we start drawing the
                            \ horizontal line just to the right of the two-pixel
                            \ border along the edge of the screen
    
    .HAL2
    
     TAX                    \ Store A in X so we can retrieve it after the following
                            \ check and again after updating screen memory
    
     AND (SC),Y             \ If the pixel we want to draw is non-zero (using A as a
     BNE HA3                \ mask), then this means it already contains something,
                            \ so we stop drawing because we have run into something
                            \ that's already on-screen, and return from the
                            \ subroutine (as HA3 contains an RTS)
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     AND #RED               \ Apply the pixel mask in A to a four-pixel block of
                            \ red pixels, so we now know which bits to set in screen
                            \ memory
    
     ORA (SC),Y             \ OR the byte with the current contents of screen
                            \ memory, so the pixel we want is set to red (because
                            \ we know the bits are already 0 from the above test)
    
     STA (SC),Y             \ Store the updated pixel in screen memory
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     LSR A                  \ Shift A to the right to move on to the next pixel
    
     BCC HAL2               \ If bit 0 before the shift was clear (i.e. we didn't
                            \ just do the fourth pixel in this block), loop back to
                            \ HAL2 to check and draw the next pixel
    
     TYA                    \ Set Y = Y + 8 (as we know the C flag is set) to point
     ADC #7                 \ to the next character block along
     TAY
    
     LDA #%10001000         \ Reset the pixel mask in A to the first pixel in the
                            \ new four-pixel character block
    
     BCC HAL2               \ If the above addition didn't overflow, jump back to
                            \ HAL2 to keep drawing the line in the next character
                            \ block
    
     INC SC+1               \ The addition overflowed, so we have reached the last
                            \ character block in this page of memory, so increment
                            \ the high byte of SC(1 0) in SC+1 to point to the next
                            \ page (i.e. the right half of this screen row) and fall
                            \ into HAL3 to repeat the performance
    
    
    
    
           Name: HAL3                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw a hangar background line from left to right, stopping when it
                 bumps into existing on-screen content
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hal3.md)
     References: This subroutine is called as follows:
                 * [HANGER](../main/subroutine/hanger.md) calls HAL3
    
    
    
    
    
    
    .HAL3
    
     TAX                    \ Store A in X so we can retrieve it after the following
                            \ check and again after updating screen memory
    
     AND (SC),Y             \ If the pixel we want to draw is non-zero (using A as a
     BNE HA3                \ mask), then this means it already contains something,
                            \ so we stop drawing because we have run into something
                            \ that's already on-screen, and return from the
                            \ subroutine (as HA3 contains an RTS)
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     AND #RED               \ Apply the pixel mask in A to a four-pixel block of
                            \ red pixels, so we now know which bits to set in screen
                            \ memory
    
     ORA (SC),Y             \ OR the byte with the current contents of screen
                            \ memory, so the pixel we want is set to red (because
                            \ we know the bits are already 0 from the above test)
    
     STA (SC),Y             \ Store the updated pixel in screen memory
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     LSR A                  \ Shift A to the right to move on to the next pixel
    
     BCC HAL3               \ If bit 0 before the shift was clear (i.e. we didn't
                            \ just do the fourth pixel in this block), loop back to
                            \ HAL3 to check and draw the next pixel
    
     TYA                    \ Set Y = Y + 8 (as we know the C flag is set) to point
     ADC #7                 \ to the next character block along
     TAY
    
     LDA #%10001000         \ Reset the pixel mask in A to the first pixel in the
                            \ new four-pixel character block
    
     BCC HAL3               \ If the above addition didn't overflow, jump back to
                            \ HAL3 to keep drawing the line in the next character
                            \ block
    
     RTS                    \ The addition overflowed, so we have reached the last
                            \ character block in this page of memory, which is the
                            \ end of the line, so we return from the subroutine
    
    
    
    
           Name: HAS3                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw a hangar background line from right to left
    
    
        Context: See this subroutine [on its own page](../main/subroutine/has3.md)
     Variations: See [code variations](../related/compare/main/subroutine/has3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HANGER](../main/subroutine/hanger.md) calls HAS3
    
    
    
    
    
    * * *
    
    
     This routine draws a line to the left, starting with the pixel mask in A at
     screen address SC(1 0) and character block offset Y, and aborting if we bump
     into something that's already on-screen.
    
    
    
    
    .HAS3
    
     TAX                    \ Store A in X so we can retrieve it after the following
                            \ check and again after updating screen memory
    
     AND (SC),Y             \ If the pixel we want to draw is non-zero (using A as a
     BNE HA3                \ mask), then this means it already contains something,
                            \ so we stop drawing because we have run into something
                            \ that's already on-screen, and return from the
                            \ subroutine (as HA3 contains an RTS)
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     ORA (SC),Y             \ OR the byte with the current contents of screen
                            \ memory, so the pixel we want is set to red (because
                            \ we know the bits are already 0 from the above test)
    
     STA (SC),Y             \ Store the updated pixel in screen memory
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     ASL A                  \ Shift A to the left to move to the next pixel to the
                            \ left
    
     BCC HAS3               \ If bit 7 before the shift was clear (i.e. we didn't
                            \ just do the first pixel in this block), loop back to
                            \ HAS3 to check and draw the next pixel to the left
    
     TYA                    \ Set Y = Y - 8 (as we know the C flag is set) to point
     SBC #8                 \ to the next character block to the left
     TAY
    
     LDA #%00010000         \ Set a mask in A to the last pixel in the four-pixel
                            \ byte
    
     BCS HAS3               \ If the above subtraction didn't underflow, jump back
                            \ to HAS3 to keep drawing the line in the next character
                            \ block to the left
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DVID4K                                                  [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (P R) = 256 * A / Q
      Deep dive: [Shift-and-subtract division](https://elite.bbcelite.com/deep_dives/shift-and-subtract_division.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dvid4k.md)
     References: This subroutine is called as follows:
                 * [HANGER](../main/subroutine/hanger.md) calls DVID4K
    
    
    
    
    
    * * *
    
    
     Calculate the following division and remainder:
    
       P = A / Q
    
       R = remainder as a fraction of Q, where 1.0 = 255
    
     Another way of saying the above is this:
    
       (P R) = 256 * A / Q
    
     This uses the same shift-and-subtract algorithm as TIS2, but this time we
     keep the remainder.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .DVID4K
    
                            \ This is an exact duplicate of the DVID4 routine, which
                            \ is also present in this source, so it isn't clear why
                            \ this duplicate exists (especially as the other version
                            \ is slightly faster, as it unrolls the loop)
    
     LDX #8                 \ Set a counter in X to count the 8 bits in A
    
     ASL A                  \ Shift A left and store in P (we will build the result
     STA P                  \ in P)
    
     LDA #0                 \ Set A = 0 for us to build a remainder
    
    .DVL4K
    
     ROL A                  \ Shift A to the left
    
     BCS DV8K               \ If the C flag is set (i.e. bit 7 of A was set) then
                            \ skip straight to the subtraction
    
     CMP Q                  \ If A < Q skip the following subtraction
     BCC DV5K
    
    .DV8K
    
     SBC Q                  \ A >= Q, so set A = A - Q
    
    .DV5K
    
     ROL P                  \ Shift P to the left, pulling the C flag into bit 0
    
     DEX                    \ Decrement the loop counter
    
     BNE DVL4K              \ Loop back for the next bit until we have done all 8
                            \ bits of P
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: cls                                                     [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the top part of the screen and draw a border box
    
    
        Context: See this subroutine [on its own page](../main/subroutine/cls.md)
     References: This subroutine is called as follows:
                 * [CHPR](../main/subroutine/chpr.md) calls cls
    
    
    
    
    
    
    .cls
    
     JSR TTX66              \ Call TTX66 to clear the top part of the screen and
                            \ draw a border box
    
     JMP RR4                \ Jump to RR4 to restore X and Y from the stack and A
                            \ from K3, and return from the subroutine using a tail
                            \ call
    
    
    
    
           Name: TT67K                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a newline
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt67k.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt67-tt67k.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CLYNS](../main/subroutine/clyns.md) calls TT67K
                 * [STATUS](../main/subroutine/status.md) calls TT67K
    
    
    
    
    
    
    .TT67K
    
                            \ This does the same as the existing TT67 routine, which
                            \ is also present in this source, so it isn't clear why
                            \ this duplicate exists
                            \
                            \ In the original source this label is TT67, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to TT67K
    
     LDA #12                \ Set A to a carriage return character
    
                            \ Fall through into TT26 to print the newline
    
    
    
    
           Name: CHPR                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a character at the text cursor by poking into screen memory
      Deep dive: [Drawing text](https://elite.bbcelite.com/deep_dives/drawing_text.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/chpr.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt26-chpr.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BELL](../main/subroutine/bell.md) calls CHPR
                 * [COLD](../main/subroutine/cold.md) calls CHPR
                 * [GTDRV](../main/subroutine/gtdrv.md) calls CHPR
                 * [MT26](../main/subroutine/mt26.md) calls CHPR
                 * [NEWBRK](../main/subroutine/newbrk.md) calls CHPR
                 * [NUMBOR](../main/subroutine/numbor.md) calls CHPR
                 * [TT26](../main/subroutine/tt26.md) calls CHPR
                 * [cls](../main/subroutine/cls.md) calls via RR4
    
    
    
    
    
    * * *
    
    
     Print a character at the text cursor (XC, YC), do a beep, print a newline,
     or delete left (backspace).
    
     Calls to OSWRCH will end up here when A is not in the range 128-147, as those
     are reserved for the special jump table OSWRCH commands.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be printed. Can be one of the
                            following:
    
                              * 7 (beep)
    
                              * 10 (line feed)
    
                              * 11 (clear the top part of the screen and draw a
                                border)
    
                              * 12-13 (carriage return)
    
                              * 32-95 (ASCII capital letters, numbers and
                                punctuation)
    
                              * 127 (delete the character to the left of the text
                                cursor and move the cursor to the left)
    
       XC                   Contains the text column to print at (the x-coordinate)
    
       YC                   Contains the line number to print on (the y-coordinate)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       X                    X is preserved
    
       Y                    Y is preserved
    
       C flag               The C flag is cleared
    
    
    
    * * *
    
    
     Other entry points:
    
       RR4                  Restore the registers and return from the subroutine
    
    
    
    
    .CHPR
    
     STA K3                 \ Store the A, X and Y registers, so we can restore
     PHY                    \ them at the end (so they don't get changed by this
     PHX                    \ routine)
    
     LDY QQ17               \ Load the QQ17 flag, which contains the text printing
                            \ flags
    
     CPY #255               \ If QQ17 = 255 then printing is disabled, so jump to
     BEQ RR4S               \ RR4 (via the JMP in RR4S) to restore the registers
                            \ and return from the subroutine using a tail call
    
    IF _COMPACT
    
     TAY                    \ Copy the character to be printed from A into Y
    
     BEQ RR4S               \ If the character to be printed is 0 or >= 128, jump to
     BMI RR4S               \ RR4S (via the JMP in RR4S) to restore the registers
                            \ and return from the subroutine using a tail call
    
    .RRNEW
    
    ENDIF
    
     LDY #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     TAY                    \ Set Y = the character to be printed
    
    IF _SNG47
    
     BEQ RR4S               \ If the character is zero, which is typically a string
                            \ terminator character, jump down to RR4 (via the JMP in
                            \ RR4S) to restore the registers and return from the
                            \ subroutine using a tail call
    
     BMI RR4S               \ If A > 127 then there is nothing to print, so jump to
                            \ RR4 (via the JMP in RR4S) to restore the registers and
                            \ return from the subroutine
    
    ELIF _COMPACT
    
     LDX CATF               \ If CATF <> 0, skip the following two instructions, as
     BNE P%+6               \ we are printing a disc catalogue and we don't want any
                            \ control characters lurking in the catalogue to trigger
                            \ the screen clearing routine
    
    ENDIF
    
     CMP #11                \ If this is control code 11 (clear screen), jump to cls
     BEQ cls                \ to clear the top part of the screen, draw a white
                            \ border and return from the subroutine via RR4
    
     CMP #7                 \ If this is not control code 7 (beep), skip the next
     BNE P%+5               \ instruction
    
     JMP R5                 \ This is control code 7 (beep), so jump to R5 to make
                            \ a beep and return from the subroutine via RR4
    
     CMP #32                \ If this is an ASCII character (A >= 32), jump to RR1
     BCS RR1                \ below, which will print the character, restore the
                            \ registers and return from the subroutine
    
     CMP #10                \ If this is control code 10 (line feed) then jump to
     BEQ RRX1               \ RRX1, which will move down a line, restore the
                            \ registers and return from the subroutine
    
     LDX #1                 \ If we get here, then this is control code 12 or 13,
     STX XC                 \ both of which are used. This code prints a newline,
                            \ which we can achieve by moving the text cursor
                            \ to the start of the line (carriage return) and down
                            \ one line (line feed). These two lines do the first
                            \ bit by setting XC = 1, and we then fall through into
                            \ the line feed routine that's used by control code 10
    
    .RRX1
    
     CMP #13                \ If this is control code 13 (carriage return) then jump
     BEQ RR4S               \ to RR4 (via the JMP in RR4S) to restore the registers
                            \ and return from the subroutine using a tail call
    
     INC YC                 \ Increment the text cursor y-coordinate to move it
                            \ down one row
    
    .RR4S
    
     JMP RR4                \ Jump to RR4 to restore the registers and return from
                            \ the subroutine using a tail call
    
    .RR1
    
                            \ If we get here, then the character to print is an
                            \ ASCII character in the range 32-95. The quickest way
                            \ to display text on-screen is to poke the character
                            \ pixel by pixel, directly into screen memory, so
                            \ that's what the rest of this routine does
                            \
                            \ The first step, then, is to get hold of the bitmap
                            \ definition for the character we want to draw on the
                            \ screen (i.e. we need the pixel shape of this
                            \ character). The MOS ROM contains bitmap definitions
                            \ of the system's ASCII characters, starting from &C000
                            \ for space (ASCII 32) and ending with the £ symbol
                            \ (ASCII 126)
                            \
                            \ There are definitions for 32 characters in each of the
                            \ three pages of MOS memory, as each definition takes up
                            \ 8 bytes (8 rows of 8 pixels) and 32 * 8 = 256 bytes =
                            \ 1 page. So:
                            \
                            \   ASCII 32-63  are defined in &C000-&C0FF (page 0)
                            \   ASCII 64-95  are defined in &C100-&C1FF (page 1)
                            \   ASCII 96-126 are defined in &C200-&C2F0 (page 2)
                            \
                            \ The following code reads the relevant character
                            \ those values into the correct position in screen
                            \ memory, thus printing the character on-screen
                            \
                            \ It's a long way from 10 PRINT "Hello world!":GOTO 10
    
                            \ Now we want to set X to point to the relevant page
    
                            \ The following logic is easier to follow if we look
                            \ at the three character number ranges in binary:
                            \
                            \   Bit #  76543210
                            \
                            \   32  = %00100000     Page 0 of bitmap definitions
                            \   63  = %00111111
                            \
                            \   64  = %01000000     Page 1 of bitmap definitions
                            \   95  = %01011111
                            \
                            \   96  = %01100000     Page 2 of bitmap definitions
                            \   125 = %01111101
                            \
                            \ We'll refer to this below
    
     LDX #(FONT%-1)         \ Set X to point to the page before the first font page,
                            \ which is FONT% - 1
    
     ASL A                  \ If bit 6 of the character is clear (A is 32-63)
     ASL A                  \ then skip the following instruction
     BCC P%+4
    
     LDX #(FONT%+1)         \ A is 64-126, so set X to point to page FONT% + 1
    
     ASL A                  \ If bit 5 of the character is clear (A is 64-95)
     BCC P%+3               \ then skip the following instruction
    
     INX                    \ Increment X
                            \
                            \ In other words, X points to the relevant page. But
                            \ what about the value of A? That gets shifted to the
                            \ left three times during the above code, which
                            \ multiplies the number by 8 but also drops bits 7, 6
                            \ and 5 in the process. Look at the above binary
                            \ figures and you can see that if we cleared bits 5-7,
                            \ then that would change 32-53 to 0-31... but it would
                            \ do exactly the same to 64-95 and 96-125. And because
                            \ we also multiply this figure by 8, A now points to
                            \ the start of the character's definition within its
                            \ page (because there are 8 bytes per character
                            \ definition)
                            \
                            \ Or, to put it another way, X contains the high byte
                            \ (the page) of the address of the definition that we
                            \ want, while A contains the low byte (the offset into
                            \ the page) of the address
    
     STA P                  \ Store the address of this character's definition in
     STX P+1                \ P(1 0)
    
     LDA XC                 \ Fetch XC, the x-coordinate (column) of the text cursor
                            \ into A
    
     LDX CATF               \ If CATF = 0, jump to RR5, otherwise we are printing a
     BEQ RR5                \ disc catalogue
    
    IF _SNG47
    
     CPY #' '               \ If the character we want to print in Y is a space,
     BNE RR5                \ jump to RR5
    
                            \ If we get here, then CATF is non-zero, so we are
                            \ printing a disc catalogue and we are not printing a
                            \ space, so we drop column 17 from the output so the
                            \ catalogue will fit on-screen (column 17 is a blank
                            \ column in the middle of the catalogue, between the
                            \ two lists of filenames, so it can be dropped without
                            \ affecting the layout). Without this, the catalogue
                            \ would be one character too wide for the square screen
                            \ mode (it's 34 characters wide, while the screen mode
                            \ is only 33 characters across)
    
     CMP #17                \ If A = 17, i.e. the text cursor is in column 17, jump
     BEQ RR4                \ to RR4 to restore the registers and return from the
                            \ subroutine, thus omitting this column
    
    ELIF _COMPACT
    
     CMP #21                \ If A < 21, i.e. the text cursor is in column 0-20,
     BCC RR5                \ jump to RR5 to skip the following
    
                            \ If we get here, then CATF is non-zero, so we are
                            \ printing a disc catalogue and we have reached column
                            \ 21, so we move to the start of the next line so the
                            \ catalogue line-wraps to fit within the bounds of the
                            \ screen
    
     INC YC                 \ More the text cursor down a line
    
     LDA #1                 \ Move the text cursor to column 1
     STA XC
    
    ENDIF
    
    .RR5
    
     ASL A                  \ Multiply A by 8, and store in SC, so we now have:
     ASL A                  \
     ASL A                  \   SC = XC * 8
     STA SC
    
     LDA YC                 \ Fetch YC, the y-coordinate (row) of the text cursor
    
     CPY #127               \ If the character number (which is in Y) <> 127, then
     BNE RR2                \ skip to RR2 to print that character, otherwise this is
                            \ the delete character, so continue on
    
     DEC XC                 \ We want to delete the character to the left of the
                            \ text cursor and move the cursor back one, so let's
                            \ do that by decrementing YC. Note that this doesn't
                            \ have anything to do with the actual deletion below,
                            \ we're just updating the cursor so it's in the right
                            \ position following the deletion
    
     ASL A                  \ A contains YC (from above), so this sets A = YC * 2
    
     ASL SC                 \ Double the low byte of SC(1 0), catching bit 7 in the
                            \ C flag. As each character is 8 pixels wide, and the
                            \ special screen mode Elite uses for the top part of the
                            \ screen is 256 pixels across with two bits per pixel,
                            \ this value is not only double the screen address
                            \ offset of the text cursor from the left side of the
                            \ screen, it's also the least significant byte of the
                            \ screen address where we want to print this character,
                            \ as each row of on-screen pixels corresponds to two
                            \ pages. To put this more explicitly, the screen starts
                            \ at &4000, so the text rows are stored in screen
                            \ memory like this:
                            \
                            \   Row 1: &4000 - &41FF    YC = 1, XC = 0 to 31
                            \   Row 2: &4200 - &43FF    YC = 2, XC = 0 to 31
                            \   Row 3: &4400 - &45FF    YC = 3, XC = 0 to 31
                            \
                            \ and so on
    
     ADC #&3F               \ Set X = A
     TAX                    \       = A + &3F + C
                            \       = YC * 2 + &3F + C
    
                            \ Because YC starts at 0 for the first text row, this
                            \ means that X will be &3F for row 0, &41 for row 1 and
                            \ so on. In other words, X is now set to the page number
                            \ for the row before the one containing the text cursor,
                            \ and given that we set SC above to point to the offset
                            \ in memory of the text cursor within the row's page,
                            \ this means that (X SC) now points to the character
                            \ above the text cursor
    
     LDY #&F0               \ Set Y = &F0, so the following call to ZES2 will count
                            \ Y upwards from &F0 to &FF
    
     JSR ZES2               \ Call ZES2, which zero-fills from address (X SC) + Y to
                            \ (X SC) + &FF. (X SC) points to the character above the
                            \ text cursor, and adding &FF to this would point to the
                            \ cursor, so adding &F0 points to the character before
                            \ the cursor, which is the one we want to delete. So
                            \ this call zero-fills the character to the left of the
                            \ cursor, which erases it from the screen
    
     BEQ RR4                \ We are done deleting, so restore the registers and
                            \ return from the subroutine (this BNE is effectively
                            \ a JMP as ZES2 always returns with the Z flag set)
    
    .RR2
    
                            \ Now to actually print the character
    
     INC XC                 \ Once we print the character, we want to move the text
                            \ cursor to the right, so we do this by incrementing
                            \ XC. Note that this doesn't have anything to do
                            \ with the actual printing below, we're just updating
                            \ the cursor so it's in the right position following
                            \ the print
    
     CMP #24                \ If the text cursor is on the screen (i.e. YC < 24, so
     BCC RR3                \ we are on rows 0-23), then jump to RR3 to print the
                            \ character
    
    IF _SNG47
    
     JSR TTX66              \ Otherwise we are off the bottom of the screen, so
                            \ clear the screen and draw a border box
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
    ELIF _COMPACT
    
     LDA CATF               \ If CATF = 0, skip the next two instructions, as we are
     BEQ P%+7               \ not printing a disc catalogue
    
     JSR RETURN             \ We have just printed the disc catalogue, so wait until
     BPL P%-3               \ RETURN is pressed, looping indefinitely until it gets
                            \ tapped
    
     JSR TTX66              \ Call TTX66 to clear the screen
    
    ENDIF
    
     LDA #1                 \ Move the text cursor to column 1, row 1
     STA XC
     STA YC
    
     LDA K3                 \ Set A to the character to be printed, though again
                            \ this has no effect, as the following call to RR4 does
                            \ the exact same thing
    
    IF _SNG47
    
     JMP RR4                \ And restore the registers and return from the
                            \ subroutine
    
    ELIF _COMPACT
    
     JMP RRNEW              \ Jump back to RRNEW to print the character
    
    ENDIF
    
    .RR3
    
                            \ A contains the value of YC - the screen row where we
                            \ want to print this character - so now we need to
                            \ convert this into a screen address, so we can poke
                            \ the character data to the right place in screen
                            \ memory
    
     ASL A                  \ Set A = 2 * A
                            \       = 2 * YC
    
     ASL SC                 \ Back in RR5 we set SC = XC * 8, so this does the
                            \ following:
                            \
                            \   SC = SC * 2
                            \      = XC * 16
                            \
                            \ so SC contains the low byte of the screen address we
                            \ want to poke the character into, as each text
                            \ character is 8 pixels wide, and there are four pixels
                            \ per byte, so the offset within the row's 512 bytes
                            \ is XC * 8 pixels * 2 bytes for each 8 pixels = XC * 16
    
     ADC #&40               \ Set A = &40 + A
                            \       = &40 + (2 * YC)
                            \
                            \ so A contains the high byte of the screen address we
                            \ want to poke the character into, as screen memory
                            \ starts at &4000 (page &40) and each screen row takes
                            \ up 2 pages (512 bytes)
    
    .RREN
    
     STA SC+1               \ Store the page number of the destination screen
                            \ location in SC+1, so SC now points to the full screen
                            \ location where this character should go
    
     LDA SC                 \ Set P(3 2) = SC(1 0) + 8
     CLC                    \
     ADC #8                 \ starting with the low bytes
     STA P+2
    
     LDA SC+1               \ And then adding the high bytes, so P(3 2) points to
     STA P+3                \ the character block after the one pointed to by
                            \ SC(1 0)
    
     LDY #7                 \ We want to print the 8 bytes of character data to the
                            \ screen (one byte per row), so set up a counter in Y
                            \ to count these bytes
    
    .RRL1
    
                            \ We print the character's eight-pixel row in two parts,
                            \ starting with the first four pixels (one byte of
                            \ screen memory), and then the second four (a second
                            \ byte of screen memory)
    
     LDA (P),Y              \ The character definition is at P(1 0) - we set this up
                            \ above - so load the Y-th byte from P(1 0), which will
                            \ contain the bitmap for the Y-th row of the character
    
     AND #%11110000         \ Extract the high nibble of the character definition
                            \ byte, so the first four pixels on this row of the
                            \ character are in the first nibble, i.e. xxxx 0000
                            \ where xxxx is the pattern of those four pixels in the
                            \ character
    
     STA W                  \ Set A = (A >> 4) OR A
     LSR A                  \
     LSR A                  \ which duplicates the high nibble into the low nibble
     LSR A                  \ to give xxxx xxxx
     LSR A
     ORA W
    
     AND COL                \ AND with the colour byte so that the pixels take on
                            \ the colour we want to draw (i.e. A is acting as a mask
                            \ on the colour byte)
    
     EOR (SC),Y             \ If we EOR this value with the existing screen
                            \ contents, then it's reversible (so reprinting the
                            \ same character in the same place will revert the
                            \ screen to what it looked like before we printed
                            \ anything); this means that printing a white pixel
                            \ onto a white background results in a black pixel, but
                            \ that's a small price to pay for easily erasable text
    
     STA (SC),Y             \ Store the Y-th byte at the screen address for this
                            \ character location
    
                            \ We now repeat the process for the second batch of four
                            \ pixels in this character row
    
     LDA (P),Y              \ Fetch the bitmap for the Y-th row of the character
                            \ again
    
     AND #%00001111         \ This time we extract the low nibble of the character
                            \ definition, to get 0000 xxxx
    
     STA W                  \ Set A = (A << 4) OR A
     ASL A                  \
     ASL A                  \ which duplicates the low nibble into the high nibble
     ASL A                  \ to give xxxx xxxx
     ASL A
     ORA W
    
     AND COL                \ AND with the colour byte so that the pixels take on
                            \ the colour we want to draw (i.e. A is acting as a mask
                            \ on the colour byte)
    
     EOR (P+2),Y            \ EOR this value with the existing screen contents of
                            \ P(3 2), which is equal to SC(1 0) + 8, the next four
                            \ pixels along from the first four pixels we just
                            \ plotted in SC(1 0)
    
     STA (P+2),Y            \ Store the Y-th byte at the screen address for this
                            \ character location
    
     DEY                    \ Decrement the loop counter
    
     BPL RRL1               \ Loop back for the next byte to print to the screen
    
    .RR4
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     PLX                    \ We're done printing, so restore the values of the
     PLY                    \ A, X and Y registers that we saved above and clear the
     LDA K3                 \ C flag, so everything is back to how it was
     CLC
    
     RTS                    \ Return from the subroutine
    
    .R5
    
     JSR BEEP               \ Call the BEEP subroutine to make a short, high beep
    
     JMP RR4                \ Jump to RR4 to restore the registers and return from
                            \ the subroutine using a tail call
    
    
    
    
           Name: TTX66                                                   [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the top part of the screen and draw a border box
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ttx66.md)
     Variations: See [code variations](../related/compare/main/subroutine/ttx66.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CHPR](../main/subroutine/chpr.md) calls TTX66
                 * [cls](../main/subroutine/cls.md) calls TTX66
                 * [TT18](../main/subroutine/tt18.md) calls TTX66
                 * [TTX66K](../main/subroutine/ttx66k.md) calls TTX66
                 * [DEATH](../main/subroutine/death.md) calls via BOX
    
    
    
    
    
    * * *
    
    
     Clear the top part of the screen (the space view) and draw a border box
     along the top and sides.
    
    
    
    * * *
    
    
     Other entry points:
    
       BOX                  Just draw the border box along the top and sides
    
    
    
    
    .TTX66
    
     LDX #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STX VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDX #&40               \ Set X to point to page &40, which is the start of the
                            \ screen memory at &4000
    
    .BOL1
    
     JSR ZES1               \ Call ZES1 to zero-fill the page in X, which will clear
                            \ half a character row
    
     INX                    \ Increment X to point to the next page in screen
                            \ memory
    
     CPX #&70               \ Loop back to keep clearing character rows until we
     BNE BOL1               \ have cleared up to &7000, which is where the dashboard
                            \ starts
    
    .BOX
    
     LDX #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STX VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA COL                \ Store the current colour on the stack, so we can
     PHA                    \ restore it once we have drawn the border
    
     LDA #%00001111         \ Set COL = %00001111 to act as a four-pixel yellow
     STA COL                \ character byte (i.e. set the line colour to yellow)
    
     LDY #1                 \ Move the text cursor to row 1
     STY YC
    
     STY XC                 \ Move the text cursor to column 1
    
    IF _SNG47
    
     LDX #0                 \ Set X1 = Y1 = Y2 = 0
     STX Y1
     STX Y2
     STX X1
    
     DEX                    \ Set X2 = 255
     STX X2
    
    ELIF _COMPACT
    
     STZ Y1                 \ Set X1 = Y1 = Y2 = 0
     STZ Y2
     STZ X1
    
     LDX #255               \ Set X2 = 255
     STX X2
    
    ENDIF
    
     JSR LOINQ              \ Draw a line from (X1, Y1) to (X2, Y2), so that's from
                            \ (0, 0) to (255, 0), along the very top of the screen
    
     LDA #2                 \ Set X1 = X2 = 2
     STA X1
     STA X2
    
     JSR BOS2               \ Call BOS2 below, which will call BOS1 twice, and then
     JSR BOS2               \ call BOS2 again, so we effectively do BOS1 four times,
                            \ decrementing X1 and X2 each time before calling LOIN,
                            \ so this whole loop-within-a-loop mind-bender ends up
                            \ drawing these four lines:
                            \
                            \   (1, 0)   to (1, 191)
                            \   (0, 0)   to (0, 191)
                            \   (255, 0) to (255, 191)
                            \   (254, 0) to (254, 191)
                            \
                            \ So that's a two-pixel wide vertical border along the
                            \ left edge of the upper part of the screen, and a
                            \ two-pixel wide vertical border along the right edge
    
     LDA COL                \ Set locations &4000 and &41F8 to the correct colour,
     STA &4000              \ as otherwise the top-left and top-right corners will
     STA &41F8              \ be black (as the lines overlap at the corners, and
                            \ the EOR logic used by LOINQ will otherwise make them
                            \ black)
    
     PLA                    \ Restore the original colour that we stored above
     STA COL
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    .BOS2
    
     JSR BOS1               \ Call BOS1 below and then fall through into it, which
                            \ ends up running BOS1 twice. This is all part of the
                            \ loop-the-loop border-drawing mind-bender explained
                            \ above
    
    .BOS1
    
     STZ Y1                 \ Set Y1 = 0
    
     LDA #2*Y-1             \ Set Y2 = 2 * #Y - 1. The constant #Y is 96, the
     STA Y2                 \ y-coordinate of the mid-point of the space view, so
                            \ this sets Y2 to 191, the y-coordinate of the bottom
                            \ pixel row of the space view
    
     DEC X1                 \ Decrement X1 and X2
     DEC X2
    
     JMP LOINQ              \ Draw a line from (X1, Y1) to (X2, Y2) and return from
                            \ the subroutine using a tail call
    
    
    
    
           Name: ZES1                                                    [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Zero-fill the page whose number is in X
    
    
        Context: See this subroutine [on its own page](../main/subroutine/zes1.md)
     Variations: See [code variations](../related/compare/main/subroutine/zes1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TTX66](../main/subroutine/ttx66.md) calls ZES1
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The page we want to zero-fill
    
    
    
    
    .ZES1
    
     LDY #0                 \ If we set Y = SC = 0 and fall through into ZES2
     STY SC                 \ below, then we will zero-fill 255 bytes starting from
                            \ SC - in other words, we will zero-fill the whole of
                            \ page X
    
    
    
    
           Name: ZES2                                                    [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Zero-fill a specific page
    
    
        Context: See this subroutine [on its own page](../main/subroutine/zes2.md)
     Variations: See [code variations](../related/compare/main/subroutine/zes2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CHPR](../main/subroutine/chpr.md) calls ZES2
    
    
    
    
    
    * * *
    
    
     Zero-fill from address (X SC) + Y to (X SC) + &FF.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The high byte (i.e. the page) of the starting point of
                            the zero-fill
    
       Y                    The offset from (X SC) where we start zeroing, counting
                            up to &FF
    
       SC                   The low byte (i.e. the offset into the page) of the
                            starting point of the zero-fill
    
    
    
    * * *
    
    
     Returns:
    
       Z flag               Z flag is set
    
    
    
    
    .ZES2
    
     LDA #0                 \ Load A with the byte we want to fill the memory block
                            \ with - i.e. zero
    
     STX SC+1               \ We want to zero-fill page X, so store this in the
                            \ high byte of SC, so the 16-bit address in SC and
                            \ SC+1 is now pointing to the SC-th byte of page X
    
    .ZEL1
    
     STA (SC),Y             \ Zero the Y-th byte of the block pointed to by SC,
                            \ so that's effectively the Y-th byte before SC
    
     INY                    \ Increment the loop counter
    
     BNE ZEL1               \ Loop back to zero the next byte
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: CLYNS                                                   [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the bottom three text rows of the space view
    
    
        Context: See this subroutine [on its own page](../main/subroutine/clyns.md)
     Variations: See [code variations](../related/compare/main/subroutine/clyns.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [dockEd](../main/subroutine/docked.md) calls CLYNS
                 * [EQSHP](../main/subroutine/eqshp.md) calls CLYNS
                 * [hm](../main/subroutine/hm.md) calls CLYNS
                 * [JMTB](../main/variable/jmtb.md) calls CLYNS
                 * [me2](../main/subroutine/me2.md) calls CLYNS
                 * [MESS](../main/subroutine/mess.md) calls CLYNS
                 * [qv](../main/subroutine/qv.md) calls CLYNS
                 * [TT219](../main/subroutine/tt219.md) calls CLYNS
    
    
    
    
    
    
    .CLYNS
    
     STZ DLY                \ Set the delay in DLY to 0, to indicate that we are
                            \ no longer showing an in-flight message, so any new
                            \ in-flight messages will be shown instantly
    
     STZ de                 \ Clear de, the flag that appends " DESTROYED" to the
                            \ end of the next text token, so that it doesn't
    
     LDA #%11111111         \ Set DTW2 = %11111111 to denote that we are not
     STA DTW2               \ currently printing a word
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch standard tokens to
     STA QQ17               \ Sentence Case
    
     LDA #20                \ Move the text cursor to row 20, near the bottom of
     STA YC                 \ the screen
    
     JSR TT67K              \ Print a newline
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA #&6A               \ Set SC+1 = &6A, for the high byte of SC(1 0)
     STA SC+1
    
     LDA #0                 \ Set SC = 0, so now SC(1 0) = &6A00
     STA SC
    
     LDX #3                 \ We want to clear three text rows, so set a counter in
                            \ X for 3 rows
    
    .CLYL
    
     LDY #8                 \ We want to clear each text row, starting from the
                            \ left, but we don't want to overwrite the border, so we
                            \ start from the second character block, which is byte
                            \ #8 from the edge, so set Y to 8 to act as the byte
                            \ counter within the row
    
    .EE2
    
     STA (SC),Y             \ Zero the Y-th byte from SC(1 0), which clears it by
                            \ setting it to colour 0, black
    
     INY                    \ Increment the byte counter in Y
    
     BNE EE2                \ Loop back to EE2 to blank the next byte along, until
                            \ we have done one page's worth (from byte #8 to #255)
    
     INC SC+1               \ We have just finished the first page - which covers
                            \ the left half of the text row - so we increment SC+1
                            \ so SC(1 0) points to the start of the next page, or
                            \ the start of the right half of the row
    
     STA (SC),Y             \ Clear the byte at SC(1 0), as that won't be caught by
                            \ the next loop
    
     LDY #247               \ The second page covers the right half of the text row,
                            \ and as before we don't want to overwrite the border,
                            \ which we can do by starting from the last-but-one
                            \ character block and working our way left towards the
                            \ centre of the row. The last-but-one character block
                            \ ends at byte 247 (that's 255 - 8, as each character
                            \ is 8 bytes), so we put this in Y to act as a byte
                            \ counter, as before
    
    .EE3K                   \ This label is a duplicate of a label in the TT23
                            \ routine
                            \
                            \ In the original source this label is EE3, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to EE3K
    
     STA (SC),Y             \ Zero the Y-th byte from SC(1 0), which clears it by
                            \ setting it to colour 0, black
    
     DEY                    \ Decrement the byte counter in Y
    
     BNE EE3K               \ Loop back to EE3K to blank the next byte to the left,
                            \ until we have done one page's worth (from byte #247 to
                            \ #1)
    
     INC SC+1               \ We have now blanked a whole text row, so increment
                            \ SC+1 so that SC(1 0) points to the next row
    
     DEX                    \ Decrement the row counter in X
    
     BNE CLYL               \ Loop back to blank another row, until we have done the
                            \ number of rows in X
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDA #0                 \ Set A = 0 as this is a return value for this routine
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DIALS (Part 1 of 4)                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the dashboard: speed indicator
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dials_part_1_of_4.md)
     Variations: See [code variations](../related/compare/main/subroutine/dials_part_1_of_4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md) calls DIALS
    
    
    
    
    
    * * *
    
    
     This routine updates the dashboard. First we draw all the indicators in the
     right part of the dashboard, from top (speed) to bottom (energy banks), and
     then we move on to the left part, again drawing from top (forward shield) to
     bottom (altitude).
    
     This first section starts us off with the speedometer in the top right.
    
    
    
    
    .DIALS
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA #1                 \ Set location &DDEB to 1. This location is in HAZEL,
     STA &DDEB              \ which contains the filing system RAM space, though
                            \ I'm not sure what effect this has
    
     LDA #&A0               \ Set SC(1 0) = &71A0, which is the screen address for
     STA SC                 \ the character block containing the left end of the
     LDA #&71               \ top indicator in the right part of the dashboard, the
     STA SC+1               \ one showing our speed
    
     JSR PZW2               \ Call PZW2 to set A to the colour for dangerous values
                            \ and X to the colour for safe values, suitable for
                            \ non-striped indicators
    
     STX K+1                \ Set K+1 (the colour we should show for low values) to
                            \ X (the colour to use for safe values)
    
     STA K                  \ Set K (the colour we should show for high values) to
                            \ A (the colour to use for dangerous values)
    
                            \ The above sets the following indicators to show red
                            \ for high values and yellow/white for low values
    
     LDA #14                \ Set T1 to 14, the threshold at which we change the
     STA T1                 \ indicator's colour
    
     LDA DELTA              \ Fetch our ship's speed into A, in the range 0-40
    
    \LSR A                  \ Draw the speed indicator using a range of 0-31, and
     JSR DIL-1              \ increment SC to point to the next indicator (the roll
                            \ indicator). The LSR is commented out as it isn't
                            \ required with a call to DIL-1, so perhaps this was
                            \ originally a call to DIL that got optimised
    
    
    
    
           Name: DIALS (Part 2 of 4)                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the dashboard: pitch and roll indicators
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dials_part_2_of_4.md)
     Variations: See [code variations](../related/compare/main/subroutine/dials_part_2_of_4.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
     STZ R                  \ Set R = P = 0 for the low bytes in the call to the ADD
     STZ P                  \ routine below
    
     LDA #8                 \ Set S = 8, which is the value of the centre of the
     STA S                  \ roll indicator
    
     LDA ALP1               \ Fetch the roll angle alpha as a value between 0 and
     LSR A                  \ 31, and divide by 4 to get a value of 0 to 7
     LSR A
    
     ORA ALP2               \ Apply the roll sign to the value, and flip the sign,
     EOR #%10000000         \ so it's now in the range -7 to +7, with a positive
                            \ roll angle alpha giving a negative value in A
    
     JSR ADDK               \ We now add A to S to give us a value in the range 1 to
                            \ 15, which we can pass to DIL2 to draw the vertical
                            \ bar on the indicator at this position. We use the ADD
                            \ routine like this:
                            \
                            \ (A X) = (A 0) + (S 0)
                            \
                            \ and just take the high byte of the result. We use ADD
                            \ rather than a normal ADC because ADD separates out the
                            \ sign bit and does the arithmetic using absolute values
                            \ and separate sign bits, which we want here rather than
                            \ the two's complement that ADC uses
    
     JSR DIL2               \ Draw a vertical bar on the roll indicator at offset A
                            \ and increment SC to point to the next indicator (the
                            \ pitch indicator)
    
     LDA BETA               \ Fetch the pitch angle beta as a value between -8 and
                            \ +8
    
     LDX BET1               \ Fetch the magnitude of the pitch angle beta, and if it
     BEQ P%+4               \ is 0 (i.e. we are not pitching), skip the next
                            \ instruction
    
     SBC #1                 \ The pitch angle beta is non-zero, so set A = A - 1
                            \ (the C flag is set by the call to DIL2 above, so we
                            \ don't need to do a SEC). This gives us a value of A
                            \ from -7 to +7 because these are magnitude-based
                            \ numbers with sign bits, rather than two's complement
                            \ numbers
    
     JSR ADDK               \ We now add A to S to give us a value in the range 1 to
                            \ 15, which we can pass to DIL2 to draw the vertical
                            \ bar on the indicator at this position (see the JSR ADD
                            \ above for more on this)
    
     JSR DIL2               \ Draw a vertical bar on the pitch indicator at offset A
                            \ and increment SC to point to the next indicator (the
                            \ four energy banks)
    
    
    
    
           Name: DIALS (Part 3 of 4)                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the dashboard: four energy banks
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dials_part_3_of_4.md)
     Variations: See [code variations](../related/compare/main/subroutine/dials_part_3_of_4.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
     LDY #0                 \ Set Y = 0, for use in various places below
    
     JSR PZW                \ Call PZW to set A to the colour for dangerous values
                            \ and X to the colour for safe values
    
     STX K                  \ Set K (the colour we should show for high values) to X
                            \ (the colour to use for safe values)
    
     STA K+1                \ Set K+1 (the colour we should show for low values) to
                            \ A (the colour to use for dangerous values)
    
                            \ The above sets the following indicators to show red
                            \ for low values and yellow/white for high values, which
                            \ we use not only for the energy banks, but also for the
                            \ shield levels and current fuel
    
     LDX #3                 \ Set up a counter in X so we can zero the four bytes at
                            \ XX15, so we can then calculate each of the four energy
                            \ banks' values before drawing them later
    
     STX T1                 \ Set T1 to 3, the threshold at which we change the
                            \ indicator's colour
    
    .DLL23
    
     STY XX15,X             \ Set the X-th byte of XX15 to 0
    
     DEX                    \ Decrement the counter
    
     BPL DLL23              \ Loop back for the next byte until the four bytes at
                            \ XX12 are all zeroed
    
     LDX #3                 \ Set up a counter in X to loop through the 4 energy
                            \ bank indicators, so we can calculate each of the four
                            \ energy banks' values and store them in XX12
    
     LDA ENERGY             \ Set A = Q = ENERGY / 4, so they are both now in the
     LSR A                  \ range 0-63 (so that's a maximum of 16 in each of the
     LSR A                  \ banks, and a maximum of 15 in the top bank)
    
     STA Q                  \ Set Q to A, so we can use Q to hold the remaining
                            \ energy as we work our way through each bank, from the
                            \ full ones at the bottom to the empty ones at the top
    
    .DLL24
    
     SEC                    \ Set A = A - 16 to reduce the energy count by a full
     SBC #16                \ bank
    
     BCC DLL26              \ If the C flag is clear then A < 16, so this bank is
                            \ not full to the brim, and is therefore the last one
                            \ with any energy in it, so jump to DLL26
    
     STA Q                  \ This bank is full, so update Q with the energy of the
                            \ remaining banks
    
     LDA #16                \ Store this bank's level in XX15 as 16, as it is full,
     STA XX15,X             \ with XX15+3 for the bottom bank and XX15+0 for the top
    
     LDA Q                  \ Set A to the remaining energy level again
    
     DEX                    \ Decrement X to point to the next bank, i.e. the one
                            \ above the bank we just processed
    
     BPL DLL24              \ Loop back to DLL24 until we have either processed all
                            \ four banks, or jumped out early to DLL26 if the top
                            \ banks have no charge
    
     BMI DLL9               \ Jump to DLL9 as we have processed all four banks (this
                            \ BMI is effectively a JMP as A will never be positive)
    
    .DLL26
    
     LDA Q                  \ If we get here then the bank we just checked is not
     STA XX15,X             \ fully charged, so store its value in XX15 (using Q,
                            \ which contains the energy of the remaining banks -
                            \ i.e. this one)
    
                            \ Now that we have the four energy bank values in XX12,
                            \ we can draw them, starting with the top bank in XX12
                            \ and looping down to the bottom bank in XX12+3, using Y
                            \ as a loop counter, which was set to 0 above
    
    .DLL9
    
     LDA XX15,Y             \ Fetch the value of the Y-th indicator, starting from
                            \ the top
    
     STY P                  \ Store the indicator number in P for retrieval later
    
     JSR DIL                \ Draw the energy bank using a range of 0-15, and
                            \ increment SC to point to the next indicator (the
                            \ next energy bank down)
    
     LDY P                  \ Restore the indicator number into Y
    
     INY                    \ Increment the indicator number
    
     CPY #4                 \ Check to see if we have drawn the last energy bank
    
     BNE DLL9               \ Loop back to DLL9 if we have more banks to draw,
                            \ otherwise we are done
    
    
    
    
           Name: DIALS (Part 4 of 4)                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the dashboard: shields, fuel, laser & cabin temp, altitude
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dials_part_4_of_4.md)
     Variations: See [code variations](../related/compare/main/subroutine/dials_part_4_of_4.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
     LDA #&70               \ Set SC(1 0) = &7020, which is the screen address for
     STA SC+1               \ the character block containing the left end of the
     LDA #&20               \ top indicator in the left part of the dashboard, the
     STA SC                 \ one showing the forward shield
    
     LDA FSH                \ Draw the forward shield indicator using a range of
     JSR DILX               \ 0-255, and increment SC to point to the next indicator
                            \ (the aft shield)
    
     LDA ASH                \ Draw the aft shield indicator using a range of 0-255,
     JSR DILX               \ and increment SC to point to the next indicator (the
                            \ fuel level)
    
     LDA #YELLOW2           \ Set K (the colour we should show for high values) to
     STA K                  \ yellow
    
     STA K+1                \ Set K+1 (the colour we should show for low values) to
                            \ yellow, so the fuel indicator always shows in this
                            \ colour
    
     LDA QQ14               \ Draw the fuel level indicator using a range of 0-63,
     JSR DILX+2             \ and increment SC to point to the next indicator (the
                            \ cabin temperature)
    
     JSR PZW2               \ Call PZW2 to set A to the colour for dangerous values
                            \ and X to the colour for safe values, suitable for
                            \ non-striped indicators
    
     STX K+1                \ Set K+1 (the colour we should show for low values) to
                            \ X (the colour to use for safe values)
    
     STA K                  \ Set K (the colour we should show for high values) to
                            \ A (the colour to use for dangerous values)
    
                            \ The above sets the following indicators to show red
                            \ for high values and yellow/white for low values, which
                            \ we use for the cabin and laser temperature bars
    
     LDX #11                \ Set T1 to 11, the threshold at which we change the
     STX T1                 \ cabin and laser temperature indicators' colours
    
     LDA CABTMP             \ Draw the cabin temperature indicator using a range of
     JSR DILX               \ 0-255, and increment SC to point to the next indicator
                            \ (the laser temperature)
    
     LDA GNTMP              \ Draw the laser temperature indicator using a range of
     JSR DILX               \ 0-255, and increment SC to point to the next indicator
                            \ (the altitude)
    
     LDA #240               \ Set T1 to 240, the threshold at which we change the
     STA T1                 \ altitude indicator's colour. As the altitude has a
                            \ range of 0-255, pixel 16 will not be filled in, and
                            \ 240 would change the colour when moving between pixels
                            \ 15 and 16, so this effectively switches off the colour
                            \ change for the altitude indicator
    
     LDA #YELLOW2           \ Set K (the colour we should show for high values) to
     STA K                  \ yellow
    
     STA K+1                \ Set K+1 (the colour we should show for low values) to
                            \ 240, or &F0 (dashboard colour 2, yellow/white), so the
                            \ altitude indicator always shows in this colour
    
     LDA ALTIT              \ Draw the altitude indicator using a range of 0-255
     JSR DILX
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     JMP COMPAS             \ We have now drawn all the indicators, so jump to
                            \ COMPAS to draw the compass, returning from the
                            \ subroutine using a tail call
    
    
    
    
           Name: PZW2                                                    [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Fetch the current dashboard colours for non-striped indicators, to
                 support flashing
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pzw2.md)
     References: This subroutine is called as follows:
                 * [DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md) calls PZW2
                 * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md) calls PZW2
    
    
    
    
    
    
    .PZW2
    
     LDX #WHITE2            \ Set X to white, so we can return that as the safe
                            \ colour in PZW below
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &23, or BIT &23A9, which does nothing apart
                            \ from affect the flags
    
                            \ Fall through into PZW to fetch the current dashboard
                            \ colours, returning white for safe colours rather than
                            \ stripes
    
    
    
    
           Name: PZW                                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Fetch the current dashboard colours, to support flashing
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pzw.md)
     Variations: See [code variations](../related/compare/main/subroutine/pzw.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md) calls PZW
    
    
    
    
    
    * * *
    
    
     Set A and X to the colours we should use for indicators showing dangerous and
     safe values respectively. This enables us to implement flashing indicators,
     which is one of the game's configurable options.
    
     If flashing is enabled, the colour returned in A (dangerous values) will be
     red for 8 iterations of the main loop, and green for the next 8, before
     going back to red. If we always use PZW to decide which colours we should use
     when updating indicators, flashing colours will be automatically taken care of
     for us.
    
     The values returned are #GREEN2 for green and #RED2 for red. These are mode 2
     bytes that contain 2 pixels, with the colour of each pixel given in four bits.
    
    
    
    * * *
    
    
     Returns:
    
       A                    The colour to use for indicators with dangerous values
    
       X                    The colour to use for indicators with safe values
    
    
    
    
    .PZW
    
     LDX #STRIPE            \ Set X to the dashboard stripe colour, which is stripe
                            \ 5-1 (magenta/red)
    
     LDA MCNT               \ A will be non-zero for 8 out of every 16 main loop
     AND #%00001000         \ counts, when bit 4 is set, so this is what we use to
                            \ flash the "danger" colour
    
     AND FLH                \ A will be zeroed if flashing colours are disabled
    
     BEQ P%+5               \ If A is zero, skip the next two instructions
    
     LDA #GREEN2            \ Otherwise flashing colours are enabled and it's the
     RTS                    \ main loop iteration where we flash them, so set A to
                            \ dashboard colour 2 (green) and return from the
                            \ subroutine
    
     LDA #RED2              \ Set A to dashboard colour 1 (red)
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DILX                                                    [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update a bar-based indicator on the dashboard
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dilx.md)
     Variations: See [code variations](../related/compare/main/subroutine/dilx.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md) calls DILX
                 * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md) calls via DILX+2
                 * [DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md) calls via DIL-1
                 * [DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md) calls via DIL
    
    
    
    
    
    * * *
    
    
     The range of values shown on the indicator depends on which entry point is
     called. For the default entry point of DILX, the range is 0-255 (as the value
     passed in A is one byte). The other entry points are shown below.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The value to be shown on the indicator (so the larger
                            the value, the longer the bar)
    
       T1                   The threshold at which we change the indicator's colour
                            from the low value colour to the high value colour. The
                            threshold is in pixels, so it should have a value from
                            0-16, as each bar indicator is 16 pixels wide
    
       K                    The colour to use when A is a high value, as a two-pixel
                            mode 2 character row byte
    
       K+1                  The colour to use when A is a low value, as a two-pixel
                            mode 2 character row byte
    
       SC(1 0)              The screen address of the first character block in the
                            indicator
    
    
    
    * * *
    
    
     Other entry points:
    
       DILX+2               The range of the indicator is 0-64 (for the fuel
                            indicator)
    
       DIL-1                The range of the indicator is 0-32 (for the speed
                            indicator)
    
       DIL                  The range of the indicator is 0-16 (for the energy
                            banks)
    
    
    
    
    .DILX
    
     LSR A                  \ If we call DILX, we set A = A / 16, so A is 0-15
     LSR A
    
     LSR A                  \ If we call DILX+2, we set A = A / 4, so A is 0-15
    
     LSR A                  \ If we call DIL-1, we set A = A / 2, so A is 0-15
    
    .DIL
    
                            \ If we call DIL, we leave A alone, so A is 0-15
    
     STA Q                  \ Store the indicator value in Q, now reduced to 0-15,
                            \ which is the length of the indicator to draw in pixels
    
     LDX #&FF               \ Set R = &FF, to use as a mask for drawing each row of
     STX R                  \ each character block of the bar, starting with a full
                            \ character's width of 4 pixels
    
     CMP T1                 \ If A >= T1 then we have passed the threshold where we
     BCS DL30               \ change bar colour, so jump to DL30 to set A to the
                            \ "high value" colour
    
     LDA K+1                \ Set A to K+1, the "low value" colour to use
    
     BNE DL31               \ Jump down to DL31 (this BNE is effectively a JMP as A
                            \ will never be zero)
    
    .DL30
    
     LDA K                  \ Set A to K, the "high value" colour to use
    
    .DL31
    
     STA COL                \ Store the colour of the indicator in COL
    
     LDY #2                 \ We want to start drawing the indicator on the third
                            \ line in this character row, so set Y to point to that
                            \ row's offset
    
     LDX #7                 \ Set up a counter in X for the width of the indicator,
                            \ which is 8 characters (each of which is 2 pixels wide,
                            \ to give a total width of 16 pixels)
    
    .DL1
    
     LDA Q                  \ Fetch the indicator value (0-15) from Q into A
    
     CMP #2                 \ If Q < 2, then we need to draw the end cap of the
     BCC DL2                \ indicator, which is less than a full character's
                            \ width, so jump down to DL2 to do this
    
     SBC #2                 \ Otherwise we can draw a two-pixel wide block, so
     STA Q                  \ subtract 2 from Q so it contains the amount of the
                            \ indicator that's left to draw after this character
    
     LDA R                  \ Fetch the shape of the indicator row that we need to
                            \ display from R, so we can use it as a mask when
                            \ painting the indicator. It will be &FF at this point
                            \ (i.e. a full four-pixel row)
    
    .DL5
    
     AND COL                \ Fetch the two-pixel mode 2 colour byte from COL, and
                            \ only keep pixels that have their equivalent bits set
                            \ in the mask byte in A
    
     STA (SC),Y             \ Draw the shape of the mask on pixel row Y of the
                            \ character block we are processing
    
     INY                    \ Draw the next pixel row, incrementing Y
     STA (SC),Y
    
     INY                    \ And draw the third pixel row, incrementing Y
     STA (SC),Y
    
     TYA                    \ Add 6 to Y, so Y is now 8 more than when we started
     CLC                    \ this loop iteration, so Y now points to the address
     ADC #6                 \ of the first line of the indicator bar in the next
     TAY                    \ character block (as each character is 8 bytes of
                            \ screen memory)
    
     DEX                    \ Decrement the loop counter for the next character
                            \ block along in the indicator
    
     BMI DL6                \ If we just drew the last character block then we are
                            \ done drawing, so jump down to DL6 to finish off
    
     BPL DL1                \ Loop back to DL1 to draw the next character block of
                            \ the indicator (this BPL is effectively a JMP as A will
                            \ never be negative following the previous BMI)
    
    .DL2
    
     EOR #1                 \ If we get here then we are drawing the indicator's
     STA Q                  \ end cap, so Q is < 2, and this EOR flips the bits, so
                            \ instead of containing the number of indicator columns
                            \ we need to fill in on the left side of the cap's
                            \ character block, Q now contains the number of blank
                            \ columns there should be on the right side of the cap's
                            \ character block
    
     LDA R                  \ Fetch the current mask from R, which will be &FF at
                            \ this point, so we need to turn Q of the columns on the
                            \ right side of the mask to black to get the correct end
                            \ cap shape for the indicator
    
    .DL3
    
     ASL A                  \ Shift the mask left and clear bits 0, 2, 4 and 8,
     AND #%10101010         \ which has the effect of shifting zeroes from the left
                            \ into each two-bit segment (i.e. xx xx xx xx becomes
                            \ x0 x0 x0 x0, which blanks out the last column in the
                            \ two-pixel mode 2 character block)
    
     DEC Q                  \ Decrement the counter for the number of columns to
                            \ blank out
    
     BPL DL3                \ If we still have columns to blank out in the mask,
                            \ loop back to DL3 until the mask is correct for the
                            \ end cap
    
     PHA                    \ Store the mask byte on the stack while we use the
                            \ accumulator for a bit
    
     STZ R                  \ Change the mask so no bits are set, so the characters
                            \ after the one we're about to draw will be all blank
    
     LDA #99                \ Set Q to a high number (99, why not) so we will keep
     STA Q                  \ drawing blank characters until we reach the end of
                            \ the indicator row
    
     PLA                    \ Restore the mask byte from the stack so we can use it
                            \ to draw the end cap of the indicator
    
     JMP DL5                \ Jump back up to DL5 to draw the mask byte on-screen
    
    .DL6
    
     INC SC+1               \ Increment the high byte of SC to point to the next
     INC SC+1               \ character row on-screen (as each row takes up exactly
                            \ two pages of 256 bytes) - so this sets up SC to point
                            \ to the next indicator, i.e. the one below the one we
                            \ just drew
    
    .DL9
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DIL2                                                    [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the roll or pitch indicator on the dashboard
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dil2.md)
     Variations: See [code variations](../related/compare/main/subroutine/dil2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md) calls DIL2
    
    
    
    
    
    * * *
    
    
     The indicator can show a vertical bar in 16 positions, with a value of 8
     showing the bar in the middle of the indicator.
    
     In practice this routine is only ever called with A in the range 1 to 15, so
     the vertical bar never appears in the leftmost position (though it does appear
     in the rightmost).
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The offset of the vertical bar to show in the indicator,
                            from 0 at the far left, to 8 in the middle, and 15 at
                            the far right
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is set
    
    
    
    
    .DIL2
    
     LDY #1                 \ We want to start drawing the vertical indicator bar on
                            \ the second line in the indicator's character block, so
                            \ set Y to point to that row's offset
    
     STA Q                  \ Store the offset of the vertical bar to draw in Q
    
                            \ We are now going to work our way along the indicator
                            \ on the dashboard, from left to right, working our way
                            \ along one character block at a time. Y will be used as
                            \ a pixel row counter to work our way through the
                            \ character blocks, so each time we draw a character
                            \ block, we will increment Y by 8 to move on to the next
                            \ block (as each character block contains 8 rows)
    
    .DLL10
    
     SEC                    \ Set A = Q - 2, so that A contains the offset of the
     LDA Q                  \ vertical bar from the start of this character block
     SBC #2
    
     BCS DLL11              \ If Q >= 2 then the character block we are drawing does
                            \ not contain the vertical indicator bar, so jump to
                            \ DLL11 to draw a blank character block
    
     LDA #&FF               \ Set A to a high number (and &FF is as high as they go)
    
     LDX Q                  \ Set X to the offset of the vertical bar, which we know
                            \ is within this character block
    
     STA Q                  \ Set Q to a high number (&FF, why not) so we will keep
                            \ drawing blank characters after this one until we reach
                            \ the end of the indicator row
    
     LDA CTWOS,X            \ CTWOS is a table of ready-made one-pixel mode 2 bytes,
                            \ just like the TWOS and TWOS2 tables for mode 1 (see
                            \ the PIXEL routine for details of how they work). This
                            \ fetches a mode 2 one-pixel byte with the pixel
                            \ position at X, so the pixel is at the offset that we
                            \ want for our vertical bar
    
     AND #WHITE2            \ The two-pixel mode 2 byte in #WHITE2 represents two
                            \ pixels of colour %0111 (7), which is white in both
                            \ dashboard palettes. We AND this with A so that we only
                            \ keep the pixel that matches the position of the
                            \ vertical bar (i.e. A is acting as a mask on the
                            \ two-pixel colour byte)
    
     BNE DLL12              \ Jump to DLL12 to skip the code for drawing a blank,
                            \ and move on to drawing the indicator (this BNE is
                            \ effectively a JMP as A is always non-zero)
    
    .DLL11
    
                            \ If we get here then we want to draw a blank for this
                            \ character block
    
     STA Q                  \ Update Q with the new offset of the vertical bar, so
                            \ it becomes the offset after the character block we
                            \ are about to draw
    
     LDA #0                 \ Change the mask so no bits are set, so all of the
                            \ character blocks we display from now on will be blank
    .DLL12
    
     STA (SC),Y             \ Draw the shape of the mask on pixel row Y of the
                            \ character block we are processing
    
     INY                    \ Draw the next pixel row, incrementing Y
     STA (SC),Y
    
     INY                    \ And draw the third pixel row, incrementing Y
     STA (SC),Y
    
     INY                    \ And draw the fourth pixel row, incrementing Y
     STA (SC),Y
    
     TYA                    \ Add 5 to Y, so Y is now 8 more than when we started
     CLC                    \ this loop iteration, so Y now points to the address
     ADC #5                 \ of the first line of the indicator bar in the next
     TAY                    \ character block (as each character is 8 bytes of
                            \ screen memory)
    
     CPY #60                \ If Y < 60 then we still have some more character
     BCC DLL10              \ blocks to draw, so loop back to DLL10 to display the
                            \ next one along
    
     INC SC+1               \ Increment the high byte of SC to point to the next
     INC SC+1               \ character row on-screen (as each row takes up exactly
                            \ two pages of 256 bytes) - so this sets up SC to point
                            \ to the next indicator, i.e. the one below the one we
                            \ just drew
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: ADDK                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A X) = (A P) + (S R)
    
      Deep dive: [Adding sign-magnitude numbers](https://elite.bbcelite.com/deep_dives/adding_sign-magnitude_numbers.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/addk.md)
     References: This subroutine is called as follows:
                 * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md) calls ADDK
                 * [PIX1](../main/subroutine/pix1.md) calls ADDK
    
    
    
    
    
    * * *
    
    
     Add two 16-bit sign-magnitude numbers together, calculating:
    
       (A X) = (A P) + (S R)
    
     This is an exact duplicate of the ADD routine, which is also present in this
     source, so it isn't clear why this duplicate exists.
    
    
    
    
    .ADDK
    
     STA T1                 \ Store argument A in T1
    
     AND #%10000000         \ Extract the sign (bit 7) of A and store it in T
     STA T
    
     EOR S                  \ EOR bit 7 of A with S. If they have different bit 7s
     BMI MU8K               \ (i.e. they have different signs) then bit 7 in the
                            \ EOR result will be 1, which means the EOR result is
                            \ negative. So the AND, EOR and BMI together mean "jump
                            \ to MU8K if A and S have different signs"
    
                            \ If we reach here, then A and S have the same sign, so
                            \ we can add them and set the sign to get the result
    
     LDA R                  \ Add the least significant bytes together into X:
     CLC                    \
     ADC P                  \   X = P + R
     TAX
    
     LDA S                  \ Add the most significant bytes together into A. We
     ADC T1                 \ stored the original argument A in T1 earlier, so we
                            \ can do this with:
                            \
                            \   A = A  + S + C
                            \     = T1 + S + C
    
     ORA T                  \ If argument A was negative (and therefore S was also
                            \ negative) then make sure result A is negative by
                            \ OR'ing the result with the sign bit from argument A
                            \ (which we stored in T)
    
     RTS                    \ Return from the subroutine
    
    .MU8K
    
                            \ If we reach here, then A and S have different signs,
                            \ so we can subtract their absolute values and set the
                            \ sign to get the result
    
     LDA S                  \ Clear the sign (bit 7) in S and store the result in
     AND #%01111111         \ U, so U now contains |S|
     STA U
    
     LDA P                  \ Subtract the least significant bytes into X:
     SEC                    \
     SBC R                  \   X = P - R
     TAX
    
     LDA T1                 \ Restore the A of the argument (A P) from T1 and
     AND #%01111111         \ clear the sign (bit 7), so A now contains |A|
    
     SBC U                  \ Set A = |A| - |S|
    
                            \ At this point we have |A P| - |S R| in (A X), so we
                            \ need to check whether the subtraction above was the
                            \ right way round (i.e. that we subtracted the smaller
                            \ absolute value from the larger absolute value)
    
     BCS MU9K               \ If |A| >= |S|, our subtraction was the right way
                            \ round, so jump to MU9K to set the sign
    
                            \ If we get here, then |A| < |S|, so our subtraction
                            \ above was the wrong way round (we actually subtracted
                            \ the larger absolute value from the smaller absolute
                            \ value). So let's subtract the result we have in (A X)
                            \ from zero, so that the subtraction is the right way
                            \ round
    
     STA U                  \ Store A in U
    
     TXA                    \ Set X = 0 - X using two's complement (to negate a
     EOR #&FF               \ number in two's complement, you can invert the bits
     ADC #1                 \ and add one - and we know the C flag is clear as we
     TAX                    \ didn't take the BCS branch above, so the ADC will do
                            \ the correct addition)
    
     LDA #0                 \ Set A = 0 - A, which we can do this time using a
     SBC U                  \ subtraction with the C flag clear
    
     ORA #%10000000         \ We now set the sign bit of A, so that the EOR on the
                            \ next line will give the result the opposite sign to
                            \ argument A (as T contains the sign bit of argument
                            \ A). This is the same as giving the result the same
                            \ sign as argument S (as A and S have different signs),
                            \ which is what we want, as S has the larger absolute
                            \ value
    
    .MU9K
    
     EOR T                  \ If we get here from the BCS above, then |A| >= |S|,
                            \ so we want to give the result the same sign as
                            \ argument A, so if argument A was negative, we flip
                            \ the sign of the result with an EOR (to make it
                            \ negative)
    
     RTS                    \ Return from the subroutine
    
     IF _SNG47
    
      SKIP 77               \ These bytes appear to be unused
    
     ELIF _COMPACT
    
      SKIP 3                \ These bytes appear to be unused
    
     ENDIF
    
    
    
    
    
           Name: FONT%                                                   [Show more]
           Type: Variable
       Category: Text
        Summary: A copy of the character definition bitmap table from the MOS ROM
    
    
        Context: See this variable [on its own page](../main/variable/font_per_cent.md)
     Variations: See [code variations](../related/compare/main/variable/font_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [CHPR](../main/subroutine/chpr.md) uses FONT%
    
    
    
    
    
    * * *
    
    
     This is used by the TT26 routine to save time looking up the character bitmaps
     from the ROM. Note that FONT% contains just the high byte (i.e. the page
     number) of the address of this table, rather than the full address.
    
     The contents of the P.FONT.bin file included here are taken straight from the
     following three pages in the BBC Micro OS 1.20 ROM:
    
       ASCII 32-63  are defined in &C000-&C0FF (page 0)
       ASCII 64-95  are defined in &C100-&C1FF (page 1)
       ASCII 96-126 are defined in &C200-&C2F0 (page 2)
    
     The code could look these values up each time (as the cassette version does),
     but it's quicker to use a lookup table, at the expense of three pages of
     memory.
    
    
    
    
     FONT% = HI(P%)
    
     INCBIN "1-source-files/fonts/P.FONT.bin"
    
    
    
    
           Name: log                                                     [Show more]
           Type: Variable
       Category: Maths (Arithmetic)
        Summary: Binary logarithm table (high byte)
    
    
        Context: See this variable [on its own page](../main/variable/log.md)
     Variations: See [code variations](../related/compare/main/variable/log.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DVID4](../main/subroutine/dvid4.md) uses log
                 * [FMLTU](../main/subroutine/fmltu.md) uses log
                 * [LL28](../main/subroutine/ll28.md) uses log
                 * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md) uses log
                 * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md) uses log
    
    
    
    
    
    * * *
    
    
     At byte n, the table contains the high byte of:
    
       &2000 * log10(n) / log10(2) = 32 * 256 * log10(n) / log10(2)
    
     where log10 is the logarithm to base 10. The change-of-base formula says that:
    
       log2(n) = log10(n) / log10(2)
    
     so byte n contains the high byte of:
    
       32 * log2(n) * 256
    
    
    
    
    .log
    
     SKIP 1
    
     FOR I%, 1, 255
    
      EQUB HI(INT(&2000 * LOG(I%) / LOG(2) + 0.5))
    
     NEXT
    
    
    
    
    
           Name: logL                                                    [Show more]
           Type: Variable
       Category: Maths (Arithmetic)
        Summary: Binary logarithm table (low byte)
    
    
        Context: See this variable [on its own page](../main/variable/logl.md)
     Variations: See [code variations](../related/compare/main/variable/logl.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DVID4](../main/subroutine/dvid4.md) uses logL
                 * [FMLTU](../main/subroutine/fmltu.md) uses logL
                 * [LL28](../main/subroutine/ll28.md) uses logL
                 * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md) uses logL
                 * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md) uses logL
    
    
    
    
    
    * * *
    
    
     Byte n contains the low byte of:
    
       32 * log2(n) * 256
    
    
    
    
    .logL
    
     SKIP 1
    
     FOR I%, 1, 255
    
      EQUB LO(INT(&2000 * LOG(I%) / LOG(2) + 0.5))
    
     NEXT
    
    
    
    
    
           Name: alogh                                                   [Show more]
           Type: Variable
       Category: Maths (Arithmetic)
        Summary: Binary antilogarithm table
    
    
        Context: See this variable [on its own page](../main/variable/alogh.md)
     Variations: See [code variations](../related/compare/main/variable/antilog-alogh.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DVID4](../main/subroutine/dvid4.md) uses alogh
                 * [FMLTU](../main/subroutine/fmltu.md) uses alogh
                 * [LL28](../main/subroutine/ll28.md) uses alogh
                 * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md) uses alogh
                 * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md) uses alogh
    
    
    
    
    
    * * *
    
    
     At byte n, the table contains:
    
       2^((n / 2 + 128) / 16) / 256
    
     which equals:
    
       2^(n / 32 + 8) / 256
    
    
    
    
    .alogh
    
     FOR I%, 0, 255
    
      EQUB HI(INT(2^((I% / 2 + 128) / 16) + 0.5))
    
     NEXT
    
    
    
    
           Name: SCTBX1                                                  [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Lookup table for converting a pixel x-coordinate to the bit number
                 within the pixel row byte that corresponds to this pixel
    
    
        Context: See this variable [on its own page](../main/variable/sctbx1.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     This routine is not used in this version of Elite. It is left over from the
     Apple II version.
    
    
    
    
    .SCTBX1
    
    FOR I%, 0, 255
    
     EQUB (I% + 8) MOD 7
    
    NEXT
    
    
    
    
           Name: SCTBX2                                                  [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: Lookup table for converting a pixel x-coordinate to the byte
                 number in the pixel row that corresponds to this pixel
    
    
        Context: See this variable [on its own page](../main/variable/sctbx2.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     This routine is not used in this version of Elite. It is left over from the
     Apple II version.
    
    
    
    
    .SCTBX2
    
    FOR I%, 0, 255
    
     EQUB (I% + 8) DIV 7
    
    NEXT
    
    
    
    
           Name: wtable                                                  [Show more]
           Type: Variable
       Category: Save and load
        Summary: 6-bit to 7-bit nibble conversion table
    
    
        Context: See this variable [on its own page](../main/variable/wtable.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     This table is not used in this version of Elite. It is left over from the
     Apple II version.
    
    
    
    
    .wtable
    
     EQUD &9B9A9796         \ NIBL     DFB $96,$97,$9A
     EQUD &A69F9E9D         \          DFB $9B,$9D,$9E
     EQUD &ADACABA7         \          DFB $9F,$A6,$A7
     EQUD &B3B2AFAE         \          DFB $AB,$AC,$AD
     EQUD &B7B6B5B4         \          DFB $AE,$AF,$B2
     EQUD &BCBBBAB9         \          DFB $B3,$B4,$B5
     EQUD &CBBFBEBD         \          DFB $B6,$B7,$B9
     EQUD &D3CFCECD         \          DFB $BA,$BB,$BC
     EQUD &DAD9D7D6         \          DFB $BD,$BE,$BF
     EQUD &DEDDDCDB         \          DFB $CB,$CD,$CE
     EQUD &E7E6E5DF         \          DFB $CF,$D3,$D6
     EQUD &ECEBEAE9         \          DFB $D7,$D9,$DA
     EQUD &F2EFEEED         \          DFB $DB,$DC,$DD
     EQUD &F6F5F4F3         \          DFB $DE,$DF,$E5
     EQUD &FBFAF9F7         \          DFB $E6,$E7,$E9
     EQUD &FFFEFDFC         \          DFB $EA,$EB,$EC
                            \          DFB $ED,$EE,$EF
                            \          DFB $F2,$F3,$F4
                            \          DFB $F5,$F6,$F7
                            \          DFB $F9,$FA,$FB
                            \          DFB $FC,$FD,$FE
                            \          DFB $FF
    
    
    
    
           Name: UP                                                      [Show more]
           Type: Workspace
        Address: &2C40 to &2C61
       Category: Workspaces
        Summary: Configuration variables
    
    
        Context: See this workspace [on its own page](../main/workspace/up.md)
     Variations: See [code variations](../related/compare/main/workspace/up.md) for this workspace in the different versions
     References: No direct references to this workspace in this source file
    
    
    
    
    
    
    .UP
    
     SKIP 0                 \ The start of the UP workspace
    
    .COMC
    
     SKIP 1                 \ The colour of the dot on the compass
                            \
                            \   * #WHITE2 = the object in the compass is in front of
                            \     us, so the dot is white
                            \
                            \   * #GREEN2 = the object in the compass is behind us,
                            \     so the dot is green
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BEGIN](../main/subroutine/begin.md)
                            \   * [DOT](../main/subroutine/dot.md)
                            \   * [SP2](../main/subroutine/sp2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .dials
    
     SKIP 14                \ These bytes appear to be unused
    
    .mscol
    
     SKIP 4                 \ This byte appears to be unused
    
    .CATF
    
     SKIP 1                 \ The disc catalogue flag
                            \
                            \ Determines whether a disc catalogue is currently in
                            \ progress, so the TT26 print routine can format the
                            \ output correctly:
                            \
                            \   * 0 = disc is not currently being catalogued
                            \
                            \   * 1 = disc is currently being catalogued
                            \
                            \ Specifically, when CATF is non-zero, TT26 will omit
                            \ column 17 from the catalogue so that it will fit
                            \ on-screen (column 17 is blank column in the middle
                            \ of the catalogue, between the two lists of filenames,
                            \ so it can be dropped without affecting the layout)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CATS](../main/subroutine/cats.md)
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [NEWBRK](../main/subroutine/newbrk.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DFLAG
    
     SKIP 1                 \ This byte appears to be unused
    
    .DNOIZ
    
     SKIP 1                 \ Sound on/off configuration setting
                            \
                            \   * 0 = sound is on (default)
                            \
                            \   * Non-zero = sound is off
                            \
                            \ Toggled by pressing "S" when paused, see the DK4
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../main/subroutine/dk4.md)
                            \   * [NOISE](../main/subroutine/noise.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DAMP
    
     SKIP 1                 \ Keyboard damping configuration setting
                            \
                            \   * 0 = damping is enabled (default)
                            \
                            \   * &FF = damping is disabled
                            \
                            \ Toggled by pressing CAPS LOCK when paused, see the
                            \ DKS3 routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [cntr](../main/subroutine/cntr.md)
                            \   * [DKS3](../main/subroutine/dks3.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DJD
    
     SKIP 1                 \ Keyboard auto-recentre configuration setting
                            \
                            \   * 0 = auto-recentre is enabled (default)
                            \
                            \   * &FF = auto-recentre is disabled
                            \
                            \ Toggled by pressing "A" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [REDU2](../main/subroutine/redu2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .PATG
    
     SKIP 1                 \ Configuration setting to show the author names on the
                            \ start-up screen and enable manual hyperspace mis-jumps
                            \
                            \   * 0 = no author names or manual mis-jumps (default)
                            \
                            \   * &FF = show author names and allow manual mis-jumps
                            \
                            \ Toggled by pressing "X" when paused, see the DKS3
                            \ routine for details
                            \
                            \ This needs to be turned on for manual mis-jumps to be
                            \ possible. To do a manual mis-jump, first toggle the
                            \ author display by pausing the game and pressing "X",
                            \ and during the next hyperspace, hold down CTRL to
                            \ force a mis-jump. See routine ee5 for the "AND PATG"
                            \ instruction that implements this logic
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT18](../main/subroutine/tt18.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .FLH
    
     SKIP 1                 \ Flashing console bars configuration setting
                            \
                            \   * 0 = static bars (default)
                            \
                            \   * &FF = flashing bars
                            \
                            \ Toggled by pressing "F" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [PZW](../main/subroutine/pzw.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTGY
    
     SKIP 1                 \ Reverse joystick Y-channel configuration setting
                            \
                            \   * 0 = reversed Y-channel
                            \
                            \   * &FF = standard Y-channel (default)
                            \
                            \ Toggled by pressing "Y" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \   * [RESET](../main/subroutine/reset.md)
                            \   * [TT17X](../main/subroutine/tt17x.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTE
    
     SKIP 1                 \ Reverse both joystick channels configuration setting
                            \
                            \   * 0 = standard channels (default)
                            \
                            \   * &FF = reversed channels
                            \
                            \ Toggled by pressing "J" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../main/subroutine/dk4.md)
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \   * [TT17X](../main/subroutine/tt17x.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTK
    
     SKIP 1                 \ Keyboard or joystick configuration setting
                            \
                            \   * 0 = keyboard (default)
                            \
                            \   * &FF = joystick
                            \
                            \ Toggled by pressing "K" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../main/subroutine/dk4.md)
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT17](../main/subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .UPTOG
    
     SKIP 1                 \ The configuration setting for toggle key "U", which
                            \ isn't actually used but is still updated by pressing
                            \ "U" while the game is paused. This is a configuration
                            \ option from the Apple II version of Elite that lets
                            \ you switch between lower-case and upper-case text
    
    .DISK
    
     SKIP 1                 \ The configuration setting for toggle key "T", which
                            \ isn't actually used but is still updated by pressing
                            \ "T" while the game is paused. This is a configuration
                            \ option from the Commodore 64 version of Elite that
                            \ lets you switch between tape and disc
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BEGIN](../main/subroutine/begin.md)
                            \   * [FILEPR](../main/subroutine/filepr.md)
                            \   * [OTHERFILEPR](../main/subroutine/otherfilepr.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BSTK
    
     SKIP 1                 \ Bitstik configuration setting
                            \
                            \   * 0 = keyboard or joystick (default)
                            \
                            \   * &FF = Bitstik
                            \
                            \ Toggled by pressing "B" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../main/subroutine/dk4.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SKIP 1                 \ This byte appears to be unused
    
    .VOL
    
     EQUB 7                 \ The volume level for the game's sound effects (0-7)
                            \
                            \ This is controlled by the "<" and ">" keys while the
                            \ game is paused, and the default level is 7
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../main/subroutine/dk4.md)
                            \   * [SOINT](../main/subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     PRINT "UP workspace from ", ~UP, "to ", ~P%-1, "inclusive"
    
    
    
    
           Name: TGINT                                                   [Show more]
           Type: Variable
       Category: Keyboard
        Summary: The keys used to toggle configuration settings when the game is
                 paused
    
    
        Context: See this variable [on its own page](../main/variable/tgint.md)
     References: This variable is used as follows:
                 * [DKS3](../main/subroutine/dks3.md) uses TGINT
    
    
    
    
    
    
    .TGINT
    
     EQUB 1                 \ The configuration keys in the same order as their
     EQUS "AXFYJKUT"        \ configuration bytes (starting from DAMP). The 1 is
                            \ for CAPS LOCK, and although "U" and "T" still toggle
                            \ the relevant configuration bytes, those values are not
                            \ used, so those keys have no effect
    
    
    
    
           Name: S%                                                      [Show more]
           Type: Subroutine
       Category: Loader
        Summary: Move code, set up break handler and start the game
    
    
        Context: See this subroutine [on its own page](../main/subroutine/s_per_cent.md)
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
    
    
    
    
           Name: DEEOR                                                   [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Unscramble the main code
    
    
        Context: See this subroutine [on its own page](../main/subroutine/deeor.md)
     References: This subroutine is called as follows:
                 * [S%](../main/subroutine/s_per_cent.md) calls DEEOR
    
    
    
    
    
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
    
    
    
    
           Name: DEEORS                                                  [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Decrypt a multi-page block of memory
    
    
        Context: See this subroutine [on its own page](../main/subroutine/deeors.md)
     References: This subroutine is called as follows:
                 * [DEEOR](../main/subroutine/deeor.md) calls DEEORS
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       FRIN(1 0)            The start address of the block to decrypt
    
       (A Y)                The end address of the block to decrypt
    
       X                    The decryption seed
    
    
    
    
    .DEEORS
    
     STX T                  \ Store the decryption seed in T as our starting point
    
     STA SC+1               \ Set SC(1 0) = (A 0) to point to the start of page A,
     LDA #0                 \ so we can use SC(1 0) + Y as our pointer to the next
     STA SC                 \ byte to decrypt
    
    .DEEORL
    
     LDA (SC),Y             \ Set A to the Y-th byte of SC(1 0)
    
     SEC                    \ Set A = A - T
     SBC T
    
     STA (SC),Y             \ Update the Y-th byte of SC to the new value in A
    
     STA T                  \ Update T with the new value in A
    
     TYA                    \ Set A to the current byte index in Y
    
     BNE P%+4               \ If A <> 0 then decrement the high byte of SC(1 0) to
     DEC SC+1               \ point to the previous page
    
     DEY                    \ Decrement the byte pointer
    
     CPY FRIN               \ Loop back to decrypt the next byte, until Y = the low
     BNE DEEORL             \ byte of FRIN(1 0), at which point we have decrypted a
                            \ whole page
    
     LDA SC+1               \ Check whether SC(1 0) matches FRIN(1 0) and loop back
     CMP FRIN+1             \ to decrypt the next byte until it does, at which point
     BNE DEEORL             \ we have decrypted the whole block
    
     RTS                    \ Return from the subroutine
    
     EQUB &B7, &AA          \ These bytes appear to be unused, though there is a
     EQUB &45, &23          \ comment in the original source that says "red
                            \ herring", so this would appear to be a red herring
                            \ aimed at confusing any crackers
    
    
    
    
           Name: G%                                                      [Show more]
           Type: Variable
       Category: Utility routines
        Summary: Denotes the start of the main game code, from ELITE A to ELITE H
    
    
        Context: See this variable [on its own page](../main/variable/g_per_cent.md)
     References: This variable is used as follows:
                 * [DEEOR](../main/subroutine/deeor.md) uses G%
    
    
    
    
    
    
    .G%
    
     SKIP 0                 \ The game code is scrambled from here to F% (or, as the
                            \ original source code puts it, "mutilated")
    
    
    
    
           Name: DOENTRY                                                 [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Dock at the space station, show the ship hangar and work out any
                 mission progression
      Deep dive: [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
                 [The Thargoid Plans mission](https://elite.bbcelite.com/deep_dives/the_thargoid_plans_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/doentry.md)
     Variations: See [code variations](../related/compare/main/subroutine/doentry.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md) calls DOENTRY
    
    
    
    
    
    
    .DOENTRY
    
     JSR RES2               \ Reset a number of flight variables and workspaces
    
     JSR LAUN               \ Show the space station docking tunnel
    
     LDA #0                 \ Reduce the speed to 0
     STA DELTA
    
     STA GNTMP              \ Cool down the lasers completely
    
     STA QQ22+1             \ Reset the on-screen hyperspace counter
    
     LDA #&FF               \ Recharge the forward and aft shields
     STA FSH
     STA ASH
    
     STA ENERGY             \ Recharge the energy banks
    
     JSR HALL               \ Show the ship hangar
    
     LDY #44                \ Wait for 44/50 of a second (0.88 seconds)
     JSR DELAY
    
     LDA TP                 \ Fetch bits 0 and 1 of TP, and if they are non-zero
     AND #%00000011         \ (i.e. mission 1 is either in progress or has been
     BNE EN1                \ completed), skip to EN1
    
     LDA TALLY+1            \ If the high byte of TALLY is zero (so we have a combat
     BEQ EN4                \ rank below Competent, or we are Competent but have not
                            \ yet earned a grand total of at least 256 kill points),
                            \ jump to EN4 as we are not yet good enough to qualify
                            \ for a mission
    
     LDA GCNT               \ Fetch the galaxy number into A, and if any of bits 1-7
     LSR A                  \ are set (i.e. A > 1), jump to EN4 as mission 1 can
     BNE EN4                \ only be triggered in the first two galaxies
    
     JMP BRIEF              \ If we get here then mission 1 hasn't started, we have
                            \ reached a combat rank of at least Competent plus 128
                            \ kill points, and we are in galaxy 0 or 1 (shown
                            \ in-game as galaxy 1 or 2), so it's time to start
                            \ mission 1 by calling BRIEF
    
    .EN1
    
                            \ If we get here then mission 1 is either in progress or
                            \ has been completed
    
     CMP #%00000011         \ If bits 0 and 1 are not both set, then jump to EN2
     BNE EN2
    
     JMP DEBRIEF            \ Bits 0 and 1 are both set, so mission 1 is both in
                            \ progress and has been completed, which means we have
                            \ only just completed it, so jump to DEBRIEF to end the
                            \ mission get our reward
    
    .EN2
    
                            \ Mission 1 has been completed, so now to check for
                            \ mission 2
    
     LDA GCNT               \ Fetch the galaxy number into A
    
     CMP #2                 \ If this is not galaxy 2 (shown in-game as galaxy 3),
     BNE EN4                \ jump to EN4 as we can only start mission 2 in the
                            \ third galaxy
    
     LDA TP                 \ Extract bits 0-3 of TP into A
     AND #%00001111
    
     CMP #%00000010         \ If mission 1 is complete and no longer in progress,
     BNE EN3                \ and mission 2 is not yet started, then bits 0-3 of TP
                            \ will be %0010, so this jumps to EN3 if this is not the
                            \ case
    
     LDA TALLY+1            \ If the high byte of TALLY is < 5 (so we have a combat
     CMP #5                 \ rank that is less than 3/8 of the way from Dangerous
     BCC EN4                \ to Deadly), jump to EN4 as our rank isn't high enough
                            \ for mission 2
    
     JMP BRIEF2             \ If we get here, mission 1 is complete and no longer in
                            \ progress, mission 2 hasn't started, we have reached a
                            \ combat rank of 3/8 of the way from Dangerous to
                            \ Deadly, and we are in galaxy 2 (shown in-game as
                            \ galaxy 3), so it's time to start mission 2 by calling
                            \ BRIEF2
    
    .EN3
    
     CMP #%00000110         \ If mission 1 is complete and no longer in progress,
     BNE EN5                \ and mission 2 has started but we have not yet been
                            \ briefed and picked up the plans, then bits 0-3 of TP
                            \ will be %0110, so this jumps to EN5 if this is not the
                            \ case
    
     LDA QQ0                \ Set A = the current system's galactic x-coordinate
    
     CMP #215               \ If A <> 215 then jump to EN4
     BNE EN4
    
     LDA QQ1                \ Set A = the current system's galactic y-coordinate
    
     CMP #84                \ If A <> 84 then jump to EN4
     BNE EN4
    
     JMP BRIEF3             \ If we get here, mission 1 is complete and no longer in
                            \ progress, mission 2 has started but we have not yet
                            \ picked up the plans, and we have just arrived at
                            \ Ceerdi at galactic coordinates (215, 84), so we jump
                            \ to BRIEF3 to get a mission brief and pick up the plans
                            \ that we need to carry to Birera
    
    .EN5
    
     CMP #%00001010         \ If mission 1 is complete and no longer in progress,
     BNE EN4                \ and mission 2 has started and we have picked up the
                            \ plans, then bits 0-3 of TP will be %1010, so this
                            \ jumps to EN5 if this is not the case
    
     LDA QQ0                \ Set A = the current system's galactic x-coordinate
    
     CMP #63                \ If A <> 63 then jump to EN4
     BNE EN4
    
     LDA QQ1                \ Set A = the current system's galactic y-coordinate
    
     CMP #72                \ If A <> 72 then jump to EN4
     BNE EN4
    
     JMP DEBRIEF2           \ If we get here, mission 1 is complete and no longer in
                            \ progress, mission 2 has started and we have picked up
                            \ the plans, and we have just arrived at Birera at
                            \ galactic coordinates (63, 72), so we jump to DEBRIEF2
                            \ to end the mission and get our reward
    
    .EN4
    
    \LDA CASH+2             \ These instructions are commented out in the original
    \CMP #&C4               \ source
    \BCC EN6
    \
    \LDA TP
    \AND #&10
    \BNE EN6
    \
    \JMP TBRIEF
    \
    \.EN6
    
     JMP BAY                \ If we get here them we didn't start or any missions,
                            \ so jump to BAY to go to the docking bay (i.e. show the
                            \ Status Mode screen)
    
    
    
    
           Name: TRIBDIR                                                 [Show more]
           Type: Variable
       Category: Missions
        Summary: The low byte of the four 16-bit directions in which Trumble
                 sprites can move
    
    
        Context: See this variable [on its own page](../main/variable/tribdir.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    \.TRIBDIR               \ These instructions are commented out in the original
    \                       \ source
    \EQUB 0
    \EQUB 1
    \EQUB &FF
    \EQUB 0
    
    
    
    
           Name: TRIBDIRH                                                [Show more]
           Type: Variable
       Category: Missions
        Summary: The high byte of the four 16-bit directions in which Trumble
                 sprites can move
    
    
        Context: See this variable [on its own page](../main/variable/tribdirh.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    \.TRIBDIRH              \ These instructions are commented out in the original
    \                       \ source
    \EQUB 0
    \EQUB 0
    \EQUB &FF
    \EQUB 0
    
    
    
    
           Name: SPMASK                                                  [Show more]
           Type: Variable
       Category: Missions
        Summary: Masks for updating sprite bits in VIC+&10 for the top bit of the
                 9-bit x-coordinates of the Trumble sprites
    
    
        Context: See this variable [on its own page](../main/variable/spmask.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .SPMASK
    
     EQUW &04FB             \ These bytes are unused and are left over from the
     EQUW &08F7             \ Commodore 64 version
     EQUW &10EF
     EQUW &20DF
     EQUW &40BF
     EQUW &807F
    
    
    
    
           Name: Main flight loop (Part 1 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Seed the random number generator
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Generating random numbers](https://elite.bbcelite.com/deep_dives/generating_random_numbers.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_1_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_1_of_16.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](../main/subroutine/death.md) calls via M%
                 * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md) calls via M%
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Seed the random number generator
    
    
    
    * * *
    
    
     Other entry points:
    
       M%                   The entry point for the main flight loop
    
    
    
    
    .M%
    
     LDA K%                 \ We want to seed the random number generator with a
                            \ pretty random number, so fetch the contents of K%,
                            \ which is the x_lo coordinate of the planet. This value
                            \ will be fairly unpredictable, so it's a pretty good
                            \ candidate
    
     STA RAND               \ Store the seed in the first byte of the four-byte
                            \ random number seed that's stored in RAND
    
    \LDA TRIBCT             \ These instructions are commented out in the original
    \BEQ NOMVETR            \ source
    \JMP MVTRIBS
    \
    \.NOMVETR
    
    
    
    
           Name: Main flight loop (Part 2 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Calculate the alpha and beta angles from the current pitch and
                 roll of our ship
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Pitching and rolling](https://elite.bbcelite.com/deep_dives/pitching_and_rolling.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_2_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_2_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Calculate the alpha and beta angles from the current pitch and roll
    
     Here we take the current rate of pitch and roll, as set by the joystick or
     keyboard, and convert them into alpha and beta angles that we can use in the
     matrix functions to rotate space around our ship. The alpha angle covers
     roll, while the beta angle covers pitch (there is no yaw in this version of
     Elite). The angles are in radians, which allows us to use the small angle
     approximation when moving objects in the sky (see the MVEIT routine for more
     on this). Also, the signs of the two angles are stored separately, in both
     the sign and the flipped sign, as this makes calculations easier.
    
    
    
    
     LDX JSTX               \ Set X to the current rate of roll in JSTX
    
     JSR cntr               \ Apply keyboard damping twice (if enabled) so the roll
     JSR cntr               \ rate in X creeps towards the centre by 2
    
                            \ The roll rate in JSTX increases if we press ">" (and
                            \ the RL indicator on the dashboard goes to the right)
                            \
                            \ This rolls our ship to the right (clockwise), but we
                            \ actually implement this by rolling everything else
                            \ to the left (anti-clockwise), so a positive roll rate
                            \ in JSTX translates to a negative roll angle alpha
    
     TXA                    \ Set A and Y to the roll rate but with the sign bit
     EOR #%10000000         \ flipped (i.e. set them to the sign we want for alpha)
     TAY
    
     AND #%10000000         \ Extract the flipped sign of the roll rate and store
     STA ALP2               \ in ALP2 (so ALP2 contains the sign of the roll angle
                            \ alpha)
    
     STX JSTX               \ Update JSTX with the damped value that's still in X
    
     EOR #%10000000         \ Extract the correct sign of the roll rate and store
     STA ALP2+1             \ in ALP2+1 (so ALP2+1 contains the flipped sign of the
                            \ roll angle alpha)
    
     TYA                    \ Set A to the roll rate but with the sign bit flipped
    
     BPL P%+7               \ If the value of A is positive, skip the following
                            \ three instructions
    
     EOR #%11111111         \ A is negative, so change the sign of A using two's
     CLC                    \ complement so that A is now positive and contains
     ADC #1                 \ the absolute value of the roll rate, i.e. |JSTX|
    
     LSR A                  \ Divide the (positive) roll rate in A by 4
     LSR A
    
     CMP #8                 \ If A >= 8, skip the following instruction
     BCS P%+3
    
     LSR A                  \ A < 8, so halve A again
    
     STA ALP1               \ Store A in ALP1, so we now have:
                            \
                            \   ALP1 = |JSTX| / 8    if |JSTX| < 32
                            \
                            \   ALP1 = |JSTX| / 4    if |JSTX| >= 32
                            \
                            \ This means that at lower roll rates, the roll angle is
                            \ reduced closer to zero than at higher roll rates,
                            \ which gives us finer control over the ship's roll at
                            \ lower roll rates
                            \
                            \ Because JSTX is in the range -127 to +127, ALP1 is
                            \ in the range 0 to 31
    
     ORA ALP2               \ Store A in ALPHA, but with the sign set to ALP2 (so
     STA ALPHA              \ ALPHA has a different sign to the actual roll rate)
    
     LDX JSTY               \ Set X to the current rate of pitch in JSTY
    
     JSR cntr               \ Apply keyboard damping so the pitch rate in X creeps
                            \ towards the centre by 1
    
     TXA                    \ Set A and Y to the pitch rate but with the sign bit
     EOR #%10000000         \ flipped
     TAY
    
     AND #%10000000         \ Extract the flipped sign of the pitch rate into A
    
     STX JSTY               \ Update JSTY with the damped value that's still in X
    
     STA BET2+1             \ Store the flipped sign of the pitch rate in BET2+1
    
     EOR #%10000000         \ Extract the correct sign of the pitch rate and store
     STA BET2               \ it in BET2
    
     TYA                    \ Set A to the pitch rate but with the sign bit flipped
    
     BPL P%+4               \ If the value of A is positive, skip the following
                            \ instruction
    
     EOR #%11111111         \ A is negative, so flip the bits
    
     ADC #4                 \ Add 4 to the (positive) pitch rate, so the maximum
                            \ value is now up to 131 (rather than 127)
    
     LSR A                  \ Divide the (positive) pitch rate in A by 16
     LSR A
     LSR A
     LSR A
    
     CMP #3                 \ If A >= 3, skip the following instruction
     BCS P%+3
    
     LSR A                  \ A < 3, so halve A again
    
     STA BET1               \ Store A in BET1, so we now have:
                            \
                            \   BET1 = |JSTY| / 32    if |JSTY| < 48
                            \
                            \   BET1 = |JSTY| / 16    if |JSTY| >= 48
                            \
                            \ This means that at lower pitch rates, the pitch angle
                            \ is reduced closer to zero than at higher pitch rates,
                            \ which gives us finer control over the ship's pitch at
                            \ lower pitch rates
                            \
                            \ Because JSTY is in the range -131 to +131, BET1 is in
                            \ the range 0 to 8
    
     ORA BET2               \ Store A in BETA, but with the sign set to BET2 (so
     STA BETA               \ BETA has the same sign as the actual pitch rate)
    
     LDA BSTK               \ If BSTK = 0 then the Bitstik is not configured, so
     BEQ BS2                \ jump to BS2 to skip the following
    
     LDA JOPOS+2            \ Fetch the Bitstik rotation value (high byte) from
                            \ JOPOS+2, which is constantly updated with the high
                            \ byte of ADC channel 3 by the interrupt handler IRQ1
    
     LSR A                  \ Divide A by 4
     LSR A
    
     CMP #40                \ If A < 40, skip the following instruction
     BCC P%+4
    
     LDA #40                \ Set A = 40, which ensures a maximum speed of 40
    
     STA DELTA              \ Update our speed in DELTA
    
     BNE MA4                \ If the speed we just set is non-zero, then jump to MA4
                            \ to skip the following, as we don't need to check the
                            \ keyboard for speed keys, otherwise do check the
                            \ keyboard (so Bitstik users can still use the keyboard
                            \ for speed adjustments if they twist the stick to zero)
    
    
    
    
           Name: Main flight loop (Part 3 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Scan for flight keys and process the results
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_3_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_3_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Scan for flight keys and process the results
    
     Flight keys are logged in the key logger at location KY1 onwards, with a
     non-zero value in the relevant location indicating a key press.
    
     The key presses that are processed are as follows:
    
       * Space and "?" to speed up and slow down
       * "U", "T" and "M" to disarm, arm and fire missiles
       * TAB to fire an energy bomb
       * ESCAPE to launch an escape pod
       * "J" to initiate an in-system jump
       * "E" to deploy E.C.M. anti-missile countermeasures
       * "C" to use the docking computer
       * "A" to fire lasers
    
    
    
    
    .BS2
    
     LDA KY2                \ If Space is being pressed, keep going, otherwise jump
     BEQ MA17               \ down to MA17 to skip the following
    
     LDA DELTA              \ The "go faster" key is being pressed, so first we
     CMP #40                \ fetch the current speed from DELTA into A, and if
     BCS MA17               \ A >= 40, we are already going at full pelt, so jump
                            \ down to MA17 to skip the following
    
     INC DELTA              \ We can go a bit faster, so increment the speed in
                            \ location DELTA
    
    .MA17
    
     LDA KY1                \ If "?" is being pressed, keep going, otherwise jump
     BEQ MA4                \ down to MA4 to skip the following
    
     DEC DELTA              \ The "slow down" key is being pressed, so we decrement
                            \ the current ship speed in DELTA
    
     BNE MA4                \ If the speed is still greater than zero, jump to MA4
    
     INC DELTA              \ Otherwise we just braked a little too hard, so bump
                            \ the speed back up to the minimum value of 1
    
    .MA4
    
     LDA KY15               \ If "U" is being pressed and the number of missiles
     AND NOMSL              \ in NOMSL is non-zero, keep going, otherwise jump down
     BEQ MA20               \ to MA20 to skip the following
    
     LDY #GREEN2            \ The "disarm missiles" key is being pressed, so call
     JSR ABORT              \ ABORT to disarm the missile and update the missile
                            \ indicators on the dashboard to green (Y = &EE)
    
     JSR BOOP               \ Call the BOOP routine to make a low, long beep to
                            \ indicate the missile is now disarmed
    
     LDA #0                 \ Set MSAR to 0 to indicate that no missiles are
     STA MSAR               \ currently armed
    
    .MA20
    
     LDA MSTG               \ If MSTG is positive (i.e. it does not have bit 7 set),
     BPL MA25               \ then it indicates we already have a missile locked on
                            \ a target (in which case MSTG contains the ship number
                            \ of the target), so jump to MA25 to skip targeting. Or
                            \ to put it another way, if MSTG = &FF, which means
                            \ there is no current target lock, keep going
    
     LDA KY14               \ If "T" is being pressed, keep going, otherwise jump
     BEQ MA25               \ down to MA25 to skip the following
    
     LDX NOMSL              \ If the number of missiles in NOMSL is zero, jump down
     BEQ MA25               \ to MA25 to skip the following
    
     STA MSAR               \ The "target missile" key is being pressed and we have
                            \ at least one missile, so set MSAR = &FF to denote that
                            \ our missile is currently armed (we know A has the
                            \ value &FF, as we just loaded it from MSTG and checked
                            \ that it was negative)
    
     LDY #YELLOW2           \ Change the leftmost missile indicator to yellow
     JSR MSBAR              \ on the missile bar (this call changes the leftmost
                            \ indicator because we set X to the number of missiles
                            \ in NOMSL above, and the indicators are numbered from
                            \ right to left, so X is the number of the leftmost
                            \ indicator)
    
    .MA25
    
     LDA KY16               \ If "M" is being pressed, keep going, otherwise jump
     BEQ MA24               \ down to MA24 to skip the following
    
     LDA MSTG               \ If MSTG = &FF then there is no target lock, so jump to
     BMI MA64               \ MA64 to skip the following (also skipping the checks
                            \ for TAB, ESCAPE, "J" and "E")
    
     JSR FRMIS              \ The "fire missile" key is being pressed and we have
                            \ a missile lock, so call the FRMIS routine to fire
                            \ the missile
    
    .MA24
    
     LDA KY12               \ If TAB is being pressed, keep going, otherwise jump
     BEQ MA76               \ down to MA76 to skip the following
    
     LDA BOMB               \ If we already set off our energy bomb, then BOMB is
     BMI MA76               \ negative, so this skips to MA76 if our energy bomb is
                            \ already going off
    
     ASL BOMB               \ The "energy bomb" key is being pressed, so double
                            \ the value in BOMB. If we have an energy bomb fitted,
                            \ BOMB will contain &7F (%01111111) before this shift
                            \ and will contain &FE (%11111110) after the shift; if
                            \ we don't have an energy bomb fitted, BOMB will still
                            \ contain 0. The bomb explosion is dealt with in the
                            \ MAL1 routine below - this just registers the fact that
                            \ we've set the bomb ticking
    
     BEQ MA76               \ If BOMB now contains 0, then the bomb is not going off
                            \ any more (or it never was), so skip the following
                            \ instruction
    
     JSR BOMBON             \ Call BOMBON to set up and display a new energy bomb
                            \ zig-zag lightning bolt
    
    .MA76
    
     LDA KY20               \ If "P" is being pressed, keep going, otherwise skip
     BEQ MA78               \ the next two instructions
    
     LDA #0                 \ The "cancel docking computer" key is bring pressed,
     STA auto               \ so turn it off by setting auto to 0
    
    \JSR stopbd             \ This instruction is commented out in the original
                            \ source
    
    .MA78
    
     LDA KY13               \ If ESCAPE is being pressed and we have an escape pod
     AND ESCP               \ fitted, keep going, otherwise jump to noescp to skip
     BEQ noescp             \ the following instructions
    
     LDA MJ                 \ If we are in witchspace, we can't launch our escape
     BNE noescp             \ pod, so jump down to noescp
    
     JMP ESCAPE             \ The button is being pressed to launch an escape pod
                            \ and we have an escape pod fitted, so jump to ESCAPE to
                            \ launch it, and exit the main flight loop using a tail
                            \ call
    
    .noescp
    
     LDA KY18               \ If "J" is being pressed, keep going, otherwise skip
     BEQ P%+5               \ the next instruction
    
     JSR WARP               \ Call the WARP routine to do an in-system jump
    
     LDA KY17               \ If "E" is being pressed and we have an E.C.M. fitted,
     AND ECM                \ keep going, otherwise jump down to MA64 to skip the
     BEQ MA64               \ following
    
     LDA ECMA               \ If ECMA is non-zero, that means an E.C.M. is already
     BNE MA64               \ operating and is counting down (this can be either
                            \ our E.C.M. or an opponent's), so jump down to MA64 to
                            \ skip the following (as we can't have two E.C.M.
                            \ systems operating at the same time)
    
     DEC ECMP               \ The E.C.M. button is being pressed and nobody else
                            \ is operating their E.C.M., so decrease the value of
                            \ ECMP to make it non-zero, to denote that our E.C.M.
                            \ is now on
    
     JSR ECBLB2             \ Call ECBLB2 to light up the E.C.M. indicator bulb on
                            \ the dashboard, set the E.C.M. countdown timer to 32,
                            \ and start making the E.C.M. sound
    
    .MA64
    
     LDA KY19               \ If "C" is being pressed, and we have a docking
     AND DKCMP              \ computer fitted, keep going, otherwise jump down to
     BEQ MA68               \ MA68 to skip the following
    
     STA auto               \ Set auto to the non-zero value of A, so the docking
                            \ computer is activated
    
    \EOR KLO+&29            \ These instructions are commented out in the original
    \BEQ MA68               \ source
    \
    \STA auto
    \
    \JSR startbd
    \
    \kill phantom Cs
    
    .MA68
    
     LDA #0                 \ Set LAS = 0, to switch the laser off while we do the
     STA LAS                \ following logic
    
     STA DELT4              \ Take the 16-bit value (DELTA 0) - i.e. a two-byte
     LDA DELTA              \ number with DELTA as the high byte and 0 as the low
     LSR A                  \ byte - and divide it by 4, storing the 16-bit result
     ROR DELT4              \ in DELT4(1 0). This has the effect of storing the
     LSR A                  \ current speed * 64 in the 16-bit location DELT4(1 0)
     ROR DELT4
     STA DELT4+1
    
     LDA LASCT              \ If LASCT is zero, keep going, otherwise the laser is
     BNE MA3                \ a pulse laser that is between pulses, so jump down to
                            \ MA3 to skip the following
    
     LDA KY7                \ If "A" is being pressed, keep going, otherwise jump
     BEQ MA3                \ down to MA3 to skip the following
    
     LDA GNTMP              \ If the laser temperature >= 242 then the laser has
     CMP #242               \ overheated, so jump down to MA3 to skip the following
     BCS MA3
    
     LDX VIEW               \ If the current space view has a laser fitted (i.e. the
     LDA LASER,X            \ laser power for this view is greater than zero), then
     BEQ MA3                \ keep going, otherwise jump down to MA3 to skip the
                            \ following
    
                            \ If we get here, then the "fire" button is being
                            \ pressed, our laser hasn't overheated and isn't already
                            \ being fired, and we actually have a laser fitted to
                            \ the current space view, so it's time to hit me with
                            \ those laser beams
    
     PHA                    \ Store the current view's laser power on the stack
    
     AND #%01111111         \ Set LAS and LAS2 to bits 0-6 of the laser power
     STA LAS
     STA LAS2
    
     JSR LASNO              \ Call the LASNO routine to make the sound of our laser
                            \ firing
    
     JSR LASLI              \ Call LASLI to draw the laser lines
    
     PLA                    \ Restore the current view's laser power into A
    
     BPL ma1                \ If the laser power has bit 7 set, then it's an "always
                            \ on" laser rather than a pulsing laser, so keep going,
                            \ otherwise jump down to ma1 to skip the following
                            \ instruction
    
     LDA #0                 \ This is an "always on" laser (i.e. a beam laser or a
                            \ military laser), so set A = 0, which will be stored in
                            \ LASCT to denote that this is not a pulsing laser
    
    .ma1
    
     AND #%11111010         \ LASCT will be set to 0 for beam lasers, and to the
     STA LASCT              \ laser power AND %11111010 for pulse lasers, which
                            \ comes to 10 for pulse lasers (as pulse lasers have a
                            \ power of 15) or 50 for mining lasers (as mining
                            \ lasers hava a power of 50). See MA23 in part 16 for
                            \ more on laser pulsing and LASCT
    
    
    
    
           Name: Main flight loop (Part 4 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Copy the ship's data block from K% to the
                 zero-page workspace at INWK
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_4_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_4_of_16.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KS1](../main/subroutine/ks1.md) calls via MAL1
                 * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md) calls via MAL1
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Start looping through all the ships in the local bubble, and for each
         one:
    
         * Copy the ship's data block from K% to INWK
    
         * Set XX0 to point to the ship's blueprint (if this is a ship)
    
    
    
    * * *
    
    
     Other entry points:
    
       MAL1                 Marks the beginning of the ship analysis loop, so we
                            can jump back here from part 12 of the main flight loop
                            to work our way through each ship in the local bubble.
                            We also jump back here when a ship is removed from the
                            bubble, so we can continue processing from the next ship
    
    
    
    
    .MA3
    
     LDX #0                 \ We're about to work our way through all the ships in
                            \ our local bubble of universe, so set a counter in X,
                            \ starting from 0, to refer to each ship slot in turn
    
    .MAL1
    
     STX XSAV               \ Store the current slot number in XSAV
    
     LDA FRIN,X             \ Fetch the contents of this slot into A. If it is 0
     BNE P%+5               \ then this slot is empty and we have no more ships to
     JMP MA18               \ process, so jump to MA18 below, otherwise A contains
                            \ the type of ship that's in this slot, so skip over the
                            \ JMP MA18 instruction and keep going
    
     STA TYPE               \ Store the ship type in TYPE
    
     JSR GINF               \ Call GINF to fetch the address of the ship data block
                            \ for the ship in slot X and store it in INF. The data
                            \ block is in the K% workspace, which is where all the
                            \ ship data blocks are stored
    
                            \ Next we want to copy the ship data block from INF to
                            \ the zero-page workspace at INWK, so we can process it
                            \ more efficiently
    
     LDY #NI%-1             \ There are NI% bytes in each ship data block (and in
                            \ the INWK workspace, so we set a counter in Y so we can
                            \ loop through them
    
    .MAL2
    
     LDA (INF),Y            \ Load the Y-th byte of INF and store it in the Y-th
     STA INWK,Y             \ byte of INWK
    
     DEY                    \ Decrement the loop counter
    
     BPL MAL2               \ Loop back for the next byte until we have copied the
                            \ last byte from INF to INWK
    
     LDA TYPE               \ If the ship type is negative then this indicates a
     BMI MA21               \ planet or sun, so jump down to MA21, as the next bit
                            \ sets up a pointer to the ship blueprint, and then
                            \ checks for energy bomb damage, and neither of these
                            \ apply to planets and suns
    
     ASL A                  \ Set Y = ship type * 2
     TAY
    
     LDA XX21-2,Y           \ The ship blueprints at XX21 start with a lookup
     STA XX0                \ table that points to the individual ship blueprints,
                            \ so this fetches the low byte of this particular ship
                            \ type's blueprint and stores it in XX0
    
     LDA XX21-1,Y           \ Fetch the high byte of this particular ship type's
     STA XX0+1              \ blueprint and store it in XX0+1
    
    
    
    
           Name: Main flight loop (Part 5 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: If an energy bomb has been set off,
                 potentially kill this ship
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_5_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_5_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * If an energy bomb has been set off and this ship can be killed, kill it
           and increase the kill tally
    
    
    
    
     LDA BOMB               \ If we set off our energy bomb (see MA24 above), then
     BPL MA21               \ BOMB is now negative, so this skips to MA21 if our
                            \ energy bomb is not going off
    
     CPY #2*SST             \ If the ship in Y is the space station, jump to BA21
     BEQ MA21               \ as energy bombs are useless against space stations
    
     CPY #2*THG             \ If the ship in Y is a Thargoid, jump to BA21 as energy
     BEQ MA21               \ bombs have no effect against Thargoids
    
     CPY #2*CON             \ If the ship in Y is the Constrictor, jump to BA21
     BCS MA21               \ as energy bombs are useless against the Constrictor
                            \ (the Constrictor is the target of mission 1, and it
                            \ would be too easy if it could just be blown out of
                            \ the sky with a single key press)
    
     LDA INWK+31            \ If the ship we are checking has bit 5 set in its ship
     AND #%00100000         \ byte #31, then it is already exploding, so jump to
     BNE MA21               \ BA21 as ships can't explode more than once
    
     ASL INWK+31            \ The energy bomb is killing this ship, so set bit 7 of
     SEC                    \ the ship byte #31 to indicate that it has now been
     ROR INWK+31            \ killed
    
     LDX TYPE               \ Set X to the type of the ship that was killed so the
                            \ following call to EXNO2 can award us the correct
                            \ number of fractional kill points
    
     JSR EXNO2              \ Call EXNO2 to process the fact that we have killed a
                            \ ship (so increase the kill tally, make an explosion
                            \ sound and possibly display "RIGHT ON COMMANDER!")
    
    
    
    
           Name: Main flight loop (Part 6 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Move the ship in space and copy the updated
                 INWK data block back to K%
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Program flow of the ship-moving routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_ship-moving_routine.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_6_of_16.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Move the ship in space
    
         * Copy the updated ship's data block from INWK back to K%
    
    
    
    
    .MA21
    
     JSR MVEIT              \ Call MVEIT to move the ship we are processing in space
    
                            \ Now that we are done processing this ship, we need to
                            \ copy the ship data back from INWK to the correct place
                            \ in the K% workspace. We already set INF in part 4 to
                            \ point to the ship's data block in K%, so we can simply
                            \ do the reverse of the copy we did before, this time
                            \ copying from INWK to INF
    
     LDY #NI%-1             \ Set a counter in Y so we can loop through the NI%
                            \ bytes in the ship data block
    
    .MAL3
    
     LDA INWK,Y             \ Load the Y-th byte of INWK and store it in the Y-th
     STA (INF),Y            \ byte of INF
    
     DEY                    \ Decrement the loop counter
    
     BPL MAL3               \ Loop back for the next byte, until we have copied the
                            \ last byte from INWK back to INF
    
    
    
    
           Name: Main flight loop (Part 7 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Check whether we are docking, scooping or
                 colliding with it
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_7_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_7_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Check how close we are to this ship and work out if we are docking,
           scooping or colliding with it
    
    
    
    
     LDA INWK+31            \ Fetch the status of this ship from bits 5 (is ship
     AND #%10100000         \ exploding?) and bit 7 (has ship been killed?) from
                            \ ship byte #31 into A
    
     JSR MAS4               \ Or this value with x_hi, y_hi and z_hi
    
     BNE MA65               \ If this value is non-zero, then either the ship is
                            \ far away (i.e. has a non-zero high byte in at least
                            \ one of the three axes), or it is already exploding,
                            \ or has been flagged as being killed - in which case
                            \ jump to MA65 to skip the following, as we can't dock
                            \ scoop or collide with it
    
     LDA INWK               \ Set A = (x_lo OR y_lo OR z_lo), and if bit 7 of the
     ORA INWK+3             \ result is set, the ship is still a fair distance
     ORA INWK+6             \ away (further than 127 in at least one axis), so jump
     BMI MA65               \ to MA65 to skip the following, as it's too far away to
                            \ dock, scoop or collide with
    
     LDX TYPE               \ If the current ship type is negative then it's either
     BMI MA65               \ a planet or a sun, so jump down to MA65 to skip the
                            \ following, as we can't dock with it or scoop it
    
     CPX #SST               \ If this ship is the space station, jump to ISDK to
     BEQ ISDK               \ check whether we are docking with it
    
     AND #%11000000         \ If bit 6 of (x_lo OR y_lo OR z_lo) is set, then the
     BNE MA65               \ ship is still a reasonable distance away (further than
                            \ 63 in at least one axis), so jump to MA65 to skip the
                            \ following, as it's too far away to dock, scoop or
                            \ collide with
    
     CPX #MSL               \ If this ship is a missile, jump down to MA65 to skip
     BEQ MA65               \ the following, as we can't scoop or dock with a
                            \ missile, and it has its own dedicated collision
                            \ checks in the TACTICS routine
    
     LDA BST                \ If we have fuel scoops fitted then BST will be &FF,
                            \ otherwise it will be 0
    
     AND INWK+5             \ Ship byte #5 contains the y_sign of this ship, so a
                            \ negative value here means the canister is below us,
                            \ which means the result of the AND will be negative if
                            \ the canister is below us and we have a fuel scoop
                            \ fitted
    
     BPL MA58               \ If the result is positive, then we either have no
                            \ scoop or the canister is above us, and in both cases
                            \ this means we can't scoop the item, so jump to MA58
                            \ to process a collision
    
    
    
    
           Name: Main flight loop (Part 8 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Process us potentially scooping this item
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_8_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_8_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Process us potentially scooping this item
    
    
    
    
     CPX #OIL               \ If this is a cargo canister, jump to oily to randomly
     BEQ oily               \ decide the canister's contents
    
     LDY #0                 \ Fetch byte #0 of the ship's blueprint
     LDA (XX0),Y
    
     LSR A                  \ Shift it right four times, so A now contains the high
     LSR A                  \ nibble (i.e. bits 4-7)
     LSR A
     LSR A
    
     BEQ MA58               \ If A = 0, jump to MA58 to skip all the docking and
                            \ scooping checks
    
                            \ Only the Thargon, alloy plate, splinter and escape pod
                            \ have non-zero high nibbles in their blueprint byte #0
                            \ so if we get here, our ship is one of those, and the
                            \ high nibble gives the market item number of the item
                            \ when scooped, less 1
    
     ADC #1                 \ Add 1 to the high nibble to get the market item
                            \ number
    
     BNE slvy2              \ Skip to slvy2 so we scoop the ship as a market item
    
    .oily
    
     JSR DORND              \ Set A and X to random numbers and reduce A to a
     AND #7                 \ random number in the range 0-7
    
    .slvy2
    
                            \ By the time we get here, we are scooping, and A
                            \ contains the type of item we are scooping (a random
                            \ number 0-7 if we are scooping a cargo canister, 3 if
                            \ we are scooping an escape pod, or 16 if we are
                            \ scooping a Thargon). These numbers correspond to the
                            \ relevant market items (see QQ23 for a list), so a
                            \ cargo canister can contain anything from food to
                            \ computers, while escape pods contain slaves, and
                            \ Thargons become alien items when scooped
    
     JSR tnpr1              \ Call tnpr1 with the scooped cargo type stored in A
                            \ to work out whether we have room in the hold for one
                            \ tonne of this cargo (A is set to 1 by this call, and
                            \ the C flag contains the result)
    
     LDY #78                \ This instruction has no effect, so presumably it used
                            \ to do something, but didn't get removed
    
     BCS MA59               \ If the C flag is set then we have no room in the hold
                            \ for the scooped item, so jump down to MA59 make a
                            \ sound to indicate failure, before destroying the
                            \ canister
    
     LDY QQ29               \ Scooping was successful, so set Y to the type of
                            \ item we just scooped, which we stored in QQ29 above
    
     ADC QQ20,Y             \ Add A (which we set to 1 above) to the number of items
     STA QQ20,Y             \ of type Y in the cargo hold, as we just successfully
                            \ scooped one canister of type Y
    
     TYA                    \ Print recursive token 48 + Y as an in-flight token,
     ADC #208               \ which will be in the range 48 ("FOOD") to 64 ("ALIEN
     JSR MESS               \ ITEMS"), so this prints the scooped item's name
    
     ASL NEWB               \ The item has now been scooped, so set bit 7 of its
     SEC                    \ NEWB flags to indicate this
     ROR NEWB
    
    .MA65
    
     JMP MA26               \ If we get here, then the ship we are processing was
                            \ too far away to be scooped, docked or collided with,
                            \ so jump to MA26 to skip over the collision routines
                            \ and move on to missile targeting
    
    
    
    
           Name: Main flight loop (Part 9 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: If it is a space station, check whether we
                 are successfully docking with it
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Docking checks](https://elite.bbcelite.com/deep_dives/docking_checks.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_9_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_9_of_16.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ESCAPE](../main/subroutine/escape.md) calls via GOIN
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Process docking with a space station
    
    
    
    * * *
    
    
     Other entry points:
    
       GOIN                 We jump here from part 3 of the main flight loop if the
                            docking computer is activated by pressing "C"
    
    
    
    
    .ISDK
    
     LDA K%+NI%+36          \ 1. Fetch the NEWB flags (byte #36) of the second ship
     AND #%00000100         \ in the ship data workspace at K%, which is reserved
     BNE MA62               \ for the sun or the space station (in this case it's
                            \ the latter), and if bit 2 is set, meaning the station
                            \ is hostile, jump down to MA62 to fail docking (so
                            \ trying to dock at a station that we have annoyed does
                            \ not end well)
    
     LDA INWK+14            \ 2. If nosev_z_hi < 214, jump down to MA62 to fail
     CMP #214               \ docking, as the angle of approach is greater than 26
     BCC MA62               \ degrees
    
     JSR SPS1               \ Call SPS1 to calculate the vector to the planet and
                            \ store it in XX15
    
     LDA XX15+2             \ Set A to the z-axis of the vector
    
                            \ This version of Elite omits check 3 (which would check
                            \ the sign of the z-axis)
    
     CMP #89                \ 4. If z-axis < 89, jump to MA62 to fail docking, as
     BCC MA62               \ we are not in the 22.0 degree safe cone of approach
    
     LDA INWK+16            \ 5. If |roofv_x_hi| < 80, jump to MA62 to fail docking,
     AND #%01111111         \ as the slot is more than 36.6 degrees from horizontal
     CMP #80
     BCC MA62
    
    .GOIN
    
                            \ If we arrive here, we just docked successfully
    
    \JSR stopbd             \ This instruction is commented out in the original
                            \ source
    
     JMP DOENTRY            \ Go to the docking bay (i.e. show the ship hangar)
    
    .MA62
    
                            \ If we arrive here, docking has just failed
    
     LDA DELTA              \ If the ship's speed is < 5, jump to MA67 to register
     CMP #5                 \ some damage, but not a huge amount
     BCC MA67
    
     JMP DEATH              \ Otherwise we have just crashed into the station, so
                            \ process our death
    
    
    
    
           Name: Main flight loop (Part 10 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Remove if scooped, or process collisions
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_10_of_16.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Remove scooped item after both successful and failed scooping attempts
    
         * Process collisions
    
    
    
    
    .MA59
    
                            \ If we get here then scooping failed
    
     JSR EXNO3              \ Make the sound of the cargo canister being destroyed
                            \ and fall through into MA60 to remove the canister
                            \ from our local bubble
    
    .MA60
    
                            \ If we get here then scooping was successful
    
     ASL INWK+31            \ Set bit 7 of the scooped or destroyed item, to denote
     SEC                    \ that it has been killed and should be removed from
     ROR INWK+31            \ the local bubble
    
    .MA61
    
     BNE MA26               \ Jump to MA26 to skip over the collision routines and
                            \ to move on to missile targeting (this BNE is
                            \ effectively a JMP as A will never be zero)
    
    .MA67
    
                            \ If we get here then we have collided with something,
                            \ but not fatally
    
     LDA #1                 \ Set the speed in DELTA to 1 (i.e. a sudden stop)
     STA DELTA
    
     LDA #5                 \ Set the amount of damage in A to 5 (a small dent) and
     BNE MA63               \ jump down to MA63 to process the damage (this BNE is
                            \ effectively a JMP as A will never be zero)
    
    .MA58
    
                            \ If we get here, we have collided with something in a
                            \ potentially fatal way
    
     ASL INWK+31            \ Set bit 7 of the ship we just collided with, to
     SEC                    \ denote that it has been killed and should be removed
     ROR INWK+31            \ from the local bubble
    
     LDA INWK+35            \ Load A with the energy level of the ship we just hit
    
     SEC                    \ Set the amount of damage in A to 128 + A / 2, so
     ROR A                  \ this is quite a big dent, and colliding with higher
                            \ energy ships will cause more damage
    
    .MA63
    
     JSR OOPS               \ The amount of damage is in A, so call OOPS to reduce
                            \ our shields, and if the shields are gone, there's a
                            \ chance of cargo loss or even death
    
     JSR EXNO3              \ Make the sound of colliding with the other ship and
                            \ fall through into MA26 to try targeting a missile
    
    
    
    
           Name: Main flight loop (Part 11 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Process missile lock and firing our laser
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Flipping axes between space views](https://elite.bbcelite.com/deep_dives/flipping_axes_between_space_views.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_11_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_11_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * If this is not the front space view, flip the axes of the ship's
            coordinates in INWK
    
         * Process missile lock
    
         * Process our laser firing
    
    
    
    
    .MA26
    
     LDA NEWB               \ If bit 7 of the ship's NEWB flags is clear, skip the
     BPL P%+5               \ following instruction
    
     JSR SCAN               \ Bit 7 of the ship's NEWB flags is set, which means the
                            \ ship has docked or been scooped, so we draw the ship
                            \ on the scanner, which has the effect of removing it
    
     LDA QQ11               \ If this is not a space view, jump to MA15 to skip
     BNE MA15               \ missile and laser locking
    
     JSR PLUT               \ Call PLUT to update the geometric axes in INWK to
                            \ match the view (front, rear, left, right)
    
     JSR HITCH              \ Call HITCH to see if this ship is in the crosshairs,
     BCC MA8                \ in which case the C flag will be set (so if there is
                            \ no missile or laser lock, we jump to MA8 to skip the
                            \ following)
    
     LDA MSAR               \ We have missile lock, so check whether the leftmost
     BEQ MA47               \ missile is currently armed, and if not, jump to MA47
                            \ to process laser fire, as we can't lock an unarmed
                            \ missile
    
     JSR BEEP               \ We have missile lock and an armed missile, so call
                            \ the BEEP subroutine to make a short, high beep
    
     LDX XSAV               \ Call ABORT2 to store the details of this missile
     LDY #RED2              \ lock, with the targeted ship's slot number in X
     JSR ABORT2             \ (which we stored in XSAV at the start of this ship's
                            \ loop at MAL1), and set the colour of the missile
                            \ indicator to the colour in Y (red = &0E)
    
    .MA47
    
                            \ If we get here then the ship is in our sights, but
                            \ we didn't lock a missile, so let's see if we're
                            \ firing the laser
    
     LDA LAS                \ If we are firing the laser then LAS will contain the
     BEQ MA8                \ laser power (which we set in MA68 above), so if this
                            \ is zero, jump down to MA8 to skip the following
    
     LDX #15                \ We are firing our laser and the ship in INWK is in
     JSR EXNO               \ the crosshairs, so call EXNO to make the sound of
                            \ us making a laser strike on another ship
    
     LDA TYPE               \ Did we just hit the space station? If so, jump to
     CMP #SST               \ MA14+2 to make the station hostile, skipping the
     BEQ MA14+2             \ following as we can't destroy a space station
    
     CMP #CON               \ If the ship we hit is less than #CON - i.e. it's not
     BCC BURN               \ a Constrictor, Cougar, Dodo station or the Elite logo,
                            \ jump to BURN to skip the following
    
     LDA LAS                \ Set A to the power of the laser we just used to hit
                            \ the ship (i.e. the laser in the current view)
    
     CMP #(Armlas AND 127)  \ If the laser is not a military laser, jump to MA14+2
     BNE MA14+2             \ to skip the following, as only military lasers have
                            \ any effect on the Constrictor or Cougar
    
     LSR LAS                \ Divide the laser power of the current view by 4, so
     LSR LAS                \ the damage inflicted on the super-ship is a quarter of
                            \ the damage our military lasers would inflict on a
                            \ normal ship
    
    .BURN
    
     LDA INWK+35            \ Fetch the hit ship's energy from byte #35 and subtract
     SEC                    \ our current laser power, and if the result is greater
     SBC LAS                \ than zero, the other ship has survived the hit, so
     BCS MA14               \ jump down to MA14 to make it angry
    
     ASL INWK+31            \ Set bit 7 of the ship byte #31 to indicate that it has
     SEC                    \ now been killed
     ROR INWK+31
    
     LDA TYPE               \ Did we just kill an asteroid? If not, jump to nosp,
     CMP #AST               \ otherwise keep going
     BNE nosp
    
     LDA LAS                \ Did we kill the asteroid using mining lasers? If not,
     CMP #Mlas              \ jump to nosp, otherwise keep going
     BNE nosp
    
     JSR DORND              \ Set A and X to random numbers
    
     LDX #SPL               \ Set X to the ship type for a splinter
    
     AND #3                 \ Reduce the random number in A to the range 0-3
    
     JSR SPIN2              \ Call SPIN2 to spawn A items of type X (i.e. spawn
                            \ 0-3 splinters)
    
    .nosp
    
     LDY #PLT               \ Randomly spawn some alloy plates
     JSR SPIN
    
     LDY #OIL               \ Randomly spawn some cargo canisters
     JSR SPIN
    
     LDX TYPE               \ Set X to the type of the ship that was killed so the
                            \ following call to EXNO2 can award us the correct
                            \ number of fractional kill points
    
     JSR EXNO2              \ Call EXNO2 to process the fact that we have killed a
                            \ ship (so increase the kill tally, make an explosion
                            \ sound and so on)
    
    .MA14
    
     STA INWK+35            \ Store the hit ship's updated energy in ship byte #35
    
     LDA TYPE               \ Call ANGRY to make the target ship or station hostile,
     JSR ANGRY              \ and if this is a ship, wake up its AI and give it a
                            \ kick of speed
    
    
    
    
           Name: Main flight loop (Part 12 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Draw the ship, remove if killed, loop back
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_12_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_12_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Draw the ship
    
         * Process removal of killed ships
    
       * Loop back up to MAL1 to move onto the next ship in the local bubble
    
    
    
    
    .MA8
    
     JSR LL9                \ Call LL9 to draw the ship we're processing on-screen
    
    .MA15
    
     LDY #35                \ Fetch the ship's energy from byte #35 and copy it to
     LDA INWK+35            \ byte #35 in INF (so the ship's data in K% gets
     STA (INF),Y            \ updated)
    
     LDA NEWB               \ If bit 7 of the ship's NEWB flags is set, which means
     BMI KS1S               \ the ship has docked or been scooped, jump to KS1S to
                            \ skip the following, as we can't get a bounty for a
                            \ ship that's no longer around
    
     LDA INWK+31            \ If bit 7 of the ship's byte #31 is clear, then the
     BPL MAC1               \ ship hasn't been killed by energy bomb, collision or
                            \ laser fire, so jump to MAC1 to skip the following
    
     AND #%00100000         \ If bit 5 of the ship's byte #31 is clear then the
     BEQ MAC1               \ ship is no longer exploding, so jump to MAC1 to skip
                            \ the following
    
     LDA NEWB               \ Extract bit 6 of the ship's NEWB flags, so A = 64 if
     AND #%01000000         \ bit 6 is set, or 0 if it is clear. Bit 6 is set if
                            \ this ship is a cop, so A = 64 if we just killed a
                            \ policeman, otherwise it is 0
    
     ORA FIST               \ Update our FIST flag ("fugitive/innocent status") to
     STA FIST               \ at least the value in A, which will instantly make us
                            \ a fugitive if we just shot the sheriff, but won't
                            \ affect our status if the enemy wasn't a copper
    
     LDA DLY                \ If we already have an in-flight message on-screen (in
     ORA MJ                 \ which case DLY > 0), or we are in witchspace (in
     BNE KS1S               \ which case MJ > 0), jump to KS1S to skip showing an
                            \ on-screen bounty for this kill
    
     LDY #10                \ Fetch byte #10 of the ship's blueprint, which is the
     LDA (XX0),Y            \ low byte of the bounty awarded when this ship is
     BEQ KS1S               \ killed (in Cr * 10), and if it's zero jump to KS1S as
                            \ there is no on-screen bounty to display
    
     TAX                    \ Put the low byte of the bounty into X
    
     INY                    \ Fetch byte #11 of the ship's blueprint, which is the
     LDA (XX0),Y            \ high byte of the bounty awarded (in Cr * 10), and put
     TAY                    \ it into Y
    
     JSR MCASH              \ Call MCASH to add (Y X) to the cash pot
    
     LDA #0                 \ Print control code 0 (current cash, right-aligned to
     JSR MESS               \ width 9, then " CR", newline) as an in-flight message
    
    .KS1S
    
     JMP KS1                \ Process the killing of this ship (which removes this
                            \ ship from its slot and shuffles all the other ships
                            \ down to close up the gap)
    
    .MAC1
    
     LDA TYPE               \ If the ship we are processing is a planet or sun,
     BMI MA27               \ jump to MA27 to skip the following two instructions
    
     JSR FAROF              \ If the ship we are processing is a long way away (its
     BCC KS1S               \ distance in any one direction is > 224, jump to KS1S
                            \ to remove the ship from our local bubble, as it's just
                            \ left the building
    
    .MA27
    
     LDY #31                \ Fetch the ship's explosion/killed state from byte #31
     LDA INWK+31            \ and copy it to byte #31 in INF (so the ship's data in
     STA (INF),Y            \ K% gets updated)
    
     LDX XSAV               \ We're done processing this ship, so fetch the ship's
                            \ slot number, which we saved in XSAV back at the start
                            \ of the loop
    
     INX                    \ Increment the slot number to move on to the next slot
    
     JMP MAL1               \ And jump back up to the beginning of the loop to get
                            \ the next ship in the local bubble for processing
    
    
    
    
           Name: Main flight loop (Part 13 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Show energy bomb effect, charge shields and energy banks
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Scheduling tasks with the main loop counter](https://elite.bbcelite.com/deep_dives/scheduling_tasks_with_the_main_loop_counter.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_13_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_13_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Show energy bomb effect (if applicable)
    
       * Charge shields and energy banks (every 7 iterations of the main loop)
    
    
    
    
    .MA18
    
     LDA BOMB               \ If we set off our energy bomb (see MA24 above), then
     BPL MA77               \ BOMB is now negative, so this skips to MA21 if our
                            \ energy bomb is not going off
    
     JSR BOMBEFF2           \ Call BOMBEFF2 to erase the energy bomb zig-zag
                            \ lightning bolt that we drew in part 3, make the sound
                            \ of the energy bomb going off, draw a new lightning
                            \ bolt, and repeat the process four times so the bolt
                            \ flashes
    
     ASL BOMB               \ We set off our energy bomb, so rotate BOMB to the
                            \ left by one place. BOMB was rotated left once already
                            \ during this iteration of the main loop, back at MA24,
                            \ so if this is the first pass it will already be
                            \ %11111110, and this will shift it to %11111100 - so
                            \ if we set off an energy bomb, it stays activated
                            \ (BOMB > 0) for four iterations of the main loop
    
     BMI MA77               \ If the result has bit 7 set, skip the following
                            \ instruction as the bomb is still going off
    
     JSR BOMBOFF            \ Our energy bomb has finished going off, so call
                            \ BOMBOFF to draw the zig-zag lightning bolt, which
                            \ erases it from the screen
    
    .MA77
    
     LDA MCNT               \ Fetch the main loop counter and calculate MCNT mod 8,
     AND #7                 \ jumping to MA22 if it is non-zero (so the following
     BNE MA22               \ code only runs every 8 iterations of the main loop)
    
     LDX ENERGY             \ Fetch our ship's energy levels and skip to b if bit 7
     BPL b                  \ is not set, i.e. only charge the shields from the
                            \ energy banks if they are at more than 50% charge
    
     LDX ASH                \ Call SHD to recharge our aft shield and update the
     JSR SHD                \ shield status in ASH
     STX ASH
    
     LDX FSH                \ Call SHD to recharge our forward shield and update
     JSR SHD                \ the shield status in FSH
     STX FSH
    
    .b
    
     SEC                    \ Set A = ENERGY + ENGY + 1, so our ship's energy
     LDA ENGY               \ level goes up by 2 if we have an energy unit fitted,
     ADC ENERGY             \ otherwise it goes up by 1
    
     BCS paen1              \ If the value of A did not overflow (the maximum
     STA ENERGY             \ energy level is &FF), then store A in ENERGY
    
    .paen1
    
    
    
    
           Name: Main flight loop (Part 14 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Spawn a space station if we are close enough to the planet
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Scheduling tasks with the main loop counter](https://elite.bbcelite.com/deep_dives/scheduling_tasks_with_the_main_loop_counter.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [The space station safe zone](https://elite.bbcelite.com/deep_dives/the_space_station_safe_zone.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_14_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_14_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Spawn a space station if we are close enough to the planet (every 32
         iterations of the main loop)
    
    
    
    
     LDA MJ                 \ If we are in witchspace, jump down to MA23S to skip
     BNE MA23S              \ the following, as there are no space stations in
                            \ witchspace
    
     LDA MCNT               \ Fetch the main loop counter and calculate MCNT mod 32,
     AND #31                \ jumping to MA93 if it is on-zero (so the following
     BNE MA93               \ code only runs every 32 iterations of the main loop)
    
     LDA SSPR               \ If we are inside the space station safe zone, jump to
     BNE MA23S              \ MA23S to skip the following, as we already have a
                            \ space station and don't need another
    
     TAY                    \ Set Y = A = 0 (A is 0 as we didn't branch with the
                            \ previous BNE instruction)
    
     JSR MAS2               \ Call MAS2 to calculate the largest distance to the
     BNE MA23S              \ planet in any of the three axes, and if it's
                            \ non-zero, jump to MA23S to skip the following, as we
                            \ are too far from the planet to bump into a space
                            \ station
    
                            \ We now want to spawn a space station, so first we
                            \ need to set up a ship data block for the station in
                            \ INWK that we can then pass to NWSPS to add a new
                            \ station to our bubble of universe. We do this by
                            \ copying the planet data block from K% to INWK so we
                            \ can work on it, but we only need the first 29 bytes,
                            \ as we don't need to worry about bytes #29 to #35
                            \ for planets (as they don't have rotation counters,
                            \ AI, explosions, missiles, a ship line heap or energy
                            \ levels)
    
     LDX #28                \ So we set a counter in X to copy 29 bytes from K%+0
                            \ to K%+28
    
    .MAL4
    
     LDA K%,X               \ Load the X-th byte of K% and store in the X-th byte
     STA INWK,X             \ of the INWK workspace
    
     DEX                    \ Decrement the loop counter
    
     BPL MAL4               \ Loop back for the next byte until we have copied the
                            \ first 28 bytes of K% to INWK
    
                            \ We now check the distance from our ship (at the
                            \ origin) towards the point where we will spawn the
                            \ space station if we are close enough
                            \
                            \ This point is calculated by starting at the planet's
                            \ centre and adding 2 * nosev, which takes us to a point
                            \ above the planet's surface, at an altitude that
                            \ matches the planet's radius
                            \
                            \ This point pitches and rolls around the planet as the
                            \ nosev vector rotates with the planet, and if our ship
                            \ is within a distance of (192 0) from this point in all
                            \ three axes, then we spawn the space station at this
                            \ point, with the station's slot facing towards the
                            \ planet, along the nosev vector
                            \
                            \ This works because in the following, we calculate the
                            \ station's coordinates one axis at a time, and store
                            \ the results in the INWK block, so by the time we have
                            \ calculated and checked all three, the ship data block
                            \ is set up with the correct spawning coordinates
    
     INX                    \ Set X = 0 (as we ended the above loop with X as &FF)
    
     LDY #9                 \ Call MAS1 with X = 0, Y = 9 to do the following:
     JSR MAS1               \
                            \   (x_sign x_hi x_lo) += (nosev_x_hi nosev_x_lo) * 2
                            \
                            \   A = |x_sign|
    
     BNE MA23S              \ If A > 0, jump to MA23S to skip the following, as we
                            \ are too far from the planet in the x-direction to
                            \ bump into a space station
    
     LDX #3                 \ Call MAS1 with X = 3, Y = 11 to do the following:
     LDY #11                \
     JSR MAS1               \   (y_sign y_hi y_lo) += (nosev_y_hi nosev_y_lo) * 2
                            \
                            \   A = |y_sign|
    
     BNE MA23S              \ If A > 0, jump to MA23S to skip the following, as we
                            \ are too far from the planet in the y-direction to
                            \ bump into a space station
    
     LDX #6                 \ Call MAS1 with X = 6, Y = 13 to do the following:
     LDY #13                \
     JSR MAS1               \   (z_sign z_hi z_lo) += (nosev_z_hi nosev_z_lo) * 2
                            \
                            \   A = |z_sign|
    
     BNE MA23S              \ If A > 0, jump to MA23S to skip the following, as we
                            \ are too far from the planet in the z-direction to
                            \ bump into a space station
    
     LDA #192               \ Call FAROF2 to compare x_hi, y_hi and z_hi with 192,
     JSR FAROF2             \ which will set the C flag if all three are < 192, or
                            \ clear the C flag if any of them are >= 192
    
     BCC MA23S              \ Jump to MA23S if any one of x_hi, y_hi or z_hi are
                            \ >= 192 (i.e. they must all be < 192 for us to be near
                            \ enough to the planet to bump into a space station)
    
     JSR WPLS               \ Call WPLS to remove the sun from the screen, as we
                            \ can't have both the sun and the space station at the
                            \ same time
    
     JSR NWSPS              \ Add a new space station to our local bubble of
                            \ universe
    
    .MA23S
    
     JMP MA23               \ Jump to MA23 to skip the following planet and sun
                            \ altitude checks
    
    
    
    
           Name: Main flight loop (Part 15 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Perform altitude checks with the planet and sun and process fuel
                 scooping if appropriate
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Scheduling tasks with the main loop counter](https://elite.bbcelite.com/deep_dives/scheduling_tasks_with_the_main_loop_counter.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_15_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_15_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Perform an altitude check with the planet (every 32 iterations of the main
         loop, on iteration 10 of each 32)
    
       * Perform an altitude check with the sun and process fuel scooping (every
         32 iterations of the main loop, on iteration 20 of each 32)
    
    
    
    
    .MA22
    
     LDA MJ                 \ If we are in witchspace, jump down to MA23S to skip
     BNE MA23S              \ the following, as there are no planets or suns to
                            \ bump into in witchspace
    
     LDA MCNT               \ Fetch the main loop counter and calculate MCNT mod 32,
     AND #31                \ which tells us the position of this loop in each block
                            \ of 32 iterations
    
    .MA93
    
     CMP #10                \ If this is the tenth iteration in this block of 32,
     BNE MA29               \ do the following, otherwise jump to MA29 to skip the
                            \ planet altitude check and move on to the sun distance
                            \ check
    
     LDA #50                \ If our energy bank status in ENERGY is >= 50, skip
     CMP ENERGY             \ printing the following message (so the message is
     BCC P%+6               \ only shown if our energy is low)
    
     ASL A                  \ Print recursive token 100 ("ENERGY LOW{beep}") as an
     JSR MESS               \ in-flight message
    
     LDY #&FF               \ Set our altitude in ALTIT to &FF, the maximum
     STY ALTIT
    
     INY                    \ Set Y = 0
    
     JSR m                  \ Call m to calculate the maximum distance to the
                            \ planet in any of the three axes, returned in A
    
     BNE MA23               \ If A > 0 then we are a fair distance away from the
                            \ planet in at least one axis, so jump to MA23 to skip
                            \ the rest of the altitude check
    
     JSR MAS3               \ Set A = x_hi^2 + y_hi^2 + z_hi^2, so using Pythagoras
                            \ we now know that A now contains the square of the
                            \ distance between our ship (at the origin) and the
                            \ centre of the planet at (x_hi, y_hi, z_hi)
    
     BCS MA23               \ If the C flag was set by MAS3, then the result
                            \ overflowed (was greater than &FF) and we are still a
                            \ fair distance from the planet, so jump to MA23 as we
                            \ haven't crashed into the planet
    
     SBC #36                \ Subtract 37 from x_hi^2 + y_hi^2 + z_hi^2
                            \
                            \ The SBC subtracts 37 as we just passed through a BCS
                            \ so we know the C flag is clear
                            \
                            \ When we do the 3D Pythagoras calculation, we only use
                            \ the high bytes of the coordinates, so that's x_hi,
                            \ y_hi and z_hi and
                            \
                            \ The planet radius is (0 96 0), as defined in the
                            \ PLANET routine, so the high byte is 96
                            \
                            \ When we square the coordinates above and add them,
                            \ the result gets divided by 256 (otherwise the result
                            \ wouldn't fit into one byte), so if we do the same for
                            \ the planet's radius, we get:
                            \
                            \   96 * 96 / 256 = 36
                            \
                            \ So for the planet, the equivalent figure to test the
                            \ sum of the _hi bytes against is 36, so A now contains
                            \ the high byte of our altitude above the planet
                            \ surface, squared, with an extra 1 subtracted so the
                            \ test in the next instruction will ensure we crash
                            \ even if we are exactly one planet radius away
    
     BCC MA28               \ If A < 0 then jump to MA28 as we have crashed into
                            \ the planet
    
     STA R                  \ We are getting close to the planet, so we need to
     JSR LL5                \ work out how close. We know from the above that A
                            \ contains our altitude squared, so we store A in R
                            \ and call LL5 to calculate:
                            \
                            \   Q = SQRT(R Q) = SQRT(A Q)
                            \
                            \ Interestingly, Q doesn't appear to be set to 0 for
                            \ this calculation, so presumably this doesn't make a
                            \ difference
    
     LDA Q                  \ Store the result in ALTIT, our altitude
     STA ALTIT
    
     BNE MA23               \ If our altitude is non-zero then we haven't crashed,
                            \ so jump to MA23 to skip to the next section
    
    .MA28
    
     JMP DEATH              \ If we get here then we just crashed into the planet
                            \ or got too close to the sun, so jump to DEATH to start
                            \ the funeral preparations and return from the main
                            \ flight loop using a tail call
    
    .MA29
    
     CMP #15                \ If this is the 15th iteration in this block of 32,
     BNE MA33               \ do the following, otherwise jump to MA33 to skip the
                            \ docking computer manoeuvring
    
     LDA auto               \ If auto is zero, then the docking computer is not
     BEQ MA23               \ activated, so jump to MA23 to skip to the next
                            \ section
    
     LDA #123               \ Set A = 123 and jump down to MA34 to print token 123
     BNE MA34               \ ("DOCKING COMPUTERS ON") as an in-flight message
    
    .MA33
    
     CMP #20                \ If this is the 20th iteration in this block of 32,
     BNE MA23               \ do the following, otherwise jump to MA23 to skip the
                            \ sun altitude check
    
     LDA #30                \ Set CABTMP to 30, the cabin temperature in deep space
     STA CABTMP             \ (i.e. one notch on the dashboard bar)
    
     LDA SSPR               \ If we are inside the space station safe zone, jump to
     BNE MA23               \ MA23 to skip the following, as we can't have both the
                            \ sun and space station at the same time, so we clearly
                            \ can't be flying near the sun
    
     LDY #NI%               \ Set Y to NI%, which is the offset in K% for the sun's
                            \ data block, as the second block at K% is reserved for
                            \ the sun (or space station)
    
     JSR MAS2               \ Call MAS2 to calculate the largest distance to the
     BNE MA23               \ sun in any of the three axes, and if it's non-zero,
                            \ jump to MA23 to skip the following, as we are too far
                            \ from the sun for scooping or temperature changes
    
     JSR MAS3               \ Set (A ?) = x_hi^2 + y_hi^2 + z_hi^2, so using
                            \ Pythagoras we now know that A now contains the high
                            \ byte of the square of the distance between our ship
                            \ (at the origin) and the heart of the sun at coordinate
                            \ (x_hi, y_hi, z_hi)
                            \
                            \ If the calculation overflows so it doesn't fit into
                            \ one byte, then A is set to &FF and the C flag is set
    
     EOR #%11111111         \ Invert A, so A is now small if we are far from the
                            \ sun and large if we are close to the sun, in the
                            \ range 0 = far away to &FF = extremely close, ouch,
                            \ hot, hot, hot!
    
     ADC #30                \ Add the minimum cabin temperature of 30, plus the C
                            \ flag, so we get one of the following:
                            \
                            \
                            \   * If the MAS3 calculation overflowed then we are a
                            \     long way from the sun, A will be zero and the C
                            \     flag will be set, so this addition sets A = 31
                            \     and clears the C flag
                            \
                            \   * If the result of the MAS3 calculation fitted into
                            \     one byte, then A will be in the range 0 to 255 and
                            \     the C flag will be clear, so this addition has a
                            \     result in the range 0 to 285, with the higher
                            \     values overflowing the addition and setting the
                            \     C flag
                            \
                            \ So the C flag is set if the cabin temperature is too
                            \ hot to handle, and if it's clear then A contains the
                            \ cabin temperature
    
     STA CABTMP             \ Store the updated cabin temperature
    
     BCS MA28               \ If the C flag is set then jump to MA28 to die, as
                            \ our temperature is off the scale
    
     CMP #224               \ If the cabin temperature < 224 then jump to MA23 to
     BCC MA23               \ skip fuel scooping, as we aren't close enough
    
    \CMP #&F0               \ These instructions are commented out in the original
    \BCC nokilltr           \ source
    \
    \LDA #5
    \JSR SETL1
    \
    \LDA VIC+&15
    \AND #&3
    \STA VIC+&15
    \
    \LDA #4
    \JSR SETL1
    \
    \LSR TRIBBLE+1
    \ROR TRIBBLE
    \
    \.nokilltr
    
     LDA BST                \ If we don't have fuel scoops fitted, jump to BA23 to
     BEQ MA23               \ skip fuel scooping, as we can't scoop without fuel
                            \ scoops
    
     LDA DELT4+1            \ We are now successfully fuel scooping, so it's time
     LSR A                  \ to work out how much fuel we're scooping. Fetch the
                            \ high byte of DELT4, which contains our current speed
                            \ divided by 4, and halve it to get our current speed
                            \ divided by 8 (so it's now a value between 1 and 5, as
                            \ our speed is normally between 1 and 40). This gives
                            \ us the amount of fuel that's being scooped in A, so
                            \ the faster we go, the more fuel we scoop, and because
                            \ the fuel levels are stored as 10 * the fuel in light
                            \ years, that means we just scooped between 0.1 and 0.5
                            \ light years of free fuel
    
     ADC QQ14               \ Set A = A + the current fuel level * 10 (from QQ14)
    
     CMP #70                \ If A > 70 then set A = 70 (as 70 is the maximum fuel
     BCC P%+4               \ level, or 7.0 light years)
     LDA #70
    
     STA QQ14               \ Store the updated fuel level in QQ14
    
     LDA #160               \ Set A to token 160 ("FUEL SCOOPS ON")
    
    .MA34
    
     JSR MESS               \ Print the token in A as an in-flight message
    
    
    
    
           Name: Main flight loop (Part 16 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Process laser pulsing, E.C.M. energy drain, call stardust routine
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_flight_loop_part_16_of_16.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_flight_loop_part_16_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Process laser pulsing
    
       * Process E.C.M. energy drain
    
       * Jump to the stardust routine if we are in a space view
    
       * Return from the main flight loop
    
    
    
    
    .MA23
    
     LDA LAS2               \ If the current view has no laser, jump to MA16 to skip
     BEQ MA16               \ the following
    
     LDA LASCT              \ If LASCT >= 8, jump to MA16 to skip the following, so
     CMP #8                 \ for a pulse laser with a LASCT between 8 and 10, the
     BCS MA16               \ laser stays on, but for a LASCT of 7 or less it gets
                            \ turned off and stays off until LASCT reaches zero and
                            \ the next pulse can start (if the fire button is still
                            \ being pressed)
                            \
                            \ For pulse lasers, LASCT gets set to 10 in ma1 above,
                            \ and it decrements every vertical sync (50 times a
                            \ second), so this means it pulses five times a second,
                            \ with the laser being on for the first 3/10 of each
                            \ pulse and off for the rest of the pulse
                            \
                            \ If this is a beam laser, LASCT is 0 so we always keep
                            \ going here. This means the laser doesn't pulse, but it
                            \ does get drawn and removed every cycle, in a slightly
                            \ different place each time, so the beams still flicker
                            \ around the screen
    
     JSR LASLI2             \ Redraw the existing laser lines, which has the effect
                            \ of removing them from the screen
    
     LDA #0                 \ Set LAS2 to 0 so if this is a pulse laser, it will
     STA LAS2               \ skip over the above until the next pulse (this has no
                            \ effect if this is a beam laser)
    
    .MA16
    
     LDA ECMP               \ If our E.C.M is not on, skip to MA69, otherwise keep
     BEQ MA69               \ going to drain some energy
    
     JSR DENGY              \ Call DENGY to deplete our energy banks by 1
    
     BEQ MA70               \ If we have no energy left, jump to MA70 to turn our
                            \ E.C.M. off
    
    .MA69
    
     LDA ECMA               \ If an E.C.M is going off (ours or an opponent's) then
     BEQ MA66               \ keep going, otherwise skip to MA66
    
     LDY #soecm             \ Call the NOISE routine with Y = 7 to make the sound of
     JSR NOISE              \ the E.C.M.
    
     DEC ECMA               \ Decrement the E.C.M. countdown timer, and if it has
     BNE MA66               \ reached zero, keep going, otherwise skip to MA66
    
    .MA70
    
     JSR ECMOF              \ If we get here then either we have either run out of
                            \ energy, or the E.C.M. timer has run down, so switch
                            \ off the E.C.M.
    
    .MA66
    
     LDA QQ11               \ If this is not a space view (i.e. QQ11 is non-zero)
     BNE oh                 \ then jump to oh to return from the main flight loop
                            \ (as oh is an RTS)
    
     JMP STARS              \ This is a space view, so jump to the STARS routine to
                            \ process the stardust, and return from the main flight
                            \ loop using a tail call
    
    \JMP PBFL               \ This instruction is commented out in the original
                            \ source
    
    
    
    
           Name: SPIN                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Randomly spawn cargo from a destroyed ship
    
    
        Context: See this subroutine [on its own page](../main/subroutine/spin.md)
     Variations: See [code variations](../related/compare/main/subroutine/spin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls SPIN
                 * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md) calls via oh
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls via SPIN2
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The type of cargo to consider spawning (typically #PLT
                            or #OIL)
    
    
    
    * * *
    
    
     Other entry points:
    
       oh                   Contains an RTS
    
       SPIN2                Remove any randomness: spawn cargo of a specific type
                            (given in X), and always spawn the number given in A
    
    
    
    
    .SPIN
    
     JSR DORND              \ Fetch a random number, and jump to oh if it is
     BPL oh                 \ positive (50% chance)
    
     TYA                    \ Copy the cargo type from Y into A and X
     TAX
    
     LDY #0                 \ Fetch the first byte of the hit ship's blueprint,
     AND (XX0),Y            \ which determines the maximum number of bits of
                            \ debris shown when the ship is destroyed, and AND
                            \ with the random number we just fetched
    
     AND #15                \ Reduce the random number in A to the range 0-15
    
    .SPIN2
    
     STA CNT                \ Store the result in CNT, so CNT contains a random
                            \ number between 0 and the maximum number of bits of
                            \ debris that this ship will release when destroyed
                            \ (to a maximum of 15 bits of debris)
    
    .spl
    
     BEQ oh                 \ We're going to go round a loop using CNT as a counter
                            \ so this checks whether the counter is zero and jumps
                            \ to oh when it gets there (which might be straight
                            \ away)
    
     LDA #0                 \ Call SFS1 to spawn the specified cargo from the now
     JSR SFS1               \ deceased parent ship, giving the spawned canister an
                            \ AI flag of 0 (no AI, zero aggression, no E.C.M.)
    
     DEC CNT                \ Decrease the loop counter
    
     BNE spl+2              \ Jump back up to the LDA &0 instruction above (this BPL
                            \ is effectively a JMP as CNT will never be negative)
    
    .oh
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: BOMBOFF                                                 [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw the zig-zag lightning bolt for the energy bomb
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bomboff.md)
     References: This subroutine is called as follows:
                 * [BOMBEFF2](../main/subroutine/bombeff2.md) calls BOMBOFF
                 * [BOMBON](../main/subroutine/bombon.md) calls BOMBOFF
                 * [LOOK1](../main/subroutine/look1.md) calls BOMBOFF
                 * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md) calls BOMBOFF
    
    
    
    
    
    
    .BOMBOFF
    
     LDA #CYAN              \ Change the current colour to cyan
     STA COL
    
     LDA QQ11               \ If the current view is non-zero (i.e. not a space
     BNE BOMBR1             \ view), return from the subroutine (as BOMBR1 contains
                            \ an RTS)
    
     LDY #1                 \ We now want to loop through the 10 (BOMBTBX, BOMBTBY)
                            \ coordinates, drawing a total of 9 lines between them
                            \ to make the lightning effect, so set an index in Y
                            \ to point to the end-point for each line, starting with
                            \ the second coordinate pair
    
     LDA BOMBTBX            \ Store the first coordinate pair from (BOMBTBX,
     STA XX12               \ BOMBTBY) in (XX12, XX12+1)
     LDA BOMBTBY
     STA XX12+1
    
    .BOMBL1
    
    \JSR CLICK              \ This instruction is commented out in the original
                            \ source
    
     LDA XX12               \ Set (X1, Y1) = (XX12, XX12+1)
     STA X1                 \
     LDA XX12+1             \ so the start point for this line
     STA Y1
    
     LDA BOMBTBX,Y          \ Set X2 = Y-th x-coordinate from BOMBTBX
     STA X2
    
     STA XX12               \ Set XX12 = X2
    
     LDA BOMBTBY,Y          \ Set Y2 = Y-th y-coordinate from BOMBTBY, so we now
     STA Y2                 \ have:
                            \
                            \   (X2, Y2) = Y-th coordinate from (BOMBTBX, BOMBTBY)
    
     STA XX12+1             \ Set XX12+1 = Y2, so we now have
                            \
                            \   (XX12, XX12+1) = (X2, Y2)
                            \
                            \ so in the next loop iteration, the start point of the
                            \ line will be the end point of this line, making the
                            \ zig-zag lines all join up like a lightning bolt
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2)
    
     INY                    \ Increment the loop counter
    
     CPY #10                \ If Y < 10, loop back until we have drawn all the lines
     BCC BOMBL1
    
    .BOMBR1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: BOMBEFF2                                                [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Erase the energy bomb zig-zag lightning bolt, make the sound of
                 the energy bomb going off, draw a new bolt and repeat four times
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bombeff2.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md) calls BOMBEFF2
    
    
    
    
    
    
    .BOMBEFF2
    
     JSR P%+3               \ This pair of JSRs runs the following code four times
     JSR BOMBEFF
    
    .BOMBEFF
    
     LDY #sobomb            \ Call the NOISE routine with Y = 6 to make the sound of
     JSR NOISE              \ an energy bomb going off
    
     JSR BOMBOFF            \ Our energy bomb is going off, so call BOMBOFF to draw
                            \ the current zig-zag lightning bolt, which will erase
                            \ it from the screen
    
                            \ Fall through into BOMBON to set up and display a new
                            \ zig-zag lightning bolt
    
    
    
    
           Name: BOMBON                                                  [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Randomise and draw the energy bomb's zig-zag lightning bolt lines
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bombon.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls BOMBON
    
    
    
    
    
    
    .BOMBON
    
     LDY #0                 \ We first need to generate 10 random coordinates for a
                            \ zig-zag lightning bolt, with the x-coordinates in the
                            \ table at BOMBTBX and the y-coordinates in the table at
                            \ BOMBTBY, so set a counter in Y as an index to point at
                            \ each coordinate as we create them
                            \
                            \ Note that we generate the points from right to left,
                            \ so that's high x-coordinate to low x-coordinate
    
    .BOMBL2
    
     JSR DORND              \ Set A and X to random numbers and reduce A to a
     AND #127               \ random number in the range 0-127
    
     ADC #3                 \ Add 3 so A is now in the range 3-130, so the smallest
                            \ possible value gives a y-coordinate just below the top
                            \ border, and the highest possible value gives a
                            \ y-coordinate that's around two-thirds of the way down
                            \ the space view
    
     STA BOMBTBY,Y          \ Store A in the Y-th byte of BOMBTBY, as the
                            \ y-coordinate of the Y-th point in our lightning bolt
    
     TXA                    \ Fetch the random number from X into A and reduce it to
     AND #31                \ the range 0-31
    
     CLC                    \ Add the Y-th value from BOMBPOS table, which contains
     ADC BOMBPOS,Y          \ the smallest possible x-coordinate for the Y-th point
                            \ (so the coordinates in the bolt will step along the
                            \ screen from right to left, but with varying step
                            \ sizes)
    
     STA BOMBTBX,Y          \ Store A in the Y-th byte of BOMBTBX, as the
                            \ x-coordinate of the Y-th point in our lightning bolt
    
     INY                    \ Increment the loop index
    
     CPY #10                \ Loop back to generate the next coordinate until we
     BCC BOMBL2             \ have generated all ten
    
     LDX #0                 \ Set BOMBTBX+9 = 0, so the lightning bolt starts at the
     STX BOMBTBX+9          \ left edge of the screen
    
     DEX                    \ Set BOMBTBX = 255, so the lightning bolt ends at the
     STX BOMBTBX            \ right edge of the screen
    
     BCS BOMBOFF            \ Call BOMBOFF to draw the newly generated zig-zag
                            \ lightning bolt and return from the subroutine using a
                            \ tail call (this BCS is effectively a JMP as we passed
                            \ through the BCC above)
    
    
    
    
           Name: BOMBPOS                                                 [Show more]
           Type: Variable
       Category: Drawing lines
        Summary: A set of x-coordinates that are used as the basis for the energy
                 bomb's zig-zag lightning bolt
    
    
        Context: See this variable [on its own page](../main/variable/bombpos.md)
     References: This variable is used as follows:
                 * [BOMBON](../main/subroutine/bombon.md) uses BOMBPOS
    
    
    
    
    
    
    .BOMBPOS
    
     EQUB 224
     EQUB 224
     EQUB 192
     EQUB 160
     EQUB 128
     EQUB 96
     EQUB 64
     EQUB 32
     EQUB 0
     EQUB 0
    
    
    
    
           Name: BOMBTBX                                                 [Show more]
           Type: Variable
       Category: Drawing lines
        Summary: This is where we store the x-coordinates for the energy bomb's
                 zig-zag lightning bolt
    
    
        Context: See this variable [on its own page](../main/variable/bombtbx.md)
     References: This variable is used as follows:
                 * [BOMBOFF](../main/subroutine/bomboff.md) uses BOMBTBX
                 * [BOMBON](../main/subroutine/bombon.md) uses BOMBTBX
    
    
    
    
    
    
    .BOMBTBX
    
     SKIP 10
    
    
    
    
           Name: BOMBTBY                                                 [Show more]
           Type: Variable
       Category: Drawing lines
        Summary: This is where we store the y-coordinates for the energy bomb's
                 zig-zag lightning bolt
    
    
        Context: See this variable [on its own page](../main/variable/bombtby.md)
     References: This variable is used as follows:
                 * [BOMBOFF](../main/subroutine/bomboff.md) uses BOMBTBY
                 * [BOMBON](../main/subroutine/bombon.md) uses BOMBTBY
    
    
    
    
    
    
    .BOMBTBY
    
     SKIP 10
    
    
    
    
           Name: MT27                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print the captain's name during mission briefings
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
                 [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt27.md)
     Variations: See [code variations](../related/compare/main/subroutine/mt27.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT27
    
    
    
    
    
    * * *
    
    
     This routine prints the following tokens, depending on the galaxy number:
    
       * Token 217 ("CURRUTHERS") in galaxy 0
    
       * Token 218 ("FOSDYKE SMYTHE") in galaxy 1
    
       * Token 219 ("FORTESQUE") in galaxy 2
    
     This is used when printing extended token 213 as part of the mission
     briefings, which looks like this when printed:
    
       Commander {commander name}, I am Captain {mission captain's name} of Her
       Majesty's Space Navy
    
     where {mission captain's name} is replaced by one of the names above.
    
    
    
    
    .MT27
    
     LDA #217               \ Set A = 217, so when we fall through into MT28, the
                            \ 217 gets added to the current galaxy number, so the
                            \ extended token that is printed is 217-219 (as this is
                            \ only called in galaxies 0 through 2)
    
     BNE P%+4               \ Skip the next instruction
    
    
    
    
           Name: MT28                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print the location hint during the mission 1 briefing
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
                 [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt28.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT28
    
    
    
    
    
    * * *
    
    
     This routine prints the following tokens, depending on the galaxy number:
    
       * Token 220 ("WAS LAST SEEN AT {single cap}REESDICE") in galaxy 0
    
       * Token 221 ("IS BELIEVED TO HAVE JUMPED TO THIS GALAXY") in galaxy 1
    
     This is used when printing extended token 10 as part of the mission 1
     briefing, which looks like this when printed:
    
       It went missing from our ship yard on Xeer five months ago and {mission 1
       location hint}
    
     where {mission 1 location hint} is replaced by one of the names above.
    
    
    
    
    .MT28
    
     LDA #220               \ Set A = galaxy number in GCNT + 220, which is in the
     CLC                    \ range 220-221, as this is only called in galaxies 0
     ADC GCNT               \ and 1
    
     BNE DETOK              \ Jump to DETOK to print extended token 220-221,
                            \ returning from the subroutine using a tail call (this
                            \ BNE is effectively a JMP as A is never zero)
    
    
    
    
           Name: DETOK3                                                  [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print an extended recursive token from the RUTOK token table
      Deep dive: [Extended system descriptions](https://elite.bbcelite.com/deep_dives/extended_system_descriptions.html)
                 [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/detok3.md)
     References: This subroutine is called as follows:
                 * [PDESC](../main/subroutine/pdesc.md) calls DETOK3
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The recursive token to be printed, in the range 0-255
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       Y                    Y is preserved
    
       V(1 0)               V(1 0) is preserved
    
    
    
    
    .DETOK3
    
     PHA                    \ Store A on the stack, so we can retrieve it later
    
     TAX                    \ Copy the token number from A into X
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     LDA #LO(RUTOK)         \ Set V to the low byte of RUTOK
     STA V
    
     LDA #HI(RUTOK)         \ Set A to the high byte of RUTOK
    
     BNE DTEN               \ Call DTEN to print token number X from the RUTOK
                            \ table and restore the values of A, Y and V(1 0) from
                            \ the stack, returning from the subroutine using a tail
                            \ call (this BNE is effectively a JMP as A is never
                            \ zero)
    
    
    
    
           Name: DETOK                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print an extended recursive token from the TKN1 token table
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/detok.md)
     References: This subroutine is called as follows:
                 * [BRIS](../main/subroutine/bris.md) calls DETOK
                 * [BRP](../main/subroutine/brp.md) calls DETOK
                 * [CATS](../main/subroutine/cats.md) calls DETOK
                 * [DELT](../main/subroutine/delt.md) calls DETOK
                 * [DETOK2](../main/subroutine/detok2.md) calls DETOK
                 * [dockEd](../main/subroutine/docked.md) calls DETOK
                 * [FILEPR](../main/subroutine/filepr.md) calls DETOK
                 * [GTDIR](../main/subroutine/gtdir.md) calls DETOK
                 * [GTDRV](../main/subroutine/gtdrv.md) calls DETOK
                 * [GTNMEW](../main/subroutine/gtnmew.md) calls DETOK
                 * [HME2](../main/subroutine/hme2.md) calls DETOK
                 * [LOD](../main/subroutine/lod.md) calls DETOK
                 * [MT17](../main/subroutine/mt17.md) calls DETOK
                 * [MT28](../main/subroutine/mt28.md) calls DETOK
                 * [OTHERFILEPR](../main/subroutine/otherfilepr.md) calls DETOK
                 * [PDESC](../main/subroutine/pdesc.md) calls DETOK
                 * [STATUS](../main/subroutine/status.md) calls DETOK
                 * [SVE](../main/subroutine/sve.md) calls DETOK
                 * [TITLE](../main/subroutine/title.md) calls DETOK
                 * [TT210](../main/subroutine/tt210.md) calls DETOK
                 * [TT214](../main/subroutine/tt214.md) calls DETOK
                 * [DETOK3](../main/subroutine/detok3.md) calls via DTEN
    
    
    
    
    
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
    
    
    
    
           Name: DETOK2                                                  [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print an extended text token (1-255)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/detok2.md)
     References: This subroutine is called as follows:
                 * [DETOK](../main/subroutine/detok.md) calls DETOK2
                 * [PDESC](../main/subroutine/pdesc.md) calls DETOK2
                 * [MT18](../main/subroutine/mt18.md) calls via DTS
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The token to be printed (1-255)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       Y                    Y is preserved
    
       V(1 0)               V(1 0) is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       DTS                  Print a single letter in the correct case
    
    
    
    
    .DETOK2
    
     CMP #32                \ If A < 32 then this is a jump token, so skip to DT3 to
     BCC DT3                \ process it
    
     BIT DTW3               \ If bit 7 of DTW3 is clear, then extended tokens are
     BPL DT8                \ enabled, so jump to DT8 to process them
    
                            \ If we get there then this is not a jump token and
                            \ extended tokens are not enabled, so we can call the
                            \ standard text token routine at TT27 to print the token
    
     TAX                    \ Copy the token number from A into X
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     TXA                    \ Copy the token number from X back into A
    
     JSR TT27               \ Call TT27 to print the text token
    
     JMP DT7                \ Jump to DT7 to restore V(1 0) and Y from the stack and
                            \ return from the subroutine
    
    .DT8
    
                            \ If we get here then this is not a jump token and
                            \ extended tokens are enabled
    
     CMP #'['               \ If A < ASCII "[" (i.e. A <= ASCII "Z", or 90) then
     BCC DTS                \ this is a printable ASCII character, so jump down to
                            \ DTS to print it
    
     CMP #129               \ If A < 129, so A is in the range 91-128, jump down to
     BCC DT6                \ DT6 to print a randomised token from the MTIN table
    
     CMP #215               \ If A < 215, so A is in the range 129-214, jump to
     BCC DETOK              \ DETOK as this is a recursive token, returning from the
                            \ subroutine using a tail call
    
                            \ If we get here then A >= 215, so this is a two-letter
                            \ token from the extended TKN2/QQ16 table
    
     SBC #215               \ Subtract 215 to get a token number in the range 0-12
                            \ (the C flag is set as we passed through the BCC above,
                            \ so this subtraction is correct)
    
     ASL A                  \ Set A = A * 2, so it can be used as a pointer into the
                            \ two-letter token tables at TKN2 and QQ16
    
     PHA                    \ Store A on the stack, so we can restore it for the
                            \ second letter below
    
     TAX                    \ Fetch the first letter of the two-letter token from
     LDA TKN2,X             \ TKN2, which is at TKN2 + X
    
     JSR DTS                \ Call DTS to print it
    
     PLA                    \ Restore A from the stack and transfer it into X
     TAX
    
     LDA TKN2+1,X           \ Fetch the second letter of the two-letter token from
                            \ TKN2, which is at TKN2 + X + 1, and fall through into
                            \ DTS to print it
    
    .DTS
    
     CMP #'A'               \ If A < ASCII "A", jump to DT9 to print this as ASCII
     BCC DT9
    
     BIT DTW6               \ If bit 7 of DTW6 is set, then lower case has been
     BMI DT10               \ enabled by jump token 13, {lower case}, so jump to
                            \ DT10 to apply the lower case and single cap masks
    
     BIT DTW2               \ If bit 7 of DTW2 is set, then we are not currently
     BMI DT5                \ printing a word, so jump to DT5 so we skip the setting
                            \ of lower case in Sentence Case (which we only want to
                            \ do when we are already printing a word)
    
    .DT10
    
     ORA DTW1               \ Convert the character to lower case if DTW1 is
                            \ %00100000 (i.e. if we are in {sentence case} mode)
    
    .DT5
    
     AND DTW8               \ Convert the character to upper case if DTW8 is
                            \ %11011111 (i.e. after a {single cap} token)
    
    .DT9
    
     JMP DASC               \ Jump to DASC to print the ASCII character in A,
                            \ returning from the routine using a tail call
    
    .DT3
    
                            \ If we get here then the token number in A is in the
                            \ range 1 to 32, so this is a jump token that should
                            \ call the corresponding address in the jump table at
                            \ JMTB
    
     TAX                    \ Copy the token number from A into X
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     TXA                    \ Copy the token number from X back into A
    
     ASL A                  \ Set A = A * 2, so it can be used as a pointer into the
                            \ jump table at JMTB, though because the original range
                            \ of values is 1-32, so the doubled range is 2-64, we
                            \ need to take the offset into the jump table from
                            \ JMTB-2 rather than JMTB
    
     TAX                    \ Copy the doubled token number from A into X
    
     LDA JMTB-2,X           \ Set DTM(2 1) to the X-th address from the table at
     STA DTM+1              \ JTM-2, which modifies the JSR DASC instruction at
     LDA JMTB-1,X           \ label DTM below so that it calls the subroutine at the
     STA DTM+2              \ relevant address from the JMTB table
    
     TXA                    \ Copy the doubled token number from X back into A
    
     LSR A                  \ Halve A to get the original token number
    
    .DTM
    
     JSR DASC               \ Call the relevant JMTB subroutine, as this instruction
                            \ will have been modified by the above to point to the
                            \ relevant address
    
    .DT7
    
     PLA                    \ Restore V(1 0) from the stack, so it is preserved
     STA V+1                \ through calls to this routine
     PLA
     STA V
    
     PLA                    \ Restore Y from the stack, so it is preserved through
     TAY                    \ calls to this routine
    
     RTS                    \ Return from the subroutine
    
    .DT6
    
                            \ If we get here then the token number in A is in the
                            \ range 91-128, which means we print a randomly picked
                            \ token from the token range given in the corresponding
                            \ entry in the MTIN table
    
     STA SC                 \ Store the token number in SC
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     JSR DORND              \ Set X to a random number
     TAX
    
     LDA #0                 \ Set A to 0, so we can build a random number from 0 to
                            \ 4 in A plus the C flag, with each number being equally
                            \ likely
    
     CPX #51                \ Add 1 to A if X >= 51
     ADC #0
    
     CPX #102               \ Add 1 to A if X >= 102
     ADC #0
    
     CPX #153               \ Add 1 to A if X >= 153
     ADC #0
    
     CPX #204               \ Set the C flag if X >= 204
    
     LDX SC                 \ Fetch the token number from SC into X, so X is now in
                            \ the range 91-128
    
     ADC MTIN-91,X          \ Set A = MTIN-91 + token number (91-128) + random (0-4)
                            \       = MTIN + token number (0-37) + random (0-4)
    
     JSR DETOK              \ Call DETOK to print the extended recursive token in A
    
     JMP DT7                \ Jump to DT7 to restore V(1 0) and Y from the stack and
                            \ return from the subroutine using a tail call
    
    
    
    
           Name: MT1                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to ALL CAPS when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt1.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT1
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW1 = %00000000 (do not change case to lower case)
    
       * DTW6 = %00000000 (lower case is not enabled)
    
    
    
    
    .MT1
    
     LDA #%00000000         \ Set A = %00000000, so when we fall through into MT2,
                            \ both DTW1 and DTW6 get set to %00000000
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &20, or BIT &20A9, which does nothing apart
                            \ from affect the flags
    
    
    
    
           Name: MT2                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to Sentence Case when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt2.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT2
                 * [TTX66K](../main/subroutine/ttx66k.md) calls MT2
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW1 = %00100000 (apply lower case to the second letter of a word onwards)
    
       * DTW6 = %00000000 (lower case is not enabled)
    
    
    
    
    .MT2
    
     LDA #%00100000         \ Set DTW1 = %00100000
     STA DTW1
    
     LDA #00000000          \ Set DTW6 = %00000000
     STA DTW6
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MT8                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Tab to column 6 and start a new word when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt8.md)
     Variations: See [code variations](../related/compare/main/subroutine/mt8.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT8
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * XC = 6 (tab to column 6)
    
       * DTW2 = %11111111 (we are not currently printing a word)
    
    
    
    
    .MT8
    
     LDA #6                 \ Move the text cursor to column 6
     JSR DOXC
    
     LDA #%11111111         \ Set all the bits in DTW2
     STA DTW2
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MT9                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Clear the screen and set the current view type to 1
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt9.md)
     Variations: See [code variations](../related/compare/main/subroutine/mt9.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT9
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * XC = 1 (tab to column 1)
    
     before calling TT66 to clear the screen and set the view type to 1.
    
    
    
    
    .MT9
    
     LDA #1                 \ Move the text cursor to column 1
     STA XC
    
     JMP TT66               \ Jump to TT66 to clear the screen and set the current
                            \ view type to 1, returning from the subroutine using a
                            \ tail call
    
    
    
    
           Name: MT13                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to lower case when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt13.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT13
                 * [MT29](../main/subroutine/mt29.md) calls MT13
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW1 = %00100000 (apply lower case to the second letter of a word onwards)
    
       * DTW6 = %10000000 (lower case is enabled)
    
    
    
    
    .MT13
    
     LDA #%10000000         \ Set DTW6 = %10000000
     STA DTW6
    
     LDA #%00100000         \ Set DTW1 = %00100000
     STA DTW1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MT6                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to standard tokens in Sentence Case
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt6.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT6
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * QQ17 = %10000000 (set Sentence Case for standard tokens)
    
       * DTW3 = %11111111 (print standard tokens)
    
    
    
    
    .MT6
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch standard tokens to
     STA QQ17               \ Sentence Case
    
     LDA #%11111111         \ Set A = %11111111, so when we fall through into MT5,
                            \ DTW3 gets set to %11111111 and calls to DETOK print
                            \ standard tokens
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &00, or BIT &00A9, which does nothing apart
                            \ from affect the flags
    
    
    
    
           Name: MT5                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt5.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT5
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW3 = %00000000 (print extended tokens)
    
    
    
    
    .MT5
    
     LDA #%00000000         \ Set DTW3 = %00000000, so that calls to DETOK print
     STA DTW3               \ extended tokens
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MT14                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to justified text when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt14.md)
     References: This subroutine is called as follows:
                 * [HME2](../main/subroutine/hme2.md) calls MT14
                 * [JMTB](../main/variable/jmtb.md) calls MT14
                 * [PDESC](../main/subroutine/pdesc.md) calls MT14
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW4 = %10000000 (justify text, print buffer on carriage return)
    
       * DTW5 = 0 (reset line buffer size)
    
    
    
    
    .MT14
    
     LDA #%10000000         \ Set A = %10000000, so when we fall through into MT15,
                            \ DTW4 gets set to %10000000
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &00, or BIT &00A9, which does nothing apart
                            \ from affect the flags
    
    
    
    
           Name: MT15                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to left-aligned text when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt15.md)
     References: This subroutine is called as follows:
                 * [HME2](../main/subroutine/hme2.md) calls MT15
                 * [JMTB](../main/variable/jmtb.md) calls MT15
                 * [MESS](../main/subroutine/mess.md) calls MT15
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW4 = %00000000 (do not justify text, print buffer on carriage return)
    
       * DTW5 = 0 (reset line buffer size)
    
    
    
    
    .MT15
    
     LDA #0                 \ Set DTW4 = %00000000
     STA DTW4
    
     ASL A                  \ Set DTW5 = 0 (even when we fall through from MT14 with
     STA DTW5               \ A set to %10000000)
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MT17                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print the selected system's adjective, e.g. Lavian for Lave
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt17.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT17
    
    
    
    
    
    * * *
    
    
     The adjective for the current system is generated by taking the system name,
     removing the last character if it is a vowel, and adding "-ian" to the end,
     so:
    
       * Lave gives Lavian (as in "Lavian tree grub")
    
       * Leesti gives Leestian (as in "Leestian Evil Juice")
    
     This routine is called by jump token 17, {system name adjective}, and it can
     only be used when justified text is being printed - i.e. following jump token
     14, {justify} - because the routine needs to use the line buffer to work.
    
    
    
    
    .MT17
    
     LDA QQ17               \ Set QQ17 = %10111111 to switch to Sentence Case
     AND #%10111111
     STA QQ17
    
     LDA #3                 \ Print control code 3 (selected system name) into the
     JSR TT27               \ line buffer
    
     LDX DTW5               \ Load the last character of the line buffer BUF into A
     LDA BUF-1,X            \ (as DTW5 contains the buffer size, so character DTW5-1
                            \ is the last character in the buffer BUF)
    
     JSR VOWEL              \ Test whether the character is a vowel, in which case
                            \ this will set the C flag
    
     BCC MT171              \ If the character is not a vowel, skip the following
                            \ instruction
    
     DEC DTW5               \ The character is a vowel, so decrement DTW5, which
                            \ removes the last character from the line buffer (i.e.
                            \ it removes the trailing vowel from the system name)
    
    .MT171
    
     LDA #153               \ Print extended token 153 ("IAN"), returning from the
     JMP DETOK              \ subroutine using a tail call
    
    
    
    
           Name: MT18                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a random 1-8 letter word in Sentence Case
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt18.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT18
    
    
    
    
    
    
    .MT18
    
     JSR MT19               \ Call MT19 to capitalise the next letter (i.e. set
                            \ Sentence Case for this word only)
    
     JSR DORND              \ Set A and X to random numbers and reduce A to a
     AND #3                 \ random number in the range 0-3
    
     TAY                    \ Copy the random number into Y, so we can use Y as a
                            \ loop counter to print 1-4 words (i.e. Y+1 words)
    
    .MT18L
    
     JSR DORND              \ Set A and X to random numbers and reduce A to an even
     AND #62                \ random number in the range 0-62 (as bit 0 of 62 is 0)
    
     TAX                    \ Copy the random number into X, so X contains the table
                            \ offset of a random extended two-letter token from 0-31
                            \ which we can now use to pick a token from the combined
                            \ tables at TKN2+2 and QQ16 (we intentionally exclude
                            \ the first token in TKN2, which contains a newline)
    
     LDA TKN2+2,X           \ Print the first letter of the token at TKN2+2 + X
     JSR DTS
    
     LDA TKN2+3,X           \ Print the second letter of the token at TKN2+2 + X
     JSR DTS
    
     DEY                    \ Decrement the loop counter
    
     BPL MT18L              \ Loop back to MT18L to print another two-letter token
                            \ until we have printed Y+1 of them
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MT19                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Capitalise the next letter
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt19.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT19
                 * [MT18](../main/subroutine/mt18.md) calls MT19
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW8 = %11011111 (capitalise the next letter)
    
    
    
    
    .MT19
    
     LDA #%11011111         \ Set DTW8 = %11011111
     STA DTW8
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: VOWEL                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Test whether a character is a vowel
    
    
        Context: See this subroutine [on its own page](../main/subroutine/vowel.md)
     References: This subroutine is called as follows:
                 * [MT17](../main/subroutine/mt17.md) calls VOWEL
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be tested
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is set if the character is a vowel, otherwise
                            it is clear
    
    
    
    
    .VOWEL
    
     ORA #%00100000         \ Set bit 5 of the character to make it lower case
    
     CMP #'a'               \ If the letter is a vowel, jump to VRTS to return from
     BEQ VRTS               \ the subroutine with the C flag set (as the CMP will
     CMP #'e'               \ set the C flag if the comparison is equal)
     BEQ VRTS
     CMP #'i'
     BEQ VRTS
     CMP #'o'
     BEQ VRTS
     CMP #'u'
     BEQ VRTS
    
     CLC                    \ The character is not a vowel, so clear the C flag
    
    .VRTS
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: JMTB                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: The extended token table for jump tokens 1-32 (DETOK)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/jmtb.md)
     Variations: See [code variations](../related/compare/main/variable/jmtb.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DETOK2](../main/subroutine/detok2.md) uses JMTB
    
    
    
    
    
    
    .JMTB
    
     EQUW MT1               \ Token  1: Switch to ALL CAPS
     EQUW MT2               \ Token  2: Switch to Sentence Case
     EQUW TT27              \ Token  3: Print the selected system name
     EQUW TT27              \ Token  4: Print the commander's name
     EQUW MT5               \ Token  5: Switch to extended tokens
     EQUW MT6               \ Token  6: Switch to standard tokens, in Sentence Case
     EQUW DASC              \ Token  7: Beep
     EQUW MT8               \ Token  8: Tab to column 6
     EQUW MT9               \ Token  9: Clear screen, tab to column 1, view type = 1
     EQUW DASC              \ Token 10: Line feed
     EQUW NLIN4             \ Token 11: Draw box around title (line at pixel row 19)
     EQUW DASC              \ Token 12: Carriage return
     EQUW MT13              \ Token 13: Switch to lower case
     EQUW MT14              \ Token 14: Switch to justified text
     EQUW MT15              \ Token 15: Switch to left-aligned text
     EQUW MT16              \ Token 16: Print the character in DTW7 (drive number)
     EQUW MT17              \ Token 17: Print system name adjective in Sentence Case
     EQUW MT18              \ Token 18: Randomly print 1 to 4 two-letter tokens
     EQUW MT19              \ Token 19: Capitalise first letter of next word only
     EQUW DASC              \ Token 20: Unused
     EQUW CLYNS             \ Token 21: Clear the bottom few lines of the space view
     EQUW PAUSE             \ Token 22: Display ship and wait for key press
     EQUW MT23              \ Token 23: Move to row 10, white text, set lower case
     EQUW PAUSE2            \ Token 24: Wait for a key press
     EQUW BRIS              \ Token 25: Show incoming message screen, wait 2 seconds
     EQUW MT26              \ Token 26: Fetch line input from keyboard (filename)
     EQUW MT27              \ Token 27: Print mission captain's name (217-219)
     EQUW MT28              \ Token 28: Print mission 1 location hint (220-221)
     EQUW MT29              \ Token 29: Column 6, white text, lower case in words
     EQUW FILEPR            \ Token 30: Display currently selected media (disk/tape)
     EQUW OTHERFILEPR       \ Token 31: Display the non-selected media (disk/tape)
     EQUW DASC              \ Token 32: Unused
    
    
    
    
           Name: TKN2                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: The extended two-letter token lookup table
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/tkn2.md)
     References: This variable is used as follows:
                 * [DETOK2](../main/subroutine/detok2.md) uses TKN2
                 * [MT18](../main/subroutine/mt18.md) uses TKN2
    
    
    
    
    
    * * *
    
    
     Two-letter token lookup table for extended tokens 215-227.
    
    
    
    
    .TKN2
    
     EQUB 12, 10            \ Token 215 = {crlf}
     EQUS "AB"              \ Token 216
     EQUS "OU"              \ Token 217
     EQUS "SE"              \ Token 218
     EQUS "IT"              \ Token 219
     EQUS "IL"              \ Token 220
     EQUS "ET"              \ Token 221
     EQUS "ST"              \ Token 222
     EQUS "ON"              \ Token 223
     EQUS "LO"              \ Token 224
     EQUS "NU"              \ Token 225
     EQUS "TH"              \ Token 226
     EQUS "NO"              \ Token 227
    
    
    
    
           Name: QQ16                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: The two-letter token lookup table
      Deep dive: [Printing text tokens](https://elite.bbcelite.com/deep_dives/printing_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/qq16.md)
     Variations: See [code variations](../related/compare/main/variable/qq16.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [TT43](../main/subroutine/tt43.md) uses QQ16
    
    
    
    
    
    * * *
    
    
     Two-letter token lookup table for tokens 128-159.
    
     These two-letter tokens can also be used in the extended text token system, by
     adding 100 to the token number. So the extended two-letter token 228 is "AL",
     the same as the standard two-letter token 128. In this system, the last four
     tokens are not available, as they would have numbers greater than 255.
    
    
    
    
    .QQ16
    
     EQUS "AL"              \ Token 128
     EQUS "LE"              \ Token 129
     EQUS "XE"              \ Token 130
     EQUS "GE"              \ Token 131
     EQUS "ZA"              \ Token 132
     EQUS "CE"              \ Token 133
     EQUS "BI"              \ Token 134
     EQUS "SO"              \ Token 135
     EQUS "US"              \ Token 136
     EQUS "ES"              \ Token 137
     EQUS "AR"              \ Token 138
     EQUS "MA"              \ Token 139
     EQUS "IN"              \ Token 140
     EQUS "DI"              \ Token 141
     EQUS "RE"              \ Token 142
     EQUS "A?"              \ Token 143
     EQUS "ER"              \ Token 144
     EQUS "AT"              \ Token 145
     EQUS "EN"              \ Token 146
     EQUS "BE"              \ Token 147
     EQUS "RA"              \ Token 148
     EQUS "LA"              \ Token 149
     EQUS "VE"              \ Token 150
     EQUS "TI"              \ Token 151
     EQUS "ED"              \ Token 152
     EQUS "OR"              \ Token 153
     EQUS "QU"              \ Token 154
     EQUS "AN"              \ Token 155
     EQUS "TE"              \ Token 156
     EQUS "IS"              \ Token 157
     EQUS "RI"              \ Token 158
     EQUS "ON"              \ Token 159
    
    
    
    
           Name: NA%                                                     [Show more]
           Type: Variable
       Category: Save and load
        Summary: The data block for the last saved commander
    
    
        Context: See this variable [on its own page](../main/variable/na_per_cent.md)
     Variations: See [code variations](../related/compare/main/variable/na_per_cent-na2_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [CHECK](../main/subroutine/check.md) uses NA%
                 * [DFAULT](../main/subroutine/dfault.md) uses NA%
                 * [GTNMEW](../main/subroutine/gtnmew.md) uses NA%
                 * [JAMESON](../main/subroutine/jameson.md) uses NA%
                 * [LOD](../main/subroutine/lod.md) uses NA%
                 * [SVE](../main/subroutine/sve.md) uses NA%
                 * [TR1](../main/subroutine/tr1.md) uses NA%
                 * [TRNME](../main/subroutine/trnme.md) uses NA%
                 * [wfile](../main/subroutine/wfile.md) uses NA%
    
    
    
    
    
    
     EQUS ":0.E."           \ The drive part of this string (the "0") is updated
                            \ with the chosen drive in the GTNMEW routine, but the
                            \ directory part (the "E") is fixed. The variable is
                            \ followed directly by the commander file at NA%, which
                            \ starts with the commander name, so the full string at
                            \ NA%-5 is in the format ":0.E.jameson", which gives the
                            \ full filename of the commander file
    
    .NA%
    
     EQUS "jameson"         \ The current commander name
     EQUB 13
    
     SKIP 53                \ Placeholders for bytes #0 to #52
    
     EQUB 16                \ AVL+0  = Market availability of food, #53
     EQUB 15                \ AVL+1  = Market availability of textiles, #54
     EQUB 17                \ AVL+2  = Market availability of radioactives, #55
     EQUB 0                 \ AVL+3  = Market availability of slaves, #56
     EQUB 3                 \ AVL+4  = Market availability of liquor/Wines, #57
     EQUB 28                \ AVL+5  = Market availability of luxuries, #58
     EQUB 14                \ AVL+6  = Market availability of narcotics, #59
     EQUB 0                 \ AVL+7  = Market availability of computers, #60
     EQUB 0                 \ AVL+8  = Market availability of machinery, #61
     EQUB 10                \ AVL+9  = Market availability of alloys, #62
     EQUB 0                 \ AVL+10 = Market availability of firearms, #63
     EQUB 17                \ AVL+11 = Market availability of furs, #64
     EQUB 58                \ AVL+12 = Market availability of minerals, #65
     EQUB 7                 \ AVL+13 = Market availability of gold, #66
     EQUB 9                 \ AVL+14 = Market availability of platinum, #67
     EQUB 8                 \ AVL+15 = Market availability of gem-stones, #68
     EQUB 0                 \ AVL+16 = Market availability of alien items, #69
    
     SKIP 3                 \ Placeholders for bytes #70 to #72
    
     EQUB 128               \ SVC = Save count, #73
    
    
    
    
           Name: CHK2                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: Second checksum byte for the saved commander data file
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
                 [The competition code](https://elite.bbcelite.com/deep_dives/the_competition_code.html)
    
    
        Context: See this variable [on its own page](../main/variable/chk2.md)
     Variations: See [code variations](../related/compare/main/variable/chk2.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DFAULT](../main/subroutine/dfault.md) uses CHK2
                 * [SVE](../main/subroutine/sve.md) uses CHK2
    
    
    
    
    
    * * *
    
    
     Second commander checksum byte. If the default commander is changed, a new
     checksum will be calculated and inserted by the elite-checksum.py script.
    
     The offset of this byte within a saved commander file is also shown (it's at
     byte #74).
    
    
    
    
    .CHK2
    
     EQUB 0                 \ Placeholder for the checksum in byte #74
    
    
    
    
           Name: CHK                                                     [Show more]
           Type: Variable
       Category: Save and load
        Summary: First checksum byte for the saved commander data file
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
                 [The competition code](https://elite.bbcelite.com/deep_dives/the_competition_code.html)
    
    
        Context: See this variable [on its own page](../main/variable/chk.md)
     Variations: See [code variations](../related/compare/main/variable/chk.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DFAULT](../main/subroutine/dfault.md) uses CHK
                 * [SVE](../main/subroutine/sve.md) uses CHK
    
    
    
    
    
    * * *
    
    
     Commander checksum byte. If the default commander is changed, a new checksum
     will be calculated and inserted by the elite-checksum.py script.
    
     The offset of this byte within a saved commander file is also shown (it's at
     byte #75).
    
    
    
    
    .CHK
    
     EQUB 0                 \ Placeholder for the checksum in byte #75
    
    \.CHK3                  \ These instructions are commented out in the original
    \                       \ source
    \EQUB 0
    
     SKIP 12                \ These bytes appear to be unused, though the first byte
                            \ in this block is included in the commander file (it
                            \ has no effect, as it's the third checksum byte from
                            \ the Commodore 64 version, which isn't used in the
                            \ Master version)
    
    
    
    
           Name: S1%                                                     [Show more]
           Type: Variable
       Category: Save and load
        Summary: The drive and directory number used when saving or loading a
                 commander file
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
    
    
        Context: See this variable [on its own page](../main/variable/s1_per_cent.md)
     Variations: See [code variations](../related/compare/main/variable/s1_per_cent.md) for this variable in the different versions
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .S1%
    
     EQUS ":0.E."
    
    
    
    
           Name: NA2%                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: The data block for the default commander
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
                 [The competition code](https://elite.bbcelite.com/deep_dives/the_competition_code.html)
    
    
        Context: See this variable [on its own page](../main/variable/na2_per_cent.md)
     Variations: See [code variations](../related/compare/main/variable/na_per_cent-na2_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [JAMESON](../main/subroutine/jameson.md) uses NA2%
    
    
    
    
    
    * * *
    
    
     Contains the default commander data, with the name at NA% and the data at
     NA%+8 onwards. The size of the data block is given in NT% (which also includes
     the two checksum bytes that follow this block). This block is initially set up
     with the default commander, which can be maxed out for testing purposes by
     setting Q% to TRUE.
    
     The commander's name is stored at NA%, and can be up to 7 characters long
     (the DFS filename limit). It is terminated with a carriage return character,
     ASCII 13.
    
     The offset of each byte within a saved commander file is also shown as #0, #1
     and so on, so the kill tally, for example, is in bytes #71 and #72 of the
     saved file. The related variable name from the current commander block is
     also shown.
    
    
    
    
    .NA2%
    
     EQUS "JAMESON"         \ The current commander name, which defaults to JAMESON
     EQUB 13                \
                            \ The commander name can be up to seven characters (the
                            \ DFS limit for filenames), and is terminated by a
                            \ carriage return
    
                            \ NA%+8 is the start of the commander data block
                            \
                            \ This block contains the last saved commander data
                            \ block. As the game is played it uses an identical
                            \ block at location TP to store the current commander
                            \ state, and that block is copied here when the game is
                            \ saved. Conversely, when the game starts up, the block
                            \ here is copied to TP, which restores the last saved
                            \ commander when we die
                            \
                            \ The initial state of this block defines the default
                            \ commander. Q% can be set to TRUE to give the default
                            \ commander lots of credits and equipment
    
     EQUB 0                 \ TP = Mission status, #0
    
     EQUB 20                \ QQ0 = Current system X-coordinate (Lave), #1
     EQUB 173               \ QQ1 = Current system Y-coordinate (Lave), #2
    
     EQUW &5A4A             \ QQ21 = Seed s0 for system 0, galaxy 0 (Tibedied), #3-4
     EQUW &0248             \ QQ21 = Seed s1 for system 0, galaxy 0 (Tibedied), #5-6
     EQUW &B753             \ QQ21 = Seed s2 for system 0, galaxy 0 (Tibedied), #7-8
    
    IF Q%
     EQUD &00CA9A3B         \ CASH = Amount of cash (100,000,000 Cr), #9-12
    ELSE
     EQUD &E8030000         \ CASH = Amount of cash (100 Cr), #9-12
    ENDIF
    
     EQUB 70                \ QQ14 = Fuel level, #13
    
     EQUB %10000000 AND Q%  \ COK = Competition flags, #14
    
     EQUB 0                 \ GCNT = Galaxy number, 0-7, #15
    
    IF Q%
     EQUB Armlas            \ LASER = Front laser, #16
    ELSE
     EQUB POW               \ LASER = Front laser, #16
    ENDIF
    
     EQUB POW AND Q%        \ LASER = Rear laser, #16
    
     EQUB (POW+128) AND Q%  \ LASER+2 = Left laser, #18
    
     EQUB Mlas AND Q%       \ LASER+3 = Right laser, #19
    
     EQUW 0                 \ These bytes appear to be unused (they were originally
                            \ used for up/down lasers, but they were dropped),
                            \ #20-21
    
     EQUB 22 + (15 AND Q%)  \ CRGO = Cargo capacity, #22
    
     EQUB 0                 \ QQ20+0  = Amount of food in cargo hold, #23
     EQUB 0                 \ QQ20+1  = Amount of textiles in cargo hold, #24
     EQUB 0                 \ QQ20+2  = Amount of radioactives in cargo hold, #25
     EQUB 0                 \ QQ20+3  = Amount of slaves in cargo hold, #26
     EQUB 0                 \ QQ20+4  = Amount of liquor/Wines in cargo hold, #27
     EQUB 0                 \ QQ20+5  = Amount of luxuries in cargo hold, #28
     EQUB 0                 \ QQ20+6  = Amount of narcotics in cargo hold, #29
     EQUB 0                 \ QQ20+7  = Amount of computers in cargo hold, #30
     EQUB 0                 \ QQ20+8  = Amount of machinery in cargo hold, #31
     EQUB 0                 \ QQ20+9  = Amount of alloys in cargo hold, #32
     EQUB 0                 \ QQ20+10 = Amount of firearms in cargo hold, #33
     EQUB 0                 \ QQ20+11 = Amount of furs in cargo hold, #34
     EQUB 0                 \ QQ20+12 = Amount of minerals in cargo hold, #35
     EQUB 0                 \ QQ20+13 = Amount of gold in cargo hold, #36
     EQUB 0                 \ QQ20+14 = Amount of platinum in cargo hold, #37
     EQUB 0                 \ QQ20+15 = Amount of gem-stones in cargo hold, #38
     EQUB 0                 \ QQ20+16 = Amount of alien items in cargo hold, #39
    
     EQUB Q%                \ ECM = E.C.M. system, #40
    
     EQUB Q%                \ BST = Fuel scoops ("barrel status"), #41
    
     EQUB Q% AND 127        \ BOMB = Energy bomb, #42
    
     EQUB Q% AND 1          \ ENGY = Energy/shield level, #43
    
     EQUB Q%                \ DKCMP = Docking computer, #44
    
     EQUB Q%                \ GHYP = Galactic hyperdrive, #45
    
     EQUB Q%                \ ESCP = Escape pod, #46
    
     EQUD 0                 \ These four bytes appear to be unused, #47-50
    
     EQUB 3 + (Q% AND 1)    \ NOMSL = Number of missiles, #51
    
     EQUB 0                 \ FIST = Legal status ("fugitive/innocent status"), #52
    
     EQUB 16                \ AVL+0  = Market availability of food, #53
     EQUB 15                \ AVL+1  = Market availability of textiles, #54
     EQUB 17                \ AVL+2  = Market availability of radioactives, #55
     EQUB 0                 \ AVL+3  = Market availability of slaves, #56
     EQUB 3                 \ AVL+4  = Market availability of liquor/Wines, #57
     EQUB 28                \ AVL+5  = Market availability of luxuries, #58
     EQUB 14                \ AVL+6  = Market availability of narcotics, #59
     EQUB 0                 \ AVL+7  = Market availability of computers, #60
     EQUB 0                 \ AVL+8  = Market availability of machinery, #61
     EQUB 10                \ AVL+9  = Market availability of alloys, #62
     EQUB 0                 \ AVL+10 = Market availability of firearms, #63
     EQUB 17                \ AVL+11 = Market availability of furs, #64
     EQUB 58                \ AVL+12 = Market availability of minerals, #65
     EQUB 7                 \ AVL+13 = Market availability of gold, #66
     EQUB 9                 \ AVL+14 = Market availability of platinum, #67
     EQUB 8                 \ AVL+15 = Market availability of gem-stones, #68
     EQUB 0                 \ AVL+16 = Market availability of alien items, #69
    
     EQUB 0                 \ QQ26 = Random byte that changes for each visit to a
                            \ system, for randomising market prices, #70
    
     EQUW 20000 AND Q%      \ TALLY = Number of kills, #71-72
    
     EQUB 128               \ SVC = Save count, #73
    
    \.CHK2                  \ This label is commented out in the original source
    
     EQUB &AA               \ The CHK2 checksum value for the default commander
    
    \.CHK3                  \ These instructions are commented out in the original
    \                       \ source
    \EQUB CH3X%
    
    \.CHK                   \ This label is commented out in the original source
    
     EQUB &03               \ The CHK checksum value for the default commander
    
     SKIP 12                \ These bytes appear to be unused, though the first byte
                            \ in this block is included in the commander file (it
                            \ has no effect, as it's the third checksum byte from
                            \ the Commodore 64 version, which isn't used in the
                            \ Master version)
    
    .NAEND%
    
     SKIP 4                 \ These bytes appear to be unused
    
    
    
    
           Name: shpcol                                                  [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship colours
    
    
        Context: See this variable [on its own page](../main/variable/shpcol.md)
     Variations: See [code variations](../related/compare/main/variable/shpcol.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md) uses shpcol
    
    
    
    
    
    
    .shpcol
    
     EQUB 0
    
     EQUB YELLOW            \ Missile
     EQUB CYAN              \ Coriolis space station
     EQUB CYAN              \ Escape pod
     EQUB CYAN              \ Alloy plate
     EQUB CYAN              \ Cargo canister
     EQUB RED               \ Boulder
     EQUB RED               \ Asteroid
     EQUB RED               \ Splinter
     EQUB CYAN              \ Shuttle
     EQUB CYAN              \ Transporter
     EQUB CYAN              \ Cobra Mk III
     EQUB CYAN              \ Python
     EQUB CYAN              \ Boa
     EQUB CYAN              \ Anaconda
     EQUB RED               \ Rock hermit (asteroid)
     EQUB CYAN              \ Viper
     EQUB CYAN              \ Sidewinder
     EQUB CYAN              \ Mamba
     EQUB CYAN              \ Krait
     EQUB CYAN              \ Adder
     EQUB CYAN              \ Gecko
     EQUB CYAN              \ Cobra Mk I
     EQUB CYAN              \ Worm
     EQUB CYAN              \ Cobra Mk III (pirate)
     EQUB CYAN              \ Asp Mk II
     EQUB CYAN              \ Python (pirate)
     EQUB CYAN              \ Fer-de-lance
     EQUB %11001001         \ Moray (colour 3, 2, 0, 1 = cyan/red/black/yellow)
     EQUB WHITE             \ Thargoid
     EQUB WHITE             \ Thargon
     EQUB CYAN              \ Constrictor
     EQUB CYAN              \ Cougar
    
     EQUB CYAN              \ This byte appears to be unused
    
    
    
    
           Name: scacol                                                  [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship colours on the scanner
      Deep dive: [The elusive Cougar](https://elite.bbcelite.com/deep_dives/the_elusive_cougar.html)
    
    
        Context: See this variable [on its own page](../main/variable/scacol.md)
     Variations: See [code variations](../related/compare/main/variable/scacol.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [SCAN](../main/subroutine/scan.md) uses scacol
    
    
    
    
    
    
    .scacol
    
     EQUB 0                 \ This byte appears to be unused
    
     EQUB YELLOW2           \ Missile
     EQUB GREEN2            \ Coriolis space station
     EQUB BLUE2             \ Escape pod
     EQUB BLUE2             \ Alloy plate
     EQUB BLUE2             \ Cargo canister
     EQUB RED2              \ Boulder
     EQUB RED2              \ Asteroid
     EQUB RED2              \ Splinter
     EQUB CYAN2             \ Shuttle
     EQUB CYAN2             \ Transporter
     EQUB CYAN2             \ Cobra Mk III
     EQUB MAG2              \ Python
     EQUB MAG2              \ Boa
     EQUB MAG2              \ Anaconda
     EQUB RED2              \ Rock hermit (asteroid)
     EQUB CYAN2             \ Viper
     EQUB CYAN2             \ Sidewinder
     EQUB CYAN2             \ Mamba
     EQUB CYAN2             \ Krait
     EQUB CYAN2             \ Adder
     EQUB CYAN2             \ Gecko
     EQUB CYAN2             \ Cobra Mk I
     EQUB BLUE2             \ Worm
     EQUB CYAN2             \ Cobra Mk III (pirate)
     EQUB CYAN2             \ Asp Mk II
     EQUB MAG2              \ Python (pirate)
     EQUB CYAN2             \ Fer-de-lance
     EQUB CYAN2             \ Moray
     EQUB WHITE2            \ Thargoid
     EQUB CYAN2             \ Thargon
     EQUB CYAN2             \ Constrictor
     EQUB 0                 \ Cougar
    
     EQUB CYAN2             \ These bytes appear to be unused
     EQUD 0
    
    
    
     Save ELTA.bin
    
    
    
    
     PRINT "ELITE A"
     PRINT "Assembled at ", ~(NA%-5)
     PRINT "Ends at ", ~P%
     PRINT "Code size is ", ~(P% - (NA%-5))
     PRINT "Execute at ", ~LOAD%
     PRINT "Reload at ", ~LOAD_A%
    
     PRINT "S.ELTA ", ~(NA%-5), " ", ~P%, " ", ~LOAD%, " ", ~LOAD_A%
    \SAVE "3-assembled-output/ELTA.bin", (NA%-5), P%, LOAD%
    
    
    

[X]

Subroutine [ABORT](elite_e.md#header-abort) (category: Dashboard)

Disarm missiles and update the dashboard indicators

[X]

Subroutine [ABORT2](elite_e.md#header-abort2) (category: Dashboard)

Set/unset the lock target for a missile and update the dashboard

[X]

Subroutine [ADDK](elite_a.md#header-addk) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Variable [ALP1](workspaces.md#alp1) in workspace [ZP](workspaces.md#header-zp)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Variable [ALP2](workspaces.md#alp2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Variable [ALPHA](workspaces.md#alpha) in workspace [ZP](workspaces.md#header-zp)

The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31

[X]

Variable [ALTIT](workspaces.md#altit) in workspace [WP](workspaces.md#header-wp)

Our altitude above the surface of the planet or sun

[X]

Subroutine [ANGRY](elite_c.md#header-angry) (category: Tactics)

Make a ship or station hostile, and if this is a ship then enable the ship's AI and give it a kick of speed

[X]

Variable [ASH](workspaces.md#ash) in workspace [ZP](workspaces.md#header-zp)

Aft shield status

[X]

Configuration variable [AST](workspaces.md#ast)

Ship type for an asteroid

[X]

Configuration variable [Armlas](workspaces.md#armlas)

Military laser power

[X]

Subroutine [BAY](elite_f.md#header-bay) (category: Status)

Go to the docking bay (i.e. show the Status Mode screen)

[X]

Subroutine [BEEP](elite_a.md#header-beep) (category: Sound)

Make a short, high beep

[X]

Subroutine [BEGIN](elite_f.md#header-begin) (category: Loader)

Initialise the configuration variables and start the game

[X]

Variable [BET1](workspaces.md#bet1) in workspace [ZP](workspaces.md#header-zp)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BET2](workspaces.md#bet2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of BET2 = sign of the pitch angle in BETA

[X]

Variable [BETA](workspaces.md#beta) in workspace [ZP](workspaces.md#header-zp)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Configuration variable [BLUE2](workspaces.md#blue2)

Two mode 2 pixels of colour 4 (blue)

[X]

Label [BOL1](elite_a.md#bol1) in subroutine [TTX66](elite_a.md#header-ttx66)

[X]

Variable [BOMB](workspaces.md#bomb) in workspace [WP](workspaces.md#header-wp)

Energy bomb

[X]

Label [BOMBEFF](elite_a.md#bombeff) in subroutine [BOMBEFF2](elite_a.md#header-bombeff2)

[X]

Subroutine [BOMBEFF2](elite_a.md#header-bombeff2) (category: Drawing lines)

Erase the energy bomb zig-zag lightning bolt, make the sound of the energy bomb going off, draw a new bolt and repeat four times

[X]

Label [BOMBL1](elite_a.md#bombl1) in subroutine [BOMBOFF](elite_a.md#header-bomboff)

[X]

Label [BOMBL2](elite_a.md#bombl2) in subroutine [BOMBON](elite_a.md#header-bombon)

[X]

Subroutine [BOMBOFF](elite_a.md#header-bomboff) (category: Drawing lines)

Draw the zig-zag lightning bolt for the energy bomb

[X]

Subroutine [BOMBON](elite_a.md#header-bombon) (category: Drawing lines)

Randomise and draw the energy bomb's zig-zag lightning bolt lines

[X]

Variable [BOMBPOS](elite_a.md#header-bombpos) (category: Drawing lines)

A set of x-coordinates that are used as the basis for the energy bomb's zig-zag lightning bolt

[X]

Label [BOMBR1](elite_a.md#bombr1) in subroutine [BOMBOFF](elite_a.md#header-bomboff)

[X]

Variable [BOMBTBX](elite_a.md#header-bombtbx) (category: Drawing lines)

This is where we store the x-coordinates for the energy bomb's zig-zag lightning bolt

[X]

Variable [BOMBTBY](elite_a.md#header-bombtby) (category: Drawing lines)

This is where we store the y-coordinates for the energy bomb's zig-zag lightning bolt

[X]

Subroutine [BOOP](elite_a.md#header-boop) (category: Sound)

Make a long, low beep

[X]

Label [BOS1](elite_a.md#bos1) in subroutine [TTX66](elite_a.md#header-ttx66)

[X]

Label [BOS2](elite_a.md#bos2) in subroutine [TTX66](elite_a.md#header-ttx66)

[X]

Subroutine [BRIEF](elite_c.md#header-brief) (category: Missions)

Start mission 1 and show the mission briefing

[X]

Subroutine [BRIEF2](elite_c.md#header-brief2) (category: Missions)

Start mission 2

[X]

Subroutine [BRIEF3](elite_c.md#header-brief3) (category: Missions)

Receive the briefing and plans for mission 2

[X]

Subroutine [BRIS](elite_c.md#header-bris) (category: Missions)

Clear the screen, display "INCOMING MESSAGE" and wait for 2 seconds

[X]

Label [BS2](elite_a.md#bs2) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Variable [BST](workspaces.md#bst) in workspace [WP](workspaces.md#header-wp)

Fuel scoops (BST stands for "barrel status")

[X]

Variable [BSTK](elite_a.md#bstk) in workspace [UP](elite_a.md#header-up)

Bitstik configuration setting

[X]

Variable [BUF](workspaces.md#buf) in workspace [WP](workspaces.md#header-wp)

The line buffer used by DASC to print justified text

[X]

Label [BULL1](elite_a.md#bull1) in subroutine [ECBLB](elite_a.md#header-ecblb)

[X]

Label [BULL2](elite_a.md#bull2) in subroutine [SPBLB](elite_a.md#header-spblb)

[X]

Label [BURN](elite_a.md#burn) in subroutine [Main flight loop (Part 11 of 16)](elite_a.md#header-main-flight-loop-part-11-of-16)

[X]

Variable [CABTMP](workspaces.md#cabtmp) in workspace [WP](workspaces.md#header-wp)

Cabin temperature

[X]

Variable [CATF](elite_a.md#catf) in workspace [UP](elite_a.md#header-up)

The disc catalogue flag

[X]

Label [CLYL](elite_a.md#clyl) in subroutine [CLYNS](elite_a.md#header-clyns)

[X]

Subroutine [CLYNS](elite_a.md#header-clyns) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Variable [CNT](workspaces.md#cnt) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [COL](workspaces.md#col) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [COLD](elite_h.md#header-cold) (category: Save and load)

Set the standard BRKV handler for the game

[X]

Variable [COMC](elite_a.md#comc) in workspace [UP](elite_a.md#header-up)

The colour of the dot on the compass

[X]

Subroutine [COMPAS](elite_e.md#header-compas) (category: Dashboard)

Update the compass

[X]

Variable [COMX](workspaces.md#comx) in workspace [WP](workspaces.md#header-wp)

The x-coordinate of the compass dot

[X]

Variable [COMY](workspaces.md#comy) in workspace [WP](workspaces.md#header-wp)

The y-coordinate of the compass dot

[X]

Configuration variable [CON](workspaces.md#con)

Ship type for a Constrictor

[X]

Label [CP1](elite_a.md#cp1) in subroutine [CPIXK](elite_a.md#header-cpixk)

[X]

Subroutine [CPIXK](elite_a.md#header-cpixk) (category: Drawing pixels)

Draw a single-height dash on the dashboard

[X]

Variable [CTWOS](elite_a.md#header-ctwos) (category: Drawing pixels)

Ready-made single-pixel character row bytes for mode 2

[X]

Configuration variable [CYAN](workspaces.md#cyan)

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Configuration variable [CYAN2](workspaces.md#cyan2)

Two mode 2 pixels of colour 6 (cyan)

[X]

Entry point [DASC](elite_b.md#dasc) in subroutine [TT26](elite_b.md#header-tt26) (category: Text)

DASC does exactly the same as TT26 and prints a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [DEATH](elite_f.md#header-death) (category: Start and end)

Display the death screen

[X]

Subroutine [DEBRIEF](elite_c.md#header-debrief) (category: Missions)

Finish mission 1

[X]

Subroutine [DEBRIEF2](elite_c.md#header-debrief2) (category: Missions)

Finish mission 2

[X]

Subroutine [DEEOR](elite_a.md#header-deeor) (category: Utility routines)

Unscramble the main code

[X]

Label [DEEORL](elite_a.md#deeorl) in subroutine [DEEORS](elite_a.md#header-deeors)

[X]

Subroutine [DEEORS](elite_a.md#header-deeors) (category: Utility routines)

Decrypt a multi-page block of memory

[X]

Subroutine [DELAY](elite_a.md#header-delay) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Label [DELL1K](elite_a.md#dell1k) in subroutine [WSCAN](elite_a.md#header-wscan)

[X]

Variable [DELT4](workspaces.md#delt4) in workspace [ZP](workspaces.md#header-zp)

Our current speed * 64 as a 16-bit value

[X]

Variable [DELTA](workspaces.md#delta) in workspace [ZP](workspaces.md#header-zp)

Our current speed, in the range 1-40

[X]

Subroutine [DENGY](elite_e.md#header-dengy) (category: Flight)

Drain some energy from the energy banks

[X]

Subroutine [DETOK](elite_a.md#header-detok) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [DETOK2](elite_a.md#header-detok2) (category: Text)

Print an extended text token (1-255)

[X]

Entry point [DIL](elite_a.md#dil) in subroutine [DILX](elite_a.md#header-dilx) (category: Dashboard)

The range of the indicator is 0-16 (for the energy banks)

[X]

Entry point [DIL-1](elite_a.md#dil) in subroutine [DILX](elite_a.md#header-dilx) (category: Dashboard)

The range of the indicator is 0-32 (for the speed indicator)

[X]

Subroutine [DIL2](elite_a.md#header-dil2) (category: Dashboard)

Update the roll or pitch indicator on the dashboard

[X]

Subroutine [DILX](elite_a.md#header-dilx) (category: Dashboard)

Update a bar-based indicator on the dashboard

[X]

Entry point [DILX+2](elite_a.md#dilx) in subroutine [DILX](elite_a.md#header-dilx) (category: Dashboard)

The range of the indicator is 0-64 (for the fuel indicator)

[X]

Variable [DKCMP](workspaces.md#dkcmp) in workspace [WP](workspaces.md#header-wp)

Docking computer

[X]

Variable [DL](workspaces.md#dl) in workspace [ZP](workspaces.md#header-zp)

Vertical sync flag

[X]

Label [DL1](elite_a.md#dl1) in subroutine [DILX](elite_a.md#header-dilx)

[X]

Label [DL2](elite_a.md#dl2) in subroutine [DILX](elite_a.md#header-dilx)

[X]

Label [DL3](elite_a.md#dl3) in subroutine [DILX](elite_a.md#header-dilx)

[X]

Label [DL30](elite_a.md#dl30) in subroutine [DILX](elite_a.md#header-dilx)

[X]

Label [DL31](elite_a.md#dl31) in subroutine [DILX](elite_a.md#header-dilx)

[X]

Label [DL5](elite_a.md#dl5) in subroutine [DILX](elite_a.md#header-dilx)

[X]

Label [DL6](elite_a.md#dl6) in subroutine [DILX](elite_a.md#header-dilx)

[X]

Label [DLL10](elite_a.md#dll10) in subroutine [DIL2](elite_a.md#header-dil2)

[X]

Label [DLL11](elite_a.md#dll11) in subroutine [DIL2](elite_a.md#header-dil2)

[X]

Label [DLL12](elite_a.md#dll12) in subroutine [DIL2](elite_a.md#header-dil2)

[X]

Label [DLL23](elite_a.md#dll23) in subroutine [DIALS (Part 3 of 4)](elite_a.md#header-dials-part-3-of-4)

[X]

Label [DLL24](elite_a.md#dll24) in subroutine [DIALS (Part 3 of 4)](elite_a.md#header-dials-part-3-of-4)

[X]

Label [DLL26](elite_a.md#dll26) in subroutine [DIALS (Part 3 of 4)](elite_a.md#header-dials-part-3-of-4)

[X]

Label [DLL9](elite_a.md#dll9) in subroutine [DIALS (Part 3 of 4)](elite_a.md#header-dials-part-3-of-4)

[X]

Variable [DLY](workspaces.md#dly) in workspace [WP](workspaces.md#header-wp)

In-flight message delay

[X]

Variable [DNOIZ](elite_a.md#dnoiz) in workspace [UP](elite_a.md#header-up)

Sound on/off configuration setting

[X]

Subroutine [DOENTRY](elite_a.md#header-doentry) (category: Flight)

Dock at the space station, show the ship hangar and work out any mission progression

[X]

Subroutine [DORND](elite_f.md#header-dornd) (category: Maths (Arithmetic))

Generate random numbers

[X]

Label [DOWN](elite_a.md#down) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

Subroutine [DOXC](elite_d.md#header-doxc) (category: Text)

Move the text cursor to a specific column

[X]

Label [DT1](elite_a.md#dt1) in subroutine [DETOK](elite_a.md#header-detok)

[X]

Label [DT10](elite_a.md#dt10) in subroutine [DETOK2](elite_a.md#header-detok2)

[X]

Label [DT3](elite_a.md#dt3) in subroutine [DETOK2](elite_a.md#header-detok2)

[X]

Label [DT5](elite_a.md#dt5) in subroutine [DETOK2](elite_a.md#header-detok2)

[X]

Label [DT6](elite_a.md#dt6) in subroutine [DETOK2](elite_a.md#header-detok2)

[X]

Label [DT7](elite_a.md#dt7) in subroutine [DETOK2](elite_a.md#header-detok2)

[X]

Label [DT8](elite_a.md#dt8) in subroutine [DETOK2](elite_a.md#header-detok2)

[X]

Label [DT9](elite_a.md#dt9) in subroutine [DETOK2](elite_a.md#header-detok2)

[X]

Entry point [DTEN](elite_a.md#dten) in subroutine [DETOK](elite_a.md#header-detok) (category: Text)

Print recursive token number X from the token table pointed to by (A V), used to print tokens from the RUTOK table via calls to DETOK3

[X]

Label [DTEX](elite_a.md#dtex) in subroutine [DETOK](elite_a.md#header-detok)

[X]

Label [DTL1](elite_a.md#dtl1) in subroutine [DETOK](elite_a.md#header-detok)

[X]

Label [DTL2](elite_a.md#dtl2) in subroutine [DETOK](elite_a.md#header-detok)

[X]

Label [DTM](elite_a.md#dtm) in subroutine [DETOK2](elite_a.md#header-detok2)

[X]

Entry point [DTS](elite_a.md#dts) in subroutine [DETOK2](elite_a.md#header-detok2) (category: Text)

Print a single letter in the correct case

[X]

Variable [DTW1](elite_b.md#header-dtw1) (category: Text)

A mask for applying the lower case part of Sentence Case to extended text tokens

[X]

Variable [DTW2](elite_b.md#header-dtw2) (category: Text)

A flag that indicates whether we are currently printing a word

[X]

Variable [DTW3](elite_b.md#header-dtw3) (category: Text)

A flag for switching between standard and extended text tokens

[X]

Variable [DTW4](elite_b.md#header-dtw4) (category: Text)

Flags that govern how justified extended text tokens are printed

[X]

Variable [DTW5](elite_b.md#header-dtw5) (category: Text)

The size of the justified text buffer at BUF

[X]

Variable [DTW6](elite_b.md#header-dtw6) (category: Text)

A flag to denote whether printing in lower case is enabled for extended text tokens

[X]

Variable [DTW8](elite_b.md#header-dtw8) (category: Text)

A mask for capitalising the next letter in an extended text token

[X]

Label [DV5K](elite_a.md#dv5k) in subroutine [DVID4K](elite_a.md#header-dvid4k)

[X]

Label [DV8K](elite_a.md#dv8k) in subroutine [DVID4K](elite_a.md#header-dvid4k)

[X]

Subroutine [DVID4K](elite_a.md#header-dvid4k) (category: Maths (Arithmetic))

Calculate (P R) = 256 * A / Q

[X]

Label [DVL4K](elite_a.md#dvl4k) in subroutine [DVID4K](elite_a.md#header-dvid4k)

[X]

Subroutine [ECBLB2](elite_a.md#header-ecblb2) (category: Dashboard)

Start up the E.C.M. (light up the indicator, start the countdown and make the E.C.M. sound)

[X]

Variable [ECBT](elite_a.md#header-ecbt) (category: Dashboard)

The character bitmap for the E.C.M. indicator bulb

[X]

Variable [ECM](workspaces.md#ecm) in workspace [WP](workspaces.md#header-wp)

E.C.M. system

[X]

Variable [ECMA](workspaces.md#ecma) in workspace [ZP](workspaces.md#header-zp)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Subroutine [ECMOF](elite_h.md#header-ecmof) (category: Dashboard)

Switch off the E.C.M. and turn off the dashboard bulb

[X]

Variable [ECMP](workspaces.md#ecmp) in workspace [WP](workspaces.md#header-wp)

Our E.C.M. status

[X]

Label [EE2](elite_a.md#ee2) in subroutine [CLYNS](elite_a.md#header-clyns)

[X]

Label [EE3K](elite_a.md#ee3k) in subroutine [CLYNS](elite_a.md#header-clyns)

[X]

Label [EN1](elite_a.md#en1) in subroutine [DOENTRY](elite_a.md#header-doentry)

[X]

Label [EN2](elite_a.md#en2) in subroutine [DOENTRY](elite_a.md#header-doentry)

[X]

Label [EN3](elite_a.md#en3) in subroutine [DOENTRY](elite_a.md#header-doentry)

[X]

Label [EN4](elite_a.md#en4) in subroutine [DOENTRY](elite_a.md#header-doentry)

[X]

Label [EN5](elite_a.md#en5) in subroutine [DOENTRY](elite_a.md#header-doentry)

[X]

Variable [ENERGY](workspaces.md#energy) in workspace [ZP](workspaces.md#header-zp)

Energy bank status

[X]

Variable [ENGY](workspaces.md#engy) in workspace [WP](workspaces.md#header-wp)

Energy unit

[X]

Subroutine [ESCAPE](elite_b.md#header-escape) (category: Flight)

Launch our escape pod

[X]

Variable [ESCP](workspaces.md#escp) in workspace [WP](workspaces.md#header-wp)

Escape pod

[X]

Subroutine [EXNO](elite_h.md#header-exno) (category: Sound)

Make the sound of a laser strike on another ship or a ship explosion

[X]

Subroutine [EXNO2](elite_h.md#header-exno2) (category: Status)

Process us making a kill

[X]

Subroutine [EXNO3](elite_h.md#header-exno3) (category: Sound)

Make an explosion sound

[X]

Variable [F%](elite_h.md#header-f-per-cent) (category: Utility routines)

Denotes the end of the main game code, from ELITE A to ELITE H

[X]

Subroutine [FAROF](elite_f.md#header-farof) (category: Maths (Geometry))

Compare x_hi, y_hi and z_hi with 224

[X]

Subroutine [FAROF2](elite_f.md#header-farof2) (category: Maths (Geometry))

Compare x_hi, y_hi and z_hi with A

[X]

Subroutine [FILEPR](elite_f.md#header-filepr) (category: Save and load)

Display the currently selected media (disk or tape)

[X]

Variable [FIST](workspaces.md#fist) in workspace [WP](workspaces.md#header-wp)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Variable [FLH](elite_a.md#flh) in workspace [UP](elite_a.md#header-up)

Flashing console bars configuration setting

[X]

Variable [FONT%](elite_a.md#header-font-per-cent) (category: Text)

A copy of the character definition bitmap table from the MOS ROM

[X]

Variable [FRIN](workspaces.md#frin) in workspace [WP](workspaces.md#header-wp)

Slots for the ships in the local bubble of universe

[X]

Subroutine [FRMIS](elite_c.md#header-frmis) (category: Tactics)

Fire a missile from our ship

[X]

Variable [FSH](workspaces.md#fsh) in workspace [ZP](workspaces.md#header-zp)

Forward shield status

[X]

Variable [G%](elite_a.md#header-g-per-cent) (category: Utility routines)

Denotes the start of the main game code, from ELITE A to ELITE H

[X]

Variable [GCNT](workspaces.md#gcnt) in workspace [WP](workspaces.md#header-wp)

The number of the current galaxy (0-7)

[X]

Subroutine [GINF](elite_c.md#header-ginf) (category: Universe)

Fetch the address of a ship's data block into INF

[X]

Variable [GNTMP](workspaces.md#gntmp) in workspace [WP](workspaces.md#header-wp)

Laser temperature (or "gun temperature")

[X]

Configuration variable [GREEN2](workspaces.md#green2)

Two mode 2 pixels of colour 2 (green)

[X]

Label [HA2](elite_a.md#ha2) in subroutine [HANGER](elite_a.md#header-hanger)

[X]

Entry point [HA3](elite_a.md#ha3) in subroutine [HANGER](elite_a.md#header-hanger) (category: Ship hangar)

Contains an RTS

[X]

Label [HA6](elite_a.md#ha6) in subroutine [HANGER](elite_a.md#header-hanger)

[X]

Label [HAL1](elite_a.md#hal1) in subroutine [HANGER](elite_a.md#header-hanger)

[X]

Label [HAL2](elite_a.md#hal2) in subroutine [HAS2](elite_a.md#header-has2)

[X]

Subroutine [HAL3](elite_a.md#header-hal3) (category: Ship hangar)

Draw a hangar background line from left to right, stopping when it bumps into existing on-screen content

[X]

Label [HAL6](elite_a.md#hal6) in subroutine [HANGER](elite_a.md#header-hanger)

[X]

Label [HAL7](elite_a.md#hal7) in subroutine [HANGER](elite_a.md#header-hanger)

[X]

Subroutine [HALL](elite_c.md#header-hall) (category: Ship hangar)

Draw the ships in the ship hangar, then draw the hangar

[X]

Variable [HANGFLAG](elite_a.md#header-hangflag) (category: Ship hangar)

The number of ships being displayed in the ship hangar

[X]

Subroutine [HAS2](elite_a.md#header-has2) (category: Ship hangar)

Draw a hangar background line from left to right

[X]

Subroutine [HAS3](elite_a.md#header-has3) (category: Ship hangar)

Draw a hangar background line from right to left

[X]

Variable [HFX](workspaces.md#hfx) in workspace [WP](workspaces.md#header-wp)

A flag that toggles the hyperspace colour effect

[X]

Subroutine [HITCH](elite_c.md#header-hitch) (category: Tactics)

Work out if the ship in INWK is in our crosshairs

[X]

Label [HL10](elite_a.md#hl10) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Label [HL2](elite_a.md#hl2) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Label [HL3](elite_a.md#hl3) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Label [HL5](elite_a.md#hl5) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Label [HL6](elite_a.md#hl6) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Label [HL7](elite_a.md#hl7) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Label [HL8](elite_a.md#hl8) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Label [HL9](elite_a.md#hl9) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Label [HLL1](elite_a.md#hll1) in subroutine [HLOIN](elite_a.md#header-hloin)

[X]

Entry point [HLOIN3](elite_a.md#hloin3) in subroutine [HLOIN](elite_a.md#header-hloin) (category: Drawing lines)

Draw a line from (X, Y1) to (X2, Y1) in the colour given in A

[X]

Variable [INF](workspaces.md#inf) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](workspaces.md#inwk) in workspace [ZP](workspaces.md#header-zp)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [IRQ1](elite_a.md#header-irq1) (category: Drawing the screen)

The main screen-mode interrupt handler (IRQ1V points here)

[X]

Configuration variable [IRQ1V](workspaces.md#irq1v)

The IRQ1V vector that we intercept to implement the split-screen mode

[X]

Label [ISDK](elite_a.md#isdk) in subroutine [Main flight loop (Part 9 of 16)](elite_a.md#header-main-flight-loop-part-9-of-16)

[X]

Variable [JMTB](elite_a.md#header-jmtb) (category: Text)

The extended token table for jump tokens 1-32 (DETOK)

[X]

Variable [JOPOS](workspaces.md#jopos) in workspace [WP](workspaces.md#header-wp)

Contains the high byte of the latest value from ADC channel 1 (the joystick X value), which gets updated regularly by the IRQ1 interrupt handler

[X]

Variable [JSTX](workspaces.md#jstx) in workspace [ZP](workspaces.md#header-zp)

Our current roll rate

[X]

Variable [JSTY](workspaces.md#jsty) in workspace [ZP](workspaces.md#header-zp)

Our current pitch rate

[X]

Variable [K](workspaces.md#k) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Workspace [K%](workspaces.md#header-k-per-cent) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Variable [K3](workspaces.md#k3) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [KEY1](workspaces.md#key1)

The seed for encrypting the game code from DOENTRY to F%

[X]

Configuration variable [KEY2](workspaces.md#key2)

The seed for encrypting the game data from XX21 to &BBFF

[X]

Subroutine [KS1](elite_f.md#header-ks1) (category: Universe)

Remove the current ship from our local bubble of universe

[X]

Label [KS1S](elite_a.md#ks1s) in subroutine [Main flight loop (Part 12 of 16)](elite_a.md#header-main-flight-loop-part-12-of-16)

[X]

Variable [KY1](workspaces.md#ky1) in workspace [ZP](workspaces.md#header-zp)

"?" is being pressed (slow down)

[X]

Variable [KY12](workspaces.md#ky12) in workspace [ZP](workspaces.md#header-zp)

TAB is being pressed (energy bomb)

[X]

Variable [KY13](workspaces.md#ky13) in workspace [ZP](workspaces.md#header-zp)

ESCAPE is being pressed (launch escape pod)

[X]

Variable [KY14](workspaces.md#ky14) in workspace [ZP](workspaces.md#header-zp)

"T" is being pressed (target missile)

[X]

Variable [KY15](workspaces.md#ky15) in workspace [ZP](workspaces.md#header-zp)

"U" is being pressed (unarm missile)

[X]

Variable [KY16](workspaces.md#ky16) in workspace [ZP](workspaces.md#header-zp)

"M" is being pressed (fire missile)

[X]

Variable [KY17](workspaces.md#ky17) in workspace [ZP](workspaces.md#header-zp)

"E" is being pressed (activate E.C.M.)

[X]

Variable [KY18](workspaces.md#ky18) in workspace [ZP](workspaces.md#header-zp)

"J" is being pressed (in-system jump)

[X]

Variable [KY19](workspaces.md#ky19) in workspace [ZP](workspaces.md#header-zp)

"C" is being pressed (activate docking computer)

[X]

Variable [KY2](workspaces.md#ky2) in workspace [ZP](workspaces.md#header-zp)

Space is being pressed (speed up)

[X]

Variable [KY20](workspaces.md#ky20) in workspace [ZP](workspaces.md#header-zp)

"P" is being pressed (deactivate docking computer)

[X]

Variable [KY7](workspaces.md#ky7) in workspace [ZP](workspaces.md#header-zp)

"A" is being pressed (fire lasers)

[X]

Variable [LAS](workspaces.md#las) in workspace [ZP](workspaces.md#header-zp)

Contains the laser power of the laser fitted to the current space view (or 0 if there is no laser fitted to the current view)

[X]

Variable [LAS2](workspaces.md#las2) in workspace [WP](workspaces.md#header-wp)

Laser power for the current laser

[X]

Variable [LASCT](workspaces.md#lasct) in workspace [WP](workspaces.md#header-wp)

The laser pulse count for the current laser

[X]

Variable [LASER](workspaces.md#laser) in workspace [WP](workspaces.md#header-wp)

The specifications of the lasers fitted to each of the four space views

[X]

Subroutine [LASLI](elite_c.md#header-lasli) (category: Drawing lines)

Draw the laser lines for when we fire our lasers

[X]

Entry point [LASLI2](elite_c.md#lasli2) in subroutine [LASLI](elite_c.md#header-lasli) (category: Drawing lines)

Just draw the current laser lines without moving the centre point, draining energy or heating up. This has the effect of removing the lines from the screen

[X]

Subroutine [LASNO](elite_a.md#header-lasno) (category: Sound)

Make the sound of our laser firing

[X]

Subroutine [LAUN](elite_c.md#header-laun) (category: Drawing circles)

Make the launch sound and draw the launch tunnel

[X]

Label [LFT](elite_a.md#lft) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI1](elite_a.md#li1) in subroutine [LOINQ (Part 1 of 7)](elite_a.md#header-loinq-part-1-of-7)

[X]

Label [LI100](elite_a.md#li100) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

[X]

Label [LI101](elite_a.md#li101) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

Label [LI110](elite_a.md#li110) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

[X]

Label [LI111](elite_a.md#li111) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

Label [LI120](elite_a.md#li120) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

[X]

Label [LI121](elite_a.md#li121) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

Label [LI130](elite_a.md#li130) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

[X]

Label [LI131](elite_a.md#li131) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

Label [LI140](elite_a.md#li140) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

Label [LI15](elite_a.md#li15) in subroutine [LOINQ (Part 5 of 7)](elite_a.md#header-loinq-part-5-of-7)

[X]

Label [LI190](elite_a.md#li190) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

Label [LI191](elite_a.md#li191) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

Label [LI2](elite_a.md#li2) in subroutine [LOINQ (Part 1 of 7)](elite_a.md#header-loinq-part-1-of-7)

[X]

Label [LI200](elite_a.md#li200) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

[X]

Label [LI201](elite_a.md#li201) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

Label [LI210](elite_a.md#li210) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

[X]

Label [LI211](elite_a.md#li211) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

Label [LI220](elite_a.md#li220) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

[X]

Label [LI221](elite_a.md#li221) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

Label [LI230](elite_a.md#li230) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

[X]

Label [LI231](elite_a.md#li231) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

Label [LI240](elite_a.md#li240) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

Label [LI290](elite_a.md#li290) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI291](elite_a.md#li291) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI3](elite_a.md#li3) in subroutine [LOINQ (Part 2 of 7)](elite_a.md#header-loinq-part-2-of-7)

[X]

Label [LI300](elite_a.md#li300) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

[X]

Label [LI301](elite_a.md#li301) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

[X]

Label [LI301S](elite_a.md#li301s) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI302](elite_a.md#li302) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

[X]

Label [LI302S](elite_a.md#li302s) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI303](elite_a.md#li303) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

[X]

Label [LI303S](elite_a.md#li303s) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI304](elite_a.md#li304) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

[X]

Label [LI304S](elite_a.md#li304s) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI305](elite_a.md#li305) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

[X]

Label [LI306](elite_a.md#li306) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

[X]

Label [LI307](elite_a.md#li307) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

[X]

Label [LI310](elite_a.md#li310) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI311](elite_a.md#li311) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI312](elite_a.md#li312) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI313](elite_a.md#li313) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI314](elite_a.md#li314) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI315](elite_a.md#li315) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI316](elite_a.md#li316) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LI400](elite_a.md#li400) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

[X]

Label [LI401](elite_a.md#li401) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

[X]

Label [LI401S](elite_a.md#li401s) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI402](elite_a.md#li402) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

[X]

Label [LI402S](elite_a.md#li402s) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI403](elite_a.md#li403) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

[X]

Label [LI403S](elite_a.md#li403s) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI404](elite_a.md#li404) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

[X]

Label [LI404S](elite_a.md#li404s) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI405](elite_a.md#li405) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

[X]

Label [LI406](elite_a.md#li406) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

[X]

Label [LI407](elite_a.md#li407) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

[X]

Label [LI410](elite_a.md#li410) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI411](elite_a.md#li411) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI412](elite_a.md#li412) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI413](elite_a.md#li413) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI414](elite_a.md#li414) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI415](elite_a.md#li415) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LI416](elite_a.md#li416) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LIEX](elite_a.md#liex) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

Label [LIEX2](elite_a.md#liex2) in subroutine [LOINQ (Part 4 of 7)](elite_a.md#header-loinq-part-4-of-7)

[X]

Label [LIEX3](elite_a.md#liex3) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LIEX4](elite_a.md#liex4) in subroutine [LOINQ (Part 6 of 7)](elite_a.md#header-loinq-part-6-of-7)

[X]

Label [LIEX5](elite_a.md#liex5) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LIEX6](elite_a.md#liex6) in subroutine [LOINQ (Part 7 of 7)](elite_a.md#header-loinq-part-7-of-7)

[X]

Label [LIEX7](elite_a.md#liex7) in subroutine [LOINQ (Part 5 of 7)](elite_a.md#header-loinq-part-5-of-7)

[X]

Label [LIEXS](elite_a.md#liexs) in subroutine [LOINQ (Part 3 of 7)](elite_a.md#header-loinq-part-3-of-7)

[X]

Label [LINSCN](elite_a.md#linscn) in subroutine [IRQ1](elite_a.md#header-irq1)

[X]

Label [LIfudge](elite_a.md#lifudge) in subroutine [LOINQ (Part 5 of 7)](elite_a.md#header-loinq-part-5-of-7)

[X]

Label [LIlog2](elite_a.md#lilog2) in subroutine [LOINQ (Part 5 of 7)](elite_a.md#header-loinq-part-5-of-7)

[X]

Label [LIlog3](elite_a.md#lilog3) in subroutine [LOINQ (Part 5 of 7)](elite_a.md#header-loinq-part-5-of-7)

[X]

Label [LIlog5](elite_a.md#lilog5) in subroutine [LOINQ (Part 2 of 7)](elite_a.md#header-loinq-part-2-of-7)

[X]

Label [LIlog6](elite_a.md#lilog6) in subroutine [LOINQ (Part 2 of 7)](elite_a.md#header-loinq-part-2-of-7)

[X]

Label [LIlog7](elite_a.md#lilog7) in subroutine [LOINQ (Part 2 of 7)](elite_a.md#header-loinq-part-2-of-7)

[X]

Subroutine [LL5](elite_g.md#header-ll5) (category: Maths (Arithmetic))

Calculate Q = SQRT(R Q)

[X]

Subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Subroutine [LOIN](elite_a.md#header-loin) (category: Drawing lines)

Draw a one-segment line

[X]

Entry point [LOINQ](elite_a.md#loinq) in subroutine [LOINQ (Part 1 of 7)](elite_a.md#header-loinq-part-1-of-7) (category: Drawing lines)

Draw a one-segment line from (X1, Y1) to (X2, Y2)

[X]

Label [MA14](elite_a.md#ma14) in subroutine [Main flight loop (Part 11 of 16)](elite_a.md#header-main-flight-loop-part-11-of-16)

[X]

[X]

Label [MA15](elite_a.md#ma15) in subroutine [Main flight loop (Part 12 of 16)](elite_a.md#header-main-flight-loop-part-12-of-16)

[X]

Label [MA16](elite_a.md#ma16) in subroutine [Main flight loop (Part 16 of 16)](elite_a.md#header-main-flight-loop-part-16-of-16)

[X]

Label [MA17](elite_a.md#ma17) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA18](elite_a.md#ma18) in subroutine [Main flight loop (Part 13 of 16)](elite_a.md#header-main-flight-loop-part-13-of-16)

[X]

Label [MA20](elite_a.md#ma20) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA21](elite_a.md#ma21) in subroutine [Main flight loop (Part 6 of 16)](elite_a.md#header-main-flight-loop-part-6-of-16)

[X]

Label [MA22](elite_a.md#ma22) in subroutine [Main flight loop (Part 15 of 16)](elite_a.md#header-main-flight-loop-part-15-of-16)

[X]

Label [MA23](elite_a.md#ma23) in subroutine [Main flight loop (Part 16 of 16)](elite_a.md#header-main-flight-loop-part-16-of-16)

[X]

Label [MA23S](elite_a.md#ma23s) in subroutine [Main flight loop (Part 14 of 16)](elite_a.md#header-main-flight-loop-part-14-of-16)

[X]

Label [MA24](elite_a.md#ma24) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA25](elite_a.md#ma25) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA26](elite_a.md#ma26) in subroutine [Main flight loop (Part 11 of 16)](elite_a.md#header-main-flight-loop-part-11-of-16)

[X]

Label [MA27](elite_a.md#ma27) in subroutine [Main flight loop (Part 12 of 16)](elite_a.md#header-main-flight-loop-part-12-of-16)

[X]

Label [MA28](elite_a.md#ma28) in subroutine [Main flight loop (Part 15 of 16)](elite_a.md#header-main-flight-loop-part-15-of-16)

[X]

Label [MA29](elite_a.md#ma29) in subroutine [Main flight loop (Part 15 of 16)](elite_a.md#header-main-flight-loop-part-15-of-16)

[X]

Label [MA3](elite_a.md#ma3) in subroutine [Main flight loop (Part 4 of 16)](elite_a.md#header-main-flight-loop-part-4-of-16)

[X]

Label [MA33](elite_a.md#ma33) in subroutine [Main flight loop (Part 15 of 16)](elite_a.md#header-main-flight-loop-part-15-of-16)

[X]

Label [MA34](elite_a.md#ma34) in subroutine [Main flight loop (Part 15 of 16)](elite_a.md#header-main-flight-loop-part-15-of-16)

[X]

Label [MA4](elite_a.md#ma4) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA47](elite_a.md#ma47) in subroutine [Main flight loop (Part 11 of 16)](elite_a.md#header-main-flight-loop-part-11-of-16)

[X]

Label [MA58](elite_a.md#ma58) in subroutine [Main flight loop (Part 10 of 16)](elite_a.md#header-main-flight-loop-part-10-of-16)

[X]

Label [MA59](elite_a.md#ma59) in subroutine [Main flight loop (Part 10 of 16)](elite_a.md#header-main-flight-loop-part-10-of-16)

[X]

Label [MA62](elite_a.md#ma62) in subroutine [Main flight loop (Part 9 of 16)](elite_a.md#header-main-flight-loop-part-9-of-16)

[X]

Label [MA63](elite_a.md#ma63) in subroutine [Main flight loop (Part 10 of 16)](elite_a.md#header-main-flight-loop-part-10-of-16)

[X]

Label [MA64](elite_a.md#ma64) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA65](elite_a.md#ma65) in subroutine [Main flight loop (Part 8 of 16)](elite_a.md#header-main-flight-loop-part-8-of-16)

[X]

Label [MA66](elite_a.md#ma66) in subroutine [Main flight loop (Part 16 of 16)](elite_a.md#header-main-flight-loop-part-16-of-16)

[X]

Label [MA67](elite_a.md#ma67) in subroutine [Main flight loop (Part 10 of 16)](elite_a.md#header-main-flight-loop-part-10-of-16)

[X]

Label [MA68](elite_a.md#ma68) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA69](elite_a.md#ma69) in subroutine [Main flight loop (Part 16 of 16)](elite_a.md#header-main-flight-loop-part-16-of-16)

[X]

Label [MA70](elite_a.md#ma70) in subroutine [Main flight loop (Part 16 of 16)](elite_a.md#header-main-flight-loop-part-16-of-16)

[X]

Label [MA76](elite_a.md#ma76) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA77](elite_a.md#ma77) in subroutine [Main flight loop (Part 13 of 16)](elite_a.md#header-main-flight-loop-part-13-of-16)

[X]

Label [MA78](elite_a.md#ma78) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [MA8](elite_a.md#ma8) in subroutine [Main flight loop (Part 12 of 16)](elite_a.md#header-main-flight-loop-part-12-of-16)

[X]

Label [MA93](elite_a.md#ma93) in subroutine [Main flight loop (Part 15 of 16)](elite_a.md#header-main-flight-loop-part-15-of-16)

[X]

Label [MAC1](elite_a.md#mac1) in subroutine [Main flight loop (Part 12 of 16)](elite_a.md#header-main-flight-loop-part-12-of-16)

[X]

Configuration variable [MAG2](workspaces.md#mag2)

Two mode 2 pixels of colour 5 (magenta)

[X]

Entry point [MAL1](elite_a.md#mal1) in subroutine [Main flight loop (Part 4 of 16)](elite_a.md#header-main-flight-loop-part-4-of-16) (category: Main loop)

Marks the beginning of the ship analysis loop, so we can jump back here from part 12 of the main flight loop to work our way through each ship in the local bubble. We also jump back here when a ship is removed from the bubble, so we can continue processing from the next ship

[X]

Label [MAL2](elite_a.md#mal2) in subroutine [Main flight loop (Part 4 of 16)](elite_a.md#header-main-flight-loop-part-4-of-16)

[X]

Label [MAL3](elite_a.md#mal3) in subroutine [Main flight loop (Part 6 of 16)](elite_a.md#header-main-flight-loop-part-6-of-16)

[X]

Label [MAL4](elite_a.md#mal4) in subroutine [Main flight loop (Part 14 of 16)](elite_a.md#header-main-flight-loop-part-14-of-16)

[X]

Subroutine [MAS1](elite_b.md#header-mas1) (category: Maths (Geometry))

Add an orientation vector coordinate to an INWK coordinate

[X]

Subroutine [MAS2](elite_b.md#header-mas2) (category: Maths (Geometry))

Calculate a cap on the maximum distance to the planet or sun

[X]

Subroutine [MAS3](elite_b.md#header-mas3) (category: Maths (Arithmetic))

Calculate A = x_hi^2 + y_hi^2 + z_hi^2 in the K% block

[X]

Subroutine [MAS4](elite_f.md#header-mas4) (category: Maths (Geometry))

Calculate a cap on the maximum distance to a ship

[X]

Label [MBL1](elite_a.md#mbl1) in subroutine [MSBAR](elite_a.md#header-msbar)

[X]

Label [MBL2](elite_a.md#mbl2) in subroutine [MSBAR](elite_a.md#header-msbar)

[X]

Subroutine [MCASH](elite_d.md#header-mcash) (category: Maths (Arithmetic))

Add an amount of cash to the cash pot

[X]

Variable [MCNT](workspaces.md#mcnt) in workspace [ZP](workspaces.md#header-zp)

The main loop counter

[X]

Subroutine [MESS](elite_f.md#header-mess) (category: Flight)

Display an in-flight message

[X]

Variable [MJ](workspaces.md#mj) in workspace [WP](workspaces.md#header-wp)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Variable [MOS](workspaces.md#mos) in workspace [ZP](workspaces.md#header-zp)

Determines whether we are running on a Master Compact

[X]

Variable [MSAR](workspaces.md#msar) in workspace [WP](workspaces.md#header-wp)

The targeting state of our leftmost missile

[X]

Subroutine [MSBAR](elite_a.md#header-msbar) (category: Dashboard)

Draw a specific indicator in the dashboard's missile bar

[X]

Configuration variable [MSL](workspaces.md#msl)

Ship type for a missile

[X]

Variable [MSTG](workspaces.md#mstg) in workspace [ZP](workspaces.md#header-zp)

The current missile lock target

[X]

Subroutine [MT1](elite_a.md#header-mt1) (category: Text)

Switch to ALL CAPS when printing extended tokens

[X]

Subroutine [MT13](elite_a.md#header-mt13) (category: Text)

Switch to lower case when printing extended tokens

[X]

Subroutine [MT14](elite_a.md#header-mt14) (category: Text)

Switch to justified text when printing extended tokens

[X]

Subroutine [MT15](elite_a.md#header-mt15) (category: Text)

Switch to left-aligned text when printing extended tokens

[X]

Subroutine [MT16](elite_b.md#header-mt16) (category: Text)

Print the character in variable DTW7

[X]

Subroutine [MT17](elite_a.md#header-mt17) (category: Text)

Print the selected system's adjective, e.g. Lavian for Lave

[X]

Label [MT171](elite_a.md#mt171) in subroutine [MT17](elite_a.md#header-mt17)

[X]

Subroutine [MT18](elite_a.md#header-mt18) (category: Text)

Print a random 1-8 letter word in Sentence Case

[X]

Label [MT18L](elite_a.md#mt18l) in subroutine [MT18](elite_a.md#header-mt18)

[X]

Subroutine [MT19](elite_a.md#header-mt19) (category: Text)

Capitalise the next letter

[X]

Subroutine [MT2](elite_a.md#header-mt2) (category: Text)

Switch to Sentence Case when printing extended tokens

[X]

Subroutine [MT23](elite_c.md#header-mt23) (category: Text)

Move to row 10, switch to white text, and switch to lower case when printing extended tokens

[X]

Subroutine [MT26](elite_f.md#header-mt26) (category: Text)

Fetch a line of text from the keyboard

[X]

Subroutine [MT27](elite_a.md#header-mt27) (category: Text)

Print the captain's name during mission briefings

[X]

Subroutine [MT28](elite_a.md#header-mt28) (category: Text)

Print the location hint during the mission 1 briefing

[X]

Subroutine [MT29](elite_c.md#header-mt29) (category: Text)

Move to row 6, switch to white text, and switch to lower case when printing extended tokens

[X]

Subroutine [MT5](elite_a.md#header-mt5) (category: Text)

Switch to extended tokens

[X]

Subroutine [MT6](elite_a.md#header-mt6) (category: Text)

Switch to standard tokens in Sentence Case

[X]

Subroutine [MT8](elite_a.md#header-mt8) (category: Text)

Tab to column 6 and start a new word when printing extended tokens

[X]

Subroutine [MT9](elite_a.md#header-mt9) (category: Text)

Clear the screen and set the current view type to 1

[X]

Variable [MTIN](elite_c.md#header-mtin) (category: Text)

Lookup table for random tokens in the extended token table (0-37)

[X]

Label [MU8K](elite_a.md#mu8k) in subroutine [ADDK](elite_a.md#header-addk)

[X]

Label [MU9K](elite_a.md#mu9k) in subroutine [ADDK](elite_a.md#header-addk)

[X]

Subroutine [MVEIT (Part 1 of 9)](elite_h.md#header-mveit-part-1-of-9) (category: Moving)

Move current ship: Tidy the orientation vectors

[X]

Configuration variable [Mlas](workspaces.md#mlas)

Mining laser power

[X]

Variable [NEWB](workspaces.md#newb) in workspace [ZP](workspaces.md#header-zp)

The ship's "new byte flags" (or NEWB flags)

[X]

Configuration variable [NI%](workspaces.md#ni-per-cent)

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Subroutine [NLIN4](elite_b.md#header-nlin4) (category: Drawing lines)

Draw a horizontal line at pixel row 19 to box in a title

[X]

Variable [NMI](workspaces.md#nmi) in workspace [WP](workspaces.md#header-wp)

Used to store the ID of the current owner of the NMI workspace in the NMICLAIM routine, so we can hand it back to them in the NMIRELEASE routine once we are done using it

[X]

Subroutine [NMICLAIM](elite_a.md#header-nmiclaim) (category: Utility routines)

Claim the NMI workspace (&00A0 to &00A7) back from the MOS so the game can use it once again

[X]

Subroutine [NOISE](elite_a.md#header-noise) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Variable [NOMSL](workspaces.md#nomsl) in workspace [WP](workspaces.md#header-wp)

The number of missiles we have fitted (0-4)

[X]

Subroutine [NWSPS](elite_e.md#header-nwsps) (category: Universe)

Add a new space station to our local bubble of universe

[X]

Configuration variable [OIL](workspaces.md#oil)

Ship type for a cargo canister

[X]

Subroutine [OOPS](elite_e.md#header-oops) (category: Flight)

Take some damage

[X]

Configuration variable [OSBYTE](workspaces.md#osbyte)

The address for the OSBYTE routine

[X]

Subroutine [OTHERFILEPR](elite_f.md#header-otherfilepr) (category: Save and load)

Display the non-selected media (disk or tape)

[X]

Variable [P](workspaces.md#p) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [PAUSE](elite_c.md#header-pause) (category: Missions)

Display a rotating ship, waiting until a key is pressed, then remove the ship from the screen

[X]

Subroutine [PAUSE2](elite_c.md#header-pause2) (category: Keyboard)

Wait until a key is pressed, ignoring any existing key press

[X]

Configuration variable [PLT](workspaces.md#plt)

Ship type for an alloy plate

[X]

Subroutine [PLUT](elite_h.md#header-plut) (category: Flight)

Flip the coordinate axes for the four different views

[X]

Configuration variable [POW](workspaces.md#pow)

Pulse laser power

[X]

Label [PX2](elite_a.md#px2) in subroutine [PIXEL](elite_a.md#header-pixel)

[X]

Label [PX21](elite_a.md#px21) in subroutine [PIXEL2](elite_a.md#header-pixel2)

[X]

Label [PX22](elite_a.md#px22) in subroutine [PIXEL2](elite_a.md#header-pixel2)

[X]

Entry point [PXR1](elite_a.md#pxr1) in subroutine [PIXEL](elite_a.md#header-pixel) (category: Drawing pixels)

Contains an RTS

[X]

Subroutine [PZW](elite_a.md#header-pzw) (category: Dashboard)

Fetch the current dashboard colours, to support flashing

[X]

Subroutine [PZW2](elite_a.md#header-pzw2) (category: Dashboard)

Fetch the current dashboard colours for non-striped indicators, to support flashing

[X]

Variable [Q](workspaces.md#q) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [Q%](workspaces.md#q-per-cent)

Set Q% to TRUE to max out the default commander, FALSE for the standard default commander

[X]

Variable [QQ0](workspaces.md#qq0) in workspace [WP](workspaces.md#header-wp)

The current system's galactic x-coordinate (0-256)

[X]

Variable [QQ1](workspaces.md#qq1) in workspace [WP](workspaces.md#header-wp)

The current system's galactic y-coordinate (0-256)

[X]

Variable [QQ11](workspaces.md#qq11) in workspace [ZP](workspaces.md#header-zp)

The type of the current view

[X]

Variable [QQ14](workspaces.md#qq14) in workspace [WP](workspaces.md#header-wp)

Our current fuel level (0-70)

[X]

Variable [QQ17](workspaces.md#qq17) in workspace [ZP](workspaces.md#header-zp)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Variable [QQ20](workspaces.md#qq20) in workspace [WP](workspaces.md#header-wp)

The contents of our cargo hold

[X]

Variable [QQ22](workspaces.md#qq22) in workspace [ZP](workspaces.md#header-zp)

The two hyperspace countdown counters

[X]

Variable [QQ29](workspaces.md#qq29) in workspace [WP](workspaces.md#header-wp)

Temporary storage, used in a number of places

[X]

Variable [R](workspaces.md#r) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Label [R5](elite_a.md#r5) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Variable [RAND](workspaces.md#rand) in workspace [ZP](workspaces.md#header-zp)

Four 8-bit seeds for the random number generation system implemented in the DORND routine

[X]

Configuration variable [RED](workspaces.md#red)

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Configuration variable [RED2](workspaces.md#red2)

Two mode 2 pixels of colour 1 (red)

[X]

Subroutine [RES2](elite_f.md#header-res2) (category: Start and end)

Reset a number of flight variables and workspaces

[X]

Subroutine [RETURN](elite_a.md#header-return) (category: Keyboard)

Scan the keyboard to see if RETURN is currently pressed

[X]

Label [RR1](elite_a.md#rr1) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Label [RR2](elite_a.md#rr2) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Label [RR3](elite_a.md#rr3) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Entry point [RR4](elite_a.md#rr4) in subroutine [CHPR](elite_a.md#header-chpr) (category: Text)

Restore the registers and return from the subroutine

[X]

Label [RR4S](elite_a.md#rr4s) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Label [RR5](elite_a.md#rr5) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Label [RRL1](elite_a.md#rrl1) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Label [RRNEW](elite_a.md#rrnew) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Label [RRX1](elite_a.md#rrx1) in subroutine [CHPR](elite_a.md#header-chpr)

[X]

Configuration variable [RUTOK](workspaces.md#rutok)

The address of the extended system description token table, as set in elite-data.asm

[X]

Variable [S](workspaces.md#s) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [SC](workspaces.md#sc) in workspace [ZP](workspaces.md#header-zp)

Screen address (low byte)

[X]

Label [SC2](elite_a.md#sc2) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [SC3](elite_a.md#sc3) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Subroutine [SCAN](elite_a.md#header-scan) (category: Dashboard)

Display the current ship on the scanner

[X]

Label [SCD6](elite_a.md#scd6) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Variable [SCH](workspaces.md#sch) in workspace [ZP](workspaces.md#header-zp)

Screen address (high byte)

[X]

Label [SCR1](elite_a.md#scr1) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Subroutine [SFS1](elite_c.md#header-sfs1) (category: Universe)

Spawn a child ship from the current (parent) ship

[X]

Variable [SFXBT](elite_a.md#header-sfxbt) (category: Sound)

Sound data block 2

[X]

Variable [SFXFQ](elite_a.md#header-sfxfq) (category: Sound)

Sound data block 3

[X]

Variable [SFXPR](elite_a.md#header-sfxpr) (category: Sound)

Sound data block 1

[X]

Variable [SFXVC](elite_a.md#header-sfxvc) (category: Sound)

Sound data block 4

[X]

Subroutine [SHD](elite_e.md#header-shd) (category: Flight)

Charge a shield and drain some energy from the energy banks

[X]

Variable [SOCNT](elite_a.md#socnt) in workspace [Sound variables](elite_a.md#header-sound-variables)

Sound buffer for sound effect counters

[X]

Variable [SOFH](elite_a.md#header-sofh) (category: Sound)

Sound chip data mask for choosing a tone channel in the range 0-2

[X]

Variable [SOFLG](elite_a.md#soflg) in workspace [Sound variables](elite_a.md#header-sound-variables)

Sound buffer for sound effect flags

[X]

Variable [SOFRCH](elite_a.md#sofrch) in workspace [Sound variables](elite_a.md#header-sound-variables)

Sound buffer for frequency change values

[X]

Variable [SOFRQ](elite_a.md#sofrq) in workspace [Sound variables](elite_a.md#header-sound-variables)

Sound buffer for sound effect frequencies

[X]

Subroutine [SOINT](elite_a.md#header-soint) (category: Sound)

Process the contents of the sound buffer and send it to the sound chip

[X]

Label [SOKILL](elite_a.md#sokill) in subroutine [SOINT](elite_a.md#header-soint)

[X]

Variable [SOOFF](elite_a.md#header-sooff) (category: Sound)

Sound chip data to turn the volume down on all channels and to act as a mask for choosing a tone channel in the range 0-2

[X]

Variable [SOPR](elite_a.md#sopr) in workspace [Sound variables](elite_a.md#header-sound-variables)

Sound buffer for sound effect priorities

[X]

Label [SOU1](elite_a.md#sou1) in subroutine [SOINT](elite_a.md#header-soint)

[X]

Label [SOU3](elite_a.md#sou3) in subroutine [SOINT](elite_a.md#header-soint)

[X]

Label [SOUL1](elite_a.md#soul1) in subroutine [SOFLUSH](elite_a.md#header-soflush)

[X]

Label [SOUL2](elite_a.md#soul2) in subroutine [SOFLUSH](elite_a.md#header-soflush)

[X]

Label [SOUL3](elite_a.md#soul3) in subroutine [SOINT](elite_a.md#header-soint)

[X]

Label [SOUL4](elite_a.md#soul4) in subroutine [SOINT](elite_a.md#header-soint)

[X]

Label [SOUL5](elite_a.md#soul5) in subroutine [SOINT](elite_a.md#header-soint)

[X]

Label [SOUL6](elite_a.md#soul6) in subroutine [SOINT](elite_a.md#header-soint)

[X]

Label [SOUL8](elite_a.md#soul8) in subroutine [SOINT](elite_a.md#header-soint)

[X]

Entry point [SOUR1](elite_a.md#sour1) in subroutine [SOUS1](elite_a.md#header-sous1) (category: Sound)

Contains an RTS

[X]

Subroutine [SOUS1](elite_a.md#header-sous1) (category: Sound)

Write sound data directly to the 76489 sound chip

[X]

Label [SOUS4](elite_a.md#sous4) in subroutine [NOISE](elite_a.md#header-noise)

[X]

Variable [SOVCH](elite_a.md#sovch) in workspace [Sound variables](elite_a.md#header-sound-variables)

Sound buffer for the volume change rate

[X]

Variable [SOVOL](elite_a.md#sovol) in workspace [Sound variables](elite_a.md#header-sound-variables)

Sound buffer for volume levels

[X]

Variable [SPBT](elite_a.md#header-spbt) (category: Dashboard)

The bitmap definition for the space station indicator bulb

[X]

Subroutine [SPIN](elite_a.md#header-spin) (category: Universe)

Randomly spawn cargo from a destroyed ship

[X]

Entry point [SPIN2](elite_a.md#spin2) in subroutine [SPIN](elite_a.md#header-spin) (category: Universe)

Remove any randomness: spawn cargo of a specific type (given in X), and always spawn the number given in A

[X]

Configuration variable [SPL](workspaces.md#spl)

Ship type for a splinter

[X]

Subroutine [SPS1](elite_f.md#header-sps1) (category: Maths (Geometry))

Calculate the vector to the planet and store it in XX15

[X]

Variable [SSPR](workspaces.md#sspr) in workspace [WP](workspaces.md#header-wp)

"Space station present" flag

[X]

Configuration variable [SST](workspaces.md#sst)

Ship type for a Coriolis space station

[X]

Subroutine [STARS](elite_b.md#header-stars) (category: Stardust)

The main routine for processing the stardust

[X]

Label [STPX](elite_a.md#stpx) in subroutine [LOINQ (Part 2 of 7)](elite_a.md#header-loinq-part-2-of-7)

[X]

Label [STPY](elite_a.md#stpy) in subroutine [LOINQ (Part 5 of 7)](elite_a.md#header-loinq-part-5-of-7)

[X]

Configuration variable [STRIPE](workspaces.md#stripe)

Two mode 2 pixels of colour 5, 1 (magenta/red)

[X]

Variable [SWAP](workspaces.md#swap) in workspace [WP](workspaces.md#header-wp)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[X]

Variable [SYL](workspaces.md#syl) in workspace [WP](workspaces.md#header-wp)

This is where we store the y_lo coordinates for all the stardust particles

[X]

Variable [T](workspaces.md#t) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [T1](workspaces.md#t1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [TALLY](workspaces.md#tally) in workspace [WP](workspaces.md#header-wp)

Our combat rank

[X]

Configuration variable [THG](workspaces.md#thg)

Ship type for a Thargoid

[X]

Configuration variable [TKN1](workspaces.md#tkn1)

The address of the extended token table, as set in elite-data.asm

[X]

Variable [TKN2](elite_a.md#header-tkn2) (category: Text)

The extended two-letter token lookup table

[X]

Variable [TP](workspaces.md#tp) in workspace [WP](workspaces.md#header-wp)

The current mission status

[X]

Subroutine [TT27](elite_e.md#header-tt27) (category: Text)

Print a text token

[X]

Subroutine [TT66](elite_h.md#header-tt66) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Subroutine [TT67K](elite_a.md#header-tt67k) (category: Text)

Print a newline

[X]

Subroutine [TTX66](elite_a.md#header-ttx66) (category: Drawing the screen)

Clear the top part of the screen and draw a border box

[X]

Variable [TVT1](elite_a.md#header-tvt1) (category: Drawing the screen)

Palette data for the mode 2 part of the screen (the dashboard)

[X]

Variable [TVT3](elite_a.md#header-tvt3) (category: Drawing the screen)

Palette data for the mode 1 part of the screen (the top part)

[X]

Variable [TWFL](elite_a.md#header-twfl) (category: Drawing pixels)

Ready-made character rows for the left end of a horizontal line

[X]

Variable [TWFR](elite_a.md#header-twfr) (category: Drawing pixels)

Ready-made character rows for the right end of a horizontal line

[X]

Variable [TWOS](elite_a.md#header-twos) (category: Drawing pixels)

Ready-made single-pixel character row bytes for mode 1

[X]

Variable [TWOS2](elite_a.md#header-twos2) (category: Drawing pixels)

Ready-made double-pixel character row bytes for mode 1

[X]

Variable [TYPE](workspaces.md#type) in workspace [ZP](workspaces.md#header-zp)

The current ship type

[X]

Variable [U](workspaces.md#u) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [V](workspaces.md#v) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing an address pointer

[X]

Configuration variable [VE](workspaces.md#ve)

The obfuscation byte used to hide the extended tokens table from crackers viewing the binary code

[X]

Variable [VEC](elite_a.md#header-vec) (category: Drawing the screen)

The original value of the IRQ1 vector

[X]

Configuration variable [VIA](workspaces.md#via)

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [VIEW](workspaces.md#view) in workspace [WP](workspaces.md#header-wp)

The number of the current space view

[X]

Label [VLO1](elite_a.md#vlo1) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [VLO3](elite_a.md#vlo3) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [VLO4](elite_a.md#vlo4) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [VLO5](elite_a.md#vlo5) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [VLO6](elite_a.md#vlo6) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [VLO7](elite_a.md#vlo7) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [VLOL1](elite_a.md#vlol1) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [VLOL2](elite_a.md#vlol2) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Label [VNT2](elite_a.md#vnt2) in subroutine [IRQ1](elite_a.md#header-irq1)

[X]

Label [VNT3](elite_a.md#vnt3) in subroutine [IRQ1](elite_a.md#header-irq1)

[X]

Variable [VOL](elite_a.md#vol) in workspace [UP](elite_a.md#header-up)

The volume level for the game's sound effects (0-7)

[X]

Subroutine [VOWEL](elite_a.md#header-vowel) (category: Text)

Test whether a character is a vowel

[X]

Label [VRTS](elite_a.md#vrts) in subroutine [VOWEL](elite_a.md#header-vowel)

[X]

Variable [VSCAN](elite_a.md#header-vscan) (category: Drawing the screen)

Defines the split position in the split-screen mode

[X]

Variable [W](workspaces.md#w) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [WARP](elite_f.md#header-warp) (category: Flight)

Perform an in-system jump

[X]

Configuration variable [WHITE](workspaces.md#white)

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[X]

Configuration variable [WHITE2](workspaces.md#white2)

Two mode 2 pixels of colour 7 (white)

[X]

Subroutine [WPLS](elite_e.md#header-wpls) (category: Drawing suns)

Remove the sun from the screen

[X]

Subroutine [WSCAN](elite_a.md#header-wscan) (category: Drawing the screen)

Implement the #wscn command (wait for the vertical sync)

[X]

Variable [X1](workspaces.md#x1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](workspaces.md#x2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XC](workspaces.md#xc) in workspace [ZP](workspaces.md#header-zp)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [XSAV](workspaces.md#xsav) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX0](workspaces.md#xx0) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX12](workspaces.md#xx12) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX15](workspaces.md#xx15) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Configuration variable [XX21](workspaces.md#xx21)

The address of the ship blueprints lookup table, as set in elite-data.asm

[X]

Configuration variable [Y](workspaces.md#y)

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Y1](workspaces.md#y1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](workspaces.md#y2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [YC](workspaces.md#yc) in workspace [ZP](workspaces.md#header-zp)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Configuration variable [YELLOW](workspaces.md#yellow)

Four mode 1 pixels of colour 1 (yellow)

[X]

Configuration variable [YELLOW2](workspaces.md#yellow2)

Two mode 2 pixels of colour 3 (yellow)

[X]

Variable [YSAV](workspaces.md#ysav) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for saving the value of the Y register, used in a number of places

[X]

Variable [YY](workspaces.md#yy) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing a 16-bit y-coordinate

[X]

Label [ZEL1](elite_a.md#zel1) in subroutine [ZES2](elite_a.md#header-zes2)

[X]

Subroutine [ZES1](elite_a.md#header-zes1) (category: Utility routines)

Zero-fill the page whose number is in X

[X]

Subroutine [ZES2](elite_a.md#header-zes2) (category: Utility routines)

Zero-fill a specific page

[X]

Workspace [ZP](workspaces.md#header-zp) (category: Workspaces)

Lots of important variables are stored in the zero page workspace as it is quicker and more space-efficient to access memory here

[X]

Variable [ZZ](workspaces.md#zz) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for distance values

[X]

Variable [alogh](elite_a.md#header-alogh) (category: Maths (Arithmetic))

Binary antilogarithm table

[X]

Variable [auto](workspaces.md#auto) in workspace [WP](workspaces.md#header-wp)

Docking computer activation status

[X]

Entry point [away](elite_a.md#away) in subroutine [SPBLB](elite_a.md#header-spblb) (category: Dashboard)

Switch main memory back into &3000-&7FFF and return from the subroutine

[X]

Label [b](elite_a.md#b) in subroutine [Main flight loop (Part 13 of 16)](elite_a.md#header-main-flight-loop-part-13-of-16)

[X]

Subroutine [cls](elite_a.md#header-cls) (category: Drawing the screen)

Clear the top part of the screen and draw a border box

[X]

Subroutine [cntr](elite_c.md#header-cntr) (category: Dashboard)

Apply damping to the pitch or roll dashboard indicator

[X]

Variable [de](workspaces.md#de) in workspace [WP](workspaces.md#header-wp)

Equipment destruction flag

[X]

Entry point [getzp+3](elite_a.md#getzp) in subroutine [getzp](elite_a.md#header-getzp) (category: Utility routines)

Restore the top part of zero page, but without first claiming the NMI workspace (Master Compact variant only)

[X]

Label [j2vec](elite_a.md#j2vec) in subroutine [IRQ1](elite_a.md#header-irq1)

[X]

Label [jvec](elite_a.md#jvec) in subroutine [IRQ1](elite_a.md#header-irq1)

[X]

Label [ld246](elite_a.md#ld246) in subroutine [SCAN](elite_a.md#header-scan)

[X]

Variable [log](elite_a.md#header-log) (category: Maths (Arithmetic))

Binary logarithm table (high byte)

[X]

Variable [logL](elite_a.md#header-logl) (category: Maths (Arithmetic))

Binary logarithm table (low byte)

[X]

Entry point [m](elite_b.md#m) in subroutine [MAS2](elite_b.md#header-mas2) (category: Maths (Geometry))

Do not include A in the calculation

[X]

Label [ma1](elite_a.md#ma1) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [noescp](elite_a.md#noescp) in subroutine [Main flight loop (Part 3 of 16)](elite_a.md#header-main-flight-loop-part-3-of-16)

[X]

Label [nosp](elite_a.md#nosp) in subroutine [Main flight loop (Part 11 of 16)](elite_a.md#header-main-flight-loop-part-11-of-16)

[X]

Entry point [oh](elite_a.md#oh) in subroutine [SPIN](elite_a.md#header-spin) (category: Universe)

Contains an RTS

[X]

Label [oily](elite_a.md#oily) in subroutine [Main flight loop (Part 8 of 16)](elite_a.md#header-main-flight-loop-part-8-of-16)

[X]

Variable [orange](elite_a.md#header-orange) (category: Drawing pixels)

Lookup table for two-pixel mode 1 orange pixels for the sun

[X]

Label [paen1](elite_a.md#paen1) in subroutine [Main flight loop (Part 13 of 16)](elite_a.md#header-main-flight-loop-part-13-of-16)

[X]

Variable [scacol](elite_a.md#header-scacol) (category: Drawing ships)

Ship colours on the scanner

[X]

Label [slvy2](elite_a.md#slvy2) in subroutine [Main flight loop (Part 8 of 16)](elite_a.md#header-main-flight-loop-part-8-of-16)

[X]

Configuration variable [sobeep](workspaces.md#sobeep)

Sound 1 = Short, high beep

[X]

Configuration variable [sobomb](workspaces.md#sobomb)

Sound 6 = Energy bomb

[X]

Configuration variable [soboop](workspaces.md#soboop)

Sound 0 = Long, low beep

[X]

Configuration variable [soecm](workspaces.md#soecm)

Sound 7 = E.C.M. on

[X]

Label [spl](elite_a.md#spl) in subroutine [SPIN](elite_a.md#header-spin)

[X]

[X]

Label [sz1](elite_a.md#sz1) in subroutine [setzp](elite_a.md#header-setzp)

[X]

Label [sz2](elite_a.md#sz2) in subroutine [getzp](elite_a.md#header-getzp)

[X]

Subroutine [tnpr1](elite_d.md#header-tnpr1) (category: Market)

Work out if we have space for one tonne of cargo

[X]

Variable [ylookup](elite_a.md#header-ylookup) (category: Drawing pixels)

Lookup table for converting pixel y-coordinate to page number of screen address

[Workspaces and configuration](workspaces.md "Previous routine")[Elite B source](elite_b.md "Next routine")
