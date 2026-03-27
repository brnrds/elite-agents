---
title: "The me1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/me1.html"
---

[TT217](tt217.md "Previous routine")[MESS](mess.md "Next routine")
    
    
           Name: me1                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Erase an old in-flight message and display a new one
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-me1)
     Variations: See [code variations](../../related/compare/main/subroutine/me1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MESS](mess.md) calls me1
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
       X                    Must be set to 0
    
    
    
    
    .me1
    
     STX DLY                \ Set the message delay in DLY to 0, so any new
                            \ in-flight messages will be shown instantly
    
     PHA                    \ Store the new message token we want to print
    
     LDA #YELLOW            \ Switch to colour 1, which is yellow
     STA COL
    
     LDA MCH                \ Set A to the token number of the message that is
     JSR mes9               \ currently on-screen, and call mes9 to print it (which
                            \ will remove it from the screen, as printing is done
                            \ using EOR logic)
    
     PLA                    \ Restore the new message token
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Variable [DLY](../workspace/wp.md#dly) in workspace [WP](../workspace/wp.md)

In-flight message delay

[X]

Variable [MCH](../workspace/wp.md#mch) in workspace [WP](../workspace/wp.md)

The text token number of the in-flight message that is currently being shown, and which will be removed by the me2 routine when the counter in DLY reaches zero

[X]

Configuration variable [YELLOW](../../all/workspaces.md#yellow) = %00001111

Four mode 1 pixels of colour 1 (yellow)

[X]

Subroutine [mes9](mes9.md) (category: Flight)

Print a text token, possibly followed by " DESTROYED"

[TT217](tt217.md "Previous routine")[MESS](mess.md "Next routine")
