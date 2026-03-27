---
title: "The MLU1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mlu1.html"
---

[MU1](mu1.md "Previous routine")[MLU2](mlu2.md "Next routine")
    
    
           Name: MLU1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate Y1 = y_hi and (A P) = |y_hi| * Q for Y-th stardust
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mlu1)
     References: This subroutine is called as follows:
                 * [STARS1](stars1.md) calls MLU1
                 * [STARS6](stars6.md) calls MLU1
    
    
    
    
    
    * * *
    
    
     Do the following assignment, and multiply the Y-th stardust particle's
     y-coordinate with an unsigned number Q:
    
       Y1 = y_hi
    
       (A P) = |y_hi| * Q
    
    
    
    
    .MLU1
    
     LDA SY,Y               \ Set Y1 the Y-th byte of SY
     STA Y1
    
                            \ Fall through into MLU2 to calculate:
                            \
                            \   (A P) = |A| * Q
    

[X]

Variable [SY](../workspace/wp.md#sy) in workspace [WP](../workspace/wp.md)

This is where we store the y_hi coordinates for all the stardust particles

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[MU1](mu1.md "Previous routine")[MLU2](mlu2.md "Next routine")
