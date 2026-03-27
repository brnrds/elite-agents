---
title: "The DEBRIEF subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/debrief.html"
---

[DEBRIEF2](debrief2.md "Previous routine")[TBRIEF](tbrief.md "Next routine")
    
    
           Name: DEBRIEF                                                 [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Finish mission 1
      Deep dive: [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-debrief)
     Variations: See [code variations](../../related/compare/main/subroutine/debrief.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOENTRY](doentry.md) calls DEBRIEF
                 * [BRIEF](brief.md) calls via BRPS
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       BRPS                 Print the extended token in A, show the Status Mode
                            screen and return from the subroutine
    
    
    
    
    .DEBRIEF
    
     LSR TP                 \ Clear bit 0 of TP to indicate that mission 1 is no
     ASL TP                 \ longer in progress, as we have completed it
    
    \INC TALLY+1            \ This instruction is commented out in the original
                            \ source
    
     LDX #LO(50000)         \ Increase our cash reserves by the generous mission
     LDY #HI(50000)         \ reward of 5,000 CR
     JSR MCASH
    
     LDA #15                \ Set A = 15 so the call to BRP prints extended token 15
                            \ (the thank you message at the end of mission 1)
    
    .BRPS
    
     BNE BRP                \ Jump to BRP to print the extended token in A and show
                            \ the Status Mode screen, returning from the subroutine
                            \ using a tail call (this BNE is effectively a JMP as A
                            \ is never zero)
    

[X]

Subroutine [BRP](brp.md) (category: Missions)

Print an extended token and show the Status Mode screen

[X]

Subroutine [MCASH](mcash.md) (category: Maths (Arithmetic))

Add an amount of cash to the cash pot

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[DEBRIEF2](debrief2.md "Previous routine")[TBRIEF](tbrief.md "Next routine")
