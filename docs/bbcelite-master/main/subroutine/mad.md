---
title: "The MAD subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mad.html"
---

[TAS3](tas3.md "Previous routine")[ADD](add.md "Next routine")
    
    
           Name: MAD                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A X) = Q * A + (S R)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mad)
     References: This subroutine is called as follows:
                 * [MVS4](mvs4.md) calls MAD
                 * [STARS2](stars2.md) calls MAD
                 * [TAS3](tas3.md) calls MAD
                 * [TAS4](tas4.md) calls MAD
                 * [TIS1](tis1.md) calls MAD
                 * [TIS3](tis3.md) calls MAD
    
    
    
    
    
    * * *
    
    
     Calculate
    
       (A X) = Q * A + (S R)
    
    
    
    
    .MAD
    
     JSR MULT1              \ Call MULT1 to set (A P) = Q * A
    
                            \ Fall through into ADD to do:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = Q * A + (S R)
    

[X]

Subroutine [MULT1](mult1.md) (category: Maths (Arithmetic))

Calculate (A P) = Q * A

[TAS3](tas3.md "Previous routine")[ADD](add.md "Next routine")
