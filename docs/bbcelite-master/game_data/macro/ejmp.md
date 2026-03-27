---
title: "The EJMP (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/ejmp.html"
---

[ACT (Game data)](../variable/act.md "Previous routine")[ECHR (Game data)](echr.md "Next routine")
    
    
           Name: EJMP                                                    [Show more]
           Type: Macro
       Category: Text
        Summary: Macro definition for jump tokens in the extended token table
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-ejmp)
     References: This macro is used as follows:
                 * [RUTOK](../variable/rutok.md) uses EJMP
                 * [TKN1](../variable/tkn1.md) uses EJMP
    
    
    
    
    
    * * *
    
    
     The following macro is used when building the extended token table:
    
       EJMP n              Insert a jump to address n in the JMTB table
    
    
    
    * * *
    
    
     Arguments:
    
       n                    The jump number to insert into the table
    
    
    
    
    MACRO EJMP n
    
     EQUB n EOR VE
    
    ENDMACRO
    

[X]

Configuration variable [VE](../../all/elite_data.md#ve) = &57

The obfuscation byte used to hide the extended tokens table from crackers viewing the binary code

[X]

[ACT (Game data)](../variable/act.md "Previous routine")[ECHR (Game data)](echr.md "Next routine")
