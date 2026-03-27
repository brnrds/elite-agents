---
title: "The DTW4 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/dtw4.html"
---

[DTW3](dtw3.md "Previous routine")[DTW5](dtw5.md "Next routine")
    
    
           Name: DTW4                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: Flags that govern how justified extended text tokens are printed
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_b.md#header-dtw4)
     References: This variable is used as follows:
                 * [MESS](../subroutine/mess.md) uses DTW4
                 * [MT15](../subroutine/mt15.md) uses DTW4
                 * [TT26](../subroutine/tt26.md) uses DTW4
    
    
    
    
    
    * * *
    
    
     This variable is used to control how justified text tokens are printed as part
     of the extended text token system. There are two bits that affect justified
     text:
    
       * Bit 7: 1 = justify text
                0 = do not justify text
    
       * Bit 6: 1 = buffer the entire token before printing, including carriage
                    returns (used for in-flight messages only)
                0 = print the contents of the buffer whenever a carriage return
                    appears in the token
    
     The default value is %00000000 (do not justify text, print buffer on carriage
     return).
    
     The flag is set to %10000000 (justify text, print buffer on carriage return)
     by jump token 14, {justify}, which calls routine MT14 to change the value of
     DTW4.
    
     The flag is set to %11000000 (justify text, buffer entire token) by routine
     MESS, which prints in-flight messages.
    
     The flag is set to %00000000 (do not justify text, print buffer on carriage
     return) by jump token 15, {left align}, which calls routine MT1 to change the
     value of DTW4.
    
    
    
    
    .DTW4
    
     EQUB 0
    

[DTW3](dtw3.md "Previous routine")[DTW5](dtw5.md "Next routine")
