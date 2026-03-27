---
title: "The GCASH subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/gcash.html"
---

[MCASH](mcash.md "Previous routine")[GC2](gc2.md "Next routine")
    
    
           Name: GCASH                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (Y X) = P * Q * 4
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-gcash)
     References: This subroutine is called as follows:
                 * [TT210](tt210.md) calls GCASH
                 * [TT219](tt219.md) calls GCASH
    
    
    
    
    
    * * *
    
    
     Calculate the following multiplication of unsigned 8-bit numbers:
    
       (Y X) = P * Q * 4
    
    
    
    
    .GCASH
    
     JSR MULTU              \ Call MULTU to calculate (A P) = P * Q
    

[X]

Subroutine [MULTU](multu.md) (category: Maths (Arithmetic))

Calculate (A P) = P * Q

[MCASH](mcash.md "Previous routine")[GC2](gc2.md "Next routine")
