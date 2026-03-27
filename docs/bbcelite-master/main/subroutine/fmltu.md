---
title: "The FMLTU subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/fmltu.html"
---

[FMLTU2](fmltu2.md "Previous routine")[MLTU2](mltu2.md "Next routine")
    
    
           Name: FMLTU                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate A = A * Q / 256
      Deep dive: [Multiplication and division using logarithms](https://elite.bbcelite.com/deep_dives/multiplication_and_division_using_logarithms.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-fmltu)
     Variations: See [code variations](../../related/compare/main/subroutine/fmltu.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOEXP](doexp.md) calls FMLTU
                 * [LL51](ll51.md) calls FMLTU
                 * [LL9 (Part 5 of 12)](ll9_part_5_of_12.md) calls FMLTU
                 * [MVEIT (Part 3 of 9)](mveit_part_3_of_9.md) calls FMLTU
                 * [PLS22](pls22.md) calls FMLTU
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of two unsigned 8-bit numbers, returning only
     the high byte of the result:
    
       (A ?) = A * Q
    
     or, to put it another way:
    
       A = A * Q / 256
    
     The advanced versions of Elite use logarithms to speed up the multiplication
     process.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is clear if A = 0, or set if we return a
                            result from one of the log tables
    
    
    
    
    .FMLTU
    
     STX P                  \ Store X in P so we can preserve it through the call to
                            \ FMLTU
    
     STA widget             \ Store A in widget, so now widget = argument A
    
     TAX                    \ Transfer A into X, so now X = argument A
    
     BEQ MU3                \ If A = 0, jump to MU3 to return a result of 0, as
                            \ 0 * Q / 256 is always 0
    
                            \ We now want to calculate La + Lq, first adding the low
                            \ bytes (from the logL table), and then the high bytes
                            \ (from the log table)
    
     LDA logL,X             \ Set A = low byte of La
                            \       = low byte of La (as we set X to A above)
    
     LDX Q                  \ Set X = Q
    
     BEQ MU3again           \ If X = 0, jump to MU3again to return a result of 0, as
                            \ A * 0 / 256 is always 0
    
     CLC                    \ Set A = A + low byte of Lq
     ADC logL,X             \       = low byte of La + low byte of Lq
    
     LDA log,X              \ Set A = high byte of Lq
    
     LDX widget             \ Set A = A + C + high byte of La
     ADC log,X              \       = high byte of Lq + high byte of La + C
                            \
                            \ so we now have:
                            \
                            \   A = high byte of (La + Lq)
    
     BCC MU3again           \ If the addition fitted into one byte and didn't carry,
                            \ then La + Lq < 256, so we jump to MU3again to return a
                            \ result of 0 and the C flag clear
    
                            \ If we get here then the C flag is set, ready for when
                            \ we return from the subroutine below
    
     TAX                    \ Otherwise La + Lq >= 256, so we return the A-th entry
     LDA alogh,X            \ from the antilog table
    
     LDX P                  \ Restore X from P so it is preserved
    
     RTS                    \ Return from the subroutine
    
    .MU3again
    
     LDA #0                 \ Set A = 0
    
    .MU3
    
                            \ If we get here then A (our result) is already 0
    
     LDX P                  \ Restore X from P so it is preserved
    
     RTS                    \ Return from the subroutine
    

[X]

Label [MU3](fmltu.md#mu3) is local to this routine

[X]

Label [MU3again](fmltu.md#mu3again) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

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

Variable [widget](../workspace/zp.md#widget) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the original argument in A in the logarithmic FMLTU and LL28 routines

[FMLTU2](fmltu2.md "Previous routine")[MLTU2](mltu2.md "Next routine")
