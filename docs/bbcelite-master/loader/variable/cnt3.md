---
title: "The CNT3 (Loader) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/variable/cnt3.html"
---

[CNT2 (Loader)](cnt2.md "Previous routine")[ROOT (Loader)](../subroutine/root.md "Next routine")
    
    
           Name: CNT3                                                    [Show more]
           Type: Variable
       Category: Drawing planets
        Summary: A counter for use in drawing Saturn's rings
    
    
        Context: See this variable [in context in the source code](../../all/loader.md#header-cnt3)
     Variations: See [code variations](../../related/compare/loader/variable/cnt3.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [PLL1 (Part 3 of 3)](../subroutine/pll1_part_3_of_3.md) uses CNT3
    
    
    
    
    
    * * *
    
    
     Defines the number of iterations of the PLL3 loop, which draws the rings
     around the loading screen's Saturn.
    
    
    
    
    .CNT3
    
     EQUW &0333             \ The number of iterations of the PLL3 loop (819)
    

[CNT2 (Loader)](cnt2.md "Previous routine")[ROOT (Loader)](../subroutine/root.md "Next routine")
