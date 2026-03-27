---
title: "The cmn subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/cmn.html"
---

[cpl](cpl.md "Previous routine")[ypl](ypl.md "Next routine")
    
    
           Name: cmn                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Print the commander's name
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-cmn)
     Variations: See [code variations](../../related/compare/main/subroutine/cmn.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT27](tt27.md) calls cmn
    
    
    
    
    
    * * *
    
    
     Print control code 4 (the commander's name).
    
    
    
    * * *
    
    
     Other entry points:
    
       cmn-1                Contains an RTS
    
    
    
    
    .cmn
    
     LDY #0                 \ Set up a counter in Y, starting from 0
    
    .QUL4
    
     LDA NAME,Y             \ The commander's name is stored at NAME, so load the
                            \ Y-th character from NAME
    
     CMP #13                \ If we have reached the end of the name, return from
     BEQ ypl-1              \ the subroutine (ypl-1 points to the RTS below)
    
     JSR TT26               \ Print the character we just loaded
    
     INY                    \ Increment the loop counter
    
     BNE QUL4               \ Loop back for the next character
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [NAME](../workspace/wp.md#name) in workspace [WP](../workspace/wp.md)

The current commander name

[X]

Label [QUL4](cmn.md#qul4) is local to this routine

[X]

Subroutine [TT26](tt26.md) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[X]

Entry point [ypl-1](ypl.md#ypl) in subroutine [ypl](ypl.md) (category: Universe)

Contains an RTS

[cpl](cpl.md "Previous routine")[ypl](ypl.md "Next routine")
