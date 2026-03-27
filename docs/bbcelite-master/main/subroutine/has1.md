---
title: "The HAS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/has1.html"
---

[HALL](hall.md "Previous routine")[TACTICS (Part 1 of 7)](tactics_part_1_of_7.md "Next routine")
    
    
           Name: HAS1                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw a ship in the ship hangar
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-has1)
     Variations: See [code variations](../../related/compare/main/subroutine/has1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HALL](hall.md) calls HAS1
    
    
    
    
    
    * * *
    
    
     The ship's position within the hangar is determined by the arguments and the
     size of the ship's targetable area, as follows:
    
       * The x-coordinate is (x_sign x_hi 0) from the arguments, so the ship can be
         left of centre or right of centre
    
       * The y-coordinate is negative and is lower down the screen for smaller
         ships, so smaller ships are drawn closer to the ground (because they are)
    
       * The z-coordinate is positive, with both z_hi (which is 1 or 2) and z_lo
         coming from the arguments
    
    
    
    * * *
    
    
     Arguments:
    
       XX15                 Bits 0-7 = Ship's z_lo
                            Bit 0    = Ship's x_sign
    
       XX15+1               Bits 0-7 = Ship's x_hi
                            Bit 0    = Ship's z_hi (1 if clear, or 2 if set)
    
       XX15+2               Non-zero = Ship type to draw
                            0        = Don't draw anything
    
    
    
    
    .HAS1
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace and reset
                            \ the orientation vectors, with nosev pointing out of
                            \ the screen, so this puts the ship flat on the
                            \ horizontal deck (the y = 0 plane) with its nose
                            \ pointing towards us
    
     LDA XX15               \ Set z_lo = XX15
     STA INWK+6
    
     LSR A                  \ Set the sign bit of x_sign to bit 0 of A
     ROR INWK+2
    
     LDA XX15+1             \ Set x_hi = XX15+1
     STA INWK
    
     LSR A                  \ Set z_hi = 1 + bit 0 of XX15+1
     LDA #1
     ADC #0
     STA INWK+7
    
     LDA #%10000000         \ Set bit 7 of y_sign, so y is negative
     STA INWK+5
    
     STA RAT2               \ Set RAT2 = %10000000, so the yaw calls in HAL5 below
                            \ are negative
    
     LDA #&0B               \ Set the ship line heap pointer in INWK(34 33) to point
     STA INWK+34            \ to &0B00
    
     JSR DORND              \ We now perform a random number of small angle (3.6
     STA XSAV               \ degree) rotations to spin the ship on the deck while
                            \ keeping it flat on the deck (a bit like spinning a
                            \ bottle), so we set XSAV to a random number between 0
                            \ and 255 for the number of small yaw rotations to
                            \ perform, so the ship could be pointing in any
                            \ direction by the time we're done
    
    .HAL5
    
     LDX #21                \ Rotate (sidev_x, nosev_x) by a small angle (yaw)
     LDY #9
     JSR MVS5
    
     LDX #23                \ Rotate (sidev_y, nosev_y) by a small angle (yaw)
     LDY #11
     JSR MVS5
    
     LDX #25                \ Rotate (sidev_z, nosev_z) by a small angle (yaw)
     LDY #13
     JSR MVS5
    
     DEC XSAV               \ Decrement the yaw counter in XSAV
    
     BNE HAL5               \ Loop back to yaw a little more until we have yawed
                            \ by the number of times in XSAV
    
     LDY XX15+2             \ Set Y = XX15+2, the ship type of the ship we need to
                            \ draw
    
     BEQ HA1                \ If Y = 0, return from the subroutine (as HA1 contains
                            \ an RTS)
    
     TYA                    \ Set X = 2 * Y
     ASL A
     TAX
    
     LDA XX21-2,X           \ Set XX0(1 0) to the X-th address in the ship blueprint
     STA XX0                \ address lookup table at XX21, so XX0(1 0) now points
     LDA XX21-1,X           \ to the blueprint for the ship we need to draw
     STA XX0+1
    
     BEQ HA1                \ If the high byte of the blueprint address is 0, then
                            \ this is not a valid blueprint address, so return from
                            \ the subroutine (as HA1 contains an RTS)
    
     LDY #1                 \ Set Q = ship byte #1
     LDA (XX0),Y
     STA Q
    
     INY                    \ Set R = ship byte #2
     LDA (XX0),Y            \
     STA R                  \ so (R Q) contains the ship's targetable area, which is
                            \ a square number
    
     JSR LL5                \ Set Q = SQRT(R Q)
    
     LDA #100               \ Set y_lo = (100 - Q) / 2
     SBC Q                  \
     LSR A                  \ so the bigger the ship's targetable area, the smaller
     STA INWK+3             \ the magnitude of the y-coordinate, so because we set
                            \ y_sign to be negative above, this means smaller ships
                            \ are drawn lower down, i.e. closer to the ground, while
                            \ larger ships are drawn higher up, as you would expect
    
     JSR TIDY               \ Call TIDY to tidy up the orientation vectors, to
                            \ prevent the ship from getting elongated and out of
                            \ shape due to the imprecise nature of trigonometry
                            \ in assembly language
    
     JMP LL9                \ Jump to LL9 to display the ship and return from the
                            \ subroutine using a tail call
    
    .HA1
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Label [HA1](has1.md#ha1) is local to this routine

[X]

Label [HAL5](has1.md#hal5) is local to this routine

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [LL5](ll5.md) (category: Maths (Arithmetic))

Calculate Q = SQRT(R Q)

[X]

Subroutine [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Subroutine [MVS5](mvs5.md) (category: Moving)

Apply a 3.6 degree pitch or roll to an orientation vector

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [RAT2](../workspace/zp.md#rat2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the pitch and roll signs when moving objects and stardust

[X]

Subroutine [TIDY](tidy.md) (category: Maths (Geometry))

Orthonormalise the orientation vectors for a ship

[X]

Variable [XSAV](../workspace/zp.md#xsav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Configuration variable [XX21](../../all/workspaces.md#xx21) = &8000

The address of the ship blueprints lookup table, as set in elite-data.asm

[X]

Subroutine [ZINF](zinf.md) (category: Universe)

Reset the INWK workspace and orientation vectors

[HALL](hall.md "Previous routine")[TACTICS (Part 1 of 7)](tactics_part_1_of_7.md "Next routine")
