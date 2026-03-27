---
title: "The CNT2 (Loader) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/variable/cnt2.html"
---

[CNT (Loader)](cnt.md "Previous routine")[CNT3 (Loader)](cnt3.md "Next routine")
    
    
           Name: CNT2                                                    [Show more]
           Type: Variable
       Category: Drawing planets
        Summary: A counter for use in drawing Saturn's background stars
    
    
        Context: See this variable [in context in the source code](../../all/loader.md#header-cnt2)
     References: This variable is used as follows:
                 * [PLL1 (Part 2 of 3)](../subroutine/pll1_part_2_of_3.md) uses CNT2
    
    
    
    
    
    * * *
    
    
     Defines the number of iterations of the PLL2 loop, which draws the background
     stars on the loading screen.
    
    
    
    
    .CNT2
    
     EQUW &01DD             \ The number of iterations of the PLL2 loop (477)
    

[CNT (Loader)](cnt.md "Previous routine")[CNT3 (Loader)](cnt3.md "Next routine")
