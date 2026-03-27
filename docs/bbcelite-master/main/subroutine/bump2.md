---
title: "The BUMP2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bump2.html"
---

[cntr](cntr.md "Previous routine")[REDU2](redu2.md "Next routine")
    
    
           Name: BUMP2                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Bump up the value of the pitch or roll dashboard indicator
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-bump2)
     Variations: See [code variations](../../related/compare/main/subroutine/bump2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOKEY](dokey.md) calls BUMP2
                 * [REDU2](redu2.md) calls via RE2+2
    
    
    
    
    
    * * *
    
    
     Increase ("bump up") X by A, where X is either the current rate of pitch or
     the current rate of roll.
    
     The rate of pitch or roll ranges from 1 to 255 with 128 as the centre point.
     This is the amount by which the pitch or roll is currently changing, so 1
     means it is decreasing at the maximum rate, 128 means it is not changing,
     and 255 means it is increasing at the maximum rate. These values correspond
     to the line on the DC or RL indicators on the dashboard, with 1 meaning full
     left, 128 meaning the middle, and 255 meaning full right.
    
     If bumping up X would push it past 255, then X is set to 255.
    
     If keyboard auto-recentre is configured and the result is less than 128, we
     bump X up to the mid-point, 128. This is the equivalent of having a roll or
     pitch in the left half of the indicator, when increasing the roll or pitch
     should jump us straight to the mid-point.
    
    
    
    * * *
    
    
     Other entry points:
    
       RE2+2                Restore A from T and return from the subroutine
    
    
    
    
    .BUMP2
    
     STA T                  \ Store argument A in T so we can restore it later
    
     TXA                    \ Copy argument X into A
    
     CLC                    \ Clear the C flag so we can do addition without the
                            \ C flag affecting the result
    
     ADC T                  \ Set X = A = argument X + argument A
     TAX
    
     BCC RE2                \ If the C flag is clear, then we didn't overflow, so
                            \ jump to RE2 to auto-recentre and return the result
    
     LDX #255               \ We have an overflow, so set X to the maximum possible
                            \ value of 255
    
    .RE2
    
     BPL djd1               \ If X has bit 7 clear (i.e. the result < 128), then
                            \ jump to djd1 in routine REDU2 to do an auto-recentre,
                            \ if configured, because the result is on the left side
                            \ of the centre point of 128
    
                            \ Jumps to RE2+2 end up here
    
     LDA T                  \ Restore the original argument A from T into A
    
     RTS                    \ Return from the subroutine
    

[X]

Label [RE2](bump2.md#re2) is local to this routine

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Entry point [djd1](redu2.md#djd1) in subroutine [REDU2](redu2.md) (category: Dashboard)

Auto-recentre the value in X, if keyboard auto-recentre is configured

[cntr](cntr.md "Previous routine")[REDU2](redu2.md "Next routine")
