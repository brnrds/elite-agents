---
title: "The DJOY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/djoy.html"
---

[TT17](tt17.md "Previous routine")[KS3](ks3.md "Next routine")
    
    
           Name: DJOY                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard for cursor key or digital joystick movement
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-djoy)
     References: This subroutine is called as follows:
                 * [TT17](tt17.md) calls DJOY
    
    
    
    
    
    
    IF _COMPACT
    
    .DJOY
    
     JSR TT17X              \ Call TT17X to read the digital joystick and return
                            \ deltas as appropriate
    
     JMP TJ1                \ Jump to TJ1 in the TT17 routine to check for cursor
                            \ key presses and return the combined deltas for the
                            \ digital joystick and cursor keys, returning from the
                            \ subroutine using a tail call
    
    ENDIF
    

[X]

Entry point [TJ1](tt17.md#tj1) in subroutine [TT17](tt17.md) (category: Keyboard)

Check for cursor key presses and return the combined deltas for the digital joystick and cursor keys (Master Compact only)

[X]

Subroutine [TT17X](tt17x.md) (category: Keyboard)

Scan the digital joystick for movement

[TT17](tt17.md "Previous routine")[KS3](ks3.md "Next routine")
