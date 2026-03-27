---
title: "The LOINQ (Part 5 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/loinq_part_5_of_7.html"
---

[LOINQ (Part 4 of 7)](loinq_part_4_of_7.md "Previous routine")[LOINQ (Part 6 of 7)](loinq_part_6_of_7.md "Next routine")
    
    
           Name: LOINQ (Part 5 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a line: Line has a steep gradient, step up along y-axis
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-loinq-part-5-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/loin_part_5_of_7-loinq_part_5_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * |delta_y| >= |delta_x|
    
       * The line is closer to being vertical than horizontal
    
       * We are going to step up along the y-axis
    
       * We potentially swap coordinates to make sure Y1 >= Y2
    
    
    
    
    .STPY
    
     LDY Y1                 \ Set A = Y = Y1
     TYA
    
     LDX X1                 \ Set X = X1
    
     CPY Y2                 \ If Y1 >= Y2, jump down to LI15, as the coordinates are
     BCS LI15               \ already in the order that we want
    
     DEC SWAP               \ Otherwise decrement SWAP from 0 to &FF, to denote that
                            \ we are swapping the coordinates around
    
     LDA X2                 \ Swap the values of X1 and X2
     STA X1
     STX X2
    
     TAX                    \ Set X = X1
    
     LDA Y2                 \ Swap the values of Y1 and Y2
     STA Y1
     STY Y2
    
     TAY                    \ Set Y = A = Y1
    
    .LI15
    
                            \ By this point we know the line is vertical-ish and
                            \ Y1 >= Y2, so we're going from top to bottom as we go
                            \ from Y1 to Y2
    
     LDA ylookup,Y          \ Look up the page number of the character row that
     STA SC+1               \ contains the pixel with the y-coordinate in Y1, and
                            \ store it in the high byte of SC(1 0) at SC+1, so the
                            \ high byte of SC is set correctly for drawing our line
    
     TXA                    \ Set A = 2 * bits 2-6 of X1
     AND #%11111100         \
     ASL A                  \ and shift bit 7 of X1 into the C flag
    
     STA SC                 \ Store this value in SC, so SC(1 0) now contains the
                            \ screen address of the far left end (x-coordinate = 0)
                            \ of the horizontal pixel row that we want to draw the
                            \ start of our line on
    
     BCC P%+4               \ If bit 7 of X1 was set, so X1 > 127, increment the
     INC SC+1               \ high byte of SC(1 0) to point to the second page on
                            \ this screen row, as this page contains the right half
                            \ of the row
    
     TXA                    \ Set X = X1 mod 4, which is the horizontal pixel number
     AND #3                 \ within the character block where the line starts (as
     TAX                    \ each pixel line in the character block is 4 pixels
                            \ wide)
    
     LDA TWOS,X             \ Fetch a one-pixel byte from TWOS where pixel X is set,
     STA R                  \ and store it in R
    
                            \ The following section calculates:
                            \
                            \   P = P / Q
                            \     = |delta_x| / |delta_y|
                            \
                            \ using the log tables at logL and log to calculate:
                            \
                            \   A = log(P) - log(Q)
                            \     = log(|delta_x|) - log(|delta_y|)
                            \
                            \ by first subtracting the low bytes of the logarithms
                            \ from the table at LogL, and then subtracting the high
                            \ bytes from the table at log, before applying the
                            \ antilog to get the result of the division and putting
                            \ it in P
    
     LDX P                  \ Set X = |delta_x|
    
     BEQ LIfudge            \ If |delta_x| = 0, jump to LIfudge to return 0 as the
                            \ result of the division
    
     LDA logL,X             \ Set A = log(P) - log(Q)
     LDX Q                  \       = log(|delta_x|) - log(|delta_y|)
     SEC                    \
     SBC logL,X             \ by first subtracting the low bytes of log(P) - log(Q)
    
     LDX P                  \ And then subtracting the high bytes of log(P) - log(Q)
     LDA log,X              \ so now A contains the high byte of log(P) - log(Q)
     LDX Q
     SBC log,X
    
     BCS LIlog3             \ If the subtraction fitted into one byte and didn't
                            \ underflow, then log(P) - log(Q) < 256, so we jump to
                            \ LIlog3 to return a result of 255
    
     TAX                    \ Otherwise we set A to the A-th entry from the antilog
     LDA alogh,X            \ table so the result of the division is now in A
    
     JMP LIlog2             \ Jump to LIlog2 to return the result
    
    .LIlog3
    
     LDA #255               \ The division is very close to 1, so set A to the
                            \ closest possible answer to 256, i.e. 255
    
    .LIlog2
    
     STA P                  \ Store the result of the division in P, so we have:
                            \
                            \   P = |delta_x| / |delta_y|
    
    .LIfudge
    
     LDX Q                  \ Set X = Q
                            \       = |delta_y|
    
     BEQ LIEX7              \ If |delta_y| = 0, jump down to LIEX7 to return from
                            \ the subroutine
    
     INX                    \ Set X = Q + 1
                            \       = |delta_y| + 1
                            \
                            \ We add 1 so we can skip the first pixel plot if the
                            \ line is being drawn with swapped coordinates
    
     LDA X2                 \ Set A = X2 - X1
     SEC
     SBC X1
    
     BCS P%+6               \ If X2 >= X1 then skip the following two instructions
    
     JMP LFT                \ If X2 < X1 then jump to LFT, as we need to draw the
                            \ line to the left and down
    
    .LIEX7
    
     RTS                    \ Return from the subroutine
    

[X]

Label [LFT](loinq_part_7_of_7.md#lft) in subroutine [LOINQ (Part 7 of 7)](loinq_part_7_of_7.md)

[X]

Label [LI15](loinq_part_5_of_7.md#li15) is local to this routine

[X]

Label [LIEX7](loinq_part_5_of_7.md#liex7) is local to this routine

[X]

Label [LIfudge](loinq_part_5_of_7.md#lifudge) is local to this routine

[X]

Label [LIlog2](loinq_part_5_of_7.md#lilog2) is local to this routine

[X]

Label [LIlog3](loinq_part_5_of_7.md#lilog3) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [SWAP](../workspace/wp.md#swap) in workspace [WP](../workspace/wp.md)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[X]

Variable [TWOS](../variable/twos.md) (category: Drawing pixels)

Ready-made single-pixel character row bytes for mode 1

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

[X]

Variable [alogh](../variable/alogh.md) (category: Maths (Arithmetic))

Binary antilogarithm table

[X]

Variable [log](../variable/log.md) (category: Maths (Arithmetic))

Binary logarithm table (high byte)

[X]

Variable [logL](../variable/logl.md) (category: Maths (Arithmetic))

Binary logarithm table (low byte)

[X]

Variable [ylookup](../variable/ylookup.md) (category: Drawing pixels)

Lookup table for converting pixel y-coordinate to page number of screen address

[LOINQ (Part 4 of 7)](loinq_part_4_of_7.md "Previous routine")[LOINQ (Part 6 of 7)](loinq_part_6_of_7.md "Next routine")
