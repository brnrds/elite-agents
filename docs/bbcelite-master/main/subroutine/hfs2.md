---
title: "The HFS2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hfs2.html"
---

[LAUN](laun.md "Previous routine")[STARS2](stars2.md "Next routine")
    
    
           Name: HFS2                                                    [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw the launch or hyperspace tunnel
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-hfs2)
     Variations: See [code variations](../../related/compare/main/subroutine/hfs2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL164](ll164.md) calls HFS2
                 * [TT110](tt110.md) calls via HFS1
    
    
    
    
    
    * * *
    
    
     The animation gets drawn like this. First, we draw a circle of radius 8 at the
     centre, and then double the radius, draw another circle, double the radius
     again and draw a circle, and we keep doing this until the radius is bigger
     than 160 (which goes beyond the edge of the screen, which is 256 pixels wide,
     equivalent to a radius of 128). We then repeat this whole process for an
     initial circle of radius 9, then radius 10, all the way up to radius 15.
    
     This has the effect of making the tunnel appear to be racing towards us as we
     hurtle out into hyperspace or through the space station's docking tunnel.
    
     The hyperspace effect is done in a full mode 2 screen, which makes the rings
     all coloured and zig-zaggy, while the launch screen is in the normal
     four-colour mode 1 screen.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The step size of the straight lines making up the rings
                            (4 for launch, 8 for hyperspace)
    
    
    
    * * *
    
    
     Other entry points:
    
       HFS1                 Don't clear the screen, and draw 8 concentric rings
                            with the step size in STP
    
    
    
    
    .HFS2
    
     STA STP                \ Store the step size in A
    
     LDA QQ11               \ Store the current view type in QQ11 on the stack
     PHA
    
     LDA #0                 \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 0 (the space
                            \ view)
    
     PLA                    \ Restore the view type from the stack
     STA QQ11
    
    .HFS1
    
     LDX #X                 \ Set K3 = #X (the x-coordinate of the centre of the
     STX K3                 \ screen)
    
     LDX #Y                 \ Set K4 = #Y (the y-coordinate of the centre of the
     STX K4                 \ screen)
    
     LDX #0                 \ Set X = 0
    
     STX XX4                \ Set XX4 = 0, which we will use as a counter for
                            \ drawing eight concentric rings
    
     STX K3+1               \ Set the high bytes of K3(1 0) and K4(1 0) to 0
     STX K4+1
    
    .HFL5
    
     JSR HFL1               \ Call HFL1 below to draw a set of rings, with each one
                            \ twice the radius of the previous one, until they won't
                            \ fit on-screen
    
     INC XX4                \ Increment the counter and fetch it into X
     LDX XX4
    
     CPX #8                 \ If we haven't drawn 8 sets of rings yet, loop back to
     BNE HFL5               \ HFL5 to draw the next ring
    
     RTS                    \ Return from the subroutine
    
    .HFL1
    
     LDA XX4                \ Set K to the ring number in XX4 (0-7) + 8, so K has
     AND #7                 \ a value of 8 to 15, which we will use as the starting
     CLC                    \ radius for our next set of rings
     ADC #8
     STA K
    
    .HFL2
    
     LDA #1                 \ Set LSP = 1 to reset the ball line heap
     STA LSP
    
     JSR CIRCLE2            \ Call CIRCLE2 to draw a circle with the centre at
                            \ (K3(1 0), K4(1 0)) and radius K
    
     ASL K                  \ Double the radius in K
    
     BCS HF8                \ If the radius had a 1 in bit 7 before the above shift,
                            \ then doubling K will means the circle will no longer
                            \ fit on the screen (which is width 256), so jump to
                            \ HF8 to stop drawing circles
    
     LDA K                  \ If the radius in K <= 160, loop back to HFL2 to draw
     CMP #160               \ another one
     BCC HFL2
    
    .HF8
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [CIRCLE2](circle2.md) (category: Drawing circles)

Draw a circle (for the planet or chart)

[X]

Label [HF8](hfs2.md#hf8) is local to this routine

[X]

Label [HFL1](hfs2.md#hfl1) is local to this routine

[X]

Label [HFL2](hfs2.md#hfl2) is local to this routine

[X]

Label [HFL5](hfs2.md#hfl5) is local to this routine

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

Variable [LSP](../workspace/zp.md#lsp) in workspace [ZP](../workspace/zp.md)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Variable [STP](../workspace/zp.md#stp) in workspace [ZP](../workspace/zp.md)

The step size for drawing circles

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Configuration variable [X](../../all/workspaces.md#x) = 128

The centre x-coordinate of the 256 x 192 space view

[X]

Variable [XX4](../workspace/zp.md#xx4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[LAUN](laun.md "Previous routine")[STARS2](stars2.md "Next routine")
