---
title: "The DTW5 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/dtw5.html"
---

[DTW4](dtw4.md "Previous routine")[DTW6](dtw6.md "Next routine")
    
    
           Name: DTW5                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: The size of the justified text buffer at BUF
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_b.md#header-dtw5)
     References: This variable is used as follows:
                 * [HME2](../subroutine/hme2.md) uses DTW5
                 * [MESS](../subroutine/mess.md) uses DTW5
                 * [MT15](../subroutine/mt15.md) uses DTW5
                 * [MT17](../subroutine/mt17.md) uses DTW5
                 * [TT26](../subroutine/tt26.md) uses DTW5
    
    
    
    
    
    * * *
    
    
     When justified text is enabled by jump token 14, {justify}, during printing of
     extended text tokens, text is fed into a buffer at BUF instead of being
     printed straight away, so it can be padded out with spaces to justify the
     text. DTW5 contains the size of the buffer, so BUF + DTW5 points to the first
     free byte after the end of the buffer.
    
    
    
    
    .DTW5
    
     EQUB 0
    

[DTW4](dtw4.md "Previous routine")[DTW6](dtw6.md "Next routine")
