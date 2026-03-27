---
title: "The FMLTU2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/fmltu2.html"
---

[MU11](mu11.md "Previous routine")[FMLTU](fmltu.md "Next routine")
    
    
           Name: FMLTU2                                                  [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate A = K * sin(A)
      Deep dive: [The sine, cosine and arctan tables](https://elite.bbcelite.com/deep_dives/the_sine_cosine_and_arctan_tables.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-fmltu2)
     References: This subroutine is called as follows:
                 * [CIRCLE2](circle2.md) calls FMLTU2
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       A = K * sin(A)
    
     Because this routine uses the sine lookup table SNE, we can also call this
     routine to calculate cosine multiplication. To calculate the following:
    
       A = K * cos(B)
    
     call this routine with B + 16 in the accumulator, as sin(B + 16) = cos(B).
    
    
    
    
    .FMLTU2
    
     AND #%00011111         \ Restrict A to bits 0-5 (so it's in the range 0-31)
    
     TAX                    \ Set Q = sin(A) * 256
     LDA SNE,X
     STA Q
    
     LDA K                  \ Set A to the radius in K
    
                            \ Fall through into FMLTU to do the following:
                            \
                            \   (A ?) = A * Q
                            \         = K * sin(A) * 256
                            \
                            \ which is equivalent to:
                            \
                            \   A = K * sin(A)
    

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Configuration variable [SNE](../../all/workspaces.md#sne) = &A3C0

The address of the sine lookup table, as set in elite-data.asm

[MU11](mu11.md "Previous routine")[FMLTU](fmltu.md "Next routine")
