---
title: "The PROJ subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/proj.html"
---

[ABORT2](abort2.md "Previous routine")[PL2](pl2.md "Next routine")
    
    
           Name: PROJ                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Project the current ship or planet onto the screen
      Deep dive: [Extended screen coordinates](https://elite.bbcelite.com/deep_dives/extended_screen_coordinates.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-proj)
     Variations: See [code variations](../../related/compare/main/subroutine/proj.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLANET](planet.md) calls PROJ
                 * [SHPPT](shppt.md) calls PROJ
    
    
    
    
    
    * * *
    
    
     Project the current ship's location or the planet onto the screen, either
     returning the screen coordinates of the projection (if it's on-screen), or
     returning an error via the C flag.
    
     In this context, "on-screen" means that the point is projected into the
     following range:
    
       centre of screen - 1024 < x < centre of screen + 1024
       centre of screen - 1024 < y < centre of screen + 1024
    
     This is to cater for ships (and, more likely, planets and suns) whose centres
     are off-screen but whose edges may still be visible.
    
     The projection calculation is:
    
       K3(1 0) = #X + x / z
       K4(1 0) = #Y + y / z
    
     where #X and #Y are the pixel x-coordinate and y-coordinate of the centre of
     the screen.
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The ship data block for the ship to project on-screen
    
    
    
    * * *
    
    
     Returns:
    
       K3(1 0)              The x-coordinate of the ship's projection on-screen
    
       K4(1 0)              The y-coordinate of the ship's projection on-screen
    
       C flag               Set if the ship's projection doesn't fit on the screen,
                            clear if it does project onto the screen
    
       A                    Contains K4+1, the high byte of the y-coordinate
    
    
    
    
    .PROJ
    
     LDA INWK               \ Set P(1 0) = (x_hi x_lo)
     STA P                  \            = x
     LDA INWK+1
     STA P+1
    
     LDA INWK+2             \ Set A = x_sign
    
     JSR PLS6               \ Call PLS6 to calculate:
                            \
                            \   (X K) = (A P+1 P) / (z_sign z_hi z_lo)
                            \         = (x_sign x_hi x_lo) / (z_sign z_hi z_lo)
                            \         = x / z
    
     BCS PL2-1              \ If the C flag is set then the result overflowed and
                            \ the coordinate doesn't fit on the screen, so return
                            \ from the subroutine with the C flag set (as PL2-1
                            \ contains an RTS)
    
     LDA K                  \ Set K3(1 0) = (X K) + #X
     ADC #X                 \             = #X + x / z
     STA K3                 \
                            \ first doing the low bytes
    
     TXA                    \ And then the high bytes. #X is the x-coordinate of
     ADC #0                 \ the centre of the space view, so this converts the
     STA K3+1               \ space x-coordinate into a screen x-coordinate
    
     LDA INWK+3             \ Set P(1 0) = (y_hi y_lo)
     STA P
     LDA INWK+4
     STA P+1
    
     LDA INWK+5             \ Set A = -y_sign
     EOR #%10000000
    
     JSR PLS6               \ Call PLS6 to calculate:
                            \
                            \   (X K) = (A P+1 P) / (z_sign z_hi z_lo)
                            \         = -(y_sign y_hi y_lo) / (z_sign z_hi z_lo)
                            \         = -y / z
    
     BCS PL2-1              \ If the C flag is set then the result overflowed and
                            \ the coordinate doesn't fit on the screen, so return
                            \ from the subroutine with the C flag set (as PL2-1
                            \ contains an RTS)
    
     LDA K                  \ Set K4(1 0) = (X K) + #Y
     ADC #Y                 \             = #Y - y / z
     STA K4                 \
                            \ first doing the low bytes
    
     TXA                    \ And then the high bytes. #Y is the y-coordinate of
     ADC #0                 \ the centre of the space view, so this converts the
     STA K4+1               \ space x-coordinate into a screen y-coordinate
    
     CLC                    \ Clear the C flag to indicate success
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [K4](../workspace/zp.md#k4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Entry point [PL2-1](pl2.md#pl2) in subroutine [PL2](pl2.md) (category: Drawing planets)

Contains an RTS

[X]

Subroutine [PLS6](pls6.md) (category: Drawing planets)

Calculate (X K) = (A P+1 P) / (z_sign z_hi z_lo)

[X]

Configuration variable [X](../../all/workspaces.md#x) = 128

The centre x-coordinate of the 256 x 192 space view

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[ABORT2](abort2.md "Previous routine")[PL2](pl2.md "Next routine")
