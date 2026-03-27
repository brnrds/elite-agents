---
title: "The INCYC subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/incyc.html"
---

[DOYC](doyc.md "Previous routine")[TRADEMODE](trademode.md "Next routine")
    
    
           Name: INCYC                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Move the text cursor to the next row
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-incyc)
     Variations: See [code variations](../../related/compare/main/subroutine/incyc.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .INCYC
    
     INC YC                 \ Move the text cursor to the next row
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[DOYC](doyc.md "Previous routine")[TRADEMODE](trademode.md "Next routine")
