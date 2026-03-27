---
title: "The logL variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/logl.html"
---

[log](log.md "Previous routine")[alogh](alogh.md "Next routine")
    
    
           Name: logL                                                    [Show more]
           Type: Variable
       Category: Maths (Arithmetic)
        Summary: Binary logarithm table (low byte)
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-logl)
     Variations: See [code variations](../../related/compare/main/variable/logl.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DVID4](../subroutine/dvid4.md) uses logL
                 * [FMLTU](../subroutine/fmltu.md) uses logL
                 * [LL28](../subroutine/ll28.md) uses logL
                 * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md) uses logL
                 * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md) uses logL
    
    
    
    
    
    * * *
    
    
     Byte n contains the low byte of:
    
       32 * log2(n) * 256
    
    
    
    
    .logL
    
     SKIP 1
    
     FOR I%, 1, 255
    
      EQUB LO(INT(&2000 * LOG(I%) / LOG(2) + 0.5))
    
     NEXT
    
    

[log](log.md "Previous routine")[alogh](alogh.md "Next routine")
