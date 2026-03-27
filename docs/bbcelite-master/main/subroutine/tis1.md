---
title: "The TIS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tis1.html"
---

[ADD](add.md "Previous routine")[DV42](dv42.md "Next routine")
    
    
           Name: TIS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A ?) = (-X * A + (S R)) / 96
      Deep dive: [Shift-and-subtract division](https://elite.bbcelite.com/deep_dives/shift-and-subtract_division.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tis1)
     References: This subroutine is called as follows:
                 * [TIDY](tidy.md) calls TIS1
    
    
    
    
    
    * * *
    
    
     Calculate the following expression between sign-magnitude numbers, ignoring
     the low byte of the result:
    
       (A ?) = (-X * A + (S R)) / 96
    
     This uses the same shift-and-subtract algorithm as TIS2, just with the
     quotient A hard-coded to 96.
    
    
    
    * * *
    
    
     Returns:
    
       Q                    Gets set to the value of argument X
    
    
    
    
    .TIS1
    
     STX Q                  \ Set Q = X
    
     EOR #%10000000         \ Flip the sign bit in A
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
                            \           = X * -A + (S R)
    
    .DVID96
    
     TAX                    \ Set T to the sign bit of the result
     AND #%10000000
     STA T
    
     TXA                    \ Set A to the high byte of the result with the sign bit
     AND #%01111111         \ cleared, so (A ?) = |X * A + (S R)|
    
                            \ The following is identical to TIS2, except Q is
                            \ hard-coded to 96, so this does A = A / 96
    
     LDX #254               \ Set T1 to have bits 1-7 set, so we can rotate through
     STX T1                 \ 7 loop iterations, getting a 1 each time, and then
                            \ getting a 0 on the 8th iteration... and we can also
                            \ use T1 to catch our result bits into bit 0 each time
    
    .DVL3
    
     ASL A                  \ Shift A to the left
    
     CMP #96                \ If A < 96 skip the following subtraction
     BCC DV4
    
     SBC #96                \ Set A = A - 96
                            \
                            \ Going into this subtraction we know the C flag is
                            \ set as we passed through the BCC above, and we also
                            \ know that A >= 96, so the C flag will still be set
                            \ once we are done
    
    .DV4
    
     ROL T1                 \ Rotate the counter in T1 to the left, and catch the
                            \ result bit into bit 0 (which will be a 0 if we didn't
                            \ do the subtraction, or 1 if we did)
    
     BCS DVL3               \ If we still have set bits in T1, loop back to DVL3 to
                            \ do the next iteration of 7
    
     LDA T1                 \ Fetch the result from T1 into A
    
     ORA T                  \ Give A the sign of the result that we stored above
    
     RTS                    \ Return from the subroutine
    

[X]

Label [DV4](tis1.md#dv4) is local to this routine

[X]

Label [DVL3](tis1.md#dvl3) is local to this routine

[X]

Subroutine [MAD](mad.md) (category: Maths (Arithmetic))

Calculate (A X) = Q * A + (S R)

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[ADD](add.md "Previous routine")[DV42](dv42.md "Next routine")
