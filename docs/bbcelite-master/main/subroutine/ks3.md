---
title: "The KS3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ks3.html"
---

[DJOY](djoy.md "Previous routine")[KS1](ks1.md "Next routine")
    
    
           Name: KS3                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set the SLSP ship line heap pointer after shuffling ship slots
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-ks3)
     References: This subroutine is called as follows:
                 * [KS2](ks2.md) calls KS3
    
    
    
    
    
    * * *
    
    
     The final part of the KILLSHP routine, called after we have shuffled the ship
     slots and sorted out our missiles. This simply sets SLSP to the new bottom of
     the ship line heap.
    
    
    
    * * *
    
    
     Arguments:
    
       P(1 0)               Points to the ship line heap of the ship in the last
                            occupied slot (i.e. it points to the bottom of the
                            descending heap)
    
    
    
    
    .KS3
    
     LDA P                  \ After shuffling the ship slots, P(1 0) will point to
     STA SLSP               \ the new bottom of the ship line heap, so store this in
     LDA P+1                \ SLSP(1 0), which stores the bottom of the heap
     STA SLSP+1
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SLSP](../workspace/wp.md#slsp) in workspace [WP](../workspace/wp.md)

The address of the bottom of the ship line heap

[DJOY](djoy.md "Previous routine")[KS1](ks1.md "Next routine")
