---
title: "The SQUA2 (Loader) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/subroutine/squa2.html"
---

[RAND (Loader)](../variable/rand.md "Previous routine")[PIX (Loader)](pix.md "Next routine")
    
    
           Name: SQUA2                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = A * A
      Deep dive: [Shift-and-add multiplication](https://elite.bbcelite.com/deep_dives/shift-and-add_multiplication.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/loader.md#header-squa2)
     References: This subroutine is called as follows:
                 * [PLL1 (Part 1 of 3)](pll1_part_1_of_3.md) calls SQUA2
                 * [PLL1 (Part 2 of 3)](pll1_part_2_of_3.md) calls SQUA2
                 * [PLL1 (Part 3 of 3)](pll1_part_3_of_3.md) calls SQUA2
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of signed 8-bit numbers:
    
       (A P) = A * A
    
     This uses a similar approach to routine SQUA2 in the main game code, which
     itself uses the MU11 routine to do the multiplication. However, this version
     first ensures that A is positive, so it can support signed numbers.
    
    
    
    
    .SQUA2
    
     BPL SQUA               \ If A > 0, jump to SQUA
    
     EOR #&FF               \ Otherwise we need to negate A for the SQUA algorithm
     CLC                    \ to work, so we do this using two's complement, by
     ADC #1                 \ setting A = ~A + 1
    
    .SQUA
    
     STA Q                  \ Set Q = A and P = A
    
     STA P                  \ Set P = A
    
     LDA #0                 \ Set A = 0 so we can start building the answer in A
    
     LDY #8                 \ Set up a counter in Y to count the 8 bits in P
    
     LSR P                  \ Set P = P >> 1
                            \ and C flag = bit 0 of P
    
    .SQL1
    
     BCC SQ1                \ If C (i.e. the next bit from P) is set, do the
     CLC                    \ addition for this bit of P:
     ADC Q                  \
                            \   A = A + Q
    
    .SQ1
    
     ROR A                  \ Shift A right to catch the next digit of our result,
                            \ which the next ROR sticks into the left end of P while
                            \ also extracting the next bit of P
    
     ROR P                  \ Add the overspill from shifting A to the right onto
                            \ the start of P, and shift P right to fetch the next
                            \ bit for the calculation into the C flag
    
     DEY                    \ Decrement the loop counter
    
     BNE SQL1               \ Loop back for the next bit until P has been rotated
                            \ all the way
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [SQ1](squa2.md#sq1) is local to this routine

[X]

Label [SQL1](squa2.md#sql1) is local to this routine

[X]

Label [SQUA](squa2.md#squa) is local to this routine

[RAND (Loader)](../variable/rand.md "Previous routine")[PIX (Loader)](pix.md "Next routine")
