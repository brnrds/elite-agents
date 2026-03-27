---
title: "The MT1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mt1.html"
---

[DETOK2](detok2.md "Previous routine")[MT2](mt2.md "Next routine")
    
    
           Name: MT1                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to ALL CAPS when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-mt1)
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls MT1
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * DTW1 = %00000000 (do not change case to lower case)
    
       * DTW6 = %00000000 (lower case is not enabled)
    
    
    
    
    .MT1
    
     LDA #%00000000         \ Set A = %00000000, so when we fall through into MT2,
                            \ both DTW1 and DTW6 get set to %00000000
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &20, or BIT &20A9, which does nothing apart
                            \ from affect the flags
    

[DETOK2](detok2.md "Previous routine")[MT2](mt2.md "Next routine")
