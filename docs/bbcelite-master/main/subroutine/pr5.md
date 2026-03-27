---
title: "The pr5 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pr5.html"
---

[pr6](pr6.md "Previous routine")[TT147](tt147.md "Next routine")
    
    
           Name: pr5                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a 16-bit number, left-padded to 5 digits, and optional point
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-pr5)
     References: This subroutine is called as follows:
                 * [TT146](tt146.md) calls pr5
                 * [TT151](tt151.md) calls pr5
                 * [TT25](tt25.md) calls pr5
    
    
    
    
    
    * * *
    
    
     Print the 16-bit number in (Y X) to 5 digits, left-padding with spaces for
     numbers with fewer than 3 digits (so numbers < 10000 are right-aligned).
     Optionally include a decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The low byte of the number to print
    
       Y                    The high byte of the number to print
    
       C flag               If set, include a decimal point
    
    
    
    
    .pr5
    
     LDA #5                 \ Set the number of digits to print to 5
    
     JMP TT11               \ Call TT11 to print (Y X) to 5 digits and return from
                            \ the subroutine using a tail call
    

[X]

Subroutine [TT11](tt11.md) (category: Text)

Print a 16-bit number, left-padded to n digits, and optional point

[pr6](pr6.md "Previous routine")[TT147](tt147.md "Next routine")
