---
title: "The ECBLB2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ecblb2.html"
---

[CPIXK](cpixk.md "Previous routine")[ECBLB](ecblb.md "Next routine")
    
    
           Name: ECBLB2                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Start up the E.C.M. (light up the indicator, start the countdown
                 and make the E.C.M. sound)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-ecblb2)
     Variations: See [code variations](../../related/compare/main/subroutine/ecblb2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls ECBLB2
                 * [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md) calls ECBLB2
    
    
    
    
    
    
    .ECBLB2
    
     LDA #32                \ Set the E.C.M. countdown timer in ECMA to 32
     STA ECMA
    
                            \ Fall through into ECBLB to light up the E.C.M. bulb
    

[X]

Variable [ECMA](../workspace/zp.md#ecma) in workspace [ZP](../workspace/zp.md)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[CPIXK](cpixk.md "Previous routine")[ECBLB](ecblb.md "Next routine")
