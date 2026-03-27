---
title: "The MUT1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mut1.html"
---

[MUT2](mut2.md "Previous routine")[MULT1](mult1.md "Next routine")
    
    
           Name: MUT1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate R = XX and (A P) = Q * A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mut1)
     References: This subroutine is called as follows:
                 * [STARS6](stars6.md) calls MUT1
    
    
    
    
    
    * * *
    
    
     Do the following assignment, and multiplication of two signed 8-bit numbers:
    
       R = XX
       (A P) = Q * A
    
    
    
    
    .MUT1
    
     LDX XX                 \ Set R = XX
     STX R
    
                            \ Fall through into MULT1 to do the following:
                            \
                            \   (A P) = Q * A
    

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX](../workspace/zp.md#xx) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit x-coordinate

[MUT2](mut2.md "Previous routine")[MULT1](mult1.md "Next routine")
