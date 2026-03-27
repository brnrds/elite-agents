---
title: "The DOKEY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dokey.html"
---

[DKS3](dks3.md "Previous routine")[DK4](dk4.md "Next routine")
    
    
           Name: DOKEY                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan for the seven primary flight controls and apply the docking
                 computer manoeuvring code
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
                 [The docking computer](https://elite.bbcelite.com/deep_dives/the_docking_computer.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-dokey)
     Variations: See [code variations](../../related/compare/main/subroutine/dokey.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT17](tt17.md) calls DOKEY
    
    
    
    
    
    * * *
    
    
     Scan for the seven primary flight controls (or the equivalent on joystick),
     pause and configuration keys, and secondary flight controls, and update the
     key logger accordingly. Specifically:
    
       * If we are on keyboard configuration, clear the key logger and update it
         for the seven primary flight controls, and update the pitch and roll
         rates accordingly.
    
       * If we are on joystick configuration, clear the key logger and jump to
         DKJ1, which reads the joystick equivalents of the primary flight
         controls.
    
     Both options end up at DK4 to scan for other keys, beyond the seven primary
     flight controls.
    
    
    
    
    .DOKEY
    
     JSR RDKEY-1            \ Scan the keyboard for a key press and return the
                            \ ASCII code of the key pressed in X (calling RDKEY-1
                            \ only scans the keyboard for valid BCD key numbers,
                            \ which speeds things up a bit)
    
     LDA auto               \ If auto is 0, then the docking computer is not
     BEQ DK15               \ currently activated, so jump to DK15 to skip the
                            \ docking computer manoeuvring code below
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     LDA #96                \ Set nosev_z_hi = 96
     STA INWK+14
    
     ORA #%10000000         \ Set sidev_x_hi = -96
     STA INWK+22
    
     STA TYPE               \ Set the ship type to -96, so the negative value will
                            \ let us check in the DOCKIT routine whether this is our
                            \ ship that is activating its docking computer, rather
                            \ than an NPC ship docking
    
     LDA DELTA              \ Set the ship speed to DELTA (our speed)
     STA INWK+27
    
     JSR DOCKIT             \ Call DOCKIT to calculate the docking computer's moves
                            \ and update INWK with the results
    
                            \ We now "press" the relevant flight keys, depending on
                            \ the results from DOCKIT, starting with the pitch keys
    
     LDA INWK+27            \ Fetch the updated ship speed from byte #27 into A
    
     CMP #22                \ If A < 22, skip the next instruction
     BCC P%+4
    
     LDA #22                \ Set A = 22, so the maximum speed during docking is 22
    
     STA DELTA              \ Update DELTA to the new value in A
    
     LDA #&FF               \ Set A = &FF, which we can insert into the key logger
                            \ to "fake" the docking computer working the keyboard
    
     LDX #15                \ Set X = 15, so we "press" KY+15, i.e. KY1, below
                            \ ("?", slow down)
    
     LDY INWK+28            \ If the updated acceleration in byte #28 is zero, skip
     BEQ DK11               \ to DK11
    
     BMI P%+4               \ If the updated acceleration is negative, skip the
                            \ following instruction
    
     LDX #11                \ Set X = 11, so we "press" KY+11, i.e. KY2, with the
                            \ next instruction (Space, speed up)
    
     STA KL,X               \ Store &FF in either KY1 or KY2 to "press" the relevant
                            \ key, depending on whether the updated acceleration is
                            \ negative (in which case we "press" KY1, "?", to slow
                            \ down) or positive (in which case we "press" KY2,
                            \ Space, to speed up)
    
    .DK11
    
                            \ We now "press" the relevant roll keys, depending on
                            \ the results from DOCKIT
    
     LDA #128               \ Set A = 128, which indicates no change in roll when
                            \ stored in JSTX (i.e. the centre of the roll indicator)
    
     LDX #13                \ Set X = 13, so we "press" KY+13, i.e. KY3, below
                            \ ("<", increase roll)
    
     ASL INWK+29            \ Shift ship byte #29 left, which shifts bit 7 of the
                            \ updated roll counter (i.e. the roll direction) into
                            \ the C flag
    
     BEQ DK12               \ If the remains of byte #29 is zero, then the updated
                            \ roll counter is zero, so jump to DK12 set JSTX to 128,
                            \ to indicate there's no change in the roll
    
     BCC P%+4               \ If the C flag is clear, skip the following instruction
    
     LDX #14                \ The C flag is set, i.e. the direction of the updated
                            \ roll counter is negative, so set X to 14 so we
                            \ "press" KY+14. i.e. KY4, below (">", decrease roll)
    
     BIT INWK+29            \ We shifted the updated roll counter to the left above,
     BPL DK14               \ so this tests bit 6 of the original value, and if it
                            \ is clear (i.e. the magnitude is less than 64), jump to
                            \ DK14 to "press" the key and leave JSTX unchanged
    
     LDA #64                \ The magnitude of the updated roll is 64 or more, so
     STA JSTX               \ set JSTX to 64 (so the roll decreases at half the
                            \ maximum rate)
    
     LDA #0                 \ And set A = 0 so we do not "press" any keys (so if the
                            \ docking computer needs to make a serious roll, it does
                            \ so by setting JSTX directly rather than by "pressing"
                            \ a key)
    
    .DK14
    
     STA KL,X               \ Store A in either KY3 or KY4, depending on whether
                            \ the updated roll rate is increasing (KY3) or
                            \ decreasing (KY4)
    
     LDA JSTX               \ Fetch A from JSTX so the next instruction has no
                            \ effect
    
    .DK12
    
     STA JSTX               \ Store A in JSTX to update the current roll rate
    
                            \ We now "press" the relevant pitch keys, depending on
                            \ the results from DOCKIT
    
     LDA #128               \ Set A = 128, which indicates no change in pitch when
                            \ stored in JSTX (i.e. the centre of the pitch
                            \ indicator)
    
     LDX #6                 \ Set X = 6, so we "press" KY+6, i.e. KY5, below
                            \ ("X", decrease pitch, pulling the nose up)
    
     ASL INWK+30            \ Shift ship byte #30 left, which shifts bit 7 of the
                            \ updated pitch counter (i.e. the pitch direction) into
                            \ the C flag
    
     BEQ DK13               \ If the remains of byte #30 is zero, then the updated
                            \ pitch counter is zero, so jump to DK13 set JSTY to
                            \ 128, to indicate there's no change in the pitch
    
     BCS P%+4               \ If the C flag is set, skip the following instruction
    
     LDX #8                 \ Set X = 8, so we "press" KY+8, i.e. KY6, with the next
                            \ instruction ("S", increase pitch, so the nose dives)
    
     STA KL,X               \ Store 128 in either KY5 or KY6 to "press" the relevant
                            \ key, depending on whether the pitch direction is
                            \ negative (in which case we "press" KY5, "X", to
                            \ decrease the pitch, pulling the nose up) or positive
                            \ (in which case we "press" KY6, "S", to increase the
                            \ pitch, pushing the nose down)
    
     LDA JSTY               \ Fetch A from JSTY so the next instruction has no
                            \ effect
    
    .DK13
    
     STA JSTY               \ Store A in JSTY to update the current pitch rate
    
    IF _COMPACT
    
     JMP DK152              \ Jump to DK152 to skip reading the joystick, as this is
                            \ a Master Compact that doesn't support an analogue
                            \ joystick (instead it supports a digital joystick,
                            \ which is read elsewhere)
    
    ENDIF
    
    .DK15
    
     LDA JSTK               \ If JSTK is zero, then we are configured to use the
     BEQ DK152              \ keyboard rather than the joystick, so jump to DK152 to
                            \ skip reading the joystick
    
    IF _SNG47
    
     LDA JOPOS              \ Fetch the high byte of the joystick X value
    
     EOR JSTE               \ The high byte A is now EOR'd with the value in
                            \ location JSTE, which contains &FF if both joystick
                            \ channels are reversed and 0 otherwise (so A now
                            \ contains the high byte but inverted, if that's what
                            \ the current settings say)
    
     ORA #1                 \ Ensure the value is at least 1
    
     STA JSTX               \ Store the resulting joystick X value in JSTX
    
     LDA JOPOS+1            \ Fetch the high byte of the joystick Y value
    
     EOR #&FF               \ This EOR is used in conjunction with the EOR JSTGY
                            \ below, as having a value of 0 in JSTGY means we have
                            \ to invert the joystick Y value, and this EOR does
                            \ that part
    
     EOR JSTE               \ The high byte A is now EOR'd with the value in
                            \ location JSTE, which contains &FF if both joystick
                            \ channels are reversed and 0 otherwise (so A now
                            \ contains the high byte but inverted, if that's what
                            \ the current settings say)
    
     EOR JSTGY              \ JSTGY will be 0 if the game is configured to reverse
                            \ the joystick Y channel, so this EOR along with the
                            \ EOR #&FF above does exactly that
    
     STA JSTY               \ Store the resulting joystick Y value in JSTY
    
     LDA VIA+&40            \ Read 6522 System VIA input register IRB (SHEILA &40)
    
     AND #%00010000         \ Bit 4 of IRB (PB4) is clear if joystick 1's fire
                            \ button is pressed, otherwise it is set, so AND'ing
                            \ the value of IRB with %10000 extracts this bit
    
     BNE DK4                \ If the joystick fire button is not being pressed,
                            \ jump to DK4 to scan for other keys
    
     LDA #&FF               \ Update the key logger at KY7 to "press" the "A" (fire)
     STA KY7                \ button
    
     BNE DK4                \ Jump to DK4 to scan for other keys (this BNE is
                            \ effectively a JMP as A is never 0)
    
    ELIF _COMPACT
    
     JSR RDJOY              \ Call RDJOY to read from either the analogue or digital
                            \ joystick
    
     BCC DK4                \ If we just read from the analogue joystick, the C flag
                            \ will be clear, so jump to DK4 to scan for other keys
    
                            \ Otherwise we just read the digital joystick, so we now
                            \ update the pitch and roll
    
    ENDIF
    
    .DK152
    
     LDX JSTX               \ Set X = JSTX, the current roll rate (as shown in the
                            \ RL indicator on the dashboard)
    
     LDA #7                 \ Set A to 7, which is the amount we want to alter the
                            \ roll rate by if the roll keys are being pressed
    
     LDY KY3                \ If the "<" key is not being pressed, skip the next
     BEQ P%+5               \ instruction
    
     JSR BUMP2              \ The "<" key is being pressed, so call the BUMP2
                            \ routine to increase the roll rate in X by A
    
     LDY KY4                \ If the ">" key is not being pressed, skip the next
     BEQ P%+5               \ instruction
    
     JSR REDU2              \ The "<" key is being pressed, so call the REDU2
                            \ routine to decrease the roll rate in X by A, taking
                            \ the keyboard auto re-centre setting into account
    
     STX JSTX               \ Store the updated roll rate in JSTX
    
     ASL A                  \ Double the value of A, to 14
    
     LDX JSTY               \ Set X = JSTY, the current pitch rate (as shown in the
                            \ DC indicator on the dashboard)
    
     LDY KY5                \ If the "X" key is not being pressed, skip the next
     BEQ P%+5               \ instruction
    
     JSR REDU2              \ The "X" key is being pressed, so call the REDU2
                            \ routine to decrease the pitch rate in X by A, taking
                            \ the keyboard auto re-centre setting into account
    
     LDY KY6                \ If the "S" key is not being pressed, skip the next
     BEQ P%+5               \ instruction
    
     JSR BUMP2              \ The "S" key is being pressed, so call the BUMP2
                            \ routine to increase the pitch rate in X by A
    
     STX JSTY               \ Store the updated roll rate in JSTY
    
                            \ Fall through into DK4 to scan for other keys
    

