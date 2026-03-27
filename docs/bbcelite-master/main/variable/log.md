---
title: "The log variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/log.html"
---

[FONT%](font_per_cent.md "Previous routine")[logL](logl.md "Next routine")
    
    
           Name: log                                                     [Show more]
           Type: Variable
       Category: Maths (Arithmetic)
        Summary: Binary logarithm table (high byte)
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-log)
     Variations: See [code variations](../../related/compare/main/variable/log.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DVID4](../subroutine/dvid4.md) uses log
                 * [FMLTU](../subroutine/fmltu.md) uses log
                 * [LL28](../subroutine/ll28.md) uses log
                 * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md) uses log
                 * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md) uses log
    
    
    
    
    
    * * *
    
    
     At byte n, the table contains the high byte of:
    
       &2000 * log10(n) / log10(2) = 32 * 256 * log10(n) / log10(2)
    
     where log10 is the logarithm to base 10. The change-of-base formula says that:
    
       log2(n) = log10(n) / log10(2)
    
     so byte n contains the high byte of:
    
       32 * log2(n) * 256
    
    
    
    
    .log
    
     SKIP 1
    
     FOR I%, 1, 255
    
      EQUB HI(INT(&2000 * LOG(I%) / LOG(2) + 0.5))
    
     NEXT
    
    

[FONT%](font_per_cent.md "Previous routine")[logL](logl.md "Next routine")
