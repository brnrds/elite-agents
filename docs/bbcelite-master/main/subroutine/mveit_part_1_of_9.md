---
title: "The MVEIT (Part 1 of 9) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mveit_part_1_of_9.html"
---

[LSPUT](lsput.md "Previous routine")[MVEIT (Part 2 of 9)](mveit_part_2_of_9.md "Next routine")
    
    
           Name: MVEIT (Part 1 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Tidy the orientation vectors
      Deep dive: [Program flow of the ship-moving routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_ship-moving_routine.html)
                 [Scheduling tasks with the main loop counter](https://elite.bbcelite.com/deep_dives/scheduling_tasks_with_the_main_loop_counter.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-mveit-part-1-of-9)
     Variations: See [code variations](../../related/compare/main/subroutine/mveit_part_1_of_9.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](brief.md) calls MVEIT
                 * [ESCAPE](escape.md) calls MVEIT
                 * [Main flight loop (Part 6 of 16)](main_flight_loop_part_6_of_16.md) calls MVEIT
                 * [PAS1](pas1.md) calls MVEIT
                 * [TITLE](title.md) calls MVEIT
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Tidy the orientation vectors for one of the ship slots
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The current ship/planet/sun's data block
    
       XSAV                 The slot number of the current ship/planet/sun
    
       TYPE                 The type of the current ship/planet/sun
    
    
    
    
    .MVEIT
    
     LDA INWK+31            \ If bits 5 or 7 of ship byte #31 are set, jump to MV30
     AND #%10100000         \ as the ship is either exploding or has been killed, so
     BNE MV30               \ we don't need to tidy its orientation vectors or apply
                            \ tactics
    
     LDA MCNT               \ Fetch the main loop counter
    
     EOR XSAV               \ Fetch the slot number of the ship we are moving, EOR
     AND #15                \ with the loop counter and apply mod 15 to the result.
     BNE MV3                \ The result will be zero when "counter mod 15" matches
                            \ the slot number, so this makes sure we call TIDY 12
                            \ times every 16 main loop iterations, like this:
                            \
                            \   Iteration 0, tidy the ship in slot 0
                            \   Iteration 1, tidy the ship in slot 1
                            \   Iteration 2, tidy the ship in slot 2
                            \     ...
                            \   Iteration 11, tidy the ship in slot 11
                            \   Iteration 12, do nothing
                            \   Iteration 13, do nothing
                            \   Iteration 14, do nothing
                            \   Iteration 15, do nothing
                            \   Iteration 16, tidy the ship in slot 0
                            \     ...
                            \
                            \ and so on
    
     JSR TIDY               \ Call TIDY to tidy up the orientation vectors, to
                            \ prevent the ship from getting elongated and out of
                            \ shape due to the imprecise nature of trigonometry
                            \ in assembly language
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Label [MV3](mveit_part_2_of_9.md#mv3) in subroutine [MVEIT (Part 2 of 9)](mveit_part_2_of_9.md)

[X]

Label [MV30](mveit_part_2_of_9.md#mv30) in subroutine [MVEIT (Part 2 of 9)](mveit_part_2_of_9.md)

[X]

Subroutine [TIDY](tidy.md) (category: Maths (Geometry))

Orthonormalise the orientation vectors for a ship

[X]

Variable [XSAV](../workspace/zp.md#xsav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the X register, used in a number of places

[LSPUT](lsput.md "Previous routine")[MVEIT (Part 2 of 9)](mveit_part_2_of_9.md "Next routine")
