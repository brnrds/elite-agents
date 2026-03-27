---
title: "The MT5 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mt5.html"
---

[MT6](mt6.md "Previous routine")[MT14](mt14.md "Next routine")
    
    
           Name: MT5                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-mt5)
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls MT5
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW3 = %00000000 (print extended tokens)
    
    
    
    
    .MT5
    
     LDA #%00000000         \ Set DTW3 = %00000000, so that calls to DETOK print
     STA DTW3               \ extended tokens
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [DTW3](../variable/dtw3.md) (category: Text)

A flag for switching between standard and extended text tokens

[MT6](mt6.md "Previous routine")[MT14](mt14.md "Next routine")
