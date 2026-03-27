---
title: "The LASLI subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/lasli.html"
---

[ARCTAN](arctan.md "Previous routine")[PDESC](pdesc.md "Next routine")
    
    
           Name: LASLI                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw the laser lines for when we fire our lasers
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-lasli)
     Variations: See [code variations](../../related/compare/main/subroutine/lasli.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls LASLI
                 * [Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md) calls via LASLI2
    
    
    
    
    
    * * *
    
    
     Draw the laser lines, aiming them to slightly different place each time so
     they appear to flicker and dance. Also heat up the laser temperature and drain
     some energy.
    
    
    
    * * *
    
    
     Other entry points:
    
       LASLI2               Just draw the current laser lines without moving the
                            centre point, draining energy or heating up. This has
                            the effect of removing the lines from the screen
    
       LASLI-1              Contains an RTS
    
    
    
    
    .LASLI
    
     JSR DORND              \ Set A and X to random numbers
    
     AND #7                 \ Restrict A to a random value in the range 0 to 7
    
     ADC #Y-4               \ Set LASY to four pixels above the centre of the
     STA LASY               \ screen (#Y), plus our random number, so the laser
                            \ dances above and below the centre point
    
     JSR DORND              \ Set A and X to random numbers
    
     AND #7                 \ Restrict A to a random value in the range 0 to 7
    
     ADC #X-4               \ Set LASX to four pixels left of the centre of the
     STA LASX               \ screen (#X), plus our random number, so the laser
                            \ dances to the left and right of the centre point
    
     LDA GNTMP              \ Add 8 to the laser temperature in GNTMP
     ADC #8
     STA GNTMP
    
     JSR DENGY              \ Call DENGY to deplete our energy banks by 1
    
    .LASLI2
    
     LDA QQ11               \ If this is not a space view (i.e. QQ11 is non-zero)
     BNE ARSR1              \ then jump to MA9 to return from the main flight loop
                            \ (as ARSR1 is an RTS)
    
     LDA #RED               \ Switch to colour 2, which is red in the space view
     STA COL
    
     LDA #32                \ Set A = 32 and Y = 224 for the first set of laser
     LDY #224               \ lines (the wider pair of lines)
    
     DEC LASY               \ Decrement the y-coordinate of the centre point to move
     DEC LASY               \ it up the screen by two pixels for the top set of
                            \ lines, so the wider set of lines aim slightly higher
                            \ than the narrower set
    
     JSR las                \ Call las below to draw the first set of laser lines
    
     INC LASY               \ Increment the y-coordinate of the centre point to put
     INC LASY               \ it back to the original position
    
     LDA #48                \ Fall through into las with A = 48 and Y = 208 to draw
     LDY #208               \ a second set of lines (the narrower pair)
    
                            \ The following routine draws two laser lines, one from
                            \ the centre point down to point A on the bottom row,
                            \ and the other from the centre point down to point Y
                            \ on the bottom row. We therefore get lines from the
                            \ centre point to points 32, 48, 208 and 224 along the
                            \ bottom row, giving us the triangular laser effect
                            \ we're after
    
    .las
    
     STA X2                 \ Set X2 = A
    
     LDA LASX               \ Set (X1, Y1) to the random centre point we set above
     STA X1
     LDA LASY
     STA Y1
    
     LDA #2*Y-1             \ Set Y2 = 2 * #Y - 1. The constant #Y is 96, the
     STA Y2                 \ y-coordinate of the mid-point of the space view, so
                            \ this sets Y2 to 191, the y-coordinate of the bottom
                            \ pixel row of the space view
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2), so that's from
                            \ the centre point to (A, 191)
    
     LDA LASX               \ Set (X1, Y1) to the random centre point we set above
     STA X1
     LDA LASY
     STA Y1
    
     STY X2                 \ Set X2 = Y
    
     LDA #2*Y-1             \ Set Y2 = 2 * #Y - 1, the y-coordinate of the bottom
     STA Y2                 \ pixel row of the space view (as before)
    
     JMP LOIN               \ Draw a line from (X1, Y1) to (X2, Y2), so that's from
                            \ the centre point to (Y, 191), and return from
                            \ the subroutine using a tail call
    

[X]

Entry point [ARSR1](arctan.md#arsr1) in subroutine [ARCTAN](arctan.md) (category: Maths (Geometry))

Contains an RTS

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [DENGY](dengy.md) (category: Flight)

Drain some energy from the energy banks

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [GNTMP](../workspace/wp.md#gntmp) in workspace [WP](../workspace/wp.md)

Laser temperature (or "gun temperature")

[X]

Variable [LASX](../workspace/wp.md#lasx) in workspace [WP](../workspace/wp.md)

The x-coordinate of the tip of the laser line

[X]

Variable [LASY](../workspace/wp.md#lasy) in workspace [WP](../workspace/wp.md)

The y-coordinate of the tip of the laser line

[X]

Subroutine [LOIN](loin.md) (category: Drawing lines)

Draw a one-segment line

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Configuration variable [RED](../../all/workspaces.md#red) = %11110000

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Configuration variable [X](../../all/workspaces.md#x) = 128

The centre x-coordinate of the 256 x 192 space view

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](../workspace/zp.md#x2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Label [las](lasli.md#las) is local to this routine

[ARCTAN](arctan.md "Previous routine")[PDESC](pdesc.md "Next routine")
