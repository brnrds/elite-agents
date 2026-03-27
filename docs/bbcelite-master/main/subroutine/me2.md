---
title: "The me2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/me2.html"
---

[msblob](msblob.md "Previous routine")[Ze](ze.md "Next routine")
    
    
           Name: me2                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Remove an in-flight message from the space view
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-me2)
     Variations: See [code variations](../../related/compare/main/subroutine/me2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) calls me2
    
    
    
    
    
    
    .me2
    
     LDA QQ11               \ If this is not the space view, jump down to clynsneed
     BNE clynsneed          \ to skip displaying the in-flight message
    
     LDA MCH                \ Fetch the token number of the current message into A
    
     JSR MESS               \ Call MESS to print the token, which will remove it
                            \ from the screen as printing uses EOR logic
    
     LDA #0                 \ Set the delay in DLY to 0, so any new in-flight
     STA DLY                \ messages will be shown instantly
    
     JMP me3                \ Jump back into the main spawning loop at me3
    
    .clynsneed
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
     JMP me3                \ Jump back into the main spawning loop at me3
    

[X]

Subroutine [CLYNS](clyns.md) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Variable [DLY](../workspace/wp.md#dly) in workspace [WP](../workspace/wp.md)

In-flight message delay

[X]

Variable [MCH](../workspace/wp.md#mch) in workspace [WP](../workspace/wp.md)

The text token number of the in-flight message that is currently being shown, and which will be removed by the me2 routine when the counter in DLY reaches zero

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Label [clynsneed](me2.md#clynsneed) is local to this routine

[X]

Entry point [me3](main_game_loop_part_2_of_6.md#me3) in subroutine [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) (category: Main loop)

Used by me2 to jump back into the main game loop after printing an in-flight message

[msblob](msblob.md "Previous routine")[Ze](ze.md "Next routine")