[X]

Subroutine [BUMP2](bump2.md) (category: Dashboard)

Bump up the value of the pitch or roll dashboard indicator

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Label [DK11](dokey.md#dk11) is local to this routine

[X]

Label [DK12](dokey.md#dk12) is local to this routine

[X]

Label [DK13](dokey.md#dk13) is local to this routine

[X]

Label [DK14](dokey.md#dk14) is local to this routine

[X]

Label [DK15](dokey.md#dk15) is local to this routine

[X]

Label [DK152](dokey.md#dk152) is local to this routine

[X]

Subroutine [DK4](dk4.md) (category: Keyboard)

Scan for pause, configuration and secondary flight keys

[X]

Subroutine [DOCKIT](dockit.md) (category: Flight)

Apply docking manoeuvres to the ship in INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [JOPOS](../workspace/wp.md#jopos) in workspace [WP](../workspace/wp.md)

Contains the high byte of the latest value from ADC channel 1 (the joystick X value), which gets updated regularly by the IRQ1 interrupt handler

[X]

Variable [JSTE](../workspace/up.md#jste) in workspace [UP](../workspace/up.md)

Reverse both joystick channels configuration setting

[X]

Variable [JSTGY](../workspace/up.md#jstgy) in workspace [UP](../workspace/up.md)

Reverse joystick Y-channel configuration setting

[X]

Variable [JSTK](../workspace/up.md#jstk) in workspace [UP](../workspace/up.md)

Keyboard or joystick configuration setting

[X]

Variable [JSTX](../workspace/zp.md#jstx) in workspace [ZP](../workspace/zp.md)

Our current roll rate

[X]

Variable [JSTY](../workspace/zp.md#jsty) in workspace [ZP](../workspace/zp.md)

Our current pitch rate

[X]

Variable [KL](../workspace/zp.md#kl) in workspace [ZP](../workspace/zp.md)

The following bytes implement a key logger that enables Elite to scan for concurrent key presses of the primary flight keys, plus a secondary flight key

[X]

Variable [KY3](../workspace/zp.md#ky3) in workspace [ZP](../workspace/zp.md)

"<" is being pressed (roll left)

[X]

Variable [KY4](../workspace/zp.md#ky4) in workspace [ZP](../workspace/zp.md)

">" is being pressed (roll right)

[X]

Variable [KY5](../workspace/zp.md#ky5) in workspace [ZP](../workspace/zp.md)

"X" is being pressed (pull up)

[X]

Variable [KY6](../workspace/zp.md#ky6) in workspace [ZP](../workspace/zp.md)

"S" is being pressed (pitch down)

[X]

Variable [KY7](../workspace/zp.md#ky7) in workspace [ZP](../workspace/zp.md)

"A" is being pressed (fire lasers)

[X]

Subroutine [RDJOY](rdjoy.md) (category: Keyboard)

Read from either the analogue or digital joystick

[X]

Entry point [RDKEY-1](rdkey.md#rdkey) in subroutine [RDKEY](rdkey.md) (category: Keyboard)

Only scan the keyboard for valid BCD key numbers

[X]

Subroutine [REDU2](redu2.md) (category: Dashboard)

Reduce the value of the pitch or roll dashboard indicator

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Subroutine [ZINF](zinf.md) (category: Universe)

Reset the INWK workspace and orientation vectors

[X]

Variable [auto](../workspace/wp.md#auto) in workspace [WP](../workspace/wp.md)

Docking computer activation status

[DKS3](dks3.md "Previous routine")[DK4](dk4.md "Next routine")
