---
title: "The WSCAN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/wscan.html"
---

[VEC](../variable/vec.md "Previous routine")[DELAY](delay.md "Next routine")
    
    
           Name: WSCAN                                                   [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Implement the #wscn command (wait for the vertical sync)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-wscan)
     Variations: See [code variations](../../related/compare/main/subroutine/wscan.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DELAY](delay.md) calls WSCAN
                 * [DK4](dk4.md) calls WSCAN
                 * [TT16](tt16.md) calls WSCAN
    
    
    
    
    
    * * *
    
    
     Wait for vertical sync to occur on the video system - in other words, wait
     for the screen to start its refresh cycle, which it does 50 times a second
     (50Hz).
    
    
    
    
    .WSCAN
    
     STZ DL                 \ Set DL to 0
    
    .DELL1K                 \ This label is a duplicate of a label in the DELT
                            \ routine
                            \
                            \ In the original source this label is DELL1, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to DELL1K
    
     LDA DL                 \ Loop round these two instructions until DL is no
     BEQ DELL1K             \ longer 0 (DL gets set to 30 in the LINSCN routine,
                            \ which is run when vertical sync has occurred on the
                            \ video system, so DL will change to a non-zero value
                            \ at the start of each screen refresh)
    
     RTS                    \ Return from the subroutine
    

[X]

Label [DELL1K](wscan.md#dell1k) is local to this routine

[X]

Variable [DL](../workspace/zp.md#dl) in workspace [ZP](../workspace/zp.md)

Vertical sync flag

[VEC](../variable/vec.md "Previous routine")[DELAY](delay.md "Next routine")
