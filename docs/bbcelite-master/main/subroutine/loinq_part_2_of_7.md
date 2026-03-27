---
title: "The LOINQ (Part 2 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/loinq_part_2_of_7.html"
---

[LOINQ (Part 1 of 7)](loinq_part_1_of_7.md "Previous routine")[LOINQ (Part 3 of 7)](loinq_part_3_of_7.md "Next routine")
    
    
           Name: LOINQ (Part 2 of 7)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a line: Line has a shallow gradient, step right along x-axis
      Deep dive: [Bresenham's line algorithm](https://elite.bbcelite.com/deep_dives/bresenhams_line_algorithm.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-loinq-part-2-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/loin_part_2_of_7-loinq_part_2_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine draws a line from (X1, Y1) to (X2, Y2). It has multiple stages.
     If we get here, then:
    
       * |delta_y| < |delta_x|
    
       * The line is closer to being horizontal than vertical
    
       * We are going to step right along the x-axis
    
       * We potentially swap coordinates to make sure X1 < X2
    
    
    
    
    .STPX
    
     LDX X1                 \ Set X = X1
    
     CPX X2                 \ If X1 < X2, jump down to LI3, as the coordinates are
     BCC LI3                \ already in the order that we want
    
     DEC SWAP               \ Otherwise decrement SWAP from 0 to &FF, to denote that
                            \ we are swapping the coordinates around
    
     LDA X2                 \ Swap the values of X1 and X2
     STA X1
     STX X2
    
     TAX                    \ Set X = X1
    
     LDA Y2                 \ Swap the values of Y1 and Y2
     LDY Y1
     STA Y1
     STY Y2
    
    .LI3
    
                            \ By this point we know the line is horizontal-ish and
                            \ X1 < X2, so we're going from left to right as we go
                            \ from X1 to X2
    
     LDY Y1                 \ Look up the page number of the character row that
     LDA ylookup,Y          \ contains the pixel with the y-coordinate in Y1, and
     STA SC+1               \ store it in SC+1, so the high byte of SC is set
                            \ correctly for drawing our line
    
     LDA Y1                 \ Set Y = Y1 mod 8, which is the pixel row within the
     AND #7                 \ character block at which we want to draw the start of
     TAY                    \ our line (as each character block has 8 rows)
    
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
    
     TXA                    \ Set R = X1 mod 4, which is the horizontal pixel number
     AND #3                 \ within the character block where the line starts (as
     STA R                  \ each pixel line in the character block is 4 pixels
                            \ wide)
    
                            \ The following section calculates:
                            \
                            \   Q = Q / P
                            \     = |delta_y| / |delta_x|
                            \
                            \ using the log tables at logL and log to calculate:
                            \
                            \   A = log(Q) - log(P)
                            \     = log(|delta_y|) - log(|delta_x|)
                            \
                            \ by first subtracting the low bytes of the logarithms
                            \ from the table at LogL, and then subtracting the high
                            \ bytes from the table at log, before applying the
                            \ antilog to get the result of the division and putting
                            \ it in Q
    
     LDX Q                  \ Set X = |delta_y|
    
     BEQ LIlog7             \ If |delta_y| = 0, jump to LIlog7 to return 0 as the
                            \ result of the division
    
     LDA logL,X             \ Set A = log(Q) - log(P)
     LDX P                  \       = log(|delta_y|) - log(|delta_x|)
     SEC                    \
     SBC logL,X             \ by first subtracting the low bytes of log(Q) - log(P)
    
     LDX Q                  \ And then subtracting the high bytes of log(Q) - log(P)
     LDA log,X              \ so now A contains the high byte of log(Q) - log(P)
     LDX P
     SBC log,X
    
     BCS LIlog5             \ If the subtraction fitted into one byte and didn't
                            \ underflow, then log(Q) - log(P) < 256, so we jump to
                            \ LIlog5 to return a result of 255
    
     TAX                    \ Otherwise we set A to the A-th entry from the antilog
     LDA alogh,X            \ table so the result of the division is now in A
    
     JMP LIlog6             \ Jump to LIlog6 to return the result
    
    .LIlog5
    
     LDA #255               \ The division is very close to 1, so set A to the
     BNE LIlog6             \ closest possible answer to 256, i.e. 255, and jump to
                            \ LIlog6 to return the result (this BNE is effectively a
                            \ JMP as A is never zero)
    
    .LIlog7
    
     LDA #0                 \ The numerator in the division is 0, so set A to 0
    
    .LIlog6
    
     STA Q                  \ Store the result of the division in Q, so we have:
                            \
                            \   Q = |delta_y| / |delta_x|
    
     LDX P                  \ Set X = P
                            \       = |delta_x|
    
     BEQ LIEXS              \ If |delta_x| = 0, return from the subroutine, as LIEXS
                            \ contains a BEQ LIEX instruction, and LIEX contains an
                            \ RTS
    
     INX                    \ Set X = P + 1
                            \       = |delta_x| + 1
                            \
                            \ We add 1 so we can skip the first pixel plot if the
                            \ line is being drawn with swapped coordinates
    
     LDA Y2                 \ If Y2 < Y1 then skip the following instruction
     CMP Y1
     BCC P%+5
    
     JMP DOWN               \ Y2 >= Y1, so jump to DOWN, as we need to draw the line
                            \ to the right and down
    

[X]

Label [DOWN](loinq_part_4_of_7.md#down) in subroutine [LOINQ (Part 4 of 7)](loinq_part_4_of_7.md)

[X]

Label [LI3](loinq_part_2_of_7.md#li3) is local to this routine

[X]

Label [LIEXS](loinq_part_3_of_7.md#liexs) in subroutine [LOINQ (Part 3 of 7)](loinq_part_3_of_7.md)

[X]

Label [LIlog5](loinq_part_2_of_7.md#lilog5) is local to this routine

[X]

Label [LIlog6](loinq_part_2_of_7.md#lilog6) is local to this routine

[X]

Label [LIlog7](loinq_part_2_of_7.md#lilog7) is local to this routine

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

[LOINQ (Part 1 of 7)](loinq_part_1_of_7.md "Previous routine")[LOINQ (Part 3 of 7)](loinq_part_3_of_7.md "Next routine")
