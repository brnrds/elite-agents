---
title: "The REDU2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/redu2.html"
---

[BUMP2](bump2.md "Previous routine")[ARCTAN](arctan.md "Next routine")
    
    
           Name: REDU2                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Reduce the value of the pitch or roll dashboard indicator
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-redu2)
     Variations: See [code variations](../../related/compare/main/subroutine/redu2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOKEY](dokey.md) calls REDU2
                 * [BUMP2](bump2.md) calls via djd1
    
    
    
    
    
    * * *
    
    
     Reduce X by A, where X is either the current rate of pitch or the current
     rate of roll.
    
     The rate of pitch or roll ranges from 1 to 255 with 128 as the centre point.
     This is the amount by which the pitch or roll is currently changing, so 1
     means it is decreasing at the maximum rate, 128 means it is not changing,
     and 255 means it is increasing at the maximum rate. These values correspond
     to the line on the DC or RL indicators on the dashboard, with 1 meaning full
     left, 128 meaning the middle, and 255 meaning full right.
    
     If reducing X would bring it below 1, then X is set to 1.
    
     If keyboard auto-recentre is configured and the result is greater than 128, we
     reduce X down to the mid-point, 128. This is the equivalent of having a roll
     or pitch in the right half of the indicator, when decreasing the roll or pitch
     should jump us straight to the mid-point.
    
    
    
    * * *
    
    
     Other entry points:
    
       djd1                 Auto-recentre the value in X, if keyboard auto-recentre
                            is configured
    
    
    
    
    .REDU2
    
     STA T                  \ Store argument A in T so we can restore it later
    
     TXA                    \ Copy argument X into A
    
     SEC                    \ Set the C flag so we can do subtraction without the
                            \ C flag affecting the result
    
     SBC T                  \ Set X = A = argument X - argument A
     TAX
    
     BCS RE3                \ If the C flag is set, then we didn't underflow, so
                            \ jump to RE3 to auto-recentre and return the result
    
     LDX #1                 \ We have an underflow, so set X to the minimum possible
                            \ value, 1
    
    .RE3
    
     BPL RE2+2              \ If X has bit 7 clear (i.e. the result < 128), then
                            \ jump to RE2+2 above to return the result as is,
                            \ because the result is on the left side of the centre
                            \ point of 128, so we don't need to auto-centre
    
    .djd1
    
                            \ If we get here, then we need to apply auto-recentre,
                            \ if it is configured
    
     LDA DJD                \ If keyboard auto-recentre is disabled, then
     BNE RE2+2              \ jump to RE2+2 to restore A and return
    
     LDX #128               \ If we get here then keyboard auto-recentre is enabled,
     BMI RE2+2              \ so set X to 128 (the middle of our range) and jump to
                            \ RE2+2 to restore A and return from the subroutine
                            \ (this BMI is effectively a JMP as bit 7 of X is always
                            \ set)
    

[X]

Variable [DJD](../workspace/up.md#djd) in workspace [UP](../workspace/up.md)

Keyboard auto-recentre configuration setting

[X]

Entry point [RE2+2](bump2.md#re2) in subroutine [BUMP2](bump2.md) (category: Dashboard)

Restore A from T and return from the subroutine

[X]

Label [RE3](redu2.md#re3) is local to this routine

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[BUMP2](bump2.md "Previous routine")[ARCTAN](arctan.md "Next routine")
