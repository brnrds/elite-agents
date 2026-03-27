---
title: "The qv subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/qv.html"
---

[prx](prx.md "Previous routine")[hm](hm.md "Next routine")
    
    
           Name: qv                                                      [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Print a menu of the four space views, for buying lasers
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-qv)
     Variations: See [code variations](../../related/compare/main/subroutine/qv.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls qv
    
    
    
    
    
    * * *
    
    
     Print a menu in the bottom-middle of the screen, at row 16, column 12, that
     lists the four available space views, like this:
    
                     0 Front
                     1 Rear
                     2 Left
                     3 Right
    
     Also print a "View ?" prompt and ask for a view number. The menu is shown
     when we choose to buy a new laser in the Equip Ship screen.
    
    
    
    * * *
    
    
     Returns:
    
       X                    The chosen view number (0-3)
    
    
    
    
    .qv
    
     LDA tek                \ If the current system's tech level is less than 8,
     CMP #8                 \ skip the next two instructions, otherwise we clear the
     BCC P%+7               \ screen to prevent the view menu from clashing with the
                            \ longer equipment menu available in higher tech systems
    
     LDA #32                \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 32 (Equip
                            \ Ship screen)
    
     LDA #16                \ Move the text cursor to row 16, and at the same time
     TAY                    \ set Y to a counter going from 16 to 19 in the loop
     STA YC                 \ below
    
    .qv1
    
     LDA #12                \ Move the text cursor to column 12
     STA XC
    
     TYA                    \ Transfer the counter value from Y to A
    
     CLC                    \ Print ASCII character "0" - 16 + A, so as A goes from
     ADC #'0'-16            \ 16 to 19, this prints "0" through "3" followed by a
     JSR spc                \ space
    
     LDA YC                 \ Print recursive text token 80 + YC, so as YC goes from
     CLC                    \ 16 to 19, this prints "FRONT", "REAR", "LEFT" and
     ADC #80                \ "RIGHT"
     JSR TT27
    
     INC YC                 \ Move the text cursor down a row, and increment the
                            \ counter in YC at the same time
    
     LDY YC                 \ Update Y with the incremented counter in YC
    
     CPY #20                \ If Y < 20 then loop back up to qv1 to print the next
     BCC qv1                \ view in the menu
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
    .qv2
    
     LDA #175               \ Print recursive text token 15 ("VIEW ") followed by
     JSR prq                \ a question mark
    
     JSR TT217              \ Scan the keyboard until a key is pressed, and return
                            \ the key's ASCII code in A (and X)
    
     SEC                    \ Subtract ASCII "0" from the key pressed, to leave the
     SBC #'0'               \ numeric value of the key in A (if it was a number key)
    
     CMP #4                 \ If the number entered in A < 4, then it is a valid
     BCC qv3                \ view number, so jump down to qv3 as we are done
    
     JSR CLYNS              \ Otherwise we didn't get a valid view number, so clear
                            \ the bottom three text rows of the upper screen, and
                            \ move the text cursor to column 1 on row 21
    
     JMP qv2                \ Jump back to qv2 to try again
    
    .qv3
    
     TAX                    \ We have a valid view number, so transfer it to X
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [CLYNS](clyns.md) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Subroutine [TT217](tt217.md) (category: Keyboard)

Scan the keyboard until a key is pressed

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Subroutine [prq](prq.md) (category: Text)

Print a text token followed by a question mark

[X]

Label [qv1](qv.md#qv1) is local to this routine

[X]

Label [qv2](qv.md#qv2) is local to this routine

[X]

Label [qv3](qv.md#qv3) is local to this routine

[X]

Subroutine [spc](spc.md) (category: Text)

Print a text token followed by a space

[X]

Variable [tek](../workspace/wp.md#tek) in workspace [WP](../workspace/wp.md)

The current system's tech level (0-14)

[prx](prx.md "Previous routine")[hm](hm.md "Next routine")
