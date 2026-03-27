---
title: "The csh subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/csh.html"
---

[fwl](fwl.md "Previous routine")[plf](plf.md "Next routine")
    
    
           Name: csh                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Print the current amount of cash
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-csh)
     References: This subroutine is called as follows:
                 * [TT27](tt27.md) calls csh
    
    
    
    
    
    * * *
    
    
     Print control code 0 (the current amount of cash, right-aligned to width 9,
     followed by " CR" and a newline).
    
    
    
    
    .csh
    
     LDX #3                 \ We are going to use the BPRNT routine to print out
                            \ the current amount of cash, which is stored as a
                            \ 32-bit number at location CASH. BPRNT prints out
                            \ the 32-bit number stored in K, so before we call
                            \ BPRNT, we need to copy the four bytes from CASH into
                            \ K, so first we set up a counter in X for the 4 bytes
    
    .pc1
    
     LDA CASH,X             \ Copy byte X from CASH to K
     STA K,X
    
     DEX                    \ Decrement the loop counter
    
     BPL pc1                \ Loop back for the next byte to copy
    
     LDA #9                 \ We want to print the cash amount using up to 9 digits
     STA U                  \ (including the decimal point), so store this in U
                            \ for BRPNT to take as an argument
    
     SEC                    \ We want to print the cash amount with a decimal point,
                            \ so set the C flag for BRPNT to take as an argument
    
     JSR BPRNT              \ Print the amount of cash to 9 digits with a decimal
                            \ point
    
     LDA #226               \ Print recursive token 66 (" CR") followed by a
                            \ newline by falling through into plf
    

[X]

Subroutine [BPRNT](bprnt.md) (category: Text)

Print a 32-bit number, left-padded to a specific number of digits, with an optional decimal point

[X]

Variable [CASH](../workspace/wp.md#cash) in workspace [WP](../workspace/wp.md)

Our current cash pot

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [U](../workspace/zp.md#u) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [pc1](csh.md#pc1) is local to this routine

[fwl](fwl.md "Previous routine")[plf](plf.md "Next routine")
