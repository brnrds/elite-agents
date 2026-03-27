---
title: "The ETOK (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/etok.html"
---

[ECHR (Game data)](echr.md "Previous routine")[ETWO (Game data)](etwo.md "Next routine")
    
    
           Name: ETOK                                                    [Show more]
           Type: Macro
       Category: Text
        Summary: Macro definition for recursive tokens in the extended token table
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-etok)
     References: This macro is used as follows:
                 * [RUTOK](../variable/rutok.md) uses ETOK
                 * [TKN1](../variable/tkn1.md) uses ETOK
    
    
    
    
    
    * * *
    
    
     The following macro is used when building the extended token table:
    
       ETOK n              Insert extended recursive token [n]
    
    
    
    * * *
    
    
     Arguments:
    
       n                    The number of the recursive token to insert into the
                            table, in the range 129 to 214
    
    
    
    
    MACRO ETOK n
    
     EQUB n EOR VE
    
    ENDMACRO
    

[X]

Configuration variable [VE](../../all/elite_data.md#ve) = &57

The obfuscation byte used to hide the extended tokens table from crackers viewing the binary code

[X]

[ECHR (Game data)](echr.md "Previous routine")[ETWO (Game data)](etwo.md "Next routine")
