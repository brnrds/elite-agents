---
title: "The LOINQ (Part 7 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/loinq_part_7_of_7.html"
---

[LOINQ (Part 6 of 7)](loinq_part_6_of_7.md "Previous routine")[HLOIN](hloin.md "Next routine")
    
    
           Name: LOINQ (Part 7 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a steep line going up and right or down and left
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-loinq-part-7-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/loin_part_7_of_7-loinq_part_7_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * The line is going up and right (no swap) or down and left (swap)
    
       * X1 >= X2 and Y1 >= Y2
    
       * Draw from (X1, Y1) at bottom left to (X2, Y2) at top right, omitting the
         first pixel
    
     This routine looks complex, but that's because the loop that's used in the
     BBC Micro cassette and disc versions has been unrolled to speed it up. The
     algorithm is unchanged, it's just a lot longer.
    
    
    
    
    .LFT
    
     LDA SWAP               \ If SWAP = 0 then we didn't swap the coordinates above,
     BEQ LI291              \ so jump down to LI291 to plot the first pixel
    
     TYA                    \ Fetch bits 0-2 of the y-coordinate, so Y contains the
     AND #7                 \ y-coordinate mod 8
     TAY                    \
                            \ So Y is the pixel row within the character block where
                            \ we want to start drawing
    
     BNE P%+5               \ If Y = 0, jump to LI407+8 to start plotting from the
     JMP LI407+8            \ pixel above the top row of this character block
                            \ (LI407+8 points to the DEX instruction after the
                            \ EOR/STA instructions, so the pixel at row 0 doesn't
                            \ get plotted but we join at the right point to
                            \ decrement X and Y correctly to continue plotting from
                            \ the character row above)
    
     CPY #2                 \ If Y < 2 (i.e. Y = 1), jump to LI406+8 to start
     BCS P%+5               \ plotting from row 0 of this character block, missing
     JMP LI406+8            \ out row 1
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 2, jump to LI405+8 to start plotting from row
     JMP LI405+8            \ 1 of this character block, missing out row 2
    
     CPY #4                 \ If Y < 4 (i.e. Y = 3), jump to LI404+8 to start
     BCS P%+5               \ plotting from row 2 of this character block, missing
     JMP LI404+8            \ out row 3
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 4, jump to LI403+8 to start plotting from row
     JMP LI403+8            \ 3 of this character block, missing out row 4
    
     CPY #6                 \ If Y < 6 (i.e. Y = 5), jump to LI402+8 to start
     BCS P%+5               \ plotting from row 4 of this character block, missing
     JMP LI402+8            \ out row 5
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ P%+5               \ If Y <> 6 (i.e. Y = 7), jump to LI400+8 to start
     JMP LI400+8            \ plotting from row 6 of this character block, missing
                            \ out row 7
    
     JMP LI401+8            \ Otherwise Y = 6, so jump to LI401+8 to start plotting
                            \ from row 5 of this character block, missing out row 6
    
    .LI291
    
     DEX                    \ Decrement the counter in X because we're about to plot
                            \ the first pixel
    
     TYA                    \ Fetch bits 0-2 of the y-coordinate, so Y contains the
     AND #7                 \ y-coordinate mod 8
     TAY                    \
                            \ So Y is the pixel row within the character block where
                            \ we want to start drawing
    
     BNE P%+5               \ If Y = 0, jump to LI407 to start plotting from row 0
     JMP LI407              \ of this character block
    
     CPY #2                 \ If Y < 2 (i.e. Y = 1), jump to LI406 to start plotting
     BCS P%+5               \ from row 1 of this character block
     JMP LI406
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 2, jump to LI405 to start plotting from row 2
     JMP LI405              \ of this character block
    
     CPY #4                 \ If Y < 4 (i.e. Y = 3), jump to LI404 (via LI404S) to
     BCC LI404S             \ start plotting from row 3 of this character block
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ LI403S             \ If Y = 4, jump to LI403 (via LI403S) to start plotting
                            \ from row 4 of this character block
    
     CPY #6                 \ If Y < 6 (i.e. Y = 5), jump to LI402 (via LI402S) to
     BCC LI402S             \ start plotting from row 5 of this character block
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ LI401S             \ If Y = 6, jump to LI401 (via LI401S) to start plotting
                            \ from row 6 of this character block
    
     JMP LI400              \ Otherwise Y = 7, so jump to LI400 to start plotting
                            \ from row 7 of this character block
    
    .LI410
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI400, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI401              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI401 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI401S
    
     BCC LI401              \ Jump to LI401 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI411
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI410, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI402              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI402 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI402S
    
     BCC LI402              \ Jump to LI402 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI412
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI420, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI403              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI403 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI403S
    
     BCC LI403              \ Jump to LI403 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI413
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI430, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI404              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI404 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI404S
    
     BCC LI404              \ Jump to LI404 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LIEX5
    
     RTS                    \ Return from the subroutine
    
    .LI400
    
                            \ Plot a pixel on row 7 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX5              \ If we have just reached the right end of the line,
                            \ jump to LIEX5 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI410              \ If the addition overflowed, jump to LI410 to move to
                            \ the pixel in the row above, which returns us to LI401
                            \ below
    
    .LI401
    
                            \ Plot a pixel on row 6 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX5              \ If we have just reached the right end of the line,
                            \ jump to LIEX5 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI411              \ If the addition overflowed, jump to LI411 to move to
                            \ the pixel in the row above, which returns us to LI402
                            \ below
    
    .LI402
    
                            \ Plot a pixel on row 5 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX5              \ If we have just reached the right end of the line,
                            \ jump to LIEX5 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI412              \ If the addition overflowed, jump to LI412 to move to
                            \ the pixel in the row above, which returns us to LI403
                            \ below
    
    .LI403
    
                            \ Plot a pixel on row 4 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX5              \ If we have just reached the right end of the line,
                            \ jump to LIEX5 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI413              \ If the addition overflowed, jump to LI413 to move to
                            \ the pixel in the row above, which returns us to LI404
                            \ below
    
    .LI404
    
                            \ Plot a pixel on row 3 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX6              \ If we have just reached the right end of the line,
                            \ jump to LIEX6 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI414              \ If the addition overflowed, jump to LI414 to move to
                            \ the pixel in the row above, which returns us to LI405
                            \ below
    
    .LI405
    
                            \ Plot a pixel on row 2 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX6              \ If we have just reached the right end of the line,
                            \ jump to LIEX6 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI415              \ If the addition overflowed, jump to LI415 to move to
                            \ the pixel in the row above, which returns us to LI406
                            \ below
    
    .LI406
    
                            \ Plot a pixel on row 1 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX6              \ If we have just reached the right end of the line,
                            \ jump to LIEX6 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI416              \ If the addition overflowed, jump to LI416 to move to
                            \ the pixel in the row above, which returns us to LI407
                            \ below
    
    .LI407
    
                            \ Plot a pixel on row 0 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX6              \ If we have just reached the right end of the line,
                            \ jump to LIEX6 to return from the subroutine
    
     DEC SC+1               \ We just reached the top of the character block, so
     DEC SC+1               \ decrement the high byte in SC(1 0) twice to point to
     LDY #7                 \ the screen row above (as there are two pages per
                            \ screen row) and set Y to point to the last row in the
                            \ new character block
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS P%+5               \ If the addition didn't overflow, jump to LI400 to
     JMP LI400              \ continue plotting from row 7 of the new character
                            \ block
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI407 above, so shift the
                            \ single pixel in R to the left, so the next pixel we
                            \ plot will be at the previous x-coordinate
    
     BCS P%+5               \ If the pixel didn't fall out of the left end of R
     JMP LI400              \ into the C flag, then jump to LI400 to continue
                            \ plotting from row 7 of the new character block
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     JMP LI400              \ Jump to LI400 to continue plotting from row 7 of the
                            \ new character block
    
    .LIEX6
    
     RTS                    \ Return from the subroutine
    
    .LI414
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI440, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI405              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI405 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI405              \ Jump to LI405 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI415
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI450, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI406              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI406 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI406              \ Jump to LI406 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI416
    
     ASL R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI460, so shift the single
                            \ pixel in R to the left, so the next pixel we plot will
                            \ be at the previous x-coordinate
    
     BCC LI407              \ If the pixel didn't fall out of the left end of R
                            \ into the C flag, then jump to LI407 to plot the pixel
                            \ on the next character row up
    
     LDA #%00010001         \ Otherwise we need to move over to the next character
     STA R                  \ block to the left, so set a mask in R to the fourth
                            \ pixel in the four-pixel byte
    
     LDA SC                 \ Subtract 8 from SC, so SC(1 0) now points to the
     SBC #8                 \ previous character along to the left
     STA SC
    
     BCS P%+4               \ If the subtraction underflowed, decrement the high
     DEC SC+1               \ byte in SC(1 0) to move to the previous page in
                            \ screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     JMP LI407              \ Jump to LI407 to rejoin the pixel plotting routine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Label [LI291](loinq_part_7_of_7.md#li291) is local to this routine

[X]

Label [LI400](loinq_part_7_of_7.md#li400) is local to this routine

[X]

[X]

Label [LI401](loinq_part_7_of_7.md#li401) is local to this routine

[X]

[X]

Label [LI401S](loinq_part_7_of_7.md#li401s) is local to this routine

[X]

Label [LI402](loinq_part_7_of_7.md#li402) is local to this routine

[X]

[X]

Label [LI402S](loinq_part_7_of_7.md#li402s) is local to this routine

[X]

Label [LI403](loinq_part_7_of_7.md#li403) is local to this routine

[X]

[X]

Label [LI403S](loinq_part_7_of_7.md#li403s) is local to this routine

[X]

Label [LI404](loinq_part_7_of_7.md#li404) is local to this routine

[X]

[X]

Label [LI404S](loinq_part_7_of_7.md#li404s) is local to this routine

[X]

Label [LI405](loinq_part_7_of_7.md#li405) is local to this routine

[X]

[X]

Label [LI406](loinq_part_7_of_7.md#li406) is local to this routine

[X]

[X]

Label [LI407](loinq_part_7_of_7.md#li407) is local to this routine

[X]

[X]

Label [LI410](loinq_part_7_of_7.md#li410) is local to this routine

[X]

Label [LI411](loinq_part_7_of_7.md#li411) is local to this routine

[X]

Label [LI412](loinq_part_7_of_7.md#li412) is local to this routine

[X]

Label [LI413](loinq_part_7_of_7.md#li413) is local to this routine

[X]

Label [LI414](loinq_part_7_of_7.md#li414) is local to this routine

[X]

Label [LI415](loinq_part_7_of_7.md#li415) is local to this routine

[X]

Label [LI416](loinq_part_7_of_7.md#li416) is local to this routine

[X]

Label [LIEX5](loinq_part_7_of_7.md#liex5) is local to this routine

[X]

Label [LIEX6](loinq_part_7_of_7.md#liex6) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [SWAP](../workspace/wp.md#swap) in workspace [WP](../workspace/wp.md)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[LOINQ (Part 6 of 7)](loinq_part_6_of_7.md "Previous routine")[HLOIN](hloin.md "Next routine")
