---
title: "The DTW2 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/dtw2.html"
---

[DTW1](dtw1.md "Previous routine")[DTW3](dtw3.md "Next routine")
    
    
           Name: DTW2                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A flag that indicates whether we are currently printing a word
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_b.md#header-dtw2)
     References: This variable is used as follows:
                 * [CLYNS](../subroutine/clyns.md) uses DTW2
                 * [DETOK2](../subroutine/detok2.md) uses DTW2
                 * [MT8](../subroutine/mt8.md) uses DTW2
                 * [TT26](../subroutine/tt26.md) uses DTW2
                 * [TTX66K](../subroutine/ttx66k.md) uses DTW2
    
    
    
    
    
    * * *
    
    
     This variable is used to indicate whether we are currently printing a word. It
     has two values:
    
       * 0 = we are currently printing a word
    
       * Non-zero = we are not currently printing a word
    
     The default value is %11111111 (we are not currently printing a word).
    
     The flag is set to %00000000 (we are currently printing a word) whenever a
     non-terminator character is passed to DASC for printing.
    
     The flag is set to %11111111 (we are not currently printing a word) whenever a
     terminator character (full stop, colon, carriage return, line feed, space) is
     passed to DASC for printing. It is also set to %11111111 by jump token 8,
     {tab 6}, which calls routine MT8 to change the value of DTW2, and to %10000000
     by TTX66 when we clear the screen.
    
    
    
    
    .DTW2
    
     EQUB %11111111
    

[DTW1](dtw1.md "Previous routine")[DTW3](dtw3.md "Next routine")
