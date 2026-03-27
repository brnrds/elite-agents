---
title: "The SWAPPZERO subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/swappzero.html"
---

[ex](ex.md "Previous routine")[DOEXP](doexp.md "Next routine")
    
    
           Name: SWAPPZERO                                               [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: An unused routine that swaps bytes in and out of zero page
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-swappzero)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .SWAPPZERO
    
     LDX #K3+1              \ This routine starts copying zero page from the byte
                            \ after K3 and up, using X as an index
    
    .SWPZL
    
     LDA ZP,X               \ These instructions have no effect, as they simply swap
     LDY ZP,X               \ a byte with itself
     STA ZP,X
     STY ZP,X
    
     INX                    \ Increment the loop counter
    
     BNE SWPZL              \ Loop back for the next byte
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [SWPZL](swappzero.md#swpzl) is local to this routine

[X]

Workspace [ZP](../workspace/zp.md) (category: Workspaces)

Lots of important variables are stored in the zero page workspace as it is quicker and more space-efficient to access memory here

[ex](ex.md "Previous routine")[DOEXP](doexp.md "Next routine")
