---
title: "The CHPR subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/chpr.html"
---

[TT67K](tt67k.md "Previous routine")[TTX66](ttx66.md "Next routine")
    
    
           Name: CHPR                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a character at the text cursor by poking into screen memory
      Deep dive: [Drawing text](https://elite.bbcelite.com/deep_dives/drawing_text.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-chpr)
     Variations: See [code variations](../../related/compare/main/subroutine/tt26-chpr.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BELL](bell.md) calls CHPR
                 * [COLD](cold.md) calls CHPR
                 * [GTDRV](gtdrv.md) calls CHPR
                 * [MT26](mt26.md) calls CHPR
                 * [NEWBRK](newbrk.md) calls CHPR
                 * [NUMBOR](numbor.md) calls CHPR
                 * [TT26](tt26.md) calls CHPR
                 * [cls](cls.md) calls via RR4
    
    
    
    
    
    * * *
    
    
     Print a character at the text cursor (XC, YC), do a beep, print a newline,
     or delete left (backspace).
    
     Calls to OSWRCH will end up here when A is not in the range 128-147, as those
     are reserved for the special jump table OSWRCH commands.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be printed. Can be one of the
                            following:
    
                              * 7 (beep)
    
                              * 10 (line feed)
    
                              * 11 (clear the top part of the screen and draw a
                                border)
    
                              * 12-13 (carriage return)
    
                              * 32-95 (ASCII capital letters, numbers and
                                punctuation)
    
                              * 127 (delete the character to the left of the text
                                cursor and move the cursor to the left)
    
       XC                   Contains the text column to print at (the x-coordinate)
    
       YC                   Contains the line number to print on (the y-coordinate)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       X                    X is preserved
    
       Y                    Y is preserved
    
       C flag               The C flag is cleared
    
    
    
    * * *
    
    
     Other entry points:
    
       RR4                  Restore the registers and return from the subroutine
    
    
    
    
    .CHPR
    
     STA K3                 \ Store the A, X and Y registers, so we can restore
     PHY                    \ them at the end (so they don't get changed by this
     PHX                    \ routine)
    
     LDY QQ17               \ Load the QQ17 flag, which contains the text printing
                            \ flags
    
     CPY #255               \ If QQ17 = 255 then printing is disabled, so jump to
     BEQ RR4S               \ RR4 (via the JMP in RR4S) to restore the registers
                            \ and return from the subroutine using a tail call
    
    IF _COMPACT
    
     TAY                    \ Copy the character to be printed from A into Y
    
     BEQ RR4S               \ If the character to be printed is 0 or >= 128, jump to
     BMI RR4S               \ RR4S (via the JMP in RR4S) to restore the registers
                            \ and return from the subroutine using a tail call
    
    .RRNEW
    
    ENDIF
    
     LDY #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     TAY                    \ Set Y = the character to be printed
    
    IF _SNG47
    
     BEQ RR4S               \ If the character is zero, which is typically a string
                            \ terminator character, jump down to RR4 (via the JMP in
                            \ RR4S) to restore the registers and return from the
                            \ subroutine using a tail call
    
     BMI RR4S               \ If A > 127 then there is nothing to print, so jump to
                            \ RR4 (via the JMP in RR4S) to restore the registers and
                            \ return from the subroutine
    
    ELIF _COMPACT
    
     LDX CATF               \ If CATF <> 0, skip the following two instructions, as
     BNE P%+6               \ we are printing a disc catalogue and we don't want any
                            \ control characters lurking in the catalogue to trigger
                            \ the screen clearing routine
    
    ENDIF
    
     CMP #11                \ If this is control code 11 (clear screen), jump to cls
     BEQ cls                \ to clear the top part of the screen, draw a white
                            \ border and return from the subroutine via RR4
    
     CMP #7                 \ If this is not control code 7 (beep), skip the next
     BNE P%+5               \ instruction
    
     JMP R5                 \ This is control code 7 (beep), so jump to R5 to make
                            \ a beep and return from the subroutine via RR4
    
     CMP #32                \ If this is an ASCII character (A >= 32), jump to RR1
     BCS RR1                \ below, which will print the character, restore the
                            \ registers and return from the subroutine
    
     CMP #10                \ If this is control code 10 (line feed) then jump to
     BEQ RRX1               \ RRX1, which will move down a line, restore the
                            \ registers and return from the subroutine
    
     LDX #1                 \ If we get here, then this is control code 12 or 13,
     STX XC                 \ both of which are used. This code prints a newline,
                            \ which we can achieve by moving the text cursor
                            \ to the start of the line (carriage return) and down
                            \ one line (line feed). These two lines do the first
                            \ bit by setting XC = 1, and we then fall through into
                            \ the line feed routine that's used by control code 10
    
    .RRX1
    
     CMP #13                \ If this is control code 13 (carriage return) then jump
     BEQ RR4S               \ to RR4 (via the JMP in RR4S) to restore the registers
                            \ and return from the subroutine using a tail call
    
     INC YC                 \ Increment the text cursor y-coordinate to move it
                            \ down one row
    
    .RR4S
    
     JMP RR4                \ Jump to RR4 to restore the registers and return from
                            \ the subroutine using a tail call
    
    .RR1
    
                            \ If we get here, then the character to print is an
                            \ ASCII character in the range 32-95. The quickest way
                            \ to display text on-screen is to poke the character
                            \ pixel by pixel, directly into screen memory, so
                            \ that's what the rest of this routine does
                            \
                            \ The first step, then, is to get hold of the bitmap
                            \ definition for the character we want to draw on the
                            \ screen (i.e. we need the pixel shape of this
                            \ character). The MOS ROM contains bitmap definitions
                            \ of the system's ASCII characters, starting from &C000
                            \ for space (ASCII 32) and ending with the £ symbol
                            \ (ASCII 126)
                            \
                            \ There are definitions for 32 characters in each of the
                            \ three pages of MOS memory, as each definition takes up
                            \ 8 bytes (8 rows of 8 pixels) and 32 * 8 = 256 bytes =
                            \ 1 page. So:
                            \
                            \   ASCII 32-63  are defined in &C000-&C0FF (page 0)
                            \   ASCII 64-95  are defined in &C100-&C1FF (page 1)
                            \   ASCII 96-126 are defined in &C200-&C2F0 (page 2)
                            \
                            \ The following code reads the relevant character
                            \ those values into the correct position in screen
                            \ memory, thus printing the character on-screen
                            \
                            \ It's a long way from 10 PRINT "Hello world!":GOTO 10
    
                            \ Now we want to set X to point to the relevant page
    
                            \ The following logic is easier to follow if we look
                            \ at the three character number ranges in binary:
                            \
                            \   Bit #  76543210
                            \
                            \   32  = %00100000     Page 0 of bitmap definitions
                            \   63  = %00111111
                            \
                            \   64  = %01000000     Page 1 of bitmap definitions
                            \   95  = %01011111
                            \
                            \   96  = %01100000     Page 2 of bitmap definitions
                            \   125 = %01111101
                            \
                            \ We'll refer to this below
    
     LDX #(FONT%-1)         \ Set X to point to the page before the first font page,
                            \ which is FONT% - 1
    
     ASL A                  \ If bit 6 of the character is clear (A is 32-63)
     ASL A                  \ then skip the following instruction
     BCC P%+4
    
     LDX #(FONT%+1)         \ A is 64-126, so set X to point to page FONT% + 1
    
     ASL A                  \ If bit 5 of the character is clear (A is 64-95)
     BCC P%+3               \ then skip the following instruction
    
     INX                    \ Increment X
                            \
                            \ In other words, X points to the relevant page. But
                            \ what about the value of A? That gets shifted to the
                            \ left three times during the above code, which
                            \ multiplies the number by 8 but also drops bits 7, 6
                            \ and 5 in the process. Look at the above binary
                            \ figures and you can see that if we cleared bits 5-7,
                            \ then that would change 32-53 to 0-31... but it would
                            \ do exactly the same to 64-95 and 96-125. And because
                            \ we also multiply this figure by 8, A now points to
                            \ the start of the character's definition within its
                            \ page (because there are 8 bytes per character
                            \ definition)
                            \
                            \ Or, to put it another way, X contains the high byte
                            \ (the page) of the address of the definition that we
                            \ want, while A contains the low byte (the offset into
                            \ the page) of the address
    
     STA P                  \ Store the address of this character's definition in
     STX P+1                \ P(1 0)
    
     LDA XC                 \ Fetch XC, the x-coordinate (column) of the text cursor
                            \ into A
    
     LDX CATF               \ If CATF = 0, jump to RR5, otherwise we are printing a
     BEQ RR5                \ disc catalogue
    
    IF _SNG47
    
     CPY #' '               \ If the character we want to print in Y is a space,
     BNE RR5                \ jump to RR5
    
                            \ If we get here, then CATF is non-zero, so we are
                            \ printing a disc catalogue and we are not printing a
                            \ space, so we drop column 17 from the output so the
                            \ catalogue will fit on-screen (column 17 is a blank
                            \ column in the middle of the catalogue, between the
                            \ two lists of filenames, so it can be dropped without
                            \ affecting the layout). Without this, the catalogue
                            \ would be one character too wide for the square screen
                            \ mode (it's 34 characters wide, while the screen mode
                            \ is only 33 characters across)
    
     CMP #17                \ If A = 17, i.e. the text cursor is in column 17, jump
     BEQ RR4                \ to RR4 to restore the registers and return from the
                            \ subroutine, thus omitting this column
    
    ELIF _COMPACT
    
     CMP #21                \ If A < 21, i.e. the text cursor is in column 0-20,
     BCC RR5                \ jump to RR5 to skip the following
    
                            \ If we get here, then CATF is non-zero, so we are
                            \ printing a disc catalogue and we have reached column
                            \ 21, so we move to the start of the next line so the
                            \ catalogue line-wraps to fit within the bounds of the
                            \ screen
    
     INC YC                 \ More the text cursor down a line
    
     LDA #1                 \ Move the text cursor to column 1
     STA XC
    
    ENDIF
    
    .RR5
    
     ASL A                  \ Multiply A by 8, and store in SC, so we now have:
     ASL A                  \
     ASL A                  \   SC = XC * 8
     STA SC
    
     LDA YC                 \ Fetch YC, the y-coordinate (row) of the text cursor
    
     CPY #127               \ If the character number (which is in Y) <> 127, then
     BNE RR2                \ skip to RR2 to print that character, otherwise this is
                            \ the delete character, so continue on
    
     DEC XC                 \ We want to delete the character to the left of the
                            \ text cursor and move the cursor back one, so let's
                            \ do that by decrementing YC. Note that this doesn't
                            \ have anything to do with the actual deletion below,
                            \ we're just updating the cursor so it's in the right
                            \ position following the deletion
    
     ASL A                  \ A contains YC (from above), so this sets A = YC * 2
    
     ASL SC                 \ Double the low byte of SC(1 0), catching bit 7 in the
                            \ C flag. As each character is 8 pixels wide, and the
                            \ special screen mode Elite uses for the top part of the
                            \ screen is 256 pixels across with two bits per pixel,
                            \ this value is not only double the screen address
                            \ offset of the text cursor from the left side of the
                            \ screen, it's also the least significant byte of the
                            \ screen address where we want to print this character,
                            \ as each row of on-screen pixels corresponds to two
                            \ pages. To put this more explicitly, the screen starts
                            \ at &4000, so the text rows are stored in screen
                            \ memory like this:
                            \
                            \   Row 1: &4000 - &41FF    YC = 1, XC = 0 to 31
                            \   Row 2: &4200 - &43FF    YC = 2, XC = 0 to 31
                            \   Row 3: &4400 - &45FF    YC = 3, XC = 0 to 31
                            \
                            \ and so on
    
     ADC #&3F               \ Set X = A
     TAX                    \       = A + &3F + C
                            \       = YC * 2 + &3F + C
    
                            \ Because YC starts at 0 for the first text row, this
                            \ means that X will be &3F for row 0, &41 for row 1 and
                            \ so on. In other words, X is now set to the page number
                            \ for the row before the one containing the text cursor,
                            \ and given that we set SC above to point to the offset
                            \ in memory of the text cursor within the row's page,
                            \ this means that (X SC) now points to the character
                            \ above the text cursor
    
     LDY #&F0               \ Set Y = &F0, so the following call to ZES2 will count
                            \ Y upwards from &F0 to &FF
    
     JSR ZES2               \ Call ZES2, which zero-fills from address (X SC) + Y to
                            \ (X SC) + &FF. (X SC) points to the character above the
                            \ text cursor, and adding &FF to this would point to the
                            \ cursor, so adding &F0 points to the character before
                            \ the cursor, which is the one we want to delete. So
                            \ this call zero-fills the character to the left of the
                            \ cursor, which erases it from the screen
    
     BEQ RR4                \ We are done deleting, so restore the registers and
                            \ return from the subroutine (this BNE is effectively
                            \ a JMP as ZES2 always returns with the Z flag set)
    
    .RR2
    
                            \ Now to actually print the character
    
     INC XC                 \ Once we print the character, we want to move the text
                            \ cursor to the right, so we do this by incrementing
                            \ XC. Note that this doesn't have anything to do
                            \ with the actual printing below, we're just updating
                            \ the cursor so it's in the right position following
                            \ the print
    
     CMP #24                \ If the text cursor is on the screen (i.e. YC < 24, so
     BCC RR3                \ we are on rows 0-23), then jump to RR3 to print the
                            \ character
    
    IF _SNG47
    
     JSR TTX66              \ Otherwise we are off the bottom of the screen, so
                            \ clear the screen and draw a border box
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
    ELIF _COMPACT
    
     LDA CATF               \ If CATF = 0, skip the next two instructions, as we are
     BEQ P%+7               \ not printing a disc catalogue
    
     JSR RETURN             \ We have just printed the disc catalogue, so wait until
     BPL P%-3               \ RETURN is pressed, looping indefinitely until it gets
                            \ tapped
    
     JSR TTX66              \ Call TTX66 to clear the screen
    
    ENDIF
    
     LDA #1                 \ Move the text cursor to column 1, row 1
     STA XC
     STA YC
    
     LDA K3                 \ Set A to the character to be printed, though again
                            \ this has no effect, as the following call to RR4 does
                            \ the exact same thing
    
    IF _SNG47
    
     JMP RR4                \ And restore the registers and return from the
                            \ subroutine
    
    ELIF _COMPACT
    
     JMP RRNEW              \ Jump back to RRNEW to print the character
    
    ENDIF
    
    .RR3
    
                            \ A contains the value of YC - the screen row where we
                            \ want to print this character - so now we need to
                            \ convert this into a screen address, so we can poke
                            \ the character data to the right place in screen
                            \ memory
    
     ASL A                  \ Set A = 2 * A
                            \       = 2 * YC
    
     ASL SC                 \ Back in RR5 we set SC = XC * 8, so this does the
                            \ following:
                            \
                            \   SC = SC * 2
                            \      = XC * 16
                            \
                            \ so SC contains the low byte of the screen address we
                            \ want to poke the character into, as each text
                            \ character is 8 pixels wide, and there are four pixels
                            \ per byte, so the offset within the row's 512 bytes
                            \ is XC * 8 pixels * 2 bytes for each 8 pixels = XC * 16
    
     ADC #&40               \ Set A = &40 + A
                            \       = &40 + (2 * YC)
                            \
                            \ so A contains the high byte of the screen address we
                            \ want to poke the character into, as screen memory
                            \ starts at &4000 (page &40) and each screen row takes
                            \ up 2 pages (512 bytes)
    
    .RREN
    
     STA SC+1               \ Store the page number of the destination screen
                            \ location in SC+1, so SC now points to the full screen
                            \ location where this character should go
    
     LDA SC                 \ Set P(3 2) = SC(1 0) + 8
     CLC                    \
     ADC #8                 \ starting with the low bytes
     STA P+2
    
     LDA SC+1               \ And then adding the high bytes, so P(3 2) points to
     STA P+3                \ the character block after the one pointed to by
                            \ SC(1 0)
    
     LDY #7                 \ We want to print the 8 bytes of character data to the
                            \ screen (one byte per row), so set up a counter in Y
                            \ to count these bytes
    
    .RRL1
    
                            \ We print the character's eight-pixel row in two parts,
                            \ starting with the first four pixels (one byte of
                            \ screen memory), and then the second four (a second
                            \ byte of screen memory)
    
     LDA (P),Y              \ The character definition is at P(1 0) - we set this up
                            \ above - so load the Y-th byte from P(1 0), which will
                            \ contain the bitmap for the Y-th row of the character
    
     AND #%11110000         \ Extract the high nibble of the character definition
                            \ byte, so the first four pixels on this row of the
                            \ character are in the first nibble, i.e. xxxx 0000
                            \ where xxxx is the pattern of those four pixels in the
                            \ character
    
     STA W                  \ Set A = (A >> 4) OR A
     LSR A                  \
     LSR A                  \ which duplicates the high nibble into the low nibble
     LSR A                  \ to give xxxx xxxx
     LSR A
     ORA W
    
     AND COL                \ AND with the colour byte so that the pixels take on
                            \ the colour we want to draw (i.e. A is acting as a mask
                            \ on the colour byte)
    
     EOR (SC),Y             \ If we EOR this value with the existing screen
                            \ contents, then it's reversible (so reprinting the
                            \ same character in the same place will revert the
                            \ screen to what it looked like before we printed
                            \ anything); this means that printing a white pixel
                            \ onto a white background results in a black pixel, but
                            \ that's a small price to pay for easily erasable text
    
     STA (SC),Y             \ Store the Y-th byte at the screen address for this
                            \ character location
    
                            \ We now repeat the process for the second batch of four
                            \ pixels in this character row
    
     LDA (P),Y              \ Fetch the bitmap for the Y-th row of the character
                            \ again
    
     AND #%00001111         \ This time we extract the low nibble of the character
                            \ definition, to get 0000 xxxx
    
     STA W                  \ Set A = (A << 4) OR A
     ASL A                  \
     ASL A                  \ which duplicates the low nibble into the high nibble
     ASL A                  \ to give xxxx xxxx
     ASL A
     ORA W
    
     AND COL                \ AND with the colour byte so that the pixels take on
                            \ the colour we want to draw (i.e. A is acting as a mask
                            \ on the colour byte)
    
     EOR (P+2),Y            \ EOR this value with the existing screen contents of
                            \ P(3 2), which is equal to SC(1 0) + 8, the next four
                            \ pixels along from the first four pixels we just
                            \ plotted in SC(1 0)
    
     STA (P+2),Y            \ Store the Y-th byte at the screen address for this
                            \ character location
    
     DEY                    \ Decrement the loop counter
    
     BPL RRL1               \ Loop back for the next byte to print to the screen
    
    .RR4
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     PLX                    \ We're done printing, so restore the values of the
     PLY                    \ A, X and Y registers that we saved above and clear the
     LDA K3                 \ C flag, so everything is back to how it was
     CLC
    
     RTS                    \ Return from the subroutine
    
    .R5
    
     JSR BEEP               \ Call the BEEP subroutine to make a short, high beep
    
     JMP RR4                \ Jump to RR4 to restore the registers and return from
                            \ the subroutine using a tail call
    

