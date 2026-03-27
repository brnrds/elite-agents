---
title: "The SQUA2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/squa2.html"
---

[SQUA](squa.md "Previous routine")[MU1](mu1.md "Next routine")
    
    
           Name: SQUA2                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = A * A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-squa2)
     References: This subroutine is called as follows:
                 * [HITCH](hitch.md) calls SQUA2
                 * [MAS3](mas3.md) calls SQUA2
                 * [SUN (Part 1 of 4)](sun_part_1_of_4.md) calls SQUA2
                 * [SUN (Part 3 of 4)](sun_part_3_of_4.md) calls SQUA2
                 * [TT111](tt111.md) calls SQUA2
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of unsigned 8-bit numbers:
    
       (A P) = A * A
    
    
    
    
    .SQUA2
    
     STA P                  \ Copy A into P and X
     TAX
    
     BNE MU11               \ If X = 0 fall through into MU1 to return a 0,
                            \ otherwise jump to MU11 to return P * X
    

[X]

Subroutine [MU11](mu11.md) (category: Maths (Arithmetic))

Calculate (A P) = P * X

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[SQUA](squa.md "Previous routine")[MU1](mu1.md "Next routine")
