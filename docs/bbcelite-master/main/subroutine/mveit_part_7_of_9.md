---
title: "The MVEIT (Part 7 of 9) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mveit_part_7_of_9.html"
---

[MVEIT (Part 6 of 9)](mveit_part_6_of_9.md "Previous routine")[MVEIT (Part 8 of 9)](mveit_part_8_of_9.md "Next routine")
    
    
           Name: MVEIT (Part 7 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Rotate ship's orientation vectors by pitch/roll
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
                 [Pitching and rolling](https://elite.bbcelite.com/deep_dives/pitching_and_rolling.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-mveit-part-7-of-9)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Rotate the ship's orientation vectors according to our pitch and roll
    
     As with the previous step, this is all about moving the other ships rather
     than us (even though we are the one doing the moving). So we rotate the
     current ship's orientation vectors (which defines its orientation in space),
     by the angles we are "moving" the rest of the sky through (alpha and beta, our
     roll and pitch), so the ship appears to us to be stationary while we rotate.
    
    
    
    
     LDY #9                 \ Apply our pitch and roll rotations to the current
     JSR MVS4               \ ship's nosev vector
    
     LDY #15                \ Apply our pitch and roll rotations to the current
     JSR MVS4               \ ship's roofv vector
    
     LDY #21                \ Apply our pitch and roll rotations to the current
     JSR MVS4               \ ship's sidev vector
    

[X]

Subroutine [MVS4](mvs4.md) (category: Moving)

Apply pitch and roll to an orientation vector

[MVEIT (Part 6 of 9)](mveit_part_6_of_9.md "Previous routine")[MVEIT (Part 8 of 9)](mveit_part_8_of_9.md "Next routine")
