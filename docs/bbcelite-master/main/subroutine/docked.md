---
title: "The dockEd subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/docked.html"
---

[TT111](tt111.md "Previous routine")[hyp](hyp.md "Next routine")
    
    
           Name: dockEd                                                  [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Print a message to say there is no hyperspacing allowed inside the
                 station
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-docked)
     Variations: See [code variations](../../related/compare/main/subroutine/hy6-docked.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hyp](hyp.md) calls dockEd
    
    
    
    
    
    * * *
    
    
     Print "Docked" at the bottom of the screen to indicate we can't hyperspace
     when docked.
    
    
    
    
    .dockEd
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
     LDA #15                \ Move the text cursor to column 15 (the middle of the
     STA XC                 \ screen), setting A to 15 at the same time for the
                            \ following call to TT27
    
     LDA #RED               \ Switch to colour 2, which is magenta in the trade view
     STA COL                \ or red in the chart view
    
     LDA #205               \ Print extended token 205 ("DOCKED") and return from
     JMP DETOK              \ the subroutine using a tail call
    

[X]

Subroutine [CLYNS](clyns.md) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Configuration variable [RED](../../all/workspaces.md#red) = %11110000

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[TT111](tt111.md "Previous routine")[hyp](hyp.md "Next routine")
