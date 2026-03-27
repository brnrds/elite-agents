---
title: "The fwl subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/fwl.html"
---

[tal](tal.md "Previous routine")[csh](csh.md "Next routine")
    
    
           Name: fwl                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Print fuel and cash levels
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-fwl)
     References: This subroutine is called as follows:
                 * [TT213](tt213.md) calls fwl
                 * [TT27](tt27.md) calls fwl
    
    
    
    
    
    * * *
    
    
     Print control code 5 ("FUEL: ", fuel level, " LIGHT YEARS", newline, "CASH:",
     control code 0).
    
    
    
    
    .fwl
    
     LDA #105               \ Print recursive token 105 ("FUEL") followed by a
     JSR TT68               \ colon
    
     LDX QQ14               \ Load the current fuel level from QQ14
    
     SEC                    \ We want to print the fuel level with a decimal point,
                            \ so set the C flag for pr2 to take as an argument
    
     JSR pr2                \ Call pr2, which prints the number in X to a width of
                            \ 3 figures (i.e. in the format x.x, which will always
                            \ be exactly 3 characters as the maximum fuel is 7.0)
    
     LDA #195               \ Print recursive token 35 ("LIGHT YEARS") followed by
     JSR plf                \ a newline
    
    .PCASH
    
     LDA #119               \ Print recursive token 119 ("CASH:" then control code
     BNE TT27               \ 0, which prints cash levels, then " CR" and newline)
    

[X]

Variable [QQ14](../workspace/wp.md#qq14) in workspace [WP](../workspace/wp.md)

Our current fuel level (0-70)

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[X]

Subroutine [TT68](tt68.md) (category: Text)

Print a text token followed by a colon

[X]

Subroutine [plf](plf.md) (category: Text)

Print a text token followed by a newline

[X]

Subroutine [pr2](pr2.md) (category: Text)

Print an 8-bit number, left-padded to 3 digits, and optional point

[tal](tal.md "Previous routine")[csh](csh.md "Next routine")
