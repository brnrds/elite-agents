---
title: "The DOXC subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/doxc.html"
---

[tnpr](tnpr.md "Previous routine")[DOYC](doyc.md "Next routine")
    
    
           Name: DOXC                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Move the text cursor to a specific column
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-doxc)
     Variations: See [code variations](../../related/compare/main/subroutine/setxc-doxc.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](brief.md) calls DOXC
                 * [crlf](crlf.md) calls DOXC
                 * [MT8](mt8.md) calls DOXC
                 * [TT151](tt151.md) calls DOXC
                 * [TT163](tt163.md) calls DOXC
                 * [TT210](tt210.md) calls DOXC
                 * [TT219](tt219.md) calls DOXC
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text column
    
    
    
    
    .DOXC
    
     STA XC                 \ Store the new text column in XC
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[tnpr](tnpr.md "Previous routine")[DOYC](doyc.md "Next routine")
