---
title: "The MT2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mt2.html"
---

[MT1](mt1.md "Previous routine")[MT8](mt8.md "Next routine")
    
    
           Name: MT2                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to Sentence Case when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-mt2)
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls MT2
                 * [TTX66K](ttx66k.md) calls MT2
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW1 = %00100000 (apply lower case to the second letter of a word onwards)
    
       * DTW6 = %00000000 (lower case is not enabled)
    
    
    
    
    .MT2
    
     LDA #%00100000         \ Set DTW1 = %00100000
     STA DTW1
    
     LDA #00000000          \ Set DTW6 = %00000000
     STA DTW6
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [DTW1](../variable/dtw1.md) (category: Text)

A mask for applying the lower case part of Sentence Case to extended text tokens

[X]

Variable [DTW6](../variable/dtw6.md) (category: Text)

A flag to denote whether printing in lower case is enabled for extended text tokens

[MT1](mt1.md "Previous routine")[MT8](mt8.md "Next routine")
