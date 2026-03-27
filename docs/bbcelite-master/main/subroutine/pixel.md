---
title: "The PIXEL subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pixel.html"
---

[PIXEL2](pixel2.md "Previous routine")[PXCL](../variable/pxcl.md "Next routine")
    
    
           Name: PIXEL                                                   [Show more]
           Type: Subroutine
       Category: Drawing pixels
        Summary: Draw a one-pixel dot, two-pixel dash or four-pixel square
      Deep dive: [Drawing colour pixels on the BBC Micro](https://elite.bbcelite.com/deep_dives/drawing_colour_pixels_in_mode_5.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-pixel)
     Variations: See [code variations](../../related/compare/main/subroutine/pixel.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOEXP](doexp.md) calls PIXEL
                 * [TT22](tt22.md) calls PIXEL
                 * [PIXEL2](pixel2.md) calls via PXR1
    
    
    
    
    
    * * *
    
    
     Draw a point at screen coordinate (X, A) with the point size determined by the
     distance in ZZ. This applies to the top part of the screen (the four-colour
     mode 5 portion).
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The screen x-coordinate of the point to draw
    
       A                    The screen y-coordinate of the point to draw
    
       ZZ                   The distance of the point, with bigger distances drawing
                            smaller points:
    
                              * ZZ < 80           Double-height four-pixel square
    
                              * 80 <= ZZ <= 143   Single-height two-pixel dash
    
                              * ZZ > 143          Single-height one-pixel dot
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       PXR1                 Contains an RTS
    
    
    
    
    .PIXEL
    
     STY T1                 \ Store Y in T1 so we can restore it at the end of the
                            \ subroutine
    
     LDY #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     TAY                    \ Copy the screen y-coordinate from A into Y
    
     LDA ylookup,Y          \ Look up the page number of the character row that
     STA SC+1               \ contains the pixel with the y-coordinate in Y, and
                            \ store it in the high byte of SC(1 0) at SC+1
    
     TXA                    \ Each character block contains 8 pixel rows, so to get
     AND #%11111100         \ the address of the first byte in the character block
     ASL A                  \ that we need to draw into, as an offset from the start
                            \ of the row, we clear bits 0-1 and shift left to double
                            \ it (as each character row contains two pages of bytes,
                            \ or 512 bytes, which cover 256 pixels). This also
                            \ shifts bit 7 of the x-coordinate into the C flag
    
     STA SC                 \ Store the address of the character block in the low
                            \ byte of SC(1 0), so now SC(1 0) points to the
                            \ character block we need to draw into
    
     BCC P%+4               \ If the C flag is clear then skip the next instruction
    
     INC SC+1               \ The C flag is set, which means bit 7 of X1 was set
                            \ before the ASL above, so the x-coordinate is in the
                            \ right half of the screen (i.e. in the range 128-255).
                            \ Each row takes up two pages in memory, so the right
                            \ half is in the second page but SC+1 contains the value
                            \ we looked up from ylookup, which is the page number of
                            \ the first memory page for the row... so we need to
                            \ increment SC+1 to point to the correct page
    
     TYA                    \ Set Y = Y mod 8, which is the pixel row within the
     AND #7                 \ character block at which we want to draw the pixel
     TAY                    \ (as each character block has 8 rows)
    
     TXA                    \ Copy bits 0-1 of the x-coordinate to bits 0-1 of X,
     AND #%00000011         \ which will now be in the range 0-3, and will contain
     TAX                    \ the two pixels to show in the character row
    
     LDA ZZ                 \ Set A to the pixel's distance in ZZ
    
     CMP #80                \ If the pixel's ZZ distance is < 80, then the dot is
     BCC PX2                \ pretty close, so jump to PX2 to draw a four-pixel
                            \ square
    
     LDA TWOS2,X            \ Fetch a mode 1 two-pixel byte with the pixels set as
     AND COL                \ in X, and AND with the colour byte we fetched into COL
                            \ so that pixel takes on the colour we want to draw
                            \ (i.e. A is acting as a mask on the colour byte)
    
     EOR (SC),Y             \ Draw the pixel on-screen using EOR logic, so we can
     STA (SC),Y             \ remove it later without ruining the background that's
                            \ already on-screen
    
     LDY #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY T1                 \ Restore Y from T1, so Y is preserved by the routine
    
    .PXR1
    
     RTS                    \ Return from the subroutine
    
    .PX2
    
                            \ If we get here, we need to plot a four-pixel square in
                            \ in the correct colour for this pixel's distance
    
     LDA TWOS2,X            \ Fetch a mode 1 two-pixel byte with the pixels set as
     AND COL                \ in X, and AND with the colour byte we fetched into COL
                            \ so that pixel takes on the colour we want to draw
                            \ (i.e. A is acting as a mask on the colour byte)
    
     EOR (SC),Y             \ Draw the pixel on-screen using EOR logic, so we can
     STA (SC),Y             \ remove it later without ruining the background that's
                            \ already on-screen
    
     DEY                    \ Reduce Y by 1 to point to the pixel row above the one
     BPL P%+4               \ we just plotted, and if it is still positive, skip the
                            \ next instruction
    
     LDY #1                 \ Reducing Y by 1 made it negative, which means Y was
                            \ 0 before we did the DEY above, so set Y to 1 to point
                            \ to the pixel row after the one we just plotted
    
                            \ We now draw our second dash
    
     LDA TWOS2,X            \ Fetch a mode 1 two-pixel byte with the pixels set as
     AND COL                \ in X, and AND with the colour byte we fetched into COL
                            \ so that pixel takes on the colour we want to draw
                            \ (i.e. A is acting as a mask on the colour byte)
    
     EOR (SC),Y             \ Draw the pixel on-screen using EOR logic, so we can
     STA (SC),Y             \ remove it later without ruining the background that's
                            \ already on-screen
    
     LDY #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STY VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY T1                 \ Restore Y from T1, so Y is preserved by the routine
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Label [PX2](pixel.md#px2) is local to this routine

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [TWOS2](../variable/twos2.md) (category: Drawing pixels)

Ready-made double-pixel character row bytes for mode 1

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [ZZ](../workspace/zp.md#zz) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for distance values

[X]

Variable [ylookup](../variable/ylookup.md) (category: Drawing pixels)

Lookup table for converting pixel y-coordinate to page number of screen address

[PIXEL2](pixel2.md "Previous routine")[PXCL](../variable/pxcl.md "Next routine")
