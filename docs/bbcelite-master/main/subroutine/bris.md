---
title: "The BRIS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bris.html"
---

[BRIEF](brief.md "Previous routine")[PAUSE](pause.md "Next routine")
    
    
           Name: BRIS                                                    [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Clear the screen, display "INCOMING MESSAGE" and wait for 2
                 seconds
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-bris)
     References: This subroutine is called as follows:
                 * [BRIEF](brief.md) calls BRIS
                 * [JMTB](../variable/jmtb.md) calls BRIS
    
    
    
    
    
    
    .BRIS
    
     LDA #216               \ Print extended token 216 ("{clear screen}{tab 6}{move
     JSR DETOK              \ to row 10, white, lower case}{white}{all caps}INCOMING
                            \ MESSAGE"
    
     LDY #100               \ Wait for 100/50 of a second (2 seconds) and return
     JMP DELAY              \ from the subroutine using a tail call
    

[X]

Subroutine [DELAY](delay.md) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[BRIEF](brief.md "Previous routine")[PAUSE](pause.md "Next routine")
