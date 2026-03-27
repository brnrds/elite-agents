---
title: "The LOINQ (Part 1 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/loinq_part_1_of_7.html"
---

[CTWOS](../variable/ctwos.md "Previous routine")[LOINQ (Part 2 of 7)](loinq_part_2_of_7.md "Next routine")
    
    
           Name: LOINQ (Part 1 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a line: Calculate the line gradient in the form of deltas
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-loinq-part-1-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/loin_part_1_of_7-loinq_part_1_of_7.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOIN](loin.md) calls LOINQ
                 * [TTX66](ttx66.md) calls LOINQ
                 * [LOIN](loin.md) calls via LOINQ
                 * [TTX66](ttx66.md) calls via LOINQ
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     This stage calculates the line deltas.
    
    
    
    * * *
    
    
     Arguments:
    
       X1                   The screen x-coordinate of the start of the line
    
       Y1                   The screen y-coordinate of the start of the line
    
       X2                   The screen x-coordinate of the end of the line
    
       Y2                   The screen y-coordinate of the end of the line
    
    
    
    * * *
    
    
     Other entry points:
    
       LOINQ                Draw a one-segment line from (X1, Y1) to (X2, Y2)
    
    
    
    
    .HLOIN22
    
     JMP HLOIN3             \ This instruction doesn't appear to be used anywhere
    
                            \ In the BBC Micro cassette and disc versions of Elite,
                            \ LL30 and LOIN are synonyms for the same routine,
                            \ presumably because the two developers each had their
                            \ own line routines to start with, and then chose one of
                            \ them for the final game
                            \
                            \ In the BBC Master version, there are two different
                            \ routines: LOINQ draws a one-segment line, while LOIN
                            \ draws individual segments of multi-segment lines (the
                            \ distinction being that we switch to screen memory at
                            \ the start of LOINQ and back out again after drawing
                            \ the line, while LOIN just draws the line)
    
    .LOINQ
    
     LDA #128               \ Set S = 128, which is the starting point for the
     STA S                  \ slope error (representing half a pixel)
    
     ASL A                  \ Set SWAP = 0, as %10000000 << 1 = 0
     STA SWAP
    
     LDA X2                 \ Set A = X2 - X1
     SBC X1                 \       = delta_x
                            \
                            \ This subtraction works as the ASL A above sets the C
                            \ flag
    
     BCS LI1                \ If X2 > X1 then A is already positive and we can skip
                            \ the next three instructions
    
     EOR #%11111111         \ Negate the result in A by flipping all the bits and
     ADC #1                 \ adding 1, i.e. using two's complement to make it
                            \ positive
    
     SEC                    \ Set the C flag, ready for the subtraction below
    
    .LI1
    
     STA P                  \ Store A in P, so P = |X2 - X1|, or |delta_x|
    
     LDA Y2                 \ Set A = Y2 - Y1
     SBC Y1                 \       = delta_y
                            \
                            \ This subtraction works as we either set the C flag
                            \ above, or we skipped that SEC instruction with a BCS
    
    \BEQ HLOIN22            \ This instruction is commented out in the original
                            \ source
    
     BCS LI2                \ If Y2 > Y1 then A is already positive and we can skip
                            \ the next two instructions
    
     EOR #%11111111         \ Negate the result in A by flipping all the bits and
     ADC #1                 \ adding 1, i.e. using two's complement to make it
                            \ positive
    
    .LI2
    
     STA Q                  \ Store A in Q, so Q = |Y2 - Y1|, or |delta_y|
    
     CMP P                  \ If Q < P, jump to STPX to step along the x-axis, as
     BCC STPX               \ the line is closer to being horizontal than vertical
    
     JMP STPY               \ Otherwise Q >= P so jump to STPY to step along the
                            \ y-axis, as the line is closer to being vertical than
                            \ horizontal
    

[X]

Entry point [HLOIN3](hloin.md#hloin3) in subroutine [HLOIN](hloin.md) (category: Drawing lines)

Draw a line from (X, Y1) to (X2, Y1) in the colour given in A

[X]

Label [LI1](loinq_part_1_of_7.md#li1) is local to this routine

[X]

Label [LI2](loinq_part_1_of_7.md#li2) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [STPX](loinq_part_2_of_7.md#stpx) in subroutine [LOINQ (Part 2 of 7)](loinq_part_2_of_7.md)

[X]

Label [STPY](loinq_part_5_of_7.md#stpy) in subroutine [LOINQ (Part 5 of 7)](loinq_part_5_of_7.md)

[X]

Variable [SWAP](../workspace/wp.md#swap) in workspace [WP](../workspace/wp.md)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

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

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[CTWOS](../variable/ctwos.md "Previous routine")[LOINQ (Part 2 of 7)](loinq_part_2_of_7.md "Next routine")
