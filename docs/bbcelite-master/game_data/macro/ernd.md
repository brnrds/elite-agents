---
title: "The ERND (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/ernd.html"
---

[ETWO (Game data)](etwo.md "Previous routine")[TOKN (Game data)](tokn.md "Next routine")
    
    
           Name: ERND                                                    [Show more]
           Type: Macro
       Category: Text
        Summary: Macro definition for random tokens in the extended token table
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-ernd)
     References: This macro is used as follows:
                 * [RUTOK](../variable/rutok.md) uses ERND
                 * [TKN1](../variable/tkn1.md) uses ERND
    
    
    
    
    
    * * *
    
    
     The following macro is used when building the extended token table:
    
       ERND n              Insert recursive token [n]
    
                             * Tokens 0-123 get stored as n + 91
    
    
    
    * * *
    
    
     Arguments:
    
       n                    The number of the random token to insert into the
                            table, in the range 0 to 37
    
    
    
    
    MACRO ERND n
    
     EQUB (n + 91) EOR VE
    
    ENDMACRO
    

[X]

Configuration variable [VE](../../all/elite_data.md#ve) = &57

The obfuscation byte used to hide the extended tokens table from crackers viewing the binary code

[X]

[ETWO (Game data)](etwo.md "Previous routine")[TOKN (Game data)](tokn.md "Next routine")
