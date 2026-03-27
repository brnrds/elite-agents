---
title: "The DTW3 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/dtw3.html"
---

[DTW2](dtw2.md "Previous routine")[DTW4](dtw4.md "Next routine")
    
    
           Name: DTW3                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A flag for switching between standard and extended text tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_b.md#header-dtw3)
     References: This variable is used as follows:
                 * [DETOK2](../subroutine/detok2.md) uses DTW3
                 * [MT5](../subroutine/mt5.md) uses DTW3
    
    
    
    
    
    * * *
    
    
     This variable is used to indicate whether standard or extended text tokens
     should be printed by calls to DETOK. It allows us to mix standard tokens in
     with extended tokens. It has two values:
    
       * %00000000 = print extended tokens (i.e. those in TKN1 and RUTOK)
    
       * %11111111 = print standard tokens (i.e. those in QQ18)
    
     The default value is %00000000 (extended tokens).
    
     Standard tokens are set by jump token {6}, which calls routine MT6 to change
     the value of DTW3 to %11111111.
    
     Extended tokens are set by jump token {5}, which calls routine MT5 to change
     the value of DTW3 to %00000000.
    
    
    
    
    .DTW3
    
     EQUB %00000000
    

[DTW2](dtw2.md "Previous routine")[DTW4](dtw4.md "Next routine")
