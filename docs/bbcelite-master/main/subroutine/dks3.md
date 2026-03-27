---
title: "The DKS3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dks3.html"
---

[WARP](warp.md "Previous routine")[DOKEY](dokey.md "Next routine")
    
    
           Name: DKS3                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Toggle a configuration setting and emit a beep
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-dks3)
     Variations: See [code variations](../../related/compare/main/subroutine/dks3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DK4](dk4.md) calls DKS3
    
    
    
    
    
    * * *
    
    
     This is called when the game is paused and a key is pressed that changes the
     game's configuration.
    
     Specifically, this routine toggles the configuration settings for the
     following keys:
    
       * CAPS LOCK toggles keyboard flight damping (0)
       * A toggles keyboard auto-recentre (1)
       * X toggles author names on start-up screen (2)
       * F toggles flashing console bars (3)
       * Y toggles reverse joystick Y channel (4)
       * J toggles reverse both joystick channels (5)
       * K toggles keyboard and joystick (6)
    
     The numbers in brackets are the configuration options that we pass in Y. We
     pass the ASCII code of the key that has been pressed in X, and the option to
     check it against in Y, so this routine is typically called in a loop that
     loops through the various configuration option.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The ASCII code of the key that's been pressed
    
       Y                    The number of the configuration option to check against
                            from the list above (i.e. Y must be from 0 to 6)
    
    
    
    
    .DKS3
    
     TXA                    \ Copy the ASCII code of the key that has been pressed
                            \ into A
    
     CMP TGINT,Y            \ If the pressed key doesn't match the configuration key
     BNE Dk3                \ for option Y (as listed in the TGINT table), then jump
                            \ to Dk3 to return from the subroutine
    
     LDA DAMP,Y             \ The configuration keys listed in TGINT correspond to
     EOR #&FF               \ the configuration option settings from DAMP onwards,
     STA DAMP,Y             \ so to toggle a setting, we fetch the existing byte
                            \ from DAMP+Y, invert it and put it back (0 means no
                            \ and &FF means yes in the configuration bytes, so
                            \ this toggles the setting)
    
     BPL P%+5               \ If the result has a clear bit 7 (i.e. we just turned
                            \ the option off), skip the following instruction
    
     JSR BELL               \ We just turned the option on, so make a standard
                            \ system beep, so in all we make two beeps
    
     JSR BELL               \ Make a beep sound so we know something has happened
    
     TYA                    \ Store Y and A on the stack so we can retrieve them
     PHA                    \ below
    
     LDY #20                \ Wait for 20/50 of a second (0.4 seconds)
     JSR DELAY
    
     PLA                    \ Restore A and Y from the stack
     TAY
    
    .Dk3
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [BELL](bell.md) (category: Sound)

Make a standard system beep

[X]

Variable [DAMP](../workspace/up.md#damp) in workspace [UP](../workspace/up.md)

Keyboard damping configuration setting

[X]

Subroutine [DELAY](delay.md) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Label [Dk3](dks3.md#dk3) is local to this routine

[X]

Variable [TGINT](../variable/tgint.md) (category: Keyboard)

The keys used to toggle configuration settings when the game is paused

[WARP](warp.md "Previous routine")[DOKEY](dokey.md "Next routine")
