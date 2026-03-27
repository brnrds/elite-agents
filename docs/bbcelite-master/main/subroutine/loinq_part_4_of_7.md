---
title: "The LOINQ (Part 4 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/loinq_part_4_of_7.html"
---

[LOINQ (Part 3 of 7)](loinq_part_3_of_7.md "Previous routine")[LOINQ (Part 5 of 7)](loinq_part_5_of_7.md "Next routine")
    
    
           Name: LOINQ (Part 4 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a shallow line going right and down or left and up
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-loinq-part-4-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/loin_part_4_of_7-loinq_part_4_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * The line is going right and down (no swap) or left and up (swap)
    
       * X1 < X2 and Y1 <= Y2
    
       * Draw from (X1, Y1) at top left to (X2, Y2) at bottom right, omitting the
         first pixel
    
     This routine looks complex, but that's because the loop that's used in the
     BBC Micro cassette and disc versions has been unrolled to speed it up. The
     algorithm is unchanged, it's just a lot longer.
    
    
    
    
    .DOWN
    
     LDA #%10001000         \ Modify the value in the LDA instruction at LI200 below
     AND COL                \ to contain a pixel mask for the first pixel in the
     STA LI200+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%01000100         \ Modify the value in the LDA instruction at LI210 below
     AND COL                \ to contain a pixel mask for the second pixel in the
     STA LI210+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%00100010         \ Modify the value in the LDA instruction at LI220 below
     AND COL                \ to contain a pixel mask for the third pixel in the
     STA LI220+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA #%00010001         \ Modify the value in the LDA instruction at LI230 below
     AND COL                \ to contain a pixel mask for the fourth pixel in the
     STA LI230+1            \ four-pixel byte, in the colour COL, so that it draws
                            \ in the correct colour
    
     LDA SC                 \ Set SC(1 0) = SC(1 0) - 248
     SBC #248
     STA SC
     LDA SC+1
     SBC #0
     STA SC+1
    
     TYA                    \ Set bits 3-7 of Y, which contains the pixel row within
     EOR #%11111000         \ the character, and is therefore in the range 0-7, so
     TAY                    \ this does Y = 248 + Y
                            \
                            \ We therefore have the following:
                            \
                            \   SC(1 0) + Y = SC(1 0) - 248 + 248 + Y
                            \               = SC(1 0) + Y
                            \
                            \ so the screen location we poke hasn't changed, but Y
                            \ is now a larger number and SC is smaller. This means
                            \ we can increment Y to move down a line, as per usual,
                            \ but we can test for when it reaches the bottom of the
                            \ character block with a simple BEQ rather than checking
                            \ whether it's reached 8, so this appears to be a code
                            \ optimisation
                            \
                            \ If it helps, you can think of Y as being a negative
                            \ number that we are incrementing towards zero as we
                            \ move along the line - we just need to alter the value
                            \ of SC so that SC(1 0) + Y points to the right address
    
                            \ We now work our way along the line from left to right,
                            \ using X as a decreasing counter, and at each count we
                            \ plot a single pixel using the pixel mask in R
    
     LDA SWAP               \ If SWAP = 0 then we didn't swap the coordinates above,
     BEQ LI191              \ so jump down to LI191 to plot the first pixel
    
                            \ If we get here then we want to omit the first pixel
    
     LDA R                  \ Fetch the pixel byte from R, which we set in part 2 to
                            \ the horizontal pixel number within the character block
                            \ where the line starts (so it's 0, 1, 2 or 3)
    
     BEQ LI200+6            \ If R = 0, jump to LI200+6 to start plotting from the
                            \ second pixel in this byte (LI200+6 points to the DEX
                            \ instruction after the EOR/STA instructions, so the
                            \ pixel doesn't get plotted but we join at the right
                            \ point to decrement X correctly to plot the next three)
    
     CMP #2                 \ If R < 2 (i.e. R = 1), jump to LI210+6 to skip the
     BCC LI210+6            \ first two pixels but plot the next two
    
     CLC                    \ Clear the C flag so it doesn't affect the additions
                            \ below
    
     BEQ LI220+6            \ If R = 2, jump to LI220+6 to skip the first three
                            \ pixels but plot the last one
    
     BNE LI230+6            \ If we get here then R must be 3, so jump to LI230+6 to
                            \ skip plotting any of the pixels, but making sure we
                            \ join the routine just after the plotting instructions
                            \ (this BNE is effectively a JMP as we just passed
                            \ through a BEQ)
    
    .LI191
    
     DEX                    \ Decrement the counter in X because we're about to plot
                            \ the first pixel
    
     LDA R                  \ Fetch the pixel byte from R, which we set in part 2 to
                            \ the horizontal pixel number within the character block
                            \ where the line starts (so it's 0, 1, 2 or 3)
    
     BEQ LI200              \ If R = 0, jump to LI200 to start plotting from the
                            \ first pixel in this byte
    
     CMP #2                 \ If R < 2 (i.e. R = 1), jump to LI210 to start plotting
     BCC LI210              \ from the second pixel in this byte
    
     CLC                    \ Clear the C flag so it doesn't affect the additions
                            \ below
    
     BEQ LI220              \ If R = 2, jump to LI220 to start plotting from the
                            \ third pixel in this byte
    
     BNE LI230              \ If we get here then R must be 3, so jump to LI130 to
                            \ start plotting from the fourth pixel in this byte
                            \ (this BNE is effectively a JMP as we just passed
                            \ through a BEQ)
    
    .LI200
    
     LDA #%10001000         \ Set a mask in A to the first pixel in the four-pixel
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
    
     BCC LI210              \ If the addition didn't overflow, jump to LI210
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     INY                    \ increment Y to move to the pixel line below
    
     BEQ LI201              \ If Y is zero we need to move down into the character
                            \ block below, so jump to LI201 to increment the screen
                            \ address accordingly (jumping back to LI210 afterwards)
    
    .LI210
    
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
    
     BCC LI220              \ If the addition didn't overflow, jump to LI220
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     INY                    \ increment Y to move to the pixel line below
    
     BEQ LI211              \ If Y is zero we need to move down into the character
                            \ block below, so jump to LI211 to increment the screen
                            \ address accordingly (jumping back to LI220 afterwards)
    
    .LI220
    
     LDA #%00100010         \ Set a mask in A to the third pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX2              \ If we have just reached the right end of the line,
                            \ jump to LIEX2 to return from the subroutine
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI230              \ If the addition didn't overflow, jump to LI230
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     INY                    \ increment Y to move to the pixel line below
    
     BEQ LI221              \ If Y is zero we need to move down into the character
                            \ block below, so jump to LI221 to increment the screen
                            \ address accordingly (jumping back to LI230 afterwards)
    
    .LI230
    
     LDA #%00010001         \ Set a mask in A to the fourth pixel in the four-pixel
                            \ byte (note that this value is modified by the code at
                            \ the start of this section to be a bit mask for the
                            \ colour in COL)
    
     EOR (SC),Y             \ Store A into screen memory at SC(1 0), using EOR
     STA (SC),Y             \ logic so it merges with whatever is already on-screen
    
     LDA S                  \ Set S = S + Q to update the slope error
     ADC Q
     STA S
    
     BCC LI240              \ If the addition didn't overflow, jump to LI240
    
     CLC                    \ Otherwise we just overflowed, so clear the C flag and
     INY                    \ increment Y to move to the pixel line below
    
     BEQ LI231              \ If Y is zero we need to move down into the character
                            \ block below, so jump to LI231 to increment the screen
                            \ address accordingly (jumping back to LI240 afterwards)
    
    .LI240
    
     DEX                    \ Decrement the counter in X
    
     BEQ LIEX2              \ If we have just reached the right end of the line,
                            \ jump to LIEX2 to return from the subroutine
    
     LDA SC                 \ Add 8 to SC, so SC(1 0) now points to the next
     ADC #8                 \ character along to the right
     STA SC
    
     BCC LI200              \ If the addition didn't overflow, jump back to LI200
                            \ to plot the next pixel
    
     INC SC+1               \ Otherwise the low byte of SC(1 0) just overflowed, so
                            \ increment the high byte SC+1 as we just crossed over
                            \ into the right half of the screen
    
     CLC                    \ Clear the C flag to avoid breaking any arithmetic
    
     BCC LI200              \ Jump back to LI200 to plot the next pixel
    
    .LI201
    
     INC SC+1               \ If we get here then we need to move down into the
     INC SC+1               \ character block below, so we increment the high byte
     LDY #248               \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the first line in that
                            \ character block (as we subtracted 248 from SC above)
    
     BNE LI210              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI211
    
     INC SC+1               \ If we get here then we need to move down into the
     INC SC+1               \ character block below, so we increment the high byte
     LDY #248               \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the first line in that
                            \ character block (as we subtracted 248 from SC above)
    
     BNE LI220              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI221
    
     INC SC+1               \ If we get here then we need to move down into the
     INC SC+1               \ character block below, so we increment the high byte
     LDY #248               \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the first line in that
                            \ character block (as we subtracted 248 from SC above)
    
     BNE LI230              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LI231
    
     INC SC+1               \ If we get here then we need to move down into the
     INC SC+1               \ character block below, so we increment the high byte
     LDY #248               \ of the screen twice (as there are two pages per screen
                            \ row) and set the pixel line to the first line in that
                            \ character block (as we subtracted 248 from SC above)
    
     BNE LI240              \ Jump back to the instruction after the BMI that called
                            \ this routine
    
    .LIEX2
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Label [LI191](loinq_part_4_of_7.md#li191) is local to this routine

[X]

Label [LI200](loinq_part_4_of_7.md#li200) is local to this routine

[X]

[X]

Label [LI201](loinq_part_4_of_7.md#li201) is local to this routine

[X]

Label [LI210](loinq_part_4_of_7.md#li210) is local to this routine

[X]

[X]

Label [LI211](loinq_part_4_of_7.md#li211) is local to this routine

[X]

Label [LI220](loinq_part_4_of_7.md#li220) is local to this routine

[X]

[X]

Label [LI221](loinq_part_4_of_7.md#li221) is local to this routine

[X]

Label [LI230](loinq_part_4_of_7.md#li230) is local to this routine

[X]

[X]

Label [LI231](loinq_part_4_of_7.md#li231) is local to this routine

[X]

Label [LI240](loinq_part_4_of_7.md#li240) is local to this routine

[X]

Label [LIEX](loinq_part_3_of_7.md#liex) in subroutine [LOINQ (Part 3 of 7)](loinq_part_3_of_7.md)

[X]

Label [LIEX2](loinq_part_4_of_7.md#liex2) is local to this routine

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

[LOINQ (Part 3 of 7)](loinq_part_3_of_7.md "Previous routine")[LOINQ (Part 5 of 7)](loinq_part_5_of_7.md "Next routine")
