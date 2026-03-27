---
title: "The BRIEF3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/brief3.html"
---

[BRP](brp.md "Previous routine")[DEBRIEF2](debrief2.md "Next routine")
    
    
           Name: BRIEF3                                                  [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Receive the briefing and plans for mission 2
      Deep dive: [The Thargoid Plans mission](https://elite.bbcelite.com/deep_dives/the_thargoid_plans_mission.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-brief3)
     References: This subroutine is called as follows:
                 * [DOENTRY](doentry.md) calls BRIEF3
    
    
    
    
    
    
    .BRIEF3
    
     LDA TP                 \ Set bits 1 and 3 of TP to indicate that mission 1 is
     AND #%11110000         \ complete, and mission 2 is in progress and the plans
     ORA #%00001010         \ have been picked up
     STA TP
    
     LDA #222               \ Set A = 222 so the call to BRP prints extended token
                            \ 222 (the briefing for mission 2 where we pick up the
                            \ plans we need to take to Birera)
    
     BNE BRP                \ Jump to BRP to print the extended token in A and show
                            \ the Status Mode screen), returning from the subroutine
                            \ using a tail call (this BNE is effectively a JMP as A
                            \ is never zero)
    

[X]

Subroutine [BRP](brp.md) (category: Missions)

Print an extended token and show the Status Mode screen

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[BRP](brp.md "Previous routine")[DEBRIEF2](debrief2.md "Next routine")
