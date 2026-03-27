---
title: "The CNT (Loader) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/variable/cnt.html"
---

[TWOS (Loader)](twos.md "Previous routine")[CNT2 (Loader)](cnt2.md "Next routine")
    
    
           Name: CNT                                                     [Show more]
           Type: Variable
       Category: Drawing planets
        Summary: A counter for use in drawing Saturn's planetary body
    
    
        Context: See this variable [in context in the source code](../../all/loader.md#header-cnt)
     Variations: See [code variations](../../related/compare/loader/variable/cnt.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [PLL1 (Part 1 of 3)](../subroutine/pll1_part_1_of_3.md) uses CNT
    
    
    
    
    
    * * *
    
    
     Defines the number of iterations of the PLL1 loop, which draws the planet part
     of the loading screen's Saturn.
    
    
    
    
    .CNT
    
     EQUW &0300             \ The number of iterations of the PLL1 loop (768)
    

[TWOS (Loader)](twos.md "Previous routine")[CNT2 (Loader)](cnt2.md "Next routine")
