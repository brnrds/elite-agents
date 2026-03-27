---
title: "The MLS2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mls2.html"
---

[MULT3](mult3.md "Previous routine")[MLS1](mls1.md "Next routine")
    
    
           Name: MLS2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (S R) = XX(1 0) and (A P) = A * ALP1
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mls2)
     Variations: See [code variations](../../related/compare/main/subroutine/mls2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STARS1](stars1.md) calls MLS2
                 * [STARS6](stars6.md) calls MLS2
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       (S R) = XX(1 0)
    
       (A P) = A * ALP1
    
     where ALP1 is the magnitude of the current roll angle alpha, in the range
     0-31.
    
    
    
    
    .MLS2
    
     LDX XX                 \ Set (S R) = XX(1 0), starting with the low bytes
     STX R
    
     LDX XX+1               \ And then doing the high bytes
     STX S
    
                            \ Fall through into MLS1 to calculate (A P) = A * ALP1
    

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX](../workspace/zp.md#xx) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit x-coordinate

[MULT3](mult3.md "Previous routine")[MLS1](mls1.md "Next routine")
