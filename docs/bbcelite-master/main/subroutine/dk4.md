---
title: "The DK4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dk4.html"
---

[DOKEY](dokey.md "Previous routine")[TT217](tt217.md "Next routine")
    
    
           Name: DK4                                                     [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan for pause, configuration and secondary flight keys
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-dk4)
     Variations: See [code variations](../../related/compare/main/subroutine/dk4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOKEY](dokey.md) calls DK4
    
    
    
    
    
    * * *
    
    
     Scan for pause and configuration keys, and if this is a space view, also scan
     for secondary flight controls.
    
     Specifically:
    
       * Scan for the pause button (COPY) and if it's pressed, pause the game and
         process any configuration key presses until the game is unpaused (DELETE)
    
       * If this is a space view, scan for secondary flight keys and update the
         relevant bytes in the key logger
    
    
    
    
    .DK4
    
     LDX KL                 \ Fetch the key pressed from byte #0 of the key logger
    
     CPX #&8B               \ If COPY is not being pressed, jump to DK2 below,
     BNE DK2                \ otherwise let's process the configuration keys
    
    .FREEZE
    
                            \ COPY is being pressed, so we enter a loop that
                            \ listens for configuration keys, and we keep looping
                            \ until we detect a DELETE key press. This effectively
                            \ pauses the game when COPY is pressed, and unpauses
                            \ it when DELETE is pressed
    
     JSR WSCAN              \ Call WSCAN to wait for the vertical sync, so the whole
                            \ screen gets drawn
    
     JSR RDKEY              \ Scan the keyboard for a key press and return the ASCII
                            \ code of the key pressed in A and X (or 0 for no key
                            \ press)
    
     CPX #'Q'               \ If "Q" is not being pressed, skip to DK6
     BNE DK6
    
     LDX #&FF               \ "Q" is being pressed, so set DNOIZ to &FF to turn the
     STX DNOIZ              \ sound off
    
     LDX #'Q'               \ Set X to the ASCII for "Q" once again, so it doesn't
                            \ get changed by the above
    
    .DK6
    
     LDY #0                 \ We now want to loop through the keys that toggle
                            \ various settings, so set a counter in Y to work our
                            \ way through them
    
    .DKL4
    
     JSR DKS3               \ Call DKS3 to scan for the key given in Y, and toggle
                            \ the relevant setting if it is pressed
    
     INY                    \ Increment Y to point to the next toggle key
    
     CPY #9                 \ Check to see whether we have reached the last toggle
                            \ key
    
     BNE DKL4               \ If not, loop back to check for the next toggle key
    
     LDA VOL                \ Fetch the current volume setting into A
    
     CPX #'.'               \ If "." is being pressed (i.e. the ">" key) then jump
     BEQ DOVOL1             \ to DOVOL1 to increase the volume
    
     CPX #','               \ If "," is not being pressed (i.e. the "<" key) then
     BNE DOVOL4             \ jump to DOVOL4 to skip the following
    
     DEC A                  \ The volume down key is being pressed, so decrement the
                            \ volume level in A
    
     EQUB &24               \ Skip the next instruction by turning it into &24 &1A,
                            \ or BIT &001A, which does nothing apart from affect the
                            \ flags
    
    .DOVOL1
    
     INC A                  \ The volume up key is being pressed, so increment the
                            \ volume level in A
    
     TAY                    \ Copy the new volume level to Y
    
     AND #%11111000         \ If any of bits 3-7 are set, skip to DOVOL3 as we have
     BNE DOVOL3             \ either increased the volume past the maximum volume of
                            \ 7, or we have decreased it below 0 to -1, and in
                            \ neither case do we want to change the volume as we are
                            \ already at the maximum or minimum level
    
     STY VOL                \ Store the new volume level in VOL
    
    .DOVOL3
    
     PHX                    \ Store X on the stack so we can retrieve it below after
                            \ making a beep
    
     JSR BEEP               \ Call the BEEP subroutine to make a short, high beep at
                            \ the new volume level
    
     LDY #10                \ Wait for 10/50 of a second (0.2 seconds)
     JSR DELAY
    
     PLX                    \ Restore the value of X we stored above
    
    .DOVOL4
    
     CPX #'B'               \ If "B" is not being pressed, skip to DOVOL2
     BNE DOVOL2
    
     LDA BSTK               \ Toggle the value of BSTK between 0 and &FF
     EOR #&FF
     STA BSTK
    
     STA JSTK               \ Configure JSTK to the same value, so when the Bitstik
                            \ is enabled, so is the joystick
    
     STA JSTE               \ Configure JSTE to the same value, so when the Bitstik
                            \ is enabled, the joystick is configured with reversed
                            \ channels
    
     BPL P%+5               \ If we just toggled the Bitstik off (i.e. to 0, which
                            \ is positive), then skip the first of these two
                            \ instructions, so we get two beeps for on and one beep
                            \ for off
    
     JSR BELL               \ We just enabled the Bitstik, so give two standard
                            \ system beeps (this being the first)
    
     JSR BELL               \ Make another system beep
    
    .DOVOL2
    
     CPX #'S'               \ If "S" is not being pressed, jump to DK7
     BNE DK7
    
     LDA #0                 \ "S" is being pressed, so set DNOIZ to 0 to turn the
     STA DNOIZ              \ sound on
    
    .DK7
    
     CPX #&1B               \ If ESCAPE is not being pressed, skip over the next
     BNE P%+5               \ instruction
    
     JMP DEATH2             \ ESCAPE is being pressed, so jump to DEATH2 to end
                            \ the game
    
     CPX #&7F               \ If DELETE is not being pressed, we are still paused,
     BNE FREEZE             \ so loop back up to keep listening for configuration
                            \ keys, otherwise fall through into the rest of the
                            \ key detection code, which unpauses the game
    
    .DK2
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [BEEP](beep.md) (category: Sound)

