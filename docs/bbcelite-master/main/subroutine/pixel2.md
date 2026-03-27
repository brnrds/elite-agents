---
title: "The PIXEL2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pixel2.html"
---

[PIX1](pix1.md "Previous routine")[PIXEL](pixel.md "Next routine")
    
    
           Name: PIXEL2                                                  [Show more]
           Type: Subroutine
       Category: Drawing pixels
        Summary: Draw a stardust particle relative to the screen centre
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-pixel2)
     Variations: See [code variations](../../related/compare/main/subroutine/pixel2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [FLIP](flip.md) calls PIXEL2
                 * [nWq](nwq.md) calls PIXEL2
                 * [STARS1](stars1.md) calls PIXEL2
                 * [STARS2](stars2.md) calls PIXEL2
                 * [STARS6](stars6.md) calls PIXEL2
    
    
    
    
    
    * * *
    
    
     Draw a point (X1, Y1) from the middle of the screen with a size determined by
     a distance value. Used to draw stardust particles.
    
    
    
    * * *
    
    
     Arguments:
    
       X1                   The x-coordinate offset
    
       Y1                   The y-coordinate offset (positive means up the screen
                            from the centre, negative means down the screen)
    
       ZZ                   The distance of the point, with bigger distances drawing
                            smaller points:
    
                              * ZZ < 80           Double-height four-pixel square
    
                              * 80 <= ZZ <= 143   Single-height two-pixel dash
    
                              * ZZ > 143          Single-height one-pixel dot
    
    
    
    
    .PIXEL2
    
     LDA X1                 \ Fetch the x-coordinate offset into A
    
     BPL PX21               \ If the x-coordinate offset is positive, jump to PX21
                            \ to skip the following negation
    
     EOR #%01111111         \ The x-coordinate offset is negative, so flip all the
     CLC                    \ bits apart from the sign bit and add 1, to convert it
     ADC #1                 \ from a sign-magnitude number to a signed number
    
    .PX21
    
     EOR #%10000000         \ Set X = X1 + 128
     TAX                    \
                            \ So X is now the offset converted to an x-coordinate,
                            \ centred on x-coordinate 128
    
     LDA Y1                 \ Fetch the y-coordinate offset into A and clear the
     AND #%01111111         \ sign bit, so A = |Y1|
    
     CMP #Y                 \ If |Y1| >= #Y then it's off the screen (as #Y is half
     BCS PXR1               \ the screen height), so return from the subroutine (as
                            \ PXR1 contains an RTS)
    
     LDA Y1                 \ Fetch the y-coordinate offset into A
    
     BPL PX22               \ If the y-coordinate offset is positive, jump to PX22
                            \ to skip the following negation
    
     EOR #%01111111         \ The y-coordinate offset is negative, so flip all the
     ADC #1                 \ bits apart from the sign bit and add 1, to convert it
                            \ from a sign-magnitude number to a signed number
    
    .PX22
    
     STA T                  \ Set A = #Y + 1 - Y1
     LDA #Y+1               \
     SBC T                  \ So if Y1 is positive we display the point up from the
                            \ centre at y-coordinate 97, while a negative Y1 means
                            \ down from the centre
    
                            \ Fall through into PIXEL to draw the stardust at the
                            \ screen coordinates in (X, A)
    

[X]

Label [PX21](pixel2.md#px21) is local to this routine

[X]

Label [PX22](pixel2.md#px22) is local to this routine

[X]

Entry point [PXR1](pixel.md#pxr1) in subroutine [PIXEL](pixel.md) (category: Drawing pixels)

Contains an RTS

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[PIX1](pix1.md "Previous routine")[PIXEL](pixel.md "Next routine")
