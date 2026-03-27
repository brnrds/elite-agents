---
title: "The GTDRV subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/gtdrv.html"
---

[oldlong](../variable/oldlong.md "Previous routine")[LOD](lod.md "Next routine")
    
    
           Name: GTDRV                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Get an ASCII disc drive number from the keyboard
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-gtdrv)
     References: This subroutine is called as follows:
                 * [CATS](cats.md) calls GTDRV
                 * [SVE](sve.md) calls GTDRV
    
    
    
    
    
    * * *
    
    
     Returns:
    
       A                    The ASCII value of the entered drive number ("0" to "3")
    
       C flag               Clear if a valid drive number was entered (0-3), set
                            otherwise
    
    
    
    
    .GTDRV
    
     LDA #2                 \ Print extended token 2 ("{cr}WHICH DRIVE?")
     JSR DETOK
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     ORA #%00010000         \ Set bit 4 of A, perhaps to avoid printing any control
                            \ characters in the next instruction
    
     JSR CHPR               \ Print the character in A
    
     PHA                    \ Store A on the stack so we can retrieve it after the
                            \ call to FEED
    
     JSR FEED               \ Print a newline
    
     PLA                    \ Restore A from the stack
    
     CMP #'0'               \ If A < ASCII "0", then it is not a valid drive number,
     BCC LOR                \ so jump to LOR to set the C flag and return from the
                            \ subroutine
    
     CMP #'4'               \ If A >= ASCII "4", then it is not a valid drive
                            \ number, and this CMP sets the C flag, otherwise it is
                            \ a valid drive number in the range 0-3, so clear it
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [CHPR](chpr.md) (category: Text)

Print a character at the text cursor by poking into screen memory

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [FEED](feed.md) (category: Text)

Print a newline

[X]

Entry point [LOR](lod.md#lor) in subroutine [LOD](lod.md) (category: Save and load)

Set the C flag and return from the subroutine

[X]

Entry point [t](tt217.md#t) in subroutine [TT217](tt217.md) (category: Keyboard)

As TT217 but don't preserve Y, set it to YSAV instead

[oldlong](../variable/oldlong.md "Previous routine")[LOD](lod.md "Next routine")
