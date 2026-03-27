---
title: "The TBRIEF subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tbrief.html"
---

[DEBRIEF](debrief.md "Previous routine")[BRIEF](brief.md "Next routine")
    
    
           Name: TBRIEF                                                  [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Start mission 3
      Deep dive: [The Trumbles mission](https://elite.bbcelite.com/deep_dives/the_trumbles_mission.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tbrief)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    \.TBRIEF                \ These instructions are commented out in the original
    \                       \ source (they are the checks for the Trumble mission,
    \LDA TP                 \ which is not present in the Master version)
    \ORA #%00010000
    \STA TP
    \
    \LDA #199
    \JSR DETOK
    \
    \JSR YESNO
    \
    \BCC BAYSTEP
    \
    \LDY #HI(50000)
    \LDX #LO(50000)
    \JSR LCASH
    \
    \INC TRIBBLE
    \
    \JMP BAY
    

[DEBRIEF](debrief.md "Previous routine")[BRIEF](brief.md "Next routine")
