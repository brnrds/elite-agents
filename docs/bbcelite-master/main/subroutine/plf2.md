---
title: "The plf2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/plf2.html"
---

[STATUS](status.md "Previous routine")[MVT3](mvt3.md "Next routine")
    
    
           Name: plf2                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print text followed by a newline and indent of 6 characters
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-plf2)
     Variations: See [code variations](../../related/compare/main/subroutine/plf2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STATUS](status.md) calls plf2
    
    
    
    
    
    * * *
    
    
     Print a text token followed by a newline, and indent the next line to text
     column 6.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .plf2
    
     JSR plf                \ Print the text token in A followed by a newline
    
     LDA #6                 \ Move the text cursor to column 6
     STA XC
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Subroutine [plf](plf.md) (category: Text)

Print a text token followed by a newline

[STATUS](status.md "Previous routine")[MVT3](mvt3.md "Next routine")
