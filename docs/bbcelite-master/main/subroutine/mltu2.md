---
title: "The MLTU2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mltu2.html"
---

[FMLTU](fmltu.md "Previous routine")[MUT3](mut3.md "Next routine")
    
    
           Name: MLTU2                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P+1 P) = (A ~P) * Q
      Deep dive: [Shift-and-add multiplication](https://elite.bbcelite.com/deep_dives/shift-and-add_multiplication.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mltu2)
     References: This subroutine is called as follows:
                 * [MVEIT (Part 5 of 9)](mveit_part_5_of_9.md) calls MLTU2
                 * [MVEIT (Part 5 of 9)](mveit_part_5_of_9.md) calls via MLTU2-2
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of an unsigned 16-bit number and an unsigned
     8-bit number:
    
       (A P+1 P) = (A ~P) * Q
    
     where ~P means P EOR %11111111 (i.e. P with all its bits flipped). In other
     words, if you wanted to calculate &1234 * &56, you would:
    
       * Set A to &12
       * Set P to &34 EOR %11111111 = &CB
       * Set Q to &56
    
     before calling MLTU2.
    
     This routine is like a mash-up of MU11 and FMLTU. It uses part of FMLTU's
     inverted argument trick to work out whether or not to do an addition, and like
     MU11 it sets up a counter in X to extract bits from (P+1 P). But this time we
     extract 16 bits from (P+1 P), so the result is a 24-bit number. The core of
     the algorithm is still the shift-and-add approach explained in MULT1, just
     with more bits.
    
    
    
    * * *
    
    
     Returns:
    
       Q                    Q is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       MLTU2-2              Set Q to X, so this calculates (A P+1 P) = (A ~P) * X
    
    
    
    
     STX Q                  \ Store X in Q
    
    .MLTU2
    
     EOR #%11111111         \ Flip the bits in A and rotate right, storing the
     LSR A                  \ result in P+1, so we now calculate (P+1 P) * Q
     STA P+1
    
     LDA #0                 \ Set A = 0 so we can start building the answer in A
    
     LDX #16                \ Set up a counter in X to count the 16 bits in (P+1 P)
    
     ROR P                  \ Set P = P >> 1 with bit 7 = bit 0 of A
                            \ and C flag = bit 0 of P
    
    .MUL7
    
     BCS MU21               \ If C (i.e. the next bit from P) is set, do not do the
                            \ addition for this bit of P, and instead skip to MU21
                            \ to just do the shifts
    
     ADC Q                  \ Do the addition for this bit of P:
                            \
                            \   A = A + Q + C
                            \     = A + Q
    
     ROR A                  \ Rotate (A P+1 P) to the right, so we capture the next
     ROR P+1                \ digit of the result in P+1, and extract the next digit
     ROR P                  \ of (P+1 P) in the C flag
    
     DEX                    \ Decrement the loop counter
    
     BNE MUL7               \ Loop back for the next bit until P has been rotated
                            \ all the way
    
     RTS                    \ Return from the subroutine
    
    .MU21
    
     LSR A                  \ Shift (A P+1 P) to the right, so we capture the next
     ROR P+1                \ digit of the result in P+1, and extract the next digit
     ROR P                  \ of (P+1 P) in the C flag
    
     DEX                    \ Decrement the loop counter
    
     BNE MUL7               \ Loop back for the next bit until P has been rotated
                            \ all the way
    
     RTS                    \ Return from the subroutine
    

[X]

Label [MU21](mltu2.md#mu21) is local to this routine

[X]

Label [MUL7](mltu2.md#mul7) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[FMLTU](fmltu.md "Previous routine")[MUT3](mut3.md "Next routine")
