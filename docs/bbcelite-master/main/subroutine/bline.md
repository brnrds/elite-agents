---
title: "The BLINE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bline.html"
---

[HLOIN2](hloin2.md "Previous routine")[FLIP](flip.md "Next routine")
    
    
           Name: BLINE                                                   [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw a circle segment and add it to the ball line heap
      Deep dive: [The ball line heap](https://elite.bbcelite.com/deep_dives/the_ball_line_heap.html)
                 [Drawing circles](https://elite.bbcelite.com/deep_dives/drawing_circles.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-bline)
     Variations: See [code variations](../../related/compare/main/subroutine/bline.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CIRCLE2](circle2.md) calls BLINE
                 * [PLS22](pls22.md) calls BLINE
    
    
    
    
    
    * * *
    
    
     Draw a single segment of a circle, adding the point to the ball line heap.
    
    
    
    * * *
    
    
     Arguments:
    
       CNT                  The number of this segment
    
       STP                  The step size for the circle
    
       K6(1 0)              The x-coordinate of the new point on the circle, as
                            a screen coordinate
    
       (T X)                The y-coordinate of the new point on the circle, as
                            an offset from the centre of the circle
    
       FLAG                 Set to &FF for the first call, so it sets up the first
                            point in the heap but waits until the second call before
                            drawing anything (as we need two points, i.e. two calls,
                            before we can draw a line)
    
       K4(1 0)              Pixel y-coordinate of the centre of the circle
    
       K5(1 0)              Screen x-coordinate of the previous point added to the
                            ball line heap (if this is not the first point)
    
       K5(3 2)              Screen y-coordinate of the previous point added to the
                            ball line heap (if this is not the first point)
    
    
    
    * * *
    
    
     Returns:
    
       CNT                  CNT is updated to CNT + STP
    
       A                    The new value of CNT
    
       K5(1 0)              Screen x-coordinate of the point that we just added to
                            the ball line heap
    
       K5(3 2)              Screen y-coordinate of the point that we just added to
                            the ball line heap
    
       FLAG                 Set to 0
    
    
    
    
    .BLINE
    
     TXA                    \ Set K6(3 2) = (T X) + K4(1 0)
     ADC K4                 \             = y-coord of centre + y-coord of new point
     STA K6+2               \
     LDA K4+1               \ so K6(3 2) now contains the y-coordinate of the new
     ADC T                  \ point on the circle but as a screen coordinate, to go
     STA K6+3               \ along with the screen x-coordinate in K6(1 0)
    
     LDA FLAG               \ If FLAG = 0, jump down to BL1
     BEQ BL1
    
     INC FLAG               \ Flag is &FF so this is the first call to BLINE, so
                            \ increment FLAG to set it to 0, as then the next time
                            \ we call BLINE it can draw the first line, from this
                            \ point to the next
    
    .BL5
    
                            \ The following inserts a &FF marker into the LSY2 line
                            \ heap to indicate that the next call to BLINE should
                            \ store both the (X1, Y1) and (X2, Y2) points. We do
                            \ this on the very first call to BLINE (when FLAG is
                            \ &FF), and on subsequent calls if the segment does not
                            \ fit on-screen, in which case we don't draw or store
                            \ that segment, and we start a new segment with the next
                            \ call to BLINE that does fit on-screen
    
     LDY LSP                \ If byte LSP-1 of LSY2 = &FF, jump to BL7 to tidy up
     LDA #&FF               \ and return from the subroutine, as the point that has
     CMP LSY2-1,Y           \ been passed to BLINE is the start of a segment, so all
     BEQ BL7                \ we need to do is save the coordinate in K5, without
                            \ moving the pointer in LSP
    
     STA LSY2,Y             \ Otherwise we just tried to plot a segment but it
                            \ didn't fit on-screen, so put the &FF marker into the
                            \ heap for this point, so the next call to BLINE starts
                            \ a new segment
    
     INC LSP                \ Increment LSP to point to the next point in the heap
    
     BNE BL7                \ Jump to BL7 to tidy up and return from the subroutine
                            \ (this BNE is effectively a JMP, as LSP will never be
                            \ zero)
    
    .BL1
    
     LDA K5                 \ Set XX15 = K5 = x_lo of previous point
     STA XX15
    
     LDA K5+1               \ Set XX15+1 = K5+1 = x_hi of previous point
     STA XX15+1
    
     LDA K5+2               \ Set XX15+2 = K5+2 = y_lo of previous point
     STA XX15+2
    
     LDA K5+3               \ Set XX15+3 = K5+3 = y_hi of previous point
     STA XX15+3
    
     LDA K6                 \ Set XX15+4 = x_lo of new point
     STA XX15+4
    
     LDA K6+1               \ Set XX15+5 = x_hi of new point
     STA XX15+5
    
     LDA K6+2               \ Set XX12 = y_lo of new point
     STA XX12
    
     LDA K6+3               \ Set XX12+1 = y_hi of new point
     STA XX12+1
    
     JSR LL145              \ Call LL145 to see if the new line segment needs to be
                            \ clipped to fit on-screen, returning the clipped line's
                            \ end-points in (X1, Y1) and (X2, Y2)
    
     BCS BL5                \ If the C flag is set then the line is not visible on
                            \ screen anyway, so jump to BL5, to avoid drawing and
                            \ storing this line
    
     LDA SWAP               \ If SWAP = 0, then we didn't have to swap the line
     BEQ BL9                \ coordinates around during the clipping process, so
                            \ jump to BL9 to skip the following swap
    
     LDA X1                 \ Otherwise the coordinates were swapped by the call to
     LDY X2                 \ LL145 above, so we swap (X1, Y1) and (X2, Y2) back
     STA X2                 \ again
     STY X1
     LDA Y1
     LDY Y2
     STA Y2
     STY Y1
    
    .BL9
    
     LDY LSP                \ Set Y = LSP
    
     LDA LSY2-1,Y           \ If byte LSP-1 of LSY2 is not &FF, jump down to BL8
     CMP #&FF               \ to skip the following (X1, Y1) code
     BNE BL8
    
                            \ Byte LSP-1 of LSY2 is &FF, which indicates that we
                            \ need to store (X1, Y1) in the heap
    
     LDA X1                 \ Store X1 in the LSP-th byte of LSX2
     STA LSX2,Y
    
     LDA Y1                 \ Store Y1 in the LSP-th byte of LSY2
     STA LSY2,Y
    
     INY                    \ Increment Y to point to the next byte in LSX2/LSY2
    
    .BL8
    
     LDA X2                 \ Store X2 in the LSP-th byte of LSX2
     STA LSX2,Y
    
     LDA Y2                 \ Store Y2 in the LSP-th byte of LSX2
     STA LSY2,Y
    
     INY                    \ Increment Y to point to the next byte in LSX2/LSY2
    
     STY LSP                \ Update LSP to point to the same as Y
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2)
    
     LDA XX13               \ If XX13 is non-zero, jump up to BL5 to add a &FF
     BNE BL5                \ marker to the end of the line heap. XX13 is non-zero
                            \ after the call to the clipping routine LL145 above if
                            \ the end of the line was clipped, meaning the next line
                            \ sent to BLINE can't join onto the end but has to start
                            \ a new segment, and that's what inserting the &FF
                            \ marker does
    
    .BL7
    
     LDA K6                 \ Copy the data for this step point from K6(3 2 1 0)
     STA K5                 \ into K5(3 2 1 0), for use in the next call to BLINE:
     LDA K6+1               \
     STA K5+1               \   * K5(1 0) = screen x-coordinate of this point
     LDA K6+2               \
     STA K5+2               \   * K5(3 2) = screen y-coordinate of this point
     LDA K6+3               \
     STA K5+3               \ They now become the "previous point" in the next call
    
     LDA CNT                \ Set CNT = CNT + STP
     CLC
     ADC STP
     STA CNT
    
     RTS                    \ Return from the subroutine
    

[X]

Label [BL1](bline.md#bl1) is local to this routine

[X]

Label [BL5](bline.md#bl5) is local to this routine

[X]

Label [BL7](bline.md#bl7) is local to this routine

[X]

Label [BL8](bline.md#bl8) is local to this routine

[X]

Label [BL9](bline.md#bl9) is local to this routine

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [FLAG](../workspace/zp.md#flag) in workspace [ZP](../workspace/zp.md)

A flag that's used to define whether this is the first call to the ball line routine in BLINE, so it knows whether to wait for the second call before storing segment data in the ball line heap

[X]

Variable [K4](../workspace/zp.md#k4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [K5](../workspace/zp.md#k5) in workspace [ZP](../workspace/zp.md)

Temporary storage used to store segment coordinates across successive calls to BLINE, the ball line routine

[X]

Variable [K6](../workspace/zp.md#k6) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing coordinates during vector calculations

[X]

Subroutine [LL145 (Part 1 of 4)](ll145_part_1_of_4.md) (category: Drawing lines)

Clip line: Work out which end-points are on-screen, if any

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

Variable [STP](../workspace/zp.md#stp) in workspace [ZP](../workspace/zp.md)

The step size for drawing circles

[X]

Variable [SWAP](../workspace/wp.md#swap) in workspace [WP](../workspace/wp.md)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

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

Variable [XX13](../workspace/zp.md#xx13) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used in the line-drawing routines

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[HLOIN2](hloin2.md "Previous routine")[FLIP](flip.md "Next routine")
