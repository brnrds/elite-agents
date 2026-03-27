---
title: "The SIGHT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sight.html"
---

[LOOK1](look1.md "Previous routine")[sightcol](../variable/sightcol.md "Next routine")
    
    
           Name: SIGHT                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Draw the laser crosshairs
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-sight)
     Variations: See [code variations](../../related/compare/main/subroutine/sight.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOOK1](look1.md) calls SIGHT
    
    
    
    
    
    
    .SIGHT
    
     LDY VIEW               \ Fetch the laser power for our new view
     LDA LASER,Y
    
     BEQ LO2                \ If it is zero (i.e. there is no laser fitted to this
                            \ view), jump to LO2 to return from the subroutine (as
                            \ LO2 contains an RTS)
    
     LDY #0                 \ Set Y to 0, to represent a pulse laser
    
     CMP #POW               \ If the laser power in A is equal to a pulse laser,
     BEQ SIG1               \ jump to SIG1 with Y = 0
    
     INY                    \ Increment Y to 1, to represent a beam laser
    
     CMP #POW+128           \ If the laser power in A is equal to a beam laser,
     BEQ SIG1               \ jump to SIG1 with Y = 1
    
     INY                    \ Increment Y to 2, to represent a military laser
    
     CMP #Armlas            \ If the laser power in A is equal to a military laser,
     BEQ SIG1               \ jump to SIG1 with Y = 2
    
     INY                    \ Increment Y to 3, to represent a mining laser
    
    .SIG1
    
     LDA sightcol,Y         \ Set the colour from the sightcol table
     STA COL
    
     LDA #128               \ Set QQ19 to the x-coordinate of the centre of the
     STA QQ19               \ screen
    
     LDA #Y-24              \ Set QQ19+1 to the y-coordinate of the centre of the
     STA QQ19+1             \ screen, minus 24 (because TT15 will add 24 to the
                            \ coordinate when it draws the crosshairs)
    
     LDA #20                \ Set QQ19+2 to size 20 for the crosshairs size
     STA QQ19+2
    
     JSR TT15               \ Call TT15 to draw crosshairs of size 20 just to the
                            \ left of the middle of the screen
    
     LDA #10                \ Set QQ19+2 to size 10 for the crosshairs size
     STA QQ19+2
    
     JMP TT15               \ Call TT15 to draw crosshairs of size 10 at the same
                            \ location, which will remove the centre part from the
                            \ laser crosshairs, leaving a gap in the middle, and
                            \ return from the subroutine using a tail call
    

[X]

Configuration variable [Armlas](../../all/workspaces.md#armlas) = INT(128.5 + 1.5*POW)

Military laser power

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Variable [LASER](../workspace/wp.md#laser) in workspace [WP](../workspace/wp.md)

The specifications of the lasers fitted to each of the four space views

[X]

Entry point [LO2](look1.md#lo2) in subroutine [LOOK1](look1.md) (category: Flight)

Contains an RTS

[X]

Configuration variable [POW](../../all/workspaces.md#pow) = 15

Pulse laser power

[X]

Variable [QQ19](../workspace/zp.md#qq19) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [SIG1](sight.md#sig1) is local to this routine

[X]

Subroutine [TT15](tt15.md) (category: Drawing lines)

Draw a set of crosshairs

[X]

Variable [VIEW](../workspace/wp.md#view) in workspace [WP](../workspace/wp.md)

The number of the current space view

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [sightcol](../variable/sightcol.md) (category: Drawing lines)

Colours for the crosshair sights on the different laser types

[LOOK1](look1.md "Previous routine")[sightcol](../variable/sightcol.md "Next routine")
