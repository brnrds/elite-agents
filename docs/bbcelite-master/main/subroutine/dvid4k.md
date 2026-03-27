---
title: "The DVID4K subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dvid4k.html"
---

[HAS3](has3.md "Previous routine")[cls](cls.md "Next routine")
    
    
           Name: DVID4K                                                  [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (P R) = 256 * A / Q
      Deep dive: [Shift-and-subtract division](https://elite.bbcelite.com/deep_dives/shift-and-subtract_division.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-dvid4k)
     References: This subroutine is called as follows:
                 * [HANGER](hanger.md) calls DVID4K
    
    
    
    
    
    * * *
    
    
     Calculate the following division and remainder:
    
       P = A / Q
    
       R = remainder as a fraction of Q, where 1.0 = 255
    
     Another way of saying the above is this:
    
       (P R) = 256 * A / Q
    
     This uses the same shift-and-subtract algorithm as TIS2, but this time we
     keep the remainder.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .DVID4K
    
                            \ This is an exact duplicate of the DVID4 routine, which
                            \ is also present in this source, so it isn't clear why
                            \ this duplicate exists (especially as the other version
                            \ is slightly faster, as it unrolls the loop)
    
     LDX #8                 \ Set a counter in X to count the 8 bits in A
    
     ASL A                  \ Shift A left and store in P (we will build the result
     STA P                  \ in P)
    
     LDA #0                 \ Set A = 0 for us to build a remainder
    
    .DVL4K
    
     ROL A                  \ Shift A to the left
    
     BCS DV8K               \ If the C flag is set (i.e. bit 7 of A was set) then
                            \ skip straight to the subtraction
    
     CMP Q                  \ If A < Q skip the following subtraction
     BCC DV5K
    
    .DV8K
    
     SBC Q                  \ A >= Q, so set A = A - Q
    
    .DV5K
    
     ROL P                  \ Shift P to the left, pulling the C flag into bit 0
    
     DEX                    \ Decrement the loop counter
    
     BNE DVL4K              \ Loop back for the next bit until we have done all 8
                            \ bits of P
    
     RTS                    \ Return from the subroutine
    

[X]

Label [DV5K](dvid4k.md#dv5k) is local to this routine

[X]

Label [DV8K](dvid4k.md#dv8k) is local to this routine

[X]

Label [DVL4K](dvid4k.md#dvl4k) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[HAS3](has3.md "Previous routine")[cls](cls.md "Next routine")
