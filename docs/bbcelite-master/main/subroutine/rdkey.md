---
title: "The RDKEY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/rdkey.html"
---

[ZEKTRAN](zektran.md "Previous routine")[RDFIRE](rdfire.md "Next routine")
    
    
           Name: RDKEY                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard for key presses and update the key logger
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-rdkey)
     Variations: See [code variations](../../related/compare/main/subroutine/rdkey.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DK4](dk4.md) calls RDKEY
                 * [PAS1](pas1.md) calls RDKEY
                 * [PAUSE2](pause2.md) calls RDKEY
                 * [TITLE](title.md) calls RDKEY
                 * [TT217](tt217.md) calls RDKEY
                 * [DOKEY](dokey.md) calls via RDKEY-1
    
    
    
    
    
    * * *
    
    
     Scan the keyboard, starting with internal key number 16 ("Q") and working
     through the set of internal key numbers, returning the resulting key press in
     ASCII. The key logger is also updated.
    
     This routine is effectively the same as OSBYTE 122, though the OSBYTE call
     preserves A, unlike this routine.
    
    
    
    * * *
    
    
     Returns:
    
       X                    If a key is being pressed, X contains the ASCII code
                            of the key pressed, otherwise it contains 0
    
       A                    Contains the same as X
    
       Y                    Y is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       RDKEY-1              Only scan the keyboard for valid BCD key numbers
    
    
    
    
     SED                    \ Set the D flag to enter decimal mode. Because
                            \ internal key numbers are all valid BCD (Binary Coded
                            \ Decimal) numbers, setting this flag ensures we only
                            \ loop through valid key numbers
    
    .RDKEY
    
     TYA                    \ Store Y on the stack so we can retrieve it later
     PHA
    
     JSR FILLKL             \ Call FILLKL to scan the keyboard, update the key
                            \ logger and return any non-logger key presses in X
    
     PLA                    \ Retrieve the value of Y we stored above
     TAY
    
     LDA TRTB%,X            \ Fetch the internal key number for the key pressed
    
     STA KL                 \ Store the key pressed in KL
    
     TAX                    \ Copy the key value into X
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [FILLKL](fillkl.md) (category: Keyboard)

Scan the keyboard for a flight key and update the key logger

[X]

Variable [KL](../workspace/zp.md#kl) in workspace [ZP](../workspace/zp.md)

The following bytes implement a key logger that enables Elite to scan for concurrent key presses of the primary flight keys, plus a secondary flight key

[X]

Variable [TRTB%](../variable/trtb_per_cent.md) (category: Keyboard)

Translation table from internal key number to ASCII

[ZEKTRAN](zektran.md "Previous routine")[RDFIRE](rdfire.md "Next routine")
