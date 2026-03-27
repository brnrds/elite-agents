---
title: "The PXCL variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/pxcl.html"
---

[PIXEL](../subroutine/pixel.md "Previous routine")[DOT](../subroutine/dot.md "Next routine")
    
    
           Name: PXCL                                                    [Show more]
           Type: Variable
       Category: Drawing pixels
        Summary: A four-colour mode 1 pixel byte that represents a dot's distance
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-pxcl)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     The following table contains colour bytes for two-pixel mode 1 pixels, with
     the index into the table representing distance. Closer pixels are at the top,
     so the closest pixels are cyan/red, then yellow, then red, then red/yellow,
     then yellow.
    
     That said, this table is only used with odd distance values, as set in the
     parasite's PIXEL3 routine, so in practice the four distances are yellow, red,
     red/yellow, yellow.
    
    
    
    
    .PXCL
    
     EQUB WHITE             \ Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)
     EQUB %00001111         \ Four mode 1 pixels of colour 1 (yellow)
     EQUB %00001111         \ Four mode 1 pixels of colour 1 (yellow)
     EQUB %11110000         \ Four mode 1 pixels of colour 2 (red)
     EQUB %11110000         \ Four mode 1 pixels of colour 2 (red)
     EQUB %10100101         \ Four mode 1 pixels of colour 2, 1, 2, 1 (red/yellow)
     EQUB %10100101         \ Four mode 1 pixels of colour 2, 1, 2, 1 (red/yellow)
     EQUB %00001111         \ Four mode 1 pixels of colour 1, 1, 1, 1 (yellow)
    

[X]

Configuration variable [WHITE](../../all/workspaces.md#white) = %11111010

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[PIXEL](../subroutine/pixel.md "Previous routine")[DOT](../subroutine/dot.md "Next routine")