[X]

Subroutine [BEEP](beep.md) (category: Sound)

Make a short, high beep

[X]

Variable [CATF](../workspace/up.md#catf) in workspace [UP](../workspace/up.md)

The disc catalogue flag

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Variable [FONT%](../variable/font_per_cent.md) (category: Text)

A copy of the character definition bitmap table from the MOS ROM

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [QQ17](../workspace/zp.md#qq17) in workspace [ZP](../workspace/zp.md)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Label [R5](chpr.md#r5) is local to this routine

[X]

Subroutine [RETURN](return.md) (category: Keyboard)

Scan the keyboard to see if RETURN is currently pressed

[X]

Label [RR1](chpr.md#rr1) is local to this routine

[X]

Label [RR2](chpr.md#rr2) is local to this routine

[X]

Label [RR3](chpr.md#rr3) is local to this routine

[X]

Entry point [RR4](chpr.md#rr4) in subroutine [CHPR](chpr.md) (category: Text)

Restore the registers and return from the subroutine

[X]

Label [RR4S](chpr.md#rr4s) is local to this routine

[X]

Label [RR5](chpr.md#rr5) is local to this routine

[X]

Label [RRL1](chpr.md#rrl1) is local to this routine

[X]

Label [RRNEW](chpr.md#rrnew) is local to this routine

[X]

Label [RRX1](chpr.md#rrx1) is local to this routine

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Subroutine [TTX66](ttx66.md) (category: Drawing the screen)

Clear the top part of the screen and draw a border box

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [W](../workspace/zp.md#w) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Subroutine [ZES2](zes2.md) (category: Utility routines)

Zero-fill a specific page

[X]

Subroutine [cls](cls.md) (category: Drawing the screen)

Clear the top part of the screen and draw a border box

[TT67K](tt67k.md "Previous routine")[TTX66](ttx66.md "Next routine")
