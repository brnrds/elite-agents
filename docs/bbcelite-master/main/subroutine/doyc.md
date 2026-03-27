---
title: "The DOYC subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/doyc.html"
---

[DOXC](doxc.md "Previous routine")[INCYC](incyc.md "Next routine")
    
    
           Name: DOYC                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Move the text cursor to a specific row
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-doyc)
     Variations: See [code variations](../../related/compare/main/subroutine/setyc-doyc.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT219](tt219.md) calls DOYC
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text row
    
    
    
    
    .DOYC
    
     STA YC                 \ Store the new text row in YC
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[DOXC](doxc.md "Previous routine")[INCYC](incyc.md "Next routine")
