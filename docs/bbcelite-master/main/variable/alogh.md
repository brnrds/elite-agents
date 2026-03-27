---
title: "The alogh variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/alogh.html"
---

[logL](logl.md "Previous routine")[SCTBX1](sctbx1.md "Next routine")
    
    
           Name: alogh                                                   [Show more]
           Type: Variable
       Category: Maths (Arithmetic)
        Summary: Binary antilogarithm table
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-alogh)
     Variations: See [code variations](../../related/compare/main/variable/antilog-alogh.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DVID4](../subroutine/dvid4.md) uses alogh
                 * [FMLTU](../subroutine/fmltu.md) uses alogh
                 * [LL28](../subroutine/ll28.md) uses alogh
                 * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md) uses alogh
                 * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md) uses alogh
    
    
    
    
    
    * * *
    
    
     At byte n, the table contains:
    
       2^((n / 2 + 128) / 16) / 256
    
     which equals:
    
       2^(n / 32 + 8) / 256
    
    
    
    
    .alogh
    
     FOR I%, 0, 255
    
      EQUB HI(INT(2^((I% / 2 + 128) / 16) + 0.5))
    
     NEXT
    

[logL](logl.md "Previous routine")[SCTBX1](sctbx1.md "Next routine")
