---
title: "The CONT (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/cont.html"
---

[TWOK (Game data)](twok.md "Previous routine")[RTOK (Game data)](rtok.md "Next routine")
    
    
           Name: CONT                                                    [Show more]
           Type: Macro
       Category: Text
        Summary: Macro definition for control codes in the recursive token table
      Deep dive: [Printing text tokens](https://elite.bbcelite.com/deep_dives/printing_text_tokens.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-cont)
     References: This macro is used as follows:
                 * [QQ18](../variable/qq18.md) uses CONT
    
    
    
    
    
    * * *
    
    
     The following macro is used when building the recursive token table:
    
       CONT n              Insert control code token {n}
    
    
    
    * * *
    
    
     Arguments:
    
       n                    The control code to insert into the table
    
    
    
    
    MACRO CONT n
    
     EQUB n EOR RE
    
    ENDMACRO
    

[X]

Configuration variable [RE](../../all/elite_data.md#re) = &23

The obfuscation byte used to hide the recursive tokens table from crackers viewing the binary code

[X]

[TWOK (Game data)](twok.md "Previous routine")[RTOK (Game data)](rtok.md "Next routine")
