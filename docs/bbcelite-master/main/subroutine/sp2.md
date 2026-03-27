---
title: "The SP2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sp2.html"
---

[SP1](sp1.md "Previous routine")[OOPS](oops.md "Next routine")
    
    
           Name: SP2                                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Draw a dot on the compass, given the planet/station vector
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-sp2)
     Variations: See [code variations](../../related/compare/main/subroutine/sp2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [COMPAS](compas.md) calls SP2
    
    
    
    
    
    * * *
    
    
     Draw a dot on the compass to represent the planet or station, whose normalised
     vector is in XX15.
    
       XX15 to XX15+2      The normalised vector to the planet or space station,
                           stored as x in XX15, y in XX15+1 and z in XX15+2
    
    
    
    
    .SP2
    
     LDA XX15               \ Set A to the x-coordinate of the planet or station to
                            \ show on the compass, which will be in the range -96 to
                            \ +96 as the vector has been normalised
    
     JSR SPS2               \ Set (Y X) = A / 10, so X will be from -9 to +9, which
                            \ is the x-offset from the centre of the compass of the
                            \ dot we want to draw. Returns with the C flag clear
    
     TXA                    \ Set COMX = 195 + X, as 186 is the pixel x-coordinate
     ADC #195               \ of the leftmost dot possible on the compass, and X can
     STA COMX               \ be -9, which would be 195 - 9 = 186. This also means
                            \ that the highest value for COMX is 195 + 9 = 204,
                            \ which is the pixel x-coordinate of the rightmost dot
                            \ in the compass... but the compass dot is actually two
                            \ pixels wide, so the compass dot can overlap the right
                            \ edge of the compass, but not the left edge
    
     LDA XX15+1             \ Set A to the y-coordinate of the planet or station to
                            \ show on the compass, which will be in the range -96 to
                            \ +96 as the vector has been normalised
    
     JSR SPS2               \ Set (Y X) = A / 10, so X will be from -9 to +9, which
                            \ is the x-offset from the centre of the compass of the
                            \ dot we want to draw. Returns with the C flag clear
    
     STX T                  \ Set COMY = 204 - X, as 203 is the pixel y-coordinate
     LDA #204               \ of the centre of the compass, the C flag is clear,
     SBC T                  \ and the y-axis needs to be flipped around (because
     STA COMY               \ when the planet or station is above us, and the
                            \ vector is therefore positive, we want to show the dot
                            \ higher up on the compass, which has a smaller pixel
                            \ y-coordinate). So this calculation does this:
                            \
                            \   COMY = 204 - X - (1 - 0) = 203 - X
    
     LDA #YELLOW2           \ Set A to yellow, the colour for when the planet or
                            \ station in the compass is in front of us
    
     LDX XX15+2             \ If the z-coordinate of the XX15 vector is positive,
     BPL P%+4               \ skip the following instruction
    
     LDA #GREEN2            \ The z-coordinate of XX15 is negative, so the planet or
                            \ station is behind us and the compass dot should be in
                            \ green, so set A accordingly
    
     STA COMC               \ Store the compass colour in COMC
    
     JMP DOT                \ Jump to DOT to draw the dot on the compass and return
                            \ from the subroutine using a tail call
    

[X]

Variable [COMC](../workspace/up.md#comc) in workspace [UP](../workspace/up.md)

The colour of the dot on the compass

[X]

Variable [COMX](../workspace/wp.md#comx) in workspace [WP](../workspace/wp.md)

The x-coordinate of the compass dot

[X]

Variable [COMY](../workspace/wp.md#comy) in workspace [WP](../workspace/wp.md)

The y-coordinate of the compass dot

[X]

Subroutine [DOT](dot.md) (category: Dashboard)

Draw a dash on the compass

[X]

Configuration variable [GREEN2](../../all/workspaces.md#green2) = %00001100

Two mode 2 pixels of colour 2 (green)

[X]

Subroutine [SPS2](sps2.md) (category: Maths (Arithmetic))

Calculate (Y X) = A / 10

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Configuration variable [YELLOW2](../../all/workspaces.md#yellow2) = %00001111

Two mode 2 pixels of colour 3 (yellow)

[SP1](sp1.md "Previous routine")[OOPS](oops.md "Next routine")
