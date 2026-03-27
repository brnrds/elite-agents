---
title: "The TOKN (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/tokn.html"
---

[ERND (Game data)](ernd.md "Previous routine")[TKN1 (Game data)](../variable/tkn1.md "Next routine")
    
    
           Name: TOKN                                                    [Show more]
           Type: Macro
       Category: Text
        Summary: Macro definition for standard tokens in the extended token table
      Deep dive: [Printing text tokens](https://elite.bbcelite.com/deep_dives/printing_text_tokens.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-tokn)
     References: This macro is used as follows:
                 * [TKN1](../variable/tkn1.md) uses TOKN
    
    
    
    
    
    * * *
    
    
     The following macro is used when building the recursive token table:
    
       TOKN n              Insert recursive token [n]
    
                             * Tokens 0-95 get stored as n + 160
    
                             * Tokens 128-145 get stored as n - 114
    
                             * Tokens 96-127 get stored as n
    
    
    
    * * *
    
    
     Arguments:
    
       n                    The number of the recursive token to insert into the
                            table, in the range 0 to 145
    
    
    
    
    MACRO TOKN n
    
     IF n >= 0 AND n <= 95
      t = n + 160
     ELIF n >= 128
      t = n - 114
     ELSE
      t = n
     ENDIF
    
     EQUB t EOR VE
    
    ENDMACRO
    

[X]

Configuration variable [VE](../../all/elite_data.md#ve) = &57

The obfuscation byte used to hide the extended tokens table from crackers viewing the binary code

[X]

Configuration variable [t](rtok.md#t) in macro [RTOK](rtok.md)

[ERND (Game data)](ernd.md "Previous routine")[TKN1 (Game data)](../variable/tkn1.md "Next routine")
