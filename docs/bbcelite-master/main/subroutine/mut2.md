---
title: "The MUT2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mut2.html"
---

[MUT3](mut3.md "Previous routine")[MUT1](mut1.md "Next routine")
    
    
           Name: MUT2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (S R) = XX(1 0) and (A P) = Q * A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mut2)
     References: This subroutine is called as follows:
                 * [STARS1](stars1.md) calls MUT2
    
    
    
    
    
    * * *
    
    
     Do the following assignment, and multiplication of two signed 8-bit numbers:
    
       (S R) = XX(1 0)
       (A P) = Q * A
    
    
    
    
    .MUT2
    
     LDX XX+1               \ Set S = XX+1
     STX S
    
                            \ Fall through into MUT1 to do the following:
                            \
                            \   R = XX
                            \   (A P) = Q * A
    

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX](../workspace/zp.md#xx) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit x-coordinate

[MUT3](mut3.md "Previous routine")[MUT1](mut1.md "Next routine")
