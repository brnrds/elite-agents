---
title: "The ZEKTRAN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/zektran.html"
---

[DKS5](dks5.md "Previous routine")[RDKEY](rdkey.md "Next routine")
    
    
           Name: ZEKTRAN                                                 [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Clear the key logger
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-zektran)
     References: This subroutine is called as follows:
                 * [BR1 (Part 1 of 2)](br1_part_1_of_2.md) calls ZEKTRAN
                 * [FILLKL](fillkl.md) calls ZEKTRAN
                 * [TITLE](title.md) calls ZEKTRAN
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    X is set to 0
    
    
    
    
    .ZEKTRAN
    
     LDA #0                 \ We want to zero the key logger buffer, so set A % 0
    
     LDX #17                \ We want to clear the 17 key logger locations from
                            \ KL to KY20, so set a counter in X
    
    .ZEKLOOP
    
     STA JSTY,X             \ Store 0 in the X-th byte of the key logger
    
     DEX                    \ Decrement the counter
    
     BNE ZEKLOOP            \ And loop back for the next key, until we have just
                            \ KL+1. We don't want to clear the first key logger
                            \ location at KL, as the keyboard table at KYTB starts
                            \ with offset 1, not 0, so KL is not technically part of
                            \ the key logger (it's actually used for logging keys
                            \ that don't appear in the keyboard table, and which
                            \ therefore don't use the key logger)
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [JSTY](../workspace/zp.md#jsty) in workspace [ZP](../workspace/zp.md)

Our current pitch rate

[X]

Label [ZEKLOOP](zektran.md#zekloop) is local to this routine

[DKS5](dks5.md "Previous routine")[RDKEY](rdkey.md "Next routine")
