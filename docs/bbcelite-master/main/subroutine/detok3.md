---
title: "The DETOK3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/detok3.html"
---

[MT28](mt28.md "Previous routine")[DETOK](detok.md "Next routine")
    
    
           Name: DETOK3                                                  [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print an extended recursive token from the RUTOK token table
      Deep dive: [Extended system descriptions](https://elite.bbcelite.com/deep_dives/extended_system_descriptions.html)
                 [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-detok3)
     References: This subroutine is called as follows:
                 * [PDESC](pdesc.md) calls DETOK3
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The recursive token to be printed, in the range 0-255
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       Y                    Y is preserved
    
       V(1 0)               V(1 0) is preserved
    
    
    
    
    .DETOK3
    
     PHA                    \ Store A on the stack, so we can retrieve it later
    
     TAX                    \ Copy the token number from A into X
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     LDA #LO(RUTOK)         \ Set V to the low byte of RUTOK
     STA V
    
     LDA #HI(RUTOK)         \ Set A to the high byte of RUTOK
    
     BNE DTEN               \ Call DTEN to print token number X from the RUTOK
                            \ table and restore the values of A, Y and V(1 0) from
                            \ the stack, returning from the subroutine using a tail
                            \ call (this BNE is effectively a JMP as A is never
                            \ zero)
    

[X]

Entry point [DTEN](detok.md#dten) in subroutine [DETOK](detok.md) (category: Text)

Print recursive token number X from the token table pointed to by (A V), used to print tokens from the RUTOK table via calls to DETOK3

[X]

Configuration variable [RUTOK](../../all/workspaces.md#rutok) = &AF7C

The address of the extended system description token table, as set in elite-data.asm

[X]

Variable [V](../workspace/zp.md#v) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing an address pointer

[MT28](mt28.md "Previous routine")[DETOK](detok.md "Next routine")
