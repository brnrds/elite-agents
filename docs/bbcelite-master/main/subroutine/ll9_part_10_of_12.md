---
title: "The LL9 (Part 10 of 12) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll9_part_10_of_12.html"
---

[LL9 (Part 9 of 12)](ll9_part_9_of_12.md "Previous routine")[LL9 (Part 11 of 12)](ll9_part_11_of_12.md "Next routine")
    
    
           Name: LL9 (Part 10 of 12)                                     [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Calculate the visibility of each of the ship's edges
                 and draw the visible ones using flicker-free animation
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll9-part-10-of-12)
     Variations: See [code variations](../../related/compare/main/subroutine/ll9_part_10_of_12.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part calculates which edges are visible - in other words, which lines we
     should draw - and clips them to fit on the screen.
    
     Visible edges are drawn using flicker-free animation, which erases the
     corresponding edge from the on-screen ship at the same time.
    
     When we get here, the heap at XX3 contains all the visible vertex screen
     coordinates.
    
    
    
    
    .LL170
    
     LDY #3                 \ Fetch byte #3 of the ship's blueprint, which contains
     CLC                    \ the low byte of the offset to the edges data
     LDA (XX0),Y
    
     ADC XX0                \ Set V = low byte edges offset + XX0
     STA V
    
     LDY #16                \ Fetch byte #16 of the ship's blueprint, which contains
     LDA (XX0),Y            \ the high byte of the offset to the edges data
    
     ADC XX0+1              \ Set V+1 = high byte edges offset + XX0+1
     STA V+1                \
                            \ So V(1 0) now points to the start of the edges data
                            \ for this ship
    
     LDY #5                 \ Fetch byte #5 of the ship's blueprint, which contains
     LDA (XX0),Y            \ the maximum heap size for plotting the ship (which is
     STA CNT                \ 1 + 4 * the maximum number of visible edges) and store
                            \ it in CNT
    
    .LL75
    
     LDY #0                 \ Set Y = 0 so we start with byte #0
    
     LDA (V),Y              \ Fetch byte #0 for this edge, which contains the
                            \ visibility distance for this edge, beyond which the
                            \ edge is not shown
    
     CMP XX4                \ If XX4 > the visibility distance, where XX4 contains
     BCC LL78               \ the ship's z-distance reduced to 0-31 (which we set in
                            \ part 2), then this edge is too far away to be visible,
                            \ so jump down to LL78 to move on to the next edge
    
     INY                    \ Increment Y to point to byte #1
    
     LDA (V),Y              \ Fetch byte #1 for this edge into A, so:
                            \
                            \   A = %ffff ffff, where:
                            \
                            \     * Bits 0-3 = the number of face 1
                            \
                            \     * Bits 4-7 = the number of face 2
    
     STA P                  \ Store byte #1 into P
    
     AND #%00001111         \ Extract the number of face 1 into X
     TAX
    
     LDA XX2,X              \ If XX2+X is non-zero then we decided in part 5 that
     BNE LL79               \ face 1 is visible, so jump to LL79
    
     LDA P                  \ Fetch byte #1 for this edge into A
    
     LSR A                  \ Shift right four times to extract the number of face 2
     LSR A                  \ from bits 4-7 into X
     LSR A
     LSR A
     TAX
    
     LDA XX2,X              \ If XX2+X is zero then we decided in part 5 that
     BEQ LL78               \ face 2 is hidden, so jump to LL78
    
    .LL79
    
                            \ We now build the screen line for this edge, as
                            \ follows:
                            \
                            \   XX15(1 0) = start x-coordinate
                            \
                            \   XX15(3 2) = start y-coordinate
                            \
                            \   XX15(5 4) = end x-coordinate
                            \
                            \   XX12(1 0) = end y-coordinate
                            \
                            \ We can then pass this to the line clipping routine
                            \ before storing the resulting line in the ship line
                            \ heap
    
     INY                    \ Increment Y to point to byte #2
    
     LDA (V),Y              \ Fetch byte #2 for this edge into X, which contains
     TAX                    \ the number of the vertex at the start of the edge
                            \
                            \ Byte #2 contains the vertex number multiplied by 4,
                            \ so we can use it as an index into the heap at XX3 to
                            \ fetch the vertex's screen coordinates, which are
                            \ stored as four bytes containing two 16-bit numbers
    
     LDA XX3,X              \ Fetch the x_lo coordinate of the edge's start vertex
     STA XX15               \ from the XX3 heap into XX15
    
     LDA XX3+1,X            \ Fetch the x_hi coordinate of the edge's start vertex
     STA XX15+1             \ from the XX3 heap into XX15+1
    
     LDA XX3+2,X            \ Fetch the y_lo coordinate of the edge's start vertex
     STA XX15+2             \ from the XX3 heap into XX15+2
    
     LDA XX3+3,X            \ Fetch the y_hi coordinate of the edge's start vertex
     STA XX15+3             \ from the XX3 heap into XX15+3
    
     INY                    \ Increment Y to point to byte #3
    
     LDA (V),Y              \ Fetch byte #3 for this edge into X, which contains
     TAX                    \ the number of the vertex at the end of the edge
    
     LDA XX3,X              \ Fetch the x_lo coordinate of the edge's end vertex
     STA XX15+4             \ from the XX3 heap into XX15+4
    
     LDA XX3+2,X            \ Fetch the y_lo coordinate of the edge's end vertex
     STA XX12               \ from the XX3 heap into XX12
    
     LDA XX3+3,X            \ Fetch the y_hi coordinate of the edge's end vertex
     STA XX12+1             \ from the XX3 heap into XX12+1
    
     LDA XX3+1,X            \ Fetch the x_hi coordinate of the edge's end vertex
     STA XX15+5             \ from the XX3 heap into XX15+5
    
     JSR CLIP2              \ Call CLIP2 to see if the new line segment needs to be
                            \ clipped to fit on-screen, returning the clipped line's
                            \ end-points in (X1, Y1) and (X2, Y2)
    
     BCS LL78               \ If the C flag is set then the line is not visible on
                            \ screen, so jump to LL78 so we don't store this line
                            \ in the ship line heap
    
     JSR LSPUT              \ Draw this edge using flicker-free animation, by first
                            \ drawing the ship's new line and then erasing the
                            \ corresponding old line from the screen
    

[X]

Entry point [CLIP2](ll145_part_1_of_4.md#clip2) in subroutine [LL145 (Part 1 of 4)](ll145_part_1_of_4.md) (category: Drawing lines)

Don't initialise the values in SWAP or A

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Label [LL78](ll9_part_11_of_12.md#ll78) in subroutine [LL9 (Part 11 of 12)](ll9_part_11_of_12.md)

[X]

Label [LL79](ll9_part_10_of_12.md#ll79) is local to this routine

[X]

Subroutine [LSPUT](lsput.md) (category: Drawing lines)

Draw a ship line using flicker-free animation

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [V](../workspace/zp.md#v) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing an address pointer

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX12](../workspace/zp.md#xx12) in workspace [ZP](../workspace/zp.md)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Variable [XX2](../workspace/zp.md#xx2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the visibility of the ship's faces during the ship-drawing routine at LL9

[X]

Workspace [XX3](../workspace/xx3.md) (category: Workspaces)

Temporary storage space for complex calculations

[X]

Variable [XX4](../workspace/zp.md#xx4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[LL9 (Part 9 of 12)](ll9_part_9_of_12.md "Previous routine")[LL9 (Part 11 of 12)](ll9_part_11_of_12.md "Next routine")
