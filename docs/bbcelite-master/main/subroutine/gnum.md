---
title: "The gnum subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/gnum.html"
---

[TT219](tt219.md "Previous routine")[NWDAV4](nwdav4.md "Next routine")
    
    
           Name: gnum                                                    [Show more]
           Type: Subroutine
       Category: Market
        Summary: Get a number from the keyboard
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-gnum)
     Variations: See [code variations](../../related/compare/main/subroutine/gnum.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls gnum
                 * [TT210](tt210.md) calls gnum
                 * [TT219](tt219.md) calls gnum
                 * [OUTK](outk.md) calls via OUT
    
    
    
    
    
    * * *
    
    
     Get a number from the keyboard, up to the maximum number in QQ25, for the
     buying and selling of cargo and equipment.
    
     Pressing "Y" will return the maximum number (i.e. buy/sell all items), while
     pressing "N" will abort the sale and return a 0.
    
     Pressing a key with an ASCII code less than ASCII "0" will return a 0 in A (so
     that includes pressing Space or Return), while pressing a key with an ASCII
     code greater than ASCII "9" will jump to the Inventory screen (so that
     includes all letters and most punctuation).
    
    
    
    * * *
    
    
     Arguments:
    
       QQ25                 The maximum number allowed
    
    
    
    * * *
    
    
     Returns:
    
       A                    The number entered
    
       R                    Also contains the number entered
    
       C flag               Set if the number is too large (> QQ25), clear otherwise
    
    
    
    * * *
    
    
     Other entry points:
    
       OUT                  The OUTK routine jumps back here after printing the key
                            that was just pressed
    
    
    
    
    .gnum
    
     LDA #MAGENTA           \ Switch to colour 2, which is magenta in the trade view
     STA COL
    
     LDX #0                 \ We will build the number entered in R, so initialise
     STX R                  \ it with 0
    
     LDX #12                \ We will check for up to 12 key presses, so set a
     STX T1                 \ counter in T1
    
    .TT223
    
     JSR TT217              \ Scan the keyboard until a key is pressed, and return
                            \ the key's ASCII code in A (and X)
    
     LDX R                  \ If R is non-zero then skip to NWDAV2, as we are
     BNE NWDAV2             \ already building a number
    
     CMP #'Y'               \ If "Y" was pressed, jump to NWDAV1 to return the
     BEQ NWDAV1             \ maximum number allowed (i.e. buy/sell the whole stock)
    
     CMP #'N'               \ If "N" was pressed, jump to NWDAV3 to return from the
     BEQ NWDAV3             \ subroutine with a result of 0 (i.e. abort transaction)
    
    .NWDAV2
    
     STA Q                  \ Store the key pressed in Q
    
     SEC                    \ Subtract ASCII "0" from the key pressed, to leave the
     SBC #'0'               \ numeric value of the key in A (if it was a number key)
    
     BCC OUT                \ If A < 0, jump to OUT to load the current number and
                            \ return from the subroutine, as the key pressed was
                            \ RETURN (or some other character with a value less than
                            \ ASCII "0")
    
     CMP #10                \ If A >= 10, jump to BAY2 to display the Inventory
     BCS BAY2               \ screen, as the key pressed was a letter or other
                            \ non-digit and is greater than ASCII "9"
    
     STA S                  \ Store the numeric value of the key pressed in S
    
     LDA R                  \ Fetch the result so far into A
    
    \BEQ P%+4               \ This instruction is commented out in the original
                            \ source, and has a comment "tribs"
    
     CMP #26                \ If A >= 26, where A is the number entered so far, then
     BCS OUTK               \ adding a further digit will make it bigger than 256,
                            \ so jump to OUTK to print the key that was just pressed
                            \ before jumping to OUT below with the C flag still set
    
     ASL A                  \ Set A = (A * 2) + (A * 8) = A * 10
     STA T
     ASL A
     ASL A
     ADC T
    
     ADC S                  \ Add the pressed digit to A
    
     BCS OUTK               \ If the addition overflowed, then jump to OUTK to print
                            \ the key that was just pressed before jumping to OUT
                            \ below with the C flag still set
    
     STA R                  \ Otherwise store the result in R, so R now contains
                            \ its previous value with the new key press tacked onto
                            \ the end
    
     CMP QQ25               \ If the result in R = the maximum allowed in QQ25, jump
     BEQ TT226              \ to TT226 to print the key press and keep looping (the
                            \ BEQ is needed because the BCS below would jump to OUT
                            \ if R >= QQ25, which we don't want)
    
     BCS OUTK               \ If the result in R > QQ25, jump to OUTK to print
                            \ the key that was just pressed before jumping to OUT
                            \ below with the C flag still set
    
    .TT226
    
     LDA Q                  \ Print the character in Q (i.e. the key that was
     JSR TT26               \ pressed, as we stored the ASCII value in Q earlier)
    
     DEC T1                 \ Decrement the loop counter
    
     BNE TT223              \ Loop back to TT223 until we have checked for 12 digits
    
    .OUT
    
     LDA #CYAN              \ Switch to colour 3, which is white in the trade view
     STA COL
    
     LDA R                  \ Set A to the result we have been building in R
    
     RTS                    \ Return from the subroutine
    
    .NWDAV1
    
                            \ If we get here then "Y" was pressed, so we return the
                            \ maximum number allowed, which is in QQ25
    
     JSR TT26               \ Print the character for the key that was pressed
    
     LDA QQ25               \ Set R = QQ25, so we return the maximum value allowed
     STA R
    
     JMP OUT                \ Jump to OUT to return from the subroutine
    
    .NWDAV3
    
                            \ If we get here then "N" was pressed, so we return 0
    
     JSR TT26               \ Print the character for the key that was pressed
    
     STZ R                  \ Set R = 0, so we return 0
    
     JMP OUT                \ Jump to OUT to return from the subroutine
    

[X]

Entry point [BAY2](tt219.md#bay2) in subroutine [TT219](tt219.md) (category: Market)

Jump into the main loop at FRCE, setting the key "pressed" to red key f9 (so we show the Inventory screen)

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Configuration variable [MAGENTA](../../all/workspaces.md#magenta) = RED

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Label [NWDAV1](gnum.md#nwdav1) is local to this routine

[X]

Label [NWDAV2](gnum.md#nwdav2) is local to this routine

[X]

Label [NWDAV3](gnum.md#nwdav3) is local to this routine

[X]

Entry point [OUT](gnum.md#out) in subroutine [gnum](gnum.md) (category: Market)

The OUTK routine jumps back here after printing the key that was just pressed

[X]

Subroutine [OUTK](outk.md) (category: Text)

Print the character in Q before returning to gnum

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [QQ25](../workspace/wp.md#qq25) in workspace [WP](../workspace/wp.md)

Temporary storage, used to store the current market item's availability in routine TT151

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [TT217](tt217.md) (category: Keyboard)

Scan the keyboard until a key is pressed

[X]

Label [TT223](gnum.md#tt223) is local to this routine

[X]

Label [TT226](gnum.md#tt226) is local to this routine

[X]

Subroutine [TT26](tt26.md) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[TT219](tt219.md "Previous routine")[NWDAV4](nwdav4.md "Next routine")
