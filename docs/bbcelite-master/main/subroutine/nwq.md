---
title: "The nWq subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nwq.html"
---

[NWSTARS](nwstars.md "Previous routine")[WPSHPS](wpshps.md "Next routine")
    
    
           Name: nWq                                                     [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Create a random cloud of stardust
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-nwq)
     Variations: See [code variations](../../related/compare/main/subroutine/nwq.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](death.md) calls nWq
    
    
    
    
    
    * * *
    
    
     Create a random cloud of stardust containing the correct number of dust
     particles, i.e. NOSTM of them, which is 3 in witchspace and 18 (#NOST) in
     normal space. Also clears the scanner and initialises the LSO block.
    
     This is called by the DEATH routine when it displays our untimely demise.
    
    
    
    
    .nWq
    
     LDA #DUST              \ Switch to stripe 3-2-3-2, which is cyan/red in the
     STA COL                \ space view
    
     LDY NOSTM              \ Set Y to the current number of stardust particles, so
                            \ we can use it as a counter through all the stardust
    
    .SAL4
    
     JSR DORND              \ Set A and X to random numbers
    
     ORA #8                 \ Set A so that it's at least 8
    
     STA SZ,Y               \ Store A in the Y-th particle's z_hi coordinate at
                            \ SZ+Y, so the particle appears in front of us
    
     STA ZZ                 \ Set ZZ to the particle's z_hi coordinate
    
     JSR DORND              \ Set A and X to random numbers
    
     STA SX,Y               \ Store A in the Y-th particle's x_hi coordinate at
                            \ SX+Y, so the particle appears in front of us
    
     STA X1                 \ Set X1 to the particle's x_hi coordinate
    
     JSR DORND              \ Set A and X to random numbers
    
     STA SY,Y               \ Store A in the Y-th particle's y_hi coordinate at
                            \ SY+Y, so the particle appears in front of us
    
     STA Y1                 \ Set Y1 to the particle's y_hi coordinate
    
     JSR PIXEL2             \ Draw a stardust particle at (X1,Y1) with distance ZZ
    
     DEY                    \ Decrement the counter to point to the next particle of
                            \ stardust
    
     BNE SAL4               \ Loop back to SAL4 until we have randomised all the
                            \ stardust particles
    
    \JSR PBFL               \ This instruction is commented out in the original
                            \ source
    
                            \ Fall through into WPSHPS to clear the scanner and
                            \ reset the LSO block
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Configuration variable [DUST](../../all/workspaces.md#dust) = WHITE

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[X]

Variable [NOSTM](../workspace/zp.md#nostm) in workspace [ZP](../workspace/zp.md)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Subroutine [PIXEL2](pixel2.md) (category: Drawing pixels)

Draw a stardust particle relative to the screen centre

[X]

Label [SAL4](nwq.md#sal4) is local to this routine

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

[NWSTARS](nwstars.md "Previous routine")[WPSHPS](wpshps.md "Next routine")
