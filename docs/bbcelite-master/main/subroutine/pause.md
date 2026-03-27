---
title: "The PAUSE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pause.html"
---

[BRIS](bris.md "Previous routine")[MT23](mt23.md "Next routine")
    
    
           Name: PAUSE                                                   [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Display a rotating ship, waiting until a key is pressed, then
                 remove the ship from the screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-pause)
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls PAUSE
    
    
    
    
    
    
    .PAUSE
    
     JSR PAS1               \ Call PAS1 to display the rotating ship at space
                            \ coordinates (0, 112, 256) and scan the keyboard,
                            \ returning the internal key number in X (or 0 for no
                            \ key press)
    
     BNE PAUSE              \ If a key was already being held down when we entered
                            \ this routine, keep looping back up to PAUSE, until
                            \ the key is released
    
    .PAL1
    
     JSR PAS1               \ Call PAS1 to display the rotating ship at space
                            \ coordinates (0, 112, 256) and scan the keyboard,
                            \ returning the internal key number in X (or 0 for no
                            \ key press)
    
     BEQ PAL1               \ Keep looping up to PAL1 until a key is pressed
    
     LDA #0                 \ Set the ship's AI flag to 0 (no AI) so it doesn't get
     STA INWK+31            \ any ideas of its own
    
     LDA #1                 \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 1
    
     JSR LL9                \ Draw the ship on screen to redisplay it
    
                            \ Fall through into MT23 to move to row 10, switch to
                            \ white text, and switch to lower case when printing
                            \ extended tokens
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Label [PAL1](pause.md#pal1) is local to this routine

[X]

Subroutine [PAS1](pas1.md) (category: Missions)

Display a rotating ship at space coordinates (0, 120, 256) and scan the keyboard

[X]

Subroutine [PAUSE](pause.md) (category: Missions)

Display a rotating ship, waiting until a key is pressed, then remove the ship from the screen

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[BRIS](bris.md "Previous routine")[MT23](mt23.md "Next routine")
