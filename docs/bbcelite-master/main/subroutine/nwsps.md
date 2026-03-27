---
title: "The NWSPS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nwsps.html"
---

[SPS3](sps3.md "Previous routine")[NWSHP](nwshp.md "Next routine")
    
    
           Name: NWSPS                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Add a new space station to our local bubble of universe
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-nwsps)
     Variations: See [code variations](../../related/compare/main/subroutine/nwsps.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md) calls NWSPS
                 * [TT110](tt110.md) calls NWSPS
    
    
    
    
    
    
    .NWSPS
    
     JSR SPBLB              \ Light up the space station bulb on the dashboard
    
     LDX #%10000001         \ Set the AI flag in byte #32 to %10000001 (AI enabled,
     STX INWK+32            \ has an E.C.M.)
    
     LDX #0                 \ Set pitch counter to 0 (no pitch, roll only)
     STX INWK+30
    
     STX NEWB               \ Set NEWB to %00000000, though this gets overridden by
                            \ the default flags from E% in NWSHP below
    
    \STX INWK+31            \ This instruction is commented out in the original
                            \ source. It would set the exploding state and missile
                            \ count to 0
    
     STX FRIN+1             \ Set the second slot in the FRIN table to 0, so when we
                            \ fall through into NWSHP below, the new station that
                            \ gets created will go into slot FRIN+1, as this will be
                            \ the first empty slot that the routine finds
    
     DEX                    \ Set the roll counter to 255 (maximum anti-clockwise
     STX INWK+29            \ roll with no damping)
    
     LDX #10                \ Call NwS1 to flip the sign of nosev_x_hi (byte #10)
     JSR NwS1
    
     JSR NwS1               \ And again to flip the sign of nosev_y_hi (byte #12)
    
     JSR NwS1               \ And again to flip the sign of nosev_z_hi (byte #14)
    
     LDA spasto             \ Copy the address of the Coriolis space station's ship
     STA XX21+2*SST-2       \ blueprint from spasto to the #SST entry in the
     LDA spasto+1           \ blueprint lookup table at XX21, so when we spawn a
     STA XX21+2*SST-1       \ ship of type #SST, it will be a Coriolis station
    
     LDA tek                \ If the system's tech level in tek is less than 10,
     CMP #10                \ jump to notadodo, so tech levels 0 to 9 have Coriolis
     BCC notadodo           \ stations, while 10 and above will have Dodo stations
    
     LDA XX21+2*DOD-2       \ Copy the address of the Dodo space station's ship
     STA XX21+2*SST-2       \ blueprint from spasto to the #SST entry in the
     LDA XX21+2*DOD-1       \ blueprint lookup table at XX21, so when we spawn a
     STA XX21+2*SST-1       \ ship of type #SST, it will be a Dodo station
    
    .notadodo
    
     LDA #LO(LSO)           \ Set bytes #33 and #34 to point to LSO for the ship
     STA INWK+33            \ line heap for the space station
     LDA #HI(LSO)
     STA INWK+34
    
     LDA #SST               \ Set A to the space station type, and fall through
                            \ into NWSHP to finish adding the space station to the
                            \ universe
    

[X]

Configuration variable [DOD](../../all/workspaces.md#dod) = 33

Ship type for a Dodecahedron ("Dodo") space station

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [LSO](../workspace/wp.md#lso) in workspace [WP](../workspace/wp.md)

The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Subroutine [NwS1](nws1.md) (category: Universe)

Flip the sign and double an INWK byte

[X]

Subroutine [SPBLB](spblb.md) (category: Dashboard)

Light up the space station indicator ("S") on the dashboard

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[X]

Configuration variable [XX21](../../all/workspaces.md#xx21) = &8000

The address of the ship blueprints lookup table, as set in elite-data.asm

[X]

Label [notadodo](nwsps.md#notadodo) is local to this routine

[X]

Variable [spasto](../variable/spasto.md) (category: Universe)

Contains the address of the Coriolis space station's ship blueprint

[X]

Variable [tek](../workspace/wp.md#tek) in workspace [WP](../workspace/wp.md)

The current system's tech level (0-14)

[SPS3](sps3.md "Previous routine")[NWSHP](nwshp.md "Next routine")
