---
title: "The qw subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/qw.html"
---

[TT41](tt41.md "Previous routine")[crlf](crlf.md "Next routine")
    
    
           Name: qw                                                      [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a recursive token in the range 128-145
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-qw)
     References: This subroutine is called as follows:
                 * [TT27](tt27.md) calls qw
    
    
    
    
    
    * * *
    
    
     Print a recursive token where the token number is in 128-145 (so the value
     passed to TT27 is in the range 14-31).
    
    
    
    * * *
    
    
     Arguments:
    
       A                    A value from 128-145, which refers to a recursive token
                            in the range 14-31
    
    
    
    
    .qw
    
     ADC #114               \ This is a recursive token in the range 0-95, so add
     BNE ex                 \ 114 to the argument to get the token number 128-145
                            \ and jump to ex to print it
    

[X]

Subroutine [ex](ex.md) (category: Text)

Print a recursive token

[TT41](tt41.md "Previous routine")[crlf](crlf.md "Next routine")
