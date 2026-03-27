---
title: "The VOWEL subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/vowel.html"
---

[MT19](mt19.md "Previous routine")[JMTB](../variable/jmtb.md "Next routine")
    
    
           Name: VOWEL                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Test whether a character is a vowel
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-vowel)
     References: This subroutine is called as follows:
                 * [MT17](mt17.md) calls VOWEL
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be tested
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is set if the character is a vowel, otherwise
                            it is clear
    
    
    
    
    .VOWEL
    
     ORA #%00100000         \ Set bit 5 of the character to make it lower case
    
     CMP #'a'               \ If the letter is a vowel, jump to VRTS to return from
     BEQ VRTS               \ the subroutine with the C flag set (as the CMP will
     CMP #'e'               \ set the C flag if the comparison is equal)
     BEQ VRTS
     CMP #'i'
     BEQ VRTS
     CMP #'o'
     BEQ VRTS
     CMP #'u'
     BEQ VRTS
    
     CLC                    \ The character is not a vowel, so clear the C flag
    
    .VRTS
    
     RTS                    \ Return from the subroutine
    

[X]

Label [VRTS](vowel.md#vrts) is local to this routine

[MT19](mt19.md "Previous routine")[JMTB](../variable/jmtb.md "Next routine")
