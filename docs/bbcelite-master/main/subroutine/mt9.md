---
title: "The MT9 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mt9.html"
---

[MT8](mt8.md "Previous routine")[MT13](mt13.md "Next routine")
    
    
           Name: MT9                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Clear the screen and set the current view type to 1
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-mt9)
     Variations: See [code variations](../../related/compare/main/subroutine/mt9.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls MT9
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * XC = 1 (tab to column 1)
    
     before calling TT66 to clear the screen and set the view type to 1.
    
    
    
    
    .MT9
    
     LDA #1                 \ Move the text cursor to column 1
     STA XC
    
     JMP TT66               \ Jump to TT66 to clear the screen and set the current
                            \ view type to 1, returning from the subroutine using a
                            \ tail call
    

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[MT8](mt8.md "Previous routine")[MT13](mt13.md "Next routine")
