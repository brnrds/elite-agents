---
title: "The CPIXK subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/cpixk.html"
---

[DOT](dot.md "Previous routine")[ECBLB2](ecblb2.md "Next routine")
    
    
           Name: CPIXK                                                   [Show more]
           Type: Subroutine
       Category: Drawing pixels
        Summary: Draw a single-height dash on the dashboard
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-cpixk)
     Variations: See [code variations](../../related/compare/main/subroutine/cpix2-cpixk.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOT](dot.md) calls CPIXK
                 * [SCAN](scan.md) calls CPIXK
    
    
    
    
    
    * * *
    
    
     Draw a single-height mode 2 dash (1 pixel high, 2 pixels wide).
    
    
    
    * * *
    
    
     Arguments:
    
       X1                   The screen pixel x-coordinate of the dash
    
       A                    The screen pixel y-coordinate of the dash
    
       COL                  The colour of the dash as a mode 2 character row byte
    
    
    
    * * *
    
    
     Returns:
    
       R                    The dash's right pixel byte
    
    
    
    
    .CPIXK
    
     STA Y1                 \ Store the y-coordinate in Y1
    
     TAY                    \ Store the y-coordinate in Y
    
     LDA ylookup,Y          \ Look up the page number of the character row that
     STA SC+1               \ contains the pixel with the y-coordinate in Y, and
                            \ store it in the high byte of SC(1 0) at SC+1
    
     LDA X1                 \ Each character block contains 8 pixel rows, so to get
     AND #%11111100         \ the address of the first byte in the character block
     ASL A                  \ that we need to draw into, as an offset from the start
                            \ of the row, we clear bits 0-1 and shift left to double
                            \ it (as each character row contains two pages of bytes,
                            \ or 512 bytes, which cover 256 pixels). This also
                            \ shifts bit 7 of X1 into the C flag
    
     STA SC                 \ Store the address of the character block in the low
                            \ byte of SC(1 0), so now SC(1 0) points to the
                            \ character block we need to draw into
    
     BCC P%+5               \ If the C flag is clear then skip the next two
                            \ instructions
    
     INC SC+1               \ The C flag is set, which means bit 7 of X1 was set
                            \ before the ASL above, so the x-coordinate is in the
                            \ right half of the screen (i.e. in the range 128-255).
                            \ Each row takes up two pages in memory, so the right
                            \ half is in the second page but SC+1 contains the value
                            \ we looked up from ylookup, which is the page number of
                            \ the first memory page for the row... so we need to
                            \ increment SC+1 to point to the correct page
    
     CLC                    \ Clear the C flag
    
     TYA                    \ Set Y to the y-coordinate mod 8, which will be the
     AND #7                 \ number of the pixel row we need to draw within the
     TAY                    \ character block
    
     LDA X1                 \ Copy bit 1 of X1 to bit 1 of X. X will now be either
     AND #%00000010         \ 0 or 2, and will be double the pixel number in the
     TAX                    \ character row for the left pixel in the dash (so 0
                            \ means the left pixel in the two-pixel character row,
                            \ while 2 means the right pixel)
    
     LDA CTWOS,X            \ Fetch a mode 2 one-pixel byte with the pixel position
     AND COL                \ at X/2, and AND with the colour byte so that pixel
                            \ takes on the colour we want to draw (i.e. A is acting
                            \ as a mask on the colour byte)
    
     EOR (SC),Y             \ Draw the pixel on-screen using EOR logic, so we can
     STA (SC),Y             \ remove it later without ruining the background that's
                            \ already on-screen
    
     LDA CTWOS+2,X          \ Fetch a mode 2 one-pixel byte with the pixel position
                            \ at (X+1)/2, so we can draw the right pixel of the dash
    
     BPL CP1                \ The CTWOS table has 2 extra rows at the end of it that
                            \ repeat the first values, %10101010, so if we have not
                            \ fetched that value, then the right pixel of the dash
                            \ is in the same character block as the left pixel, so
                            \ jump to CP1 to draw it
    
     LDA SC                 \ Otherwise the left pixel we drew was at the last
     ADC #8                 \ position of four in this character block, so we add
     STA SC                 \ 8 to the screen address to move onto the next block
                            \ along (as there are 8 bytes in a character block).
                            \ The C flag was cleared above, so this ADC is correct
    
     BCC P%+4               \ If the addition we just did overflowed, then increment
     INC SC+1               \ the high byte of SC(1 0), as this means we just moved
                            \ into the right half of the screen row
    
     LDA CTWOS+2,X          \ Re-fetch the mode 2 one-pixel byte, as we just
                            \ overwrote A (the byte will still be the fifth or sixth
                            \ byte from the table, which is correct as we want to
                            \ draw the leftmost pixel in the next character along as
                            \ the dash's right pixel)
    
    .CP1
    
     AND COL                \ Apply the colour mask to the pixel byte, as above
    
     STA R                  \ Store the dash's right pixel byte in R
    
     EOR (SC),Y             \ Draw the dash's right pixel according to the mask in
     STA (SC),Y             \ A, with the colour in COL, using EOR logic, just as
                            \ above
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Label [CP1](cpixk.md#cp1) is local to this routine

[X]

Variable [CTWOS](../variable/ctwos.md) (category: Drawing pixels)

Ready-made single-pixel character row bytes for mode 2

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [ylookup](../variable/ylookup.md) (category: Drawing pixels)

Lookup table for converting pixel y-coordinate to page number of screen address

[DOT](dot.md "Previous routine")[ECBLB2](ecblb2.md "Next routine")
