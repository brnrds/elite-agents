---
title: "The TWOS (Loader) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/variable/twos.html"
---

[PIX (Loader)](../subroutine/pix.md "Previous routine")[CNT (Loader)](cnt.md "Next routine")
    
    
           Name: TWOS                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: Ready-made single-pixel character row bytes for mode 1
    
    
        Context: See this variable [in context in the source code](../../all/loader.md#header-twos)
     Variations: See [code variations](../../related/compare/loader/variable/twos.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [PIX](../subroutine/pix.md) uses TWOS
    
    
    
    
    
    * * *
    
    
     Ready-made bytes for plotting one-pixel points in mode 1 (the top part of the
     split screen). See the PIX routine for details.
    
    
    
    
    .TWOS
    
     EQUB %10000000
     EQUB %01000000
     EQUB %00100000
     EQUB %00010000
     EQUB %00001000
     EQUB %00000100
     EQUB %00000010
     EQUB %00000001
    

[PIX (Loader)](../subroutine/pix.md "Previous routine")[CNT (Loader)](cnt.md "Next routine")
