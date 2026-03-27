---
title: "The ABORT2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/abort2.html"
---

[ABORT](abort.md "Previous routine")[PROJ](proj.md "Next routine")
    
    
           Name: ABORT2                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Set/unset the lock target for a missile and update the dashboard
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-abort2)
     Variations: See [code variations](../../related/compare/main/subroutine/abort2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls ABORT2
    
    
    
    
    
    * * *
    
    
     Set the lock target for the leftmost missile and update the dashboard.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The slot number of the ship to lock our missile onto, or
                            &FF to remove missile lock
    
       Y                    The new colour of the missile indicator:
    
                              * &00 = black (no missile)
    
                              * #RED2 = red (armed and locked)
    
                              * #YELLOW2 = yellow/white (armed)
    
                              * #GREEN2 = green (disarmed)
    
    
    
    
    .ABORT2
    
     STX MSTG               \ Store the target of our missile lock in MSTG
    
     LDX NOMSL              \ Call MSBAR to update the leftmost indicator in the
     JSR MSBAR              \ dashboard's missile bar, which returns with Y = 0
    
     STY MSAR               \ Set MSAR = 0 to indicate that the leftmost missile
                            \ is no longer seeking a target lock
    
     RTS                    \ Return from the subroutine
    
    .msbpars
    
     EQUB 4, 0, 0, 0, 0     \ These bytes appear to be unused (they are left over
                            \ from the 6502 Second Processor version of Elite)
    

[X]

Variable [MSAR](../workspace/wp.md#msar) in workspace [WP](../workspace/wp.md)

The targeting state of our leftmost missile

[X]

Subroutine [MSBAR](msbar.md) (category: Dashboard)

Draw a specific indicator in the dashboard's missile bar

[X]

Variable [MSTG](../workspace/zp.md#mstg) in workspace [ZP](../workspace/zp.md)

The current missile lock target

[X]

Variable [NOMSL](../workspace/wp.md#nomsl) in workspace [WP](../workspace/wp.md)

The number of missiles we have fitted (0-4)

[ABORT](abort.md "Previous routine")[PROJ](proj.md "Next routine")
