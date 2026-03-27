---
title: "The FEED subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/feed.html"
---

[DTW8](../variable/dtw8.md "Previous routine")[MT16](mt16.md "Next routine")
    
    
           Name: FEED                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a newline
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-feed)
     References: This subroutine is called as follows:
                 * [GTDRV](gtdrv.md) calls FEED
    
    
    
    
    
    
    .FEED
    
     LDA #12                \ Set A = 12, so when we skip MT16 and fall through into
                            \ TT26, we print character 12, which is a newline
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &41, or BIT &41A9, which does nothing apart
                            \ from affect the flags
    
                            \ Fall through into TT26 (skipping MT16) to print the
                            \ newline character
    

[DTW8](../variable/dtw8.md "Previous routine")[MT16](mt16.md "Next routine")
