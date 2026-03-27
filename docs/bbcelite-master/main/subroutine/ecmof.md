---
title: "The ECMOF subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ecmof.html"
---

[yetanotherrts](yetanotherrts.md "Previous routine")[SFRMIS](sfrmis.md "Next routine")
    
    
           Name: ECMOF                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Switch off the E.C.M. and turn off the dashboard bulb
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-ecmof)
     Variations: See [code variations](../../related/compare/main/subroutine/ecmof.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md) calls ECMOF
                 * [RES2](res2.md) calls ECMOF
    
    
    
    
    
    
    .ECMOF
    
    IF _SNG47
    
     LDA #0                 \ Set ECMA and ECMP to 0 to indicate that no E.C.M. is
     STA ECMA               \ currently running
     STA ECMP
    
    ELIF _COMPACT
    
     STZ ECMA               \ Set ECMA and ECMP to 0 to indicate that no E.C.M. is
     STZ ECMP               \ currently running
    
    ENDIF
    
     JMP ECBLB              \ Update the E.C.M. indicator bulb on the dashboard and
                            \ return from the subroutine using a tail call
    

[X]

Subroutine [ECBLB](ecblb.md) (category: Dashboard)

Light up the E.C.M. indicator bulb ("E") on the dashboard

[X]

Variable [ECMA](../workspace/zp.md#ecma) in workspace [ZP](../workspace/zp.md)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Variable [ECMP](../workspace/wp.md#ecmp) in workspace [WP](../workspace/wp.md)

Our E.C.M. status

[yetanotherrts](yetanotherrts.md "Previous routine")[SFRMIS](sfrmis.md "Next routine")
