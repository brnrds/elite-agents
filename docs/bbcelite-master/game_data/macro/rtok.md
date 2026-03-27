---
title: "The RTOK (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/rtok.html"
---

[CONT (Game data)](cont.md "Previous routine")[QQ18 (Game data)](../variable/qq18.md "Next routine")
    
    
           Name: RTOK                                                    [Show more]
           Type: Macro
       Category: Text
        Summary: Macro definition for recursive tokens in the recursive token table
      Deep dive: [Printing text tokens](https://elite.bbcelite.com/deep_dives/printing_text_tokens.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-rtok)
     References: This macro is used as follows:
                 * [QQ18](../variable/qq18.md) uses RTOK
    
    
    
    
    
    * * *
    
    
     The following macro is used when building the recursive token table:
    
       RTOK n              Insert recursive token [n]
    
                             * Tokens 0-95 get stored as n + 160
    
                             * Tokens 128-145 get stored as n - 114
    
                             * Tokens 96-127 get stored as n
    
    
    
    * * *
    
    
     Arguments:
    
       n                    The number of the recursive token to insert into the
                            table, in the range 0 to 145
    
    
    
    
    MACRO RTOK n
    
     IF n >= 0 AND n <= 95
      t = n + 160
     ELIF n >= 128
      t = n - 114
     ELSE
      t = n
     ENDIF
    
     EQUB t EOR RE
    
    ENDMACRO
    

[X]

Configuration variable [RE](../../all/elite_data.md#re) = &23

The obfuscation byte used to hide the recursive tokens table from crackers viewing the binary code

[X]

Configuration variable [t](rtok.md#t) is local to this routine

[CONT (Game data)](cont.md "Previous routine")[QQ18 (Game data)](../variable/qq18.md "Next routine")
