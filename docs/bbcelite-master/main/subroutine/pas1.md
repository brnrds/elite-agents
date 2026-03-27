---
title: "The PAS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pas1.html"
---

[MT29](mt29.md "Previous routine")[PAUSE2](pause2.md "Next routine")
    
    
           Name: PAS1                                                    [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Display a rotating ship at space coordinates (0, 120, 256) and
                 scan the keyboard
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-pas1)
     Variations: See [code variations](../../related/compare/main/subroutine/pas1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](brief.md) calls PAS1
                 * [PAUSE](pause.md) calls PAS1
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    If a key is being pressed, X contains the ASCII code of
                            the key being pressed, otherwise it contains 0
    
       A                    Contains the same as X
    
    
    
    
    .PAS1
    
     LDA #120               \ Set y_lo = 120
     STA INWK+3
    
     LDA #0                 \ Set x_lo = 0
     STA INWK
    
     STA INWK+6             \ Set z_lo = 0
    
     LDA #2                 \ Set z_hi = 1, so (z_hi z_lo) = 256
     STA INWK+7
    
     JSR LL9                \ Draw the ship on screen
    
     JSR MVEIT              \ Call MVEIT to move and rotate the ship in space
    
     JMP RDKEY              \ Scan the keyboard for a key press and return the
                            \ ASCII code of the key pressed in X (or 0 for no key
                            \ press), returning from the subroutine using a tail
                            \ call
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Subroutine [MVEIT (Part 1 of 9)](mveit_part_1_of_9.md) (category: Moving)

Move current ship: Tidy the orientation vectors

[X]

Subroutine [RDKEY](rdkey.md) (category: Keyboard)

Scan the keyboard for key presses and update the key logger

[MT29](mt29.md "Previous routine")[PAUSE2](pause2.md "Next routine")
