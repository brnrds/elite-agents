---
title: "The Main flight loop (Part 12 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_12_of_16.html"
---

[Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md "Previous routine")[Main flight loop (Part 13 of 16)](main_flight_loop_part_13_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 12 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Draw the ship, remove if killed, loop back
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-12-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_12_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Draw the ship
    
         * Process removal of killed ships
    
       * Loop back up to MAL1 to move onto the next ship in the local bubble
    
    
    
    
    .MA8
    
     JSR LL9                \ Call LL9 to draw the ship we're processing on-screen
    
    .MA15
    
     LDY #35                \ Fetch the ship's energy from byte #35 and copy it to
     LDA INWK+35            \ byte #35 in INF (so the ship's data in K% gets
     STA (INF),Y            \ updated)
    
     LDA NEWB               \ If bit 7 of the ship's NEWB flags is set, which means
     BMI KS1S               \ the ship has docked or been scooped, jump to KS1S to
                            \ skip the following, as we can't get a bounty for a
                            \ ship that's no longer around
    
     LDA INWK+31            \ If bit 7 of the ship's byte #31 is clear, then the
     BPL MAC1               \ ship hasn't been killed by energy bomb, collision or
                            \ laser fire, so jump to MAC1 to skip the following
    
     AND #%00100000         \ If bit 5 of the ship's byte #31 is clear then the
     BEQ MAC1               \ ship is no longer exploding, so jump to MAC1 to skip
                            \ the following
    
     LDA NEWB               \ Extract bit 6 of the ship's NEWB flags, so A = 64 if
     AND #%01000000         \ bit 6 is set, or 0 if it is clear. Bit 6 is set if
                            \ this ship is a cop, so A = 64 if we just killed a
                            \ policeman, otherwise it is 0
    
     ORA FIST               \ Update our FIST flag ("fugitive/innocent status") to
     STA FIST               \ at least the value in A, which will instantly make us
                            \ a fugitive if we just shot the sheriff, but won't
                            \ affect our status if the enemy wasn't a copper
    
     LDA DLY                \ If we already have an in-flight message on-screen (in
     ORA MJ                 \ which case DLY > 0), or we are in witchspace (in
     BNE KS1S               \ which case MJ > 0), jump to KS1S to skip showing an
                            \ on-screen bounty for this kill
    
     LDY #10                \ Fetch byte #10 of the ship's blueprint, which is the
     LDA (XX0),Y            \ low byte of the bounty awarded when this ship is
     BEQ KS1S               \ killed (in Cr * 10), and if it's zero jump to KS1S as
                            \ there is no on-screen bounty to display
    
     TAX                    \ Put the low byte of the bounty into X
    
     INY                    \ Fetch byte #11 of the ship's blueprint, which is the
     LDA (XX0),Y            \ high byte of the bounty awarded (in Cr * 10), and put
     TAY                    \ it into Y
    
     JSR MCASH              \ Call MCASH to add (Y X) to the cash pot
    
     LDA #0                 \ Print control code 0 (current cash, right-aligned to
     JSR MESS               \ width 9, then " CR", newline) as an in-flight message
    
    .KS1S
    
     JMP KS1                \ Process the killing of this ship (which removes this
                            \ ship from its slot and shuffles all the other ships
                            \ down to close up the gap)
    
    .MAC1
    
     LDA TYPE               \ If the ship we are processing is a planet or sun,
     BMI MA27               \ jump to MA27 to skip the following two instructions
    
     JSR FAROF              \ If the ship we are processing is a long way away (its
     BCC KS1S               \ distance in any one direction is > 224, jump to KS1S
                            \ to remove the ship from our local bubble, as it's just
                            \ left the building
    
    .MA27
    
     LDY #31                \ Fetch the ship's explosion/killed state from byte #31
     LDA INWK+31            \ and copy it to byte #31 in INF (so the ship's data in
     STA (INF),Y            \ K% gets updated)
    
     LDX XSAV               \ We're done processing this ship, so fetch the ship's
                            \ slot number, which we saved in XSAV back at the start
                            \ of the loop
    
     INX                    \ Increment the slot number to move on to the next slot
    
     JMP MAL1               \ And jump back up to the beginning of the loop to get
                            \ the next ship in the local bubble for processing
    

[X]

Variable [DLY](../workspace/wp.md#dly) in workspace [WP](../workspace/wp.md)

In-flight message delay

[X]

Subroutine [FAROF](farof.md) (category: Maths (Geometry))

Compare x_hi, y_hi and z_hi with 224

[X]

Variable [FIST](../workspace/wp.md#fist) in workspace [WP](../workspace/wp.md)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [KS1](ks1.md) (category: Universe)

Remove the current ship from our local bubble of universe

[X]

Label [KS1S](main_flight_loop_part_12_of_16.md#ks1s) is local to this routine

[X]

Subroutine [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Label [MA27](main_flight_loop_part_12_of_16.md#ma27) is local to this routine

[X]

Label [MAC1](main_flight_loop_part_12_of_16.md#mac1) is local to this routine

[X]

Entry point [MAL1](main_flight_loop_part_4_of_16.md#mal1) in subroutine [Main flight loop (Part 4 of 16)](main_flight_loop_part_4_of_16.md) (category: Main loop)

Marks the beginning of the ship analysis loop, so we can jump back here from part 12 of the main flight loop to work our way through each ship in the local bubble. We also jump back here when a ship is removed from the bubble, so we can continue processing from the next ship

[X]

Subroutine [MCASH](mcash.md) (category: Maths (Arithmetic))

Add an amount of cash to the cash pot

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[X]

Variable [MJ](../workspace/wp.md#mj) in workspace [WP](../workspace/wp.md)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Variable [XSAV](../workspace/zp.md#xsav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md "Previous routine")[Main flight loop (Part 13 of 16)](main_flight_loop_part_13_of_16.md "Next routine")
