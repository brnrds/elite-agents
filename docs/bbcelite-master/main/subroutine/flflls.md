---
title: "The FLFLLS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/flflls.html"
---

[WPSHPS](wpshps.md "Previous routine")[DET1](det1.md "Next routine")
    
    
           Name: FLFLLS                                                  [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Reset the sun line heap
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-flflls)
     Variations: See [code variations](../../related/compare/main/subroutine/flflls.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KS4](ks4.md) calls FLFLLS
                 * [TT23](tt23.md) calls FLFLLS
                 * [TTX66K](ttx66k.md) calls FLFLLS
    
    
    
    
    
    * * *
    
    
     Reset the sun line heap at LSO by zero-filling it and setting the first byte
     to &FF.
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is set to 0
    
    
    
    
    .FLFLLS
    
     LDY #2*Y-1+8           \ #Y is the y-coordinate of the centre of the space
                            \ view, so this sets Y as a counter for the number of
                            \ lines in the space view (i.e. 191) - which is also the
                            \ number of lines in the LSO block - plus an extra 8
                            \ bytes
    
     LDA #0                 \ Set A to 0 so we can zero-fill the LSO block
    
    .SAL6
    
     STA LSO,Y              \ Set the Y-th byte of the LSO block to 0
    
     DEY                    \ Decrement the counter
    
     BNE SAL6               \ Loop back until we have filled all the way to LSO+1
    
     DEY                    \ Decrement Y to value of &FF (as we exit the above loop
                            \ with Y = 0)
    
     STY LSX                \ Set the first byte of the LSO block, which has its own
                            \ label LSX, to &FF, to indicate that the sun line heap
                            \ is empty
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [LSO](../workspace/wp.md#lso) in workspace [WP](../workspace/wp.md)

The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)

[X]

Variable [LSX](../workspace/zp.md#lsx) in workspace [ZP](../workspace/zp.md)

LSX contains the status of the sun line heap at LSO

[X]

Label [SAL6](flflls.md#sal6) is local to this routine

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[WPSHPS](wpshps.md "Previous routine")[DET1](det1.md "Next routine")
