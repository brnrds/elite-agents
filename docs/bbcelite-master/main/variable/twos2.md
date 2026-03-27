---
title: "The TWOS2 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/twos2.html"
---

[TWOS](twos.md "Previous routine")[CTWOS](ctwos.md "Next routine")
    
    
           Name: TWOS2                                                   [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made double-pixel character row bytes for mode 1
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-twos2)
     References: This variable is used as follows:
                 * [PIXEL](../subroutine/pixel.md) uses TWOS2
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting two-pixel dashes in mode 1 (the top part of the
     split screen).
    
    
    
    
    .TWOS2
    
     EQUB %11001100
     EQUB %01100110
     EQUB %00110011
     EQUB %00110011
    

[TWOS](twos.md "Previous routine")[CTWOS](ctwos.md "Next routine")
