---
title: "The LL9 (Part 9 of 12) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll9_part_9_of_12.html"
---

[LL9 (Part 8 of 12)](ll9_part_8_of_12.md "Previous routine")[LL9 (Part 10 of 12)](ll9_part_10_of_12.md "Next routine")
    
    
           Name: LL9 (Part 9 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Draw laser beams if the ship is firing its laser at us
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll9-part-9-of-12)
     Variations: See [code variations](../../related/compare/main/subroutine/ll9_part_9_of_12.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part sets things up so we can loop through the edges in the next part. It
     also adds a line to the ship line heap, if the ship is firing at us.
    
     When we get here, the heap at XX3 contains all the visible vertex screen
     coordinates.
    
    
    
    
    .LL72
    
     LDA XX1+31             \ If bit 5 of the ship's byte #31 is clear, then the
     AND #%00100000         \ ship is not currently exploding, so jump down to EE31
     BEQ EE31
    
     LDA XX1+31             \ The ship is exploding, so set bit 3 of the ship's byte
     ORA #%00001000         \ #31 to denote that we are drawing something on-screen
     STA XX1+31             \ for this ship
    
     JMP DOEXP              \ Jump to DOEXP to display the explosion cloud,
                            \ returning from the subroutine using a tail call
    
    .EE31
    
     LDY #9                 \ Fetch byte #9 of the ship's blueprint, which is the
     LDA (XX0),Y            \ number of edges, and store it in XX20
     STA XX20
    
     LDA #%00001000         \ Set bit 3 of A so the next instruction sets bit 3 of
                            \ the ship's byte #31 to denote that we are drawing
                            \ something on-screen for this ship
    
     ORA XX1+31             \ Apply bit 3 of A to the ship's byte #31, so if there
     STA XX1+31             \ was no ship already on screen, the bit is clear,
                            \ otherwise it is set
    
     LDY #0                 \ Set XX17 = 0, which we are going to use as a counter
    \STY LSNUM              \ for stepping through the ship's edges
     STY XX17               \
                            \ The STY is commented out in the original source
    
     BIT XX1+31             \ If bit 6 of the ship's byte #31 is clear, then the
     BVC LL170              \ ship is not firing its lasers, so jump to LL170 to
                            \ skip the drawing of laser lines
    
                            \ The ship is firing its laser at us, so we need to draw
                            \ the laser lines
    
     LDA XX1+31             \ Clear bit 6 of the ship's byte #31 so the ship doesn't
     AND #%10111111         \ keep firing endlessly
     STA XX1+31
    
     LDY #6                 \ Fetch byte #6 of the ship's blueprint, which is the
     LDA (XX0),Y            \ number * 4 of the vertex where the ship has its lasers
    
     TAY                    \ Put the vertex number into Y, where it can act as an
                            \ index into list of vertex screen coordinates we added
                            \ to the XX3 heap
    
     LDX XX3,Y              \ Fetch the x_lo coordinate of the laser vertex from the
     STX XX15               \ XX3 heap into XX15
    
     INX                    \ If X = 255 then the laser vertex is not visible, as
     BEQ LL170              \ the value we stored in part 2 wasn't overwritten by
                            \ the vertex calculation in part 6 and 7, so jump to
                            \ LL170 to skip drawing the laser lines
    
                            \ We now build a laser beam from the ship's laser vertex
                            \ towards our ship, as follows:
                            \
                            \   XX15(1 0) = laser vertex x-coordinate
                            \
                            \   XX15(3 2) = laser vertex y-coordinate
                            \
                            \   XX15(5 4) = x-coordinate of the end of the beam
                            \
                            \   XX12(1 0) = y-coordinate of the end of the beam
                            \
                            \ The end of the laser beam will be positioned to look
                            \ good, rather than being directly aimed at us, as
                            \ otherwise we would only see a flashing point of light
                            \ as they unleashed their attack
    
     LDX XX3+1,Y            \ Fetch the x_hi coordinate of the laser vertex from the
     STX XX15+1             \ XX3 heap into XX15+1
    
     INX                    \ If X = 255 then the laser vertex is not visible, as
     BEQ LL170              \ the value we stored in part 2 wasn't overwritten by
                            \ a vertex calculation in part 6 and 7, so jump to LL170
                            \ to skip drawing the laser beam
    
     LDX XX3+2,Y            \ Fetch the y_lo coordinate of the laser vertex from the
     STX XX15+2             \ XX3 heap into XX15+2
    
     LDX XX3+3,Y            \ Fetch the y_hi coordinate of the laser vertex from the
     STX XX15+3             \ XX3 heap into XX15+3
    
     LDA #0                 \ Set XX15(5 4) = 0, so their laser beam fires to the
     STA XX15+4             \ left edge of the screen
     STA XX15+5
    
     STA XX12+1             \ Set XX12(1 0) = the ship's z_lo coordinate, which will
     LDA XX1+6              \ effectively make the vertical position of the end of
     STA XX12               \ the laser beam move around as the ship moves in space
    
     LDA XX1+2              \ If the ship's x_sign is positive, skip the next
     BPL P%+4               \ instruction
    
     DEC XX15+4             \ The ship's x_sign is negative (i.e. it's on the left
                            \ side of the screen), so switch the laser beam so it
                            \ goes to the right edge of the screen by decrementing
                            \ XX15(5 4) to 255
    
     JSR CLIP               \ Call CLIP to see if the laser beam needs to be
                            \ clipped to fit on-screen, returning the clipped line's
                            \ end-points in (X1, Y1) and (X2, Y2)
    
     BCS LL170              \ If the C flag is set then the line is not visible on
                            \ screen, so jump to LL170 so we don't store this line
                            \ in the ship line heap
    
     JSR LSPUT              \ Draw the laser line using flicker-free animation, by
                            \ first drawing the new laser line and then erasing the
                            \ corresponding old line from the screen
    

[X]

Entry point [CLIP](ll145_part_1_of_4.md#clip) in subroutine [LL145 (Part 1 of 4)](ll145_part_1_of_4.md) (category: Drawing lines)

Another name for LL145

[X]

Subroutine [DOEXP](doexp.md) (category: Drawing ships)

Draw an exploding ship

[X]

Label [EE31](ll9_part_9_of_12.md#ee31) is local to this routine

[X]

Label [LL170](ll9_part_10_of_12.md#ll170) in subroutine [LL9 (Part 10 of 12)](ll9_part_10_of_12.md)

[X]

Subroutine [LSPUT](lsput.md) (category: Drawing lines)

Draw a ship line using flicker-free animation

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX1](../workspace/zp.md#xx1) in workspace [ZP](../workspace/zp.md)

This is an alias for INWK that is used in the main ship-drawing routine at LL9

[X]

Variable [XX12](../workspace/zp.md#xx12) in workspace [ZP](../workspace/zp.md)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Variable [XX17](../workspace/zp.md#xx17) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in BPRNT to store the number of characters to print, and as the edge counter in the main ship-drawing routine

[X]

Variable [XX20](../workspace/zp.md#xx20) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Workspace [XX3](../workspace/xx3.md) (category: Workspaces)

Temporary storage space for complex calculations

[LL9 (Part 8 of 12)](ll9_part_8_of_12.md "Previous routine")[LL9 (Part 10 of 12)](ll9_part_10_of_12.md "Next routine")
