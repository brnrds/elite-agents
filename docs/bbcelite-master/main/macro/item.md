---
title: "The ITEM macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/macro/item.html"
---

[ou3](../subroutine/ou3.md "Previous routine")[QQ23](../variable/qq23.md "Next routine")
    
    
           Name: ITEM                                                    [Show more]
           Type: Macro
       Category: Market
        Summary: Macro definition for the market prices table
      Deep dive: [Market item prices and availability](https://elite.bbcelite.com/deep_dives/market_item_prices_and_availability.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_f.md#header-item)
     References: This macro is used as follows:
                 * [QQ23](../variable/qq23.md) uses ITEM
    
    
    
    
    
    * * *
    
    
     The following macro is used to build the market prices table:
    
       ITEM price, factor, units, quantity, mask
    
     It inserts an item into the market prices table at QQ23.
    
    
    
    * * *
    
    
     Arguments:
    
       price                Base price
    
       factor               Economic factor
    
       units                Units: "t", "g" or "k"
    
       quantity             Base quantity
    
       mask                 Fluctuations mask
    
    
    
    
    MACRO ITEM price, factor, units, quantity, mask
    
     IF factor < 0
      s = 1 << 7
     ELSE
      s = 0
     ENDIF
    
     IF units = 't'
      u = 0
     ELIF units = 'k'
      u = 1 << 5
     ELSE
      u = 1 << 6
     ENDIF
    
     e = ABS(factor)
    
     EQUB price
     EQUB s + u + e
     EQUB quantity
     EQUB mask
    
    ENDMACRO
    

[X]

Configuration variable [e](item.md#e) is local to this routine

[X]

[X]

[X]

[X]

Configuration variable [s](item.md#s) is local to this routine

[X]

Configuration variable [u](item.md#u) is local to this routine

[ou3](../subroutine/ou3.md "Previous routine")[QQ23](../variable/qq23.md "Next routine")
