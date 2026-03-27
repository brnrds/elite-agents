---
title: "The CHAR (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/char.html"
---

[SHIP_DODO (Game data)](../variable/ship_dodo.md "Previous routine")[TWOK (Game data)](twok.md "Next routine")
    
    
           Name: CHAR                                                    [Show more]
           Type: Macro
       Category: Text
        Summary: Macro definition for characters in the recursive token table
      Deep dive: [Printing text tokens](https://elite.bbcelite.com/deep_dives/printing_text_tokens.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-char)
     References: This macro is used as follows:
                 * [QQ18](../variable/qq18.md) uses CHAR
    
    
    
    
    
    * * *
    
    
     The following macro is used when building the recursive token table:
    
       CHAR 'x'            Insert ASCII character "x"
    
     To include an apostrophe, use a backtick character, as in CHAR '`'.
    
    
    
    * * *
    
    
     Arguments:
    
       'x'                  The character to insert into the table
    
    
    
    
    MACRO CHAR x
    
     IF x = '`'
       EQUB 39 EOR RE
     ELSE
       EQUB x EOR RE
     ENDIF
    
    ENDMACRO
    

[X]

Configuration variable [RE](../../all/elite_data.md#re) = &23

The obfuscation byte used to hide the recursive tokens table from crackers viewing the binary code

[X]

[SHIP_DODO (Game data)](../variable/ship_dodo.md "Previous routine")[TWOK (Game data)](twok.md "Next routine")
