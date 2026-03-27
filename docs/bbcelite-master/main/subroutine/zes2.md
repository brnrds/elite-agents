---
title: "The ZES2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/zes2.html"
---

[ZES1](zes1.md "Previous routine")[CLYNS](clyns.md "Next routine")
    
    
           Name: ZES2                                                    [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Zero-fill a specific page
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-zes2)
     Variations: See [code variations](../../related/compare/main/subroutine/zes2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CHPR](chpr.md) calls ZES2
    
    
    
    
    
    * * *
    
    
     Zero-fill from address (X SC) + Y to (X SC) + &FF.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The high byte (i.e. the page) of the starting point of
                            the zero-fill
    
       Y                    The offset from (X SC) where we start zeroing, counting
                            up to &FF
    
       SC                   The low byte (i.e. the offset into the page) of the
                            starting point of the zero-fill
    
    
    
    * * *
    
    
     Returns:
    
       Z flag               Z flag is set
    
    
    
    
    .ZES2
    
     LDA #0                 \ Load A with the byte we want to fill the memory block
                            \ with - i.e. zero
    
     STX SC+1               \ We want to zero-fill page X, so store this in the
                            \ high byte of SC, so the 16-bit address in SC and
                            \ SC+1 is now pointing to the SC-th byte of page X
    
    .ZEL1
    
     STA (SC),Y             \ Zero the Y-th byte of the block pointed to by SC,
                            \ so that's effectively the Y-th byte before SC
    
     INY                    \ Increment the loop counter
    
     BNE ZEL1               \ Loop back to zero the next byte
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Label [ZEL1](zes2.md#zel1) is local to this routine

[ZES1](zes1.md "Previous routine")[CLYNS](clyns.md "Next routine")
