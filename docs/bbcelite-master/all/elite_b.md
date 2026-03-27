---
title: "Elite B source in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_b.html"
---

[Elite A source](elite_a.md "Previous routine")[Elite C source](elite_c.md "Next routine")
    
    
     ELITE B FILE
    
    
    
    
     CODE_B% = P%
    
     LOAD_B% = LOAD% + P% - CODE%
    
    
    
    
           Name: UNIV                                                    [Show more]
           Type: Variable
       Category: Universe
        Summary: Table of pointers to the local universe's ship data blocks
      Deep dive: [The local bubble of universe](https://elite.bbcelite.com/deep_dives/the_local_bubble_of_universe.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
    
    
        Context: See this variable [on its own page](../main/variable/univ.md)
     References: This variable is used as follows:
                 * [GINF](../main/subroutine/ginf.md) uses UNIV
                 * [KILLSHP](../main/subroutine/killshp.md) uses UNIV
                 * [KS2](../main/subroutine/ks2.md) uses UNIV
                 * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md) uses UNIV
    
    
    
    
    
    
    .UNIV
    
     FOR I%, 0, NOSH
    
      EQUW K% + I% * NI%    \ Address of block no. I%, of size NI%, in workspace K%
    
     NEXT
    
    
    
    
           Name: FLKB                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Flush the keyboard buffer
    
    
        Context: See this subroutine [on its own page](../main/subroutine/flkb.md)
     Variations: See [code variations](../related/compare/main/subroutine/flkb.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MT26](../main/subroutine/mt26.md) calls FLKB
                 * [TRADEMODE](../main/subroutine/trademode.md) calls FLKB
    
    
    
    
    
    * * *
    
    
     This routine does nothing in the BBC Master version of Elite. It does have a
     function in the disc and 6502SP versions, so the authors presumably just
     cleared out the FLKB routine for the Master version, rather than unplumbing it
     from the code.
    
    
    
    
    .FLKB
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: NLIN3                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Print a title and draw a horizontal line at row 19 to box it in
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nlin3.md)
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls NLIN3
                 * [STATUS](../main/subroutine/status.md) calls NLIN3
                 * [TT167](../main/subroutine/tt167.md) calls NLIN3
                 * [TT208](../main/subroutine/tt208.md) calls NLIN3
                 * [TT23](../main/subroutine/tt23.md) calls NLIN3
                 * [TT25](../main/subroutine/tt25.md) calls NLIN3
    
    
    
    
    
    * * *
    
    
     This routine print a text token at the cursor position and draws a horizontal
     line at pixel row 19. It is used for the Status Mode screen, the Short-range
     Chart, the Market Price screen and the Equip Ship screen.
    
    
    
    
    .NLIN3
    
     JSR TT27               \ Print the text token in A
    
                            \ Fall through into NLIN4 to draw a horizontal line at
                            \ pixel row 19
    
    
    
    
           Name: NLIN4                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a horizontal line at pixel row 19 to box in a title
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nlin4.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls NLIN4
                 * [TT213](../main/subroutine/tt213.md) calls NLIN4
    
    
    
    
    
    * * *
    
    
     This routine is used on the Inventory screen to draw a horizontal line at
     pixel row 19 to box in the title.
    
    
    
    
    .NLIN4
    
     LDA #19                \ Jump to NLIN2 to draw a horizontal line at pixel row
     BNE NLIN2              \ 19, returning from the subroutine with using a tail
                            \ call (this BNE is effectively a JMP as A will never
                            \ be zero)
    
    
    
    
           Name: NLIN                                                    [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a horizontal line at pixel row 23 to box in a title
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nlin.md)
     Variations: See [code variations](../related/compare/main/subroutine/nlin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT22](../main/subroutine/tt22.md) calls NLIN
                 * [TT22](../main/subroutine/tt22.md) calls via NLIN5
    
    
    
    
    
    * * *
    
    
     Draw a horizontal line at pixel row 23 and move the text cursor down one
     line.
    
    
    
    * * *
    
    
     Other entry points:
    
       NLIN5                Move the text cursor down one line before drawing the
                            line
    
    
    
    
    .NLIN
    
     LDA #23                \ Set A = 23 so NLIN2 below draws a horizontal line at
                            \ pixel row 23
    
    .NLIN5
    
     INC YC                 \ Move the text cursor down one line
    
                            \ Fall through into NLIN2 to draw the horizontal line
                            \ at row 23
    
    
    
    
           Name: NLIN2                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a screen-wide horizontal line at the pixel row in A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nlin2.md)
     Variations: See [code variations](../related/compare/main/subroutine/nlin2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [NLIN4](../main/subroutine/nlin4.md) calls NLIN2
    
    
    
    
    
    * * *
    
    
     This draws a line from (2, A) to (254, A), which is almost screen-wide and
     fits in nicely between the border boxes without clashing with it.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The pixel row on which to draw the horizontal line
    
    
    
    
    .NLIN2
    
     STA Y1                 \ Set Y1 = A
    
     LDA #YELLOW            \ Switch to colour 1, which is yellow
     STA COL
    
     LDX #2                 \ Set X1 = 2, so (X1, Y1) = (2, A)
     STX X1
    
     LDX #254               \ Set X2 = 254, so (X2, Y2) = (254, A)
     STX X2
    
     JSR HLOIN3             \ Call HLOIN3 to draw a line from (2, A) to (254, A)
    
     LDA #CYAN              \ Switch to colour 3, which is cyan or white
     STA COL
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: HLOIN2                                                  [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Remove a line from the sun line heap and draw it on-screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hloin2.md)
     Variations: See [code variations](../related/compare/main/subroutine/hloin2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md) calls HLOIN2
                 * [SUN (Part 4 of 4)](../main/subroutine/sun_part_4_of_4.md) calls HLOIN2
                 * [WPLS](../main/subroutine/wpls.md) calls HLOIN2
    
    
    
    
    
    * * *
    
    
     Specifically, this does the following:
    
       * Set X1 and X2 to the x-coordinates of the ends of the horizontal line with
         centre YY(1 0) and length A to the left and right
    
       * Set the Y-th byte of the LSO block to 0 (i.e. remove this line from the
         sun line heap)
    
       * Draw a horizontal line from (X1, Y) to (X2, Y)
    
    
    
    * * *
    
    
     Arguments:
    
       YY(1 0)              The x-coordinate of the centre point of the line
    
       A                    The half-width of the line, i.e. the contents of the
                            Y-th byte of the sun line heap
    
       Y                    The number of the entry in the sun line heap (which is
                            also the y-coordinate of the line)
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is preserved
    
    
    
    
    .HLOIN2
    
     JSR EDGES              \ Call EDGES to calculate X1 and X2 for the horizontal
                            \ line centred on YY(1 0) and with half-width A
    
     STY Y1                 \ Set Y1 = Y
    
     LDA #0                 \ Set the Y-th byte of the LSO block to 0
     STA LSO,Y
    
     JMP HLOIN              \ Call HLOIN to draw a horizontal line from (X1, Y) to
                            \ (X2, Y), returning from the subroutine using a tail
                            \ call
    
    
    
    
           Name: BLINE                                                   [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw a circle segment and add it to the ball line heap
      Deep dive: [The ball line heap](https://elite.bbcelite.com/deep_dives/the_ball_line_heap.html)
                 [Drawing circles](https://elite.bbcelite.com/deep_dives/drawing_circles.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bline.md)
     Variations: See [code variations](../related/compare/main/subroutine/bline.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CIRCLE2](../main/subroutine/circle2.md) calls BLINE
                 * [PLS22](../main/subroutine/pls22.md) calls BLINE
    
    
    
    
    
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
    
    
    
    
           Name: FLIP                                                    [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Reflect the stardust particles in the screen diagonal and redraw
                 the stardust field
    
    
        Context: See this subroutine [on its own page](../main/subroutine/flip.md)
     Variations: See [code variations](../related/compare/main/subroutine/flip.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOOK1](../main/subroutine/look1.md) calls FLIP
    
    
    
    
    
    * * *
    
    
     Swap the x- and y-coordinates of all the stardust particles and draw the new
     set of particles. Called by LOOK1 when we switch views.
    
     This is a quick way of making the stardust field in the new view feel
     different without having to generate a whole new field. If you look carefully
     at the stardust field when you switch views, you can just about see that the
     new field is a reflection of the previous field in the screen diagonal, i.e.
     in the line from bottom left to top right. This is the line where x = y when
     the origin is in the middle of the screen, and positive x and y are right and
     up, which is the coordinate system we use for stardust).
    
    
    
    
    .FLIP
    
     LDA #DUST              \ Switch to stripe 3-2-3-2, which is cyan/red in the
     STA COL                \ space view
    
     LDY NOSTM              \ Set Y to the current number of stardust particles, so
                            \ we can use it as a counter through all the stardust
    
    .FLL1
    
     LDX SY,Y               \ Copy the Y-th particle's y-coordinate from SY+Y into X
    
     LDA SX,Y               \ Copy the Y-th particle's x-coordinate from SX+Y into
     STA Y1                 \ both Y1 and the particle's y-coordinate
     STA SY,Y
    
     TXA                    \ Copy the Y-th particle's original y-coordinate into
     STA X1                 \ both X1 and the particle's x-coordinate, so the x- and
     STA SX,Y               \ y-coordinates are now swapped and (X1, Y1) contains
                            \ the particle's new coordinates
    
     LDA SZ,Y               \ Fetch the Y-th particle's distance from SZ+Y into ZZ
     STA ZZ
    
     JSR PIXEL2             \ Draw a stardust particle at (X1,Y1) with distance ZZ
    
     DEY                    \ Decrement the counter to point to the next particle of
                            \ stardust
    
     BNE FLL1               \ Loop back to FLL1 until we have moved all the stardust
                            \ particles
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: STARS                                                   [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: The main routine for processing the stardust
    
    
        Context: See this subroutine [on its own page](../main/subroutine/stars.md)
     Variations: See [code variations](../related/compare/main/subroutine/stars.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md) calls STARS
    
    
    
    
    
    * * *
    
    
     Called at the very end of the main flight loop.
    
    
    
    
    .STARS
    
     LDA #DUST              \ Switch to stripe 3-2-3-2, which is cyan/red in the
     STA COL                \ space view
    
     LDX VIEW               \ Load the current view into X:
                            \
                            \   0 = front
                            \   1 = rear
                            \   2 = left
                            \   3 = right
    
     BEQ STARS1             \ If this is view 0, jump to STARS1 to process the
                            \ stardust for the front view
    
     DEX                    \ If this is view 2 or 3, jump to STARS2 (via ST11) to
     BNE ST11               \ process the stardust for the left or right views
    
     JMP STARS6             \ Otherwise this is the rear view, so jump to STARS6 to
                            \ process the stardust for the rear view
    
    .ST11
    
     JMP STARS2             \ Jump to STARS2 for the left or right views, as it's
                            \ too far for the branch instruction above
    
    
    
    
           Name: STARS1                                                  [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Process the stardust for the front view
      Deep dive: [Stardust in the front view](https://elite.bbcelite.com/deep_dives/stardust_in_the_front_view.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/stars1.md)
     Variations: See [code variations](../related/compare/main/subroutine/stars1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STARS](../main/subroutine/stars.md) calls STARS1
    
    
    
    
    
    * * *
    
    
     This moves the stardust towards us according to our speed (so the dust rushes
     past us), and applies our current pitch and roll to each particle of dust, so
     the stardust moves correctly when we steer our ship.
    
     When a stardust particle rushes past us and falls off the side of the screen,
     its memory is recycled as a new particle that's positioned randomly on-screen.
    
     These are the calculations referred to in the commentary:
    
       1. q = 64 * speed / z_hi
       2. z = z - speed * 64
       3. y = y + |y_hi| * q
       4. x = x + |x_hi| * q
    
       5. y = y + alpha * x / 256
       6. x = x - alpha * y / 256
    
       7. x = x + 2 * (beta * y / 256) ^ 2
       8. y = y - beta * 256
    
     For more information see the associated deep dive.
    
    
    
    
    .STARS1
    
     LDY NOSTM              \ Set Y to the current number of stardust particles, so
                            \ we can use it as a counter through all the stardust
    
                            \ In the following, we're going to refer to the 16-bit
                            \ space coordinates of the current particle of stardust
                            \ (i.e. the Y-th particle) like this:
                            \
                            \   x = (x_hi x_lo)
                            \   y = (y_hi y_lo)
                            \   z = (z_hi z_lo)
                            \
                            \ These values are stored in (SX+Y SXL+Y), (SY+Y SYL+Y)
                            \ and (SZ+Y SZL+Y) respectively
    
    .STL1
    
     JSR DV42               \ Call DV42 to set the following:
                            \
                            \   (P R) = 256 * DELTA / z_hi
                            \         = 256 * speed / z_hi
                            \
                            \ The maximum value returned is P = 2 and R = 128 (see
                            \ DV42 for an explanation)
    
     LDA R                  \ Set A = R, so now:
                            \
                            \   (P A) = 256 * speed / z_hi
    
     LSR P                  \ Rotate (P A) right by 2 places, which sets P = 0 (as P
     ROR A                  \ has a maximum value of 2) and leaves:
     LSR P                  \
     ROR A                  \   A = 64 * speed / z_hi
    
     ORA #1                 \ Make sure A is at least 1, and store it in Q, so we
     STA Q                  \ now have result 1 above:
                            \
                            \   Q = 64 * speed / z_hi
    
     LDA SZL,Y              \ We now calculate the following:
     SBC DELT4              \
     STA SZL,Y              \  (z_hi z_lo) = (z_hi z_lo) - DELT4(1 0)
                            \
                            \ starting with the low bytes
    
     LDA SZ,Y               \ And then we do the high bytes
     STA ZZ                 \
     SBC DELT4+1            \ We also set ZZ to the original value of z_hi, which we
     STA SZ,Y               \ use below to remove the existing particle
                            \
                            \ So now we have result 2 above:
                            \
                            \   z = z - DELT4(1 0)
                            \     = z - speed * 64
    
     JSR MLU1               \ Call MLU1 to set:
                            \
                            \   Y1 = y_hi
                            \
                            \   (A P) = |y_hi| * Q
                            \
                            \ So Y1 contains the original value of y_hi, which we
                            \ use below to remove the existing particle
    
                            \ We now calculate:
                            \
                            \   (S R) = YY(1 0) = (A P) + y
    
     STA YY+1               \ First we do the low bytes with:
     LDA P                  \
     ADC SYL,Y              \   YY+1 = A
     STA YY                 \   R = YY = P + y_lo
     STA R                  \
                            \ so we get this:
                            \
                            \   (? R) = YY(1 0) = (A P) + y_lo
    
     LDA Y1                 \ And then we do the high bytes with:
     ADC YY+1               \
     STA YY+1               \   S = YY+1 = y_hi + YY+1
     STA S                  \
                            \ so we get our result:
                            \
                            \   (S R) = YY(1 0) = (A P) + (y_hi y_lo)
                            \                   = |y_hi| * Q + y
                            \
                            \ which is result 3 above, and (S R) is set to the new
                            \ value of y
    
     LDA SX,Y               \ Set X1 = A = x_hi
     STA X1                 \
                            \ So X1 contains the original value of x_hi, which we
                            \ use below to remove the existing particle
    
     JSR MLU2               \ Set (A P) = |x_hi| * Q
    
                            \ We now calculate:
                            \
                            \   XX(1 0) = (A P) + x
    
     STA XX+1               \ First we do the low bytes:
     LDA P                  \
     ADC SXL,Y              \   XX(1 0) = (A P) + x_lo
     STA XX
    
     LDA X1                 \ And then we do the high bytes:
     ADC XX+1               \
     STA XX+1               \   XX(1 0) = XX(1 0) + (x_hi 0)
                            \
                            \ so we get our result:
                            \
                            \   XX(1 0) = (A P) + x
                            \           = |x_hi| * Q + x
                            \
                            \ which is result 4 above, and we also have:
                            \
                            \   A = XX+1 = (|x_hi| * Q + x) / 256
                            \
                            \ i.e. A is the new value of x, divided by 256
    
     EOR ALP2+1             \ EOR with the flipped sign of the roll angle alpha, so
                            \ A has the opposite sign to the flipped roll angle
                            \ alpha, i.e. it gets the same sign as alpha
    
     JSR MLS1               \ Call MLS1 to calculate:
                            \
                            \   (A P) = A * ALP1
                            \         = (x / 256) * alpha
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = (x / 256) * alpha + y
                            \         = y + alpha * x / 256
    
     STA YY+1               \ Set YY(1 0) = (A X) to give:
     STX YY                 \
                            \   YY(1 0) = y + alpha * x / 256
                            \
                            \ which is result 5 above, and we also have:
                            \
                            \   A = YY+1 = y + alpha * x / 256
                            \
                            \ i.e. A is the new value of y, divided by 256
    
     EOR ALP2               \ EOR A with the correct sign of the roll angle alpha,
                            \ so A has the opposite sign to the roll angle alpha
    
     JSR MLS2               \ Call MLS2 to calculate:
                            \
                            \   (S R) = XX(1 0)
                            \         = x
                            \
                            \   (A P) = A * ALP1
                            \         = -y / 256 * alpha
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = -y / 256 * alpha + x
    
     STA XX+1               \ Set XX(1 0) = (A X), which gives us result 6 above:
     STX XX                 \
                            \   x = x - alpha * y / 256
    
     LDX BET1               \ Fetch the pitch magnitude into X
    
     LDA YY+1               \ Set A to y_hi and set it to the flipped sign of beta
     EOR BET2+1
    
     JSR MULTS-2            \ Call MULTS-2 to calculate:
                            \
                            \   (A P) = X * A
                            \         = -beta * y_hi
    
     STA Q                  \ Store the high byte of the result in Q, so:
                            \
                            \   Q = -beta * y_hi / 256
    
     JSR MUT2               \ Call MUT2 to calculate:
                            \
                            \   (S R) = XX(1 0) = x
                            \
                            \   (A P) = Q * A
                            \         = (-beta * y_hi / 256) * (-beta * y_hi / 256)
                            \         = (beta * y / 256) ^ 2
    
     ASL P                  \ Double (A P), store the top byte in A and set the C
     ROL A                  \ flag to bit 7 of the original A, so this does:
     STA T                  \
                            \   (T P) = (A P) << 1
                            \         = 2 * (beta * y / 256) ^ 2
    
     LDA #0                 \ Set bit 7 in A to the sign bit from the A in the
     ROR A                  \ calculation above and apply it to T, so we now have:
     ORA T                  \
                            \   (A P) = (A P) * 2
                            \         = 2 * (beta * y / 256) ^ 2
                            \
                            \ with the doubling retaining the sign of (A P)
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = 2 * (beta * y / 256) ^ 2 + x
    
     STA XX+1               \ Store the high byte A in XX+1
    
     TXA                    \ Store the low byte X in x_lo
     STA SXL,Y
    
                            \ So (XX+1 x_lo) now contains:
                            \
                            \   x = x + 2 * (beta * y / 256) ^ 2
                            \
                            \ which is result 7 above
    
     LDA YY                 \ Set (S R) = YY(1 0) = y
     STA R                  \
     LDA YY+1               \ The call to MAD and the two store instructions are
    \JSR MAD                \ commented out in the original source
    \STA S
    \STX R
     STA S
    
     LDA #0                 \ Set P = 0
     STA P
    
     LDA BETA               \ Set A = -beta, so:
     EOR #%10000000         \
                            \   (A P) = (-beta 0)
                            \         = -beta * 256
    
     JSR PIX1               \ Call PIX1 to calculate the following:
                            \
                            \   (YY+1 y_lo) = (A P) + (S R)
                            \               = -beta * 256 + y
                            \
                            \ i.e. y = y - beta * 256, which is result 8 above
                            \
                            \ PIX1 also draws a particle at (X1, Y1) with distance
                            \ ZZ, which will remove the old stardust particle, as we
                            \ set X1, Y1 and ZZ to the original values for this
                            \ particle during the calculations above
    
                            \ We now have our newly moved stardust particle at
                            \ x-coordinate (XX+1 x_lo) and y-coordinate (YY+1 y_lo)
                            \ and distance z_hi, so we draw it if it's still on
                            \ screen, otherwise we recycle it as a new bit of
                            \ stardust and draw that
    
     LDA XX+1               \ Set X1 and x_hi to the high byte of XX in XX+1, so
     STA X1                 \ the new x-coordinate is in (x_hi x_lo) and the high
     STA SX,Y               \ byte is in X1
    
     AND #%01111111         \ If |x_hi| >= 120 then jump to KILL1 to recycle this
     CMP #120               \ particle, as it's gone off the side of the screen,
     BCS KILL1              \ and rejoin at STC1 with the new particle
    
     LDA YY+1               \ Set Y1 and y_hi to the high byte of YY in YY+1, so
     STA SY,Y               \ the new x-coordinate is in (y_hi y_lo) and the high
     STA Y1                 \ byte is in Y1
    
     AND #%01111111         \ If |y_hi| >= 120 then jump to KILL1 to recycle this
     CMP #120               \ particle, as it's gone off the top or bottom of the
     BCS KILL1              \ screen, and rejoin at STC1 with the new particle
    
     LDA SZ,Y               \ If z_hi < 16 then jump to KILL1 to recycle this
     CMP #16                \ particle, as it's so close that it's effectively gone
     BCC KILL1              \ past us, and rejoin at STC1 with the new particle
    
     STA ZZ                 \ Set ZZ to the z-coordinate in z_hi
    
    .STC1
    
     JSR PIXEL2             \ Draw a stardust particle at (X1,Y1) with distance ZZ,
                            \ i.e. draw the newly moved particle at (x_hi, y_hi)
                            \ with distance z_hi
    
     DEY                    \ Decrement the loop counter to point to the next
                            \ stardust particle
    
     BEQ P%+5               \ If we have just done the last particle, skip the next
                            \ instruction to return from the subroutine
    
     JMP STL1               \ We have more stardust to process, so jump back up to
                            \ STL1 for the next particle
    
     RTS                    \ Return from the subroutine
    
    .KILL1
    
                            \ Our particle of stardust just flew past us, so let's
                            \ recycle that particle, starting it at a random
                            \ position that isn't too close to the centre point
    
     JSR DORND              \ Set A and X to random numbers
    
     ORA #4                 \ Make sure A is at least 4 and store it in Y1 and y_hi,
     STA Y1                 \ so the new particle starts at least 4 pixels above or
     STA SY,Y               \ below the centre of the screen
    
     JSR DORND              \ Set A and X to random numbers
    
     ORA #8                 \ Make sure A is at least 8 and store it in X1 and x_hi,
     STA X1                 \ so the new particle starts at least 8 pixels either
     STA SX,Y               \ side of the centre of the screen
    
     JSR DORND              \ Set A and X to random numbers
    
     ORA #144               \ Make sure A is at least 144 and store it in ZZ and
     STA SZ,Y               \ z_hi so the new particle starts in the far distance
     STA ZZ
    
     LDA Y1                 \ Set A to the new value of y_hi. This has no effect as
                            \ STC1 starts with a jump to PIXEL2, which starts with a
                            \ LDA instruction
    
     JMP STC1               \ Jump up to STC1 to draw this new particle
    
    
    
    
           Name: STARS6                                                  [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Process the stardust for the rear view
      Deep dive: [Stardust in the front view](https://elite.bbcelite.com/deep_dives/stardust_in_the_front_view.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/stars6.md)
     Variations: See [code variations](../related/compare/main/subroutine/stars6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STARS](../main/subroutine/stars.md) calls STARS6
    
    
    
    
    
    * * *
    
    
     This routine is very similar to STARS1, which processes stardust for the front
     view. The main difference is that the direction of travel is reversed, so the
     signs in the calculations are different, as well as the order of the first
     batch of calculations.
    
     When a stardust particle falls away into the far distance, it is removed from
     the screen and its memory is recycled as a new particle, positioned randomly
     along one of the four edges of the screen.
    
     These are the calculations referred to in the commentary:
    
       1. q = 64 * speed / z_hi
       2. z = z - speed * 64
       3. y = y + |y_hi| * q
       4. x = x + |x_hi| * q
    
       5. y = y + alpha * x / 256
       6. x = x - alpha * y / 256
    
       7. x = x + 2 * (beta * y / 256) ^ 2
       8. y = y - beta * 256
    
     For more information see the associated deep dive.
    
    
    
    
    .STARS6
    
     LDY NOSTM              \ Set Y to the current number of stardust particles, so
                            \ we can use it as a counter through all the stardust
    
    .STL6
    
     JSR DV42               \ Call DV42 to set the following:
                            \
                            \   (P R) = 256 * DELTA / z_hi
                            \         = 256 * speed / z_hi
                            \
                            \ The maximum value returned is P = 2 and R = 128 (see
                            \ DV42 for an explanation)
    
     LDA R                  \ Set A = R, so now:
                            \
                            \   (P A) = 256 * speed / z_hi
    
     LSR P                  \ Rotate (P A) right by 2 places, which sets P = 0 (as P
     ROR A                  \ has a maximum value of 2) and leaves:
     LSR P                  \
     ROR A                  \   A = 64 * speed / z_hi
    
     ORA #1                 \ Make sure A is at least 1, and store it in Q, so we
     STA Q                  \ now have result 1 above:
                            \
                            \   Q = 64 * speed / z_hi
    
     LDA SX,Y               \ Set X1 = A = x_hi
     STA X1                 \
                            \ So X1 contains the original value of x_hi, which we
                            \ use below to remove the existing particle
    
     JSR MLU2               \ Set (A P) = |x_hi| * Q
    
                            \ We now calculate:
                            \
                            \   XX(1 0) = x - (A P)
    
     STA XX+1               \ First we do the low bytes:
     LDA SXL,Y              \
     SBC P                  \   XX(1 0) = x_lo - (A P)
     STA XX
    
     LDA X1                 \ And then we do the high bytes:
     SBC XX+1               \
     STA XX+1               \   XX(1 0) = (x_hi 0) - XX(1 0)
                            \
                            \ so we get our result:
                            \
                            \   XX(1 0) = x - (A P)
                            \           = x - |x_hi| * Q
                            \
                            \ which is result 2 above, and we also have:
    
     JSR MLU1               \ Call MLU1 to set:
                            \
                            \   Y1 = y_hi
                            \
                            \   (A P) = |y_hi| * Q
                            \
                            \ So Y1 contains the original value of y_hi, which we
                            \ use below to remove the existing particle
    
                            \ We now calculate:
                            \
                            \   (S R) = YY(1 0) = y - (A P)
    
     STA YY+1               \ First we do the low bytes with:
     LDA SYL,Y              \
     SBC P                  \   YY+1 = A
     STA YY                 \   R = YY = y_lo - P
     STA R                  \
                            \ so we get this:
                            \
                            \   (? R) = YY(1 0) = y_lo - (A P)
    
     LDA Y1                 \ And then we do the high bytes with:
     SBC YY+1               \
     STA YY+1               \   S = YY+1 = y_hi - YY+1
     STA S                  \
                            \ so we get our result:
                            \
                            \   (S R) = YY(1 0) = (y_hi y_lo) - (A P)
                            \                   = y - |y_hi| * Q
                            \
                            \ which is result 3 above, and (S R) is set to the new
                            \ value of y
    
     LDA SZL,Y              \ We now calculate the following:
     ADC DELT4              \
     STA SZL,Y              \  (z_hi z_lo) = (z_hi z_lo) + DELT4(1 0)
                            \
                            \ starting with the low bytes
    
     LDA SZ,Y               \ And then we do the high bytes
     STA ZZ                 \
     ADC DELT4+1            \ We also set ZZ to the original value of z_hi, which we
     STA SZ,Y               \ use below to remove the existing particle
                            \
                            \ So now we have result 4 above:
                            \
                            \   z = z + DELT4(1 0)
                            \     = z + speed * 64
    
     LDA XX+1               \ EOR x with the correct sign of the roll angle alpha,
     EOR ALP2               \ so A has the opposite sign to the roll angle alpha
    
     JSR MLS1               \ Call MLS1 to calculate:
                            \
                            \   (A P) = A * ALP1
                            \         = (-x / 256) * alpha
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = (-x / 256) * alpha + y
                            \         = y - alpha * x / 256
    
     STA YY+1               \ Set YY(1 0) = (A X) to give:
     STX YY                 \
                            \   YY(1 0) = y - alpha * x / 256
                            \
                            \ which is result 5 above, and we also have:
                            \
                            \   A = YY+1 = y - alpha * x / 256
                            \
                            \ i.e. A is the new value of y, divided by 256
    
     EOR ALP2+1             \ EOR with the flipped sign of the roll angle alpha, so
                            \ A has the opposite sign to the flipped roll angle
                            \ alpha, i.e. it gets the same sign as alpha
    
     JSR MLS2               \ Call MLS2 to calculate:
                            \
                            \   (S R) = XX(1 0)
                            \         = x
                            \
                            \   (A P) = A * ALP1
                            \         = y / 256 * alpha
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = y / 256 * alpha + x
    
     STA XX+1               \ Set XX(1 0) = (A X), which gives us result 6 above:
     STX XX                 \
                            \   x = x + alpha * y / 256
    
     LDA YY+1               \ Set A to y_hi and set it to the flipped sign of beta
     EOR BET2+1
    
     LDX BET1               \ Fetch the pitch magnitude into X
    
     JSR MULTS-2            \ Call MULTS-2 to calculate:
                            \
                            \   (A P) = X * A
                            \         = beta * y_hi
    
     STA Q                  \ Store the high byte of the result in Q, so:
                            \
                            \   Q = beta * y_hi / 256
    
     LDA XX+1               \ Set S = x_hi
     STA S
    
     EOR #%10000000         \ Flip the sign of A, so A now contains -x
    
     JSR MUT1               \ Call MUT1 to calculate:
                            \
                            \   R = XX = x_lo
                            \
                            \   (A P) = Q * A
                            \         = (beta * y_hi / 256) * (-beta * y_hi / 256)
                            \         = (-beta * y / 256) ^ 2
    
     ASL P                  \ Double (A P), store the top byte in A and set the C
     ROL A                  \ flag to bit 7 of the original A, so this does:
     STA T                  \
                            \   (T P) = (A P) << 1
                            \         = 2 * (-beta * y / 256) ^ 2
    
     LDA #0                 \ Set bit 7 in A to the sign bit from the A in the
     ROR A                  \ calculation above and apply it to T, so we now have:
     ORA T                  \
                            \   (A P) = -2 * (beta * y / 256) ^ 2
                            \
                            \ with the doubling retaining the sign of (A P)
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = -2 * (beta * y / 256) ^ 2 + x
    
     STA XX+1               \ Store the high byte A in XX+1
    
     TXA                    \ Store the low byte X in x_lo
     STA SXL,Y
    
                            \ So (XX+1 x_lo) now contains:
                            \
                            \   x = x - 2 * (beta * y / 256) ^ 2
                            \
                            \ which is result 7 above
    
     LDA YY                 \ Set (S R) = YY(1 0) = y
     STA R
     LDA YY+1
     STA S
    
    \EOR #128               \ These instructions are commented out in the original
    \JSR MAD                \ source
    \STA S
    \STX R
    
     LDA #0                 \ Set P = 0
     STA P
    
     LDA BETA               \ Set A = beta, so (A P) = (beta 0) = beta * 256
    
     JSR PIX1               \ Call PIX1 to calculate the following:
                            \
                            \   (YY+1 y_lo) = (A P) + (S R)
                            \               = beta * 256 + y
                            \
                            \ i.e. y = y + beta * 256, which is result 8 above
                            \
                            \ PIX1 also draws a particle at (X1, Y1) with distance
                            \ ZZ, which will remove the old stardust particle, as we
                            \ set X1, Y1 and ZZ to the original values for this
                            \ particle during the calculations above
    
                            \ We now have our newly moved stardust particle at
                            \ x-coordinate (XX+1 x_lo) and y-coordinate (YY+1 y_lo)
                            \ and distance z_hi, so we draw it if it's still on
                            \ screen, otherwise we recycle it as a new bit of
                            \ stardust and draw that
    
     LDA XX+1               \ Set X1 and x_hi to the high byte of XX in XX+1, so
     STA X1                 \ the new x-coordinate is in (x_hi x_lo) and the high
     STA SX,Y               \ byte is in X1
    
     LDA YY+1               \ Set Y1 and y_hi to the high byte of YY in YY+1, so
     STA SY,Y               \ the new x-coordinate is in (y_hi y_lo) and the high
     STA Y1                 \ byte is in Y1
    
     AND #%01111111         \ If |y_hi| >= 110 then jump to KILL6 to recycle this
     CMP #110               \ particle, as it's gone off the top or bottom of the
     BCS KILL6              \ screen, and rejoin at STC6 with the new particle
    
     LDA SZ,Y               \ If z_hi >= 160 then jump to KILL6 to recycle this
     CMP #160               \ particle, as it's so far away that it's too far to
     BCS KILL6              \ see, and rejoin at STC1 with the new particle
    
     STA ZZ                 \ Set ZZ to the z-coordinate in z_hi
    
    .STC6
    
     JSR PIXEL2             \ Draw a stardust particle at (X1,Y1) with distance ZZ,
                            \ i.e. draw the newly moved particle at (x_hi, y_hi)
                            \ with distance z_hi
    
     DEY                    \ Decrement the loop counter to point to the next
                            \ stardust particle
    
     BEQ ST3                \ If we have just done the last particle, skip the next
                            \ instruction to return from the subroutine
    
     JMP STL6               \ We have more stardust to process, so jump back up to
                            \ STL6 for the next particle
    
    .ST3
    
     RTS                    \ Return from the subroutine
    
    .KILL6
    
     JSR DORND              \ Set A and X to random numbers
    
     AND #%01111111         \ Clear the sign bit of A to get |A|
    
     ADC #10                \ Make sure A is at least 10 and store it in z_hi and
     STA SZ,Y               \ ZZ, so the new particle starts close to us
     STA ZZ
    
     LSR A                  \ Divide A by 2 and randomly set the C flag
    
     BCS ST4                \ Jump to ST4 half the time
    
     LSR A                  \ Randomly set the C flag again
    
     LDA #252               \ Set A to either +126 or -126 (252 >> 1) depending on
     ROR A                  \ the C flag, as this is a sign-magnitude number with
                            \ the C flag rotated into its sign bit
    
     STA X1                 \ Set x_hi and X1 to A, so this particle starts on
     STA SX,Y               \ either the left or right edge of the screen
    
     JSR DORND              \ Set A and X to random numbers
    
     STA Y1                 \ Set y_hi and Y1 to random numbers, so the particle
     STA SY,Y               \ starts anywhere along either the left or right edge
    
     JMP STC6               \ Jump up to STC6 to draw this new particle
    
    .ST4
    
     JSR DORND              \ Set A and X to random numbers
    
     STA X1                 \ Set x_hi and X1 to random numbers, so the particle
     STA SX,Y               \ starts anywhere along the x-axis
    
     LSR A                  \ Randomly set the C flag
    
     LDA #230               \ Set A to either +115 or -115 (230 >> 1) depending on
     ROR A                  \ the C flag, as this is a sign-magnitude number with
                            \ the C flag rotated into its sign bit
    
     STA Y1                 \ Set y_hi and Y1 to A, so the particle starts anywhere
     STA SY,Y               \ along either the top or bottom edge of the screen
    
     BNE STC6               \ Jump up to STC6 to draw this new particle (this BNE is
                            \ effectively a JMP as A will never be zero)
    
    
    
    
           Name: MAS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Add an orientation vector coordinate to an INWK coordinate
      Deep dive: [The space station safe zone](https://elite.bbcelite.com/deep_dives/the_space_station_safe_zone.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mas1.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md) calls MAS1
    
    
    
    
    
    * * *
    
    
     Add a doubled nosev vector coordinate, e.g. (nosev_y_hi nosev_y_lo) * 2, to
     an INWK coordinate, e.g. (x_sign x_hi x_lo), storing the result in the INWK
     coordinate. The axes used in each side of the addition are specified by the
     arguments X and Y.
    
     In the comments below, we document the routine as if we are doing the
     following, i.e. if X = 0 and Y = 11:
    
       (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (nosev_y_hi nosev_y_lo) * 2
    
     as that way the variable names in the comments contain "x" and "y" to match
     the registers that specify the vector axis to use.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The coordinate to add, as follows:
    
                              * If X = 0, add (x_sign x_hi x_lo)
                              * If X = 3, add (y_sign y_hi y_lo)
                              * If X = 6, add (z_sign z_hi z_lo)
    
       Y                    The vector to add, as follows:
    
                              * If Y = 9,  add (nosev_x_hi nosev_x_lo)
                              * If Y = 11, add (nosev_y_hi nosev_y_lo)
                              * If Y = 13, add (nosev_z_hi nosev_z_lo)
    
    
    
    * * *
    
    
     Returns:
    
       A                    The highest byte of the result with the sign cleared
                            (e.g. |x_sign| when X = 0, etc.)
    
    
    
    * * *
    
    
     Other entry points:
    
       MA9                  Contains an RTS
    
    
    
    
    .MAS1
    
     LDA INWK,Y             \ Set K(2 1) = (nosev_y_hi nosev_y_lo) * 2
     ASL A
     STA K+1
     LDA INWK+1,Y
     ROL A
     STA K+2
    
     LDA #0                 \ Set K+3 bit 7 to the C flag, so the sign bit of the
     ROR A                  \ above result goes into K+3
     STA K+3
    
     JSR MVT3               \ Add (x_sign x_hi x_lo) to K(3 2 1)
    
     STA INWK+2,X           \ Store the sign of the result in x_sign
    
     LDY K+1                \ Store K(2 1) in (x_hi x_lo)
     STY INWK,X
     LDY K+2
     STY INWK+1,X
    
     AND #%01111111         \ Set A to the sign byte with the sign cleared,
                            \ i.e. |x_sign| when X = 0
    
    .MA9
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MAS2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate a cap on the maximum distance to the planet or sun
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mas2.md)
     Variations: See [code variations](../related/compare/main/subroutine/mas2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md) calls MAS2
                 * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md) calls MAS2
                 * [WARP](../main/subroutine/warp.md) calls MAS2
                 * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md) calls via m
                 * [WARP](../main/subroutine/warp.md) calls via m
    
    
    
    
    
    * * *
    
    
     Given a value in Y that points to the start of a ship data block as an offset
     from K%, calculate the following:
    
       A = A OR x_sign OR y_sign OR z_sign
    
     and clear the sign bit of the result. The K% workspace contains the ship data
     blocks, so the offset in Y must be 0 or a multiple of NI% (as each block in
     K% contains NI% bytes).
    
     The result effectively contains a maximum cap of the three values (though it
     might not be one of the three input values - it's just guaranteed to be
     larger than all of them).
    
     If Y = 0 and A = 0, then this calculates the maximum cap of the highest byte
     containing the distance to the planet, as K%+2 = x_sign, K%+5 = y_sign and
     K%+8 = z_sign (the first slot in the K% workspace represents the planet).
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The offset from K% for the start of the ship data block
                            to use
    
    
    
    * * *
    
    
     Returns:
    
       A                    A OR K%+2+Y OR K%+5+Y OR K%+8+Y, with bit 7 cleared
    
    
    
    * * *
    
    
     Other entry points:
    
       m                    Do not include A in the calculation
    
    
    
    
    .m
    
     LDA #0                 \ Set A = 0 and fall through into MAS2 to calculate the
                            \ OR of the three bytes at K%+2+Y, K%+5+Y and K%+8+Y
    
    .MAS2
    
     ORA K%+2,Y             \ Set A = A OR x_sign OR y_sign OR z_sign
     ORA K%+5,Y
     ORA K%+8,Y
    
     AND #%01111111         \ Clear bit 7 in A
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MAS3                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate A = x_hi^2 + y_hi^2 + z_hi^2 in the K% block
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mas3.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md) calls MAS3
    
    
    
    
    
    * * *
    
    
     Given a value in Y that points to the start of a ship data block as an offset
     from K%, calculate the following:
    
       (A ?) = x_hi^2 + y_hi^2 + z_hi^2
    
     returning A = &FF if the calculation overflows a one-byte result. The K%
     workspace contains the ship data blocks, so the offset in Y must be 0 or a
     multiple of NI% (as each block in K% contains NI% bytes).
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The offset from K% for the start of the ship data block
                            to use
    
     Returns
    
       A                    The high byte of x_hi^2 + y_hi^2 + z_hi^2
    
       C flag               The overflow status (i.e. did the result fit into one
                            byte):
    
                              * Clear if the calculation didn't overflow
    
                              * Set if the calculation overflowed (in which case A
                                is set to &FF)
    
    
    
    
    .MAS3
    
     LDA K%+1,Y             \ Set (A P) = x_hi * x_hi
     JSR SQUA2
    
     STA R                  \ Store A (high byte of result) in R
    
     LDA K%+4,Y             \ Set (A P) = y_hi * y_hi
     JSR SQUA2
    
     ADC R                  \ Add A (high byte of second result) to R
    
     BCS MA30               \ If the addition of the two high bytes caused a carry
                            \ (i.e. they overflowed), jump to MA30 to return A = &FF
    
     STA R                  \ Store A (sum of the two high bytes) in R
    
     LDA K%+7,Y             \ Set (A P) = z_hi * z_hi
     JSR SQUA2
    
     ADC R                  \ Add A (high byte of third result) to R, so R now
                            \ contains the high byte of the entire sum, i.e. of
                            \ x_hi^2 + y_hi^2 + z_hi^2
    
     BCC P%+4               \ If there is no carry, skip the following instruction
                            \ to return straight from the subroutine
    
    .MA30
    
     LDA #&FF               \ The calculation has overflowed, so set A = &FF
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: STATUS                                                  [Show more]
           Type: Subroutine
       Category: Status
        Summary: Show the Status Mode screen (red key f8)
      Deep dive: [Combat rank](https://elite.bbcelite.com/deep_dives/combat_rank.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/status.md)
     Variations: See [code variations](../related/compare/main/subroutine/status.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls STATUS
    
    
    
    
    
    
    .wearedocked
    
                            \ We call this from STATUS below if we are docked
    
     LDA #205               \ Print extended token 205 ("DOCKED") and return from
     JSR DETOK              \ the subroutine using a tail call
    
     JSR TT67K              \ Print a newline
    
     JMP st6+3              \ Jump down to st6+3, to print recursive token 125 and
                            \ continue to the rest of the Status Mode screen
    
    .st4
    
                            \ We call this from st5 below with the high byte of the
                            \ kill tally in A, which is non-zero, and want to return
                            \ with the following in X, depending on our rating:
                            \
                            \   Competent = 6
                            \   Dangerous = 7
                            \   Deadly    = 8
                            \   Elite     = 9
                            \
                            \ The high bytes of the top tier ratings are as follows,
                            \ so this a relatively simple calculation:
                            \
                            \   Competent = 1
                            \   Dangerous = 2 to 9
                            \   Deadly    = 10 to 24
                            \   Elite     = 25 and up
    
     LDX #9                 \ Set X to 9 for an Elite rating
    
     CMP #25                \ If A >= 25, jump to st3 to print out our rating, as we
     BCS st3                \ are Elite
    
     DEX                    \ Decrement X to 8 for a Deadly rating
    
     CMP #10                \ If A >= 10, jump to st3 to print out our rating, as we
     BCS st3                \ are Deadly
    
     DEX                    \ Decrement X to 7 for a Dangerous rating
    
     CMP #2                 \ If A >= 2, jump to st3 to print out our rating, as we
     BCS st3                \ are Dangerous
    
     DEX                    \ Decrement X to 6 for a Competent rating
    
     BNE st3                \ Jump to st3 to print out our rating, as we are
                            \ Competent (this BNE is effectively a JMP as A will
                            \ never be zero)
    
    .STATUS
    
     LDA #8                 \ Clear the top part of the screen, draw a border box,
     JSR TRADEMODE          \ and set up a trading screen with a view type in QQ11
                            \ of 8 (Status Mode screen)
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     LDA #7                 \ Move the text cursor to column 7
     STA XC
    
     LDA #126               \ Print recursive token 126, which prints the top
     JSR NLIN3              \ four lines of the Status Mode screen:
                            \
                            \         COMMANDER {commander name}
                            \
                            \
                            \   Present System      : {current system name}
                            \   Hyperspace System   : {selected system name}
                            \   Condition           :
                            \
                            \ and draw a horizontal line at pixel row 19 to box
                            \ in the title
    
     LDA #15                \ This instruction is left over from the cassette
                            \ version, where it sets the token number for the
                            \ "DOCKED" text, but it has no effect in this version
                            \ as the "DOCKED" text is now an extended token
    
     LDY QQ12               \ Fetch the docked status from QQ12, and if we are
     BNE wearedocked        \ docked, jump to wearedocked
    
     LDA #230               \ Otherwise we are in space, so start off by setting A
                            \ to token 70 ("GREEN")
    
     LDY JUNK               \ Set Y to the number of junk items in our local bubble
                            \ of universe (where junk is asteroids, canisters,
                            \ escape pods and so on)
    
     LDX FRIN+2,Y           \ The ship slots at FRIN are ordered with the first two
                            \ slots reserved for the planet and sun/space station,
                            \ and then any ships, so if the slot at FRIN+2+Y is not
                            \ empty (i.e. is non-zero), then that means the number
                            \ of non-asteroids in the vicinity is at least 1
    
     BEQ st6                \ So if X = 0, there are no ships in the vicinity, so
                            \ jump to st6 to print "Green" for our ship's condition
    
     LDY ENERGY             \ Otherwise we have ships in the vicinity, so we load
                            \ our energy levels into Y
    
     CPY #128               \ Set the C flag if Y >= 128, so C is set if we have
                            \ more than half of our energy banks charged
    
     ADC #1                 \ Add 1 + C to A, so if C is not set (i.e. we have low
                            \ energy levels) then A is set to token 231 ("RED"),
                            \ and if C is set (i.e. we have healthy energy levels)
                            \ then A is set to token 232 ("YELLOW")
    
    .st6
    
     JSR plf                \ Print the text token in A (which contains our ship's
                            \ condition) followed by a newline
    
     LDA #125               \ Print recursive token 125, which prints the next
     JSR spc                \ three lines of the Status Mode screen:
                            \
                            \   Fuel: {fuel level} Light Years
                            \   Cash: {cash} Cr
                            \   Legal Status:
                            \
                            \ followed by a space
    
     LDA #19                \ Set A to token 133 ("CLEAN")
    
     LDY FIST               \ Fetch our legal status, and if it is 0, we are clean,
     BEQ st5                \ so jump to st5 to print "Clean"
    
     CPY #50                \ Set the C flag if Y >= 50, so C is set if we have
                            \ a legal status of 50+ (i.e. we are a fugitive)
    
     ADC #1                 \ Add 1 + C to A, so if C is not set (i.e. we have a
                            \ legal status between 1 and 49) then A is set to token
                            \ 134 ("OFFENDER"), and if C is set (i.e. we have a
                            \ legal status of 50+) then A is set to token 135
                            \ ("FUGITIVE")
    
    .st5
    
     JSR plf                \ Print the text token in A (which contains our legal
                            \ status) followed by a newline
    
     LDA #16                \ Print recursive token 130 ("RATING:") followed by a
     JSR spc                \ space
    
     LDA TALLY+1            \ Fetch the high byte of the kill tally, and if it is
     BNE st4                \ not zero, then we have more than 256 kill points, so
                            \ jump to st4 to work out whether we are Competent,
                            \ Dangerous, Deadly or Elite
    
                            \ Otherwise we have fewer than 256 kill pointss, so we
                            \ are one of Harmless, Mostly Harmless, Poor, Average,
                            \ Above Average or Competent
    
     TAX                    \ Set X to 0 (as A is 0)
    
     LDA TALLY              \ Set A to the lower byte of tally, with bits 0 and 1
     LSR A                  \ shifted off to the right, so we can now analyse bits
     LSR A                  \ 2 to 7 by shifting A to the right one bit at a time
    
                            \ We now loop through bits 2 to 7, shifting each of them
                            \ off the end of A until there are no set bits left, and
                            \ incrementing X before each shift, so at the end of the
                            \ process, X contains the position of the leftmost 1 in
                            \ A. Looking at the rank values in TALLY:
                            \
                            \   Harmless        = %00000000 to %00000111
                            \   Mostly Harmless = %00001000 to %00001111
                            \   Poor            = %00010000 to %00011111
                            \   Average         = %00100000 to %00111111
                            \   Above Average   = %01000000 to %01111111
                            \   Competent       = %10000000 to %11111111
                            \
                            \ we can see that the values returned by this process
                            \ are:
                            \
                            \   Harmless        = 1
                            \   Mostly Harmless = 2
                            \   Poor            = 3
                            \   Average         = 4
                            \   Above Average   = 5
                            \   Competent       = 6
    
     INX                    \ Increment X to count the number of shifts
    
     LSR A                  \ Shift A to the right
    
     BNE P%-2               \ Keep looping back two instructions (i.e. to the INX
                            \ instruction) until A = 0, which means there are no set
                            \ bits left in A
    
    .st3
    
     TXA                    \ A now contains our rating as a value of 1 to 9, so
                            \ transfer X to A, so we can print it out
    
     CLC                    \ Print recursive token 135 + A, which will be in the
     ADC #21                \ range 136 ("HARMLESS") to 144 ("---- E L I T E ----")
     JSR plf                \ followed by a newline
    
     LDA #18                \ Print recursive token 132, which prints the next bit
     JSR plf2               \ of the Status Mode screen:
                            \
                            \   EQUIPMENT:
                            \
                            \ followed by a newline and an indent of 6 characters
    
     LDA ESCP               \ If we don't have an escape pod fitted (i.e. ESCP is
     BEQ P%+7               \ zero), skip the following two instructions
    
     LDA #112               \ We do have an escape pod fitted, so print recursive
     JSR plf2               \ token 112 ("ESCAPE POD"), followed by a newline and an
                            \ indent of 6 characters
    
     LDA BST                \ If we don't have fuel scoops fitted, skip the
     BEQ P%+7               \ following two instructions
    
     LDA #111               \ We do have fuel scoops fitted, so print recursive
     JSR plf2               \ token 111 ("FUEL SCOOPS"), followed by a newline and
                            \ an indent of 6 characters
    
     LDA ECM                \ If we don't have an E.C.M. fitted, skip the following
     BEQ P%+7               \ two instructions
    
     LDA #108               \ We do have an E.C.M. fitted, so print recursive token
     JSR plf2               \ 108 ("E.C.M.SYSTEM"), followed by a newline and an
                            \ indent of 6 characters
    
     LDA #113               \ We now cover the four pieces of equipment whose flags
     STA XX4                \ are stored in BOMB through BOMB+3, and whose names
                            \ correspond with text tokens 113 through 116:
                            \
                            \   BOMB+0 = BOMB  = token 113 = Energy bomb
                            \   BOMB+1 = ENGY  = token 114 = Energy unit
                            \   BOMB+2 = DKCMP = token 115 = Docking computer
                            \   BOMB+3 = GHYP  = token 116 = Galactic hyperdrive
                            \
                            \ We can print these out using a loop, so we set XX4 to
                            \ 113 as a counter (and we also set A as well, to pass
                            \ through to plf2)
    
    .stqv
    
     TAY                    \ Fetch byte BOMB+0 through BOMB+4 for values of XX4
     LDX BOMB-113,Y         \ from 113 through 117
    
     BEQ P%+5               \ If it is zero then we do not own that piece of
                            \ equipment, so skip the next instruction
    
     JSR plf2               \ Print the recursive token in A from 113 ("ENERGY
                            \ BOMB") through 116 ("GALACTIC HYPERSPACE "), followed
                            \ by a newline and an indent of 6 characters
    
     INC XX4                \ Increment the counter (and A as well)
     LDA XX4
    
     CMP #117               \ If A < 117, loop back up to stqv to print the next
     BCC stqv               \ piece of equipment
    
     LDX #0                 \ Now to print our ship's lasers, so set a counter in X
                            \ to count through the four views (0 = front, 1 = rear,
                            \ 2 = left, 3 = right)
    
    .st
    
     STX CNT                \ Store the view number in CNT
    
     LDY LASER,X            \ Fetch the laser power for view X, and if we do not
     BEQ st1                \ have a laser fitted to that view, jump to st1 to move
                            \ on to the next one
    
     TXA                    \ Print recursive token 96 + X, which will print from 96
     CLC                    \ ("FRONT") through to 99 ("RIGHT"), followed by a space
     ADC #96
     JSR spc
    
     LDA #103               \ Set A to token 103 ("PULSE LASER")
    
     LDX CNT                \ Retrieve the view number from CNT that we stored above
    
     LDY LASER,X            \ Set Y = the laser power for view X
    
     CPY #128+POW           \ If the laser power for view X is not #POW+128 (beam
     BNE P%+4               \ laser), skip the next LDA instruction
    
     LDA #104               \ This sets A = 104 if the laser in view X is a beam
                            \ laser (token 104 is "BEAM LASER")
    
     CPY #Armlas            \ If the laser power for view X is not #Armlas (military
     BNE P%+4               \ laser), skip the next LDA instruction
    
     LDA #117               \ This sets A = 117 if the laser in view X is a military
                            \ laser (token 117 is "MILITARY  LASER")
    
     CPY #Mlas              \ If the laser power for view X is not #Mlas (mining
     BNE P%+4               \ laser), skip the next LDA instruction
    
     LDA #118               \ This sets A = 118 if the laser in view X is a mining
                            \ laser (token 118 is "MINING  LASER")
    
     JSR plf2               \ Print the text token in A (which contains the laser
                            \ type) followed by a newline and an indent of 6
                            \ characters
    
    .st1
    
     LDX CNT                \ Increment the counter in X and CNT to point to the
     INX                    \ next view
    
     CPX #4                 \ If this isn't the last of the four views, jump back up
     BCC st                 \ to st to print out the next one
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: plf2                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print text followed by a newline and indent of 6 characters
    
    
        Context: See this subroutine [on its own page](../main/subroutine/plf2.md)
     Variations: See [code variations](../related/compare/main/subroutine/plf2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STATUS](../main/subroutine/status.md) calls plf2
    
    
    
    
    
    * * *
    
    
     Print a text token followed by a newline, and indent the next line to text
     column 6.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .plf2
    
     JSR plf                \ Print the text token in A followed by a newline
    
     LDA #6                 \ Move the text cursor to column 6
     STA XC
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MVT3                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mvt3.md)
     References: This subroutine is called as follows:
                 * [MAS1](../main/subroutine/mas1.md) calls MVT3
                 * [MV40](../main/subroutine/mv40.md) calls MVT3
                 * [TAS1](../main/subroutine/tas1.md) calls MVT3
    
    
    
    
    
    * * *
    
    
     Add an INWK position coordinate - i.e. x, y or z - to K(3 2 1), like this:
    
       K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)
    
     The INWK coordinate to add to K(3 2 1) is specified by X.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The coordinate to add to K(3 2 1), as follows:
    
                              * If X = 0, add (x_sign x_hi x_lo)
    
                              * If X = 3, add (y_sign y_hi y_lo)
    
                              * If X = 6, add (z_sign z_hi z_lo)
    
    
    
    * * *
    
    
     Returns:
    
       A                    Contains a copy of the high byte of the result, K+3
    
       X                    X is preserved
    
    
    
    
    .MVT3
    
     LDA K+3                \ Set S = K+3
     STA S
    
     AND #%10000000         \ Set T = sign bit of K(3 2 1)
     STA T
    
     EOR INWK+2,X           \ If x_sign has a different sign to K(3 2 1), jump to
     BMI MV13               \ MV13 to process the addition as a subtraction
    
     LDA K+1                \ Set K(3 2 1) = K(3 2 1) + (x_sign x_hi x_lo)
     CLC                    \ starting with the low bytes
     ADC INWK,X
     STA K+1
    
     LDA K+2                \ Then the middle bytes
     ADC INWK+1,X
     STA K+2
    
     LDA K+3                \ And finally the high bytes
     ADC INWK+2,X
    
     AND #%01111111         \ Setting the sign bit of K+3 to T, the original sign
     ORA T                  \ of K(3 2 1)
     STA K+3
    
     RTS                    \ Return from the subroutine
    
    .MV13
    
     LDA S                  \ Set S = |K+3| (i.e. K+3 with the sign bit cleared)
     AND #%01111111
     STA S
    
     LDA INWK,X             \ Set K(3 2 1) = (x_sign x_hi x_lo) - K(3 2 1)
     SEC                    \ starting with the low bytes
     SBC K+1
     STA K+1
    
     LDA INWK+1,X           \ Then the middle bytes
     SBC K+2
     STA K+2
    
     LDA INWK+2,X           \ And finally the high bytes, doing A = |x_sign| - |K+3|
     AND #%01111111         \ and setting the C flag for testing below
     SBC S
    
     ORA #%10000000         \ Set the sign bit of K+3 to the opposite sign of T,
     EOR T                  \ i.e. the opposite sign to the original K(3 2 1)
     STA K+3
    
     BCS MV14               \ If the C flag is set, i.e. |x_sign| >= |K+3|, then
                            \ the sign of K(3 2 1). In this case, we want the
                            \ result to have the same sign as the largest argument,
                            \ which is (x_sign x_hi x_lo), which we know has the
                            \ opposite sign to K(3 2 1), and that's what we just set
                            \ the sign of K(3 2 1) to... so we can jump to MV14 to
                            \ return from the subroutine
    
     LDA #1                 \ We need to swap the sign of the result in K(3 2 1),
     SBC K+1                \ which we do by calculating 0 - K(3 2 1), which we can
     STA K+1                \ do with 1 - C - K(3 2 1), as we know the C flag is
                            \ clear. We start with the low bytes
    
     LDA #0                 \ Then the middle bytes
     SBC K+2
     STA K+2
    
     LDA #0                 \ And finally the high bytes
     SBC K+3
    
     AND #%01111111         \ Set the sign bit of K+3 to the same sign as T,
     ORA T                  \ i.e. the same sign as the original K(3 2 1), as
     STA K+3                \ that's the largest argument
    
    .MV14
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MVS5                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Apply a 3.6 degree pitch or roll to an orientation vector
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
                 [Pitching and rolling by a fixed angle](https://elite.bbcelite.com/deep_dives/pitching_and_rolling_by_a_fixed_angle.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mvs5.md)
     References: This subroutine is called as follows:
                 * [HAS1](../main/subroutine/has1.md) calls MVS5
                 * [MVEIT (Part 8 of 9)](../main/subroutine/mveit_part_8_of_9.md) calls MVS5
    
    
    
    
    
    * * *
    
    
     Pitch or roll a ship by a small, fixed amount (1/16 radians, or 3.6 degrees),
     in a specified direction, by rotating the orientation vectors. The vectors to
     rotate are given in X and Y, and the direction of the rotation is given in
     RAT2. The calculation is as follows:
    
       * If the direction is positive:
    
         X = X * (1 - 1/512) + Y / 16
         Y = Y * (1 - 1/512) - X / 16
    
       * If the direction is negative:
    
         X = X * (1 - 1/512) - Y / 16
         Y = Y * (1 - 1/512) + X / 16
    
     So if X = 15 (roofv_x), Y = 21 (sidev_x) and RAT2 is positive, it does this:
    
       roofv_x = roofv_x * (1 - 1/512)  + sidev_x / 16
       sidev_x = sidev_x * (1 - 1/512)  - roofv_x / 16
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The first vector to rotate:
    
                              * If X = 15, rotate roofv_x
    
                              * If X = 17, rotate roofv_y
    
                              * If X = 19, rotate roofv_z
    
                              * If X = 21, rotate sidev_x
    
                              * If X = 23, rotate sidev_y
    
                              * If X = 25, rotate sidev_z
    
       Y                    The second vector to rotate:
    
                              * If Y = 9,  rotate nosev_x
    
                              * If Y = 11, rotate nosev_y
    
                              * If Y = 13, rotate nosev_z
    
                              * If Y = 21, rotate sidev_x
    
                              * If Y = 23, rotate sidev_y
    
                              * If Y = 25, rotate sidev_z
    
       RAT2                 The direction of the pitch or roll to perform, positive
                            or negative (i.e. the sign of the roll or pitch counter
                            in bit 7)
    
    
    
    
    .MVS5
    
     LDA INWK+1,X           \ Fetch roofv_x_hi, clear the sign bit, divide by 2 and
     AND #%01111111         \ store in T, so:
     LSR A                  \
     STA T                  \ T = |roofv_x_hi| / 2
                            \   = |roofv_x| / 512
                            \
                            \ The above is true because:
                            \
                            \ |roofv_x| = |roofv_x_hi| * 256 + roofv_x_lo
                            \
                            \ so:
                            \
                            \ |roofv_x| / 512 = |roofv_x_hi| * 256 / 512
                            \                    + roofv_x_lo / 512
                            \                  = |roofv_x_hi| / 2
    
     LDA INWK,X             \ Now we do the following subtraction:
     SEC                    \
     SBC T                  \ (S R) = (roofv_x_hi roofv_x_lo) - |roofv_x| / 512
     STA R                  \       = (1 - 1/512) * roofv_x
                            \
                            \ by doing the low bytes first
    
     LDA INWK+1,X           \ And then the high bytes (the high byte of the right
     SBC #0                 \ side of the subtraction being 0)
     STA S
    
     LDA INWK,Y             \ Set P = nosev_x_lo
     STA P
    
     LDA INWK+1,Y           \ Fetch the sign of nosev_x_hi (bit 7) and store in T
     AND #%10000000
     STA T
    
     LDA INWK+1,Y           \ Fetch nosev_x_hi into A and clear the sign bit, so
     AND #%01111111         \ A = |nosev_x_hi|
    
     LSR A                  \ Set (A P) = (A P) / 16
     ROR P                  \           = |nosev_x_hi nosev_x_lo| / 16
     LSR A                  \           = |nosev_x| / 16
     ROR P
     LSR A
     ROR P
     LSR A
     ROR P
    
     ORA T                  \ Set the sign of A to the sign in T (i.e. the sign of
                            \ the original nosev_x), so now:
                            \
                            \ (A P) = nosev_x / 16
    
     EOR RAT2               \ Give it the sign as if we multiplied by the direction
                            \ by the pitch or roll direction
    
     STX Q                  \ Store the value of X so it can be restored after the
                            \ call to ADD
    
     JSR ADD                \ (A X) = (A P) + (S R)
                            \       = +/-nosev_x / 16 + (1 - 1/512) * roofv_x
    
     STA K+1                \ Set K(1 0) = (1 - 1/512) * roofv_x +/- nosev_x / 16
     STX K
    
     LDX Q                  \ Restore the value of X from before the call to ADD
    
     LDA INWK+1,Y           \ Fetch nosev_x_hi, clear the sign bit, divide by 2 and
     AND #%01111111         \ store in T, so:
     LSR A                  \
     STA T                  \ T = |nosev_x_hi| / 2
                            \   = |nosev_x| / 512
    
     LDA INWK,Y             \ Now we do the following subtraction:
     SEC                    \
     SBC T                  \ (S R) = (nosev_x_hi nosev_x_lo) - |nosev_x| / 512
     STA R                  \       = (1 - 1/512) * nosev_x
                            \
                            \ by doing the low bytes first
    
     LDA INWK+1,Y           \ And then the high bytes (the high byte of the right
     SBC #0                 \ side of the subtraction being 0)
     STA S
    
     LDA INWK,X             \ Set P = roofv_x_lo
     STA P
    
     LDA INWK+1,X           \ Fetch the sign of roofv_x_hi (bit 7) and store in T
     AND #%10000000
     STA T
    
     LDA INWK+1,X           \ Fetch roofv_x_hi into A and clear the sign bit, so
     AND #%01111111         \ A = |roofv_x_hi|
    
     LSR A                  \ Set (A P) = (A P) / 16
     ROR P                  \           = |roofv_x_hi roofv_x_lo| / 16
     LSR A                  \           = |roofv_x| / 16
     ROR P
     LSR A
     ROR P
     LSR A
     ROR P
    
     ORA T                  \ Set the sign of A to the opposite sign to T (i.e. the
     EOR #%10000000         \ sign of the original -roofv_x), so now:
                            \
                            \ (A P) = -roofv_x / 16
    
     EOR RAT2               \ Give it the sign as if we multiplied by the direction
                            \ by the pitch or roll direction
    
     STX Q                  \ Store the value of X so it can be restored after the
                            \ call to ADD
    
     JSR ADD                \ (A X) = (A P) + (S R)
                            \       = -/+roofv_x / 16 + (1 - 1/512) * nosev_x
    
     STA INWK+1,Y           \ Set nosev_x = (1-1/512) * nosev_x -/+ roofv_x / 16
     STX INWK,Y
    
     LDX Q                  \ Restore the value of X from before the call to ADD
    
     LDA K                  \ Set roofv_x = K(1 0)
     STA INWK,X             \             = (1-1/512) * roofv_x +/- nosev_x / 16
     LDA K+1
     STA INWK+1,X
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TENS                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A constant used when printing large numbers in BPRNT
      Deep dive: [Printing decimal numbers](https://elite.bbcelite.com/deep_dives/printing_decimal_numbers.html)
    
    
        Context: See this variable [on its own page](../main/variable/tens.md)
     References: This variable is used as follows:
                 * [BPRNT](../main/subroutine/bprnt.md) uses TENS
    
    
    
    
    
    * * *
    
    
     Contains the four low bytes of the value 100,000,000,000 (100 billion).
    
     The maximum number of digits that we can print with the BPRNT routine is 11,
     so the biggest number we can print is 99,999,999,999. This maximum number
     plus 1 is 100,000,000,000, which in hexadecimal is:
    
       17 48 76 E8 00
    
     The TENS variable contains the lowest four bytes in this number, with the
     most significant byte first, i.e. 48 76 E8 00. This value is used in the
     BPRNT routine when working out which decimal digits to print when printing a
     number.
    
    
    
    
    .TENS
    
     EQUD &00E87648
    
    
    
    
           Name: pr2                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print an 8-bit number, left-padded to 3 digits, and optional point
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pr2.md)
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls pr2
                 * [fwl](../main/subroutine/fwl.md) calls pr2
                 * [tal](../main/subroutine/tal.md) calls pr2
                 * [TT210](../main/subroutine/tt210.md) calls pr2
                 * [TT25](../main/subroutine/tt25.md) calls pr2
                 * [TT151](../main/subroutine/tt151.md) calls via pr2+2
    
    
    
    
    
    * * *
    
    
     Print the 8-bit number in X to 3 digits, left-padding with spaces for numbers
     with fewer than 3 digits (so numbers < 100 are right-aligned). Optionally
     include a decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The number to print
    
       C flag               If set, include a decimal point
    
    
    
    * * *
    
    
     Other entry points:
    
       pr2+2                Print the 8-bit number in X to the number of digits in A
    
    
    
    
    .pr2
    
     LDA #3                 \ Set A to the number of digits (3)
    
     LDY #0                 \ Zero the Y register, so we can fall through into TT11
                            \ to print the 16-bit number (Y X) to 3 digits, which
                            \ effectively prints X to 3 digits as the high byte is
                            \ zero
    
    
    
    
           Name: TT11                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a 16-bit number, left-padded to n digits, and optional point
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt11.md)
     References: This subroutine is called as follows:
                 * [ee3](../main/subroutine/ee3.md) calls TT11
                 * [EQSHP](../main/subroutine/eqshp.md) calls TT11
                 * [pr5](../main/subroutine/pr5.md) calls TT11
                 * [TT210](../main/subroutine/tt210.md) calls TT11
    
    
    
    
    
    * * *
    
    
     Print the 16-bit number in (Y X) to a specific number of digits, left-padding
     with spaces for numbers with fewer digits (so lower numbers will be right-
     aligned). Optionally include a decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The low byte of the number to print
    
       Y                    The high byte of the number to print
    
       A                    The number of digits
    
       C flag               If set, include a decimal point
    
    
    
    
    .TT11
    
     STA U                  \ We are going to use the BPRNT routine (below) to
                            \ print this number, so we store the number of digits
                            \ in U, as that's what BPRNT takes as an argument
    
     LDA #0                 \ BPRNT takes a 32-bit number in K to K+3, with the
     STA K                  \ most significant byte first (big-endian), so we set
     STA K+1                \ the two most significant bytes to zero (K and K+1)
     STY K+2                \ and store (Y X) in the least two significant bytes
     STX K+3                \ (K+2 and K+3), so we are going to print the 32-bit
                            \ number (0 0 Y X)
    
                            \ Finally we fall through into BPRNT to print out the
                            \ number in K to K+3, which now contains (Y X), to A
                            \ digits (as U = A), using the same C flag as when pr2
                            \ was called to control the decimal point
    
    
    
    
           Name: BPRNT                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a 32-bit number, left-padded to a specific number of digits,
                 with an optional decimal point
      Deep dive: [Printing decimal numbers](https://elite.bbcelite.com/deep_dives/printing_decimal_numbers.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bprnt.md)
     Variations: See [code variations](../related/compare/main/subroutine/bprnt.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [csh](../main/subroutine/csh.md) calls BPRNT
    
    
    
    
    
    * * *
    
    
     Print the 32-bit number stored in K(0 1 2 3) to a specific number of digits,
     left-padding with spaces for numbers with fewer digits (so lower numbers are
     right-aligned). Optionally include a decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       K(0 1 2 3)           The number to print, stored with the most significant
                            byte in K and the least significant in K+3 (i.e. as a
                            big-endian number, which is the opposite way to how the
                            6502 assembler stores addresses, for example)
    
       U                    The maximum number of digits to print, including the
                            decimal point (spaces will be used on the left to pad
                            out the result to this width, so the number is right-
                            aligned to this width). U must be 11 or less
    
       C flag               If set, include a decimal point followed by one
                            fractional digit (i.e. show the number to 1 decimal
                            place). In this case, the number in K(0 1 2 3) contains
                            10 * the number we end up printing, so to print 123.4,
                            we would pass 1234 in K(0 1 2 3) and would set the C
                            flag to include the decimal point
    
    
    
    
    .BPRNT
    
     LDX #11                \ Set T to the maximum number of digits allowed (11
     STX T                  \ characters, which is the number of digits in 10
                            \ billion). We will use this as a flag when printing
                            \ characters in TT37 below
    
     PHP                    \ Make a copy of the status register (in particular
                            \ the C flag) so we can retrieve it later
    
     BCC TT30               \ If the C flag is clear, we do not want to print a
                            \ decimal point, so skip the next two instructions
    
     DEC T                  \ As we are going to show a decimal point, decrement
     DEC U                  \ both the number of characters and the number of
                            \ digits (as one of them is now a decimal point)
    
    .TT30
    
     LDA #11                \ Set A to 11, the maximum number of digits allowed
    
     SEC                    \ Set the C flag so we can do subtraction without the
                            \ C flag affecting the result
    
     STA XX17               \ Store the maximum number of digits allowed (11) in
                            \ XX17
    
     SBC U                  \ Set U = 11 - U + 1, so U now contains the maximum
     STA U                  \ number of digits minus the number of digits we want
     INC U                  \ to display, plus 1 (so this is the number of digits
                            \ we should skip before starting to print the number
                            \ itself, and the plus 1 is there to ensure we print at
                            \ least one digit)
    
     LDY #0                 \ In the main loop below, we use Y to count the number
                            \ of times we subtract 10 billion to get the leftmost
                            \ digit, so set this to zero
    
     STY S                  \ In the main loop below, we use location S as an
                            \ 8-bit overflow for the 32-bit calculations, so
                            \ we need to set this to 0 before joining the loop
    
     JMP TT36               \ Jump to TT36 to start the process of printing this
                            \ number's digits
    
    .TT35
    
                            \ This subroutine multiplies K(S 0 1 2 3) by 10 and
                            \ stores the result back in K(S 0 1 2 3), using the fact
                            \ that K * 10 = (K * 2) + (K * 2 * 2 * 2)
    
     ASL K+3                \ Set K(S 0 1 2 3) = K(S 0 1 2 3) * 2 by rotating left
     ROL K+2
     ROL K+1
     ROL K
     ROL S
    
     LDX #3                 \ Now we want to make a copy of the newly doubled K in
                            \ XX15, so we can use it for the first (K * 2) in the
                            \ equation above, so set up a counter in X for copying
                            \ four bytes, starting with the last byte in memory
                            \ (i.e. the least significant)
    
    .tt35
    
     LDA K,X                \ Copy the X-th byte of K(0 1 2 3) to the X-th byte of
     STA XX15,X             \ XX15(0 1 2 3), so that XX15 will contain a copy of
                            \ K(0 1 2 3) once we've copied all four bytes
    
     DEX                    \ Decrement the loop counter
    
     BPL tt35               \ Loop back to copy the next byte until we have copied
                            \ all four
    
     LDA S                  \ Store the value of location S, our overflow byte, in
     STA XX15+4             \ XX15+4, so now XX15(4 0 1 2 3) contains a copy of
                            \ K(S 0 1 2 3), which is the value of (K * 2) that we
                            \ want to use in our calculation
    
     ASL K+3                \ Now to calculate the (K * 2 * 2 * 2) part. We still
     ROL K+2                \ have (K * 2) in K(S 0 1 2 3), so we just need to shift
     ROL K+1                \ it twice. This is the first one, so we do this:
     ROL K                  \
     ROL S                  \   K(S 0 1 2 3) = K(S 0 1 2 3) * 2 = K * 4
    
     ASL K+3                \ And then we do it again, so that means:
     ROL K+2                \
     ROL K+1                \   K(S 0 1 2 3) = K(S 0 1 2 3) * 2 = K * 8
     ROL K
     ROL S
    
     CLC                    \ Clear the C flag so we can do addition without the
                            \ C flag affecting the result
    
     LDX #3                 \ By now we've got (K * 2) in XX15(4 0 1 2 3) and
                            \ (K * 8) in K(S 0 1 2 3), so the final step is to add
                            \ these two 32-bit numbers together to get K * 10.
                            \ So we set a counter in X for four bytes, starting
                            \ with the last byte in memory (i.e. the least
                            \ significant)
    
    .tt36
    
     LDA K,X                \ Fetch the X-th byte of K into A
    
     ADC XX15,X             \ Add the X-th byte of XX15 to A, with carry
    
     STA K,X                \ Store the result in the X-th byte of K
    
     DEX                    \ Decrement the loop counter
    
     BPL tt36               \ Loop back to add the next byte, moving from the least
                            \ significant byte to the most significant, until we
                            \ have added all four
    
     LDA XX15+4             \ Finally, fetch the overflow byte from XX15(4 0 1 2 3)
    
     ADC S                  \ And add it to the overflow byte from K(S 0 1 2 3),
                            \ with carry
    
     STA S                  \ And store the result in the overflow byte from
                            \ K(S 0 1 2 3), so now we have our desired result, i.e.
                            \
                            \   K(S 0 1 2 3) = K(S 0 1 2 3) * 10
    
     LDY #0                 \ In the main loop below, we use Y to count the number
                            \ of times we subtract 10 billion to get the leftmost
                            \ digit, so set this to zero so we can rejoin the main
                            \ loop for another subtraction process
    
    .TT36
    
                            \ This is the main loop of our digit-printing routine.
                            \ In the following loop, we are going to count the
                            \ number of times that we can subtract 10 million and
                            \ store that count in Y, which we have already set to 0
    
     LDX #3                 \ Our first calculation concerns 32-bit numbers, so
                            \ set up a counter for a four-byte loop
    
     SEC                    \ Set the C flag so we can do subtraction without the
                            \ C flag affecting the result
    
    .tt37
    
                            \ We now loop through each byte in turn to do this:
                            \
                            \   XX15(4 0 1 2 3) = K(S 0 1 2 3) - 100,000,000,000
    
     LDA K,X                \ Subtract the X-th byte of TENS (i.e. 10 billion) from
     SBC TENS,X             \ the X-th byte of K
    
     STA XX15,X             \ Store the result in the X-th byte of XX15
    
     DEX                    \ Decrement the loop counter
    
     BPL tt37               \ Loop back to subtract the next byte, moving from the
                            \ least significant byte to the most significant, until
                            \ we have subtracted all four
    
     LDA S                  \ Subtract the fifth byte of 10 billion (i.e. &17) from
     SBC #&17               \ the fifth (overflow) byte of K, which is S
    
     STA XX15+4             \ Store the result in the overflow byte of XX15
    
     BCC TT37               \ If subtracting 10 billion took us below zero, jump to
                            \ TT37 to print out this digit, which is now in Y
    
     LDX #3                 \ We now want to copy XX15(4 0 1 2 3) back into
                            \ K(S 0 1 2 3), so we can loop back up to do the next
                            \ subtraction, so set up a counter for a four-byte loop
    
    .tt38
    
     LDA XX15,X             \ Copy the X-th byte of XX15(0 1 2 3) to the X-th byte
     STA K,X                \ of K(0 1 2 3), so that K(0 1 2 3) will contain a copy
                            \ of XX15(0 1 2 3) once we've copied all four bytes
    
     DEX                    \ Decrement the loop counter
    
     BPL tt38               \ Loop back to copy the next byte, until we have copied
                            \ all four
    
     LDA XX15+4             \ Store the value of location XX15+4, our overflow
     STA S                  \ byte in S, so now K(S 0 1 2 3) contains a copy of
                            \ XX15(4 0 1 2 3)
    
     INY                    \ We have now managed to subtract 10 billion from our
                            \ number, so increment Y, which is where we are keeping
                            \ a count of the number of subtractions so far
    
     JMP TT36               \ Jump back to TT36 to subtract the next 10 billion
    
    .TT37
    
     TYA                    \ If we get here then Y contains the digit that we want
                            \ to print (as Y has now counted the total number of
                            \ subtractions of 10 billion), so transfer Y into A
    
     BNE TT32               \ If the digit is non-zero, jump to TT32 to print it
    
     LDA T                  \ Otherwise the digit is zero. If we are already
                            \ printing the number then we will want to print a 0,
                            \ but if we haven't started printing the number yet,
                            \ then we probably don't, as we don't want to print
                            \ leading zeroes unless this is the only digit before
                            \ the decimal point
                            \
                            \ To help with this, we are going to use T as a flag
                            \ that tells us whether we have already started
                            \ printing digits:
                            \
                            \   * If T <> 0 we haven't printed anything yet
                            \
                            \   * If T = 0 then we have started printing digits
                            \
                            \ We initially set T above to the maximum number of
                            \ characters allowed, less 1 if we are printing a
                            \ decimal point, so the first time we enter the digit
                            \ printing routine at TT37, it is definitely non-zero
    
     BEQ TT32               \ If T = 0, jump straight to the print routine at TT32,
                            \ as we have already started printing the number, so we
                            \ definitely want to print this digit too
    
     DEC U                  \ We initially set U to the number of digits we want to
     BPL TT34               \ skip before starting to print the number. If we get
                            \ here then we haven't printed any digits yet, so
                            \ decrement U to see if we have reached the point where
                            \ we should start printing the number, and if not, jump
                            \ to TT34 to set up things for the next digit
    
     LDA #' '               \ We haven't started printing any digits yet, but we
     BNE tt34               \ have reached the point where we should start printing
                            \ our number, so call TT26 (via tt34) to print a space
                            \ so that the number is left-padded with spaces (this
                            \ BNE is effectively a JMP as A will never be zero)
    
    .TT32
    
     LDY #0                 \ We are printing an actual digit, so first set T to 0,
     STY T                  \ to denote that we have now started printing digits as
                            \ opposed to spaces
    
     CLC                    \ The digit value is in A, so add ASCII "0" to get the
     ADC #'0'               \ ASCII character number to print
    
    .tt34
    
     JSR TT26               \ Call TT26 to print the character in A and fall through
                            \ into TT34 to get things ready for the next digit
    
    .TT34
    
     DEC T                  \ Decrement T but keep T >= 0 (by incrementing it
     BPL P%+4               \ again if the above decrement made T negative)
     INC T
    
     DEC XX17               \ Decrement the total number of characters left to
                            \ print, which we stored in XX17
    
     BMI rT10               \ If the result is negative, we have printed all the
                            \ characters, so jump down to rT10 to return from the
                            \ subroutine
    
     BNE P%+10              \ If the result is positive (> 0) then we still have
                            \ characters left to print, so loop back to TT35 (via
                            \ the JMP TT35 instruction below) to print the next
                            \ digit
    
     PLP                    \ If we get here then we have printed the exact number
                            \ of digits that we wanted to, so restore the C flag
                            \ that we stored at the start of the routine
    
     BCC P%+7               \ If the C flag is clear, we don't want a decimal point,
                            \ so loop back to TT35 (via the JMP TT35 instruction
                            \ below) to print the next digit
    
     LDA #'.'               \ Otherwise the C flag is set, so print the decimal
     JSR TT26               \ point
    
     JMP TT35               \ Loop back to TT35 to print the next digit
    
    .rT10
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DTW1                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A mask for applying the lower case part of Sentence Case to
                 extended text tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/dtw1.md)
     References: This variable is used as follows:
                 * [DETOK2](../main/subroutine/detok2.md) uses DTW1
                 * [MT13](../main/subroutine/mt13.md) uses DTW1
                 * [MT2](../main/subroutine/mt2.md) uses DTW1
    
    
    
    
    
    * * *
    
    
     This variable is used to change characters to lower case as part of applying
     Sentence Case to extended text tokens. It has two values:
    
       * %00100000 = apply lower case to the second letter of a word onwards
    
       * %00000000 = do not change case to lower case
    
     The default value is %00100000 (apply lower case).
    
     The flag is set to %00100000 (apply lower case) by jump token 2, {sentence
     case}, which calls routine MT2 to change the value of DTW1.
    
     The flag is set to %00000000 (do not change case to lower case) by jump token
     1, {all caps}, which calls routine MT1 to change the value of DTW1.
    
     The letter to print is OR'd with DTW1 in DETOK2, which lower-cases the letter
     by setting bit 5 (if DTW1 is %00100000). However, this OR is only done if bit
     7 of DTW2 is clear, i.e. we are printing a word, so this doesn't affect the
     first letter of the word, which remains capitalised.
    
    
    
    
    .DTW1
    
     EQUB %00100000
    
    
    
    
           Name: DTW2                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A flag that indicates whether we are currently printing a word
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/dtw2.md)
     References: This variable is used as follows:
                 * [CLYNS](../main/subroutine/clyns.md) uses DTW2
                 * [DETOK2](../main/subroutine/detok2.md) uses DTW2
                 * [MT8](../main/subroutine/mt8.md) uses DTW2
                 * [TT26](../main/subroutine/tt26.md) uses DTW2
                 * [TTX66K](../main/subroutine/ttx66k.md) uses DTW2
    
    
    
    
    
    * * *
    
    
     This variable is used to indicate whether we are currently printing a word. It
     has two values:
    
       * 0 = we are currently printing a word
    
       * Non-zero = we are not currently printing a word
    
     The default value is %11111111 (we are not currently printing a word).
    
     The flag is set to %00000000 (we are currently printing a word) whenever a
     non-terminator character is passed to DASC for printing.
    
     The flag is set to %11111111 (we are not currently printing a word) whenever a
     terminator character (full stop, colon, carriage return, line feed, space) is
     passed to DASC for printing. It is also set to %11111111 by jump token 8,
     {tab 6}, which calls routine MT8 to change the value of DTW2, and to %10000000
     by TTX66 when we clear the screen.
    
    
    
    
    .DTW2
    
     EQUB %11111111
    
    
    
    
           Name: DTW3                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A flag for switching between standard and extended text tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/dtw3.md)
     References: This variable is used as follows:
                 * [DETOK2](../main/subroutine/detok2.md) uses DTW3
                 * [MT5](../main/subroutine/mt5.md) uses DTW3
    
    
    
    
    
    * * *
    
    
     This variable is used to indicate whether standard or extended text tokens
     should be printed by calls to DETOK. It allows us to mix standard tokens in
     with extended tokens. It has two values:
    
       * %00000000 = print extended tokens (i.e. those in TKN1 and RUTOK)
    
       * %11111111 = print standard tokens (i.e. those in QQ18)
    
     The default value is %00000000 (extended tokens).
    
     Standard tokens are set by jump token {6}, which calls routine MT6 to change
     the value of DTW3 to %11111111.
    
     Extended tokens are set by jump token {5}, which calls routine MT5 to change
     the value of DTW3 to %00000000.
    
    
    
    
    .DTW3
    
     EQUB %00000000
    
    
    
    
           Name: DTW4                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: Flags that govern how justified extended text tokens are printed
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/dtw4.md)
     References: This variable is used as follows:
                 * [MESS](../main/subroutine/mess.md) uses DTW4
                 * [MT15](../main/subroutine/mt15.md) uses DTW4
                 * [TT26](../main/subroutine/tt26.md) uses DTW4
    
    
    
    
    
    * * *
    
    
     This variable is used to control how justified text tokens are printed as part
     of the extended text token system. There are two bits that affect justified
     text:
    
       * Bit 7: 1 = justify text
                0 = do not justify text
    
       * Bit 6: 1 = buffer the entire token before printing, including carriage
                    returns (used for in-flight messages only)
                0 = print the contents of the buffer whenever a carriage return
                    appears in the token
    
     The default value is %00000000 (do not justify text, print buffer on carriage
     return).
    
     The flag is set to %10000000 (justify text, print buffer on carriage return)
     by jump token 14, {justify}, which calls routine MT14 to change the value of
     DTW4.
    
     The flag is set to %11000000 (justify text, buffer entire token) by routine
     MESS, which prints in-flight messages.
    
     The flag is set to %00000000 (do not justify text, print buffer on carriage
     return) by jump token 15, {left align}, which calls routine MT1 to change the
     value of DTW4.
    
    
    
    
    .DTW4
    
     EQUB 0
    
    
    
    
           Name: DTW5                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: The size of the justified text buffer at BUF
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/dtw5.md)
     References: This variable is used as follows:
                 * [HME2](../main/subroutine/hme2.md) uses DTW5
                 * [MESS](../main/subroutine/mess.md) uses DTW5
                 * [MT15](../main/subroutine/mt15.md) uses DTW5
                 * [MT17](../main/subroutine/mt17.md) uses DTW5
                 * [TT26](../main/subroutine/tt26.md) uses DTW5
    
    
    
    
    
    * * *
    
    
     When justified text is enabled by jump token 14, {justify}, during printing of
     extended text tokens, text is fed into a buffer at BUF instead of being
     printed straight away, so it can be padded out with spaces to justify the
     text. DTW5 contains the size of the buffer, so BUF + DTW5 points to the first
     free byte after the end of the buffer.
    
    
    
    
    .DTW5
    
     EQUB 0
    
    
    
    
           Name: DTW6                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A flag to denote whether printing in lower case is enabled for
                 extended text tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/dtw6.md)
     References: This variable is used as follows:
                 * [DETOK2](../main/subroutine/detok2.md) uses DTW6
                 * [MT13](../main/subroutine/mt13.md) uses DTW6
                 * [MT2](../main/subroutine/mt2.md) uses DTW6
    
    
    
    
    
    * * *
    
    
     This variable is used to indicate whether lower case is currently enabled. It
     has two values:
    
       * %10000000 = lower case is enabled
    
       * %00000000 = lower case is not enabled
    
     The default value is %00000000 (lower case is not enabled).
    
     The flag is set to %10000000 (lower case is enabled) by jump token 13 {lower
     case}, which calls routine MT10 to change the value of DTW6.
    
     The flag is set to %00000000 (lower case is not enabled) by jump token 1, {all
     caps}, and jump token 2, {sentence case}, which call routines MT1 and MT2 to
     change the value of DTW6.
    
    
    
    
    .DTW6
    
     EQUB %00000000
    
    
    
    
           Name: DTW8                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A mask for capitalising the next letter in an extended text token
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/dtw8.md)
     References: This variable is used as follows:
                 * [DETOK2](../main/subroutine/detok2.md) uses DTW8
                 * [MT19](../main/subroutine/mt19.md) uses DTW8
                 * [TT26](../main/subroutine/tt26.md) uses DTW8
    
    
    
    
    
    * * *
    
    
     This variable is only used by one specific extended token, the {single cap}
     jump token, which capitalises the next letter only. It has two values:
    
       * %11011111 = capitalise the next letter
    
       * %11111111 = do not change case
    
     The default value is %11111111 (do not change case).
    
     The flag is set to %11011111 (capitalise the next letter) by jump token 19,
     {single cap}, which calls routine MT19 to change the value of DTW.
    
     The flag is set to %11111111 (do not change case) at the start of DASC, after
     the letter has been capitalised in DETOK2, so the effect is to capitalise one
     letter only.
    
     The letter to print is AND'd with DTW8 in DETOK2, which capitalises the letter
     by clearing bit 5 (if DTW8 is %11011111). However, this AND is only done if at
     least one of the following is true:
    
       * Bit 7 of DTW2 is set (we are not currently printing a word)
    
       * Bit 7 of DTW6 is set (lower case has been enabled by jump token 13, {lower
         case}
    
     In other words, we only capitalise the next letter if it's the first letter in
     a word, or we are printing in lower case.
    
    
    
    
    .DTW8
    
     EQUB %11111111
    
    
    
    
           Name: FEED                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a newline
    
    
        Context: See this subroutine [on its own page](../main/subroutine/feed.md)
     References: This subroutine is called as follows:
                 * [GTDRV](../main/subroutine/gtdrv.md) calls FEED
    
    
    
    
    
    
    .FEED
    
     LDA #12                \ Set A = 12, so when we skip MT16 and fall through into
                            \ TT26, we print character 12, which is a newline
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &41, or BIT &41A9, which does nothing apart
                            \ from affect the flags
    
                            \ Fall through into TT26 (skipping MT16) to print the
                            \ newline character
    
    
    
    
           Name: MT16                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print the character in variable DTW7
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt16.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT16
    
    
    
    
    
    
    .MT16
    
     LDA #'A'               \ Set A to the contents of DTW7, as DTW7 points to the
                            \ second byte of this instruction, so updating DTW7 will
                            \ modify this instruction (the default value of DTW7 is
                            \ an "A")
    
     DTW7 = MT16 + 1        \ Point DTW7 to the second byte of the instruction above
                            \ so that modifying DTW7 changes the value loaded into A
    
                            \ Fall through into TT26 to print the character in A
    
    
    
    
           Name: TT26                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a character at the text cursor, with support for verified
                 text in extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt26.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt26.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BPRNT](../main/subroutine/bprnt.md) calls TT26
                 * [cmn](../main/subroutine/cmn.md) calls TT26
                 * [gnum](../main/subroutine/gnum.md) calls TT26
                 * [TITLE](../main/subroutine/title.md) calls TT26
                 * [TT102](../main/subroutine/tt102.md) calls TT26
                 * [TT160](../main/subroutine/tt160.md) calls TT26
                 * [TT161](../main/subroutine/tt161.md) calls TT26
                 * [TT16a](../main/subroutine/tt16a.md) calls TT26
                 * [TT214](../main/subroutine/tt214.md) calls TT26
                 * [TT25](../main/subroutine/tt25.md) calls TT26
                 * [TT42](../main/subroutine/tt42.md) calls TT26
                 * [TT74](../main/subroutine/tt74.md) calls TT26
                 * [DETOK2](../main/subroutine/detok2.md) calls via DASC
                 * [JMTB](../main/variable/jmtb.md) calls via DASC
                 * [OUTK](../main/subroutine/outk.md) calls via DASC
                 * [TT210](../main/subroutine/tt210.md) calls via DASC
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to print
    
    
    
    * * *
    
    
     Returns:
    
       X                    X is preserved
    
       C flag               The C flag is cleared
    
    
    
    * * *
    
    
     Other entry points:
    
       DASC                 DASC does exactly the same as TT26 and prints a
                            character at the text cursor, with support for verified
                            text in extended tokens
    
    
    
    
    .DASC
    
    .TT26
    
     STX SC                 \ Store X in SC, so we can retrieve it below
    
     LDX #%11111111         \ Set DTW8 = %11111111, to disable the effect of {19} if
     STX DTW8               \ it was set (as {19} capitalises one character only)
    
     CMP #'.'               \ If the character in A is a word terminator:
     BEQ DA8                \
     CMP #':'               \   * Full stop
     BEQ DA8                \   * Colon
     CMP #10                \   * Line feed
     BEQ DA8                \   * Carriage return
     CMP #12                \   * Space
     BEQ DA8                \
     CMP #' '               \ then skip the following instruction
     BEQ DA8
    
     INX                    \ Increment X to 0, so DTW2 gets set to %00000000 below
    
    .DA8
    
     STX DTW2               \ Store X in DTW2, so DTW2 is now:
                            \
                            \   * %00000000 if this character is a word terminator
                            \
                            \   * %11111111 if it isn't
                            \
                            \ so DTW2 indicates whether or not we are currently
                            \ printing a word
    
     LDX SC                 \ Retrieve the original value of X from SC
    
     BIT DTW4               \ If bit 7 of DTW4 is set then we are currently printing
     BMI P%+5               \ justified text, so skip the next instruction
    
     JMP CHPR               \ Bit 7 of DTW4 is clear, so jump down to CHPR to print
                            \ this character, as we are not printing justified text
    
                            \ If we get here then we are printing justified text, so
                            \ we need to buffer the text until we reach the end of
                            \ the paragraph, so we can then pad it out with spaces
    
     BIT DTW4               \ If bit 6 of DTW4 is set, then this is an in-flight
     BVS P%+6               \ message and we should buffer the carriage return
                            \ character {12}, so skip the following two instructions
    
     CMP #12                \ If the character in A is a carriage return, then we
     BEQ DA1                \ have reached the end of the paragraph, so jump down to
                            \ DA1 to print out the contents of the buffer,
                            \ justifying it as we go
    
                            \ If we get here then we need to buffer this character
                            \ in the line buffer at BUF
    
     LDX DTW5               \ DTW5 contains the current size of the buffer, so this
     STA BUF,X              \ stores the character in A at BUF + DTW5, the next free
                            \ space in the buffer
    
     LDX SC                 \ Retrieve the original value of X from SC so we can
                            \ preserve it through this subroutine call
    
     INC DTW5               \ Increment the size of the BUF buffer that is stored in
                            \ DTW5
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    
    .DA1
    
                            \ If we get here then we are justifying text and we have
                            \ reached the end of the paragraph, so we need to print
                            \ out the contents of the buffer, justifying it as we go
    
     TXA                    \ Store X and Y on the stack
     PHA
     TYA
     PHA
    
    .DA5
    
     LDX DTW5               \ Set X = DTW5, which contains the size of the buffer
    
     BEQ DA6+3              \ If X = 0 then the buffer is empty, so jump down to
                            \ DA6+3 to print a newline
    
     CPX #(LL+1)            \ If X < LL+1, i.e. X <= LL, then the buffer contains
     BCC DA6                \ fewer than LL characters, which is less than a line
                            \ length, so jump down to DA6 to print the contents of
                            \ BUF followed by a newline, as we don't justify the
                            \ last line of the paragraph
    
                            \ Otherwise X > LL, so the buffer does not fit into one
                            \ line, and we therefore need to justify the text, which
                            \ we do one line at a time
    
     LSR SC+1               \ Shift SC+1 to the right, which clears bit 7 of SC+1,
                            \ so we pass through the following comparison on the
                            \ first iteration of the loop and set SC+1 to %01000000
    
    .DA11
    
     LDA SC+1               \ If bit 7 of SC+1 is set, skip the following two
     BMI P%+6               \ instructions
    
     LDA #%01000000         \ Set SC+1 = %01000000
     STA SC+1
    
     LDY #(LL-1)            \ Set Y = line length, so we can loop backwards from the
                            \ end of the first line in the buffer using Y as the
                            \ loop counter
    
    .DAL1
    
     LDA BUF+LL             \ If the LL-th byte in BUF is a space, jump down to DA2
     CMP #' '               \ to print out the first line from the buffer, as it
     BEQ DA2                \ fits the line width exactly (i.e. it's justified)
    
                            \ We now want to find the last space character in the
                            \ first line in the buffer, so we loop through the line
                            \ using Y as a counter
    
    .DAL2
    
     DEY                    \ Decrement the loop counter in Y
    
     BMI DA11               \ If Y <= 0, loop back to DA11, as we have now looped
     BEQ DA11               \ through the whole line
    
     LDA BUF,Y              \ If the Y-th byte in BUF is not a space, loop back up
     CMP #' '               \ to DAL2 to check the next character
     BNE DAL2
    
                            \ Y now points to a space character in the line buffer
    
     ASL SC+1               \ Shift SC+1 to the left
    
     BMI DAL2               \ If bit 7 of SC+1 is set, jump to DAL2 to find the next
                            \ space character
    
                            \ We now want to insert a space into the line buffer at
                            \ position Y, which we do by shifting every character
                            \ after position Y along by 1, and then inserting the
                            \ space
    
     STY SC                 \ Store Y in SC, so we want to insert the space at
                            \ position SC
    
     LDY DTW5               \ Fetch the buffer size from DTW5 into Y, to act as a
                            \ loop counter for moving the line buffer along by 1
    
    .DAL6
    
     LDA BUF,Y              \ Copy the Y-th character from BUF into the Y+1-th
     STA BUF+1,Y            \ position
    
     DEY                    \ Decrement the loop counter in Y
    
     CPY SC                 \ Loop back to shift the next character along, until we
     BCS DAL6               \ have moved the SC-th character (i.e. Y < SC)
    
     INC DTW5               \ Increment the buffer size in DTW5
    
    \LDA #' '               \ This instruction is commented out in the original
                            \ source, as it has no effect because A already contains
                            \ ASCII " ". This is because the last character that is
                            \ tested in the above loop is at position SC, which we
                            \ know contains a space, so we know A contains a space
                            \ character when the loop finishes
    
                            \ We've now shifted the line to the right by 1 from
                            \ position SC onwards, so SC and SC+1 both contain
                            \ spaces, and Y is now SC-1 as we did a DEY just before
                            \ the end of the loop - in other words, we have inserted
                            \ a space at position SC, and Y points to the character
                            \ before the newly inserted space
    
                            \ We now want to move the pointer Y left to find the
                            \ next space in the line buffer, before looping back to
                            \ check whether we are done, and if not, insert another
                            \ space
    
    .DAL3
    
     CMP BUF,Y              \ If the character at position Y is not a space, jump to
     BNE DAL1               \ DAL1 to see whether we have now justified the line
    
     DEY                    \ Decrement the loop counter in Y
    
     BPL DAL3               \ Loop back to check the next character to the left,
                            \ until we have found a space
    
     BMI DA11               \ Jump back to DA11 (this BMI is effectively a JMP as
                            \ we already passed through a BPL to get here)
    
    .DA2
    
                            \ This subroutine prints out a full line of characters
                            \ from the start of the line buffer in BUF, followed by
                            \ a newline. It then removes that line from the buffer,
                            \ shuffling the rest of the buffer contents down
    
     LDX #LL                \ Call DAS1 to print out the first LL characters from
     JSR DAS1               \ the line buffer in BUF
    
     LDA #12                \ Print a newline
     JSR CHPR
    
     LDA DTW5               \ Subtract #LL from the end-of-buffer pointer in DTW5
    \CLC                    \
     SBC #LL                \ The CLC instruction is commented out in the original
     STA DTW5               \ source. It isn't needed as CHPR clears the C flag
    
     TAX                    \ Copy the new value of DTW5 into X
    
     BEQ DA6+3              \ If DTW5 = 0 then jump down to DA6+3 to print a newline
                            \ as the buffer is now empty
    
                            \ If we get here then we have printed our line but there
                            \ is more in the buffer, so we now want to remove the
                            \ line we just printed from the start of BUF
    
     LDY #0                 \ Set Y = 0 to count through the characters in BUF
    
     INX                    \ Increment X, so it now contains the number of
                            \ characters in the buffer (as DTW5 is a zero-based
                            \ pointer and is therefore equal to the number of
                            \ characters minus 1)
    
    .DAL4
    
     LDA BUF+LL+1,Y         \ Copy the Y-th character from BUF+LL to BUF
     STA BUF,Y
    
     INY                    \ Increment the character pointer
    
     DEX                    \ Decrement the character count
    
     BNE DAL4               \ Loop back to copy the next character until we have
                            \ shuffled down the whole buffer
    
     BEQ DA5                \ Jump back to DA5 (this BEQ is effectively a JMP as we
                            \ have already passed through the BNE above)
    
    .DAS1
    
                            \ This subroutine prints out X characters from BUF,
                            \ returning with X = 0
    
     LDY #0                 \ Set Y = 0 to point to the first character in BUF
    
    .DAL5
    
     LDA BUF,Y              \ Print the Y-th character in BUF using CHPR, which also
     JSR CHPR               \ clears the C flag for when we return from the
                            \ subroutine below
    
     INY                    \ Increment Y to point to the next character
    
     DEX                    \ Decrement the loop counter
    
     BNE DAL5               \ Loop back for the next character until we have printed
                            \ X characters from BUF
    
    .dec27
    
     RTS                    \ Return from the subroutine
    
    .DA6
    
     JSR DAS1               \ Call DAS1 to print X characters from BUF, returning
                            \ with X = 0
    
     STX DTW5               \ Set the buffer size in DTW5 to 0, as the buffer is now
                            \ empty
    
     PLA                    \ Restore Y and X from the stack
     TAY
     PLA
     TAX
    
     LDA #12                \ Set A = 12, so when we skip BELL and fall through into
                            \ CHPR, we print character 12, which is a newline
    
    .DA7
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &07, or BIT &07A9, which does nothing apart
                            \ from affect the flags
    
                            \ Fall through into CHPR (skipping BELL) to print the
                            \ character and return with the C flag cleared
    
    
    
    
           Name: BELL                                                    [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Make a standard system beep
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bell.md)
     Variations: See [code variations](../related/compare/main/subroutine/bell.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DK4](../main/subroutine/dk4.md) calls BELL
                 * [DKS3](../main/subroutine/dks3.md) calls BELL
    
    
    
    
    
    * * *
    
    
     This is the standard system beep, as made by the ASCII 7 "BELL" control code.
    
    
    
    
    .BELL
    
     LDA #7                 \ Control code 7 makes a beep, so load this into A
    
     JMP CHPR               \ Call the CHPR print routine to actually make the sound
    
    
    
    
           Name: ESCAPE                                                  [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Launch our escape pod
    
    
        Context: See this subroutine [on its own page](../main/subroutine/escape.md)
     Variations: See [code variations](../related/compare/main/subroutine/escape.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls ESCAPE
    
    
    
    
    
    * * *
    
    
     This routine displays our doomed Cobra Mk III disappearing off into the ether
     before arranging our replacement ship. Called when we press ESCAPE during
     flight and have an escape pod fitted.
    
    
    
    
    .ESCAPE
    
     JSR RES2               \ Reset a number of flight variables and workspaces
    
     LDX #CYL               \ Set the current ship type to a Cobra Mk III, so we
     STX TYPE               \ can show our ship disappear into the distance when we
                            \ eject in our pod
    
     JSR FRS1               \ Call FRS1 to launch the Cobra Mk III straight ahead,
                            \ like a missile launch, but with our ship instead
    
     BCS ES1                \ If the Cobra was successfully added to the local
                            \ bubble, jump to ES1 to skip the following instructions
    
     LDX #CYL2              \ The Cobra wasn't added to the local bubble for some
     JSR FRS1               \ reason, so try launching a pirate Cobra Mk III instead
    
    .ES1
    
     LDA #8                 \ Set the Cobra's byte #27 (speed) to 8
     STA INWK+27
    
     LDA #194               \ Set the Cobra's byte #30 (pitch counter) to 194, so it
     STA INWK+30            \ pitches up as we pull away
    
     LSR A                  \ Set the Cobra's byte #32 (AI flag) to %01100001, so it
     STA INWK+32            \ has no AI, and we can use this value as a counter to
                            \ do the following loop 97 times
    
    .ESL1
    
     JSR MVEIT              \ Call MVEIT to move the Cobra in space
    
     LDA QQ11               \ If either of QQ11 or VIEW is non-zero (i.e. this is
     ORA VIEW               \ not the front space view), skip the following
     BNE P%+5               \ instruction
    
     JSR LL9                \ Call LL9 to draw the Cobra on-screen
    
     DEC INWK+32            \ Decrement the counter in byte #32
    
     BNE ESL1               \ Loop back to keep moving the Cobra until the AI flag
                            \ is 0, which gives it time to drift away from our pod
    
     JSR SCAN               \ Call SCAN to remove the Cobra from the scanner (by
                            \ redrawing it)
    
     LDA #0                 \ Set A = 0 so we can use it to zero the contents of
                            \ the cargo hold
    
     LDX #16                \ We lose all our cargo when using our escape pod, so
                            \ up a counter in X so we can zero the 17 cargo slots
                            \ in QQ20
    
    .ESL2
    
     STA QQ20,X             \ Set the X-th byte of QQ20 to zero, so we no longer
                            \ have any of item type X in the cargo hold
    
     DEX                    \ Decrement the counter
    
     BPL ESL2               \ Loop back to ESL2 until we have emptied the entire
                            \ cargo hold
    
     STA FIST               \ Launching an escape pod also clears our criminal
                            \ record, so set our legal status in FIST to 0 ("clean")
    
     STA ESCP               \ The escape pod is a one-use item, so set ESCP to 0 so
                            \ we no longer have one fitted
    
    \LDA TRIBBLE            \ These instructions are commented out in the original
    \ORA TRIBBLE+1          \ source
    \BEQ nosurviv           \
    \                       \ They ensure that in games with the Trumble mission,
    \JSR DORND              \ at least one Trumble will hitch a ride in the escape
    \AND #7                 \ pod (so using an escape pod is not a solution to the
    \ORA #1                 \ trouble with Trumbles)
    \STA TRIBBLE            \
    \LDA #0                 \ This version of Elite does not contain the Trumble
    \STA TRIBBLE+1          \ mission, so the code is disabled
    \
    \.nosurviv
    
     LDA #70                \ Our replacement ship is delivered with a full tank of
     STA QQ14               \ fuel, so set the current fuel level in QQ14 to 70, or
                            \ 7.0 light years
    
     JMP GOIN               \ Go to the docking bay (i.e. show the ship hangar
                            \ screen) and return from the subroutine with a tail
                            \ call
    
    
    
    
           Name: HME2                                                    [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Search the galaxy for a system
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hme2.md)
     Variations: See [code variations](../related/compare/main/subroutine/hme2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls HME2
    
    
    
    
    
    
    .HME2
    
     LDA #CYAN              \ Switch to colour 3, which is white in the chart view
     STA COL
    
     LDA #14                \ Print extended token 14 ("{clear bottom of screen}
     JSR DETOK              \ PLANET NAME?{fetch line input from keyboard}"). The
                            \ last token calls MT26, which puts the entered search
                            \ term in INWK+5 and the term length in Y
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will erase the crosshairs currently there
    
     JSR TT81               \ Set the seeds in QQ15 (the selected system) to those
                            \ of system 0 in the current galaxy (i.e. copy the seeds
                            \ from QQ21 to QQ15)
    
     LDA #0                 \ We now loop through the galaxy's systems in order,
     STA XX20               \ until we find a match, so set XX20 to act as a system
                            \ counter, starting with system 0
    
    .HME3
    
     JSR MT14               \ Switch to justified text when printing extended
                            \ tokens, so the call to cpl prints into the justified
                            \ text buffer at BUF instead of the screen, and DTW5
                            \ gets set to the length of the system name
    
     JSR cpl                \ Print the selected system name into the justified text
                            \ buffer
    
     LDX DTW5               \ Fetch DTW5 into X, so X is now equal to the length of
                            \ the selected system name
    
     LDA INWK+5,X           \ Fetch the X-th character from the entered search term
    
     CMP #13                \ If the X-th character is not a carriage return, then
     BNE HME6               \ the selected system name and the entered search term
                            \ are different lengths, so jump to HME6 to move on to
                            \ the next system
    
    .HME4
    
     DEX                    \ Decrement X so it points to the last letter of the
                            \ selected system name (and, when we loop back here, it
                            \ points to the next letter to the left)
    
     LDA INWK+5,X           \ Set A to the X-th character of the entered search term
    
     ORA #%00100000         \ Set bit 5 of the character to make it lower case
    
     CMP BUF,X              \ If the character in A matches the X-th character of
     BEQ HME4               \ the selected system name in BUF, loop back to HME4 to
                            \ check the next letter to the left
    
     TXA                    \ The last comparison didn't match, so copy the letter
     BMI HME5               \ number into A, and if it's negative, that means we
                            \ managed to go past the first letters of each term
                            \ before we failed to get a match, so the terms are the
                            \ same, so jump to HME5 to process a successful search
    
    .HME6
    
                            \ If we get here then the selected system name and the
                            \ entered search term did not match
    
     JSR TT20               \ We want to move on to the next system, so call TT20
                            \ to twist the three 16-bit seeds in QQ15
    
     INC XX20               \ Increment the system counter in XX20
    
     BNE HME3               \ If we haven't yet checked all 256 systems in the
                            \ current galaxy, loop back to HME3 to check the next
                            \ system
    
                            \ If we get here then the entered search term did not
                            \ match any systems in the current galaxy
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10), so we can put the crosshairs back where
                            \ they were before the search
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10)
    
     JSR BOOP               \ Call the BOOP routine to make a low, long beep to
                            \ indicate a failed search
    
     LDA #215               \ Print extended token 215 ("{left align} UNKNOWN
     JMP DETOK              \ PLANET"), which will print on-screen as the left align
                            \ code disables justified text, and return from the
                            \ subroutine using a tail call
    
    .HME5
    
                            \ If we get here then we have found a match for the
                            \ entered search
    
     LDA QQ15+3             \ The x-coordinate of the system described by the seeds
     STA QQ9                \ in QQ15 is in QQ15+3 (s1_hi), so we copy this to QQ9
                            \ as the x-coordinate of the search result
    
     LDA QQ15+1             \ The y-coordinate of the system described by the seeds
     STA QQ10               \ in QQ15 is in QQ15+1 (s0_hi), so we copy this to QQ10
                            \ as the y-coordinate of the search result
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10)
    
     JSR MT15               \ Switch to left-aligned text when printing extended
                            \ tokens so future tokens will print to the screen (as
                            \ this disables justified text)
    
     JMP T95                \ Jump to T95 to print the distance to the selected
                            \ system and return from the subroutine using a tail
                            \ call
    
    
    
     Save ELTB.bin
    
    
    
    
     PRINT "ELITE B"
     PRINT "Assembled at ", ~CODE_B%
     PRINT "Ends at ", ~P%
     PRINT "Code size is ", ~(P% - CODE_B%)
     PRINT "Execute at ", ~LOAD%
     PRINT "Reload at ", ~LOAD_B%
    
     PRINT "S.ELTB ", ~CODE_B%, " ", ~P%, " ", ~LOAD%, " ", ~LOAD_B%
    \SAVE "3-assembled-output/ELTB.bin", CODE_B%, P%, LOAD%
    
    
    

[X]

Subroutine [ADD](elite_c.md#header-add) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Variable [ALP2](workspaces.md#alp2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Configuration variable [Armlas](workspaces.md#armlas)

Military laser power

[X]

Variable [BET1](workspaces.md#bet1) in workspace [ZP](workspaces.md#header-zp)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BET2](workspaces.md#bet2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of BET2 = sign of the pitch angle in BETA

[X]

Variable [BETA](workspaces.md#beta) in workspace [ZP](workspaces.md#header-zp)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Label [BL1](elite_b.md#bl1) in subroutine [BLINE](elite_b.md#header-bline)

[X]

Label [BL5](elite_b.md#bl5) in subroutine [BLINE](elite_b.md#header-bline)

[X]

Label [BL7](elite_b.md#bl7) in subroutine [BLINE](elite_b.md#header-bline)

[X]

Label [BL8](elite_b.md#bl8) in subroutine [BLINE](elite_b.md#header-bline)

[X]

Label [BL9](elite_b.md#bl9) in subroutine [BLINE](elite_b.md#header-bline)

[X]

Variable [BOMB](workspaces.md#bomb) in workspace [WP](workspaces.md#header-wp)

Energy bomb

[X]

Subroutine [BOOP](elite_a.md#header-boop) (category: Sound)

Make a long, low beep

[X]

Variable [BST](workspaces.md#bst) in workspace [WP](workspaces.md#header-wp)

Fuel scoops (BST stands for "barrel status")

[X]

Variable [BUF](workspaces.md#buf) in workspace [WP](workspaces.md#header-wp)

The line buffer used by DASC to print justified text

[X]

Subroutine [CHPR](elite_a.md#header-chpr) (category: Text)

Print a character at the text cursor by poking into screen memory

[X]

Variable [CNT](workspaces.md#cnt) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [COL](workspaces.md#col) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](workspaces.md#cyan)

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Configuration variable [CYL](workspaces.md#cyl)

Ship type for a Cobra Mk III

[X]

Configuration variable [CYL2](workspaces.md#cyl2)

Ship type for a Cobra Mk III (pirate)

[X]

Label [DA1](elite_b.md#da1) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DA11](elite_b.md#da11) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DA2](elite_b.md#da2) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DA5](elite_b.md#da5) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DA6](elite_b.md#da6) in subroutine [TT26](elite_b.md#header-tt26)

[X]

[X]

Label [DA8](elite_b.md#da8) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DAL1](elite_b.md#dal1) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DAL2](elite_b.md#dal2) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DAL3](elite_b.md#dal3) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DAL4](elite_b.md#dal4) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DAL5](elite_b.md#dal5) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DAL6](elite_b.md#dal6) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Label [DAS1](elite_b.md#das1) in subroutine [TT26](elite_b.md#header-tt26)

[X]

Variable [DELT4](workspaces.md#delt4) in workspace [ZP](workspaces.md#header-zp)

Our current speed * 64 as a 16-bit value

[X]

Subroutine [DETOK](elite_a.md#header-detok) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [DORND](elite_f.md#header-dornd) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [DTW2](elite_b.md#header-dtw2) (category: Text)

A flag that indicates whether we are currently printing a word

[X]

Variable [DTW4](elite_b.md#header-dtw4) (category: Text)

Flags that govern how justified extended text tokens are printed

[X]

Variable [DTW5](elite_b.md#header-dtw5) (category: Text)

The size of the justified text buffer at BUF

[X]

Variable [DTW8](elite_b.md#header-dtw8) (category: Text)

A mask for capitalising the next letter in an extended text token

[X]

Configuration variable [DUST](workspaces.md#dust)

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[X]

Subroutine [DV42](elite_c.md#header-dv42) (category: Maths (Arithmetic))

Calculate (P R) = 256 * DELTA / z_hi

[X]

Variable [ECM](workspaces.md#ecm) in workspace [WP](workspaces.md#header-wp)

E.C.M. system

[X]

Subroutine [EDGES](elite_e.md#header-edges) (category: Drawing lines)

Draw a horizontal line given a centre and a half-width

[X]

Variable [ENERGY](workspaces.md#energy) in workspace [ZP](workspaces.md#header-zp)

Energy bank status

[X]

Label [ES1](elite_b.md#es1) in subroutine [ESCAPE](elite_b.md#header-escape)

[X]

Variable [ESCP](workspaces.md#escp) in workspace [WP](workspaces.md#header-wp)

Escape pod

[X]

Label [ESL1](elite_b.md#esl1) in subroutine [ESCAPE](elite_b.md#header-escape)

[X]

Label [ESL2](elite_b.md#esl2) in subroutine [ESCAPE](elite_b.md#header-escape)

[X]

Variable [FIST](workspaces.md#fist) in workspace [WP](workspaces.md#header-wp)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Variable [FLAG](workspaces.md#flag) in workspace [ZP](workspaces.md#header-zp)

A flag that's used to define whether this is the first call to the ball line routine in BLINE, so it knows whether to wait for the second call before storing segment data in the ball line heap

[X]

Label [FLL1](elite_b.md#fll1) in subroutine [FLIP](elite_b.md#header-flip)

[X]

Variable [FRIN](workspaces.md#frin) in workspace [WP](workspaces.md#header-wp)

Slots for the ships in the local bubble of universe

[X]

Subroutine [FRS1](elite_c.md#header-frs1) (category: Tactics)

Launch a ship straight ahead of us, below the laser sights

[X]

Entry point [GOIN](elite_a.md#goin) in subroutine [Main flight loop (Part 9 of 16)](elite_a.md#header-main-flight-loop-part-9-of-16) (category: Main loop)

We jump here from part 3 of the main flight loop if the docking computer is activated by pressing "C"

[X]

Subroutine [HLOIN](elite_a.md#header-hloin) (category: Drawing lines)

Draw a horizontal line from (X1, Y1) to (X2, Y1)

[X]

Entry point [HLOIN3](elite_a.md#hloin3) in subroutine [HLOIN](elite_a.md#header-hloin) (category: Drawing lines)

Draw a line from (X, Y1) to (X2, Y1) in the colour given in A

[X]

Label [HME3](elite_b.md#hme3) in subroutine [HME2](elite_b.md#header-hme2)

[X]

Label [HME4](elite_b.md#hme4) in subroutine [HME2](elite_b.md#header-hme2)

[X]

Label [HME5](elite_b.md#hme5) in subroutine [HME2](elite_b.md#header-hme2)

[X]

Label [HME6](elite_b.md#hme6) in subroutine [HME2](elite_b.md#header-hme2)

[X]

Variable [INWK](workspaces.md#inwk) in workspace [ZP](workspaces.md#header-zp)

The zero-page internal workspace for the current ship data block

[X]

Variable [JUNK](workspaces.md#junk) in workspace [WP](workspaces.md#header-wp)

The amount of junk in the local bubble

[X]

Variable [K](workspaces.md#k) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Workspace [K%](workspaces.md#header-k-per-cent) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Variable [K4](workspaces.md#k4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K5](workspaces.md#k5) in workspace [ZP](workspaces.md#header-zp)

Temporary storage used to store segment coordinates across successive calls to BLINE, the ball line routine

[X]

Variable [K6](workspaces.md#k6) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing coordinates during vector calculations

[X]

Label [KILL1](elite_b.md#kill1) in subroutine [STARS1](elite_b.md#header-stars1)

[X]

Label [KILL6](elite_b.md#kill6) in subroutine [STARS6](elite_b.md#header-stars6)

[X]

Variable [LASER](workspaces.md#laser) in workspace [WP](workspaces.md#header-wp)

The specifications of the lasers fitted to each of the four space views

[X]

Configuration variable [LL](workspaces.md#ll)

The length of lines (in characters) of justified text in the extended tokens system

[X]

Subroutine [LL145 (Part 1 of 4)](elite_g.md#header-ll145-part-1-of-4) (category: Drawing lines)

Clip line: Work out which end-points are on-screen, if any

[X]

Subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Subroutine [LOIN](elite_a.md#header-loin) (category: Drawing lines)

Draw a one-segment line

[X]

Variable [LSO](workspaces.md#lso) in workspace [WP](workspaces.md#header-wp)

The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)

[X]

Variable [LSP](workspaces.md#lsp) in workspace [ZP](workspaces.md#header-zp)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Variable [LSX2](workspaces.md#lsx2) in workspace [WP](workspaces.md#header-wp)

The ball line heap for storing x-coordinates

[X]

Variable [LSY2](workspaces.md#lsy2) in workspace [WP](workspaces.md#header-wp)

The ball line heap for storing y-coordinates

[X]

Label [MA30](elite_b.md#ma30) in subroutine [MAS3](elite_b.md#header-mas3)

[X]

Subroutine [MLS1](elite_c.md#header-mls1) (category: Maths (Arithmetic))

Calculate (A P) = ALP1 * A

[X]

Subroutine [MLS2](elite_c.md#header-mls2) (category: Maths (Arithmetic))

Calculate (S R) = XX(1 0) and (A P) = A * ALP1

[X]

Subroutine [MLU1](elite_c.md#header-mlu1) (category: Maths (Arithmetic))

Calculate Y1 = y_hi and (A P) = |y_hi| * Q for Y-th stardust

[X]

Subroutine [MLU2](elite_c.md#header-mlu2) (category: Maths (Arithmetic))

Calculate (A P) = |A| * Q

[X]

Subroutine [MT14](elite_a.md#header-mt14) (category: Text)

Switch to justified text when printing extended tokens

[X]

Subroutine [MT15](elite_a.md#header-mt15) (category: Text)

Switch to left-aligned text when printing extended tokens

[X]

Entry point [MULTS-2](elite_c.md#mults) in subroutine [MLS1](elite_c.md#header-mls1) (category: Maths (Arithmetic))

Calculate (A P) = X * A

[X]

Subroutine [MUT1](elite_c.md#header-mut1) (category: Maths (Arithmetic))

Calculate R = XX and (A P) = Q * A

[X]

Subroutine [MUT2](elite_c.md#header-mut2) (category: Maths (Arithmetic))

Calculate (S R) = XX(1 0) and (A P) = Q * A

[X]

Label [MV13](elite_b.md#mv13) in subroutine [MVT3](elite_b.md#header-mvt3)

[X]

Label [MV14](elite_b.md#mv14) in subroutine [MVT3](elite_b.md#header-mvt3)

[X]

Subroutine [MVEIT (Part 1 of 9)](elite_h.md#header-mveit-part-1-of-9) (category: Moving)

Move current ship: Tidy the orientation vectors

[X]

Subroutine [MVT3](elite_b.md#header-mvt3) (category: Moving)

Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)

[X]

Configuration variable [Mlas](workspaces.md#mlas)

Mining laser power

[X]

Configuration variable [NI%](workspaces.md#ni-per-cent)

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Subroutine [NLIN2](elite_b.md#header-nlin2) (category: Drawing lines)

Draw a screen-wide horizontal line at the pixel row in A

[X]

Subroutine [NLIN3](elite_b.md#header-nlin3) (category: Drawing lines)

Print a title and draw a horizontal line at row 19 to box it in

[X]

Variable [NOSTM](workspaces.md#nostm) in workspace [ZP](workspaces.md#header-zp)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Variable [P](workspaces.md#p) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [PIX1](elite_a.md#header-pix1) (category: Maths (Arithmetic))

Calculate (YY+1 SYL+Y) = (A P) + (S R) and draw stardust particle

[X]

Subroutine [PIXEL2](elite_a.md#header-pixel2) (category: Drawing pixels)

Draw a stardust particle relative to the screen centre

[X]

Configuration variable [POW](workspaces.md#pow)

Pulse laser power

[X]

Variable [Q](workspaces.md#q) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [QQ10](workspaces.md#qq10) in workspace [ZP](workspaces.md#header-zp)

The galactic y-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic y-coordinate)

[X]

Variable [QQ11](workspaces.md#qq11) in workspace [ZP](workspaces.md#header-zp)

The type of the current view

[X]

Variable [QQ12](workspaces.md#qq12) in workspace [ZP](workspaces.md#header-zp)

Our "docked" status

[X]

Variable [QQ14](workspaces.md#qq14) in workspace [WP](workspaces.md#header-wp)

Our current fuel level (0-70)

[X]

Variable [QQ15](workspaces.md#qq15) in workspace [ZP](workspaces.md#header-zp)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ20](workspaces.md#qq20) in workspace [WP](workspaces.md#header-wp)

The contents of our cargo hold

[X]

Variable [QQ9](workspaces.md#qq9) in workspace [ZP](workspaces.md#header-zp)

The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)

[X]

Variable [R](workspaces.md#r) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [RAT2](workspaces.md#rat2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the pitch and roll signs when moving objects and stardust

[X]

Subroutine [RES2](elite_f.md#header-res2) (category: Start and end)

Reset a number of flight variables and workspaces

[X]

Variable [S](workspaces.md#s) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [SC](workspaces.md#sc) in workspace [ZP](workspaces.md#header-zp)

Screen address (low byte)

[X]

Subroutine [SCAN](elite_a.md#header-scan) (category: Dashboard)

Display the current ship on the scanner

[X]

Subroutine [SQUA2](elite_c.md#header-squa2) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[X]

Label [ST11](elite_b.md#st11) in subroutine [STARS](elite_b.md#header-stars)

[X]

Label [ST3](elite_b.md#st3) in subroutine [STARS6](elite_b.md#header-stars6)

[X]

Label [ST4](elite_b.md#st4) in subroutine [STARS6](elite_b.md#header-stars6)

[X]

Subroutine [STARS1](elite_b.md#header-stars1) (category: Stardust)

Process the stardust for the front view

[X]

Subroutine [STARS2](elite_c.md#header-stars2) (category: Stardust)

Process the stardust for the left or right view

[X]

Subroutine [STARS6](elite_b.md#header-stars6) (category: Stardust)

Process the stardust for the rear view

[X]

Label [STC1](elite_b.md#stc1) in subroutine [STARS1](elite_b.md#header-stars1)

[X]

Label [STC6](elite_b.md#stc6) in subroutine [STARS6](elite_b.md#header-stars6)

[X]

Label [STL1](elite_b.md#stl1) in subroutine [STARS1](elite_b.md#header-stars1)

[X]

Label [STL6](elite_b.md#stl6) in subroutine [STARS6](elite_b.md#header-stars6)

[X]

Variable [STP](workspaces.md#stp) in workspace [ZP](workspaces.md#header-zp)

The step size for drawing circles

[X]

Variable [SWAP](workspaces.md#swap) in workspace [WP](workspaces.md#header-wp)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[X]

Variable [SX](workspaces.md#sx) in workspace [WP](workspaces.md#header-wp)

This is where we store the x_hi coordinates for all the stardust particles

[X]

Variable [SXL](workspaces.md#sxl) in workspace [WP](workspaces.md#header-wp)

This is where we store the x_lo coordinates for all the stardust particles

[X]

Variable [SY](workspaces.md#sy) in workspace [WP](workspaces.md#header-wp)

This is where we store the y_hi coordinates for all the stardust particles

[X]

Variable [SYL](workspaces.md#syl) in workspace [WP](workspaces.md#header-wp)

This is where we store the y_lo coordinates for all the stardust particles

[X]

Variable [SZ](workspaces.md#sz) in workspace [WP](workspaces.md#header-wp)

This is where we store the z_hi coordinates for all the stardust particles

[X]

Variable [SZL](workspaces.md#szl) in workspace [WP](workspaces.md#header-wp)

This is where we store the z_lo coordinates for all the stardust particles

[X]

Variable [T](workspaces.md#t) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Entry point [T95](elite_f.md#t95) in subroutine [TT102](elite_f.md#header-tt102) (category: Keyboard)

Print the distance to the selected system

[X]

Variable [TALLY](workspaces.md#tally) in workspace [WP](workspaces.md#header-wp)

Our combat rank

[X]

Variable [TENS](elite_b.md#header-tens) (category: Text)

A constant used when printing large numbers in BPRNT

[X]

Subroutine [TRADEMODE](elite_d.md#header-trademode) (category: Drawing the screen)

Clear the screen and set up a trading screen

[X]

Subroutine [TT103](elite_d.md#header-tt103) (category: Charts)

Draw a small set of crosshairs on a chart

[X]

Subroutine [TT111](elite_d.md#header-tt111) (category: Universe)

Set the current system to the nearest system to a point

[X]

Subroutine [TT20](elite_d.md#header-tt20) (category: Universe)

Twist the selected system's seeds four times

[X]

Subroutine [TT26](elite_b.md#header-tt26) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [TT27](elite_e.md#header-tt27) (category: Text)

Print a text token

[X]

Label [TT30](elite_b.md#tt30) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [TT32](elite_b.md#tt32) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [TT34](elite_b.md#tt34) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [TT35](elite_b.md#tt35) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [TT36](elite_b.md#tt36) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [TT37](elite_b.md#tt37) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Subroutine [TT67K](elite_a.md#header-tt67k) (category: Text)

Print a newline

[X]

Subroutine [TT81](elite_d.md#header-tt81) (category: Universe)

Set the selected system's seeds to those of system 0

[X]

Variable [TYPE](workspaces.md#type) in workspace [ZP](workspaces.md#header-zp)

The current ship type

[X]

Variable [U](workspaces.md#u) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [VIEW](workspaces.md#view) in workspace [WP](workspaces.md#header-wp)

The number of the current space view

[X]

Variable [X1](workspaces.md#x1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](workspaces.md#x2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XC](workspaces.md#xc) in workspace [ZP](workspaces.md#header-zp)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [XX](workspaces.md#xx) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing a 16-bit x-coordinate

[X]

Variable [XX12](workspaces.md#xx12) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX13](workspaces.md#xx13) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used in the line-drawing routines

[X]

Variable [XX15](workspaces.md#xx15) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Variable [XX17](workspaces.md#xx17) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in BPRNT to store the number of characters to print, and as the edge counter in the main ship-drawing routine

[X]

Variable [XX20](workspaces.md#xx20) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [XX4](workspaces.md#xx4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [Y1](workspaces.md#y1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](workspaces.md#y2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [YC](workspaces.md#yc) in workspace [ZP](workspaces.md#header-zp)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Configuration variable [YELLOW](workspaces.md#yellow)

Four mode 1 pixels of colour 1 (yellow)

[X]

Variable [YY](workspaces.md#yy) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing a 16-bit y-coordinate

[X]

Variable [ZZ](workspaces.md#zz) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for distance values

[X]

Subroutine [cpl](elite_e.md#header-cpl) (category: Universe)

Print the selected system name

[X]

Subroutine [plf](elite_e.md#header-plf) (category: Text)

Print a text token followed by a newline

[X]

Subroutine [plf2](elite_b.md#header-plf2) (category: Text)

Print text followed by a newline and indent of 6 characters

[X]

Label [rT10](elite_b.md#rt10) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Subroutine [spc](elite_d.md#header-spc) (category: Text)

Print a text token followed by a space

[X]

Label [st](elite_b.md#st) in subroutine [STATUS](elite_b.md#header-status)

[X]

Label [st1](elite_b.md#st1) in subroutine [STATUS](elite_b.md#header-status)

[X]

Label [st3](elite_b.md#st3) in subroutine [STATUS](elite_b.md#header-status)

[X]

Label [st4](elite_b.md#st4) in subroutine [STATUS](elite_b.md#header-status)

[X]

Label [st5](elite_b.md#st5) in subroutine [STATUS](elite_b.md#header-status)

[X]

Label [st6](elite_b.md#st6) in subroutine [STATUS](elite_b.md#header-status)

[X]

[X]

Label [stqv](elite_b.md#stqv) in subroutine [STATUS](elite_b.md#header-status)

[X]

Label [tt34](elite_b.md#tt34) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [tt35](elite_b.md#tt35) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [tt36](elite_b.md#tt36) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [tt37](elite_b.md#tt37) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [tt38](elite_b.md#tt38) in subroutine [BPRNT](elite_b.md#header-bprnt)

[X]

Label [wearedocked](elite_b.md#wearedocked) in subroutine [STATUS](elite_b.md#header-status)

[Elite A source](elite_a.md "Previous routine")[Elite C source](elite_c.md "Next routine")
