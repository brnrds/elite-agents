---
title: "The SPS2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sps2.html"
---

[COMPAS](compas.md "Previous routine")[SPS4](sps4.md "Next routine")
    
    
           Name: SPS2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (Y X) = A / 10
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-sps2)
     Variations: See [code variations](../../related/compare/main/subroutine/sps2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SP2](sp2.md) calls SPS2
    
    
    
    
    
    * * *
    
    
     Calculate the following, where A is a sign-magnitude 8-bit integer and the
     result is a signed 16-bit integer:
    
       (Y X) = A / 10
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .SPS2
    
     ASL A                  \ Set X = |A| * 2, and set the C flag to the sign bit of
     TAX                    \ A
    
     LDA #0                 \ Set Y to have the sign bit from A in bit 7, with the
     ROR A                  \ rest of its bits zeroed, so Y now contains the sign of
     TAY                    \ the original argument
    
     LDA #20                \ Set Q = 20
     STA Q
    
     TXA                    \ Copy X into A, so A now contains the argument A * 2
    
     JSR DVID4              \ Calculate the following:
                            \
                            \   P = A / Q
                            \     = |argument A| * 2 / 20
                            \     = |argument A| / 10
    
     LDX P                  \ Set X to the result
    
     TYA                    \ If the sign of the original argument A is negative,
     BMI LL163              \ jump to LL163 to flip the sign of the result
    
     LDY #0                 \ Set the high byte of the result to 0, as the result is
                            \ positive
    
     RTS                    \ Return from the subroutine
    
    .LL163
    
     LDY #&FF               \ The result is negative, so set the high byte to &FF
    
     TXA                    \ Flip the low byte and add 1 to get the negated low
     EOR #&FF               \ byte, using two's complement
     TAX
     INX
    
    .COR1
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [DVID4](dvid4.md) (category: Maths (Arithmetic))

Calculate (P R) = 256 * A / Q

[X]

Label [LL163](sps2.md#ll163) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[COMPAS](compas.md "Previous routine")[SPS4](sps4.md "Next routine")
