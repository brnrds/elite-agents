---
title: "The WP workspace - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/workspace/wp.html"
---

[K%](k_per_cent.md "Previous routine")[TVT3](../variable/tvt3.md "Next routine")
    
    
           Name: WP                                                      [Show more]
           Type: Workspace
        Address: &0E41 to &12A9
       Category: Workspaces
        Summary: Ship slots, variables
    
    
        Context: See this workspace [in context in the source code](../../all/workspaces.md#header-wp)
     Variations: See [code variations](../../related/compare/main/workspace/wp.md) for this workspace in the different versions
     References: No direct references to this workspace in this source file
    
    
    
    
    
    
     ORG &0E41              \ Set the assembly address to &0E41
    
    .WP
    
     SKIP 0                 \ The start of the WP workspace
    
    .FRIN
    
     SKIP NOSH + 1          \ Slots for the ships in the local bubble of universe
                            \
                            \ There are #NOSH + 1 slots, but the ship-spawning
                            \ routine at NWSHP only populates #NOSH of them, so
                            \ there are 13 slots but only 12 are used for ships
                            \ (the last slot is effectively used as a null
                            \ terminator when shuffling the slots down in the
                            \ KILLSHP routine)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEATH](../subroutine/death.md)
                            \   * [DEEOR](../subroutine/deeor.md)
                            \   * [DEEORS](../subroutine/deeors.md)
                            \   * [FRMIS](../subroutine/frmis.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [KS2](../subroutine/ks2.md)
                            \   * [KS4](../subroutine/ks4.md)
                            \   * [Main flight loop (Part 4 of 16)](../subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [NWSPS](../subroutine/nwsps.md)
                            \   * [STATUS](../subroutine/status.md)
                            \   * [WARP](../subroutine/warp.md)
                            \   * [WPSHPS](../subroutine/wpshps.md)
                            \   * [ZERO](../subroutine/zero.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .MANY
    
     SKIP SST               \ The number of ships of each type in the local bubble
                            \ of universe
                            \
                            \ The number of ships of type X in the local bubble is
                            \ stored at MANY+X
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [Main game loop (Part 3 of 6)](../subroutine/main_game_loop_part_3_of_6.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \   * [MJP](../subroutine/mjp.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [TACTICS (Part 2 of 7)](../subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 3 of 7)](../subroutine/tactics_part_3_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SSPR
    
     SKIP NTY + 1 - SST     \ "Space station present" flag
                            \
                            \   * Non-zero if we are inside the space station's safe
                            \     zone
                            \
                            \   * 0 if we aren't (in which case we can show the sun)
                            \
                            \ This flag is at MANY+SST, which is no coincidence, as
                            \ MANY+SST is a count of how many space stations there
                            \ are in our local bubble, which is the same as saying
                            \ "space station present"
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [COMPAS](../subroutine/compas.md)
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [KS4](../subroutine/ks4.md)
                            \   * [Main flight loop (Part 14 of 16)](../subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../subroutine/main_game_loop_part_2_of_6.md)
                            \   * [Main game loop (Part 3 of 6)](../subroutine/main_game_loop_part_3_of_6.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [TACTICS (Part 3 of 7)](../subroutine/tactics_part_3_of_7.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JUNK
    
     SKIP 1                 \ The amount of junk in the local bubble
                            \
                            \ "Junk" is defined as being one of these:
                            \
                            \   * Escape pod
                            \   * Alloy plate
                            \   * Cargo canister
                            \   * Asteroid
                            \   * Splinter
                            \   * Shuttle
                            \   * Transporter
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [Main game loop (Part 2 of 6)](../subroutine/main_game_loop_part_2_of_6.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [STATUS](../subroutine/status.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .auto
    
     SKIP 1                 \ Docking computer activation status
                            \
                            \   * 0 = Docking computer is off
                            \
                            \   * Non-zero = Docking computer is running
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [cntr](../subroutine/cntr.md)
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ECMP
    
     SKIP 1                 \ Our E.C.M. status
                            \
                            \   * 0 = E.C.M. is off
                            \
                            \   * Non-zero = E.C.M. is on
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ECMOF](../subroutine/ecmof.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../subroutine/main_flight_loop_part_16_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .MJ
    
     SKIP 1                 \ Are we in witchspace (i.e. have we mis-jumped)?
                            \
                            \   * 0 = no, we are in normal space
                            \
                            \   * &FF = yes, we are in witchspace
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main flight loop (Part 14 of 16)](../subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../subroutine/main_game_loop_part_2_of_6.md)
                            \   * [MJP](../subroutine/mjp.md)
                            \   * [TT151](../subroutine/tt151.md)
                            \   * [WARP](../subroutine/warp.md)
                            \   * [ypl](../subroutine/ypl.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .CABTMP
    
     SKIP 1                 \ Cabin temperature
                            \
                            \ The ambient cabin temperature in deep space is 30,
                            \ which is displayed as one notch on the dashboard bar
                            \
                            \ We get higher temperatures closer to the sun
                            \
                            \ CABTMP shares a location with MANY, but that's OK as
                            \ MANY+0 would contain the number of ships of type 0,
                            \ and as there is no ship type 0 (they start at 1), the
                            \ byte at MANY+0 is not used for storing a ship type
                            \ and can be used for the cabin temperature instead
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LAS2
    
     SKIP 1                 \ Laser power for the current laser
                            \
                            \   * Bits 0-6 contain the laser power of the current
                            \     space view
                            \
                            \   * Bit 7 denotes whether or not the laser pulses:
                            \
                            \     * 0 = pulsing laser
                            \
                            \     * 1 = beam laser (i.e. always on)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../subroutine/main_flight_loop_part_16_of_16.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .MSAR
    
     SKIP 1                 \ The targeting state of our leftmost missile
                            \
                            \   * 0 = missile is not looking for a target, or it
                            \     already has a target lock (indicator is not
                            \     yellow/white)
                            \
                            \   * Non-zero = missile is currently looking for a
                            \     target (indicator is yellow/white)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ABORT2](../subroutine/abort2.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../subroutine/main_flight_loop_part_11_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .VIEW
    
     SKIP 1                 \ The number of the current space view
                            \
                            \   * 0 = front
                            \   * 1 = rear
                            \   * 2 = left
                            \   * 3 = right
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ESCAPE](../subroutine/escape.md)
                            \   * [LOOK1](../subroutine/look1.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [PLUT](../subroutine/plut.md)
                            \   * [SIGHT](../subroutine/sight.md)
                            \   * [STARS](../subroutine/stars.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LASCT
    
     SKIP 1                 \ The laser pulse count for the current laser
                            \
                            \ This is a counter that defines the gap between the
                            \ pulses of a pulse laser. It is set as follows:
                            \
                            \   * 0 for a beam laser
                            \
                            \   * 10 for a pulse laser
                            \
                            \ It gets decremented by 2 on each iteration round the
                            \ main game loop and is set to a non-zero value for
                            \ pulse lasers only
                            \
                            \ The laser only fires when the value of LASCT hits
                            \ zero, so for pulse lasers with a value of 10, that
                            \ means the laser fires once every four iterations
                            \ round the main game loop (LASCT = 10, 6, 2, 0)
                            \
                            \ In comparison, beam lasers fire continuously as the
                            \ value of LASCT is always 0
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEATH](../subroutine/death.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../subroutine/main_flight_loop_part_16_of_16.md)
                            \   * [Main game loop (Part 5 of 6)](../subroutine/main_game_loop_part_5_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .GNTMP
    
     SKIP 1                 \ Laser temperature (or "gun temperature")
                            \
                            \ If the laser temperature exceeds 242 then the laser
                            \ overheats and cannot be fired again until it has
                            \ cooled down
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [LASLI](../subroutine/lasli.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main game loop (Part 5 of 6)](../subroutine/main_game_loop_part_5_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .HFX
    
     SKIP 1                 \ A flag that toggles the hyperspace colour effect
                            \
                            \   * 0 = no colour effect
                            \
                            \   * Non-zero = hyperspace colour effect enabled
                            \
                            \ When HFX is set to 1, the mode 1 screen that makes
                            \ up the top part of the display is temporarily switched
                            \ to mode 2 (the same screen mode as the dashboard),
                            \ which has the effect of blurring and colouring the
                            \ hyperspace rings in the top part of the screen. The
                            \ code to do this is in the LINSCN routine, which is
                            \ called as part of the screen mode routine at IRQ1.
                            \ It's in LINSCN that HFX is checked, and if it is
                            \ non-zero, the top part of the screen is not switched
                            \ to mode 1, thus leaving the top part of the screen in
                            \ the more colourful mode 2
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [IRQ1](../subroutine/irq1.md)
                            \   * [LL164](../subroutine/ll164.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .EV
    
     SKIP 1                 \ The "extra vessels" spawning counter
                            \
                            \ This counter is set to 0 on arrival in a system and
                            \ following an in-system jump, and is bumped up when we
                            \ spawn bounty hunters or pirates (i.e. "extra vessels")
                            \
                            \ It decreases by 1 each time we consider spawning more
                            \ "extra vessels" in part 4 of the main game loop, so
                            \ increasing the value of EV has the effect of delaying
                            \ the spawning of more vessels
                            \
                            \ In other words, this counter stops bounty hunters and
                            \ pirates from continually appearing, and ensures that
                            \ there's a delay between spawnings
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DLY
    
     SKIP 1                 \ In-flight message delay
                            \
                            \ This counter is used to keep an in-flight message up
                            \ for a specified time before it gets removed. The value
                            \ in DLY is decremented each time we start another
                            \ iteration of the main game loop at TT100
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CLYNS](../subroutine/clyns.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../subroutine/main_game_loop_part_2_of_6.md)
                            \   * [me1](../subroutine/me1.md)
                            \   * [me2](../subroutine/me2.md)
                            \   * [MESS](../subroutine/mess.md)
                            \   * [OUCH](../subroutine/ouch.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .de
    
     SKIP 1                 \ Equipment destruction flag
                            \
                            \   * Bit 1 denotes whether or not the in-flight message
                            \     about to be shown by the MESS routine is about
                            \     destroyed equipment:
                            \
                            \     * 0 = the message is shown normally
                            \
                            \     * 1 = the string " DESTROYED" gets added to the
                            \       end of the message
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CLYNS](../subroutine/clyns.md)
                            \   * [mes9](../subroutine/mes9.md)
                            \   * [MESS](../subroutine/mess.md)
                            \   * [OUCH](../subroutine/ouch.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \   * [ZERO](../subroutine/zero.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSX2
    
     SKIP 256               \ The ball line heap for storing x-coordinates
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [CIRCLE](../subroutine/circle.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [WP1](../subroutine/wp1.md)
                            \   * [WPLS2](../subroutine/wpls2.md)
                            \   * [WPSHPS](../subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSY2
    
     SKIP 256               \ The ball line heap for storing y-coordinates
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [WPLS2](../subroutine/wpls2.md)
                            \   * [WPSHPS](../subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSO
    
     SKIP 200               \ The ship line heap for the space station (see NWSPS)
                            \ and the sun line heap (see SUN)
                            \
                            \ The spaces can be shared as our local bubble of
                            \ universe can support either the sun or a space
                            \ station, but not both
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EDGES](../subroutine/edges.md)
                            \   * [FLFLLS](../subroutine/flflls.md)
                            \   * [HLOIN2](../subroutine/hloin2.md)
                            \   * [NWSPS](../subroutine/nwsps.md)
                            \   * [SUN (Part 2 of 4)](../subroutine/sun_part_2_of_4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [SUN (Part 4 of 4)](../subroutine/sun_part_4_of_4.md)
                            \   * [WPLS](../subroutine/wpls.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BUF
    
     SKIP 90                \ The line buffer used by DASC to print justified text
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HME2](../subroutine/hme2.md)
                            \   * [MT17](../subroutine/mt17.md)
                            \   * [TT26](../subroutine/tt26.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SX
    
     SKIP NOST + 1          \ This is where we store the x_hi coordinates for all
                            \ the stardust particles
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FLIP](../subroutine/flip.md)
                            \   * [nWq](../subroutine/nwq.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SXL
    
     SKIP NOST + 1          \ This is where we store the x_lo coordinates for all
                            \ the stardust particles
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SY
    
     SKIP NOST + 1          \ This is where we store the y_hi coordinates for all
                            \ the stardust particles
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FLIP](../subroutine/flip.md)
                            \   * [MLU1](../subroutine/mlu1.md)
                            \   * [nWq](../subroutine/nwq.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SYL
    
     SKIP NOST + 1          \ This is where we store the y_lo coordinates for all
                            \ the stardust particles
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [PIX1](../subroutine/pix1.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SZ
    
     SKIP NOST + 1          \ This is where we store the z_hi coordinates for all
                            \ the stardust particles
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DV42](../subroutine/dv42.md)
                            \   * [FLIP](../subroutine/flip.md)
                            \   * [nWq](../subroutine/nwq.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SZL
    
     SKIP NOST + 1          \ This is where we store the z_lo coordinates for all
                            \ the stardust particles
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LASX
    
     SKIP 1                 \ The x-coordinate of the tip of the laser line
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LASLI](../subroutine/lasli.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LASY
    
     SKIP 1                 \ The y-coordinate of the tip of the laser line
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LASLI](../subroutine/lasli.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX24
    
     SKIP 1                 \ This byte appears to be unused
    
    .ALTIT
    
     SKIP 1                 \ Our altitude above the surface of the planet or sun
                            \
                            \   * 255 = we are a long way above the surface
                            \
                            \   * 1-254 = our altitude as the square root of:
                            \
                            \       x_hi^2 + y_hi^2 + z_hi^2 - 6^2
                            \
                            \     where our ship is at the origin, the centre of the
                            \     planet/sun is at (x_hi, y_hi, z_hi), and the
                            \     radius of the planet/sun is 6
                            \
                            \   * 0 = we have crashed into the surface
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SWAP
    
     SKIP 1                 \ Temporary storage, used to store a flag that records
                            \ whether or not we had to swap a line's start and end
                            \ coordinates around when clipping the line in routine
                            \ LL145 (the flag is used in places like BLINE to swap
                            \ them back)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [LL145 (Part 1 of 4)](../subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../subroutine/ll145_part_4_of_4.md)
                            \   * [LOINQ (Part 1 of 7)](../subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../subroutine/loinq_part_7_of_7.md)
                            \   * [WPLS2](../subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XP
    
     SKIP 1                 \ This byte appears to be unused
    
    .YP
    
     SKIP 1                 \ This byte appears to be unused
    
    .YS
    
     SKIP 1                 \ This byte appears to be unused
    
    .BALI
    
     SKIP 1                 \ This byte appears to be unused
    
    .UPO
    
     SKIP 1                 \ This byte appears to be unused
    
    .boxsize
    
     SKIP 1                 \ This byte appears to be unused
    
    .distaway
    
     SKIP 1                 \ Used to store the nearest distance of the rotating
                            \ ship on the title screen
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TITLE](../subroutine/title.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XSAV2
    
     SKIP 1                 \ This byte appears to be unused
    
    .YSAV2
    
     SKIP 1                 \ This byte appears to be unused
    
    IF _COMPACT
    
    .NMI
    
     SKIP 1                 \ Used to store the ID of the current owner of the NMI
                            \ workspace in the NMICLAIM routine, so we can hand it
                            \ back to them in the NMIRELEASE routine once we are
                            \ done using it
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NMICLAIM](../subroutine/nmiclaim.md)
                            \   * [NMIRELEASE](../subroutine/nmirelease.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    ENDIF
    
    .NAME
    
     SKIP 8                 \ The current commander name
                            \
                            \ The commander name can be up to 7 characters (the DFS
                            \ limit for filenames), and is terminated by a carriage
                            \ return
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [cmn](../subroutine/cmn.md)
                            \   * [DFAULT](../subroutine/dfault.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .TP
    
     SKIP 1                 \ The current mission status
                            \
                            \   * Bits 0-1 = Mission 1 status
                            \
                            \     * %00 = Mission not started
                            \     * %01 = Mission in progress, hunting for ship
                            \     * %11 = Constrictor killed, not debriefed yet
                            \     * %10 = Mission and debrief complete
                            \
                            \   * Bits 2-3 = Mission 2 status
                            \
                            \     * %00 = Mission not started
                            \     * %01 = Mission in progress, plans not picked up
                            \     * %10 = Mission in progress, plans picked up
                            \     * %11 = Mission complete
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BRIEF](../subroutine/brief.md)
                            \   * [BRIEF2](../subroutine/brief2.md)
                            \   * [BRIEF3](../subroutine/brief3.md)
                            \   * [DEBRIEF](../subroutine/debrief.md)
                            \   * [DEBRIEF2](../subroutine/debrief2.md)
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \   * [PDESC](../subroutine/pdesc.md)
                            \   * [SVE](../subroutine/sve.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ0
    
     SKIP 1                 \ The current system's galactic x-coordinate (0-256)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [jmp](../subroutine/jmp.md)
                            \   * [ping](../subroutine/ping.md)
                            \   * [THERE](../subroutine/there.md)
                            \   * [TT105](../subroutine/tt105.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT14](../subroutine/tt14.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ1
    
     SKIP 1                 \ The current system's galactic y-coordinate (0-256)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [jmp](../subroutine/jmp.md)
                            \   * [MJP](../subroutine/mjp.md)
                            \   * [THERE](../subroutine/there.md)
                            \   * [TT105](../subroutine/tt105.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT14](../subroutine/tt14.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ21
    
     SKIP 6                 \ The three 16-bit seeds for the current galaxy
                            \
                            \ These seeds define system 0 in the current galaxy, so
                            \ they can be used as a starting point to generate all
                            \ 256 systems in the galaxy
                            \
                            \ Using a galactic hyperdrive rotates each byte to the
                            \ left (rolling each byte within itself) to get the
                            \ seeds for the next galaxy, so after eight galactic
                            \ jumps, the seeds roll around to the first galaxy again
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Ghy](../subroutine/ghy.md)
                            \   * [TT81](../subroutine/tt81.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .CASH
    
     SKIP 4                 \ Our current cash pot
                            \
                            \ The cash stash is stored as a 32-bit unsigned integer,
                            \ with the most significant byte in CASH and the least
                            \ significant in CASH+3. This is big-endian, which is
                            \ the opposite way round to most of the numbers used in
                            \ Elite - to use our notation for multi-byte numbers,
                            \ the amount of cash is CASH(0 1 2 3)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [csh](../subroutine/csh.md)
                            \   * [LCASH](../subroutine/lcash.md)
                            \   * [MCASH](../subroutine/mcash.md)
                            \   * [SVE](../subroutine/sve.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ14
    
     SKIP 1                 \ Our current fuel level (0-70)
                            \
                            \ The fuel level is stored as the number of light years
                            \ multiplied by 10, so QQ14 = 1 represents 0.1 light
                            \ years, and the maximum possible value is 70, for 7.0
                            \ light years
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [ESCAPE](../subroutine/escape.md)
                            \   * [fwl](../subroutine/fwl.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [TT14](../subroutine/tt14.md)
                            \   * [TT18](../subroutine/tt18.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .COK
    
     SKIP 1                 \ Flags used to generate the competition code
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DFAULT](../subroutine/dfault.md)
                            \   * [MJP](../subroutine/mjp.md)
                            \   * [SVE](../subroutine/sve.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .GCNT
    
     SKIP 1                 \ The number of the current galaxy (0-7)
                            \
                            \ When this is displayed in-game, 1 is added to the
                            \ number, so we start in galaxy 1 in-game, but it's
                            \ stored as galaxy 0 internally
                            \
                            \ The galaxy number increases by one every time a
                            \ galactic hyperdrive is used, and wraps back around to
                            \ the start after eight galaxies
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [Ghy](../subroutine/ghy.md)
                            \   * [MT28](../subroutine/mt28.md)
                            \   * [PDESC](../subroutine/pdesc.md)
                            \   * [tal](../subroutine/tal.md)
                            \   * [THERE](../subroutine/there.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LASER
    
     SKIP 4                 \ The specifications of the lasers fitted to each of the
                            \ four space views:
                            \
                            \   * Byte #0 = front view
                            \   * Byte #1 = rear view
                            \   * Byte #2 = left view
                            \   * Byte #3 = right view
                            \
                            \ For each of the views:
                            \
                            \   * 0 = no laser is fitted to this view
                            \
                            \   * Non-zero = a laser is fitted to this view, with
                            \     the following specification:
                            \
                            \     * Bits 0-6 contain the laser's power
                            \
                            \     * Bit 7 determines whether or not the laser pulses
                            \       (0 = pulse or mining laser) or is always on
                            \       (1 = beam or military laser)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [refund](../subroutine/refund.md)
                            \   * [SIGHT](../subroutine/sight.md)
                            \   * [STATUS](../subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SKIP 2                 \ These bytes appear to be unused (they were originally
                            \ used for up/down lasers, but they were dropped)
    
    .CRGO
    
     SKIP 1                 \ Our ship's cargo capacity
                            \
                            \   * 22 = standard cargo bay of 20 tonnes
                            \
                            \   * 37 = large cargo bay of 35 tonnes
                            \
                            \ The value is two greater than the actual capacity to
                            \ make the maths in tnpr slightly more efficient
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [tnpr](../subroutine/tnpr.md)
                            \   * [TT213](../subroutine/tt213.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ20
    
     SKIP 17                \ The contents of our cargo hold
                            \
                            \ The amount of market item X that we have in our hold
                            \ can be found in the X-th byte of QQ20. For example:
                            \
                            \   * QQ20 contains the amount of food (item 0)
                            \
                            \   * QQ20+7 contains the amount of computers (item 7)
                            \
                            \ See QQ23 for a list of market item numbers and their
                            \ storage units
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BAD](../subroutine/bad.md)
                            \   * [ESCAPE](../subroutine/escape.md)
                            \   * [Main flight loop (Part 8 of 16)](../subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [OUCH](../subroutine/ouch.md)
                            \   * [SOLAR](../subroutine/solar.md)
                            \   * [tnpr](../subroutine/tnpr.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ECM
    
     SKIP 1                 \ E.C.M. system
                            \
                            \   * 0 = not fitted
                            \
                            \   * &FF = fitted
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [STATUS](../subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BST
    
     SKIP 1                 \ Fuel scoops (BST stands for "barrel status")
                            \
                            \   * 0 = not fitted
                            \
                            \   * &FF = fitted
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [Main flight loop (Part 7 of 16)](../subroutine/main_flight_loop_part_7_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [STATUS](../subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BOMB
    
     SKIP 1                 \ Energy bomb
                            \
                            \   * 0 = not fitted
                            \
                            \   * &7F = fitted
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [LOOK1](../subroutine/look1.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 5 of 16)](../subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [Main flight loop (Part 13 of 16)](../subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [STATUS](../subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ENGY
    
     SKIP 1                 \ Energy unit
                            \
                            \   * 0 = not fitted
                            \
                            \   * Non-zero = fitted
                            \
                            \ The actual value determines the refresh rate of our
                            \ energy banks, as they refresh by ENGY+1 each time (so
                            \ our ship's energy level goes up by 2 each time if we
                            \ have an energy unit fitted, otherwise it goes up by 1)
                            \
                            \ The enhanced versions of Elite set ENGY to 2 as the
                            \ reward for completing mission 2, where we receive a
                            \ special naval energy unit that recharges at a fast
                            \ rate than a standard energy unit, i.e. by 3 each time
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEBRIEF2](../subroutine/debrief2.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [Main flight loop (Part 13 of 16)](../subroutine/main_flight_loop_part_13_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DKCMP
    
     SKIP 1                 \ Docking computer
                            \
                            \   * 0 = not fitted
                            \
                            \   * &FF = fitted
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .GHYP
    
     SKIP 1                 \ Galactic hyperdrive
                            \
                            \   * 0 = not fitted
                            \
                            \   * &FF = fitted
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [Ghy](../subroutine/ghy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ESCP
    
     SKIP 1                 \ Escape pod
                            \
                            \   * 0 = not fitted
                            \
                            \   * &FF = fitted
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [ESCAPE](../subroutine/escape.md)
                            \   * [IRQ1](../subroutine/irq1.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [STATUS](../subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SKIP 1                 \ This byte appears to be unused
    
    .TRIBBLE
    
     SKIP 2                 \ The number of Trumbles in the cargo hold
                            \
                            \ The Master version doesn't actually have Trumbles, but
                            \ the Trumble code from the other versions was kept when
                            \ the Master version was put together
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [SOLAR](../subroutine/solar.md)
                            \   * [tnpr](../subroutine/tnpr.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .TALLYL
    
     SKIP 1                 \ Combat rank fraction
                            \
                            \ Contains the fraction part of the kill count, which
                            \ together with the integer in TALLY(1 0) determines our
                            \ combat rank. The fraction is stored as the numerator
                            \ of a fraction with a denominator of 256, so a TALLYL
                            \ of 128 would represent 0.5 (i.e. 128 / 256)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EXNO2](../subroutine/exno2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .NOMSL
    
     SKIP 1                 \ The number of missiles we have fitted (0-4)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ABORT2](../subroutine/abort2.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [FRMIS](../subroutine/frmis.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [msblob](../subroutine/msblob.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .FIST
    
     SKIP 1                 \ Our legal status (FIST stands for "fugitive/innocent
                            \ status"):
                            \
                            \   * 0 = Clean
                            \
                            \   * 1-49 = Offender
                            \
                            \   * 50+ = Fugitive
                            \
                            \ You get 64 points if you kill a cop, so that's a fast
                            \ ticket to fugitive status
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ESCAPE](../subroutine/escape.md)
                            \   * [Ghy](../subroutine/ghy.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main game loop (Part 3 of 6)](../subroutine/main_game_loop_part_3_of_6.md)
                            \   * [SOLAR](../subroutine/solar.md)
                            \   * [STATUS](../subroutine/status.md)
                            \   * [TACTICS (Part 3 of 7)](../subroutine/tactics_part_3_of_7.md)
                            \   * [TT110](../subroutine/tt110.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .AVL
    
     SKIP 17                \ Market availability in the current system
                            \
                            \ The available amount of market item X is stored in
                            \ the X-th byte of AVL, so for example:
                            \
                            \   * AVL contains the amount of food (item 0)
                            \
                            \   * AVL+7 contains the amount of computers (item 7)
                            \
                            \ See QQ23 for a list of market item numbers and their
                            \ storage units
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [GVL](../subroutine/gvl.md)
                            \   * [TT151](../subroutine/tt151.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \   * [var](../subroutine/var.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ26
    
     SKIP 1                 \ A random value used to randomise market data
                            \
                            \ This value is set to a new random number for each
                            \ change of system, so we can add a random factor into
                            \ the calculations for market prices
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [GVL](../subroutine/gvl.md)
                            \   * [TT151](../subroutine/tt151.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .TALLY
    
     SKIP 2                 \ Our combat rank
                            \
                            \ The combat rank is stored as the number of kills, in a
                            \ 16-bit number TALLY(1 0) - so the high byte is in
                            \ TALLY+1 and the low byte in TALLY
                            \
                            \ There is also a fractional part of the kill count,
                            \ which is stored in TALLYL
                            \
                            \ If the high byte in TALLY+1 is 0 then we have between
                            \ 0 and 255 kills, so our rank is Harmless, Mostly
                            \ Harmless, Poor, Average Above Average or Competent,
                            \ according to the value of the low byte in TALLY:
                            \
                            \   Harmless         %00000000 to %00000111 = 0 to 7
                            \   Mostly Harmless  %00001000 to %00001111 = 8 to 15
                            \   Poor             %00010000 to %00011111 = 16 to 31
                            \   Average          %00100000 to %00111111 = 32 to 63
                            \   Above Average    %01000000 to %01111111 = 64 to 127
                            \   Competent        %10000000 to %11111111 = 128 to 255
                            \
                            \ Note that the Competent range also covers kill counts
                            \ from 256 to 511, as follows
                            \
                            \ If the high byte in TALLY+1 is non-zero then we are
                            \ Competent, Dangerous, Deadly or Elite, according to
                            \ the value of TALLY(1 0):
                            \
                            \   Competent   (1 0) to (1 255)   = 256 to 511 kills
                            \   Dangerous   (2 0) to (9 255)   = 512 to 2559 kills
                            \   Deadly      (10 0) to (24 255) = 2560 to 6399 kills
                            \   Elite       (25 0) and up      = 6400 kills and up
                            \
                            \ You can see the rating calculation in the STATUS
                            \ subroutine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEBRIEF2](../subroutine/debrief2.md)
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [EXNO2](../subroutine/exno2.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [STATUS](../subroutine/status.md)
                            \   * [SVE](../subroutine/sve.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SVC
    
     SKIP 1                 \ The save count
                            \
                            \ When a new commander is created, the save count gets
                            \ set to 128. This value gets halved each time the
                            \ commander file is saved, but it is otherwise unused.
                            \ It is presumably part of the security system for the
                            \ competition, possibly another flag to catch out
                            \ entries with manually altered commander files
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [SVE](../subroutine/sve.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SKIP 2                 \ The commander file checksum
                            \
                            \ These two bytes are reserved for the commander file
                            \ checksum, so when the current commander block is
                            \ copied from here to the last saved commander block at
                            \ NA%, CHK and CHK2 get overwritten
    
     NT% = SVC + 3 - TP     \ This sets the variable NT% to the size of the current
                            \ commander data block, which starts at TP and ends at
                            \ SVC+3 (inclusive), i.e. with the last checksum byte
    
     SKIP 1                 \ This byte appears to be unused
    
    .MCH
    
     SKIP 1                 \ The text token number of the in-flight message that is
                            \ currently being shown, and which will be removed by
                            \ the me2 routine when the counter in DLY reaches zero
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [me1](../subroutine/me1.md)
                            \   * [me2](../subroutine/me2.md)
                            \   * [MESS](../subroutine/mess.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .COMX
    
     SKIP 1                 \ The x-coordinate of the compass dot
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOT](../subroutine/dot.md)
                            \   * [SP2](../subroutine/sp2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .COMY
    
     SKIP 1                 \ The y-coordinate of the compass dot
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOT](../subroutine/dot.md)
                            \   * [SP2](../subroutine/sp2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .dialc
    
     SKIP 14                \ These bytes are unused in this version of Elite
                            \
                            \ They are left over from the Apple II version, where
                            \ they are used to store the current colour of the
                            \ dashboard indicators
    
    .QQ24
    
     SKIP 1                 \ Temporary storage, used to store the current market
                            \ item's price in routine TT151
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT151](../subroutine/tt151.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ25
    
     SKIP 1                 \ Temporary storage, used to store the current market
                            \ item's availability in routine TT151
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [gnum](../subroutine/gnum.md)
                            \   * [TT151](../subroutine/tt151.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ28
    
     SKIP 1                 \ The current system's economy (0-7)
                            \
                            \   * 0 = Rich Industrial
                            \   * 1 = Average Industrial
                            \   * 2 = Poor Industrial
                            \   * 3 = Mainly Industrial
                            \   * 4 = Mainly Agricultural
                            \   * 5 = Rich Agricultural
                            \   * 6 = Average Agricultural
                            \   * 7 = Poor Agricultural
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \   * [var](../subroutine/var.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ29
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 8 of 16)](../subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [NWDAV4](../subroutine/nwdav4.md)
                            \   * [tnpr](../subroutine/tnpr.md)
                            \   * [tnpr1](../subroutine/tnpr1.md)
                            \   * [TT167](../subroutine/tt167.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .gov
    
     SKIP 1                 \ The current system's government type (0-7)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .tek
    
     SKIP 1                 \ The current system's tech level (0-14)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \   * [NWSPS](../subroutine/nwsps.md)
                            \   * [qv](../subroutine/qv.md)
                            \   * [SOS1](../subroutine/sos1.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SLSP
    
     SKIP 2                 \ The address of the bottom of the ship line heap
                            \
                            \ The ship line heap is a descending block of memory
                            \ extended downwards by the NWSHP routine when adding
                            \ new ships (and their associated ship line heaps), in
                            \ which case SLSP is lowered to provide more heap space,
                            \ assuming there is enough free memory to do so
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [KS3](../subroutine/ks3.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [RES2](../subroutine/res2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ2
    
     SKIP 6                 \ The three 16-bit seeds for the current system, i.e.
                            \ the one we are currently in
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \   * [ypl](../subroutine/ypl.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .safehouse
    
     SKIP 6                 \ Backup storage for the seeds for the selected system
                            \
                            \ The seeds for the current system get stored here as
                            \ soon as a hyperspace is initiated, so we can fetch
                            \ them in the hyp1 routine. This fixes a bug in an
                            \ earlier version where you could hyperspace while
                            \ docking and magically appear in your destination
                            \ station
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Ghy](../subroutine/ghy.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .frump
    
     SKIP 1                 \ Used to store the number of particles in the explosion
                            \ cloud, though the number is never actually used
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../subroutine/doexp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JOPOS
    
     SKIP 1                 \ Contains the high byte of the latest value from ADC
                            \ channel 1 (the joystick X value), which gets updated
                            \ regularly by the IRQ1 interrupt handler
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [IRQ1](../subroutine/irq1.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SKIP 1                 \ Contains the high byte of the latest value from ADC
                            \ channel 2 (the joystick Y value), which gets updated
                            \ regularly by the IRQ1 interrupt handler
    
     SKIP 1                 \ Contains the high byte of the latest value from ADC
                            \ channel 3 (the Bitstik rotation value), which gets
                            \ updated regularly by the IRQ1 interrupt handler
    
     PRINT "WP workspace from ", ~WP, "to ", ~P%-1, "inclusive"
    

[X]

Configuration variable [NOSH](../../all/workspaces.md#nosh) = 12

The maximum number of ships in our local bubble of universe

[X]

Configuration variable [NOST](../../all/workspaces.md#nost) = 20

The number of stardust particles in normal space (this goes down to 3 in witchspace)

[X]

Configuration variable [NTY](../../all/workspaces.md#nty) = 33

The number of different ship types

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[K%](k_per_cent.md "Previous routine")[TVT3](../variable/tvt3.md "Next routine")
