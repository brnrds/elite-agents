---
title: "The TKN2 variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/tkn2.html"
---

[JMTB](jmtb.md "Previous routine")[QQ16](qq16.md "Next routine")
    
    
           Name: TKN2                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: The extended two-letter token lookup table
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-tkn2)
     References: This variable is used as follows:
                 * [DETOK2](../subroutine/detok2.md) uses TKN2
                 * [MT18](../subroutine/mt18.md) uses TKN2
    
    
    
    
    
    * * *
    
    
     Two-letter token lookup table for extended tokens 215-227.
    
    
    
    
    .TKN2
    
     EQUB 12, 10            \ Token 215 = {crlf}
     EQUS "AB"              \ Token 216
     EQUS "OU"              \ Token 217
     EQUS "SE"              \ Token 218
     EQUS "IT"              \ Token 219
     EQUS "IL"              \ Token 220
     EQUS "ET"              \ Token 221
     EQUS "ST"              \ Token 222
     EQUS "ON"              \ Token 223
     EQUS "LO"              \ Token 224
     EQUS "NU"              \ Token 225
     EQUS "TH"              \ Token 226
     EQUS "NO"              \ Token 227
    

[JMTB](jmtb.md "Previous routine")[QQ16](qq16.md "Next routine")
