---
title: "The IRQ1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/irq1.html"
---

[TVT1](../variable/tvt1.md "Previous routine")[VSCAN](../variable/vscan.md "Next routine")
    
    
           Name: IRQ1                                                    [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: The main screen-mode interrupt handler (IRQ1V points here)
      Deep dive: [The split-screen mode in BBC Micro Elite](https://elite.bbcelite.com/deep_dives/the_split-screen_mode.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-irq1)
     Variations: See [code variations](../../related/compare/main/subroutine/irq1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SETINTS](setints.md) calls IRQ1
    
    
    
    
    
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
    

[X]

Variable [DL](../workspace/zp.md#dl) in workspace [ZP](../workspace/zp.md)

Vertical sync flag

[X]

Variable [ESCP](../workspace/wp.md#escp) in workspace [WP](../workspace/wp.md)

Escape pod

[X]

Variable [HFX](../workspace/wp.md#hfx) in workspace [WP](../workspace/wp.md)

A flag that toggles the hyperspace colour effect

[X]

Variable [JOPOS](../workspace/wp.md#jopos) in workspace [WP](../workspace/wp.md)

Contains the high byte of the latest value from ADC channel 1 (the joystick X value), which gets updated regularly by the IRQ1 interrupt handler

[X]

Label [LINSCN](irq1.md#linscn) is local to this routine

[X]

Variable [MOS](../workspace/zp.md#mos) in workspace [ZP](../workspace/zp.md)

Determines whether we are running on a Master Compact

[X]

Subroutine [SOINT](soint.md) (category: Sound)

Process the contents of the sound buffer and send it to the sound chip

[X]

Variable [TVT1](../variable/tvt1.md) (category: Drawing the screen)

Palette data for the mode 2 part of the screen (the dashboard)

[X]

Variable [TVT3](../variable/tvt3.md) (category: Drawing the screen)

Palette data for the mode 1 part of the screen (the top part)

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Label [VNT2](irq1.md#vnt2) is local to this routine

[X]

Label [VNT3](irq1.md#vnt3) is local to this routine

[X]

Variable [VSCAN](../variable/vscan.md) (category: Drawing the screen)

Defines the split position in the split-screen mode

[X]

Label [j2vec](irq1.md#j2vec) is local to this routine

[X]

Label [jvec](irq1.md#jvec) is local to this routine

[TVT1](../variable/tvt1.md "Previous routine")[VSCAN](../variable/vscan.md "Next routine")
