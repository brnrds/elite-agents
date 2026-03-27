---
title: "The BOMBOFF subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bomboff.html"
---

[SPIN](spin.md "Previous routine")[BOMBEFF2](bombeff2.md "Next routine")
    
    
           Name: BOMBOFF                                                 [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw the zig-zag lightning bolt for the energy bomb
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-bomboff)
     References: This subroutine is called as follows:
                 * [BOMBEFF2](bombeff2.md) calls BOMBOFF
                 * [BOMBON](bombon.md) calls BOMBOFF
                 * [LOOK1](look1.md) calls BOMBOFF
                 * [Main flight loop (Part 13 of 16)](main_flight_loop_part_13_of_16.md) calls BOMBOFF
    
    
    
    
    
    
    .BOMBOFF
    
     LDA #CYAN              \ Change the current colour to cyan
     STA COL
    
     LDA QQ11               \ If the current view is non-zero (i.e. not a space
     BNE BOMBR1             \ view), return from the subroutine (as BOMBR1 contains
                            \ an RTS)
    
     LDY #1                 \ We now want to loop through the 10 (BOMBTBX, BOMBTBY)
                            \ coordinates, drawing a total of 9 lines between them
                            \ to make the lightning effect, so set an index in Y
                            \ to point to the end-point for each line, starting with
                            \ the second coordinate pair
    
     LDA BOMBTBX            \ Store the first coordinate pair from (BOMBTBX,
     STA XX12               \ BOMBTBY) in (XX12, XX12+1)
     LDA BOMBTBY
     STA XX12+1
    
    .BOMBL1
    
    \JSR CLICK              \ This instruction is commented out in the original
                            \ source
    
     LDA XX12               \ Set (X1, Y1) = (XX12, XX12+1)
     STA X1                 \
     LDA XX12+1             \ so the start point for this line
     STA Y1
    
     LDA BOMBTBX,Y          \ Set X2 = Y-th x-coordinate from BOMBTBX
     STA X2
    
     STA XX12               \ Set XX12 = X2
    
     LDA BOMBTBY,Y          \ Set Y2 = Y-th y-coordinate from BOMBTBY, so we now
     STA Y2                 \ have:
                            \
                            \   (X2, Y2) = Y-th coordinate from (BOMBTBX, BOMBTBY)
    
     STA XX12+1             \ Set XX12+1 = Y2, so we now have
                            \
                            \   (XX12, XX12+1) = (X2, Y2)
                            \
                            \ so in the next loop iteration, the start point of the
                            \ line will be the end point of this line, making the
                            \ zig-zag lines all join up like a lightning bolt
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2)
    
     INY                    \ Increment the loop counter
    
     CPY #10                \ If Y < 10, loop back until we have drawn all the lines
     BCC BOMBL1
    
    .BOMBR1
    
     RTS                    \ Return from the subroutine
    

[X]

Label [BOMBL1](bomboff.md#bombl1) is local to this routine

[X]

Label [BOMBR1](bomboff.md#bombr1) is local to this routine

[X]

Variable [BOMBTBX](../variable/bombtbx.md) (category: Drawing lines)

This is where we store the x-coordinates for the energy bomb's zig-zag lightning bolt

[X]

Variable [BOMBTBY](../variable/bombtby.md) (category: Drawing lines)

This is where we store the y-coordinates for the energy bomb's zig-zag lightning bolt

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Subroutine [LOIN](loin.md) (category: Drawing lines)

Draw a one-segment line

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

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

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[SPIN](spin.md "Previous routine")[BOMBEFF2](bombeff2.md "Next routine")
