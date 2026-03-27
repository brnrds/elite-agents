---
title: "The KTRAN variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/ktran.html"
---

[DVIDT](../subroutine/dvidt.md "Previous routine")[SHPPT](../subroutine/shppt.md "Next routine")
    
    
           Name: KTRAN                                                   [Show more]
           Type: Variable
       Category: Keyboard
        Summary: An unused key logger buffer that's left over from the 6502 Second
                 Processor version of Elite
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-ktran)
     Variations: See [code variations](../../related/compare/main/variable/ktran.md) for this variable in the different versions
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .buf
    
     EQUB 2                 \ Transmit 2 bytes as part of this command
    
     EQUB 15                \ Receive 15 bytes as part of this command
    
    .KTRAN
    
     EQUS "1234567890"      \ A 17-byte buffer to hold the key logger data from the
     EQUS "1234567"         \ KEYBOARD routine in the I/O processor (note that only
                            \ 12 of these bytes are actually updated by the KEYBOARD
                            \ routine)
    

[DVIDT](../subroutine/dvidt.md "Previous routine")[SHPPT](../subroutine/shppt.md "Next routine")
