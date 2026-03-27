---
title: "The YESNO subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/yesno.html"
---

[PLS6](pls6.md "Previous routine")[TT17](tt17.md "Next routine")
    
    
           Name: YESNO                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Wait until either "Y" or "N" is pressed
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-yesno)
     References: This subroutine is called as follows:
                 * [SVE](sve.md) calls YESNO
    
    
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if "Y" was pressed, clear if "N" was pressed
    
    
    
    
    .YESNO
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     CMP #'Y'               \ If "Y" was pressed, return from the subroutine with
     BEQ PL6                \ the C flag set (as the CMP sets the C flag, and PL6
                            \ contains an RTS)
    
     CMP #'N'               \ If "N" was not pressed, loop back to keep scanning
     BNE YESNO              \ for key presses
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    

[X]

Entry point [PL6](pls6.md#pl6) in subroutine [PLS6](pls6.md) (category: Drawing planets)

Contains an RTS

[X]

Subroutine [YESNO](yesno.md) (category: Keyboard)

Wait until either "Y" or "N" is pressed

[X]

Entry point [t](tt217.md#t) in subroutine [TT217](tt217.md) (category: Keyboard)

As TT217 but don't preserve Y, set it to YSAV instead

[PLS6](pls6.md "Previous routine")[TT17](tt17.md "Next routine")
