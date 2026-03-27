---
title: "The var subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/var.html"
---

[TT167](tt167.md "Previous routine")[hyp1](hyp1.md "Next routine")
    
    
           Name: var                                                     [Show more]
           Type: Subroutine
       Category: Market
        Summary: Calculate QQ19+3 = economy * |economic_factor|
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-var)
     References: This subroutine is called as follows:
                 * [GVL](gvl.md) calls var
                 * [TT151](tt151.md) calls var
    
    
    
    
    
    * * *
    
    
     Set QQ19+3 = economy * |economic_factor|, given byte #1 of the market prices
     table for an item. Also sets the availability of alien items to 0.
    
     This routine forms part of the calculations for market item prices (TT151)
     and availability (GVL).
    
    
    
    * * *
    
    
     Arguments:
    
       QQ19+1               Byte #1 of the market prices table for this market item
                            (which contains the economic_factor in bits 0-5, and the
                            sign of the economic_factor in bit 7)
    
    
    
    
    .var
    
     LDA QQ19+1             \ Extract bits 0-5 from QQ19+1 into A, to get the
     AND #31                \ economic_factor without its sign, in other words:
                            \
                            \   A = |economic_factor|
    
     LDY QQ28               \ Set Y to the economy byte of the current system
    
     STA QQ19+2             \ Store A in QQ19+2
    
     CLC                    \ Clear the C flag so we can do additions below
    
     LDA #0                 \ Set AVL+16 (availability of alien items) to 0,
     STA AVL+16             \ setting A to 0 in the process
    
    .TT153
    
                            \ We now do the multiplication by doing a series of
                            \ additions in a loop, building the result in A. Each
                            \ loop adds QQ19+2 (|economic_factor|) to A, and it
                            \ loops the number of times given by the economy byte;
                            \ in other words, because A starts at 0, this sets:
                            \
                            \   A = economy * |economic_factor|
    
     DEY                    \ Decrement the economy in Y, exiting the loop when it
     BMI TT154              \ becomes negative
    
     ADC QQ19+2             \ Add QQ19+2 to A
    
     JMP TT153              \ Loop back to TT153 to do another addition
    
    .TT154
    
     STA QQ19+3             \ Store the result in QQ19+3
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [AVL](../workspace/wp.md#avl) in workspace [WP](../workspace/wp.md)

Market availability in the current system

[X]

Variable [QQ19](../workspace/zp.md#qq19) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [QQ28](../workspace/wp.md#qq28) in workspace [WP](../workspace/wp.md)

The current system's economy (0-7)

[X]

Label [TT153](var.md#tt153) is local to this routine

[X]

Label [TT154](var.md#tt154) is local to this routine

[TT167](tt167.md "Previous routine")[hyp1](hyp1.md "Next routine")
