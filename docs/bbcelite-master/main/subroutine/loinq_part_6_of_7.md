---
title: "The LOINQ (Part 6 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/loinq_part_6_of_7.html"
---

[LOINQ (Part 5 of 7)](loinq_part_5_of_7.md "Previous routine")[LOINQ (Part 7 of 7)](loinq_part_7_of_7.md "Next routine")
    
    
           Name: LOINQ (Part 6 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a steep line going up and left or down and right
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-loinq-part-6-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/loin_part_6_of_7-loinq_part_6_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * The line is going up and left (no swap) or down and right (swap)
    
       * X1 < X2 and Y1 >= Y2
    
       * Draw from (X1, Y1) at top left to (X2, Y2) at bottom right, omitting the
         first pixel
    
     This routine looks complex, but that's because the loop that's used in the
     BBC Micro cassette and disc versions has been unrolled to speed it up. The
     algorithm is unchanged, it's just a lot longer.
    
    
    
    
     LDA SWAP               \ If SWAP = 0 then we didn't swap the coordinates above,
     BEQ LI290              \ so jump down to LI290 to plot the first pixel
    
     TYA                    \ Fetch bits 0-2 of the y-coordinate, so Y contains the
     AND #7                 \ y-coordinate mod 8
     TAY
    
     BNE P%+5               \ If Y = 0, jump to LI307+8 to start plotting from the
     JMP LI307+8            \ pixel above the top row of this character block
                            \ (LI307+8 points to the DEX instruction after the
                            \ EOR/STA instructions, so the pixel at row 0 doesn't
                            \ get plotted but we join at the right point to
                            \ decrement X and Y correctly to continue plotting from
                            \ the character row above)
    
     CPY #2                 \ If Y < 2 (i.e. Y = 1), jump to LI306+8 to start
     BCS P%+5               \ plotting from row 0 of this character block, missing
     JMP LI306+8            \ out row 1
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 2, jump to LI305+8 to start plotting from row
     JMP LI305+8            \ 1 of this character block, missing out row 2
    
     CPY #4                 \ If Y < 4 (i.e. Y = 3), jump to LI304+8 to start
     BCS P%+5               \ plotting from row 2 of this character block, missing
     JMP LI304+8            \ out row 3
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 4, jump to LI303+8 to start plotting from row
     JMP LI303+8            \ 3 of this character block, missing out row 4
    
     CPY #6                 \ If Y < 6 (i.e. Y = 5), jump to LI302+8 to start
     BCS P%+5               \ plotting from row 4 of this character block, missing
     JMP LI302+8            \ out row 5
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ P%+5               \ If Y <> 6 (i.e. Y = 7), jump to LI300+8 to start
     JMP LI300+8            \ plotting from row 6 of this character block, missing
                            \ out row 7
    
     JMP LI301+8            \ Otherwise Y = 6, so jump to LI301+8 to start plotting
                            \ from row 5 of this character block, missing out row 6
    
    .LI290
    
     DEX                    \ Decrement the counter in X because we're about to plot
                            \ the first pixel
    
     TYA                    \ Fetch bits 0-2 of the y-coordinate, so Y contains the
     AND #7                 \ y-coordinate mod 8
     TAY
    
     BNE P%+5               \ If Y = 0, jump to LI307 to start plotting from row 0
     JMP LI307              \ of this character block
    
     CPY #2                 \ If Y < 2 (i.e. Y = 1), jump to LI306 to start plotting
     BCS P%+5               \ from row 1 of this character block
     JMP LI306
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BNE P%+5               \ If Y = 2, jump to LI305 to start plotting from row 2
     JMP LI305              \ of this character block
    
     CPY #4                 \ If Y < 4 (i.e. Y = 3), jump to LI304 (via LI304S) to
     BCC LI304S             \ start plotting from row 3 of this character block
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ LI303S             \ If Y = 4, jump to LI303 (via LI303S) to start plotting
                            \ from row 4 of this character block
    
     CPY #6                 \ If Y < 6 (i.e. Y = 5), jump to LI302 (via LI302S) to
     BCC LI302S             \ start plotting from row 5 of this character block
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BEQ LI301S             \ If Y = 6, jump to LI301 (via LI301S) to start plotting
                            \ from row 6 of this character block
    
     JMP LI300              \ Otherwise Y = 7, so jump to LI300 to start plotting
                            \ from row 7 of this character block
    
    .LI310
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI300, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI301              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI301 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI301              \ If the addition didn't overflow, jump to LI301 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI301S
    
     BCC LI301              \ Jump to LI301 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI311
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI301, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI302              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI302 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI302              \ If the addition didn't overflow, jump to LI302 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI302S
    
     BCC LI302              \ Jump to LI302 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI312
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI302, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI303              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI303 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI303              \ If the addition didn't overflow, jump to LI303 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI303S
    
     BCC LI303              \ Jump to LI303 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI313
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI303, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI304              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI304 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI304              \ If the addition didn't overflow, jump to LI304 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
    .LI304S
    
     BCC LI304              \ Jump to LI304 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LIEX3
    
     RTS                    \ Return from the subroutine
    
    .LI300
    
                            \ Plot a pixel on row 7 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX3              \ If we have just reached the right end of the line,
                            \ jump to LIEX3 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI310              \ If the addition overflowed, jump to LI310 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI301 below
    
    .LI301
    
                            \ Plot a pixel on row 6 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX3              \ If we have just reached the right end of the line,
                            \ jump to LIEX3 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI311              \ If the addition overflowed, jump to LI311 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI302 below
    
    .LI302
    
                            \ Plot a pixel on row 5 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX3              \ If we have just reached the right end of the line,
                            \ jump to LIEX3 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI312              \ If the addition overflowed, jump to LI312 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI303 below
    
    .LI303
    
                            \ Plot a pixel on row 4 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX3              \ If we have just reached the right end of the line,
                            \ jump to LIEX3 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI313              \ If the addition overflowed, jump to LI313 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI304 below
    
    .LI304
    
                            \ Plot a pixel on row 3 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX4              \ If we have just reached the right end of the line,
                            \ jump to LIEX4 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI314              \ If the addition overflowed, jump to LI314 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI305 below
    
    .LI305
    
                            \ Plot a pixel on row 2 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX4              \ If we have just reached the right end of the line,
                            \ jump to LIEX4 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI315              \ If the addition overflowed, jump to LI315 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI306 below
    
    .LI306
    
                            \ Plot a pixel on row 1 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX4              \ If we have just reached the right end of the line,
                            \ jump to LIEX4 to return from the subroutine
    
     DEY                    \ Decrement Y to step up along the y-axis
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS LI316              \ If the addition overflowed, jump to LI316 to move to
                            \ the pixel in the next character block along, which
                            \ returns us to LI307 below
    
    .LI307
    
                            \ Plot a pixel on row 0 of this character block
    
     LDA R                  \ Fetch the pixel byte from R and apply the colour in
     AND COL                \ COL to it
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX4              \ If we have just reached the right end of the line,
                            \ jump to LIEX4 to return from the subroutine
    
     DEC SC+1               \ We just reached the top of the character block, so
     DEC SC+1               \ decrement the high byte in SC(1 0) twice to point to
     LDY #7                 \ the screen row above (as there are two pages per
                            \ screen row) and set Y to point to the last row in the
                            \ new character block
    
     LDA S                  \ Set S = S + P to update the slope error
     ADC P
     STA S
    
     BCS P%+5               \ If the addition didn't overflow, jump to LI300 to
     JMP LI300              \ continue plotting in the next character block along
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI307 above, so shift the
                            \ single pixel in R to the right, so the next pixel we
                            \ plot will be at the next x-coordinate
    
     BCS P%+5               \ If the pixel didn't fall out of the right end of R
     JMP LI300              \ into the C flag, then jump to LI400 to continue
                            \ plotting in the next character block along
    
     LDA #%10001000         \ Otherwise we need to move over to the next character
     STA R                  \ along, so set a mask in R to the first pixel in the
                            \ four-pixel byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ took the above BCS, so the ADC adds 8)
    
     BCS P%+5               \ If the addition didn't overflow, ump to LI300 to
     JMP LI300              \ continue plotting in the next character block along
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     JMP LI300              \ Jump to LI300 to continue plotting in the next
                            \ character block along
    
    .LIEX4
    
     RTS                    \ Return from the subroutine
    
    .LI314
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI304, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI305              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI305 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI305              \ If the addition didn't overflow, jump to LI305 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI305              \ Jump to LI305 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI315
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI305, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI306              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI306 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI306              \ If the addition didn't overflow, jump to LI306 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI306              \ Jump to LI306 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    
    .LI316
    
     LSR R                  \ If we get here then the slope error just overflowed
                            \ after plotting the pixel in LI306, so shift the single
                            \ pixel in R to the right, so the next pixel we plot
                            \ will be at the next x-coordinate along
    
     BCC LI307              \ If the pixel didn't fall out of the right end of R
                            \ into the C flag, then jump to LI307 to plot the pixel
                            \ on the next character row up
    
     LDA #%10001000         \ Set a mask in R to the first pixel in the four-pixel
     STA R                  \ byte
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #7                 \ character along to the right (the C flag is set as we
     STA SC                 \ didn't take the above BCC, so the ADC adds 8)
    
     BCC LI307              \ If the addition didn't overflow, jump to LI307 to plot
                            \ the pixel on the next character row up
    
     INC SC+1               \ The addition overflowed, so increment the high byte in
                            \ SC(1 0) to move to the next page in screen memory
    
     CLC                    \ Clear the C flag so it doesn't affect the arithmetic
                            \ below
    
     BCC LI307              \ Jump to LI307 to rejoin the pixel plotting routine
                            \ (this BCC is effectively a JMP as the C flag is clear)
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Label [LI290](loinq_part_6_of_7.md#li290) is local to this routine

[X]

Label [LI300](loinq_part_6_of_7.md#li300) is local to this routine

[X]

[X]

Label [LI301](loinq_part_6_of_7.md#li301) is local to this routine

[X]

[X]

Label [LI301S](loinq_part_6_of_7.md#li301s) is local to this routine

[X]

Label [LI302](loinq_part_6_of_7.md#li302) is local to this routine

[X]

[X]

Label [LI302S](loinq_part_6_of_7.md#li302s) is local to this routine

[X]

Label [LI303](loinq_part_6_of_7.md#li303) is local to this routine

[X]

[X]

Label [LI303S](loinq_part_6_of_7.md#li303s) is local to this routine

[X]

Label [LI304](loinq_part_6_of_7.md#li304) is local to this routine

[X]

[X]

Label [LI304S](loinq_part_6_of_7.md#li304s) is local to this routine

[X]

Label [LI305](loinq_part_6_of_7.md#li305) is local to this routine

[X]

[X]

Label [LI306](loinq_part_6_of_7.md#li306) is local to this routine

[X]

[X]

Label [LI307](loinq_part_6_of_7.md#li307) is local to this routine

[X]

[X]

Label [LI310](loinq_part_6_of_7.md#li310) is local to this routine

[X]

Label [LI311](loinq_part_6_of_7.md#li311) is local to this routine

[X]

Label [LI312](loinq_part_6_of_7.md#li312) is local to this routine

[X]

Label [LI313](loinq_part_6_of_7.md#li313) is local to this routine

[X]

Label [LI314](loinq_part_6_of_7.md#li314) is local to this routine

[X]

Label [LI315](loinq_part_6_of_7.md#li315) is local to this routine

[X]

Label [LI316](loinq_part_6_of_7.md#li316) is local to this routine

[X]

Label [LIEX3](loinq_part_6_of_7.md#liex3) is local to this routine

[X]

Label [LIEX4](loinq_part_6_of_7.md#liex4) is local to this routine

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

[LOINQ (Part 5 of 7)](loinq_part_5_of_7.md "Previous routine")[LOINQ (Part 7 of 7)](loinq_part_7_of_7.md "Next routine")
