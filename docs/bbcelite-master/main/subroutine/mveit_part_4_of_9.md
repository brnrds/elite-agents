---
title: "The MVEIT (Part 4 of 9) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mveit_part_4_of_9.html"
---

[MVEIT (Part 3 of 9)](mveit_part_3_of_9.md "Previous routine")[MVEIT (Part 5 of 9)](mveit_part_5_of_9.md "Next routine")
    
    
           Name: MVEIT (Part 4 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Apply acceleration to ship's speed as a one-off
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-mveit-part-4-of-9)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Apply acceleration to the ship's speed (if acceleration is non-zero),
         and then zero the acceleration as it's a one-off change
    
    
    
    
     LDA INWK+27            \ Set A = the ship's speed in byte #24 + the ship's
     CLC                    \ acceleration in byte #28
     ADC INWK+28
    
     BPL P%+4               \ If the result is positive, skip the following
                            \ instruction
    
     LDA #0                 \ Set A to 0 to stop the speed from going negative
    
     LDY #15                \ We now fetch byte #15 from the ship's blueprint, which
                            \ contains the ship's maximum speed, so set Y = 15 to
                            \ use as an index
    
     CMP (XX0),Y            \ If A < the ship's maximum speed, skip the following
     BCC P%+4               \ instruction
    
     LDA (XX0),Y            \ Set A to the ship's maximum speed
    
     STA INWK+27            \ We have now calculated the new ship's speed after
                            \ accelerating and keeping the speed within the ship's
                            \ limits, so store the updated speed in byte #27
    
     LDA #0                 \ We have added the ship's acceleration, so we now set
     STA INWK+28            \ it back to 0 in byte #28, as it's a one-off change
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[MVEIT (Part 3 of 9)](mveit_part_3_of_9.md "Previous routine")[MVEIT (Part 5 of 9)](mveit_part_5_of_9.md "Next routine")
