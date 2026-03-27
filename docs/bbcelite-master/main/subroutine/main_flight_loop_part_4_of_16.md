---
title: "The Main flight loop (Part 4 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_4_of_16.html"
---

[Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md "Previous routine")[Main flight loop (Part 5 of 16)](main_flight_loop_part_5_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 4 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Copy the ship's data block from K% to the
                 zero-page workspace at INWK
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-4-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_4_of_16.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KS1](ks1.md) calls via MAL1
                 * [Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md) calls via MAL1
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Start looping through all the ships in the local bubble, and for each
         one:
    
         * Copy the ship's data block from K% to INWK
    
         * Set XX0 to point to the ship's blueprint (if this is a ship)
    
    
    
    * * *
    
    
     Other entry points:
    
       MAL1                 Marks the beginning of the ship analysis loop, so we
                            can jump back here from part 12 of the main flight loop
                            to work our way through each ship in the local bubble.
                            We also jump back here when a ship is removed from the
                            bubble, so we can continue processing from the next ship
    
    
    
    
    .MA3
    
     LDX #0                 \ We're about to work our way through all the ships in
                            \ our local bubble of universe, so set a counter in X,
                            \ starting from 0, to refer to each ship slot in turn
    
    .MAL1
    
     STX XSAV               \ Store the current slot number in XSAV
    
     LDA FRIN,X             \ Fetch the contents of this slot into A. If it is 0
     BNE P%+5               \ then this slot is empty and we have no more ships to
     JMP MA18               \ process, so jump to MA18 below, otherwise A contains
                            \ the type of ship that's in this slot, so skip over the
                            \ JMP MA18 instruction and keep going
    
     STA TYPE               \ Store the ship type in TYPE
    
     JSR GINF               \ Call GINF to fetch the address of the ship data block
                            \ for the ship in slot X and store it in INF. The data
                            \ block is in the K% workspace, which is where all the
                            \ ship data blocks are stored
    
                            \ Next we want to copy the ship data block from INF to
                            \ the zero-page workspace at INWK, so we can process it
                            \ more efficiently
    
     LDY #NI%-1             \ There are NI% bytes in each ship data block (and in
                            \ the INWK workspace, so we set a counter in Y so we can
                            \ loop through them
    
    .MAL2
    
     LDA (INF),Y            \ Load the Y-th byte of INF and store it in the Y-th
     STA INWK,Y             \ byte of INWK
    
     DEY                    \ Decrement the loop counter
    
     BPL MAL2               \ Loop back for the next byte until we have copied the
                            \ last byte from INF to INWK
    
     LDA TYPE               \ If the ship type is negative then this indicates a
     BMI MA21               \ planet or sun, so jump down to MA21, as the next bit
                            \ sets up a pointer to the ship blueprint, and then
                            \ checks for energy bomb damage, and neither of these
                            \ apply to planets and suns
    
     ASL A                  \ Set Y = ship type * 2
     TAY
    
     LDA XX21-2,Y           \ The ship blueprints at XX21 start with a lookup
     STA XX0                \ table that points to the individual ship blueprints,
                            \ so this fetches the low byte of this particular ship
                            \ type's blueprint and stores it in XX0
    
     LDA XX21-1,Y           \ Fetch the high byte of this particular ship type's
     STA XX0+1              \ blueprint and store it in XX0+1
    

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Subroutine [GINF](ginf.md) (category: Universe)

Fetch the address of a ship's data block into INF

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Label [MA18](main_flight_loop_part_13_of_16.md#ma18) in subroutine [Main flight loop (Part 13 of 16)](main_flight_loop_part_13_of_16.md)

[X]

Label [MA21](main_flight_loop_part_6_of_16.md#ma21) in subroutine [Main flight loop (Part 6 of 16)](main_flight_loop_part_6_of_16.md)

[X]

Label [MAL2](main_flight_loop_part_4_of_16.md#mal2) is local to this routine

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Variable [XSAV](../workspace/zp.md#xsav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Configuration variable [XX21](../../all/workspaces.md#xx21) = &8000

The address of the ship blueprints lookup table, as set in elite-data.asm

[Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md "Previous routine")[Main flight loop (Part 5 of 16)](main_flight_loop_part_5_of_16.md "Next routine")
