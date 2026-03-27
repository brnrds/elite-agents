---
title: "The MLS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mls1.html"
---

[MLS2](mls2.md "Previous routine")[MU6](mu6.md "Next routine")
    
    
           Name: MLS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = ALP1 * A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mls1)
     References: This subroutine is called as follows:
                 * [STARS1](stars1.md) calls MLS1
                 * [STARS6](stars6.md) calls MLS1
                 * [STARS1](stars1.md) calls via MULTS-2
                 * [STARS2](stars2.md) calls via MULTS-2
                 * [STARS6](stars6.md) calls via MULTS-2
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       (A P) = ALP1 * A
    
     where ALP1 is the magnitude of the current roll angle alpha, in the range
     0-31.
    
     This routine uses an unrolled version of MU11. MU11 calculates P * X, so we
     use the same algorithm but with P set to ALP1 and X set to A. The unrolled
     version here can skip the bit tests for bits 5-7 of P as we know P < 32, so
     only 5 shifts with bit tests are needed (for bits 0-4), while the other 3
     shifts can be done without a test (for bits 5-7).
    
    
    
    * * *
    
    
     Other entry points:
    
       MULTS-2              Calculate (A P) = X * A
    
    
    
    
    .MLS1
    
     LDX ALP1               \ Set P to the roll angle alpha magnitude in ALP1
     STX P                  \ (0-31), so now we calculate P * A
    
    .MULTS
    
     TAX                    \ Set X = A, so now we can calculate P * X instead of
                            \ P * A to get our result, and we can use the algorithm
                            \ from MU11 to do that, just unrolled (as MU11 returns
                            \ P * X)
    
     AND #%10000000         \ Set T to the sign bit of A
     STA T
    
     TXA                    \ Set A = |A|
     AND #127
    
     BEQ MU6                \ If A = 0, jump to MU6 to set P(1 0) = 0 and return
                            \ from the subroutine using a tail call
    
     TAX                    \ Set T1 = X - 1
     DEX                    \
     STX T1                 \ We subtract 1 as the C flag will be set when we want
                            \ to do an addition in the loop below
    
     LDA #0                 \ Set A = 0 so we can start building the answer in A
    
     LSR P                  \ Set P = P >> 1
                            \ and C flag = bit 0 of P
    
                            \ We are now going to work our way through the bits of
                            \ P, and do a shift-add for any bits that are set,
                            \ keeping the running total in A, but instead of using a
                            \ loop like MU11, we just unroll it, starting with bit 0
    
     BCC P%+4               \ If C (i.e. the next bit from P) is set, do the
     ADC T1                 \ addition for this bit of P:
                            \
                            \   A = A + T1 + C
                            \     = A + X - 1 + 1
                            \     = A + X
    
     ROR A                  \ Shift A right to catch the next digit of our result,
                            \ which the next ROR sticks into the left end of P while
                            \ also extracting the next bit of P
    
     ROR P                  \ Add the overspill from shifting A to the right onto
                            \ the start of P, and shift P right to fetch the next
                            \ bit for the calculation into the C flag
    
     BCC P%+4               \ Repeat the shift-and-add loop for bit 1
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat the shift-and-add loop for bit 2
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat the shift-and-add loop for bit 3
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat the shift-and-add loop for bit 4
     ADC T1
     ROR A
     ROR P
    
     LSR A                  \ Just do the "shift" part for bit 5
     ROR P
    
     LSR A                  \ Just do the "shift" part for bit 6
     ROR P
    
     LSR A                  \ Just do the "shift" part for bit 7
     ROR P
    
     ORA T                  \ Give A the sign bit of the original argument A that
                            \ we put into T above
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [ALP1](../workspace/zp.md#alp1) in workspace [ZP](../workspace/zp.md)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Subroutine [MU6](mu6.md) (category: Maths (Arithmetic))

Set P(1 0) = (A A)

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MLS2](mls2.md "Previous routine")[MU6](mu6.md "Next routine")
