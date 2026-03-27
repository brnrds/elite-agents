---
title: "The FLIP subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/flip.html"
---

[BLINE](bline.md "Previous routine")[STARS](stars.md "Next routine")
    
    
           Name: FLIP                                                    [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Reflect the stardust particles in the screen diagonal and redraw
                 the stardust field
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-flip)
     Variations: See [code variations](../../related/compare/main/subroutine/flip.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOOK1](look1.md) calls FLIP
    
    
    
    
    
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
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [DUST](../../all/workspaces.md#dust) = WHITE

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[X]

Label [FLL1](flip.md#fll1) is local to this routine

[X]

Variable [NOSTM](../workspace/zp.md#nostm) in workspace [ZP](../workspace/zp.md)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Subroutine [PIXEL2](pixel2.md) (category: Drawing pixels)

Draw a stardust particle relative to the screen centre

[X]

Variable [SX](../workspace/wp.md#sx) in workspace [WP](../workspace/wp.md)

This is where we store the x_hi coordinates for all the stardust particles

[X]

Variable [SY](../workspace/wp.md#sy) in workspace [WP](../workspace/wp.md)

This is where we store the y_hi coordinates for all the stardust particles

[X]

Variable [SZ](../workspace/wp.md#sz) in workspace [WP](../workspace/wp.md)

This is where we store the z_hi coordinates for all the stardust particles

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [ZZ](../workspace/zp.md#zz) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for distance values

[BLINE](bline.md "Previous routine")[STARS](stars.md "Next routine")
