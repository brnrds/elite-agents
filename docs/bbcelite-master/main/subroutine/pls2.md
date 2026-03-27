---
title: "The PLS2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pls2.html"
---

[PLS1](pls1.md "Previous routine")[PLS22](pls22.md "Next routine")
    
    
           Name: PLS2                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw a half-ellipse
      Deep dive: [Drawing ellipses](https://elite.bbcelite.com/deep_dives/drawing_ellipses.html)
                 [Drawing meridians and equators](https://elite.bbcelite.com/deep_dives/drawing_meridians_and_equators.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-pls2)
     References: This subroutine is called as follows:
                 * [PL9 (Part 2 of 3)](pl9_part_2_of_3.md) calls PLS2
    
    
    
    
    
    * * *
    
    
     Draw a half-ellipse, used for the planet's equator and meridian.
    
    
    
    
    .PLS2
    
     LDA #31                \ Set TGT = 31, so we only draw half an ellipse
     STA TGT
    
                            \ Fall through into PLS22 to draw the half-ellipse
    

[X]

Variable [TGT](../workspace/zp.md#tgt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used as a target value for counters when drawing explosion clouds and partial circles

[PLS1](pls1.md "Previous routine")[PLS22](pls22.md "Next routine")
