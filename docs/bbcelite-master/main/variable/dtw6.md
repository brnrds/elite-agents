---
title: "The DTW6 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/dtw6.html"
---

[DTW5](dtw5.md "Previous routine")[DTW8](dtw8.md "Next routine")
    
    
           Name: DTW6                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A flag to denote whether printing in lower case is enabled for
                 extended text tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_b.md#header-dtw6)
     References: This variable is used as follows:
                 * [DETOK2](../subroutine/detok2.md) uses DTW6
                 * [MT13](../subroutine/mt13.md) uses DTW6
                 * [MT2](../subroutine/mt2.md) uses DTW6
    
    
    
    
    
    * * *
    
    
     This variable is used to indicate whether lower case is currently enabled. It
     has two values:
    
       * %10000000 = lower case is enabled
    
       * %00000000 = lower case is not enabled
    
     The default value is %00000000 (lower case is not enabled).
    
     The flag is set to %10000000 (lower case is enabled) by jump token 13 {lower
     case}, which calls routine MT10 to change the value of DTW6.
    
     The flag is set to %00000000 (lower case is not enabled) by jump token 1, {all
     caps}, and jump token 2, {sentence case}, which call routines MT1 and MT2 to
     change the value of DTW6.
    
    
    
    
    .DTW6
    
     EQUB %00000000
    

[DTW5](dtw5.md "Previous routine")[DTW8](dtw8.md "Next routine")
