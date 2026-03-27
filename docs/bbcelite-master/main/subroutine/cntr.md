---
title: "The cntr subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/cntr.html"
---

[DVID3B2](dvid3b2.md "Previous routine")[BUMP2](bump2.md "Next routine")
    
    
           Name: cntr                                                    [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Apply damping to the pitch or roll dashboard indicator
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-cntr)
     Variations: See [code variations](../../related/compare/main/subroutine/cntr.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 2 of 16)](main_flight_loop_part_2_of_16.md) calls cntr
    
    
    
    
    
    * * *
    
    
     Apply damping to the value in X, where X ranges from 1 to 255 with 128 as the
     centre point (so X represents a position on a centre-based dashboard slider,
     such as pitch or roll). If the value is in the left-hand side of the slider
     (1-127) then it bumps the value up by 1 so it moves towards the centre, and
     if it's in the right-hand side, it reduces it by 1, also moving it towards the
     centre.
    
    
    
    
    .cntr
    
     LDA auto               \ If the docking computer is currently activated, jump
     BNE cnt2               \ to cnt2 to skip the following as we always want to
                            \ enable damping for the docking computer
    
     LDA DAMP               \ If DAMP is non-zero, then keyboard damping is not
     BNE RE1                \ enabled, so jump to RE1 to return from the subroutine
    
    .cnt2
    
     TXA                    \ If X < 128, then it's in the left-hand side of the
     BPL BUMP               \ dashboard slider, so jump to BUMP to bump it up by 1,
                            \ to move it closer to the centre
    
     DEX                    \ Otherwise X >= 128, so it's in the right-hand side
     BMI ARSR1              \ of the dashboard slider, so decrement X by 1, and if
                            \ it's still >= 128, jump to ARSR1 to return from the
                            \ subroutine, otherwise fall through to BUMP to undo
                            \ the bump and then return
    
    .BUMP
    
     INX                    \ Bump X up by 1, and if it hasn't overshot the end of
     BNE RE1                \ the dashboard slider, jump to RE1 to return from the
                            \ subroutine, otherwise fall through to REDU to drop
                            \ it down by 1 again
    
    .REDU
    
     DEX                    \ Reduce X by 1, and if we have reached 0 jump up to
     BEQ BUMP               \ BUMP to add 1, because we need the value to be in the
                            \ range 1 to 255
    
    .RE1
    
     RTS                    \ Return from the subroutine
    

[X]

Entry point [ARSR1](arctan.md#arsr1) in subroutine [ARCTAN](arctan.md) (category: Maths (Geometry))

Contains an RTS

[X]

Label [BUMP](cntr.md#bump) is local to this routine

[X]

Variable [DAMP](../workspace/up.md#damp) in workspace [UP](../workspace/up.md)

Keyboard damping configuration setting

[X]

Label [RE1](cntr.md#re1) is local to this routine

[X]

Variable [auto](../workspace/wp.md#auto) in workspace [WP](../workspace/wp.md)

Docking computer activation status

[X]

Label [cnt2](cntr.md#cnt2) is local to this routine

[DVID3B2](dvid3b2.md "Previous routine")[BUMP2](bump2.md "Next routine")
