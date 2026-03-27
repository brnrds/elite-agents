---
title: "The WPLS2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/wpls2.html"
---

[CIRCLE2](circle2.md "Previous routine")[WP1](wp1.md "Next routine")
    
    
           Name: WPLS2                                                   [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Remove the planet from the screen
      Deep dive: [The ball line heap](https://elite.bbcelite.com/deep_dives/the_ball_line_heap.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-wpls2)
     References: This subroutine is called as follows:
                 * [PL2](pl2.md) calls WPLS2
                 * [PL9 (Part 1 of 3)](pl9_part_1_of_3.md) calls WPLS2
    
    
    
    
    
    * * *
    
    
     We do this by redrawing it using the lines stored in the ball line heap when
     the planet was originally drawn by the BLINE routine.
    
    
    
    
    .WPLS2
    
     LDY LSX2               \ If LSX2 is non-zero (which indicates the ball line
     BNE WP1                \ heap is empty), jump to WP1 to reset the line heap
                            \ without redrawing the planet
    
                            \ Otherwise Y is now 0, so we can use it as a counter to
                            \ loop through the lines in the line heap, redrawing
                            \ each one to remove the planet from the screen, before
                            \ resetting the line heap once we are done
    
    .WPL1
    
     CPY LSP                \ If Y >= LSP then we have reached the end of the line
     BCS WP1                \ heap and have finished redrawing the planet (as LSP
                            \ points to the end of the heap), so jump to WP1 to
                            \ reset the line heap, returning from the subroutine
                            \ using a tail call
    
     LDA LSY2,Y             \ Set A to the y-coordinate of the current heap entry
    
     CMP #&FF               \ If the y-coordinate is &FF, this indicates that the
     BEQ WP2                \ next point in the heap denotes the start of a line
                            \ segment, so jump to WP2 to put it into (X1, Y1)
    
     STA Y2                 \ Set (X2, Y2) to the x- and y-coordinates from the
     LDA LSX2,Y             \ heap
     STA X2
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2)
    
     INY                    \ Increment the loop counter to point to the next point
    
     LDA SWAP               \ If SWAP is non-zero then we swapped the coordinates
     BNE WPL1               \ when filling the heap in BLINE, so loop back WPL1
                            \ for the next point in the heap
    
     LDA X2                 \ Swap (X1, Y1) and (X2, Y2), so the next segment will
     STA X1                 \ be drawn from the current (X2, Y2) to the next point
     LDA Y2                 \ in the heap
     STA Y1
    
     JMP WPL1               \ Loop back to WPL1 for the next point in the heap
    
    .WP2
    
     INY                    \ Increment the loop counter to point to the next point
    
     LDA LSX2,Y             \ Set (X1, Y1) to the x- and y-coordinates from the
     STA X1                 \ heap
     LDA LSY2,Y
     STA Y1
    
     INY                    \ Increment the loop counter to point to the next point
    
     JMP WPL1               \ Loop back to WPL1 for the next point in the heap
    

[X]

Subroutine [LOIN](loin.md) (category: Drawing lines)

Draw a one-segment line

[X]

Variable [LSP](../workspace/zp.md#lsp) in workspace [ZP](../workspace/zp.md)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Variable [LSX2](../workspace/wp.md#lsx2) in workspace [WP](../workspace/wp.md)

The ball line heap for storing x-coordinates

[X]

Variable [LSY2](../workspace/wp.md#lsy2) in workspace [WP](../workspace/wp.md)

The ball line heap for storing y-coordinates

[X]

Variable [SWAP](../workspace/wp.md#swap) in workspace [WP](../workspace/wp.md)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[X]

Subroutine [WP1](wp1.md) (category: Drawing planets)

Reset the ball line heap

[X]

Label [WP2](wpls2.md#wp2) is local to this routine

[X]

Label [WPL1](wpls2.md#wpl1) is local to this routine

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](../workspace/zp.md#x2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[CIRCLE2](circle2.md "Previous routine")[WP1](wp1.md "Next routine")
