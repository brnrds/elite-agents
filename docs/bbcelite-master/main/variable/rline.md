---
title: "The RLINE variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/rline.html"
---

[MT26](../subroutine/mt26.md "Previous routine")[FILEPR](../subroutine/filepr.md "Next routine")
    
    
           Name: RLINE                                                   [Show more]
           Type: Variable
       Category: Text
        Summary: The OSWORD configuration block used to fetch a line of text from
                 the keyboard
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-rline)
     Variations: See [code variations](../../related/compare/main/variable/rline.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [GTDIR](../subroutine/gtdir.md) uses RLINE
                 * [GTNMEW](../subroutine/gtnmew.md) uses RLINE
                 * [MT26](../subroutine/mt26.md) uses RLINE
    
    
    
    
    
    
    .RLINE
    
     EQUW INWK+5            \ The address to store the input, so the text entered
                            \ will be stored in INWK+5 as it is typed
    
     EQUB 9                 \ Maximum line length = 9, as that's the maximum size
                            \ for a commander's name including a directory name
    
     EQUB '!'               \ Allow ASCII characters from "!" through to "{" in
     EQUB '{'               \ the input
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[MT26](../subroutine/mt26.md "Previous routine")[FILEPR](../subroutine/filepr.md "Next routine")
