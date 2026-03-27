---
title: "The DOT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dot.html"
---

[PXCL](../variable/pxcl.md "Previous routine")[CPIXK](cpixk.md "Next routine")
    
    
           Name: DOT                                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Draw a dash on the compass
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-dot)
     Variations: See [code variations](../../related/compare/main/subroutine/dot.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [COMPAS](compas.md) calls DOT
                 * [SP2](sp2.md) calls DOT
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       COMX                 The screen pixel x-coordinate of the dash
    
       COMY                 The screen pixel y-coordinate of the dash
    
       COMC                 The colour and thickness of the dash:
    
                              * &F0 = a double-height dash in yellow/white, for when
                                the object in the compass is in front of us
    
                              * &FF = a single-height dash in green/cyan, for when
                                the object in the compass is behind us
    
    
    
    
    .DOT
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA COMX               \ Set X1 = COMX, the x-coordinate of the dash
     STA X1
    
     LDX COMC               \ Set COL = COMC, the mode 2 colour byte for the dash
     STX COL
    
     LDA COMY               \ Set Y1 = COMY, the y-coordinate of the dash
    
     CPX #YELLOW2           \ If the colour in X is yellow, then the planet/station
     BNE P%+8               \ is behind us, so skip the following three instructions
                            \ so we only draw a single-height dash
    
     JSR CPIXK              \ Call CPIXK to draw a single-height dash, i.e. the top
                            \ row of a double-height dash
    
     LDA Y1                 \ Fetch the y-coordinate of the row we just drew and
     DEC A                  \ decrement it, ready to draw the bottom row
    
    .DOT2
    
     JSR CPIXK              \ Call CPIXK to draw a single-height dash
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

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

Subroutine [CPIXK](cpixk.md) (category: Drawing pixels)

Draw a single-height dash on the dashboard

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Configuration variable [YELLOW2](../../all/workspaces.md#yellow2) = %00001111

Two mode 2 pixels of colour 3 (yellow)

[PXCL](../variable/pxcl.md "Previous routine")[CPIXK](cpixk.md "Next routine")
