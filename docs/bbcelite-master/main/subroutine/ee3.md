---
title: "The ee3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ee3.html"
---

[jmp](jmp.md "Previous routine")[pr6](pr6.md "Next routine")
    
    
           Name: ee3                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Print the hyperspace countdown in the top-left of the screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-ee3)
     Variations: See [code variations](../../related/compare/main/subroutine/ee3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](tt102.md) calls ee3
                 * [TTX66K](ttx66k.md) calls ee3
                 * [wW](ww.md) calls ee3
    
    
    
    
    
    * * *
    
    
     5 digits, left-padding with spaces for numbers with fewer than 3 digits (so
     numbers < 10000 are right-aligned), with no decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The number to print
    
    
    
    
    .ee3
    
     LDA #RED               \ Switch to colour 2, which is red in the space view
     STA COL
    
     LDA #1                 \ Move the text cursor to column 1
     STA XC
    
     STA YC                 \ Move the text cursor to row 1
    
     LDY #0                 \ Set Y = 0 for the high byte in pr6
    
     CLC                    \ Call TT11 to print X to 3 digits with no decimal point
     LDA #3                 \ and return from the subroutine using a tail call
     JMP TT11
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [RED](../../all/workspaces.md#red) = %11110000

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Subroutine [TT11](tt11.md) (category: Text)

Print a 16-bit number, left-padded to n digits, and optional point

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[jmp](jmp.md "Previous routine")[pr6](pr6.md "Next routine")
