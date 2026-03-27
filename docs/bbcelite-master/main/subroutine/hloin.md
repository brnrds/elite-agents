---
title: "The HLOIN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hloin.html"
---

[LOINQ (Part 7 of 7)](loinq_part_7_of_7.md "Previous routine")[TWFL](../variable/twfl.md "Next routine")
    
    
           Name: HLOIN                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a horizontal line from (X1, Y1) to (X2, Y1)
      Deep dive: [Drawing colour pixels on the BBC Micro](https://elite.bbcelite.com/deep_dives/drawing_colour_pixels_in_mode_5.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-hloin)
     Variations: See [code variations](../../related/compare/main/subroutine/hloin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HLOIN2](hloin2.md) calls HLOIN
                 * [SUN (Part 3 of 4)](sun_part_3_of_4.md) calls HLOIN
                 * [LOINQ (Part 1 of 7)](loinq_part_1_of_7.md) calls via HLOIN3
                 * [NLIN2](nlin2.md) calls via HLOIN3
                 * [TT15](tt15.md) calls via HLOIN3
    
    
    
    
    
    * * *
    
    
     This routine draws a horizontal orange line in the space view.
    
     We do not draw a pixel at the right end of the line.
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       HLOIN3               Draw a line from (X, Y1) to (X2, Y1) in the colour given
                            in A
    
    
    
    
    .HLOIN
    
     LDA Y1                 \ Set A = Y1, the pixel y-coordinate of the line
    
     AND #3                 \ Set A to the correct order of red/yellow pixels to
     TAX                    \ make this line an orange colour (by using bits 0-1 of
     LDA orange,X           \ the pixel y-coordinate as the index into the orange
                            \ lookup table)
    
     STA COL                \ Store the correct orange colour in COL
    
    .HLOIN3
    
     STY YSAV               \ Store Y into YSAV, so we can preserve it across the
                            \ call to this subroutine
    
     LDY #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDX X1                 \ Set X = X1
    
     CPX X2                 \ If X1 = X2 then the start and end points are the same,
     BEQ HL6                \ so return from the subroutine (as HL6 contains an RTS)
    
     BCC HL5                \ If X1 < X2, jump to HL5 to skip the following code, as
                            \ (X1, Y1) is already the left point
    
     LDA X2                 \ Swap the values of X1 and X2, so we know that (X1, Y1)
     STA X1                 \ is on the left and (X2, Y1) is on the right
     STX X2
    
     TAX                    \ Set X = X1
    
    .HL5
    
     DEC X2                 \ Decrement X2 so we do not draw a pixel at the end
                            \ point
    
     LDY Y1                 \ Look up the page number of the character row that
     LDA ylookup,Y          \ contains the pixel with the y-coordinate in Y1, and
     STA SC+1               \ store it in SC+1, so the high byte of SC is set
                            \ correctly for drawing our line
    
     TYA                    \ Set A = Y1 mod 8, which is the pixel row within the
     AND #7                 \ character block at which we want to draw our line (as
                            \ each character block has 8 rows)
    
     STA SC                 \ Store this value in SC, so SC(1 0) now contains the
                            \ screen address of the far left end (x-coordinate = 0)
                            \ of the horizontal pixel row that we want to draw our
                            \ horizontal line on
    
     TXA                    \ Set Y = 2 * bits 2-6 of X1
     AND #%11111100         \
     ASL A                  \ and shift bit 7 of X1 into the C flag
     TAY
    
     BCC P%+4               \ If bit 7 of X1 was set, so X1 > 127, increment the
     INC SC+1               \ high byte of SC(1 0) to point to the second page on
                            \ this screen row, as this page contains the right half
                            \ of the row
    
    .HL1
    
     TXA                    \ Set T = bits 2-7 of X1, which will contain the
     AND #%11111100         \ character number of the start of the line * 4
     STA T
    
     LDA X2                 \ Set A = bits 2-7 of X2, which will contain the
     AND #%11111100         \ character number of the end of the line * 4
    
     SEC                    \ Set A = A - T, which will contain the number of
     SBC T                  \ character blocks we need to fill - 1 * 4
    
     BEQ HL2                \ If A = 0 then the start and end character blocks are
                            \ the same, so the whole line fits within one block, so
                            \ jump down to HL2 to draw the line
    
                            \ Otherwise the line spans multiple characters, so we
                            \ start with the left character, then do any characters
                            \ in the middle, and finish with the right character
    
     LSR A                  \ Set R = A / 4, so R now contains the number of
     LSR A                  \ character blocks we need to fill - 1
     STA R
    
     LDA X1                 \ Set X = X1 mod 4, which is the horizontal pixel number
     AND #3                 \ within the character block where the line starts (as
     TAX                    \ each pixel line in the character block is 4 pixels
                            \ wide)
    
     LDA TWFR,X             \ Fetch a ready-made byte with X pixels filled in at the
                            \ right end of the byte (so the filled pixels start at
                            \ point X and go all the way to the end of the byte),
                            \ which is the shape we want for the left end of the
                            \ line
    
     AND COL                \ Apply the pixel mask in A to the four-pixel block of
                            \ coloured pixels in COL, so we now know which bits to
                            \ set in screen memory to paint the relevant pixels in
                            \ the required colour
    
     EOR (SC),Y             \ Store this into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen,
                            \ so we have now drawn the line's left cap
    
     TYA                    \ Set Y = Y + 8 so (SC),Y points to the next character
     ADC #8                 \ block along, on the same pixel row as before
     TAY
    
     BCS HL7                \ If the above addition overflowed, then we have just
                            \ crossed over from the left half of the screen into the
                            \ right half, so call HL7 to increment the high byte in
                            \ SC+1 so that SC(1 0) points to the page in screen
                            \ memory for the right half of the screen row. HL7 also
                            \ clears the C flag and jumps back to HL8, so this acts
                            \ like a conditional JSR instruction
    
    .HL8
    
     LDX R                  \ Fetch the number of character blocks we need to fill
                            \ from R
    
     DEX                    \ Decrement the number of character blocks in X
    
     BEQ HL3                \ If X = 0 then we only have the last block to do (i.e.
                            \ the right cap), so jump down to HL3 to draw it
    
     CLC                    \ Otherwise clear the C flag so we can do some additions
                            \ while we draw the character blocks with full-width
                            \ lines in them
    
    .HLL1
    
     LDA COL                \ Store a full-width four-pixel horizontal line of
     EOR (SC),Y             \ colour COL in SC(1 0) so that it draws the line
     STA (SC),Y             \ on-screen, using EOR logic so it merges with whatever
                            \ is already on-screen
    
     TYA                    \ Set Y = Y + 8 so (SC),Y points to the next character
     ADC #8                 \ block along, on the same pixel row as before
     TAY
    
     BCS HL9                \ If the above addition overflowed, then we have just
                            \ crossed over from the left half of the screen into the
                            \ right half, so call HL9 to increment the high byte in
                            \ SC+1 so that SC(1 0) points to the page in screen
                            \ memory for the right half of the screen row. HL9 also
                            \ clears the C flag and jumps back to HL10, so this acts
                            \ like a conditional JSR instruction
    
    .HL10
    
     DEX                    \ Decrement the number of character blocks in X
    
     BNE HLL1               \ Loop back to draw more full-width lines, if we have
                            \ any more to draw
    
    .HL3
    
     LDA X2                 \ Now to draw the last character block at the right end
     AND #3                 \ of the line, so set X = X2 mod 3, which is the
     TAX                    \ horizontal pixel number where the line ends
    
     LDA TWFL,X             \ Fetch a ready-made byte with X pixels filled in at the
                            \ left end of the byte (so the filled pixels start at
                            \ the left edge and go up to point X), which is the
                            \ shape we want for the right end of the line
    
     AND COL                \ Apply the pixel mask in A to the four-pixel block of
                            \ coloured pixels in COL, so we now know which bits to
                            \ set in screen memory to paint the relevant pixels in
                            \ the required colour
    
     EOR (SC),Y             \ Store this into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen,
                            \ so we have now drawn the line's right cap
    
    .HL6
    
     LDY #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY YSAV               \ Restore Y from YSAV, so that it's preserved
    
     RTS                    \ Return from the subroutine
    
    .HL2
    
                            \ If we get here then the entire horizontal line fits
                            \ into one character block
    
     LDA X1                 \ Set X = X1 mod 4, which is the horizontal pixel number
     AND #3                 \ within the character block where the line starts (as
     TAX                    \ each pixel line in the character block is 4 pixels
                            \ wide)
    
     LDA TWFR,X             \ Fetch a ready-made byte with X pixels filled in at the
     STA T                  \ right end of the byte (so the filled pixels start at
                            \ point X and go all the way to the end of the byte)
    
     LDA X2                 \ Set X = X2 mod 4, which is the horizontal pixel number
     AND #3                 \ where the line ends
     TAX
    
     LDA TWFL,X             \ Fetch a ready-made byte with X pixels filled in at the
                            \ left end of the byte (so the filled pixels start at
                            \ the left edge and go up to point X)
    
     AND T                  \ We now have two bytes, one (T) containing pixels from
                            \ the starting point X1 onwards, and the other (A)
                            \ containing pixels up to the end point at X2, so we can
                            \ get the actual line we want to draw by AND'ing them
                            \ together. For example, if we want to draw a line from
                            \
                            \   T       = %00111111
                            \   A       = %11111100
                            \   T AND A = %00111100
                            \
                            \ So we can stick T AND A in screen memory to get the
                            \ line we want, which is what we do here by setting
                            \ A = A AND T
    
     AND COL                \ Apply the pixel mask in A to the four-pixel block of
                            \ coloured pixels in COL, so we now know which bits to
                            \ set in screen memory to paint the relevant pixels in
                            \ the required colour
    
     EOR (SC),Y             \ Store our horizontal line byte into screen memory at
     STA (SC),Y             \ SC(1 0), using EOR logic so it merges with whatever is
                            \ already on-screen
    
     LDY #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY YSAV               \ Restore Y from YSAV, so that it's preserved
    
     RTS                    \ Return from the subroutine
    
    .HL7
    
     INC SC+1               \ We have just crossed over from the left half of the
                            \ screen into the right half, so increment the high byte
                            \ in SC+1 so that SC(1 0) points to the page in screen
                            \ memory for the right half of the screen row
    
     CLC                    \ Clear the C flag (as HL7 is called with the C flag
                            \ set, which this instruction reverts)
    
     JMP HL8                \ Jump back to HL8, just after the instruction that
                            \ called HL7
    
    .HL9
    
     INC SC+1               \ We have just crossed over from the left half of the
                            \ screen into the right half, so increment the high byte
                            \ in SC+1 so that SC(1 0) points to the page in screen
                            \ memory for the right half of the screen row
    
     CLC                    \ Clear the C flag (as HL9 is called with the C flag
                            \ set, which this instruction reverts)
    
     JMP HL10               \ Jump back to HL10, just after the instruction that
                            \ called HL9
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Label [HL10](hloin.md#hl10) is local to this routine

[X]

Label [HL2](hloin.md#hl2) is local to this routine

[X]

Label [HL3](hloin.md#hl3) is local to this routine

[X]

Label [HL5](hloin.md#hl5) is local to this routine

[X]

Label [HL6](hloin.md#hl6) is local to this routine

[X]

Label [HL7](hloin.md#hl7) is local to this routine

[X]

Label [HL8](hloin.md#hl8) is local to this routine

[X]

Label [HL9](hloin.md#hl9) is local to this routine

[X]

Label [HLL1](hloin.md#hll1) is local to this routine

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [TWFL](../variable/twfl.md) (category: Drawing pixels)

Ready-made character rows for the left end of a horizontal line

[X]

Variable [TWFR](../variable/twfr.md) (category: Drawing pixels)

Ready-made character rows for the right end of a horizontal line

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](../workspace/zp.md#x2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [YSAV](../workspace/zp.md#ysav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the Y register, used in a number of places

[X]

Variable [orange](../variable/orange.md) (category: Drawing pixels)

Lookup table for two-pixel mode 1 orange pixels for the sun

[X]

Variable [ylookup](../variable/ylookup.md) (category: Drawing pixels)

Lookup table for converting pixel y-coordinate to page number of screen address

[LOINQ (Part 7 of 7)](loinq_part_7_of_7.md "Previous routine")[TWFL](../variable/twfl.md "Next routine")
