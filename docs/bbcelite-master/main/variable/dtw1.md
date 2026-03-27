---
title: "The DTW1 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/dtw1.html"
---

[BPRNT](../subroutine/bprnt.md "Previous routine")[DTW2](dtw2.md "Next routine")
    
    
           Name: DTW1                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A mask for applying the lower case part of Sentence Case to
                 extended text tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_b.md#header-dtw1)
     References: This variable is used as follows:
                 * [DETOK2](../subroutine/detok2.md) uses DTW1
                 * [MT13](../subroutine/mt13.md) uses DTW1
                 * [MT2](../subroutine/mt2.md) uses DTW1
    
    
    
    
    
    * * *
    
    
     This variable is used to change characters to lower case as part of applying
     Sentence Case to extended text tokens. It has two values:
    
       * %00100000 = apply lower case to the second letter of a word onwards
    
       * %00000000 = do not change case to lower case
    
     The default value is %00100000 (apply lower case).
    
     The flag is set to %00100000 (apply lower case) by jump token 2, {sentence
     case}, which calls routine MT2 to change the value of DTW1.
    
     The flag is set to %00000000 (do not change case to lower case) by jump token
     1, {all caps}, which calls routine MT1 to change the value of DTW1.
    
     The letter to print is OR'd with DTW1 in DETOK2, which lower-cases the letter
     by setting bit 5 (if DTW1 is %00100000). However, this OR is only done if bit
     7 of DTW2 is clear, i.e. we are printing a word, so this doesn't affect the
     first letter of the word, which remains capitalised.
    
    
    
    
    .DTW1
    
     EQUB %00100000
    

[BPRNT](../subroutine/bprnt.md "Previous routine")[DTW2](dtw2.md "Next routine")
