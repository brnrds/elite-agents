---
title: "The LL9 (Part 1 of 12) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll9_part_1_of_12.html"
---

[LL51](ll51.md "Previous routine")[LL9 (Part 2 of 12)](ll9_part_2_of_12.md "Next routine")
    
    
           Name: LL9 (Part 1 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Check if ship is exploding, check if ship is in front
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll9-part-1-of-12)
     Variations: See [code variations](../../related/compare/main/subroutine/ll9_part_1_of_12.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](brief.md) calls LL9
                 * [ESCAPE](escape.md) calls LL9
                 * [HAS1](has1.md) calls LL9
                 * [Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md) calls LL9
                 * [PAS1](pas1.md) calls LL9
                 * [PAUSE](pause.md) calls LL9
                 * [TITLE](title.md) calls LL9
    
    
    
    
    
    * * *
    
    
     This routine draws the current ship on the screen. This part checks to see if
     the ship is exploding, or if it should start exploding, and if it does it sets
     things up accordingly.
    
     In this code, XX1 is used to point to the current ship's data block at INWK
     (the two labels are interchangeable).
    
    
    
    * * *
    
    
     Arguments:
    
       XX1                  XX1 shares its location with INWK, which contains the
                            zero-page copy of the data block for this ship from the
                            K% workspace
    
       INF                  The address of the data block for this ship in workspace
                            K%
    
       XX19(1 0)            XX19(1 0) shares its location with INWK(34 33), which
                            contains the ship line heap address pointer
    
       XX0                  The address of the blueprint for this ship
    
    
    
    * * *
    
    
     Other entry points:
    
       EE51                 Remove the current ship from the screen, called from
                            SHPPT before drawing the ship as a point
    
    
    
    
    .LL25
    
     JMP PLANET             \ Jump to the PLANET routine, returning from the
                            \ subroutine using a tail call
    
    .LL9
    
     LDX TYPE               \ If the ship type is negative then this indicates a
     BMI LL25               \ planet or sun, so jump to PLANET via LL25 above
    
     LDA shpcol,X           \ Set A to the ship colour for this type, from the X-th
                            \ entry in the shpcol table
    
     STA COL                \ Switch to this colour
    
     LDA #31                \ Set XX4 = 31 to store the ship's distance for later
     STA XX4                \ comparison with the visibility distance. We will
                            \ update this value below with the actual ship's
                            \ distance if it turns out to be visible on-screen
    
                            \ We now set things up for flicker-free ship plotting,
                            \ by setting the following:
                            \
                            \   LSNUM = offset to the first coordinate in the ship's
                            \           line heap
                            \
                            \   LSNUM2 = the number of bytes in the heap for the
                            \            ship that's currently on-screen (or 0 if
                            \            there is no ship currently on-screen)
    
     LDY #1                 \ Set LSNUM = 1, the offset of the first set of line
     STY LSNUM              \ coordinates in the ship line heap
    
     DEY                    \ Decrement Y to 0
    
     LDA #%00001000         \ If bit 3 of the ship's byte #31 is set, then the ship
     BIT INWK+31            \ is currently being drawn on-screen, so skip the
     BNE P%+5               \ following two instructions
    
     LDA #0                 \ The ship is not being drawn on screen, so set A = 0
                            \ so that LSNUM2 gets set to 0 below (as there are no
                            \ existing coordinates on the ship line heap for this
                            \ ship)
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &B1 &BD, or BIT &BDB1 which does nothing apart
                            \ from affect the flags
    
     LDA (XX19),Y           \ Set LSNUM2 to the first byte of the ship's line heap,
     STA LSNUM2             \ which contains the number of bytes in the heap
    
     LDA NEWB               \ If bit 7 of the ship's NEWB flags is set, then the
     BMI EE51               \ ship has been scooped or has docked, so jump down to
                            \ EE51 to redraw its wireframe, to remove it from the
                            \ screen
    
     LDA #%00100000         \ If bit 5 of the ship's byte #31 is set, then the ship
     BIT XX1+31             \ is currently exploding, so jump down to EE28
     BNE EE28
    
     BPL EE28               \ If bit 7 of the ship's byte #31 is clear then the ship
                            \ has not just been killed, so jump down to EE28
    
                            \ Otherwise bit 5 is clear and bit 7 is set, so the ship
                            \ is not yet exploding but it has been killed, so we
                            \ need to start an explosion
    
     ORA XX1+31             \ Clear bits 6 and 7 of the ship's byte #31, to stop the
     AND #%00111111         \ ship from firing its laser and to mark it as no longer
     STA XX1+31             \ having just been killed
    
     LDA #0                 \ Set the ship's acceleration in byte #31 to 0, updating
     LDY #28                \ the byte in the workspace K% data block so we don't
     STA (INF),Y            \ have to copy it back from INWK later
    
     LDY #30                \ Set the ship's pitch counter in byte #30 to 0, to stop
     STA (INF),Y            \ the ship from pitching
    
     JSR EE51               \ Call EE51 to remove the ship from the screen
    
                            \ We now need to set up a new explosion cloud. We
                            \ initialise it with a size of 18 (which gets increased
                            \ by 4 every time the cloud gets redrawn), and the
                            \ explosion count (i.e. the number of particles in the
                            \ explosion), which go into bytes 1 and 2 of the ship
                            \ line heap. See DOEXP for more details of explosion
                            \ clouds
    
     LDY #1                 \ Set byte #1 of the ship line heap to 18, the initial
     LDA #18                \ size of the explosion cloud
     STA (XX19),Y
    
     LDY #7                 \ Fetch byte #7 from the ship's blueprint, which
     LDA (XX0),Y            \ determines the explosion count (i.e. the number of
     LDY #2                 \ vertices used as origins for explosion clouds), and
     STA (XX19),Y           \ store it in byte #2 of the ship line heap
    
                            \ The following loop sets bytes 3-6 of the of the ship
                            \ line heap to random numbers
    
    .EE55
    
     INY                    \ Increment Y (so the loop starts at 3)
    
     JSR DORND              \ Set A and X to random numbers
    
     STA (XX19),Y           \ Store A in the Y-th byte of the ship line heap
    
     CPY #6                 \ Loop back until we have randomised the 6th byte
     BNE EE55
    
    .EE28
    
     LDA XX1+8              \ Set A = z_sign
    
    .EE49
    
     BPL LL10               \ If A is positive, i.e. the ship is in front of us,
                            \ jump down to LL10
    
    .LL14
    
                            \ The following removes the ship from the screen by
                            \ redrawing it (or, if it is exploding, by redrawing the
                            \ explosion cloud). We call it when the ship is no
                            \ longer on-screen, is too far away to be fully drawn,
                            \ and so on
    
     LDA XX1+31             \ If bit 5 of the ship's byte #31 is clear, then the
     AND #%00100000         \ ship is not currently exploding, so jump down to EE51
     BEQ EE51               \ to redraw its wireframe
    
     LDA XX1+31             \ The ship is exploding, so clear bit 3 of the ship's
     AND #%11110111         \ byte #31 to denote that the ship is no longer being
     STA XX1+31             \ drawn on-screen
    
     JMP DOEXP              \ Jump to DOEXP to display the explosion cloud, which
                            \ will remove it from the screen, returning from the
                            \ subroutine using a tail call
    
    .EE51
    
     LDA #%00001000         \ If bit 3 of the ship's byte #31 is clear, then there
     BIT XX1+31             \ is already nothing being shown for this ship, so
     BEQ LL10-1             \ return from the subroutine (as LL10-1 contains an RTS)
    
     EOR XX1+31             \ Otherwise flip bit 3 of byte #31 and store it (which
     STA XX1+31             \ clears bit 3 as we know it was set before the EOR), so
                            \ this sets this ship as no longer being drawn on-screen
    
     JMP LSCLR              \ Jump to LSCLR to draw the ship, which removes it from
                            \ the screen, returning from the subroutine using a
                            \ tail call
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [DOEXP](doexp.md) (category: Drawing ships)

