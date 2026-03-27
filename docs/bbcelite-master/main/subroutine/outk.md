---
title: "The OUTK subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/outk.html"
---

[NWDAV4](nwdav4.md "Previous routine")[TT208](tt208.md "Next routine")
    
    
           Name: OUTK                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print the character in Q before returning to gnum
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-outk)
     References: This subroutine is called as follows:
                 * [gnum](gnum.md) calls OUTK
    
    
    
    
    
    
    .OUTK
    
     LDA Q                  \ Print the character in Q, which is the key that was
     JSR DASC               \ just pressed in the gnum routine
    
     SEC                    \ Set the C flag, as this routine is only called if the
                            \ key pressed makes the number too high
    
     JMP OUT                \ Jump back into the gnum routine to return the number
                            \ that has been built
    

[X]

Entry point [DASC](tt26.md#dasc) in subroutine [TT26](tt26.md) (category: Text)

DASC does exactly the same as TT26 and prints a character at the text cursor, with support for verified text in extended tokens

[X]

Entry point [OUT](gnum.md#out) in subroutine [gnum](gnum.md) (category: Market)

The OUTK routine jumps back here after printing the key that was just pressed

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[NWDAV4](nwdav4.md "Previous routine")[TT208](tt208.md "Next routine")
