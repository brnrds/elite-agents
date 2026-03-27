---
title: "The SOINT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/soint.html"
---

[NOISE](noise.md "Previous routine")[Sound variables](../workspace/sound_variables.md "Next routine")
    
    
           Name: SOINT                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Process the contents of the sound buffer and send it to the sound
                 chip
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-soint)
     References: This subroutine is called as follows:
                 * [IRQ1](irq1.md) calls SOINT
    
    
    
    
    
    
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
    

[X]

Variable [SOCNT](../workspace/sound_variables.md#socnt) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for sound effect counters

[X]

Variable [SOFH](../variable/sofh.md) (category: Sound)

Sound chip data mask for choosing a tone channel in the range 0-2

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

Label [SOKILL](soint.md#sokill) is local to this routine

[X]

Variable [SOOFF](../variable/sooff.md) (category: Sound)

Sound chip data to turn the volume down on all channels and to act as a mask for choosing a tone channel in the range 0-2

[X]

Variable [SOPR](../workspace/sound_variables.md#sopr) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for sound effect priorities

[X]

Label [SOU1](soint.md#sou1) is local to this routine

[X]

Label [SOU3](soint.md#sou3) is local to this routine

[X]

Label [SOUL3](soint.md#soul3) is local to this routine

[X]

Label [SOUL4](soint.md#soul4) is local to this routine

[X]

Label [SOUL5](soint.md#soul5) is local to this routine

[X]

Label [SOUL6](soint.md#soul6) is local to this routine

[X]

Label [SOUL8](soint.md#soul8) is local to this routine

[X]

Subroutine [SOUS1](sous1.md) (category: Sound)

Write sound data directly to the 76489 sound chip

[X]

Variable [SOVCH](../workspace/sound_variables.md#sovch) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for the volume change rate

[X]

Variable [SOVOL](../workspace/sound_variables.md#sovol) in workspace [Sound variables](../workspace/sound_variables.md)

Sound buffer for volume levels

[X]

Variable [VOL](../workspace/up.md#vol) in workspace [UP](../workspace/up.md)

The volume level for the game's sound effects (0-7)

[NOISE](noise.md "Previous routine")[Sound variables](../workspace/sound_variables.md "Next routine")
