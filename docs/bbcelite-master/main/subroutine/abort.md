---
title: "The ABORT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/abort.html"
---

[NwS1](nws1.md "Previous routine")[ABORT2](abort2.md "Next routine")
    
    
           Name: ABORT                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Disarm missiles and update the dashboard indicators
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-abort)
     Variations: See [code variations](../../related/compare/main/subroutine/abort.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [FRMIS](frmis.md) calls ABORT
                 * [KILLSHP](killshp.md) calls ABORT
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls ABORT
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The new colour of the missile indicator:
    
                              * &00 = black (no missile)
    
                              * #RED2 = red (armed and locked)
    
                              * #YELLOW2 = yellow/white (armed)
    
                              * #GREEN2 = green (disarmed)
    
    
    
    
    .ABORT
    
     LDX #&FF               \ Set X to &FF, which is the value of MSTG when we have
                            \ no target lock for our missile
    
                            \ Fall through into ABORT2 to set the missile lock to
                            \ the value in X, which effectively disarms the missile
    

[NwS1](nws1.md "Previous routine")[ABORT2](abort2.md "Next routine")
