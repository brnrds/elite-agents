---
title: "The PLS6 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pls6.html"
---

[PLS5](pls5.md "Previous routine")[YESNO](yesno.md "Next routine")
    
    
           Name: PLS6                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Calculate (X K) = (A P+1 P) / (z_sign z_hi z_lo)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-pls6)
     Variations: See [code variations](../../related/compare/main/subroutine/pls6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PROJ](proj.md) calls PLS6
                 * [CHKON](chkon.md) calls via PL44
                 * [YESNO](yesno.md) calls via PL6
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       (X K) = (A P+1 P) / (z_sign z_hi z_lo)
    
     returning an overflow in the C flag if the result is >= 1024.
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The planet or sun's ship data block
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the result >= 1024, clear otherwise
    
    
    
    * * *
    
    
     Other entry points:
    
       PL44                 Clear the C flag and return from the subroutine
    
       PL6                  Contains an RTS
    
    
    
    
    .PLS6
    
     JSR DVID3B2            \ Call DVID3B2 to calculate:
                            \
                            \   K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)
    
     LDA K+3                \ Set A = |K+3| OR K+2
     AND #%01111111
     ORA K+2
    
     BNE PL21               \ If A is non-zero then the two high bytes of K(3 2 1 0)
                            \ are non-zero, so jump to PL21 to set the C flag and
                            \ return from the subroutine
    
                            \ We can now just consider K(1 0), as we know the top
                            \ two bytes of K(3 2 1 0) are both 0
    
     LDX K+1                \ Set X = K+1, so now (X K) contains the result in
                            \ K(1 0), which is the format we want to return the
                            \ result in
    
     CPX #4                 \ If the high byte of K(1 0) >= 4 then the result is
     BCS PL6                \ >= 1024, so return from the subroutine with the C flag
                            \ set to indicate an overflow (as PL6 contains an RTS)
    
     LDA K+3                \ Fetch the sign of the result from K+3 (which we know
                            \ has zeroes in bits 0-6, so this just fetches the sign)
    
    \CLC                    \ This instruction is commented out in the original
                            \ source. It would have no effect as we know the C flag
                            \ is already clear, as we skipped past the BCS above
    
     BPL PL6                \ If the sign bit is clear and the result is positive,
                            \ then the result is already correct, so return from
                            \ the subroutine with the C flag clear to indicate
                            \ success (as PL6 contains an RTS)
    
     LDA K                  \ Otherwise we need to negate the result, which we do
     EOR #%11111111         \ using two's complement, starting with the low byte:
     ADC #1                 \
     STA K                  \   K = ~K + 1
    
     TXA                    \ And then the high byte:
     EOR #%11111111         \
     ADC #0                 \   X = ~X
     TAX
    
    .PL44
    
     CLC                    \ Clear the C flag to indicate success
    
    .PL6
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [DVID3B2](dvid3b2.md) (category: Maths (Arithmetic))

Calculate K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PL21](pl21.md) (category: Drawing planets)

Return from a planet/sun-drawing routine with a failure flag

[X]

Entry point [PL6](pls6.md#pl6) in subroutine [PLS6](pls6.md) (category: Drawing planets)

Contains an RTS

[PLS5](pls5.md "Previous routine")[YESNO](yesno.md "Next routine")
