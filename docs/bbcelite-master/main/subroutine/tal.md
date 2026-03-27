---
title: "The tal subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tal.html"
---

[ypl](ypl.md "Previous routine")[fwl](fwl.md "Next routine")
    
    
           Name: tal                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Print the current galaxy number
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-tal)
     References: This subroutine is called as follows:
                 * [TT27](tt27.md) calls tal
    
    
    
    
    
    * * *
    
    
     Print control code 1 (the current galaxy number, right-aligned to width 3).
    
    
    
    
    .tal
    
     CLC                    \ We don't want to print the galaxy number with a
                            \ decimal point, so clear the C flag for pr2 to take as
                            \ an argument
    
     LDX GCNT               \ Load the current galaxy number from GCNT into X
    
     INX                    \ Add 1 to the galaxy number, as the galaxy numbers
                            \ are 0-7 internally, but we want to display them as
                            \ galaxy 1 through 8
    
     JMP pr2                \ Jump to pr2, which prints the number in X to a width
                            \ of 3 figures, left-padding with spaces to a width of
                            \ 3, and return from the subroutine using a tail call
    

[X]

Variable [GCNT](../workspace/wp.md#gcnt) in workspace [WP](../workspace/wp.md)

The number of the current galaxy (0-7)

[X]

Subroutine [pr2](pr2.md) (category: Text)

Print an 8-bit number, left-padded to 3 digits, and optional point

[ypl](ypl.md "Previous routine")[fwl](fwl.md "Next routine")