Make a short, high beep

[X]

Subroutine [BELL](bell.md) (category: Sound)

Make a standard system beep

[X]

Variable [BSTK](../workspace/up.md#bstk) in workspace [UP](../workspace/up.md)

Bitstik configuration setting

[X]

Subroutine [DEATH2](death2.md) (category: Start and end)

Reset most of the game and restart from the title screen

[X]

Subroutine [DELAY](delay.md) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Label [DK2](dk4.md#dk2) is local to this routine

[X]

Label [DK6](dk4.md#dk6) is local to this routine

[X]

Label [DK7](dk4.md#dk7) is local to this routine

[X]

Label [DKL4](dk4.md#dkl4) is local to this routine

[X]

Subroutine [DKS3](dks3.md) (category: Keyboard)

Toggle a configuration setting and emit a beep

[X]

Variable [DNOIZ](../workspace/up.md#dnoiz) in workspace [UP](../workspace/up.md)

Sound on/off configuration setting

[X]

Label [DOVOL1](dk4.md#dovol1) is local to this routine

[X]

Label [DOVOL2](dk4.md#dovol2) is local to this routine

[X]

Label [DOVOL3](dk4.md#dovol3) is local to this routine

[X]

Label [DOVOL4](dk4.md#dovol4) is local to this routine

[X]

Label [FREEZE](dk4.md#freeze) is local to this routine

[X]

Variable [JSTE](../workspace/up.md#jste) in workspace [UP](../workspace/up.md)

Reverse both joystick channels configuration setting

[X]

Variable [JSTK](../workspace/up.md#jstk) in workspace [UP](../workspace/up.md)

Keyboard or joystick configuration setting

[X]

Variable [KL](../workspace/zp.md#kl) in workspace [ZP](../workspace/zp.md)

The following bytes implement a key logger that enables Elite to scan for concurrent key presses of the primary flight keys, plus a secondary flight key

[X]

Subroutine [RDKEY](rdkey.md) (category: Keyboard)

Scan the keyboard for key presses and update the key logger

[X]

Variable [VOL](../workspace/up.md#vol) in workspace [UP](../workspace/up.md)

The volume level for the game's sound effects (0-7)

[X]

Subroutine [WSCAN](wscan.md) (category: Drawing the screen)

Implement the #wscn command (wait for the vertical sync)

[DOKEY](dokey.md "Previous routine")[TT217](tt217.md "Next routine")
