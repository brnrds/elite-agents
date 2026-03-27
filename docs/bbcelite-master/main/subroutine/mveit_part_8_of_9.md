---
title: "The MVEIT (Part 8 of 9) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mveit_part_8_of_9.html"
---

[MVEIT (Part 7 of 9)](mveit_part_7_of_9.md "Previous routine")[MVEIT (Part 9 of 9)](mveit_part_9_of_9.md "Next routine")
    
    
           Name: MVEIT (Part 8 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Rotate ship about itself by its own pitch/roll
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
                 [Pitching and rolling by a fixed angle](https://elite.bbcelite.com/deep_dives/pitching_and_rolling_by_a_fixed_angle.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-mveit-part-8-of-9)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * If the ship we are processing is rolling or pitching itself, rotate it and
         apply damping if required
    
    
    
    
     LDA INWK+30            \ Fetch the ship's pitch counter and extract the sign
     AND #%10000000         \ into RAT2
     STA RAT2
    
     LDA INWK+30            \ Fetch the ship's pitch counter and extract the value
     AND #%01111111         \ without the sign bit into A
    
     BEQ MV8                \ If the pitch counter is 0, then jump to MV8 to skip
                            \ the following, as the ship is not pitching
    
     CMP #%01111111         \ If bits 0-6 are set in the pitch counter (i.e. the
                            \ ship's pitch is not damping down), then the C flag
                            \ will be set by this instruction
    
     SBC #0                 \ Set A = A - 0 - (1 - C), so if we are damping then we
                            \ reduce A by 1, otherwise it is unchanged
    
     ORA RAT2               \ Change bit 7 of A to the sign we saved in RAT2, so
                            \ the updated pitch counter in A retains its sign
    
     STA INWK+30            \ Store the updated pitch counter in byte #30
    
     LDX #15                \ Rotate (roofv_x, nosev_x) by a small angle (pitch)
     LDY #9
     JSR MVS5
    
     LDX #17                \ Rotate (roofv_y, nosev_y) by a small angle (pitch)
     LDY #11
     JSR MVS5
    
     LDX #19                \ Rotate (roofv_z, nosev_z) by a small angle (pitch)
     LDY #13
     JSR MVS5
    
    .MV8
    
     LDA INWK+29            \ Fetch the ship's roll counter and extract the sign
     AND #%10000000         \ into RAT2
     STA RAT2
    
     LDA INWK+29            \ Fetch the ship's roll counter and extract the value
     AND #%01111111         \ without the sign bit into A
    
     BEQ MV5                \ If the roll counter is 0, then jump to MV5 to skip the
                            \ following, as the ship is not rolling
    
     CMP #%01111111         \ If bits 0-6 are set in the roll counter (i.e. the
                            \ ship's roll is not damping down), then the C flag
                            \ will be set by this instruction
    
     SBC #0                 \ Set A = A - 0 - (1 - C), so if we are damping then we
                            \ reduce A by 1, otherwise it is unchanged
    
     ORA RAT2               \ Change bit 7 of A to the sign we saved in RAT2, so
                            \ the updated roll counter in A retains its sign
    
     STA INWK+29            \ Store the updated pitch counter in byte #29
    
     LDX #15                \ Rotate (roofv_x, sidev_x) by a small angle (roll)
     LDY #21
     JSR MVS5
    
     LDX #17                \ Rotate (roofv_y, sidev_y) by a small angle (roll)
     LDY #23
     JSR MVS5
    
     LDX #19                \ Rotate (roofv_z, sidev_z) by a small angle (roll)
     LDY #25
     JSR MVS5
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Label [MV5](mveit_part_9_of_9.md#mv5) in subroutine [MVEIT (Part 9 of 9)](mveit_part_9_of_9.md)

[X]

Label [MV8](mveit_part_8_of_9.md#mv8) is local to this routine

[X]

Subroutine [MVS5](mvs5.md) (category: Moving)

Apply a 3.6 degree pitch or roll to an orientation vector

[X]

Variable [RAT2](../workspace/zp.md#rat2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the pitch and roll signs when moving objects and stardust

[MVEIT (Part 7 of 9)](mveit_part_7_of_9.md "Previous routine")[MVEIT (Part 9 of 9)](mveit_part_9_of_9.md "Next routine")
