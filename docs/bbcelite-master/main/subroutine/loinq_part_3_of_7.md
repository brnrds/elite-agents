---
title: "The LOINQ (Part 3 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/loinq_part_3_of_7.html"
---

[LOINQ (Part 2 of 7)](loinq_part_2_of_7.md "Previous routine")[LOINQ (Part 4 of 7)](loinq_part_4_of_7.md "Next routine")
    
    
           Name: LOINQ (Part 3 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a shallow line going right and up or left and down
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-loinq-part-3-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/loin_part_3_of_7-loinq_part_3_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * The line is going right and up (no swap) or left and down (swap)
    
       * X1 < X2 and Y1 > Y2
    
       * Draw from (X1, Y1) at bottom left to (X2, Y2) at top right, omitting the
         first pixel
    
     This routine looks complex, but that's because the loop that's used in the
     BBC Micro cassette and disc versions has been unrolled to speed it up. The
     algorithm is unchanged, it's just a lot longer.
    
    
    
    
     LDA #%10001000         \ Modify the value in the LDA instruction at LI100 below
     AND COL                \ to contain a pixel mask for the first pixel in the
     STA LI100+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%01000100         \ Modify the value in the LDA instruction at LI110 below
     AND COL                \ to contain a pixel mask for the second pixel in the
     STA LI110+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%00100010         \ Modify the value in the LDA instruction at LI120 below
     AND COL                \ to contain a pixel mask for the third pixel in the
     STA LI120+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%00010001         \ Modify the value in the LDA instruction at LI130 below
     AND COL                \ to contain a pixel mask for the fourth pixel in the
     STA LI130+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
                            \ We now work our way along the line from left to right,
                            \ using X as a decreasing counter, and at each count we
                            \ plot a single pixel using the pixel mask in R
    
     LDA SWAP               \ If SWAP = 0 then we didn't swap the coordinates above,
     BEQ LI190              \ so jump down to LI190 to plot the first pixel
    
                            \ If we get here then we want to omit the first pixel
    
     LDA R                  \ Fetch the pixel byte from R, which we set in part 2 to
                            \ the horizontal pixel number within the character block
                            \ where the line starts (so it's 0, 1, 2 or 3)
    
     BEQ LI100+6            \ If R = 0, jump to LI100+6 to start plotting from the
                            \ second pixel in this byte (LI100+6 points to the DEX
                            \ instruction after the EOR/STA instructions, so the
                            \ pixel doesn't get plotted but we join at the right
                            \ point to decrement X correctly to plot the next three)
    
     CMP #2                 \ If R < 2 (i.e. R = 1), jump to LI110+6 to skip the
     BCC LI110+6            \ first two pixels but plot the next two
    
     CLC                    \ Clear the C flag so it doesn't affect the additions
                            \ below
    
     BEQ LI120+6            \ If R = 2, jump to LI120+6 to skip the first three
                            \ pixels but plot the last one
    
     BNE LI130+6            \ If we get here then R must be 3, so jump to LI130+6 to
                            \ skip plotting any of the pixels, but making sure we
                            \ join the routine just after the plotting instructions
                            \ (this BNE is effectively a JMP as we just passed
                            \ through a BEQ)
    
    .LI190
    
     DEX                    \ Decrement the counter in X because we're about to plot
                            \ the first pixel
    
     LDA R                  \ Fetch the pixel byte from R, which we set in part 2 to
                            \ the horizontal pixel number within the character block
                            \ where the line starts (so it's 0, 1, 2 or 3)
    
     BEQ LI100              \ If R = 0, jump to LI100 to start plotting from the
                            \ first pixel in this byte
    
     CMP #2                 \ If R < 2 (i.e. R = 1), jump to LI110 to start plotting
     BCC LI110              \ from the second pixel in this byte
    
     CLC                    \ Clear the C flag so it doesn't affect the additions
                            \ below
    
     BEQ LI120              \ If R = 2, jump to LI120 to start plotting from the
                            \ third pixel in this byte
    
     JMP LI130              \ If we get here then R must be 3, so jump to LI130 to
                            \ start plotting from the fourth pixel in this byte
    
    .LI100
    
     LDA #%10001000         \ Set a mask in A to the first pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
    .LIEXS
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI110              \ If the addition didn't overflow, jump to LI110
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     DEY                    \ decrement Y to move to the pixel line above
    
     BMI LI101              \ If Y is negative we need to move up into the character
                            \ block above, so jump to LI101 to decrement the screen
                            \ address accordingly (jumping back to LI110 afterwards)
    
    .LI110
    
     LDA #%01000100         \ Set a mask in A to the second pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI120              \ If the addition didn't overflow, jump to LI120
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     DEY                    \ decrement Y to move to the pixel line above
    
     BMI LI111              \ If Y is negative we need to move up into the character
                            \ block above, so jump to LI111 to decrement the screen
                            \ address accordingly (jumping back to LI120 afterwards)
    
    .LI120
    
     LDA #%00100010         \ Set a mask in A to the third pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI130              \ If the addition didn't overflow, jump to LI130
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     DEY                    \ decrement Y to move to the pixel line above
    
     BMI LI121              \ If Y is negative we need to move up into the character
                            \ block above, so jump to LI121 to decrement the screen
                            \ address accordingly (jumping back to LI130 afterwards)
    
    .LI130
    
     LDA #%00010001         \ Set a mask in A to the fourth pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI140              \ If the addition didn't overflow, jump to LI140
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     DEY                    \ decrement Y to move to the pixel line above
    
     BMI LI131              \ If Y is negative we need to move up into the character
                            \ block above, so jump to LI131 to decrement the screen
                            \ address accordingly (jumping back to LI140 afterwards)
    
    .LI140
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX               \ If we have just reached the right end of the line,
                            \ jump to LIEX to return from the subroutine
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #8                 \ character along to the right
     STA SC
    
     BCC LI100              \ If the addition didn't overflow, jump back to LI100
                            \ to plot the next pixel
    
     INC SC+1               \ Otherwise the low byte of SC(1 0) just overflowed, so
                            \ increment the high byte SC+1 as we just crossed over
                            \ into the right half of the screen
    
     CLC                    \ Clear the C flag to avoid breaking any arithmetic
    
     BCC LI100              \ Jump back to LI100 to plot the next pixel
    
    .LI101
    
     DEC SC+1               \ If we get here then we need to move up into the
     DEC SC+1               \ character block above, so we decrement the high byte
     LDY #7                 \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the last line in
                            \ that character block
    
     BPL LI110              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI111
    
     DEC SC+1               \ If we get here then we need to move up into the
     DEC SC+1               \ character block above, so we decrement the high byte
     LDY #7                 \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the last line in
                            \ that character block
    
     BPL LI120              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI121
    
     DEC SC+1               \ If we get here then we need to move up into the
     DEC SC+1               \ character block above, so we decrement the high byte
     LDY #7                 \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the last line in
                            \ that character block
    
     BPL LI130              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI131
    
     DEC SC+1               \ If we get here then we need to move up into the
     DEC SC+1               \ character block above, so we decrement the high byte
     LDY #7                 \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the last line in
                            \ that character block
    
     BPL LI140              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LIEX
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Label [LI100](loinq_part_3_of_7.md#li100) is local to this routine

[X]

[X]

Label [LI101](loinq_part_3_of_7.md#li101) is local to this routine

[X]

Label [LI110](loinq_part_3_of_7.md#li110) is local to this routine

[X]

[X]

Label [LI111](loinq_part_3_of_7.md#li111) is local to this routine

[X]

Label [LI120](loinq_part_3_of_7.md#li120) is local to this routine

[X]

[X]

Label [LI121](loinq_part_3_of_7.md#li121) is local to this routine

[X]

Label [LI130](loinq_part_3_of_7.md#li130) is local to this routine

[X]

[X]

Label [LI131](loinq_part_3_of_7.md#li131) is local to this routine

[X]

Label [LI140](loinq_part_3_of_7.md#li140) is local to this routine

[X]

Label [LI190](loinq_part_3_of_7.md#li190) is local to this routine

[X]

Label [LIEX](loinq_part_3_of_7.md#liex) is local to this routine

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

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

[LOINQ (Part 2 of 7)](loinq_part_2_of_7.md "Previous routine")[LOINQ (Part 4 of 7)](loinq_part_4_of_7.md "Next routine")
