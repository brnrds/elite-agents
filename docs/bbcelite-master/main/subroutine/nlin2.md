---
title: "The NLIN2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nlin2.html"
---

[NLIN](nlin.md "Previous routine")[HLOIN2](hloin2.md "Next routine")
    
    
           Name: NLIN2                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a screen-wide horizontal line at the pixel row in A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-nlin2)
     Variations: See [code variations](../../related/compare/main/subroutine/nlin2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [NLIN4](nlin4.md) calls NLIN2
    
    
    
    
    
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
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Entry point [HLOIN3](hloin.md#hloin3) in subroutine [HLOIN](hloin.md) (category: Drawing lines)

Draw a line from (X, Y1) to (X2, Y1) in the colour given in A

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](../workspace/zp.md#x2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Configuration variable [YELLOW](../../all/workspaces.md#yellow) = %00001111

Four mode 1 pixels of colour 1 (yellow)

[NLIN](nlin.md "Previous routine")[HLOIN2](hloin2.md "Next routine")
