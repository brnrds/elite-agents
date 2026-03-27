---
title: "The GC2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/gc2.html"
---

[GCASH](gcash.md "Previous routine")[EQSHP](eqshp.md "Next routine")
    
    
           Name: GC2                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (Y X) = (A P) * 4
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-gc2)
     References: This subroutine is called as follows:
                 * [TT151](tt151.md) calls GC2
    
    
    
    
    
    * * *
    
    
     Calculate the following multiplication of unsigned 16-bit numbers:
    
       (Y X) = (A P) * 4
    
    
    
    
    .GC2
    
     ASL P                  \ Set (A P) = (A P) * 4
     ROL A
     ASL P
     ROL A
    
     TAY                    \ Set (Y X) = (A P)
     LDX P
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[GCASH](gcash.md "Previous routine")[EQSHP](eqshp.md "Next routine")
