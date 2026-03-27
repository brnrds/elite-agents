---
title: "The pr6 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pr6.html"
---

[ee3](ee3.md "Previous routine")[pr5](pr5.md "Next routine")
    
    
           Name: pr6                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print 16-bit number, left-padded to 5 digits, no point
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-pr6)
     References: This subroutine is called as follows:
                 * [TT25](tt25.md) calls pr6
    
    
    
    
    
    * * *
    
    
     Print the 16-bit number in (Y X) to 5 digits, left-padding with spaces for
     numbers with fewer than 3 digits (so numbers < 10000 are right-aligned),
     with no decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The low byte of the number to print
    
       Y                    The high byte of the number to print
    
    
    
    
    .pr6
    
     CLC                    \ Do not display a decimal point when printing
    
                            \ Fall through into pr5 to print X to 5 digits
    

[ee3](ee3.md "Previous routine")[pr5](pr5.md "Next routine")
