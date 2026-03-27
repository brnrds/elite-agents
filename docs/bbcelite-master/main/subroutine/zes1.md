---
title: "The ZES1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/zes1.html"
---

[TTX66](ttx66.md "Previous routine")[ZES2](zes2.md "Next routine")
    
    
           Name: ZES1                                                    [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Zero-fill the page whose number is in X
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-zes1)
     Variations: See [code variations](../../related/compare/main/subroutine/zes1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TTX66](ttx66.md) calls ZES1
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The page we want to zero-fill
    
    
    
    
    .ZES1
    
     LDY #0                 \ If we set Y = SC = 0 and fall through into ZES2
     STY SC                 \ below, then we will zero-fill 255 bytes starting from
                            \ SC - in other words, we will zero-fill the whole of
                            \ page X
    

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[TTX66](ttx66.md "Previous routine")[ZES2](zes2.md "Next routine")
