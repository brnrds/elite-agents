---
title: "The ECHR (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/echr.html"
---

[EJMP (Game data)](ejmp.md "Previous routine")[ETOK (Game data)](etok.md "Next routine")
    
    
           Name: ECHR                                                    [Show more]
           Type: Macro
       Category: Text
        Summary: Macro definition for characters in the extended token table
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-echr)
     References: This macro is used as follows:
                 * [RUTOK](../variable/rutok.md) uses ECHR
                 * [TKN1](../variable/tkn1.md) uses ECHR
    
    
    
    
    
    * * *
    
    
     The following macro is used when building the extended token table:
    
       ECHR 'x'            Insert ASCII character "x"
    
     To include an apostrophe, use a backtick character, as in ECHR '`'.
    
    
    
    * * *
    
    
     Arguments:
    
       'x'                  The character to insert into the table
    
    
    
    
    MACRO ECHR x
    
     IF x = '`'
      EQUB 39 EOR VE
     ELSE
      EQUB x EOR VE
     ENDIF
    
    ENDMACRO
    

[X]

Configuration variable [VE](../../all/elite_data.md#ve) = &57

The obfuscation byte used to hide the extended tokens table from crackers viewing the binary code

[X]

[EJMP (Game data)](ejmp.md "Previous routine")[ETOK (Game data)](etok.md "Next routine")
