---
title: "The pr2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pr2.html"
---

[TENS](../variable/tens.md "Previous routine")[TT11](tt11.md "Next routine")
    
    
           Name: pr2                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print an 8-bit number, left-padded to 3 digits, and optional point
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-pr2)
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls pr2
                 * [fwl](fwl.md) calls pr2
                 * [tal](tal.md) calls pr2
                 * [TT210](tt210.md) calls pr2
                 * [TT25](tt25.md) calls pr2
                 * [TT151](tt151.md) calls via pr2+2
    
    
    
    
    
    * * *
    
    
     Print the 8-bit number in X to 3 digits, left-padding with spaces for numbers
     with fewer than 3 digits (so numbers < 100 are right-aligned). Optionally
     include a decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The number to print
    
       C flag               If set, include a decimal point
    
    
    
    * * *
    
    
     Other entry points:
    
       pr2+2                Print the 8-bit number in X to the number of digits in A
    
    
    
    
    .pr2
    
     LDA #3                 \ Set A to the number of digits (3)
    
     LDY #0                 \ Zero the Y register, so we can fall through into TT11
                            \ to print the 16-bit number (Y X) to 3 digits, which
                            \ effectively prints X to 3 digits as the high byte is
                            \ zero
    

[TENS](../variable/tens.md "Previous routine")[TT11](tt11.md "Next routine")
