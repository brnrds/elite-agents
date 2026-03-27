---
title: "The DEBRIEF2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/debrief2.html"
---

[BRIEF3](brief3.md "Previous routine")[DEBRIEF](debrief.md "Next routine")
    
    
           Name: DEBRIEF2                                                [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Finish mission 2
      Deep dive: [The Thargoid Plans mission](https://elite.bbcelite.com/deep_dives/the_thargoid_plans_mission.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-debrief2)
     References: This subroutine is called as follows:
                 * [DOENTRY](doentry.md) calls DEBRIEF2
    
    
    
    
    
    
    .DEBRIEF2
    
     LDA TP                 \ Set bit 2 of TP to indicate mission 2 is complete (so
     ORA #%00000100         \ both bits 2 and 3 are now set)
     STA TP
    
     LDA #2                 \ Set ENGY to 2 so our energy banks recharge at a faster
     STA ENGY               \ rate, as our mission reward is a special navy energy
                            \ unit that recharges at a rate of 3 units of energy on
                            \ each iteration of the main loop, compared to a rate of
                            \ 2 units of energy for the standard energy unit
    
     INC TALLY+1            \ Award 256 kill points for completing the mission
    
     LDA #223               \ Set A = 223 so the call to BRP prints extended token
                            \ 223 (the thank you message at the end of mission 2)
    
     BNE BRP                \ Jump to BRP to print the extended token in A and show
                            \ the Status Mode screen), returning from the subroutine
                            \ using a tail call (this BNE is effectively a JMP as A
                            \ is never zero)
    

[X]

Subroutine [BRP](brp.md) (category: Missions)

Print an extended token and show the Status Mode screen

[X]

Variable [ENGY](../workspace/wp.md#engy) in workspace [WP](../workspace/wp.md)

Energy unit

[X]

Variable [TALLY](../workspace/wp.md#tally) in workspace [WP](../workspace/wp.md)

Our combat rank

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[BRIEF3](brief3.md "Previous routine")[DEBRIEF](debrief.md "Next routine")
