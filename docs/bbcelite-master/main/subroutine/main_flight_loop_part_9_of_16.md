---
title: "The Main flight loop (Part 9 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_9_of_16.html"
---

[Main flight loop (Part 8 of 16)](main_flight_loop_part_8_of_16.md "Previous routine")[Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 9 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: If it is a space station, check whether we
                 are successfully docking with it
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Docking checks](https://elite.bbcelite.com/deep_dives/docking_checks.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-9-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_9_of_16.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ESCAPE](escape.md) calls via GOIN
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Process docking with a space station
    
    
    
    * * *
    
    
     Other entry points:
    
       GOIN                 We jump here from part 3 of the main flight loop if the
                            docking computer is activated by pressing "C"
    
    
    
    
    .ISDK
    
     LDA K%+NI%+36          \ 1. Fetch the NEWB flags (byte #36) of the second ship
     AND #%00000100         \ in the ship data workspace at K%, which is reserved
     BNE MA62               \ for the sun or the space station (in this case it's
                            \ the latter), and if bit 2 is set, meaning the station
                            \ is hostile, jump down to MA62 to fail docking (so
                            \ trying to dock at a station that we have annoyed does
                            \ not end well)
    
     LDA INWK+14            \ 2. If nosev_z_hi < 214, jump down to MA62 to fail
     CMP #214               \ docking, as the angle of approach is greater than 26
     BCC MA62               \ degrees
    
     JSR SPS1               \ Call SPS1 to calculate the vector to the planet and
                            \ store it in XX15
    
     LDA XX15+2             \ Set A to the z-axis of the vector
    
                            \ This version of Elite omits check 3 (which would check
                            \ the sign of the z-axis)
    
     CMP #89                \ 4. If z-axis < 89, jump to MA62 to fail docking, as
     BCC MA62               \ we are not in the 22.0 degree safe cone of approach
    
     LDA INWK+16            \ 5. If |roofv_x_hi| < 80, jump to MA62 to fail docking,
     AND #%01111111         \ as the slot is more than 36.6 degrees from horizontal
     CMP #80
     BCC MA62
    
    .GOIN
    
                            \ If we arrive here, we just docked successfully
    
    \JSR stopbd             \ This instruction is commented out in the original
                            \ source
    
     JMP DOENTRY            \ Go to the docking bay (i.e. show the ship hangar)
    
    .MA62
    
                            \ If we arrive here, docking has just failed
    
     LDA DELTA              \ If the ship's speed is < 5, jump to MA67 to register
     CMP #5                 \ some damage, but not a huge amount
     BCC MA67
    
     JMP DEATH              \ Otherwise we have just crashed into the station, so
                            \ process our death
    

[X]

Subroutine [DEATH](death.md) (category: Start and end)

Display the death screen

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Subroutine [DOENTRY](doentry.md) (category: Flight)

Dock at the space station, show the ship hangar and work out any mission progression

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Label [MA62](main_flight_loop_part_9_of_16.md#ma62) is local to this routine

[X]

Label [MA67](main_flight_loop_part_10_of_16.md#ma67) in subroutine [Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md)

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Subroutine [SPS1](sps1.md) (category: Maths (Geometry))

Calculate the vector to the planet and store it in XX15

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[Main flight loop (Part 8 of 16)](main_flight_loop_part_8_of_16.md "Previous routine")[Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md "Next routine")
