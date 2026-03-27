---
title: "The crlf subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/crlf.html"
---

[qw](qw.md "Previous routine")[TT45](tt45.md "Next routine")
    
    
           Name: crlf                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Tab to column 21 and print a colon
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-crlf)
     Variations: See [code variations](../../related/compare/main/subroutine/crlf.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT27](tt27.md) calls crlf
    
    
    
    
    
    * * *
    
    
     Print control code 9 (tab to column 21 and print a colon). The subroutine
     name is pretty misleading, as it doesn't have anything to do with carriage
     returns or line feeds.
    
    
    
    
    .crlf
    
     LDA #21                \ Set the X-column in XC to 21
     JSR DOXC
    
     JMP TT73               \ Jump to TT73, which prints a colon (this BNE is
                            \ effectively a JMP as A will never be zero)
    

[X]

Subroutine [DOXC](doxc.md) (category: Text)

Move the text cursor to a specific column

[X]

Subroutine [TT73](tt73.md) (category: Text)

Print a colon

[qw](qw.md "Previous routine")[TT45](tt45.md "Next routine")
