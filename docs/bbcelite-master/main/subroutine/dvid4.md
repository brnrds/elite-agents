---
title: "The DVID4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dvid4.html"
---

[DV41](dv41.md "Previous routine")[DVID3B2](dvid3b2.md "Next routine")
    
    
           Name: DVID4                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (P R) = 256 * A / Q
      Deep dive: [Shift-and-subtract division](https://elite.bbcelite.com/deep_dives/shift-and-subtract_division.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-dvid4)
     Variations: See [code variations](../../related/compare/main/subroutine/dvid4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOEXP](doexp.md) calls DVID4
                 * [SPS2](sps2.md) calls DVID4
    
    
    
    
    
    * * *
    
    
     Calculate the following division and remainder:
    
       P = A / Q
    
       R = remainder as a fraction of Q, where 1.0 = 255
    
     Another way of saying the above is this:
    
       (P R) = 256 * A / Q
    
     This uses the same shift-and-subtract algorithm as TIS2, but this time we
     keep the remainder and the loop is unrolled.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .DVID4
    
    \LDX #8                 \ This instruction is commented out in the original
                            \ source
    
     ASL A                  \ Shift A left and store in P (we will build the result
     STA P                  \ in P)
    
     LDA #0                 \ Set A = 0 for us to build a remainder
    
    \.DVL4                  \ This label is commented out in the original source
    
                            \ We now repeat the following five instruction block
                            \ eight times, one for each bit in P. In the BBC Micro
                            \ cassette and disc versions of Elite the following is
                            \ done with a loop, but it is marginally faster to
                            \ unroll the loop and have eight copies of the code,
                            \ though it does take up a bit more memory (though that
                            \ isn't a big concern when you have a 6502 Second
                            \ Processor)
    
     ROL A                  \ Shift A to the left
    
     CMP Q                  \ If A < Q skip the following subtraction
     BCC P%+4
    
     SBC Q                  \ A >= Q, so set A = A - Q
    
     ROL P                  \ Shift P to the left, pulling the C flag into bit 0
    
     ROL A                  \ Repeat for the second time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the third time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the fourth time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the fifth time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the sixth time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the seventh time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the eighth time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     LDX #0                 \ Set X = 0 so this unrolled version of DVID4 also
                            \ returns X = 0
    
     STA widget             \ This contains the code from the LL28+4 routine, so
     TAX                    \ this section is exactly equivalent to a JMP LL28+4
     BEQ LLfix22            \ call, but is slightly faster as it's been inlined
     LDA logL,X             \ (so it converts the remainder in A into an integer
     LDX Q                  \ representation of the fractional value A / Q, in R,
     SEC                    \ where 1.0 = 255, and it also clears the C flag
     SBC logL,X
     LDX widget
     LDA log,X
     LDX Q
     SBC log,X
     BCS LL222
     TAX
     LDA alogh,X
    
    .LLfix22
    
     STA R                  \ This is also part of the inline LL28+4 routine
     RTS
    
    .LL222
    
     LDA #255               \ This is also part of the inline LL28+4 routine
     STA R
     RTS
    

[X]

Label [LL222](dvid4.md#ll222) is local to this routine

[X]

Label [LLfix22](dvid4.md#llfix22) is local to this routine

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

[DV41](dv41.md "Previous routine")[DVID3B2](dvid3b2.md "Next routine")