Draw an exploding ship

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Label [EE28](ll9_part_1_of_12.md#ee28) is local to this routine

[X]

Entry point [EE51](ll9_part_1_of_12.md#ee51) in subroutine [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) (category: Drawing ships)

Remove the current ship from the screen, called from SHPPT before drawing the ship as a point

[X]

Label [EE55](ll9_part_1_of_12.md#ee55) is local to this routine

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Label [LL10](ll9_part_2_of_12.md#ll10) in subroutine [LL9 (Part 2 of 12)](ll9_part_2_of_12.md)

[X]

Entry point [LL10-1](ll9_part_2_of_12.md#ll10) in subroutine [LL9 (Part 2 of 12)](ll9_part_2_of_12.md) (category: Drawing ships)

Contains an RTS

[X]

Label [LL25](ll9_part_1_of_12.md#ll25) is local to this routine

[X]

Entry point [LSCLR](ll9_part_12_of_12.md#lsclr) in subroutine [LL9 (Part 12 of 12)](ll9_part_12_of_12.md) (category: Drawing ships)

Draw any remaining lines from the old ship that are still in the ship line heap

[X]

Variable [LSNUM](../workspace/zp.md#lsnum) in workspace [ZP](../workspace/zp.md)

The pointer to the current position in the ship line heap as we work our way through the new ship's edges (and the corresponding old ship's edges) when drawing the ship in the main ship-drawing routine at LL9

[X]

Variable [LSNUM2](../workspace/zp.md#lsnum2) in workspace [ZP](../workspace/zp.md)

The size of the existing ship line heap for the ship we are drawing in LL9, i.e. the number of lines in the old ship that is currently shown on-screen and which we need to erase

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Subroutine [PLANET](planet.md) (category: Drawing planets)

Draw the planet or sun

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX1](../workspace/zp.md#xx1) in workspace [ZP](../workspace/zp.md)

This is an alias for INWK that is used in the main ship-drawing routine at LL9

[X]

Variable [XX19](../workspace/zp.md#xx19) in workspace [ZP](../workspace/zp.md)

XX19(1 0) shares its location with INWK(34 33), which contains the address of the ship line heap

[X]

Variable [XX4](../workspace/zp.md#xx4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [shpcol](../variable/shpcol.md) (category: Drawing ships)

Ship colours

[LL51](ll51.md "Previous routine")[LL9 (Part 2 of 12)](ll9_part_2_of_12.md "Next routine")
