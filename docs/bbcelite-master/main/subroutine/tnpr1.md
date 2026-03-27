---
title: "The tnpr1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tnpr1.html"
---

[DVLOIN](dvloin.md "Previous routine")[tnpr](tnpr.md "Next routine")
    
    
           Name: tnpr1                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Work out if we have space for one tonne of cargo
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-tnpr1)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 8 of 16)](main_flight_loop_part_8_of_16.md) calls tnpr1
    
    
    
    
    
    * * *
    
    
     Given a market item, work out whether there is room in the cargo hold for one
     tonne of this item.
    
     For standard tonne canisters, the limit is given by the type of cargo hold we
     have, with a standard cargo hold having a capacity of 20t and an extended
     cargo bay being 35t.
    
     For items measured in kg (gold, platinum), g (gem-stones) and alien items,
     the individual limit on each of these is 200 units.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The type of market item (see QQ23 for a list of market
                            item numbers)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A = 1
    
       C flag               Returns the result:
    
                              * Set if there is no room for this item
    
                              * Clear if there is room for this item
    
    
    
    
    .tnpr1
    
     STA QQ29               \ Store the type of market item in QQ29
    
     LDA #1                 \ Set the number of units of this market item to 1
    
                            \ Fall through into tnpr to work out whether there is
                            \ room in the cargo hold for A tonnes of the item of
                            \ type QQ29
    

[X]

Variable [QQ29](../workspace/wp.md#qq29) in workspace [WP](../workspace/wp.md)

Temporary storage, used in a number of places

[DVLOIN](dvloin.md "Previous routine")[tnpr](tnpr.md "Next routine")
