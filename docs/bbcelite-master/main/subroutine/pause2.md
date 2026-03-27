---
title: "The PAUSE2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pause2.html"
---

[PAS1](pas1.md "Previous routine")[GINF](ginf.md "Next routine")
    
    
           Name: PAUSE2                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Wait until a key is pressed, ignoring any existing key press
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-pause2)
     Variations: See [code variations](../../related/compare/main/subroutine/pause2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls PAUSE2
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    The ASCII code of the key that was pressed
    
    
    
    
    .PAUSE2
    
     JSR RDKEY              \ Scan the keyboard for a key press and return the ASCII
                            \ code of the key pressed in A and X (or 0 for no key
                            \ press)
    
     BNE PAUSE2             \ If a key was already being held down when we entered
                            \ this routine, keep looping back up to PAUSE2, until
                            \ the key is released
    
     JSR RDKEY              \ Any pre-existing key press is now gone, so we can
                            \ start scanning the keyboard again, returning the
                            \ ASCII code of the key pressed in X (or 0 for no key
                            \ press)
    
     BEQ PAUSE2             \ Keep looping up to PAUSE2 until a key is pressed
    
    .newyearseve
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [PAUSE2](pause2.md) (category: Keyboard)

Wait until a key is pressed, ignoring any existing key press

[X]

Subroutine [RDKEY](rdkey.md) (category: Keyboard)

Scan the keyboard for key presses and update the key logger

[PAS1](pas1.md "Previous routine")[GINF](ginf.md "Next routine")
