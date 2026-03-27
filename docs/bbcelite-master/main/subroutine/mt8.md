---
title: "The MT8 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mt8.html"
---

[MT2](mt2.md "Previous routine")[MT9](mt9.md "Next routine")
    
    
           Name: MT8                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Tab to column 6 and start a new word when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-mt8)
     Variations: See [code variations](../../related/compare/main/subroutine/mt8.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls MT8
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * XC = 6 (tab to column 6)
    
       * DTW2 = %11111111 (we are not currently printing a word)
    
    
    
    
    .MT8
    
     LDA #6                 \ Move the text cursor to column 6
     JSR DOXC
    
     LDA #%11111111         \ Set all the bits in DTW2
     STA DTW2
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [DOXC](doxc.md) (category: Text)

Move the text cursor to a specific column

[X]

Variable [DTW2](../variable/dtw2.md) (category: Text)

A flag that indicates whether we are currently printing a word

[MT2](mt2.md "Previous routine")[MT9](mt9.md "Next routine")
