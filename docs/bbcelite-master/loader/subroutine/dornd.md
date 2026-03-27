---
title: "The DORND (Loader) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/subroutine/dornd.html"
---

[PLL1 (Part 3 of 3) (Loader)](pll1_part_3_of_3.md "Previous routine")[RAND (Loader)](../variable/rand.md "Next routine")
    
    
           Name: DORND                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Generate random numbers
      Deep dive: [Generating random numbers](https://elite.bbcelite.com/deep_dives/generating_random_numbers.html)
                 [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/loader.md#header-dornd)
     References: This subroutine is called as follows:
                 * [PLL1 (Part 1 of 3)](pll1_part_1_of_3.md) calls DORND
                 * [PLL1 (Part 2 of 3)](pll1_part_2_of_3.md) calls DORND
                 * [PLL1 (Part 3 of 3)](pll1_part_3_of_3.md) calls DORND
    
    
    
    
    
    * * *
    
    
     Set A and X to random numbers (though note that X is set to the random number
     that was returned in A the last time DORND was called).
    
     The C and V flags are also set randomly.
    
     This is a simplified version of the DORND routine in the main game code. It
     swaps the two calculations around and omits the ROL A instruction, but is
     otherwise very similar. See the DORND routine in the main game code for more
     details.
    
    
    
    
    .DORND
    
     LDA RAND+1             \ r1´ = r1 + r3 + C
     TAX                    \ r3´ = r1
     ADC RAND+3
     STA RAND+1
     STX RAND+3
    
     LDA RAND               \ X = r2´ = r0
     TAX                    \ A = r0´ = r0 + r2
     ADC RAND+2
     STA RAND
     STX RAND+2
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [RAND](../variable/rand.md) (category: Drawing planets)

The random number seed used for drawing Saturn

[PLL1 (Part 3 of 3) (Loader)](pll1_part_3_of_3.md "Previous routine")[RAND (Loader)](../variable/rand.md "Next routine")
