---
title: "The LL9 (Part 11 of 12) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll9_part_11_of_12.html"
---

[LL9 (Part 10 of 12)](ll9_part_10_of_12.md "Previous routine")[LL118](ll118.md "Next routine")
    
    
           Name: LL9 (Part 11 of 12)                                     [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Loop back for the next edge
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll9-part-11-of-12)
     Variations: See [code variations](../../related/compare/main/subroutine/ll9_part_11_of_12.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       LL81+2               Draw the contents of the ship line heap, used to draw
                            the ship as a dot from SHPPT
    
    
    
    
    .LL78
    
     LDA LSNUM              \ If LSNUM >= CNT, skip to LL81 so we don't loop back
     CMP CNT                \ for the next edge (CNT was set to the maximum heap
     BCS LL81               \ size for this ship in part 10, so this checks whether
                            \ we have just run out of space in the ship line heap,
                            \ and stops drawing edges if we have)
    
     LDA V                  \ Increment V by 4 so V(1 0) points to the data for the
     CLC                    \ next edge
     ADC #4
     STA V
    
     BCC P%+4               \ If the above addition didn't overflow, skip the
                            \ following instruction
    
     INC V+1                \ Otherwise increment the high byte of V(1 0), as we
                            \ just moved the V(1 0) pointer past a page boundary
    
     INC XX17               \ Increment the edge counter to point to the next edge
    
     LDY XX17               \ If Y < XX20, which contains the number of edges in
     CPY XX20               \ the blueprint, loop back to LL75 to process the next
     BCC LL75               \ edge
    
    .LL81
    
     JMP LSCLR              \ Jump down to part 12 below to draw any remaining lines
                            \ from the old ship that are still in the ship line heap
    

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Label [LL75](ll9_part_10_of_12.md#ll75) in subroutine [LL9 (Part 10 of 12)](ll9_part_10_of_12.md)

[X]

Label [LL81](ll9_part_11_of_12.md#ll81) is local to this routine

[X]

Entry point [LSCLR](ll9_part_12_of_12.md#lsclr) in subroutine [LL9 (Part 12 of 12)](ll9_part_12_of_12.md) (category: Drawing ships)

Draw any remaining lines from the old ship that are still in the ship line heap

[X]

Variable [LSNUM](../workspace/zp.md#lsnum) in workspace [ZP](../workspace/zp.md)

The pointer to the current position in the ship line heap as we work our way through the new ship's edges (and the corresponding old ship's edges) when drawing the ship in the main ship-drawing routine at LL9

[X]

Variable [V](../workspace/zp.md#v) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing an address pointer

[X]

Variable [XX17](../workspace/zp.md#xx17) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in BPRNT to store the number of characters to print, and as the edge counter in the main ship-drawing routine

[X]

Variable [XX20](../workspace/zp.md#xx20) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[LL9 (Part 10 of 12)](ll9_part_10_of_12.md "Previous routine")[LL118](ll118.md "Next routine")
