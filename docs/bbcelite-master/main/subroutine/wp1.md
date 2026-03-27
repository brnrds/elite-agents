---
title: "The WP1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/wp1.html"
---

[WPLS2](wpls2.md "Previous routine")[WPLS](wpls.md "Next routine")
    
    
           Name: WP1                                                     [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Reset the ball line heap
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-wp1)
     References: This subroutine is called as follows:
                 * [WPLS2](wpls2.md) calls WP1
    
    
    
    
    
    
    .WP1
    
     LDA #1                 \ Set LSP = 1 to reset the ball line heap pointer
     STA LSP
    
     LDA #&FF               \ Set LSX2 = &FF to indicate the ball line heap is empty
     STA LSX2
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [LSP](../workspace/zp.md#lsp) in workspace [ZP](../workspace/zp.md)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Variable [LSX2](../workspace/wp.md#lsx2) in workspace [WP](../workspace/wp.md)

The ball line heap for storing x-coordinates

[WPLS2](wpls2.md "Previous routine")[WPLS](wpls.md "Next routine")
