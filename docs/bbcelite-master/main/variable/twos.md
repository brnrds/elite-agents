---
title: "The TWOS variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/twos.html"
---

[LOIN](../subroutine/loin.md "Previous routine")[TWOS2](twos2.md "Next routine")
    
    
           Name: TWOS                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made single-pixel character row bytes for mode 1
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-twos)
     Variations: See [code variations](../../related/compare/loader/variable/twos.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md) uses TWOS
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting one-pixel points in mode 1 (the top part of the
     split screen).
    
    
    
    
    .TWOS
    
     EQUB %10001000
     EQUB %01000100
     EQUB %00100010
     EQUB %00010001
    

[LOIN](../subroutine/loin.md "Previous routine")[TWOS2](twos2.md "Next routine")
