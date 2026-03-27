---
title: "The CHECK2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/check2.html"
---

[CHECK](check.md "Previous routine")[JAMESON](jameson.md "Next routine")
    
    
           Name: CHECK2                                                  [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Calculate the third checksum for the last saved commander data
                 block (Commodore 64 and Apple II versions only)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-check2)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .CHECK2
    
    \LDX #NT%-3             \ These instructions are commented out in the original
    \                       \ source (they are from the Commodore 64 and Apple II
    \CLC                    \ versions, and implement the third commander checksum
    \                       \ which the Master version doesn't have)
    \TXA
    \
    \.QU2L2
    \
    \STX T
    \EOR T
    \ROR A
    \
    \ADC NA%+7,X
    \
    \EOR NA%+8,X
    \
    \DEX
    \
    \BNE QU2L2
    \
    \RTS
    

[CHECK](check.md "Previous routine")[JAMESON](jameson.md "Next routine")
