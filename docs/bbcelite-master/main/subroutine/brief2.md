---
title: "The BRIEF2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/brief2.html"
---

[PDESC](pdesc.md "Previous routine")[BRP](brp.md "Next routine")
    
    
           Name: BRIEF2                                                  [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Start mission 2
      Deep dive: [The Thargoid Plans mission](https://elite.bbcelite.com/deep_dives/the_thargoid_plans_mission.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-brief2)
     References: This subroutine is called as follows:
                 * [DOENTRY](doentry.md) calls BRIEF2
    
    
    
    
    
    
    .BRIEF2
    
     LDA TP                 \ Set bit 2 of TP to indicate mission 2 is in progress
     ORA #%00000100         \ but plans have not yet been picked up
     STA TP
    
     LDA #11                \ Set A = 11 so the call to BRP prints extended token 11
                            \ (the initial contact at the start of mission 2, asking
                            \ us to head for Ceerdi for a mission briefing)
    
                            \ Fall through into BRP to print the extended token in A
                            \ and show the Status Mode screen
    

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[PDESC](pdesc.md "Previous routine")[BRP](brp.md "Next routine")
