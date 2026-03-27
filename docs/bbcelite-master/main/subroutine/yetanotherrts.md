---
title: "The yetanotherrts subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/yetanotherrts.html"
---

[TT17X](tt17x.md "Previous routine")[ECMOF](ecmof.md "Next routine")
    
    
           Name: yetanotherrts                                           [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Contains an RTS
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-yetanotherrts)
     References: This subroutine is called as follows:
                 * [SFRMIS](sfrmis.md) calls yetanotherrts
    
    
    
    
    
    * * *
    
    
     This routine contains an RTS so we can return from the SFRMIS subroutine with
     a branch instruction.
    
     It also contains the DEMON label, which is left over from the 6502 Second
     Processor version, where it implements the demo (there is no demo in this
     version of Elite).
    
    
    
    
    .yetanotherrts
    
    .DEMON
    
     RTS                    \ Return from the subroutine
    

[TT17X](tt17x.md "Previous routine")[ECMOF](ecmof.md "Next routine")
