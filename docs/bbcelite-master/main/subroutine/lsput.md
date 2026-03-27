---
title: "The LSPUT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/lsput.html"
---

[LL9 (Part 12 of 12)](ll9_part_12_of_12.md "Previous routine")[MVEIT (Part 1 of 9)](mveit_part_1_of_9.md "Next routine")
    
    
           Name: LSPUT                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a ship line using flicker-free animation
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-lsput)
     References: This subroutine is called as follows:
                 * [LL9 (Part 9 of 12)](ll9_part_9_of_12.md) calls LSPUT
                 * [LL9 (Part 10 of 12)](ll9_part_10_of_12.md) calls LSPUT
                 * [SHPPT](shppt.md) calls LSPUT
    
    
    
    
    
    * * *
    
    
     This routine implements flicker-free ship animation by erasing and redrawing
     each individual line in the ship, rather than the approach in the other
     Acornsoft versions of the game, which erase the entire existing ship before
     drawing the new one.
    
     Here's the new approach in this routine:
    
       * Draw the new line
    
       * Fetch the corresponding existing line (in position LSNUM) from the heap
    
       * Store the new line in the heap at this position, replacing the old one
    
       * If the existing line we just took from the heap is on-screen, erase it
    
    
    
    * * *
    
    
     Arguments:
    
       LSNUM                The offset within the line heap where we add the new
                            line's coordinates
    
       X1                   The screen x-coordinate of the start of the line to add
                            to the ship line heap
    
       Y1                   The screen y-coordinate of the start of the line to add
                            to the ship line heap
    
       X2                   The screen x-coordinate of the end of the line to add
                            to the ship line heap
    
       Y2                   The screen y-coordinate of the end of the line to add
                            to the ship line heap
    
       XX19(1 0)            XX19(1 0) shares its location with INWK(34 33), which
                            contains the ship line heap address pointer
    
    
    
    * * *
    
    
     Returns:
    
       LSNUM                The offset of the next line in the line heap
    
    
    
    
    .LSPUT
    
     LDY LSNUM              \ Set Y = LSNUM, to get the offset within the ship line
                            \ heap where we want to insert our new line
    
     CPY LSNUM2             \ Compare LSNUM and LSNUM2 and store the flags on the
     PHP                    \ stack so we can retrieve them later
    
     LDX #3                 \ We now want to copy the line coordinates (X1, Y1) and
                            \ (X2, Y2) to XX12...XX12+3, so set a counter to copy
                            \ 4 bytes
    
    .LSC4
    
     LDA X1,X               \ Copy the X-th byte of X1/Y1/X2/Y2 to the X-th byte of
     STA XX12,X             \ XX12
    
     DEX                    \ Decrement the loop counter
    
     BPL LSC4               \ Loop back until we have copied all four bytes
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2)
    
     LDA (XX19),Y           \ Set X1 to the Y-th coordinate on the ship line heap,
     STA X1                 \ i.e. the one we are replacing in the heap
    
     LDA XX12               \ Replace it with the X1 coordinate in XX12
     STA (XX19),Y
    
     INY                    \ Increment the index to point to the Y1 coordinate
    
     LDA (XX19),Y           \ Set Y1 to the Y-th coordinate on the ship line heap,
     STA Y1                 \ i.e. the one we are replacing in the heap
    
     LDA XX12+1             \ Replace it with the Y1 coordinate in XX12+1
     STA (XX19),Y
    
     INY                    \ Increment the index to point to the X2 coordinate
    
     LDA (XX19),Y           \ Set X2 to the Y-th coordinate on the ship line heap,
     STA X2                 \ i.e. the one we are replacing in the heap
    
     LDA XX12+2             \ Replace it with the X2 coordinate in XX12+2
     STA (XX19),Y
    
     INY                    \ Increment the index to point to the Y2 coordinate
    
     LDA (XX19),Y           \ Set Y2 to the Y-th coordinate on the ship line heap,
     STA Y2                 \ i.e. the one we are replacing in the heap
    
     LDA XX12+3             \ Replace it with the Y2 coordinate in XX12+3
     STA (XX19),Y
    
     INY                    \ Increment the index to point to the next coordinate
     STY LSNUM              \ and store the updated index in LSNUM
    
     PLP                    \ Restore the result of the comparison above, so if the
     BCS LSC3               \ original value of LSNUM >= LSNUM2, then we have
                            \ already redrawn all the lines from the old ship's line
                            \ heap, so return from the subroutine (as LSC3 contains
                            \ an RTS)
    
     JMP LOIN               \ Otherwise there are still more lines to erase from the
                            \ old ship on-screen, so the coordinates in (X1, Y1) and
                            \ (X2, Y2) that we just pulled from the ship line heap
                            \ point to a line that is still on-screen, so call LOIN
                            \ to draw this line and erase it from the screen,
                            \ returning from the subroutine using a tail call
    

[X]

Subroutine [LOIN](loin.md) (category: Drawing lines)

Draw a one-segment line

[X]

Entry point [LSC3](ll9_part_12_of_12.md#lsc3) in subroutine [LL9 (Part 12 of 12)](ll9_part_12_of_12.md) (category: Drawing ships)

Contains an RTS

[X]

Label [LSC4](lsput.md#lsc4) is local to this routine

[X]

Variable [LSNUM](../workspace/zp.md#lsnum) in workspace [ZP](../workspace/zp.md)

The pointer to the current position in the ship line heap as we work our way through the new ship's edges (and the corresponding old ship's edges) when drawing the ship in the main ship-drawing routine at LL9

[X]

Variable [LSNUM2](../workspace/zp.md#lsnum2) in workspace [ZP](../workspace/zp.md)

The size of the existing ship line heap for the ship we are drawing in LL9, i.e. the number of lines in the old ship that is currently shown on-screen and which we need to erase

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](../workspace/zp.md#x2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XX12](../workspace/zp.md#xx12) in workspace [ZP](../workspace/zp.md)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX19](../workspace/zp.md#xx19) in workspace [ZP](../workspace/zp.md)

XX19(1 0) shares its location with INWK(34 33), which contains the address of the ship line heap

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[LL9 (Part 12 of 12)](ll9_part_12_of_12.md "Previous routine")[MVEIT (Part 1 of 9)](mveit_part_1_of_9.md "Next routine")
