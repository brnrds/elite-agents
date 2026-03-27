---
title: "The TTX69 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ttx69.html"
---

[TT60](tt60.md "Previous routine")[TT69](tt69.md "Next routine")
    
    
           Name: TTX69                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a paragraph break
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-ttx69)
     Variations: See [code variations](../../related/compare/main/subroutine/ttx69.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT25](tt25.md) calls TTX69
    
    
    
    
    
    * * *
    
    
     Print a paragraph break (a blank line between paragraphs) by moving the cursor
     down a line, setting Sentence Case, and then printing a newline.
    
    
    
    
    .TTX69
    
     INC YC                 \ Move the text cursor down a line
    
                            \ Fall through into TT69 to set Sentence Case and print
                            \ a newline
    

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[TT60](tt60.md "Previous routine")[TT69](tt69.md "Next routine")
