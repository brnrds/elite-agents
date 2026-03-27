---
title: "The LCASH subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/lcash.html"
---

[TT114](tt114.md "Previous routine")[MCASH](mcash.md "Next routine")
    
    
           Name: LCASH                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Subtract an amount of cash from the cash pot
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-lcash)
     References: This subroutine is called as follows:
                 * [eq](eq.md) calls LCASH
                 * [TT219](tt219.md) calls LCASH
    
    
    
    
    
    * * *
    
    
     Subtract (Y X) cash from the cash pot in CASH, but only if there is enough
     cash in the pot. As CASH is a four-byte number, this calculates:
    
       CASH(0 1 2 3) = CASH(0 1 2 3) - (0 0 Y X)
    
    
    
    * * *
    
    
     Returns:
    
       C flag               If set, there was enough cash to do the subtraction
    
                            If clear, there was not enough cash to do the
                            subtraction
    
    
    
    
    .LCASH
    
     STX T1                 \ Subtract the least significant bytes:
     LDA CASH+3             \
     SEC                    \   CASH+3 = CASH+3 - X
     SBC T1
     STA CASH+3
    
     STY T1                 \ Then the second most significant bytes:
     LDA CASH+2             \
     SBC T1                 \   CASH+2 = CASH+2 - Y
     STA CASH+2
    
     LDA CASH+1             \ Then the third most significant bytes (which are 0):
     SBC #0                 \
     STA CASH+1             \   CASH+1 = CASH+1 - 0
    
     LDA CASH               \ And finally the most significant bytes (which are 0):
     SBC #0                 \
     STA CASH               \   CASH = CASH - 0
    
     BCS TT113              \ If the C flag is set then the subtraction didn't
                            \ underflow, so the value in CASH is correct and we can
                            \ jump to TT113 to return from the subroutine with the
                            \ C flag set to indicate success (as TT113 contains an
                            \ RTS)
    
                            \ Otherwise we didn't have enough cash in CASH to
                            \ subtract (Y X) from it, so fall through into
                            \ MCASH to reverse the sum and restore the original
                            \ value in CASH, and returning with the C flag clear
    

[X]

Variable [CASH](../workspace/wp.md#cash) in workspace [WP](../workspace/wp.md)

Our current cash pot

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Entry point [TT113](mcash.md#tt113) in subroutine [MCASH](mcash.md) (category: Maths (Arithmetic))

Contains an RTS

[TT114](tt114.md "Previous routine")[MCASH](mcash.md "Next routine")
