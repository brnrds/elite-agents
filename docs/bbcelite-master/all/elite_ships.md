---
title: "Ship blueprints in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_ships.html"
---

[Game data](elite_data.md "Previous routine")[Text tokens](text_tokens.md "Next routine")
    
    
     ELITE SHIP BLUEPRINTS FILE
    
    
    
    
     SKIP 256               \ These bytes appear to be unused, but they get moved to
                            \ &7F00-&7FFF along with the ship blueprints and text
                            \ tokens
    
    
    
    
           Name: XX21                                                    [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprints lookup table
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/xx21.md)
     Variations: See [code variations](../related/compare/main/variable/xx21.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [BEGIN](../main/subroutine/begin.md) uses XX21
                 * [DEATH](../main/subroutine/death.md) uses XX21
                 * [DEEOR](../main/subroutine/deeor.md) uses XX21
                 * [HAS1](../main/subroutine/has1.md) uses XX21
                 * [KILLSHP](../main/subroutine/killshp.md) uses XX21
                 * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md) uses XX21
                 * [NWSHP](../main/subroutine/nwshp.md) uses XX21
                 * [NWSPS](../main/subroutine/nwsps.md) uses XX21
    
    
    
    
    
    
    .XX21
    
     EQUW SHIP_MISSILE      \ MSL  =  1 = Missile
     EQUW SHIP_CORIOLIS     \ SST  =  2 = Coriolis space station
     EQUW SHIP_ESCAPE_POD   \ ESC  =  3 = Escape pod
     EQUW SHIP_PLATE        \ PLT  =  4 = Alloy plate
     EQUW SHIP_CANISTER     \ OIL  =  5 = Cargo canister
     EQUW SHIP_BOULDER      \         6 = Boulder
     EQUW SHIP_ASTEROID     \ AST  =  7 = Asteroid
     EQUW SHIP_SPLINTER     \ SPL  =  8 = Splinter
     EQUW SHIP_SHUTTLE      \ SHU  =  9 = Shuttle
     EQUW SHIP_TRANSPORTER  \        10 = Transporter
     EQUW SHIP_COBRA_MK_3   \ CYL  = 11 = Cobra Mk III
     EQUW SHIP_PYTHON       \        12 = Python
     EQUW SHIP_BOA          \        13 = Boa
     EQUW SHIP_ANACONDA     \ ANA  = 14 = Anaconda
     EQUW SHIP_ROCK_HERMIT  \ HER  = 15 = Rock hermit (asteroid)
     EQUW SHIP_VIPER        \ COPS = 16 = Viper
     EQUW SHIP_SIDEWINDER   \ SH3  = 17 = Sidewinder
     EQUW SHIP_MAMBA        \        18 = Mamba
     EQUW SHIP_KRAIT        \ KRA  = 19 = Krait
     EQUW SHIP_ADDER        \ ADA  = 20 = Adder
     EQUW SHIP_GECKO        \        21 = Gecko
     EQUW SHIP_COBRA_MK_1   \        22 = Cobra Mk I
     EQUW SHIP_WORM         \ WRM  = 23 = Worm
     EQUW SHIP_COBRA_MK_3_P \ CYL2 = 24 = Cobra Mk III (pirate)
     EQUW SHIP_ASP_MK_2     \ ASP  = 25 = Asp Mk II
     EQUW SHIP_PYTHON_P     \        26 = Python (pirate)
     EQUW SHIP_FER_DE_LANCE \        27 = Fer-de-lance
     EQUW SHIP_MORAY        \        28 = Moray
     EQUW SHIP_THARGOID     \ THG  = 29 = Thargoid
     EQUW SHIP_THARGON      \ TGL  = 30 = Thargon
     EQUW SHIP_CONSTRICTOR  \ CON  = 31 = Constrictor
     EQUW SHIP_COUGAR       \ COU  = 32 = Cougar
     EQUW SHIP_DODO         \ DOD  = 33 = Dodecahedron ("Dodo") space station
    
    
    
    
           Name: E%                                                      [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprints default NEWB flags
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Advanced tactics with the NEWB flags](https://elite.bbcelite.com/deep_dives/advanced_tactics_with_the_newb_flags.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/e_per_cent.md)
     Variations: See [code variations](../related/compare/main/variable/e_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [NWSHP](../main/subroutine/nwshp.md) uses E%
                 * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md) uses E%
    
    
    
    
    
    * * *
    
    
     When spawning a new ship, the bits from this table are applied to the new
     ship's NEWB flags in byte #36 (i.e. a set bit in this table will set that bit
     in the NEWB flags). In other words, if a ship blueprint is set to one of the
     following, then all spawned ships of that type will be too: trader, bounty
     hunter, hostile, pirate, innocent, cop.
    
     The NEWB flags are as follows:
    
       * Bit 0: Trader flag (0 = not a trader, 1 = trader)
       * Bit 1: Bounty hunter flag (0 = not a bounty hunter, 1 = bounty hunter)
       * Bit 2: Hostile flag (0 = not hostile, 1 = hostile)
       * Bit 3: Pirate flag (0 = not a pirate, 1 = pirate)
       * Bit 4: Docking flag (0 = not docking, 1 = docking)
       * Bit 5: Innocent bystander (0 = normal, 1 = innocent bystander)
       * Bit 6: Cop flag (0 = not a cop, 1 = cop)
       * Bit 7: For spawned ships: ship been scooped or has docked
                For blueprints: this ship type has an escape pod fitted
    
    
    
    
    .E%
    
     EQUB %00000000         \ Missile
     EQUB %00000000         \ Coriolis space station
     EQUB %00000001         \ Escape pod                                      Trader
     EQUB %00000000         \ Alloy plate
     EQUB %00000000         \ Cargo canister
     EQUB %00000000         \ Boulder
     EQUB %00000000         \ Asteroid
     EQUB %00000000         \ Splinter
     EQUB %00100001         \ Shuttle                               Trader, innocent
     EQUB %01100001         \ Transporter                      Trader, innocent, cop
     EQUB %10100000         \ Cobra Mk III                      Innocent, escape pod
     EQUB %10100000         \ Python                            Innocent, escape pod
     EQUB %10100000         \ Boa                               Innocent, escape pod
     EQUB %10100001         \ Anaconda                  Trader, innocent, escape pod
     EQUB %10100001         \ Rock hermit (asteroid)    Trader, innocent, escape pod
     EQUB %11000010         \ Viper                   Bounty hunter, cop, escape pod
     EQUB %00001100         \ Sidewinder                             Hostile, pirate
     EQUB %10001100         \ Mamba                      Hostile, pirate, escape pod
     EQUB %10001100         \ Krait                      Hostile, pirate, escape pod
     EQUB %10001100         \ Adder                      Hostile, pirate, escape pod
     EQUB %00001100         \ Gecko                                  Hostile, pirate
     EQUB %10001100         \ Cobra Mk I                 Hostile, pirate, escape pod
     EQUB %00000101         \ Worm                                   Hostile, trader
     EQUB %10001100         \ Cobra Mk III (pirate)      Hostile, pirate, escape pod
     EQUB %10001100         \ Asp Mk II                  Hostile, pirate, escape pod
     EQUB %10001100         \ Python (pirate)            Hostile, pirate, escape pod
     EQUB %10000010         \ Fer-de-lance                 Bounty hunter, escape pod
     EQUB %00001100         \ Moray                                  Hostile, pirate
     EQUB %00001100         \ Thargoid                               Hostile, pirate
     EQUB %00000100         \ Thargon                                        Hostile
     EQUB %00000100         \ Constrictor                                    Hostile
     EQUB %00100000         \ Cougar                                        Innocent
    
     EQUB 0                 \ This byte appears to be unused
    
    
    
    
           Name: KWL%                                                    [Show more]
           Type: Variable
       Category: Status
        Summary: Fractional number of kills awarded for destroying each type of
                 ship
    
    
        Context: See this variable [on its own page](../game_data/variable/kwl_per_cent.md)
     References: This variable is used as follows:
                 * [EXNO2](../main/subroutine/exno2.md) uses KWL%
    
    
    
    
    
    * * *
    
    
     This figure contains the fractional part of the points that are added to the
     combat rank in TALLY when destroying a ship of this type. This is different to
     the original BBC Micro versions, where you always get a single combat point
     for everything you kill; in the Master version, it's more sophisticated.
    
     The integral part is stored in the KWH% table.
    
     Each fraction is stored as the numerator in a fraction with a denominator of
     256, so 149 represents 149 / 256 = 0.58203125 points.
    
    
    
    
    .KWL%
    
     EQUB 149               \ Missile                               0.58203125
     EQUB 0                 \ Coriolis space station                0.0
     EQUB 16                \ Escape pod                            0.0625
     EQUB 10                \ Alloy plate                           0.0390625
     EQUB 10                \ Cargo canister                        0.0390625
     EQUB 6                 \ Boulder                               0.0234375
     EQUB 8                 \ Asteroid                              0.03125
     EQUB 10                \ Splinter                              0.0390625
     EQUB 16                \ Shuttle                               0.0625
     EQUB 17                \ Transporter                           0.06640625
     EQUB 234               \ Cobra Mk III                          0.9140625
     EQUB 170               \ Python                                0.6640625
     EQUB 213               \ Boa                                   0.83203125
     EQUB 0                 \ Anaconda                              1.0
     EQUB 85                \ Rock hermit (asteroid)                0.33203125
     EQUB 26                \ Viper                                 0.1015625
     EQUB 85                \ Sidewinder                            0.33203125
     EQUB 128               \ Mamba                                 0.5
     EQUB 85                \ Krait                                 0.33203125
     EQUB 90                \ Adder                                 0.3515625
     EQUB 85                \ Gecko                                 0.33203125
     EQUB 170               \ Cobra Mk I                            0.6640625
     EQUB 50                \ Worm                                  0.1953125
     EQUB 42                \ Cobra Mk III (pirate)                 1.1640625
     EQUB 21                \ Asp Mk II                             1.08203125
     EQUB 42                \ Python (pirate)                       1.1640625
     EQUB 64                \ Fer-de-lance                          1.25
     EQUB 192               \ Moray                                 0.75
     EQUB 170               \ Thargoid                              2.6640625
     EQUB 33                \ Thargon                               0.12890625
     EQUB 85                \ Constrictor                           5.33203125
     EQUB 85                \ Cougar                                5.33203125
     EQUB 0                 \ Dodecahedron ("Dodo") space station   0.0
    
    
    
    
           Name: KWH%                                                    [Show more]
           Type: Variable
       Category: Status
        Summary: Integer number of kills awarded for destroying each type of ship
    
    
        Context: See this variable [on its own page](../game_data/variable/kwh_per_cent.md)
     References: This variable is used as follows:
                 * [EXNO2](../main/subroutine/exno2.md) uses KWH%
    
    
    
    
    
    * * *
    
    
     This figure contains the integer part of the points that are added to the
     combat rank in TALLY when destroying a ship of this type. This is different to
     the original BBC Micro versions, where you always get a single combat point
     for everything you kill; in the Master version, it's more sophisticated.
    
     The fractional part is stored in the KWL% table.
    
    
    
    
    .KWH%
    
     EQUB 0                 \ Missile                               0.58203125
     EQUB 0                 \ Coriolis space station                0.0
     EQUB 0                 \ Escape pod                            0.0625
     EQUB 0                 \ Alloy plate                           0.0390625
     EQUB 0                 \ Cargo canister                        0.0390625
     EQUB 0                 \ Boulder                               0.0234375
     EQUB 0                 \ Asteroid                              0.03125
     EQUB 0                 \ Splinter                              0.0390625
     EQUB 0                 \ Shuttle                               0.0625
     EQUB 0                 \ Transporter                           0.06640625
     EQUB 0                 \ Cobra Mk III                          0.9140625
     EQUB 0                 \ Python                                0.6640625
     EQUB 0                 \ Boa                                   0.83203125
     EQUB 1                 \ Anaconda                              1.0
     EQUB 0                 \ Rock hermit (asteroid)                0.33203125
     EQUB 0                 \ Viper                                 0.1015625
     EQUB 0                 \ Sidewinder                            0.33203125
     EQUB 0                 \ Mamba                                 0.5
     EQUB 0                 \ Krait                                 0.33203125
     EQUB 0                 \ Adder                                 0.3515625
     EQUB 0                 \ Gecko                                 0.33203125
     EQUB 0                 \ Cobra Mk I                            0.6640625
     EQUB 0                 \ Worm                                  0.1953125
     EQUB 1                 \ Cobra Mk III (pirate)                 1.1640625
     EQUB 1                 \ Asp Mk II                             1.08203125
     EQUB 1                 \ Python (pirate)                       1.1640625
     EQUB 1                 \ Fer-de-lance                          1.25
     EQUB 0                 \ Moray                                 0.75
     EQUB 2                 \ Thargoid                              2.6640625
     EQUB 0                 \ Thargon                               0.12890625
     EQUB 5                 \ Constrictor                           5.33203125
     EQUB 5                 \ Cougar                                5.33203125
     EQUB 0                 \ Dodecahedron ("Dodo") space station   0.0
    
    
    
    
           Name: VERTEX                                                  [Show more]
           Type: Macro
       Category: Drawing ships
        Summary: Macro definition for adding vertices to ship blueprints
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this macro [on its own page](../game_data/macro/vertex.md)
     References: This macro is used as follows:
                 * [SHIP_ADDER](../game_data/variable/ship_adder.md) uses VERTEX
                 * [SHIP_ANACONDA](../game_data/variable/ship_anaconda.md) uses VERTEX
                 * [SHIP_ASP_MK_2](../game_data/variable/ship_asp_mk_2.md) uses VERTEX
                 * [SHIP_ASTEROID](../game_data/variable/ship_asteroid.md) uses VERTEX
                 * [SHIP_BOA](../game_data/variable/ship_boa.md) uses VERTEX
                 * [SHIP_BOULDER](../game_data/variable/ship_boulder.md) uses VERTEX
                 * [SHIP_CANISTER](../game_data/variable/ship_canister.md) uses VERTEX
                 * [SHIP_COBRA_MK_1](../game_data/variable/ship_cobra_mk_1.md) uses VERTEX
                 * [SHIP_COBRA_MK_3](../game_data/variable/ship_cobra_mk_3.md) uses VERTEX
                 * [SHIP_COBRA_MK_3_P](../game_data/variable/ship_cobra_mk_3_p.md) uses VERTEX
                 * [SHIP_CONSTRICTOR](../game_data/variable/ship_constrictor.md) uses VERTEX
                 * [SHIP_CORIOLIS](../game_data/variable/ship_coriolis.md) uses VERTEX
                 * [SHIP_COUGAR](../game_data/variable/ship_cougar.md) uses VERTEX
                 * [SHIP_DODO](../game_data/variable/ship_dodo.md) uses VERTEX
                 * [SHIP_ESCAPE_POD](../game_data/variable/ship_escape_pod.md) uses VERTEX
                 * [SHIP_FER_DE_LANCE](../game_data/variable/ship_fer_de_lance.md) uses VERTEX
                 * [SHIP_GECKO](../game_data/variable/ship_gecko.md) uses VERTEX
                 * [SHIP_KRAIT](../game_data/variable/ship_krait.md) uses VERTEX
                 * [SHIP_MAMBA](../game_data/variable/ship_mamba.md) uses VERTEX
                 * [SHIP_MISSILE](../game_data/variable/ship_missile.md) uses VERTEX
                 * [SHIP_MORAY](../game_data/variable/ship_moray.md) uses VERTEX
                 * [SHIP_PLATE](../game_data/variable/ship_plate.md) uses VERTEX
                 * [SHIP_PYTHON](../game_data/variable/ship_python.md) uses VERTEX
                 * [SHIP_PYTHON_P](../game_data/variable/ship_python_p.md) uses VERTEX
                 * [SHIP_ROCK_HERMIT](../game_data/variable/ship_rock_hermit.md) uses VERTEX
                 * [SHIP_SHUTTLE](../game_data/variable/ship_shuttle.md) uses VERTEX
                 * [SHIP_SIDEWINDER](../game_data/variable/ship_sidewinder.md) uses VERTEX
                 * [SHIP_SPLINTER](../game_data/variable/ship_splinter.md) uses VERTEX
                 * [SHIP_THARGOID](../game_data/variable/ship_thargoid.md) uses VERTEX
                 * [SHIP_THARGON](../game_data/variable/ship_thargon.md) uses VERTEX
                 * [SHIP_TRANSPORTER](../game_data/variable/ship_transporter.md) uses VERTEX
                 * [SHIP_VIPER](../game_data/variable/ship_viper.md) uses VERTEX
                 * [SHIP_WORM](../game_data/variable/ship_worm.md) uses VERTEX
    
    
    
    
    
    * * *
    
    
     The following macro is used to build the ship blueprints:
    
       VERTEX x, y, z, face1, face2, face3, face4, visibility
    
    
    
    * * *
    
    
     Arguments:
    
       x                    The vertex's x-coordinate
    
       y                    The vertex's y-coordinate
    
       z                    The vertex's z-coordinate
    
       face1                The number of face 1 associated with this vertex
    
       face2                The number of face 2 associated with this vertex
    
       face3                The number of face 3 associated with this vertex
    
       face4                The number of face 4 associated with this vertex
    
       visibility           The visibility distance, beyond which the vertex is not
                            shown
    
    
    
    
    MACRO VERTEX x, y, z, face1, face2, face3, face4, visibility
    
     IF x < 0
      s_x = 1 << 7
     ELSE
      s_x = 0
     ENDIF
    
     IF y < 0
      s_y = 1 << 6
     ELSE
      s_y = 0
     ENDIF
    
     IF z < 0
      s_z = 1 << 5
     ELSE
      s_z = 0
     ENDIF
    
     s = s_x + s_y + s_z + visibility
     f1 = face1 + (face2 << 4)
     f2 = face3 + (face4 << 4)
     ax = ABS(x)
     ay = ABS(y)
     az = ABS(z)
    
     EQUB ax, ay, az, s, f1, f2
    
    ENDMACRO
    
    
    
    
           Name: EDGE                                                    [Show more]
           Type: Macro
       Category: Drawing ships
        Summary: Macro definition for adding edges to ship blueprints
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this macro [on its own page](../game_data/macro/edge.md)
     References: This macro is used as follows:
                 * [SHIP_ADDER](../game_data/variable/ship_adder.md) uses EDGE
                 * [SHIP_ANACONDA](../game_data/variable/ship_anaconda.md) uses EDGE
                 * [SHIP_ASP_MK_2](../game_data/variable/ship_asp_mk_2.md) uses EDGE
                 * [SHIP_ASTEROID](../game_data/variable/ship_asteroid.md) uses EDGE
                 * [SHIP_BOA](../game_data/variable/ship_boa.md) uses EDGE
                 * [SHIP_BOULDER](../game_data/variable/ship_boulder.md) uses EDGE
                 * [SHIP_CANISTER](../game_data/variable/ship_canister.md) uses EDGE
                 * [SHIP_COBRA_MK_1](../game_data/variable/ship_cobra_mk_1.md) uses EDGE
                 * [SHIP_COBRA_MK_3](../game_data/variable/ship_cobra_mk_3.md) uses EDGE
                 * [SHIP_CONSTRICTOR](../game_data/variable/ship_constrictor.md) uses EDGE
                 * [SHIP_CORIOLIS](../game_data/variable/ship_coriolis.md) uses EDGE
                 * [SHIP_COUGAR](../game_data/variable/ship_cougar.md) uses EDGE
                 * [SHIP_DODO](../game_data/variable/ship_dodo.md) uses EDGE
                 * [SHIP_ESCAPE_POD](../game_data/variable/ship_escape_pod.md) uses EDGE
                 * [SHIP_FER_DE_LANCE](../game_data/variable/ship_fer_de_lance.md) uses EDGE
                 * [SHIP_GECKO](../game_data/variable/ship_gecko.md) uses EDGE
                 * [SHIP_KRAIT](../game_data/variable/ship_krait.md) uses EDGE
                 * [SHIP_MAMBA](../game_data/variable/ship_mamba.md) uses EDGE
                 * [SHIP_MISSILE](../game_data/variable/ship_missile.md) uses EDGE
                 * [SHIP_MORAY](../game_data/variable/ship_moray.md) uses EDGE
                 * [SHIP_PLATE](../game_data/variable/ship_plate.md) uses EDGE
                 * [SHIP_PYTHON](../game_data/variable/ship_python.md) uses EDGE
                 * [SHIP_SHUTTLE](../game_data/variable/ship_shuttle.md) uses EDGE
                 * [SHIP_SIDEWINDER](../game_data/variable/ship_sidewinder.md) uses EDGE
                 * [SHIP_THARGOID](../game_data/variable/ship_thargoid.md) uses EDGE
                 * [SHIP_TRANSPORTER](../game_data/variable/ship_transporter.md) uses EDGE
                 * [SHIP_VIPER](../game_data/variable/ship_viper.md) uses EDGE
                 * [SHIP_WORM](../game_data/variable/ship_worm.md) uses EDGE
    
    
    
    
    
    * * *
    
    
     The following macro is used to build the ship blueprints:
    
       EDGE vertex1, vertex2, face1, face2, visibility
    
     When stored in memory, bytes #2 and #3 contain the vertex numbers multiplied
     by 4, so we can use them as indices into the heap at XX3 to fetch the screen
     coordinates for each vertex, as they are stored as four bytes containing two
     16-bit numbers (see part 10 of the LL9 routine for details).
    
    
    
    * * *
    
    
     Arguments:
    
       vertex1              The number of the vertex at the start of the edge
    
       vertex1              The number of the vertex at the end of the edge
    
       face1                The number of face 1 associated with this edge
    
       face2                The number of face 2 associated with this edge
    
       visibility           The visibility distance, beyond which the edge is not
                            shown
    
    
    
    
    MACRO EDGE vertex1, vertex2, face1, face2, visibility
    
     f = face1 + (face2 << 4)
     EQUB visibility, f, vertex1 << 2, vertex2 << 2
    
    ENDMACRO
    
    
    
    
           Name: FACE                                                    [Show more]
           Type: Macro
       Category: Drawing ships
        Summary: Macro definition for adding faces to ship blueprints
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this macro [on its own page](../game_data/macro/face.md)
     References: This macro is used as follows:
                 * [SHIP_ADDER](../game_data/variable/ship_adder.md) uses FACE
                 * [SHIP_ANACONDA](../game_data/variable/ship_anaconda.md) uses FACE
                 * [SHIP_ASP_MK_2](../game_data/variable/ship_asp_mk_2.md) uses FACE
                 * [SHIP_ASTEROID](../game_data/variable/ship_asteroid.md) uses FACE
                 * [SHIP_BOA](../game_data/variable/ship_boa.md) uses FACE
                 * [SHIP_BOULDER](../game_data/variable/ship_boulder.md) uses FACE
                 * [SHIP_CANISTER](../game_data/variable/ship_canister.md) uses FACE
                 * [SHIP_COBRA_MK_1](../game_data/variable/ship_cobra_mk_1.md) uses FACE
                 * [SHIP_COBRA_MK_3](../game_data/variable/ship_cobra_mk_3.md) uses FACE
                 * [SHIP_CONSTRICTOR](../game_data/variable/ship_constrictor.md) uses FACE
                 * [SHIP_CORIOLIS](../game_data/variable/ship_coriolis.md) uses FACE
                 * [SHIP_COUGAR](../game_data/variable/ship_cougar.md) uses FACE
                 * [SHIP_DODO](../game_data/variable/ship_dodo.md) uses FACE
                 * [SHIP_ESCAPE_POD](../game_data/variable/ship_escape_pod.md) uses FACE
                 * [SHIP_FER_DE_LANCE](../game_data/variable/ship_fer_de_lance.md) uses FACE
                 * [SHIP_GECKO](../game_data/variable/ship_gecko.md) uses FACE
                 * [SHIP_KRAIT](../game_data/variable/ship_krait.md) uses FACE
                 * [SHIP_MAMBA](../game_data/variable/ship_mamba.md) uses FACE
                 * [SHIP_MISSILE](../game_data/variable/ship_missile.md) uses FACE
                 * [SHIP_MORAY](../game_data/variable/ship_moray.md) uses FACE
                 * [SHIP_PLATE](../game_data/variable/ship_plate.md) uses FACE
                 * [SHIP_PYTHON](../game_data/variable/ship_python.md) uses FACE
                 * [SHIP_SHUTTLE](../game_data/variable/ship_shuttle.md) uses FACE
                 * [SHIP_SIDEWINDER](../game_data/variable/ship_sidewinder.md) uses FACE
                 * [SHIP_SPLINTER](../game_data/variable/ship_splinter.md) uses FACE
                 * [SHIP_THARGOID](../game_data/variable/ship_thargoid.md) uses FACE
                 * [SHIP_THARGON](../game_data/variable/ship_thargon.md) uses FACE
                 * [SHIP_TRANSPORTER](../game_data/variable/ship_transporter.md) uses FACE
                 * [SHIP_VIPER](../game_data/variable/ship_viper.md) uses FACE
                 * [SHIP_WORM](../game_data/variable/ship_worm.md) uses FACE
    
    
    
    
    
    * * *
    
    
     The following macro is used to build the ship blueprints:
    
       FACE normal_x, normal_y, normal_z, visibility
    
    
    
    * * *
    
    
     Arguments:
    
       normal_x             The face normal's x-coordinate
    
       normal_y             The face normal's y-coordinate
    
       normal_z             The face normal's z-coordinate
    
       visibility           The visibility distance, beyond which the edge is always
                            shown
    
    
    
    
    MACRO FACE normal_x, normal_y, normal_z, visibility
    
     IF normal_x < 0
      s_x = 1 << 7
     ELSE
      s_x = 0
     ENDIF
    
     IF normal_y < 0
      s_y = 1 << 6
     ELSE
      s_y = 0
     ENDIF
    
     IF normal_z < 0
      s_z = 1 << 5
     ELSE
      s_z = 0
     ENDIF
    
     s = s_x + s_y + s_z + visibility
     ax = ABS(normal_x)
     ay = ABS(normal_y)
     az = ABS(normal_z)
    
     EQUB s, ax, ay, az
    
    ENDMACRO
    
    
    
    
           Name: SHIP_MISSILE                                            [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a missile
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_missile.md)
     Variations: See [code variations](../related/compare/main/variable/ship_missile.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_MISSILE
    
    
    
    
    
    
    .SHIP_MISSILE
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 40 * 40           \ Targetable area          = 40 * 40
    
     EQUB LO(SHIP_MISSILE_EDGES - SHIP_MISSILE)        \ Edges data offset (low)
     EQUB LO(SHIP_MISSILE_FACES - SHIP_MISSILE)        \ Faces data offset (low)
    
     EQUB 85                \ Max. edge count          = (85 - 1) / 4 = 21
     EQUB 0                 \ Gun vertex               = 0
     EQUB 10                \ Explosion count          = 1, as (4 * n) + 6 = 10
     EQUB 102               \ Number of vertices       = 102 / 6 = 17
     EQUB 24                \ Number of edges          = 24
     EQUW 0                 \ Bounty                   = 0
     EQUB 36                \ Number of faces          = 36 / 4 = 9
     EQUB 14                \ Visibility distance      = 14
     EQUB 2                 \ Max. energy              = 2
     EQUB 44                \ Max. speed               = 44
    
     EQUB HI(SHIP_MISSILE_EDGES - SHIP_MISSILE)        \ Edges data offset (high)
     EQUB HI(SHIP_MISSILE_FACES - SHIP_MISSILE)        \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_MISSILE_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,   68,     0,      1,    2,     3,         31    \ Vertex 0
     VERTEX    8,   -8,   36,     1,      2,    4,     5,         31    \ Vertex 1
     VERTEX    8,    8,   36,     2,      3,    4,     7,         31    \ Vertex 2
     VERTEX   -8,    8,   36,     0,      3,    6,     7,         31    \ Vertex 3
     VERTEX   -8,   -8,   36,     0,      1,    5,     6,         31    \ Vertex 4
     VERTEX    8,    8,  -44,     4,      7,    8,     8,         31    \ Vertex 5
     VERTEX    8,   -8,  -44,     4,      5,    8,     8,         31    \ Vertex 6
     VERTEX   -8,   -8,  -44,     5,      6,    8,     8,         31    \ Vertex 7
     VERTEX   -8,    8,  -44,     6,      7,    8,     8,         31    \ Vertex 8
     VERTEX   12,   12,  -44,     4,      7,    8,     8,          8    \ Vertex 9
     VERTEX   12,  -12,  -44,     4,      5,    8,     8,          8    \ Vertex 10
     VERTEX  -12,  -12,  -44,     5,      6,    8,     8,          8    \ Vertex 11
     VERTEX  -12,   12,  -44,     6,      7,    8,     8,          8    \ Vertex 12
     VERTEX   -8,    8,  -12,     6,      7,    7,     7,          8    \ Vertex 13
     VERTEX   -8,   -8,  -12,     5,      6,    6,     6,          8    \ Vertex 14
     VERTEX    8,    8,  -12,     4,      7,    7,     7,          8    \ Vertex 15
     VERTEX    8,   -8,  -12,     4,      5,    5,     5,          8    \ Vertex 16
    
    .SHIP_MISSILE_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     1,     2,         31    \ Edge 0
     EDGE       0,       2,     2,     3,         31    \ Edge 1
     EDGE       0,       3,     0,     3,         31    \ Edge 2
     EDGE       0,       4,     0,     1,         31    \ Edge 3
     EDGE       1,       2,     4,     2,         31    \ Edge 4
     EDGE       1,       4,     1,     5,         31    \ Edge 5
     EDGE       3,       4,     0,     6,         31    \ Edge 6
     EDGE       2,       3,     3,     7,         31    \ Edge 7
     EDGE       2,       5,     4,     7,         31    \ Edge 8
     EDGE       1,       6,     4,     5,         31    \ Edge 9
     EDGE       4,       7,     5,     6,         31    \ Edge 10
     EDGE       3,       8,     6,     7,         31    \ Edge 11
     EDGE       7,       8,     6,     8,         31    \ Edge 12
     EDGE       5,       8,     7,     8,         31    \ Edge 13
     EDGE       5,       6,     4,     8,         31    \ Edge 14
     EDGE       6,       7,     5,     8,         31    \ Edge 15
     EDGE       6,      10,     5,     8,          8    \ Edge 16
     EDGE       5,       9,     7,     8,          8    \ Edge 17
     EDGE       8,      12,     7,     8,          8    \ Edge 18
     EDGE       7,      11,     5,     8,          8    \ Edge 19
     EDGE       9,      15,     4,     7,          8    \ Edge 20
     EDGE      10,      16,     4,     5,          8    \ Edge 21
     EDGE      12,      13,     6,     7,          8    \ Edge 22
     EDGE      11,      14,     5,     6,          8    \ Edge 23
    
    .SHIP_MISSILE_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE      -64,        0,       16,         31      \ Face 0
     FACE        0,      -64,       16,         31      \ Face 1
     FACE       64,        0,       16,         31      \ Face 2
     FACE        0,       64,       16,         31      \ Face 3
     FACE       32,        0,        0,         31      \ Face 4
     FACE        0,      -32,        0,         31      \ Face 5
     FACE      -32,        0,        0,         31      \ Face 6
     FACE        0,       32,        0,         31      \ Face 7
     FACE        0,        0,     -176,         31      \ Face 8
    
    
    
    
           Name: SHIP_CORIOLIS                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Coriolis space station
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_coriolis.md)
     Variations: See [code variations](../related/compare/main/variable/ship_coriolis.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_CORIOLIS
    
    
    
    
    
    
    .SHIP_CORIOLIS
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 160 * 160         \ Targetable area          = 160 * 160
    
     EQUB LO(SHIP_CORIOLIS_EDGES - SHIP_CORIOLIS)      \ Edges data offset (low)
     EQUB LO(SHIP_CORIOLIS_FACES - SHIP_CORIOLIS)      \ Faces data offset (low)
    
     EQUB 89                \ Max. edge count          = (89 - 1) / 4 = 22
     EQUB 0                 \ Gun vertex               = 0
     EQUB 54                \ Explosion count          = 12, as (4 * n) + 6 = 54
     EQUB 96                \ Number of vertices       = 96 / 6 = 16
     EQUB 28                \ Number of edges          = 28
     EQUW 0                 \ Bounty                   = 0
     EQUB 56                \ Number of faces          = 56 / 4 = 14
     EQUB 120               \ Visibility distance      = 120
     EQUB 240               \ Max. energy              = 240
     EQUB 0                 \ Max. speed               = 0
    
     EQUB HI(SHIP_CORIOLIS_EDGES - SHIP_CORIOLIS)      \ Edges data offset (high)
     EQUB HI(SHIP_CORIOLIS_FACES - SHIP_CORIOLIS)      \ Faces data offset (high)
    
     EQUB 0                 \ Normals are scaled by    = 2^0 = 1
     EQUB %00000110         \ Laser power              = 0
                            \ Missiles                 = 6
    
    .SHIP_CORIOLIS_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  160,    0,  160,     0,      1,    2,     6,         31    \ Vertex 0
     VERTEX    0,  160,  160,     0,      2,    3,     8,         31    \ Vertex 1
     VERTEX -160,    0,  160,     0,      3,    4,     7,         31    \ Vertex 2
     VERTEX    0, -160,  160,     0,      1,    4,     5,         31    \ Vertex 3
     VERTEX  160, -160,    0,     1,      5,    6,    10,         31    \ Vertex 4
     VERTEX  160,  160,    0,     2,      6,    8,    11,         31    \ Vertex 5
     VERTEX -160,  160,    0,     3,      7,    8,    12,         31    \ Vertex 6
     VERTEX -160, -160,    0,     4,      5,    7,     9,         31    \ Vertex 7
     VERTEX  160,    0, -160,     6,     10,   11,    13,         31    \ Vertex 8
     VERTEX    0,  160, -160,     8,     11,   12,    13,         31    \ Vertex 9
     VERTEX -160,    0, -160,     7,      9,   12,    13,         31    \ Vertex 10
     VERTEX    0, -160, -160,     5,      9,   10,    13,         31    \ Vertex 11
     VERTEX   10,  -30,  160,     0,      0,    0,     0,         30    \ Vertex 12
     VERTEX   10,   30,  160,     0,      0,    0,     0,         30    \ Vertex 13
     VERTEX  -10,   30,  160,     0,      0,    0,     0,         30    \ Vertex 14
     VERTEX  -10,  -30,  160,     0,      0,    0,     0,         30    \ Vertex 15
    
    .SHIP_CORIOLIS_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       3,     0,     1,         31    \ Edge 0
     EDGE       0,       1,     0,     2,         31    \ Edge 1
     EDGE       1,       2,     0,     3,         31    \ Edge 2
     EDGE       2,       3,     0,     4,         31    \ Edge 3
     EDGE       3,       4,     1,     5,         31    \ Edge 4
     EDGE       0,       4,     1,     6,         31    \ Edge 5
     EDGE       0,       5,     2,     6,         31    \ Edge 6
     EDGE       5,       1,     2,     8,         31    \ Edge 7
     EDGE       1,       6,     3,     8,         31    \ Edge 8
     EDGE       2,       6,     3,     7,         31    \ Edge 9
     EDGE       2,       7,     4,     7,         31    \ Edge 10
     EDGE       3,       7,     4,     5,         31    \ Edge 11
     EDGE       8,      11,    10,    13,         31    \ Edge 12
     EDGE       8,       9,    11,    13,         31    \ Edge 13
     EDGE       9,      10,    12,    13,         31    \ Edge 14
     EDGE      10,      11,     9,    13,         31    \ Edge 15
     EDGE       4,      11,     5,    10,         31    \ Edge 16
     EDGE       4,       8,     6,    10,         31    \ Edge 17
     EDGE       5,       8,     6,    11,         31    \ Edge 18
     EDGE       5,       9,     8,    11,         31    \ Edge 19
     EDGE       6,       9,     8,    12,         31    \ Edge 20
     EDGE       6,      10,     7,    12,         31    \ Edge 21
     EDGE       7,      10,     7,     9,         31    \ Edge 22
     EDGE       7,      11,     5,     9,         31    \ Edge 23
     EDGE      12,      13,     0,     0,         30    \ Edge 24
     EDGE      13,      14,     0,     0,         30    \ Edge 25
     EDGE      14,      15,     0,     0,         30    \ Edge 26
     EDGE      15,      12,     0,     0,         30    \ Edge 27
    
    .SHIP_CORIOLIS_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,        0,      160,         31      \ Face 0
     FACE      107,     -107,      107,         31      \ Face 1
     FACE      107,      107,      107,         31      \ Face 2
     FACE     -107,      107,      107,         31      \ Face 3
     FACE     -107,     -107,      107,         31      \ Face 4
     FACE        0,     -160,        0,         31      \ Face 5
     FACE      160,        0,        0,         31      \ Face 6
     FACE     -160,        0,        0,         31      \ Face 7
     FACE        0,      160,        0,         31      \ Face 8
     FACE     -107,     -107,     -107,         31      \ Face 9
     FACE      107,     -107,     -107,         31      \ Face 10
     FACE      107,      107,     -107,         31      \ Face 11
     FACE     -107,      107,     -107,         31      \ Face 12
     FACE        0,        0,     -160,         31      \ Face 13
    
    
    
    
           Name: SHIP_ESCAPE_POD                                         [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an escape pod
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_escape_pod.md)
     Variations: See [code variations](../related/compare/main/variable/ship_escape_pod.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_ESCAPE_POD
    
    
    
    
    
    
    .SHIP_ESCAPE_POD
    
     EQUB 0 + (2 << 4)      \ Max. canisters on demise = 0
                            \ Market item when scooped = 2 + 1 = 3 (slaves)
     EQUW 16 * 16           \ Targetable area          = 16 * 16
    
     EQUB LO(SHIP_ESCAPE_POD_EDGES - SHIP_ESCAPE_POD)  \ Edges data offset (low)
     EQUB LO(SHIP_ESCAPE_POD_FACES - SHIP_ESCAPE_POD)  \ Faces data offset (low)
    
     EQUB 29                \ Max. edge count          = (29 - 1) / 4 = 7
     EQUB 0                 \ Gun vertex               = 0
     EQUB 22                \ Explosion count          = 4, as (4 * n) + 6 = 22
     EQUB 24                \ Number of vertices       = 24 / 6 = 4
     EQUB 6                 \ Number of edges          = 6
     EQUW 0                 \ Bounty                   = 0
     EQUB 16                \ Number of faces          = 16 / 4 = 4
     EQUB 8                 \ Visibility distance      = 8
     EQUB 17                \ Max. energy              = 17
     EQUB 8                 \ Max. speed               = 8
    
     EQUB HI(SHIP_ESCAPE_POD_EDGES - SHIP_ESCAPE_POD)  \ Edges data offset (high)
     EQUB HI(SHIP_ESCAPE_POD_FACES - SHIP_ESCAPE_POD)  \ Faces data offset (high)
    
     EQUB 4                 \ Normals are scaled by    =  2^4 = 16
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_ESCAPE_POD_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   -7,    0,   36,     2,      1,    3,     3,         31    \ Vertex 0
     VERTEX   -7,  -14,  -12,     2,      0,    3,     3,         31    \ Vertex 1
     VERTEX   -7,   14,  -12,     1,      0,    3,     3,         31    \ Vertex 2
     VERTEX   21,    0,    0,     1,      0,    2,     2,         31    \ Vertex 3
    
    .SHIP_ESCAPE_POD_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     3,     2,         31    \ Edge 0
     EDGE       1,       2,     3,     0,         31    \ Edge 1
     EDGE       2,       3,     1,     0,         31    \ Edge 2
     EDGE       3,       0,     2,     1,         31    \ Edge 3
     EDGE       0,       2,     3,     1,         31    \ Edge 4
     EDGE       3,       1,     2,     0,         31    \ Edge 5
    
    .SHIP_ESCAPE_POD_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE       52,        0,     -122,         31      \ Face 0
     FACE       39,      103,       30,         31      \ Face 1
     FACE       39,     -103,       30,         31      \ Face 2
     FACE     -112,        0,        0,         31      \ Face 3
    
    
    
    
           Name: SHIP_PLATE                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an alloy plate
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_plate.md)
     Variations: See [code variations](../related/compare/main/variable/ship_plate.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_PLATE
    
    
    
    
    
    
    .SHIP_PLATE
    
     EQUB 0 + (8 << 4)      \ Max. canisters on demise = 0
                            \ Market item when scooped = 8 + 1 = 9 (alloys)
     EQUW 10 * 10           \ Targetable area          = 10 * 10
    
     EQUB LO(SHIP_PLATE_EDGES - SHIP_PLATE)            \ Edges data offset (low)
     EQUB LO(SHIP_PLATE_FACES - SHIP_PLATE)            \ Faces data offset (low)
    
     EQUB 21                \ Max. edge count          = (21 - 1) / 4 = 5
     EQUB 0                 \ Gun vertex               = 0
     EQUB 10                \ Explosion count          = 1, as (4 * n) + 6 = 10
     EQUB 24                \ Number of vertices       = 24 / 6 = 4
     EQUB 4                 \ Number of edges          = 4
     EQUW 0                 \ Bounty                   = 0
     EQUB 4                 \ Number of faces          = 4 / 4 = 1
     EQUB 5                 \ Visibility distance      = 5
     EQUB 16                \ Max. energy              = 16
     EQUB 16                \ Max. speed               = 16
    
     EQUB HI(SHIP_PLATE_EDGES - SHIP_PLATE)            \ Edges data offset (high)
     EQUB HI(SHIP_PLATE_FACES - SHIP_PLATE)            \ Faces data offset (high)
    
     EQUB 3                 \ Normals are scaled by    = 2^3 = 8
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_PLATE_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -15,  -22,   -9,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX  -15,   38,   -9,    15,     15,   15,    15,         31    \ Vertex 1
     VERTEX   19,   32,   11,    15,     15,   15,    15,         20    \ Vertex 2
     VERTEX   10,  -46,    6,    15,     15,   15,    15,         20    \ Vertex 3
    
    .SHIP_PLATE_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,    15,    15,         31    \ Edge 0
     EDGE       1,       2,    15,    15,         16    \ Edge 1
     EDGE       2,       3,    15,    15,         20    \ Edge 2
     EDGE       3,       0,    15,    15,         16    \ Edge 3
    
    .SHIP_PLATE_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,        0,        0,          0      \ Face 0
    
    
    
    
           Name: SHIP_CANISTER                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a cargo canister
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_canister.md)
     Variations: See [code variations](../related/compare/main/variable/ship_canister.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_CANISTER
    
    
    
    
    
    
    .SHIP_CANISTER
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 20 * 20           \ Targetable area          = 20 * 20
    
     EQUB LO(SHIP_CANISTER_EDGES - SHIP_CANISTER)      \ Edges data offset (low)
     EQUB LO(SHIP_CANISTER_FACES - SHIP_CANISTER)      \ Faces data offset (low)
    
     EQUB 53                \ Max. edge count          = (53 - 1) / 4 = 13
     EQUB 0                 \ Gun vertex               = 0
     EQUB 18                \ Explosion count          = 3, as (4 * n) + 6 = 18
     EQUB 60                \ Number of vertices       = 60 / 6 = 10
     EQUB 15                \ Number of edges          = 15
     EQUW 0                 \ Bounty                   = 0
     EQUB 28                \ Number of faces          = 28 / 4 = 7
     EQUB 12                \ Visibility distance      = 12
     EQUB 17                \ Max. energy              = 17
     EQUB 15                \ Max. speed               = 15
    
     EQUB HI(SHIP_CANISTER_EDGES - SHIP_CANISTER)      \ Edges data offset (high)
     EQUB HI(SHIP_CANISTER_FACES - SHIP_CANISTER)      \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_CANISTER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   24,   16,    0,     0,      1,    5,     5,         31    \ Vertex 0
     VERTEX   24,    5,   15,     0,      1,    2,     2,         31    \ Vertex 1
     VERTEX   24,  -13,    9,     0,      2,    3,     3,         31    \ Vertex 2
     VERTEX   24,  -13,   -9,     0,      3,    4,     4,         31    \ Vertex 3
     VERTEX   24,    5,  -15,     0,      4,    5,     5,         31    \ Vertex 4
     VERTEX  -24,   16,    0,     1,      5,    6,     6,         31    \ Vertex 5
     VERTEX  -24,    5,   15,     1,      2,    6,     6,         31    \ Vertex 6
     VERTEX  -24,  -13,    9,     2,      3,    6,     6,         31    \ Vertex 7
     VERTEX  -24,  -13,   -9,     3,      4,    6,     6,         31    \ Vertex 8
     VERTEX  -24,    5,  -15,     4,      5,    6,     6,         31    \ Vertex 9
    
    .SHIP_CANISTER_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     0,     1,         31    \ Edge 0
     EDGE       1,       2,     0,     2,         31    \ Edge 1
     EDGE       2,       3,     0,     3,         31    \ Edge 2
     EDGE       3,       4,     0,     4,         31    \ Edge 3
     EDGE       0,       4,     0,     5,         31    \ Edge 4
     EDGE       0,       5,     1,     5,         31    \ Edge 5
     EDGE       1,       6,     1,     2,         31    \ Edge 6
     EDGE       2,       7,     2,     3,         31    \ Edge 7
     EDGE       3,       8,     3,     4,         31    \ Edge 8
     EDGE       4,       9,     4,     5,         31    \ Edge 9
     EDGE       5,       6,     1,     6,         31    \ Edge 10
     EDGE       6,       7,     2,     6,         31    \ Edge 11
     EDGE       7,       8,     3,     6,         31    \ Edge 12
     EDGE       8,       9,     4,     6,         31    \ Edge 13
     EDGE       9,       5,     5,     6,         31    \ Edge 14
    
    .SHIP_CANISTER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE       96,        0,        0,         31      \ Face 0
     FACE        0,       41,       30,         31      \ Face 1
     FACE        0,      -18,       48,         31      \ Face 2
     FACE        0,      -51,        0,         31      \ Face 3
     FACE        0,      -18,      -48,         31      \ Face 4
     FACE        0,       41,      -30,         31      \ Face 5
     FACE      -96,        0,        0,         31      \ Face 6
    
    
    
    
           Name: SHIP_BOULDER                                            [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a boulder
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_boulder.md)
     Variations: See [code variations](../related/compare/main/variable/ship_boulder.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_BOULDER
    
    
    
    
    
    
    .SHIP_BOULDER
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 30 * 30           \ Targetable area          = 30 * 30
    
     EQUB LO(SHIP_BOULDER_EDGES - SHIP_BOULDER)        \ Edges data offset (low)
     EQUB LO(SHIP_BOULDER_FACES - SHIP_BOULDER)        \ Faces data offset (low)
    
     EQUB 49                \ Max. edge count          = (49 - 1) / 4 = 12
     EQUB 0                 \ Gun vertex               = 0
     EQUB 14                \ Explosion count          = 2, as (4 * n) + 6 = 14
     EQUB 42                \ Number of vertices       = 42 / 6 = 7
     EQUB 15                \ Number of edges          = 15
     EQUW 1                 \ Bounty                   = 1
     EQUB 40                \ Number of faces          = 40 / 4 = 10
     EQUB 20                \ Visibility distance      = 20
     EQUB 20                \ Max. energy              = 20
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_BOULDER_EDGES - SHIP_BOULDER)        \ Edges data offset (high)
     EQUB HI(SHIP_BOULDER_FACES - SHIP_BOULDER)        \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_BOULDER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -18,   37,  -11,     1,      0,    9,     5,         31    \ Vertex 0
     VERTEX   30,    7,   12,     2,      1,    6,     5,         31    \ Vertex 1
     VERTEX   28,   -7,  -12,     3,      2,    7,     6,         31    \ Vertex 2
     VERTEX    2,    0,  -39,     4,      3,    8,     7,         31    \ Vertex 3
     VERTEX  -28,   34,  -30,     4,      0,    9,     8,         31    \ Vertex 4
     VERTEX    5,  -10,   13,    15,     15,   15,    15,         31    \ Vertex 5
     VERTEX   20,   17,  -30,    15,     15,   15,    15,         31    \ Vertex 6
    
    .SHIP_BOULDER_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     5,     1,         31    \ Edge 0
     EDGE       1,       2,     6,     2,         31    \ Edge 1
     EDGE       2,       3,     7,     3,         31    \ Edge 2
     EDGE       3,       4,     8,     4,         31    \ Edge 3
     EDGE       4,       0,     9,     0,         31    \ Edge 4
     EDGE       0,       5,     1,     0,         31    \ Edge 5
     EDGE       1,       5,     2,     1,         31    \ Edge 6
     EDGE       2,       5,     3,     2,         31    \ Edge 7
     EDGE       3,       5,     4,     3,         31    \ Edge 8
     EDGE       4,       5,     4,     0,         31    \ Edge 9
     EDGE       0,       6,     9,     5,         31    \ Edge 10
     EDGE       1,       6,     6,     5,         31    \ Edge 11
     EDGE       2,       6,     7,     6,         31    \ Edge 12
     EDGE       3,       6,     8,     7,         31    \ Edge 13
     EDGE       4,       6,     9,     8,         31    \ Edge 14
    
    .SHIP_BOULDER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE      -15,       -3,        8,         31      \ Face 0
     FACE       -7,       12,       30,         31      \ Face 1
     FACE       32,      -47,       24,         31      \ Face 2
     FACE       -3,      -39,       -7,         31      \ Face 3
     FACE       -5,       -4,       -1,         31      \ Face 4
     FACE       49,       84,        8,         31      \ Face 5
     FACE      112,       21,      -21,         31      \ Face 6
     FACE       76,      -35,      -82,         31      \ Face 7
     FACE       22,       56,     -137,         31      \ Face 8
     FACE       40,      110,      -38,         31      \ Face 9
    
    
    
    
           Name: SHIP_ASTEROID                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an asteroid
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_asteroid.md)
     Variations: See [code variations](../related/compare/main/variable/ship_asteroid.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_ASTEROID
    
    
    
    
    
    
    .SHIP_ASTEROID
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 80 * 80           \ Targetable area          = 80 * 80
    
     EQUB LO(SHIP_ASTEROID_EDGES - SHIP_ASTEROID)      \ Edges data offset (low)
     EQUB LO(SHIP_ASTEROID_FACES - SHIP_ASTEROID)      \ Faces data offset (low)
    
     EQUB 69                \ Max. edge count          = (69 - 1) / 4 = 17
     EQUB 0                 \ Gun vertex               = 0
     EQUB 34                \ Explosion count          = 7, as (4 * n) + 6 = 34
     EQUB 54                \ Number of vertices       = 54 / 6 = 9
     EQUB 21                \ Number of edges          = 21
     EQUW 5                 \ Bounty                   = 5
     EQUB 56                \ Number of faces          = 56 / 4 = 14
     EQUB 50                \ Visibility distance      = 50
     EQUB 60                \ Max. energy              = 60
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_ASTEROID_EDGES - SHIP_ASTEROID)      \ Edges data offset (high)
     EQUB HI(SHIP_ASTEROID_FACES - SHIP_ASTEROID)      \ Faces data offset (high)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_ASTEROID_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,   80,    0,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX  -80,  -10,    0,    15,     15,   15,    15,         31    \ Vertex 1
     VERTEX    0,  -80,    0,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX   70,  -40,    0,    15,     15,   15,    15,         31    \ Vertex 3
     VERTEX   60,   50,    0,     5,      6,   12,    13,         31    \ Vertex 4
     VERTEX   50,    0,   60,    15,     15,   15,    15,         31    \ Vertex 5
     VERTEX  -40,    0,   70,     0,      1,    2,     3,         31    \ Vertex 6
     VERTEX    0,   30,  -75,    15,     15,   15,    15,         31    \ Vertex 7
     VERTEX    0,  -50,  -60,     8,      9,   10,    11,         31    \ Vertex 8
    
    .SHIP_ASTEROID_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     2,     7,         31    \ Edge 0
     EDGE       0,       4,     6,    13,         31    \ Edge 1
     EDGE       3,       4,     5,    12,         31    \ Edge 2
     EDGE       2,       3,     4,    11,         31    \ Edge 3
     EDGE       1,       2,     3,    10,         31    \ Edge 4
     EDGE       1,       6,     2,     3,         31    \ Edge 5
     EDGE       2,       6,     1,     3,         31    \ Edge 6
     EDGE       2,       5,     1,     4,         31    \ Edge 7
     EDGE       5,       6,     0,     1,         31    \ Edge 8
     EDGE       0,       5,     0,     6,         31    \ Edge 9
     EDGE       3,       5,     4,     5,         31    \ Edge 10
     EDGE       0,       6,     0,     2,         31    \ Edge 11
     EDGE       4,       5,     5,     6,         31    \ Edge 12
     EDGE       1,       8,     8,    10,         31    \ Edge 13
     EDGE       1,       7,     7,     8,         31    \ Edge 14
     EDGE       0,       7,     7,    13,         31    \ Edge 15
     EDGE       4,       7,    12,    13,         31    \ Edge 16
     EDGE       3,       7,     9,    12,         31    \ Edge 17
     EDGE       3,       8,     9,    11,         31    \ Edge 18
     EDGE       2,       8,    10,    11,         31    \ Edge 19
     EDGE       7,       8,     8,     9,         31    \ Edge 20
    
    .SHIP_ASTEROID_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        9,       66,       81,         31      \ Face 0
     FACE        9,      -66,       81,         31      \ Face 1
     FACE      -72,       64,       31,         31      \ Face 2
     FACE      -64,      -73,       47,         31      \ Face 3
     FACE       45,      -79,       65,         31      \ Face 4
     FACE      135,       15,       35,         31      \ Face 5
     FACE       38,       76,       70,         31      \ Face 6
     FACE      -66,       59,      -39,         31      \ Face 7
     FACE      -67,      -15,      -80,         31      \ Face 8
     FACE       66,      -14,      -75,         31      \ Face 9
     FACE      -70,      -80,      -40,         31      \ Face 10
     FACE       58,     -102,      -51,         31      \ Face 11
     FACE       81,        9,      -67,         31      \ Face 12
     FACE       47,       94,      -63,         31      \ Face 13
    
    
    
    
           Name: SHIP_SPLINTER                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a splinter
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_splinter.md)
     Variations: See [code variations](../related/compare/main/variable/ship_splinter.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_SPLINTER
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the splinter reuses the edges data from the escape pod,
     so the edges data offset is negative.
    
    
    
    
    .SHIP_SPLINTER
    
     EQUB 0 + (11 << 4)     \ Max. canisters on demise = 0
                            \ Market item when scooped = 11 + 1 = 12 (minerals)
     EQUW 16 * 16           \ Targetable area          = 16 * 16
    
     EQUB LO(SHIP_ESCAPE_POD_EDGES - SHIP_SPLINTER)    \ Edges from escape pod
     EQUB LO(SHIP_SPLINTER_FACES - SHIP_SPLINTER) + 24 \ Faces data offset (low)
    
     EQUB 29                \ Max. edge count          = (29 - 1) / 4 = 7
     EQUB 0                 \ Gun vertex               = 0
     EQUB 22                \ Explosion count          = 4, as (4 * n) + 6 = 22
     EQUB 24                \ Number of vertices       = 24 / 6 = 4
     EQUB 6                 \ Number of edges          = 6
     EQUW 0                 \ Bounty                   = 0
     EQUB 16                \ Number of faces          = 16 / 4 = 4
     EQUB 8                 \ Visibility distance      = 8
     EQUB 20                \ Max. energy              = 20
     EQUB 10                \ Max. speed               = 10
    
     EQUB HI(SHIP_ESCAPE_POD_EDGES - SHIP_SPLINTER)    \ Edges from escape pod
     EQUB HI(SHIP_SPLINTER_FACES - SHIP_SPLINTER)      \ Faces data offset (low)
    
     EQUB 5                 \ Normals are scaled by    = 2^5 = 32
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_SPLINTER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -24,  -25,   16,     2,      1,    3,     3,         31    \ Vertex 0
     VERTEX    0,   12,  -10,     2,      0,    3,     3,         31    \ Vertex 1
     VERTEX   11,   -6,    2,     1,      0,    3,     3,         31    \ Vertex 2
     VERTEX   12,   42,    7,     1,      0,    2,     2,         31    \ Vertex 3
    
    .SHIP_SPLINTER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE       35,        0,        4,         31      \ Face 0
     FACE        3,        4,        8,         31      \ Face 1
     FACE        1,        8,       12,         31      \ Face 2
     FACE       18,       12,        0,         31      \ Face 3
    
    
    
    
           Name: SHIP_SHUTTLE                                            [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Shuttle
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_shuttle.md)
     Variations: See [code variations](../related/compare/main/variable/ship_shuttle.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_SHUTTLE
    
    
    
    
    
    
    .SHIP_SHUTTLE
    
     EQUB 15                \ Max. canisters on demise = 15
     EQUW 50 * 50           \ Targetable area          = 50 * 50
    
     EQUB LO(SHIP_SHUTTLE_EDGES - SHIP_SHUTTLE)        \ Edges data offset (low)
     EQUB LO(SHIP_SHUTTLE_FACES - SHIP_SHUTTLE)        \ Faces data offset (low)
    
     EQUB 113               \ Max. edge count          = (113 - 1) / 4 = 28
     EQUB 0                 \ Gun vertex               = 0
     EQUB 38                \ Explosion count          = 8, as (4 * n) + 6 = 38
     EQUB 114               \ Number of vertices       = 114 / 6 = 19
     EQUB 30                \ Number of edges          = 30
     EQUW 0                 \ Bounty                   = 0
     EQUB 52                \ Number of faces          = 52 / 4 = 13
     EQUB 22                \ Visibility distance      = 22
     EQUB 32                \ Max. energy              = 32
     EQUB 8                 \ Max. speed               = 8
    
     EQUB HI(SHIP_SHUTTLE_EDGES - SHIP_SHUTTLE)        \ Edges data offset (high)
     EQUB HI(SHIP_SHUTTLE_FACES - SHIP_SHUTTLE)        \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_SHUTTLE_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,  -17,   23,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX  -17,    0,   23,    15,     15,   15,    15,         31    \ Vertex 1
     VERTEX    0,   18,   23,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX   18,    0,   23,    15,     15,   15,    15,         31    \ Vertex 3
     VERTEX  -20,  -20,  -27,     2,      1,    9,     3,         31    \ Vertex 4
     VERTEX  -20,   20,  -27,     4,      3,    9,     5,         31    \ Vertex 5
     VERTEX   20,   20,  -27,     6,      5,    9,     7,         31    \ Vertex 6
     VERTEX   20,  -20,  -27,     7,      1,    9,     8,         31    \ Vertex 7
     VERTEX    5,    0,  -27,     9,      9,    9,     9,         16    \ Vertex 8
     VERTEX    0,   -2,  -27,     9,      9,    9,     9,         16    \ Vertex 9
     VERTEX   -5,    0,  -27,     9,      9,    9,     9,          9    \ Vertex 10
     VERTEX    0,    3,  -27,     9,      9,    9,     9,          9    \ Vertex 11
     VERTEX    0,   -9,   35,    10,      0,   12,    11,         16    \ Vertex 12
     VERTEX    3,   -1,   31,    15,     15,    2,     0,          7    \ Vertex 13
     VERTEX    4,   11,   25,     1,      0,    4,    15,          8    \ Vertex 14
     VERTEX   11,    4,   25,     1,     10,   15,     3,          8    \ Vertex 15
     VERTEX   -3,   -1,   31,    11,      6,    3,     2,          7    \ Vertex 16
     VERTEX   -3,   11,   25,     8,     15,    0,    12,          8    \ Vertex 17
     VERTEX  -10,    4,   25,    15,      4,    8,     1,          8    \ Vertex 18
    
    .SHIP_SHUTTLE_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     2,     0,         31    \ Edge 0
     EDGE       1,       2,    10,     4,         31    \ Edge 1
     EDGE       2,       3,    11,     6,         31    \ Edge 2
     EDGE       0,       3,    12,     8,         31    \ Edge 3
     EDGE       0,       7,     8,     1,         31    \ Edge 4
     EDGE       0,       4,     2,     1,         24    \ Edge 5
     EDGE       1,       4,     3,     2,         31    \ Edge 6
     EDGE       1,       5,     4,     3,         24    \ Edge 7
     EDGE       2,       5,     5,     4,         31    \ Edge 8
     EDGE       2,       6,     6,     5,         12    \ Edge 9
     EDGE       3,       6,     7,     6,         31    \ Edge 10
     EDGE       3,       7,     8,     7,         24    \ Edge 11
     EDGE       4,       5,     9,     3,         31    \ Edge 12
     EDGE       5,       6,     9,     5,         31    \ Edge 13
     EDGE       6,       7,     9,     7,         31    \ Edge 14
     EDGE       4,       7,     9,     1,         31    \ Edge 15
     EDGE       0,      12,    12,     0,         16    \ Edge 16
     EDGE       1,      12,    10,     0,         16    \ Edge 17
     EDGE       2,      12,    11,    10,         16    \ Edge 18
     EDGE       3,      12,    12,    11,         16    \ Edge 19
     EDGE       8,       9,     9,     9,         16    \ Edge 20
     EDGE       9,      10,     9,     9,          7    \ Edge 21
     EDGE      10,      11,     9,     9,          9    \ Edge 22
     EDGE       8,      11,     9,     9,          7    \ Edge 23
     EDGE      13,      14,    11,    11,          5    \ Edge 24
     EDGE      14,      15,    11,    11,          8    \ Edge 25
     EDGE      13,      15,    11,    11,          7    \ Edge 26
     EDGE      16,      17,    10,    10,          5    \ Edge 27
     EDGE      17,      18,    10,    10,          8    \ Edge 28
     EDGE      16,      18,    10,    10,          7    \ Edge 29
    
    .SHIP_SHUTTLE_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE      -55,      -55,       40,         31      \ Face 0
     FACE        0,      -74,        4,         31      \ Face 1
     FACE      -51,      -51,       23,         31      \ Face 2
     FACE      -74,        0,        4,         31      \ Face 3
     FACE      -51,       51,       23,         31      \ Face 4
     FACE        0,       74,        4,         31      \ Face 5
     FACE       51,       51,       23,         31      \ Face 6
     FACE       74,        0,        4,         31      \ Face 7
     FACE       51,      -51,       23,         31      \ Face 8
     FACE        0,        0,     -107,         31      \ Face 9
     FACE      -41,       41,       90,         31      \ Face 10
     FACE       41,       41,       90,         31      \ Face 11
     FACE       55,      -55,       40,         31      \ Face 12
    
    
    
    
           Name: SHIP_TRANSPORTER                                        [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Transporter
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_transporter.md)
     Variations: See [code variations](../related/compare/main/variable/ship_transporter.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_TRANSPORTER
    
    
    
    
    
    
    .SHIP_TRANSPORTER
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 50 * 50           \ Targetable area          = 50 * 50
    
     EQUB LO(SHIP_TRANSPORTER_EDGES - SHIP_TRANSPORTER)   \ Edges data offset (low)
     EQUB LO(SHIP_TRANSPORTER_FACES - SHIP_TRANSPORTER)   \ Faces data offset (low)
    
     EQUB 149               \ Max. edge count          = (149 - 1) / 4 = 37
     EQUB 48                \ Gun vertex               = 48 / 4 = 12
     EQUB 26                \ Explosion count          = 5, as (4 * n) + 6 = 26
     EQUB 222               \ Number of vertices       = 222 / 6 = 37
     EQUB 46                \ Number of edges          = 46
     EQUW 0                 \ Bounty                   = 0
     EQUB 56                \ Number of faces          = 56 / 4 = 14
     EQUB 16                \ Visibility distance      = 16
     EQUB 32                \ Max. energy              = 32
     EQUB 10                \ Max. speed               = 10
    
     EQUB HI(SHIP_TRANSPORTER_EDGES - SHIP_TRANSPORTER)   \ Edges data offset (high)
     EQUB HI(SHIP_TRANSPORTER_FACES - SHIP_TRANSPORTER)   \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_TRANSPORTER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,   10,  -26,     6,      0,    7,     7,         31    \ Vertex 0
     VERTEX  -25,    4,  -26,     1,      0,    7,     7,         31    \ Vertex 1
     VERTEX  -28,   -3,  -26,     1,      0,    2,     2,         31    \ Vertex 2
     VERTEX  -25,   -8,  -26,     2,      0,    3,     3,         31    \ Vertex 3
     VERTEX   26,   -8,  -26,     3,      0,    4,     4,         31    \ Vertex 4
     VERTEX   29,   -3,  -26,     4,      0,    5,     5,         31    \ Vertex 5
     VERTEX   26,    4,  -26,     5,      0,    6,     6,         31    \ Vertex 6
     VERTEX    0,    6,   12,    15,     15,   15,    15,         19    \ Vertex 7
     VERTEX  -30,   -1,   12,     7,      1,    9,     8,         31    \ Vertex 8
     VERTEX  -33,   -8,   12,     2,      1,    9,     3,         31    \ Vertex 9
     VERTEX   33,   -8,   12,     4,      3,   10,     5,         31    \ Vertex 10
     VERTEX   30,   -1,   12,     6,      5,   11,    10,         31    \ Vertex 11
     VERTEX  -11,   -2,   30,     9,      8,   13,    12,         31    \ Vertex 12
     VERTEX  -13,   -8,   30,     9,      3,   13,    13,         31    \ Vertex 13
     VERTEX   14,   -8,   30,    10,      3,   13,    13,         31    \ Vertex 14
     VERTEX   11,   -2,   30,    11,     10,   13,    12,         31    \ Vertex 15
     VERTEX   -5,    6,    2,     7,      7,    7,     7,          7    \ Vertex 16
     VERTEX  -18,    3,    2,     7,      7,    7,     7,          7    \ Vertex 17
     VERTEX   -5,    7,   -7,     7,      7,    7,     7,          7    \ Vertex 18
     VERTEX  -18,    4,   -7,     7,      7,    7,     7,          7    \ Vertex 19
     VERTEX  -11,    6,  -14,     7,      7,    7,     7,          7    \ Vertex 20
     VERTEX  -11,    5,   -7,     7,      7,    7,     7,          7    \ Vertex 21
     VERTEX    5,    7,  -14,     6,      6,    6,     6,          7    \ Vertex 22
     VERTEX   18,    4,  -14,     6,      6,    6,     6,          7    \ Vertex 23
     VERTEX   11,    5,   -7,     6,      6,    6,     6,          7    \ Vertex 24
     VERTEX    5,    6,   -3,     6,      6,    6,     6,          7    \ Vertex 25
     VERTEX   18,    3,   -3,     6,      6,    6,     6,          7    \ Vertex 26
     VERTEX   11,    4,    8,     6,      6,    6,     6,          7    \ Vertex 27
     VERTEX   11,    5,   -3,     6,      6,    6,     6,          7    \ Vertex 28
     VERTEX  -16,   -8,  -13,     3,      3,    3,     3,          6    \ Vertex 29
     VERTEX  -16,   -8,   16,     3,      3,    3,     3,          6    \ Vertex 30
     VERTEX   17,   -8,  -13,     3,      3,    3,     3,          6    \ Vertex 31
     VERTEX   17,   -8,   16,     3,      3,    3,     3,          6    \ Vertex 32
     VERTEX  -13,   -3,  -26,     0,      0,    0,     0,          8    \ Vertex 33
     VERTEX   13,   -3,  -26,     0,      0,    0,     0,          8    \ Vertex 34
     VERTEX    9,    3,  -26,     0,      0,    0,     0,          5    \ Vertex 35
     VERTEX   -8,    3,  -26,     0,      0,    0,     0,          5    \ Vertex 36
    
    .SHIP_TRANSPORTER_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     7,     0,         31    \ Edge 0
     EDGE       1,       2,     1,     0,         31    \ Edge 1
     EDGE       2,       3,     2,     0,         31    \ Edge 2
     EDGE       3,       4,     3,     0,         31    \ Edge 3
     EDGE       4,       5,     4,     0,         31    \ Edge 4
     EDGE       5,       6,     5,     0,         31    \ Edge 5
     EDGE       0,       6,     6,     0,         31    \ Edge 6
     EDGE       0,       7,     7,     6,         16    \ Edge 7
     EDGE       1,       8,     7,     1,         31    \ Edge 8
     EDGE       2,       9,     2,     1,         11    \ Edge 9
     EDGE       3,       9,     3,     2,         31    \ Edge 10
     EDGE       4,      10,     4,     3,         31    \ Edge 11
     EDGE       5,      10,     5,     4,         11    \ Edge 12
     EDGE       6,      11,     6,     5,         31    \ Edge 13
     EDGE       7,       8,     8,     7,         17    \ Edge 14
     EDGE       8,       9,     9,     1,         17    \ Edge 15
     EDGE      10,      11,    10,     5,         17    \ Edge 16
     EDGE       7,      11,    11,     6,         17    \ Edge 17
     EDGE       7,      15,    12,    11,         19    \ Edge 18
     EDGE       7,      12,    12,     8,         19    \ Edge 19
     EDGE       8,      12,     9,     8,         16    \ Edge 20
     EDGE       9,      13,     9,     3,         31    \ Edge 21
     EDGE      10,      14,    10,     3,         31    \ Edge 22
     EDGE      11,      15,    11,    10,         16    \ Edge 23
     EDGE      12,      13,    13,     9,         31    \ Edge 24
     EDGE      13,      14,    13,     3,         31    \ Edge 25
     EDGE      14,      15,    13,    10,         31    \ Edge 26
     EDGE      12,      15,    13,    12,         31    \ Edge 27
     EDGE      16,      17,     7,     7,          7    \ Edge 28
     EDGE      18,      19,     7,     7,          7    \ Edge 29
     EDGE      19,      20,     7,     7,          7    \ Edge 30
     EDGE      18,      20,     7,     7,          7    \ Edge 31
     EDGE      20,      21,     7,     7,          7    \ Edge 32
     EDGE      22,      23,     6,     6,          7    \ Edge 33
     EDGE      23,      24,     6,     6,          7    \ Edge 34
     EDGE      24,      22,     6,     6,          7    \ Edge 35
     EDGE      25,      26,     6,     6,          7    \ Edge 36
     EDGE      26,      27,     6,     6,          7    \ Edge 37
     EDGE      25,      27,     6,     6,          7    \ Edge 38
     EDGE      27,      28,     6,     6,          7    \ Edge 39
     EDGE      29,      30,     3,     3,          6    \ Edge 40
     EDGE      31,      32,     3,     3,          6    \ Edge 41
     EDGE      33,      34,     0,     0,          8    \ Edge 42
     EDGE      34,      35,     0,     0,          5    \ Edge 43
     EDGE      35,      36,     0,     0,          5    \ Edge 44
     EDGE      36,      33,     0,     0,          5    \ Edge 45
    
    .SHIP_TRANSPORTER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,        0,     -103,         31      \ Face 0
     FACE     -111,       48,       -7,         31      \ Face 1
     FACE     -105,      -63,      -21,         31      \ Face 2
     FACE        0,      -34,        0,         31      \ Face 3
     FACE      105,      -63,      -21,         31      \ Face 4
     FACE      111,       48,       -7,         31      \ Face 5
     FACE        8,       32,        3,         31      \ Face 6
     FACE       -8,       32,        3,         31      \ Face 7
     FACE       -8,       34,       11,         19      \ Face 8
     FACE      -75,       32,       79,         31      \ Face 9
     FACE       75,       32,       79,         31      \ Face 10
     FACE        8,       34,       11,         19      \ Face 11
     FACE        0,       38,       17,         31      \ Face 12
     FACE        0,        0,      121,         31      \ Face 13
    
    
    
    
           Name: SHIP_COBRA_MK_3                                         [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Cobra Mk III
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_cobra_mk_3.md)
     Variations: See [code variations](../related/compare/main/variable/ship_cobra_mk_3.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_COBRA_MK_3
    
    
    
    
    
    
    .SHIP_COBRA_MK_3
    
     EQUB 3                 \ Max. canisters on demise = 3
     EQUW 95 * 95           \ Targetable area          = 95 * 95
    
     EQUB LO(SHIP_COBRA_MK_3_EDGES - SHIP_COBRA_MK_3)  \ Edges data offset (low)
     EQUB LO(SHIP_COBRA_MK_3_FACES - SHIP_COBRA_MK_3)  \ Faces data offset (low)
    
     EQUB 157               \ Max. edge count          = (157 - 1) / 4 = 39
     EQUB 84                \ Gun vertex               = 84 / 4 = 21
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 168               \ Number of vertices       = 168 / 6 = 28
     EQUB 38                \ Number of edges          = 38
     EQUW 0                 \ Bounty                   = 0
     EQUB 52                \ Number of faces          = 52 / 4 = 13
     EQUB 50                \ Visibility distance      = 50
     EQUB 150               \ Max. energy              = 150
     EQUB 28                \ Max. speed               = 28
    
     EQUB HI(SHIP_COBRA_MK_3_EDGES - SHIP_COBRA_MK_3)  \ Edges data offset (low)
     EQUB HI(SHIP_COBRA_MK_3_FACES - SHIP_COBRA_MK_3)  \ Faces data offset (low)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00010011         \ Laser power              = 2
                            \ Missiles                 = 3
    
    .SHIP_COBRA_MK_3_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   32,    0,   76,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX  -32,    0,   76,    15,     15,   15,    15,         31    \ Vertex 1
     VERTEX    0,   26,   24,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX -120,   -3,   -8,     3,      7,   10,    10,         31    \ Vertex 3
     VERTEX  120,   -3,   -8,     4,      8,   12,    12,         31    \ Vertex 4
     VERTEX  -88,   16,  -40,    15,     15,   15,    15,         31    \ Vertex 5
     VERTEX   88,   16,  -40,    15,     15,   15,    15,         31    \ Vertex 6
     VERTEX  128,   -8,  -40,     8,      9,   12,    12,         31    \ Vertex 7
     VERTEX -128,   -8,  -40,     7,      9,   10,    10,         31    \ Vertex 8
     VERTEX    0,   26,  -40,     5,      6,    9,     9,         31    \ Vertex 9
     VERTEX  -32,  -24,  -40,     9,     10,   11,    11,         31    \ Vertex 10
     VERTEX   32,  -24,  -40,     9,     11,   12,    12,         31    \ Vertex 11
     VERTEX  -36,    8,  -40,     9,      9,    9,     9,         20    \ Vertex 12
     VERTEX   -8,   12,  -40,     9,      9,    9,     9,         20    \ Vertex 13
     VERTEX    8,   12,  -40,     9,      9,    9,     9,         20    \ Vertex 14
     VERTEX   36,    8,  -40,     9,      9,    9,     9,         20    \ Vertex 15
     VERTEX   36,  -12,  -40,     9,      9,    9,     9,         20    \ Vertex 16
     VERTEX    8,  -16,  -40,     9,      9,    9,     9,         20    \ Vertex 17
     VERTEX   -8,  -16,  -40,     9,      9,    9,     9,         20    \ Vertex 18
     VERTEX  -36,  -12,  -40,     9,      9,    9,     9,         20    \ Vertex 19
     VERTEX    0,    0,   76,     0,     11,   11,    11,          6    \ Vertex 20
     VERTEX    0,    0,   90,     0,     11,   11,    11,         31    \ Vertex 21
     VERTEX  -80,   -6,  -40,     9,      9,    9,     9,          8    \ Vertex 22
     VERTEX  -80,    6,  -40,     9,      9,    9,     9,          8    \ Vertex 23
     VERTEX  -88,    0,  -40,     9,      9,    9,     9,          6    \ Vertex 24
     VERTEX   80,    6,  -40,     9,      9,    9,     9,          8    \ Vertex 25
     VERTEX   88,    0,  -40,     9,      9,    9,     9,          6    \ Vertex 26
     VERTEX   80,   -6,  -40,     9,      9,    9,     9,          8    \ Vertex 27
    
    .SHIP_COBRA_MK_3_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     0,    11,         31    \ Edge 0
     EDGE       0,       4,     4,    12,         31    \ Edge 1
     EDGE       1,       3,     3,    10,         31    \ Edge 2
     EDGE       3,       8,     7,    10,         31    \ Edge 3
     EDGE       4,       7,     8,    12,         31    \ Edge 4
     EDGE       6,       7,     8,     9,         31    \ Edge 5
     EDGE       6,       9,     6,     9,         31    \ Edge 6
     EDGE       5,       9,     5,     9,         31    \ Edge 7
     EDGE       5,       8,     7,     9,         31    \ Edge 8
     EDGE       2,       5,     1,     5,         31    \ Edge 9
     EDGE       2,       6,     2,     6,         31    \ Edge 10
     EDGE       3,       5,     3,     7,         31    \ Edge 11
     EDGE       4,       6,     4,     8,         31    \ Edge 12
     EDGE       1,       2,     0,     1,         31    \ Edge 13
     EDGE       0,       2,     0,     2,         31    \ Edge 14
     EDGE       8,      10,     9,    10,         31    \ Edge 15
     EDGE      10,      11,     9,    11,         31    \ Edge 16
     EDGE       7,      11,     9,    12,         31    \ Edge 17
     EDGE       1,      10,    10,    11,         31    \ Edge 18
     EDGE       0,      11,    11,    12,         31    \ Edge 19
     EDGE       1,       5,     1,     3,         29    \ Edge 20
     EDGE       0,       6,     2,     4,         29    \ Edge 21
     EDGE      20,      21,     0,    11,          6    \ Edge 22
     EDGE      12,      13,     9,     9,         20    \ Edge 23
     EDGE      18,      19,     9,     9,         20    \ Edge 24
     EDGE      14,      15,     9,     9,         20    \ Edge 25
     EDGE      16,      17,     9,     9,         20    \ Edge 26
     EDGE      15,      16,     9,     9,         19    \ Edge 27
     EDGE      14,      17,     9,     9,         17    \ Edge 28
     EDGE      13,      18,     9,     9,         19    \ Edge 29
     EDGE      12,      19,     9,     9,         19    \ Edge 30
     EDGE       2,       9,     5,     6,         30    \ Edge 31
     EDGE      22,      24,     9,     9,          6    \ Edge 32
     EDGE      23,      24,     9,     9,          6    \ Edge 33
     EDGE      22,      23,     9,     9,          8    \ Edge 34
     EDGE      25,      26,     9,     9,          6    \ Edge 35
     EDGE      26,      27,     9,     9,          6    \ Edge 36
     EDGE      25,      27,     9,     9,          8    \ Edge 37
    
    .SHIP_COBRA_MK_3_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       62,       31,         31      \ Face 0
     FACE      -18,       55,       16,         31      \ Face 1
     FACE       18,       55,       16,         31      \ Face 2
     FACE      -16,       52,       14,         31      \ Face 3
     FACE       16,       52,       14,         31      \ Face 4
     FACE      -14,       47,        0,         31      \ Face 5
     FACE       14,       47,        0,         31      \ Face 6
     FACE      -61,      102,        0,         31      \ Face 7
     FACE       61,      102,        0,         31      \ Face 8
     FACE        0,        0,      -80,         31      \ Face 9
     FACE       -7,      -42,        9,         31      \ Face 10
     FACE        0,      -30,        6,         31      \ Face 11
     FACE        7,      -42,        9,         31      \ Face 12
    
    
    
    
           Name: SHIP_PYTHON                                             [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Python
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_python.md)
     Variations: See [code variations](../related/compare/main/variable/ship_python.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_PYTHON
    
    
    
    
    
    
    .SHIP_PYTHON
    
     EQUB 5                 \ Max. canisters on demise = 5
     EQUW 80 * 80           \ Targetable area          = 80 * 80
    
     EQUB LO(SHIP_PYTHON_EDGES - SHIP_PYTHON)          \ Edges data offset (low)
     EQUB LO(SHIP_PYTHON_FACES - SHIP_PYTHON)          \ Faces data offset (low)
    
     EQUB 89                \ Max. edge count          = (89 - 1) / 4 = 22
     EQUB 0                 \ Gun vertex               = 0
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 66                \ Number of vertices       = 66 / 6 = 11
     EQUB 26                \ Number of edges          = 26
     EQUW 0                 \ Bounty                   = 0
     EQUB 52                \ Number of faces          = 52 / 4 = 13
     EQUB 40                \ Visibility distance      = 40
     EQUB 250               \ Max. energy              = 250
     EQUB 20                \ Max. speed               = 20
    
     EQUB HI(SHIP_PYTHON_EDGES - SHIP_PYTHON)          \ Edges data offset (high)
     EQUB HI(SHIP_PYTHON_FACES - SHIP_PYTHON)          \ Faces data offset (high)
    
     EQUB 0                 \ Normals are scaled by    = 2^0 = 1
     EQUB %00011011         \ Laser power              = 3
                            \ Missiles                 = 3
    
    .SHIP_PYTHON_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,  224,     0,      1,    2,     3,         31    \ Vertex 0
     VERTEX    0,   48,   48,     0,      1,    4,     5,         31    \ Vertex 1
     VERTEX   96,    0,  -16,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX  -96,    0,  -16,    15,     15,   15,    15,         31    \ Vertex 3
     VERTEX    0,   48,  -32,     4,      5,    8,     9,         31    \ Vertex 4
     VERTEX    0,   24, -112,     9,      8,   12,    12,         31    \ Vertex 5
     VERTEX  -48,    0, -112,     8,     11,   12,    12,         31    \ Vertex 6
     VERTEX   48,    0, -112,     9,     10,   12,    12,         31    \ Vertex 7
     VERTEX    0,  -48,   48,     2,      3,    6,     7,         31    \ Vertex 8
     VERTEX    0,  -48,  -32,     6,      7,   10,    11,         31    \ Vertex 9
     VERTEX    0,  -24, -112,    10,     11,   12,    12,         31    \ Vertex 10
    
    .SHIP_PYTHON_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       8,     2,     3,         31    \ Edge 0
     EDGE       0,       3,     0,     2,         31    \ Edge 1
     EDGE       0,       2,     1,     3,         31    \ Edge 2
     EDGE       0,       1,     0,     1,         31    \ Edge 3
     EDGE       2,       4,     9,     5,         31    \ Edge 4
     EDGE       1,       2,     1,     5,         31    \ Edge 5
     EDGE       2,       8,     7,     3,         31    \ Edge 6
     EDGE       1,       3,     0,     4,         31    \ Edge 7
     EDGE       3,       8,     2,     6,         31    \ Edge 8
     EDGE       2,       9,     7,    10,         31    \ Edge 9
     EDGE       3,       4,     4,     8,         31    \ Edge 10
     EDGE       3,       9,     6,    11,         31    \ Edge 11
     EDGE       3,       5,     8,     8,          7    \ Edge 12
     EDGE       3,      10,    11,    11,          7    \ Edge 13
     EDGE       2,       5,     9,     9,          7    \ Edge 14
     EDGE       2,      10,    10,    10,          7    \ Edge 15
     EDGE       2,       7,     9,    10,         31    \ Edge 16
     EDGE       3,       6,     8,    11,         31    \ Edge 17
     EDGE       5,       6,     8,    12,         31    \ Edge 18
     EDGE       5,       7,     9,    12,         31    \ Edge 19
     EDGE       7,      10,    12,    10,         31    \ Edge 20
     EDGE       6,      10,    11,    12,         31    \ Edge 21
     EDGE       4,       5,     8,     9,         31    \ Edge 22
     EDGE       9,      10,    10,    11,         31    \ Edge 23
     EDGE       1,       4,     4,     5,         31    \ Edge 24
     EDGE       8,       9,     6,     7,         31    \ Edge 25
    
    .SHIP_PYTHON_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE      -27,       40,       11,        31    \ Face 0
     FACE       27,       40,       11,        31    \ Face 1
     FACE      -27,      -40,       11,        31    \ Face 2
     FACE       27,      -40,       11,        31    \ Face 3
     FACE      -19,       38,        0,        31    \ Face 4
     FACE       19,       38,        0,        31    \ Face 5
     FACE      -19,      -38,        0,        31    \ Face 6
     FACE       19,      -38,        0,        31    \ Face 7
     FACE      -25,       37,      -11,        31    \ Face 8
     FACE       25,       37,      -11,        31    \ Face 9
     FACE       25,      -37,      -11,        31    \ Face 10
     FACE      -25,      -37,      -11,        31    \ Face 11
     FACE        0,        0,     -112,        31    \ Face 12
    
    
    
    
           Name: SHIP_BOA                                                [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Boa
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_boa.md)
     Variations: See [code variations](../related/compare/main/variable/ship_boa.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_BOA
    
    
    
    
    
    
    .SHIP_BOA
    
     EQUB 5                 \ Max. canisters on demise = 5
     EQUW 70 * 70           \ Targetable area          = 70 * 70
    
     EQUB LO(SHIP_BOA_EDGES - SHIP_BOA)                \ Edges data offset (low)
     EQUB LO(SHIP_BOA_FACES - SHIP_BOA)                \ Faces data offset (low)
    
     EQUB 93                \ Max. edge count          = (93 - 1) / 4 = 23
     EQUB 0                 \ Gun vertex               = 0
     EQUB 38                \ Explosion count          = 8, as (4 * n) + 6 = 38
     EQUB 78                \ Number of vertices       = 78 / 6 = 13
     EQUB 24                \ Number of edges          = 24
     EQUW 0                 \ Bounty                   = 0
     EQUB 52                \ Number of faces          = 52 / 4 = 13
     EQUB 40                \ Visibility distance      = 40
     EQUB 250               \ Max. energy              = 250
     EQUB 24                \ Max. speed               = 24
    
     EQUB HI(SHIP_BOA_EDGES - SHIP_BOA)                \ Edges data offset (high)
     EQUB HI(SHIP_BOA_FACES - SHIP_BOA)                \ Faces data offset (high)
    
     EQUB 0                 \ Normals are scaled by    = 2^0 = 1
     EQUB %00011100         \ Laser power              = 3
                            \ Missiles                 = 4
    
    .SHIP_BOA_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,   93,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX    0,   40,  -87,     2,      0,    3,     3,         24    \ Vertex 1
     VERTEX   38,  -25,  -99,     1,      0,    4,     4,         24    \ Vertex 2
     VERTEX  -38,  -25,  -99,     2,      1,    5,     5,         24    \ Vertex 3
     VERTEX  -38,   40,  -59,     3,      2,    9,     6,         31    \ Vertex 4
     VERTEX   38,   40,  -59,     3,      0,   11,     6,         31    \ Vertex 5
     VERTEX   62,    0,  -67,     4,      0,   11,     8,         31    \ Vertex 6
     VERTEX   24,  -65,  -79,     4,      1,   10,     8,         31    \ Vertex 7
     VERTEX  -24,  -65,  -79,     5,      1,   10,     7,         31    \ Vertex 8
     VERTEX  -62,    0,  -67,     5,      2,    9,     7,         31    \ Vertex 9
     VERTEX    0,    7, -107,     2,      0,   10,    10,         22    \ Vertex 10
     VERTEX   13,   -9, -107,     1,      0,   10,    10,         22    \ Vertex 11
     VERTEX  -13,   -9, -107,     2,      1,   12,    12,         22    \ Vertex 12
    
    .SHIP_BOA_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       5,    11,     6,         31    \ Edge 0
     EDGE       0,       7,    10,     8,         31    \ Edge 1
     EDGE       0,       9,     9,     7,         31    \ Edge 2
     EDGE       0,       4,     9,     6,         29    \ Edge 3
     EDGE       0,       6,    11,     8,         29    \ Edge 4
     EDGE       0,       8,    10,     7,         29    \ Edge 5
     EDGE       4,       5,     6,     3,         31    \ Edge 6
     EDGE       5,       6,    11,     0,         31    \ Edge 7
     EDGE       6,       7,     8,     4,         31    \ Edge 8
     EDGE       7,       8,    10,     1,         31    \ Edge 9
     EDGE       8,       9,     7,     5,         31    \ Edge 10
     EDGE       4,       9,     9,     2,         31    \ Edge 11
     EDGE       1,       4,     3,     2,         24    \ Edge 12
     EDGE       1,       5,     3,     0,         24    \ Edge 13
     EDGE       3,       9,     5,     2,         24    \ Edge 14
     EDGE       3,       8,     5,     1,         24    \ Edge 15
     EDGE       2,       6,     4,     0,         24    \ Edge 16
     EDGE       2,       7,     4,     1,         24    \ Edge 17
     EDGE       1,      10,     2,     0,         22    \ Edge 18
     EDGE       2,      11,     1,     0,         22    \ Edge 19
     EDGE       3,      12,     2,     1,         22    \ Edge 20
     EDGE      10,      11,    12,     0,         14    \ Edge 21
     EDGE      11,      12,    12,     1,         14    \ Edge 22
     EDGE      12,      10,    12,     2,         14    \ Edge 23
    
    .SHIP_BOA_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE       43,       37,      -60,         31      \ Face 0
     FACE        0,      -45,      -89,         31      \ Face 1
     FACE      -43,       37,      -60,         31      \ Face 2
     FACE        0,       40,        0,         31      \ Face 3
     FACE       62,      -32,      -20,         31      \ Face 4
     FACE      -62,      -32,      -20,         31      \ Face 5
     FACE        0,       23,        6,         31      \ Face 6
     FACE      -23,      -15,        9,         31      \ Face 7
     FACE       23,      -15,        9,         31      \ Face 8
     FACE      -26,       13,       10,         31      \ Face 9
     FACE        0,      -31,       12,         31      \ Face 10
     FACE       26,       13,       10,         31      \ Face 11
     FACE        0,        0,     -107,         14      \ Face 12
    
    
    
    
           Name: SHIP_ANACONDA                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an Anaconda
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_anaconda.md)
     Variations: See [code variations](../related/compare/main/variable/ship_anaconda.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_ANACONDA
    
    
    
    
    
    
    .SHIP_ANACONDA
    
     EQUB 7                 \ Max. canisters on demise = 7
     EQUW 100 * 100         \ Targetable area          = 100 * 100
    
     EQUB LO(SHIP_ANACONDA_EDGES - SHIP_ANACONDA)      \ Edges data offset (low)
     EQUB LO(SHIP_ANACONDA_FACES - SHIP_ANACONDA)      \ Faces data offset (low)
    
     EQUB 93                \ Max. edge count          = (93 - 1) / 4 = 23
     EQUB 48                \ Gun vertex               = 48 / 4 = 12
     EQUB 46                \ Explosion count          = 10, as (4 * n) + 6 = 46
     EQUB 90                \ Number of vertices       = 90 / 6 = 15
     EQUB 25                \ Number of edges          = 25
     EQUW 0                 \ Bounty                   = 0
     EQUB 48                \ Number of faces          = 48 / 4 = 12
     EQUB 36                \ Visibility distance      = 36
     EQUB 252               \ Max. energy              = 252
     EQUB 14                \ Max. speed               = 14
    
     EQUB HI(SHIP_ANACONDA_EDGES - SHIP_ANACONDA)      \ Edges data offset (high)
     EQUB HI(SHIP_ANACONDA_FACES - SHIP_ANACONDA)      \ Faces data offset (high)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00111111         \ Laser power              = 7
                            \ Missiles                 = 7
    
    .SHIP_ANACONDA_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    7,  -58,     1,      0,    5,     5,         30    \ Vertex 0
     VERTEX  -43,  -13,  -37,     1,      0,    2,     2,         30    \ Vertex 1
     VERTEX  -26,  -47,   -3,     2,      0,    3,     3,         30    \ Vertex 2
     VERTEX   26,  -47,   -3,     3,      0,    4,     4,         30    \ Vertex 3
     VERTEX   43,  -13,  -37,     4,      0,    5,     5,         30    \ Vertex 4
     VERTEX    0,   48,  -49,     5,      1,    6,     6,         30    \ Vertex 5
     VERTEX  -69,   15,  -15,     2,      1,    7,     7,         30    \ Vertex 6
     VERTEX  -43,  -39,   40,     3,      2,    8,     8,         31    \ Vertex 7
     VERTEX   43,  -39,   40,     4,      3,    9,     9,         31    \ Vertex 8
     VERTEX   69,   15,  -15,     5,      4,   10,    10,         30    \ Vertex 9
     VERTEX  -43,   53,  -23,    15,     15,   15,    15,         31    \ Vertex 10
     VERTEX  -69,   -1,   32,     7,      2,    8,     8,         31    \ Vertex 11
     VERTEX    0,    0,  254,    15,     15,   15,    15,         31    \ Vertex 12
     VERTEX   69,   -1,   32,     9,      4,   10,    10,         31    \ Vertex 13
     VERTEX   43,   53,  -23,    15,     15,   15,    15,         31    \ Vertex 14
    
    .SHIP_ANACONDA_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     1,     0,         30    \ Edge 0
     EDGE       1,       2,     2,     0,         30    \ Edge 1
     EDGE       2,       3,     3,     0,         30    \ Edge 2
     EDGE       3,       4,     4,     0,         30    \ Edge 3
     EDGE       0,       4,     5,     0,         30    \ Edge 4
     EDGE       0,       5,     5,     1,         29    \ Edge 5
     EDGE       1,       6,     2,     1,         29    \ Edge 6
     EDGE       2,       7,     3,     2,         29    \ Edge 7
     EDGE       3,       8,     4,     3,         29    \ Edge 8
     EDGE       4,       9,     5,     4,         29    \ Edge 9
     EDGE       5,      10,     6,     1,         30    \ Edge 10
     EDGE       6,      10,     7,     1,         30    \ Edge 11
     EDGE       6,      11,     7,     2,         30    \ Edge 12
     EDGE       7,      11,     8,     2,         30    \ Edge 13
     EDGE       7,      12,     8,     3,         31    \ Edge 14
     EDGE       8,      12,     9,     3,         31    \ Edge 15
     EDGE       8,      13,     9,     4,         30    \ Edge 16
     EDGE       9,      13,    10,     4,         30    \ Edge 17
     EDGE       9,      14,    10,     5,         30    \ Edge 18
     EDGE       5,      14,     6,     5,         30    \ Edge 19
     EDGE      10,      14,    11,     6,         30    \ Edge 20
     EDGE      10,      12,    11,     7,         31    \ Edge 21
     EDGE      11,      12,     8,     7,         31    \ Edge 22
     EDGE      12,      13,    10,     9,         31    \ Edge 23
     EDGE      12,      14,    11,    10,         31    \ Edge 24
    
    .SHIP_ANACONDA_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,      -51,      -49,         30      \ Face 0
     FACE      -51,       18,      -87,         30      \ Face 1
     FACE      -77,      -57,      -19,         30      \ Face 2
     FACE        0,      -90,       16,         31      \ Face 3
     FACE       77,      -57,      -19,         30      \ Face 4
     FACE       51,       18,      -87,         30      \ Face 5
     FACE        0,      111,      -20,         30      \ Face 6
     FACE      -97,       72,       24,         31      \ Face 7
     FACE     -108,      -68,       34,         31      \ Face 8
     FACE      108,      -68,       34,         31      \ Face 9
     FACE       97,       72,       24,         31      \ Face 10
     FACE        0,       94,       18,         31      \ Face 11
    
    
    
    
           Name: SHIP_ROCK_HERMIT                                        [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a rock hermit (asteroid)
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_rock_hermit.md)
     Variations: See [code variations](../related/compare/main/variable/ship_rock_hermit.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_ROCK_HERMIT
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the rock hermit reuses the edges and faces data from
     the asteroid, so the edges and faces data offsets are negative.
    
    
    
    
    .SHIP_ROCK_HERMIT
    
     EQUB 7                 \ Max. canisters on demise = 7
     EQUW 80 * 80           \ Targetable area          = 80 * 80
    
     EQUB LO(SHIP_ASTEROID_EDGES - SHIP_ROCK_HERMIT)   \ Edges from asteroid
     EQUB LO(SHIP_ASTEROID_FACES - SHIP_ROCK_HERMIT)   \ Faces from asteroid
    
     EQUB 69                \ Max. edge count          = (69 - 1) / 4 = 17
     EQUB 0                 \ Gun vertex               = 0
     EQUB 50                \ Explosion count          = 11, as (4 * n) + 6 = 50
     EQUB 54                \ Number of vertices       = 54 / 6 = 9
     EQUB 21                \ Number of edges          = 21
     EQUW 0                 \ Bounty                   = 0
     EQUB 56                \ Number of faces          = 56 / 4 = 14
     EQUB 50                \ Visibility distance      = 50
     EQUB 180               \ Max. energy              = 180
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_ASTEROID_EDGES - SHIP_ROCK_HERMIT)   \ Edges from asteroid
     EQUB HI(SHIP_ASTEROID_FACES - SHIP_ROCK_HERMIT)   \ Faces from asteroid
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00000010         \ Laser power              = 0
                            \ Missiles                 = 2
    
    .SHIP_ROCK_HERMIT_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,   80,    0,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX  -80,  -10,    0,    15,     15,   15,    15,         31    \ Vertex 1
     VERTEX    0,  -80,    0,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX   70,  -40,    0,    15,     15,   15,    15,         31    \ Vertex 3
     VERTEX   60,   50,    0,     5,      6,   12,    13,         31    \ Vertex 4
     VERTEX   50,    0,   60,    15,     15,   15,    15,         31    \ Vertex 5
     VERTEX  -40,    0,   70,     0,      1,    2,     3,         31    \ Vertex 6
     VERTEX    0,   30,  -75,    15,     15,   15,    15,         31    \ Vertex 7
     VERTEX    0,  -50,  -60,     8,      9,   10,    11,         31    \ Vertex 8
    
    
    
    
           Name: SHIP_VIPER                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Viper
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_viper.md)
     Variations: See [code variations](../related/compare/main/variable/ship_viper.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_VIPER
    
    
    
    
    
    
    .SHIP_VIPER
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 75 * 75           \ Targetable area          = 75 * 75
    
     EQUB LO(SHIP_VIPER_EDGES - SHIP_VIPER)            \ Edges data offset (low)
     EQUB LO(SHIP_VIPER_FACES - SHIP_VIPER)            \ Faces data offset (low)
    
     EQUB 81                \ Max. edge count          = (81 - 1) / 4 = 20
     EQUB 0                 \ Gun vertex               = 0
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 90                \ Number of vertices       = 90 / 6 = 15
     EQUB 20                \ Number of edges          = 20
     EQUW 0                 \ Bounty                   = 0
     EQUB 28                \ Number of faces          = 28 / 4 = 7
     EQUB 23                \ Visibility distance      = 23
     EQUB 140               \ Max. energy              = 140
     EQUB 32                \ Max. speed               = 32
    
     EQUB HI(SHIP_VIPER_EDGES - SHIP_VIPER)            \ Edges data offset (high)
     EQUB HI(SHIP_VIPER_FACES - SHIP_VIPER)            \ Faces data offset (high)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00010001         \ Laser power              = 2
                            \ Missiles                 = 1
    
    .SHIP_VIPER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,   72,     1,      2,    3,     4,         31    \ Vertex 0
     VERTEX    0,   16,   24,     0,      1,    2,     2,         30    \ Vertex 1
     VERTEX    0,  -16,   24,     3,      4,    5,     5,         30    \ Vertex 2
     VERTEX   48,    0,  -24,     2,      4,    6,     6,         31    \ Vertex 3
     VERTEX  -48,    0,  -24,     1,      3,    6,     6,         31    \ Vertex 4
     VERTEX   24,  -16,  -24,     4,      5,    6,     6,         30    \ Vertex 5
     VERTEX  -24,  -16,  -24,     5,      3,    6,     6,         30    \ Vertex 6
     VERTEX   24,   16,  -24,     0,      2,    6,     6,         31    \ Vertex 7
     VERTEX  -24,   16,  -24,     0,      1,    6,     6,         31    \ Vertex 8
     VERTEX  -32,    0,  -24,     6,      6,    6,     6,         19    \ Vertex 9
     VERTEX   32,    0,  -24,     6,      6,    6,     6,         19    \ Vertex 10
     VERTEX    8,    8,  -24,     6,      6,    6,     6,         19    \ Vertex 11
     VERTEX   -8,    8,  -24,     6,      6,    6,     6,         19    \ Vertex 12
     VERTEX   -8,   -8,  -24,     6,      6,    6,     6,         18    \ Vertex 13
     VERTEX    8,   -8,  -24,     6,      6,    6,     6,         18    \ Vertex 14
    
    .SHIP_VIPER_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       3,     2,     4,         31    \ Edge 0
     EDGE       0,       1,     1,     2,         30    \ Edge 1
     EDGE       0,       2,     3,     4,         30    \ Edge 2
     EDGE       0,       4,     1,     3,         31    \ Edge 3
     EDGE       1,       7,     0,     2,         30    \ Edge 4
     EDGE       1,       8,     0,     1,         30    \ Edge 5
     EDGE       2,       5,     4,     5,         30    \ Edge 6
     EDGE       2,       6,     3,     5,         30    \ Edge 7
     EDGE       7,       8,     0,     6,         31    \ Edge 8
     EDGE       5,       6,     5,     6,         30    \ Edge 9
     EDGE       4,       8,     1,     6,         31    \ Edge 10
     EDGE       4,       6,     3,     6,         30    \ Edge 11
     EDGE       3,       7,     2,     6,         31    \ Edge 12
     EDGE       3,       5,     6,     4,         30    \ Edge 13
     EDGE       9,      12,     6,     6,         19    \ Edge 14
     EDGE       9,      13,     6,     6,         18    \ Edge 15
     EDGE      10,      11,     6,     6,         19    \ Edge 16
     EDGE      10,      14,     6,     6,         18    \ Edge 17
     EDGE      11,      14,     6,     6,         16    \ Edge 18
     EDGE      12,      13,     6,     6,         16    \ Edge 19
    
    .SHIP_VIPER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       32,        0,         31      \ Face 0
     FACE      -22,       33,       11,         31      \ Face 1
     FACE       22,       33,       11,         31      \ Face 2
     FACE      -22,      -33,       11,         31      \ Face 3
     FACE       22,      -33,       11,         31      \ Face 4
     FACE        0,      -32,        0,         31      \ Face 5
     FACE        0,        0,      -48,         31      \ Face 6
    
    
    
    
           Name: SHIP_SIDEWINDER                                         [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Sidewinder
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_sidewinder.md)
     Variations: See [code variations](../related/compare/main/variable/ship_sidewinder.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_SIDEWINDER
    
    
    
    
    
    
    .SHIP_SIDEWINDER
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 65 * 65           \ Targetable area          = 65 * 65
    
     EQUB LO(SHIP_SIDEWINDER_EDGES - SHIP_SIDEWINDER)  \ Edges data offset (low)
     EQUB LO(SHIP_SIDEWINDER_FACES - SHIP_SIDEWINDER)  \ Faces data offset (low)
    
     EQUB 65                \ Max. edge count          = (65 - 1) / 4 = 16
     EQUB 0                 \ Gun vertex               = 0
     EQUB 30                \ Explosion count          = 6, as (4 * n) + 6 = 30
     EQUB 60                \ Number of vertices       = 60 / 6 = 10
     EQUB 15                \ Number of edges          = 15
     EQUW 50                \ Bounty                   = 50
     EQUB 28                \ Number of faces          = 28 / 4 = 7
     EQUB 20                \ Visibility distance      = 20
     EQUB 70                \ Max. energy              = 70
     EQUB 37                \ Max. speed               = 37
    
     EQUB HI(SHIP_SIDEWINDER_EDGES - SHIP_SIDEWINDER)  \ Edges data offset (high)
     EQUB HI(SHIP_SIDEWINDER_FACES - SHIP_SIDEWINDER)  \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00010000         \ Laser power              = 2
                            \ Missiles                 = 0
    
    .SHIP_SIDEWINDER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -32,    0,   36,     0,      1,    4,     5,         31    \ Vertex 0
     VERTEX   32,    0,   36,     0,      2,    5,     6,         31    \ Vertex 1
     VERTEX   64,    0,  -28,     2,      3,    6,     6,         31    \ Vertex 2
     VERTEX  -64,    0,  -28,     1,      3,    4,     4,         31    \ Vertex 3
     VERTEX    0,   16,  -28,     0,      1,    2,     3,         31    \ Vertex 4
     VERTEX    0,  -16,  -28,     3,      4,    5,     6,         31    \ Vertex 5
     VERTEX  -12,    6,  -28,     3,      3,    3,     3,         15    \ Vertex 6
     VERTEX   12,    6,  -28,     3,      3,    3,     3,         15    \ Vertex 7
     VERTEX   12,   -6,  -28,     3,      3,    3,     3,         12    \ Vertex 8
     VERTEX  -12,   -6,  -28,     3,      3,    3,     3,         12    \ Vertex 9
    
    .SHIP_SIDEWINDER_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     0,     5,         31    \ Edge 0
     EDGE       1,       2,     2,     6,         31    \ Edge 1
     EDGE       1,       4,     0,     2,         31    \ Edge 2
     EDGE       0,       4,     0,     1,         31    \ Edge 3
     EDGE       0,       3,     1,     4,         31    \ Edge 4
     EDGE       3,       4,     1,     3,         31    \ Edge 5
     EDGE       2,       4,     2,     3,         31    \ Edge 6
     EDGE       3,       5,     3,     4,         31    \ Edge 7
     EDGE       2,       5,     3,     6,         31    \ Edge 8
     EDGE       1,       5,     5,     6,         31    \ Edge 9
     EDGE       0,       5,     4,     5,         31    \ Edge 10
     EDGE       6,       7,     3,     3,         15    \ Edge 11
     EDGE       7,       8,     3,     3,         12    \ Edge 12
     EDGE       6,       9,     3,     3,         12    \ Edge 13
     EDGE       8,       9,     3,     3,         12    \ Edge 14
    
    .SHIP_SIDEWINDER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       32,        8,         31      \ Face 0
     FACE      -12,       47,        6,         31      \ Face 1
     FACE       12,       47,        6,         31      \ Face 2
     FACE        0,        0,     -112,         31      \ Face 3
     FACE      -12,      -47,        6,         31      \ Face 4
     FACE        0,      -32,        8,         31      \ Face 5
     FACE       12,      -47,        6,         31      \ Face 6
    
    
    
    
           Name: SHIP_MAMBA                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Mamba
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_mamba.md)
     Variations: See [code variations](../related/compare/main/variable/ship_mamba.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_MAMBA
    
    
    
    
    
    
    .SHIP_MAMBA
    
     EQUB 1                 \ Max. canisters on demise = 1
     EQUW 70 * 70           \ Targetable area          = 70 * 70
    
     EQUB LO(SHIP_MAMBA_EDGES - SHIP_MAMBA)            \ Edges data offset (low)
     EQUB LO(SHIP_MAMBA_FACES - SHIP_MAMBA)            \ Faces data offset (low)
    
     EQUB 97                \ Max. edge count          = (97 - 1) / 4 = 24
     EQUB 0                 \ Gun vertex               = 0
     EQUB 34                \ Explosion count          = 7, as (4 * n) + 6 = 34
     EQUB 150               \ Number of vertices       = 150 / 6 = 25
     EQUB 28                \ Number of edges          = 28
     EQUW 150               \ Bounty                   = 150
     EQUB 20                \ Number of faces          = 20 / 4 = 5
     EQUB 25                \ Visibility distance      = 25
     EQUB 90                \ Max. energy              = 90
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_MAMBA_EDGES - SHIP_MAMBA)            \ Edges data offset (high)
     EQUB HI(SHIP_MAMBA_FACES - SHIP_MAMBA)            \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00010010         \ Laser power              = 2
                            \ Missiles                 = 2
    
    .SHIP_MAMBA_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,   64,     0,      1,    2,     3,         31    \ Vertex 0
     VERTEX  -64,   -8,  -32,     0,      2,    4,     4,         31    \ Vertex 1
     VERTEX  -32,    8,  -32,     1,      2,    4,     4,         30    \ Vertex 2
     VERTEX   32,    8,  -32,     1,      3,    4,     4,         30    \ Vertex 3
     VERTEX   64,   -8,  -32,     0,      3,    4,     4,         31    \ Vertex 4
     VERTEX   -4,    4,   16,     1,      1,    1,     1,         14    \ Vertex 5
     VERTEX    4,    4,   16,     1,      1,    1,     1,         14    \ Vertex 6
     VERTEX    8,    3,   28,     1,      1,    1,     1,         13    \ Vertex 7
     VERTEX   -8,    3,   28,     1,      1,    1,     1,         13    \ Vertex 8
     VERTEX  -20,   -4,   16,     0,      0,    0,     0,         20    \ Vertex 9
     VERTEX   20,   -4,   16,     0,      0,    0,     0,         20    \ Vertex 10
     VERTEX  -24,   -7,  -20,     0,      0,    0,     0,         20    \ Vertex 11
     VERTEX  -16,   -7,  -20,     0,      0,    0,     0,         16    \ Vertex 12
     VERTEX   16,   -7,  -20,     0,      0,    0,     0,         16    \ Vertex 13
     VERTEX   24,   -7,  -20,     0,      0,    0,     0,         20    \ Vertex 14
     VERTEX   -8,    4,  -32,     4,      4,    4,     4,         13    \ Vertex 15
     VERTEX    8,    4,  -32,     4,      4,    4,     4,         13    \ Vertex 16
     VERTEX    8,   -4,  -32,     4,      4,    4,     4,         14    \ Vertex 17
     VERTEX   -8,   -4,  -32,     4,      4,    4,     4,         14    \ Vertex 18
     VERTEX  -32,    4,  -32,     4,      4,    4,     4,          7    \ Vertex 19
     VERTEX   32,    4,  -32,     4,      4,    4,     4,          7    \ Vertex 20
     VERTEX   36,   -4,  -32,     4,      4,    4,     4,          7    \ Vertex 21
     VERTEX  -36,   -4,  -32,     4,      4,    4,     4,          7    \ Vertex 22
     VERTEX  -38,    0,  -32,     4,      4,    4,     4,          5    \ Vertex 23
     VERTEX   38,    0,  -32,     4,      4,    4,     4,          5    \ Vertex 24
    
    .SHIP_MAMBA_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     0,     2,         31    \ Edge 0
     EDGE       0,       4,     0,     3,         31    \ Edge 1
     EDGE       1,       4,     0,     4,         31    \ Edge 2
     EDGE       1,       2,     2,     4,         30    \ Edge 3
     EDGE       2,       3,     1,     4,         30    \ Edge 4
     EDGE       3,       4,     3,     4,         30    \ Edge 5
     EDGE       5,       6,     1,     1,         14    \ Edge 6
     EDGE       6,       7,     1,     1,         12    \ Edge 7
     EDGE       7,       8,     1,     1,         13    \ Edge 8
     EDGE       5,       8,     1,     1,         12    \ Edge 9
     EDGE       9,      11,     0,     0,         20    \ Edge 10
     EDGE       9,      12,     0,     0,         16    \ Edge 11
     EDGE      10,      13,     0,     0,         16    \ Edge 12
     EDGE      10,      14,     0,     0,         20    \ Edge 13
     EDGE      13,      14,     0,     0,         14    \ Edge 14
     EDGE      11,      12,     0,     0,         14    \ Edge 15
     EDGE      15,      16,     4,     4,         13    \ Edge 16
     EDGE      17,      18,     4,     4,         14    \ Edge 17
     EDGE      15,      18,     4,     4,         12    \ Edge 18
     EDGE      16,      17,     4,     4,         12    \ Edge 19
     EDGE      20,      21,     4,     4,          7    \ Edge 20
     EDGE      20,      24,     4,     4,          5    \ Edge 21
     EDGE      21,      24,     4,     4,          5    \ Edge 22
     EDGE      19,      22,     4,     4,          7    \ Edge 23
     EDGE      19,      23,     4,     4,          5    \ Edge 24
     EDGE      22,      23,     4,     4,          5    \ Edge 25
     EDGE       0,       2,     1,     2,         30    \ Edge 26
     EDGE       0,       3,     1,     3,         30    \ Edge 27
    
    .SHIP_MAMBA_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,      -24,        2,         30      \ Face 0
     FACE        0,       24,        2,         30      \ Face 1
     FACE      -32,       64,       16,         30      \ Face 2
     FACE       32,       64,       16,         30      \ Face 3
     FACE        0,        0,     -127,         30      \ Face 4
    
    
    
    
           Name: SHIP_KRAIT                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Krait
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_krait.md)
     Variations: See [code variations](../related/compare/main/variable/ship_krait.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_KRAIT
    
    
    
    
    
    
    .SHIP_KRAIT
    
     EQUB 1                 \ Max. canisters on demise = 1
     EQUW 60 * 60           \ Targetable area          = 60 * 60
    
     EQUB LO(SHIP_KRAIT_EDGES - SHIP_KRAIT)            \ Edges data offset (low)
     EQUB LO(SHIP_KRAIT_FACES - SHIP_KRAIT)            \ Faces data offset (low)
    
     EQUB 89                \ Max. edge count          = (89 - 1) / 4 = 22
     EQUB 0                 \ Gun vertex               = 0
     EQUB 18                \ Explosion count          = 3, as (4 * n) + 6 = 18
     EQUB 102               \ Number of vertices       = 102 / 6 = 17
     EQUB 21                \ Number of edges          = 21
     EQUW 100               \ Bounty                   = 100
     EQUB 24                \ Number of faces          = 24 / 4 = 6
     EQUB 20                \ Visibility distance      = 20
     EQUB 80                \ Max. energy              = 80
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_KRAIT_EDGES - SHIP_KRAIT)            \ Edges data offset (high)
     EQUB HI(SHIP_KRAIT_FACES - SHIP_KRAIT)            \ Faces data offset (high)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00010000         \ Laser power              = 2
                            \ Missiles                 = 0
    
    .SHIP_KRAIT_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,   96,     1,      0,    3,     2,         31    \ Vertex 0
     VERTEX    0,   18,  -48,     3,      0,    5,     4,         31    \ Vertex 1
     VERTEX    0,  -18,  -48,     2,      1,    5,     4,         31    \ Vertex 2
     VERTEX   90,    0,   -3,     1,      0,    4,     4,         31    \ Vertex 3
     VERTEX  -90,    0,   -3,     3,      2,    5,     5,         31    \ Vertex 4
     VERTEX   90,    0,   87,     1,      0,    1,     1,         30    \ Vertex 5
     VERTEX  -90,    0,   87,     3,      2,    3,     3,         30    \ Vertex 6
     VERTEX    0,    5,   53,     0,      0,    3,     3,          9    \ Vertex 7
     VERTEX    0,    7,   38,     0,      0,    3,     3,          6    \ Vertex 8
     VERTEX  -18,    7,   19,     3,      3,    3,     3,          9    \ Vertex 9
     VERTEX   18,    7,   19,     0,      0,    0,     0,          9    \ Vertex 10
     VERTEX   18,   11,  -39,     4,      4,    4,     4,          8    \ Vertex 11
     VERTEX   18,  -11,  -39,     4,      4,    4,     4,          8    \ Vertex 12
     VERTEX   36,    0,  -30,     4,      4,    4,     4,          8    \ Vertex 13
     VERTEX  -18,   11,  -39,     5,      5,    5,     5,          8    \ Vertex 14
     VERTEX  -18,  -11,  -39,     5,      5,    5,     5,          8    \ Vertex 15
     VERTEX  -36,    0,  -30,     5,      5,    5,     5,          8    \ Vertex 16
    
    .SHIP_KRAIT_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     3,     0,         31    \ Edge 0
     EDGE       0,       2,     2,     1,         31    \ Edge 1
     EDGE       0,       3,     1,     0,         31    \ Edge 2
     EDGE       0,       4,     3,     2,         31    \ Edge 3
     EDGE       1,       4,     5,     3,         31    \ Edge 4
     EDGE       4,       2,     5,     2,         31    \ Edge 5
     EDGE       2,       3,     4,     1,         31    \ Edge 6
     EDGE       3,       1,     4,     0,         31    \ Edge 7
     EDGE       3,       5,     1,     0,         30    \ Edge 8
     EDGE       4,       6,     3,     2,         30    \ Edge 9
     EDGE       1,       2,     5,     4,          8    \ Edge 10
     EDGE       7,      10,     0,     0,          9    \ Edge 11
     EDGE       8,      10,     0,     0,          6    \ Edge 12
     EDGE       7,       9,     3,     3,          9    \ Edge 13
     EDGE       8,       9,     3,     3,          6    \ Edge 14
     EDGE      11,      13,     4,     4,          8    \ Edge 15
     EDGE      13,      12,     4,     4,          8    \ Edge 16
     EDGE      12,      11,     4,     4,          7    \ Edge 17
     EDGE      14,      15,     5,     5,          7    \ Edge 18
     EDGE      15,      16,     5,     5,          8    \ Edge 19
     EDGE      16,      14,     5,     5,          8    \ Edge 20
    
    .SHIP_KRAIT_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        3,       24,        3,         31      \ Face 0
     FACE        3,      -24,        3,         31      \ Face 1
     FACE       -3,      -24,        3,         31      \ Face 2
     FACE       -3,       24,        3,         31      \ Face 3
     FACE       38,        0,      -77,         31      \ Face 4
     FACE      -38,        0,      -77,         31      \ Face 5
    
    
    
    
           Name: SHIP_ADDER                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an Adder
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_adder.md)
     Variations: See [code variations](../related/compare/main/variable/ship_adder.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_ADDER
    
    
    
    
    
    
    .SHIP_ADDER
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 50 * 50           \ Targetable area          = 50 * 50
    
     EQUB LO(SHIP_ADDER_EDGES - SHIP_ADDER)            \ Edges data offset (low)
     EQUB LO(SHIP_ADDER_FACES - SHIP_ADDER)            \ Faces data offset (low)
    
     EQUB 101               \ Max. edge count          = (101 - 1) / 4 = 25
     EQUB 0                 \ Gun vertex               = 0
     EQUB 22                \ Explosion count          = 4, as (4 * n) + 6 = 22
     EQUB 108               \ Number of vertices       = 108 / 6 = 18
     EQUB 29                \ Number of edges          = 29
     EQUW 40                \ Bounty                   = 40
     EQUB 60                \ Number of faces          = 60 / 4 = 15
     EQUB 20                \ Visibility distance      = 20
     EQUB 85                \ Max. energy              = 85
     EQUB 24                \ Max. speed               = 24
    
     EQUB HI(SHIP_ADDER_EDGES - SHIP_ADDER)            \ Edges data offset (high)
     EQUB HI(SHIP_ADDER_FACES - SHIP_ADDER)            \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00010000         \ Laser power              = 2
                            \ Missiles                 = 0
    
    .SHIP_ADDER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -18,    0,   40,     1,      0,   12,    11,         31    \ Vertex 0
     VERTEX   18,    0,   40,     1,      0,    3,     2,         31    \ Vertex 1
     VERTEX   30,    0,  -24,     3,      2,    5,     4,         31    \ Vertex 2
     VERTEX   30,    0,  -40,     5,      4,    6,     6,         31    \ Vertex 3
     VERTEX   18,   -7,  -40,     6,      5,   14,     7,         31    \ Vertex 4
     VERTEX  -18,   -7,  -40,     8,      7,   14,    10,         31    \ Vertex 5
     VERTEX  -30,    0,  -40,     9,      8,   10,    10,         31    \ Vertex 6
     VERTEX  -30,    0,  -24,    10,      9,   12,    11,         31    \ Vertex 7
     VERTEX  -18,    7,  -40,     8,      7,   13,     9,         31    \ Vertex 8
     VERTEX   18,    7,  -40,     6,      4,   13,     7,         31    \ Vertex 9
     VERTEX  -18,    7,   13,     9,      0,   13,    11,         31    \ Vertex 10
     VERTEX   18,    7,   13,     2,      0,   13,     4,         31    \ Vertex 11
     VERTEX  -18,   -7,   13,    10,      1,   14,    12,         31    \ Vertex 12
     VERTEX   18,   -7,   13,     3,      1,   14,     5,         31    \ Vertex 13
     VERTEX  -11,    3,   29,     0,      0,    0,     0,          5    \ Vertex 14
     VERTEX   11,    3,   29,     0,      0,    0,     0,          5    \ Vertex 15
     VERTEX   11,    4,   24,     0,      0,    0,     0,          4    \ Vertex 16
     VERTEX  -11,    4,   24,     0,      0,    0,     0,          4    \ Vertex 17
    
    .SHIP_ADDER_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     1,     0,         31    \ Edge 0
     EDGE       1,       2,     3,     2,          7    \ Edge 1
     EDGE       2,       3,     5,     4,         31    \ Edge 2
     EDGE       3,       4,     6,     5,         31    \ Edge 3
     EDGE       4,       5,    14,     7,         31    \ Edge 4
     EDGE       5,       6,    10,     8,         31    \ Edge 5
     EDGE       6,       7,    10,     9,         31    \ Edge 6
     EDGE       7,       0,    12,    11,          7    \ Edge 7
     EDGE       3,       9,     6,     4,         31    \ Edge 8
     EDGE       9,       8,    13,     7,         31    \ Edge 9
     EDGE       8,       6,     9,     8,         31    \ Edge 10
     EDGE       0,      10,    11,     0,         31    \ Edge 11
     EDGE       7,      10,    11,     9,         31    \ Edge 12
     EDGE       1,      11,     2,     0,         31    \ Edge 13
     EDGE       2,      11,     4,     2,         31    \ Edge 14
     EDGE       0,      12,    12,     1,         31    \ Edge 15
     EDGE       7,      12,    12,    10,         31    \ Edge 16
     EDGE       1,      13,     3,     1,         31    \ Edge 17
     EDGE       2,      13,     5,     3,         31    \ Edge 18
     EDGE      10,      11,    13,     0,         31    \ Edge 19
     EDGE      12,      13,    14,     1,         31    \ Edge 20
     EDGE       8,      10,    13,     9,         31    \ Edge 21
     EDGE       9,      11,    13,     4,         31    \ Edge 22
     EDGE       5,      12,    14,    10,         31    \ Edge 23
     EDGE       4,      13,    14,     5,         31    \ Edge 24
     EDGE      14,      15,     0,     0,          5    \ Edge 25
     EDGE      15,      16,     0,     0,          3    \ Edge 26
     EDGE      16,      17,     0,     0,          4    \ Edge 27
     EDGE      17,      14,     0,     0,          3    \ Edge 28
    
    .SHIP_ADDER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       39,       10,         31      \ Face 0
     FACE        0,      -39,       10,         31      \ Face 1
     FACE       69,       50,       13,         31      \ Face 2
     FACE       69,      -50,       13,         31      \ Face 3
     FACE       30,       52,        0,         31      \ Face 4
     FACE       30,      -52,        0,         31      \ Face 5
     FACE        0,        0,     -160,         31      \ Face 6
     FACE        0,        0,     -160,         31      \ Face 7
     FACE        0,        0,     -160,         31      \ Face 8
     FACE      -30,       52,        0,         31      \ Face 9
     FACE      -30,      -52,        0,         31      \ Face 10
     FACE      -69,       50,       13,         31      \ Face 11
     FACE      -69,      -50,       13,         31      \ Face 12
     FACE        0,       28,        0,         31      \ Face 13
     FACE        0,      -28,        0,         31      \ Face 14
    
    
    
    
           Name: SHIP_GECKO                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Gecko
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_gecko.md)
     Variations: See [code variations](../related/compare/main/variable/ship_gecko.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_GECKO
    
    
    
    
    
    
    .SHIP_GECKO
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 99 * 99           \ Targetable area          = 99 * 99
    
     EQUB LO(SHIP_GECKO_EDGES - SHIP_GECKO)            \ Edges data offset (low)
     EQUB LO(SHIP_GECKO_FACES - SHIP_GECKO)            \ Faces data offset (low)
    
     EQUB 69                \ Max. edge count          = (69 - 1) / 4 = 17
     EQUB 0                 \ Gun vertex               = 0
     EQUB 26                \ Explosion count          = 5, as (4 * n) + 6 = 26
     EQUB 72                \ Number of vertices       = 72 / 6 = 12
     EQUB 17                \ Number of edges          = 17
     EQUW 55                \ Bounty                   = 55
     EQUB 36                \ Number of faces          = 36 / 4 = 9
     EQUB 18                \ Visibility distance      = 18
     EQUB 70                \ Max. energy              = 70
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_GECKO_EDGES - SHIP_GECKO)            \ Edges data offset (high)
     EQUB HI(SHIP_GECKO_FACES - SHIP_GECKO)            \ Faces data offset (high)
    
     EQUB 3                 \ Normals are scaled by    = 2^3 = 8
     EQUB %00010000         \ Laser power              = 2
                            \ Missiles                 = 0
    
    .SHIP_GECKO_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -10,   -4,   47,     3,      0,    5,     4,         31    \ Vertex 0
     VERTEX   10,   -4,   47,     1,      0,    3,     2,         31    \ Vertex 1
     VERTEX  -16,    8,  -23,     5,      0,    7,     6,         31    \ Vertex 2
     VERTEX   16,    8,  -23,     1,      0,    8,     7,         31    \ Vertex 3
     VERTEX  -66,    0,   -3,     5,      4,    6,     6,         31    \ Vertex 4
     VERTEX   66,    0,   -3,     2,      1,    8,     8,         31    \ Vertex 5
     VERTEX  -20,  -14,  -23,     4,      3,    7,     6,         31    \ Vertex 6
     VERTEX   20,  -14,  -23,     3,      2,    8,     7,         31    \ Vertex 7
     VERTEX   -8,   -6,   33,     3,      3,    3,     3,         16    \ Vertex 8
     VERTEX    8,   -6,   33,     3,      3,    3,     3,         17    \ Vertex 9
     VERTEX   -8,  -13,  -16,     3,      3,    3,     3,         16    \ Vertex 10
     VERTEX    8,  -13,  -16,     3,      3,    3,     3,         17    \ Vertex 11
    
    .SHIP_GECKO_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     3,     0,         31    \ Edge 0
     EDGE       1,       5,     2,     1,         31    \ Edge 1
     EDGE       5,       3,     8,     1,         31    \ Edge 2
     EDGE       3,       2,     7,     0,         31    \ Edge 3
     EDGE       2,       4,     6,     5,         31    \ Edge 4
     EDGE       4,       0,     5,     4,         31    \ Edge 5
     EDGE       5,       7,     8,     2,         31    \ Edge 6
     EDGE       7,       6,     7,     3,         31    \ Edge 7
     EDGE       6,       4,     6,     4,         31    \ Edge 8
     EDGE       0,       2,     5,     0,         29    \ Edge 9
     EDGE       1,       3,     1,     0,         30    \ Edge 10
     EDGE       0,       6,     4,     3,         29    \ Edge 11
     EDGE       1,       7,     3,     2,         30    \ Edge 12
     EDGE       2,       6,     7,     6,         20    \ Edge 13
     EDGE       3,       7,     8,     7,         20    \ Edge 14
     EDGE       8,      10,     3,     3,         16    \ Edge 15
     EDGE       9,      11,     3,     3,         17    \ Edge 16
    
    .SHIP_GECKO_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       31,        5,         31      \ Face 0
     FACE        4,       45,        8,         31      \ Face 1
     FACE       25,     -108,       19,         31      \ Face 2
     FACE        0,      -84,       12,         31      \ Face 3
     FACE      -25,     -108,       19,         31      \ Face 4
     FACE       -4,       45,        8,         31      \ Face 5
     FACE      -88,       16,     -214,         31      \ Face 6
     FACE        0,        0,     -187,         31      \ Face 7
     FACE       88,       16,     -214,         31      \ Face 8
    
    
    
    
           Name: SHIP_COBRA_MK_1                                         [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Cobra Mk I
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_cobra_mk_1.md)
     Variations: See [code variations](../related/compare/main/variable/ship_cobra_mk_1.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_COBRA_MK_1
    
    
    
    
    
    
    .SHIP_COBRA_MK_1
    
     EQUB 3                 \ Max. canisters on demise = 3
     EQUW 99 * 99           \ Targetable area          = 99 * 99
    
     EQUB LO(SHIP_COBRA_MK_1_EDGES - SHIP_COBRA_MK_1)  \ Edges data offset (low)
     EQUB LO(SHIP_COBRA_MK_1_FACES - SHIP_COBRA_MK_1)  \ Faces data offset (low)
    
     EQUB 73                \ Max. edge count          = (73 - 1) / 4 = 18
     EQUB 40                \ Gun vertex               = 40 / 4 = 10
     EQUB 26                \ Explosion count          = 5, as (4 * n) + 6 = 26
     EQUB 66                \ Number of vertices       = 66 / 6 = 11
     EQUB 18                \ Number of edges          = 18
     EQUW 75                \ Bounty                   = 75
     EQUB 40                \ Number of faces          = 40 / 4 = 10
     EQUB 19                \ Visibility distance      = 19
     EQUB 90                \ Max. energy              = 90
     EQUB 26                \ Max. speed               = 26
    
     EQUB HI(SHIP_COBRA_MK_1_EDGES - SHIP_COBRA_MK_1)  \ Edges data offset (high)
     EQUB HI(SHIP_COBRA_MK_1_FACES - SHIP_COBRA_MK_1)  \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00010010         \ Laser power              = 2
                            \ Missiles                 = 2
    
    .SHIP_COBRA_MK_1_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -18,   -1,   50,     1,      0,    3,     2,         31    \ Vertex 0
     VERTEX   18,   -1,   50,     1,      0,    5,     4,         31    \ Vertex 1
     VERTEX  -66,    0,    7,     3,      2,    8,     8,         31    \ Vertex 2
     VERTEX   66,    0,    7,     5,      4,    9,     9,         31    \ Vertex 3
     VERTEX  -32,   12,  -38,     6,      2,    8,     7,         31    \ Vertex 4
     VERTEX   32,   12,  -38,     6,      4,    9,     7,         31    \ Vertex 5
     VERTEX  -54,  -12,  -38,     3,      1,    8,     7,         31    \ Vertex 6
     VERTEX   54,  -12,  -38,     5,      1,    9,     7,         31    \ Vertex 7
     VERTEX    0,   12,   -6,     2,      0,    6,     4,         20    \ Vertex 8
     VERTEX    0,   -1,   50,     1,      0,    1,     1,          2    \ Vertex 9
     VERTEX    0,   -1,   60,     1,      0,    1,     1,         31    \ Vertex 10
    
    .SHIP_COBRA_MK_1_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       1,       0,     1,     0,         31    \ Edge 0
     EDGE       0,       2,     3,     2,         31    \ Edge 1
     EDGE       2,       6,     8,     3,         31    \ Edge 2
     EDGE       6,       7,     7,     1,         31    \ Edge 3
     EDGE       7,       3,     9,     5,         31    \ Edge 4
     EDGE       3,       1,     5,     4,         31    \ Edge 5
     EDGE       2,       4,     8,     2,         31    \ Edge 6
     EDGE       4,       5,     7,     6,         31    \ Edge 7
     EDGE       5,       3,     9,     4,         31    \ Edge 8
     EDGE       0,       8,     2,     0,         20    \ Edge 9
     EDGE       8,       1,     4,     0,         20    \ Edge 10
     EDGE       4,       8,     6,     2,         16    \ Edge 11
     EDGE       8,       5,     6,     4,         16    \ Edge 12
     EDGE       4,       6,     8,     7,         31    \ Edge 13
     EDGE       5,       7,     9,     7,         31    \ Edge 14
     EDGE       0,       6,     3,     1,         20    \ Edge 15
     EDGE       1,       7,     5,     1,         20    \ Edge 16
     EDGE      10,       9,     1,     0,          2    \ Edge 17
    
    .SHIP_COBRA_MK_1_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       41,       10,         31      \ Face 0
     FACE        0,      -27,        3,         31      \ Face 1
     FACE       -8,       46,        8,         31      \ Face 2
     FACE      -12,      -57,       12,         31      \ Face 3
     FACE        8,       46,        8,         31      \ Face 4
     FACE       12,      -57,       12,         31      \ Face 5
     FACE        0,       49,        0,         31      \ Face 6
     FACE        0,        0,     -154,         31      \ Face 7
     FACE     -121,      111,      -62,         31      \ Face 8
     FACE      121,      111,      -62,         31      \ Face 9
    
    
    
    
           Name: SHIP_WORM                                               [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Worm
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_worm.md)
     Variations: See [code variations](../related/compare/main/variable/ship_worm.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_WORM
    
    
    
    
    
    
    .SHIP_WORM
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 99 * 99           \ Targetable area          = 99 * 99
    
     EQUB LO(SHIP_WORM_EDGES - SHIP_WORM)              \ Edges data offset (low)
     EQUB LO(SHIP_WORM_FACES - SHIP_WORM)              \ Faces data offset (low)
    
     EQUB 77                \ Max. edge count          = (77 - 1) / 4 = 19
     EQUB 0                 \ Gun vertex               = 0
     EQUB 18                \ Explosion count          = 3, as (4 * n) + 6 = 18
     EQUB 60                \ Number of vertices       = 60 / 6 = 10
     EQUB 16                \ Number of edges          = 16
     EQUW 0                 \ Bounty                   = 0
     EQUB 32                \ Number of faces          = 32 / 4 = 8
     EQUB 19                \ Visibility distance      = 19
     EQUB 30                \ Max. energy              = 30
     EQUB 23                \ Max. speed               = 23
    
     EQUB HI(SHIP_WORM_EDGES - SHIP_WORM)              \ Edges data offset (high)
     EQUB HI(SHIP_WORM_FACES - SHIP_WORM)              \ Faces data offset (high)
    
     EQUB 3                 \ Normals are scaled by    = 2^3 = 8
     EQUB %00001000         \ Laser power              = 1
                            \ Missiles                 = 0
    
    .SHIP_WORM_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   10,  -10,   35,     2,      0,    7,     7,         31    \ Vertex 0
     VERTEX  -10,  -10,   35,     3,      0,    7,     7,         31    \ Vertex 1
     VERTEX    5,    6,   15,     1,      0,    4,     2,         31    \ Vertex 2
     VERTEX   -5,    6,   15,     1,      0,    5,     3,         31    \ Vertex 3
     VERTEX   15,  -10,   25,     4,      2,    7,     7,         31    \ Vertex 4
     VERTEX  -15,  -10,   25,     5,      3,    7,     7,         31    \ Vertex 5
     VERTEX   26,  -10,  -25,     6,      4,    7,     7,         31    \ Vertex 6
     VERTEX  -26,  -10,  -25,     6,      5,    7,     7,         31    \ Vertex 7
     VERTEX    8,   14,  -25,     4,      1,    6,     6,         31    \ Vertex 8
     VERTEX   -8,   14,  -25,     5,      1,    6,     6,         31    \ Vertex 9
    
    .SHIP_WORM_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     7,     0,         31    \ Edge 0
     EDGE       1,       5,     7,     3,         31    \ Edge 1
     EDGE       5,       7,     7,     5,         31    \ Edge 2
     EDGE       7,       6,     7,     6,         31    \ Edge 3
     EDGE       6,       4,     7,     4,         31    \ Edge 4
     EDGE       4,       0,     7,     2,         31    \ Edge 5
     EDGE       0,       2,     2,     0,         31    \ Edge 6
     EDGE       1,       3,     3,     0,         31    \ Edge 7
     EDGE       4,       2,     4,     2,         31    \ Edge 8
     EDGE       5,       3,     5,     3,         31    \ Edge 9
     EDGE       2,       8,     4,     1,         31    \ Edge 10
     EDGE       8,       6,     6,     4,         31    \ Edge 11
     EDGE       3,       9,     5,     1,         31    \ Edge 12
     EDGE       9,       7,     6,     5,         31    \ Edge 13
     EDGE       2,       3,     1,     0,         31    \ Edge 14
     EDGE       8,       9,     6,     1,         31    \ Edge 15
    
    .SHIP_WORM_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       88,       70,         31      \ Face 0
     FACE        0,       69,       14,         31      \ Face 1
     FACE       70,       66,       35,         31      \ Face 2
     FACE      -70,       66,       35,         31      \ Face 3
     FACE       64,       49,       14,         31      \ Face 4
     FACE      -64,       49,       14,         31      \ Face 5
     FACE        0,        0,     -200,         31      \ Face 6
     FACE        0,      -80,        0,         31      \ Face 7
    
    
    
    
           Name: SHIP_COBRA_MK_3_P                                       [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Cobra Mk III (pirate)
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_cobra_mk_3_p.md)
     Variations: See [code variations](../related/compare/main/variable/ship_cobra_mk_3_p.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_COBRA_MK_3_P
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the pirate Cobra Mk III reuses the edges and faces data
     from the non-pirate Cobra Mk III, so the edges and faces data offsets are
     negative.
    
    
    
    
    .SHIP_COBRA_MK_3_P
    
     EQUB 1                 \ Max. canisters on demise = 1
     EQUW 95 * 95           \ Targetable area          = 95 * 95
    
     EQUB LO(SHIP_COBRA_MK_3_EDGES - SHIP_COBRA_MK_3_P)   \ Edges from Cobra Mk III
     EQUB LO(SHIP_COBRA_MK_3_FACES - SHIP_COBRA_MK_3_P)   \ Faces from Cobra Mk III
    
     EQUB 157               \ Max. edge count          = (157 - 1) / 4 = 39
     EQUB 84                \ Gun vertex               = 84 / 4 = 21
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 168               \ Number of vertices       = 168 / 6 = 28
     EQUB 38                \ Number of edges          = 38
     EQUW 175               \ Bounty                   = 175
     EQUB 52                \ Number of faces          = 52 / 4 = 13
     EQUB 50                \ Visibility distance      = 50
     EQUB 150               \ Max. energy              = 150
     EQUB 28                \ Max. speed               = 28
    
     EQUB HI(SHIP_COBRA_MK_3_EDGES - SHIP_COBRA_MK_3_P)   \ Edges from Cobra Mk III
     EQUB HI(SHIP_COBRA_MK_3_FACES - SHIP_COBRA_MK_3_P)   \ Faces from Cobra Mk III
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00010010         \ Laser power              = 2
                            \ Missiles                 = 2
    
    .SHIP_COBRA_MK_3_P_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   32,    0,   76,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX  -32,    0,   76,    15,     15,   15,    15,         31    \ Vertex 1
     VERTEX    0,   26,   24,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX -120,   -3,   -8,     3,      7,   10,    10,         31    \ Vertex 3
     VERTEX  120,   -3,   -8,     4,      8,   12,    12,         31    \ Vertex 4
     VERTEX  -88,   16,  -40,    15,     15,   15,    15,         31    \ Vertex 5
     VERTEX   88,   16,  -40,    15,     15,   15,    15,         31    \ Vertex 6
     VERTEX  128,   -8,  -40,     8,      9,   12,    12,         31    \ Vertex 7
     VERTEX -128,   -8,  -40,     7,      9,   10,    10,         31    \ Vertex 8
     VERTEX    0,   26,  -40,     5,      6,    9,     9,         31    \ Vertex 9
     VERTEX  -32,  -24,  -40,     9,     10,   11,    11,         31    \ Vertex 10
     VERTEX   32,  -24,  -40,     9,     11,   12,    12,         31    \ Vertex 11
     VERTEX  -36,    8,  -40,     9,      9,    9,     9,         20    \ Vertex 12
     VERTEX   -8,   12,  -40,     9,      9,    9,     9,         20    \ Vertex 13
     VERTEX    8,   12,  -40,     9,      9,    9,     9,         20    \ Vertex 14
     VERTEX   36,    8,  -40,     9,      9,    9,     9,         20    \ Vertex 15
     VERTEX   36,  -12,  -40,     9,      9,    9,     9,         20    \ Vertex 16
     VERTEX    8,  -16,  -40,     9,      9,    9,     9,         20    \ Vertex 17
     VERTEX   -8,  -16,  -40,     9,      9,    9,     9,         20    \ Vertex 18
     VERTEX  -36,  -12,  -40,     9,      9,    9,     9,         20    \ Vertex 19
     VERTEX    0,    0,   76,     0,     11,   11,    11,          6    \ Vertex 20
     VERTEX    0,    0,   90,     0,     11,   11,    11,         31    \ Vertex 21
     VERTEX  -80,   -6,  -40,     9,      9,    9,     9,          8    \ Vertex 22
     VERTEX  -80,    6,  -40,     9,      9,    9,     9,          8    \ Vertex 23
     VERTEX  -88,    0,  -40,     9,      9,    9,     9,          6    \ Vertex 24
     VERTEX   80,    6,  -40,     9,      9,    9,     9,          8    \ Vertex 25
     VERTEX   88,    0,  -40,     9,      9,    9,     9,          6    \ Vertex 26
     VERTEX   80,   -6,  -40,     9,      9,    9,     9,          8    \ Vertex 27
    
    
    
    
           Name: SHIP_ASP_MK_2                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an Asp Mk II
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_asp_mk_2.md)
     Variations: See [code variations](../related/compare/main/variable/ship_asp_mk_2.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_ASP_MK_2
    
    
    
    
    
    
    .SHIP_ASP_MK_2
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 60 * 60           \ Targetable area          = 60 * 60
    
     EQUB LO(SHIP_ASP_MK_2_EDGES - SHIP_ASP_MK_2)      \ Edges data offset (low)
     EQUB LO(SHIP_ASP_MK_2_FACES - SHIP_ASP_MK_2)      \ Faces data offset (low)
    
     EQUB 105               \ Max. edge count          = (105 - 1) / 4 = 26
     EQUB 32                \ Gun vertex               = 32 / 4 = 8
     EQUB 26                \ Explosion count          = 5, as (4 * n) + 6 = 26
     EQUB 114               \ Number of vertices       = 114 / 6 = 19
     EQUB 28                \ Number of edges          = 28
     EQUW 200               \ Bounty                   = 200
     EQUB 48                \ Number of faces          = 48 / 4 = 12
     EQUB 40                \ Visibility distance      = 40
     EQUB 150               \ Max. energy              = 150
     EQUB 40                \ Max. speed               = 40
    
     EQUB HI(SHIP_ASP_MK_2_EDGES - SHIP_ASP_MK_2)      \ Edges data offset (high)
     EQUB HI(SHIP_ASP_MK_2_FACES - SHIP_ASP_MK_2)      \ Faces data offset (high)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00101001         \ Laser power              = 5
                            \ Missiles                 = 1
    
    .SHIP_ASP_MK_2_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,  -18,    0,     1,      0,    2,     2,         22    \ Vertex 0
     VERTEX    0,   -9,  -45,     2,      1,   11,    11,         31    \ Vertex 1
     VERTEX   43,    0,  -45,     6,      1,   11,    11,         31    \ Vertex 2
     VERTEX   69,   -3,    0,     6,      1,    9,     7,         31    \ Vertex 3
     VERTEX   43,  -14,   28,     1,      0,    7,     7,         31    \ Vertex 4
     VERTEX  -43,    0,  -45,     5,      2,   11,    11,         31    \ Vertex 5
     VERTEX  -69,   -3,    0,     5,      2,   10,     8,         31    \ Vertex 6
     VERTEX  -43,  -14,   28,     2,      0,    8,     8,         31    \ Vertex 7
     VERTEX   26,   -7,   73,     4,      0,    9,     7,         31    \ Vertex 8
     VERTEX  -26,   -7,   73,     4,      0,   10,     8,         31    \ Vertex 9
     VERTEX   43,   14,   28,     4,      3,    9,     6,         31    \ Vertex 10
     VERTEX  -43,   14,   28,     4,      3,   10,     5,         31    \ Vertex 11
     VERTEX    0,    9,  -45,     5,      3,   11,     6,         31    \ Vertex 12
     VERTEX  -17,    0,  -45,    11,     11,   11,    11,         10    \ Vertex 13
     VERTEX   17,    0,  -45,    11,     11,   11,    11,          9    \ Vertex 14
     VERTEX    0,   -4,  -45,    11,     11,   11,    11,         10    \ Vertex 15
     VERTEX    0,    4,  -45,    11,     11,   11,    11,          8    \ Vertex 16
     VERTEX    0,   -7,   73,     4,      0,    4,     0,         10    \ Vertex 17
     VERTEX    0,   -7,   83,     4,      0,    4,     0,         10    \ Vertex 18
    
    .SHIP_ASP_MK_2_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     2,     1,         22    \ Edge 0
     EDGE       0,       4,     1,     0,         22    \ Edge 1
     EDGE       0,       7,     2,     0,         22    \ Edge 2
     EDGE       1,       2,    11,     1,         31    \ Edge 3
     EDGE       2,       3,     6,     1,         31    \ Edge 4
     EDGE       3,       8,     9,     7,         16    \ Edge 5
     EDGE       8,       9,     4,     0,         31    \ Edge 6
     EDGE       6,       9,    10,     8,         16    \ Edge 7
     EDGE       5,       6,     5,     2,         31    \ Edge 8
     EDGE       1,       5,    11,     2,         31    \ Edge 9
     EDGE       3,       4,     7,     1,         31    \ Edge 10
     EDGE       4,       8,     7,     0,         31    \ Edge 11
     EDGE       6,       7,     8,     2,         31    \ Edge 12
     EDGE       7,       9,     8,     0,         31    \ Edge 13
     EDGE       2,      12,    11,     6,         31    \ Edge 14
     EDGE       5,      12,    11,     5,         31    \ Edge 15
     EDGE      10,      12,     6,     3,         22    \ Edge 16
     EDGE      11,      12,     5,     3,         22    \ Edge 17
     EDGE      10,      11,     4,     3,         22    \ Edge 18
     EDGE       6,      11,    10,     5,         31    \ Edge 19
     EDGE       9,      11,    10,     4,         31    \ Edge 20
     EDGE       3,      10,     9,     6,         31    \ Edge 21
     EDGE       8,      10,     9,     4,         31    \ Edge 22
     EDGE      13,      15,    11,    11,         10    \ Edge 23
     EDGE      15,      14,    11,    11,          9    \ Edge 24
     EDGE      14,      16,    11,    11,          8    \ Edge 25
     EDGE      16,      13,    11,    11,          8    \ Edge 26
     EDGE      18,      17,     4,     0,         10    \ Edge 27
    
    .SHIP_ASP_MK_2_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,      -35,        5,         31      \ Face 0
     FACE        8,      -38,       -7,         31      \ Face 1
     FACE       -8,      -38,       -7,         31      \ Face 2
     FACE        0,       24,       -1,         22      \ Face 3
     FACE        0,       43,       19,         31      \ Face 4
     FACE       -6,       28,       -2,         31      \ Face 5
     FACE        6,       28,       -2,         31      \ Face 6
     FACE       59,      -64,       31,         31      \ Face 7
     FACE      -59,      -64,       31,         31      \ Face 8
     FACE       80,       46,       50,         31      \ Face 9
     FACE      -80,       46,       50,         31      \ Face 10
     FACE        0,        0,      -90,         31      \ Face 11
    
     EQUB &45, &4D          \ These bytes appear to be unused
     EQUB &41, &36
    
    
    
    
           Name: SHIP_PYTHON_P                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Python (pirate)
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_python_p.md)
     Variations: See [code variations](../related/compare/main/variable/ship_python_p.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_PYTHON_P
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the pirate Python reuses the edges and faces data from
     the non-pirate Python, so the edges and faces data offsets are negative.
    
    
    
    
    .SHIP_PYTHON_P
    
     EQUB 2                 \ Max. canisters on demise = 2
     EQUW 80 * 80           \ Targetable area          = 80 * 80
    
     EQUB LO(SHIP_PYTHON_EDGES - SHIP_PYTHON_P)        \ Edges from Python
     EQUB LO(SHIP_PYTHON_FACES - SHIP_PYTHON_P)        \ Faces from Python
    
     EQUB 89                \ Max. edge count          = (89 - 1) / 4 = 22
     EQUB 0                 \ Gun vertex               = 0
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 66                \ Number of vertices       = 66 / 6 = 11
     EQUB 26                \ Number of edges          = 26
     EQUW 200               \ Bounty                   = 200
     EQUB 52                \ Number of faces          = 52 / 4 = 13
     EQUB 40                \ Visibility distance      = 40
     EQUB 250               \ Max. energy              = 250
     EQUB 20                \ Max. speed               = 20
    
     EQUB HI(SHIP_PYTHON_EDGES - SHIP_PYTHON_P)        \ Edges from Python
     EQUB HI(SHIP_PYTHON_FACES - SHIP_PYTHON_P)        \ Faces from Python
    
     EQUB 0                 \ Normals are scaled by    = 2^0 = 1
     EQUB %00011011         \ Laser power              = 3
                            \ Missiles                 = 3
    
    .SHIP_PYTHON_P_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,  224,     0,      1,    2,     3,         31    \ Vertex 0
     VERTEX    0,   48,   48,     0,      1,    4,     5,         31    \ Vertex 1
     VERTEX   96,    0,  -16,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX  -96,    0,  -16,    15,     15,   15,    15,         31    \ Vertex 3
     VERTEX    0,   48,  -32,     4,      5,    8,     9,         31    \ Vertex 4
     VERTEX    0,   24, -112,     9,      8,   12,    12,         31    \ Vertex 5
     VERTEX  -48,    0, -112,     8,     11,   12,    12,         31    \ Vertex 6
     VERTEX   48,    0, -112,     9,     10,   12,    12,         31    \ Vertex 7
     VERTEX    0,  -48,   48,     2,      3,    6,     7,         31    \ Vertex 8
     VERTEX    0,  -48,  -32,     6,      7,   10,    11,         31    \ Vertex 9
     VERTEX    0,  -24, -112,    10,     11,   12,    12,         31    \ Vertex 10
    
    
    
    
           Name: SHIP_FER_DE_LANCE                                       [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Fer-de-Lance
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_fer_de_lance.md)
     Variations: See [code variations](../related/compare/main/variable/ship_fer_de_lance.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_FER_DE_LANCE
    
    
    
    
    
    
    .SHIP_FER_DE_LANCE
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 40 * 40           \ Targetable area          = 40 * 40
    
     EQUB LO(SHIP_FER_DE_LANCE_EDGES - SHIP_FER_DE_LANCE) \ Edges data offset (low)
     EQUB LO(SHIP_FER_DE_LANCE_FACES - SHIP_FER_DE_LANCE) \ Faces data offset (low)
    
     EQUB 109               \ Max. edge count          = (109 - 1) / 4 = 27
     EQUB 0                 \ Gun vertex               = 0
     EQUB 26                \ Explosion count          = 5, as (4 * n) + 6 = 26
     EQUB 114               \ Number of vertices       = 114 / 6 = 19
     EQUB 27                \ Number of edges          = 27
     EQUW 0                 \ Bounty                   = 0
     EQUB 40                \ Number of faces          = 40 / 4 = 10
     EQUB 40                \ Visibility distance      = 40
     EQUB 160               \ Max. energy              = 160
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_FER_DE_LANCE_EDGES - SHIP_FER_DE_LANCE) \ Edges data offset (high)
     EQUB HI(SHIP_FER_DE_LANCE_FACES - SHIP_FER_DE_LANCE) \ Faces data offset (high)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00010010         \ Laser power              = 2
                            \ Missiles                 = 2
    
    .SHIP_FER_DE_LANCE_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,  -14,  108,     1,      0,    9,     5,         31    \ Vertex 0
     VERTEX  -40,  -14,   -4,     2,      1,    9,     9,         31    \ Vertex 1
     VERTEX  -12,  -14,  -52,     3,      2,    9,     9,         31    \ Vertex 2
     VERTEX   12,  -14,  -52,     4,      3,    9,     9,         31    \ Vertex 3
     VERTEX   40,  -14,   -4,     5,      4,    9,     9,         31    \ Vertex 4
     VERTEX  -40,   14,   -4,     1,      0,    6,     2,         28    \ Vertex 5
     VERTEX  -12,    2,  -52,     3,      2,    7,     6,         28    \ Vertex 6
     VERTEX   12,    2,  -52,     4,      3,    8,     7,         28    \ Vertex 7
     VERTEX   40,   14,   -4,     4,      0,    8,     5,         28    \ Vertex 8
     VERTEX    0,   18,  -20,     6,      0,    8,     7,         15    \ Vertex 9
     VERTEX   -3,  -11,   97,     0,      0,    0,     0,         11    \ Vertex 10
     VERTEX  -26,    8,   18,     0,      0,    0,     0,          9    \ Vertex 11
     VERTEX  -16,   14,   -4,     0,      0,    0,     0,         11    \ Vertex 12
     VERTEX    3,  -11,   97,     0,      0,    0,     0,         11    \ Vertex 13
     VERTEX   26,    8,   18,     0,      0,    0,     0,          9    \ Vertex 14
     VERTEX   16,   14,   -4,     0,      0,    0,     0,         11    \ Vertex 15
     VERTEX    0,  -14,  -20,     9,      9,    9,     9,         12    \ Vertex 16
     VERTEX  -14,  -14,   44,     9,      9,    9,     9,         12    \ Vertex 17
     VERTEX   14,  -14,   44,     9,      9,    9,     9,         12    \ Vertex 18
    
    .SHIP_FER_DE_LANCE_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     9,     1,         31    \ Edge 0
     EDGE       1,       2,     9,     2,         31    \ Edge 1
     EDGE       2,       3,     9,     3,         31    \ Edge 2
     EDGE       3,       4,     9,     4,         31    \ Edge 3
     EDGE       0,       4,     9,     5,         31    \ Edge 4
     EDGE       0,       5,     1,     0,         28    \ Edge 5
     EDGE       5,       6,     6,     2,         28    \ Edge 6
     EDGE       6,       7,     7,     3,         28    \ Edge 7
     EDGE       7,       8,     8,     4,         28    \ Edge 8
     EDGE       0,       8,     5,     0,         28    \ Edge 9
     EDGE       5,       9,     6,     0,         15    \ Edge 10
     EDGE       6,       9,     7,     6,         11    \ Edge 11
     EDGE       7,       9,     8,     7,         11    \ Edge 12
     EDGE       8,       9,     8,     0,         15    \ Edge 13
     EDGE       1,       5,     2,     1,         14    \ Edge 14
     EDGE       2,       6,     3,     2,         14    \ Edge 15
     EDGE       3,       7,     4,     3,         14    \ Edge 16
     EDGE       4,       8,     5,     4,         14    \ Edge 17
     EDGE      10,      11,     0,     0,          8    \ Edge 18
     EDGE      11,      12,     0,     0,          9    \ Edge 19
     EDGE      10,      12,     0,     0,         11    \ Edge 20
     EDGE      13,      14,     0,     0,          8    \ Edge 21
     EDGE      14,      15,     0,     0,          9    \ Edge 22
     EDGE      13,      15,     0,     0,         11    \ Edge 23
     EDGE      16,      17,     9,     9,         12    \ Edge 24
     EDGE      16,      18,     9,     9,         12    \ Edge 25
     EDGE      17,      18,     9,     9,          8    \ Edge 26
    
    .SHIP_FER_DE_LANCE_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       24,        6,         28      \ Face 0
     FACE      -68,        0,       24,         31      \ Face 1
     FACE      -63,        0,      -37,         31      \ Face 2
     FACE        0,        0,     -104,         31      \ Face 3
     FACE       63,        0,      -37,         31      \ Face 4
     FACE       68,        0,       24,         31      \ Face 5
     FACE      -12,       46,      -19,         28      \ Face 6
     FACE        0,       45,      -22,         28      \ Face 7
     FACE       12,       46,      -19,         28      \ Face 8
     FACE        0,      -28,        0,         31      \ Face 9
    
    
    
    
           Name: SHIP_MORAY                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Moray
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_moray.md)
     Variations: See [code variations](../related/compare/main/variable/ship_moray.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_MORAY
    
    
    
    
    
    
    .SHIP_MORAY
    
     EQUB 1                 \ Max. canisters on demise = 1
     EQUW 30 * 30           \ Targetable area          = 30 * 30
    
     EQUB LO(SHIP_MORAY_EDGES - SHIP_MORAY)            \ Edges data offset (low)
     EQUB LO(SHIP_MORAY_FACES - SHIP_MORAY)            \ Faces data offset (low)
    
     EQUB 73                \ Max. edge count          = (73 - 1) / 4 = 18
     EQUB 0                 \ Gun vertex               = 0
     EQUB 26                \ Explosion count          = 5, as (4 * n) + 6 = 26
     EQUB 84                \ Number of vertices       = 84 / 6 = 14
     EQUB 19                \ Number of edges          = 19
     EQUW 50                \ Bounty                   = 50
     EQUB 36                \ Number of faces          = 36 / 4 = 9
     EQUB 40                \ Visibility distance      = 40
     EQUB 100               \ Max. energy              = 100
     EQUB 25                \ Max. speed               = 25
    
     EQUB HI(SHIP_MORAY_EDGES - SHIP_MORAY)            \ Edges data offset (high)
     EQUB HI(SHIP_MORAY_FACES - SHIP_MORAY)            \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00010000         \ Laser power              = 2
                            \ Missiles                 = 0
    
    .SHIP_MORAY_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   15,    0,   65,     2,      0,    8,     7,         31    \ Vertex 0
     VERTEX  -15,    0,   65,     1,      0,    7,     6,         31    \ Vertex 1
     VERTEX    0,   18,  -40,    15,     15,   15,    15,         17    \ Vertex 2
     VERTEX  -60,    0,    0,     3,      1,    6,     6,         31    \ Vertex 3
     VERTEX   60,    0,    0,     5,      2,    8,     8,         31    \ Vertex 4
     VERTEX   30,  -27,  -10,     5,      4,    8,     7,         24    \ Vertex 5
     VERTEX  -30,  -27,  -10,     4,      3,    7,     6,         24    \ Vertex 6
     VERTEX   -9,   -4,  -25,     4,      4,    4,     4,          7    \ Vertex 7
     VERTEX    9,   -4,  -25,     4,      4,    4,     4,          7    \ Vertex 8
     VERTEX    0,  -18,  -16,     4,      4,    4,     4,          7    \ Vertex 9
     VERTEX   13,    3,   49,     0,      0,    0,     0,          5    \ Vertex 10
     VERTEX    6,    0,   65,     0,      0,    0,     0,          5    \ Vertex 11
     VERTEX  -13,    3,   49,     0,      0,    0,     0,          5    \ Vertex 12
     VERTEX   -6,    0,   65,     0,      0,    0,     0,          5    \ Vertex 13
    
    .SHIP_MORAY_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     7,     0,         31    \ Edge 0
     EDGE       1,       3,     6,     1,         31    \ Edge 1
     EDGE       3,       6,     6,     3,         24    \ Edge 2
     EDGE       5,       6,     7,     4,         24    \ Edge 3
     EDGE       4,       5,     8,     5,         24    \ Edge 4
     EDGE       0,       4,     8,     2,         31    \ Edge 5
     EDGE       1,       6,     7,     6,         15    \ Edge 6
     EDGE       0,       5,     8,     7,         15    \ Edge 7
     EDGE       0,       2,     2,     0,         15    \ Edge 8
     EDGE       1,       2,     1,     0,         15    \ Edge 9
     EDGE       2,       3,     3,     1,         17    \ Edge 10
     EDGE       2,       4,     5,     2,         17    \ Edge 11
     EDGE       2,       5,     5,     4,         13    \ Edge 12
     EDGE       2,       6,     4,     3,         13    \ Edge 13
     EDGE       7,       8,     4,     4,          5    \ Edge 14
     EDGE       7,       9,     4,     4,          7    \ Edge 15
     EDGE       8,       9,     4,     4,          7    \ Edge 16
     EDGE      10,      11,     0,     0,          5    \ Edge 17
     EDGE      12,      13,     0,     0,          5    \ Edge 18
    
    .SHIP_MORAY_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       43,        7,         31      \ Face 0
     FACE      -10,       49,        7,         31      \ Face 1
     FACE       10,       49,        7,         31      \ Face 2
     FACE      -59,      -28,     -101,         24      \ Face 3
     FACE        0,      -52,      -78,         24      \ Face 4
     FACE       59,      -28,     -101,         24      \ Face 5
     FACE      -72,      -99,       50,         31      \ Face 6
     FACE        0,      -83,       30,         31      \ Face 7
     FACE       72,      -99,       50,         31      \ Face 8
    
    
    
    
           Name: SHIP_THARGOID                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Thargoid mothership
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_thargoid.md)
     Variations: See [code variations](../related/compare/main/variable/ship_thargoid.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_THARGOID
    
    
    
    
    
    
    .SHIP_THARGOID
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 99 * 99           \ Targetable area          = 99 * 99
    
     EQUB LO(SHIP_THARGOID_EDGES - SHIP_THARGOID)      \ Edges data offset (low)
     EQUB LO(SHIP_THARGOID_FACES - SHIP_THARGOID)      \ Faces data offset (low)
    
     EQUB 105               \ Max. edge count          = (105 - 1) / 4 = 26
     EQUB 60                \ Gun vertex               = 60 / 4 = 15
     EQUB 38                \ Explosion count          = 8, as (4 * n) + 6 = 38
     EQUB 120               \ Number of vertices       = 120 / 6 = 20
     EQUB 26                \ Number of edges          = 26
     EQUW 500               \ Bounty                   = 500
     EQUB 40                \ Number of faces          = 40 / 4 = 10
     EQUB 55                \ Visibility distance      = 55
     EQUB 240               \ Max. energy              = 240
     EQUB 39                \ Max. speed               = 39
    
     EQUB HI(SHIP_THARGOID_EDGES - SHIP_THARGOID)      \ Edges data offset (high)
     EQUB HI(SHIP_THARGOID_FACES - SHIP_THARGOID)      \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00010110         \ Laser power              = 2
                            \ Missiles                 = 6
    
    .SHIP_THARGOID_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   32,  -48,   48,     0,      4,    8,     8,         31    \ Vertex 0
     VERTEX   32,  -68,    0,     0,      1,    4,     4,         31    \ Vertex 1
     VERTEX   32,  -48,  -48,     1,      2,    4,     4,         31    \ Vertex 2
     VERTEX   32,    0,  -68,     2,      3,    4,     4,         31    \ Vertex 3
     VERTEX   32,   48,  -48,     3,      4,    5,     5,         31    \ Vertex 4
     VERTEX   32,   68,    0,     4,      5,    6,     6,         31    \ Vertex 5
     VERTEX   32,   48,   48,     4,      6,    7,     7,         31    \ Vertex 6
     VERTEX   32,    0,   68,     4,      7,    8,     8,         31    \ Vertex 7
     VERTEX  -24, -116,  116,     0,      8,    9,     9,         31    \ Vertex 8
     VERTEX  -24, -164,    0,     0,      1,    9,     9,         31    \ Vertex 9
     VERTEX  -24, -116, -116,     1,      2,    9,     9,         31    \ Vertex 10
     VERTEX  -24,    0, -164,     2,      3,    9,     9,         31    \ Vertex 11
     VERTEX  -24,  116, -116,     3,      5,    9,     9,         31    \ Vertex 12
     VERTEX  -24,  164,    0,     5,      6,    9,     9,         31    \ Vertex 13
     VERTEX  -24,  116,  116,     6,      7,    9,     9,         31    \ Vertex 14
     VERTEX  -24,    0,  164,     7,      8,    9,     9,         31    \ Vertex 15
     VERTEX  -24,   64,   80,     9,      9,    9,     9,         30    \ Vertex 16
     VERTEX  -24,   64,  -80,     9,      9,    9,     9,         30    \ Vertex 17
     VERTEX  -24,  -64,  -80,     9,      9,    9,     9,         30    \ Vertex 18
     VERTEX  -24,  -64,   80,     9,      9,    9,     9,         30    \ Vertex 19
    
    .SHIP_THARGOID_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       7,     4,     8,         31    \ Edge 0
     EDGE       0,       1,     0,     4,         31    \ Edge 1
     EDGE       1,       2,     1,     4,         31    \ Edge 2
     EDGE       2,       3,     2,     4,         31    \ Edge 3
     EDGE       3,       4,     3,     4,         31    \ Edge 4
     EDGE       4,       5,     4,     5,         31    \ Edge 5
     EDGE       5,       6,     4,     6,         31    \ Edge 6
     EDGE       6,       7,     4,     7,         31    \ Edge 7
     EDGE       0,       8,     0,     8,         31    \ Edge 8
     EDGE       1,       9,     0,     1,         31    \ Edge 9
     EDGE       2,      10,     1,     2,         31    \ Edge 10
     EDGE       3,      11,     2,     3,         31    \ Edge 11
     EDGE       4,      12,     3,     5,         31    \ Edge 12
     EDGE       5,      13,     5,     6,         31    \ Edge 13
     EDGE       6,      14,     6,     7,         31    \ Edge 14
     EDGE       7,      15,     7,     8,         31    \ Edge 15
     EDGE       8,      15,     8,     9,         31    \ Edge 16
     EDGE       8,       9,     0,     9,         31    \ Edge 17
     EDGE       9,      10,     1,     9,         31    \ Edge 18
     EDGE      10,      11,     2,     9,         31    \ Edge 19
     EDGE      11,      12,     3,     9,         31    \ Edge 20
     EDGE      12,      13,     5,     9,         31    \ Edge 21
     EDGE      13,      14,     6,     9,         31    \ Edge 22
     EDGE      14,      15,     7,     9,         31    \ Edge 23
     EDGE      16,      17,     9,     9,         30    \ Edge 24
     EDGE      18,      19,     9,     9,         30    \ Edge 25
    
    .SHIP_THARGOID_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE      103,      -60,       25,         31      \ Face 0
     FACE      103,      -60,      -25,         31      \ Face 1
     FACE      103,      -25,      -60,         31      \ Face 2
     FACE      103,       25,      -60,         31      \ Face 3
     FACE       64,        0,        0,         31      \ Face 4
     FACE      103,       60,      -25,         31      \ Face 5
     FACE      103,       60,       25,         31      \ Face 6
     FACE      103,       25,       60,         31      \ Face 7
     FACE      103,      -25,       60,         31      \ Face 8
     FACE      -48,        0,        0,         31      \ Face 9
    
    
    
    
           Name: SHIP_THARGON                                            [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Thargon
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_thargon.md)
     Variations: See [code variations](../related/compare/main/variable/ship_thargon.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_THARGON
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the Thargon reuses the edges data from the cargo
     canister, so the edges data offset is negative.
    
    
    
    
    .SHIP_THARGON
    
     EQUB 0 + (15 << 4)     \ Max. canisters on demise = 0
                            \ Market item when scooped = 15 + 1 = 16 (alien items)
     EQUW 40 * 40           \ Targetable area          = 40 * 40
    
     EQUB LO(SHIP_CANISTER_EDGES - SHIP_THARGON)       \ Edges from canister
     EQUB LO(SHIP_THARGON_FACES - SHIP_THARGON)        \ Faces data offset (low)
    
     EQUB 69                \ Max. edge count          = (69 - 1) / 4 = 17
     EQUB 0                 \ Gun vertex               = 0
     EQUB 18                \ Explosion count          = 3, as (4 * n) + 6 = 18
     EQUB 60                \ Number of vertices       = 60 / 6 = 10
     EQUB 15                \ Number of edges          = 15
     EQUW 50                \ Bounty                   = 50
     EQUB 28                \ Number of faces          = 28 / 4 = 7
     EQUB 20                \ Visibility distance      = 20
     EQUB 20                \ Max. energy              = 20
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_CANISTER_EDGES - SHIP_THARGON)       \ Edges from canister
     EQUB HI(SHIP_THARGON_FACES - SHIP_THARGON)        \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00010000         \ Laser power              = 2
                            \ Missiles                 = 0
    
    .SHIP_THARGON_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   -9,    0,   40,     1,      0,    5,     5,         31    \ Vertex 0
     VERTEX   -9,  -38,   12,     1,      0,    2,     2,         31    \ Vertex 1
     VERTEX   -9,  -24,  -32,     2,      0,    3,     3,         31    \ Vertex 2
     VERTEX   -9,   24,  -32,     3,      0,    4,     4,         31    \ Vertex 3
     VERTEX   -9,   38,   12,     4,      0,    5,     5,         31    \ Vertex 4
     VERTEX    9,    0,   -8,     5,      1,    6,     6,         31    \ Vertex 5
     VERTEX    9,  -10,  -15,     2,      1,    6,     6,         31    \ Vertex 6
     VERTEX    9,   -6,  -26,     3,      2,    6,     6,         31    \ Vertex 7
     VERTEX    9,    6,  -26,     4,      3,    6,     6,         31    \ Vertex 8
     VERTEX    9,   10,  -15,     5,      4,    6,     6,         31    \ Vertex 9
    
    .SHIP_THARGON_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE      -36,        0,        0,         31      \ Face 0
     FACE       20,       -5,        7,         31      \ Face 1
     FACE       46,      -42,      -14,         31      \ Face 2
     FACE       36,        0,     -104,         31      \ Face 3
     FACE       46,       42,      -14,         31      \ Face 4
     FACE       20,        5,        7,         31      \ Face 5
     FACE       36,        0,        0,         31      \ Face 6
    
    
    
    
           Name: SHIP_CONSTRICTOR                                        [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Constrictor
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_constrictor.md)
     Variations: See [code variations](../related/compare/main/variable/ship_constrictor.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_CONSTRICTOR
    
    
    
    
    
    
    .SHIP_CONSTRICTOR
    
     EQUB 3                 \ Max. canisters on demise = 3
     EQUW 65 * 65           \ Targetable area          = 65 * 65
    
     EQUB LO(SHIP_CONSTRICTOR_EDGES - SHIP_CONSTRICTOR)   \ Edges data offset (low)
     EQUB LO(SHIP_CONSTRICTOR_FACES - SHIP_CONSTRICTOR)   \ Faces data offset (low)
    
     EQUB 81                \ Max. edge count          = (81 - 1) / 4 = 20
     EQUB 0                 \ Gun vertex               = 0
     EQUB 46                \ Explosion count          = 10, as (4 * n) + 6 = 46
     EQUB 102               \ Number of vertices       = 102 / 6 = 17
     EQUB 24                \ Number of edges          = 24
     EQUW 0                 \ Bounty                   = 0
     EQUB 40                \ Number of faces          = 40 / 4 = 10
     EQUB 45                \ Visibility distance      = 45
     EQUB 252               \ Max. energy              = 252
     EQUB 36                \ Max. speed               = 36
    
     EQUB HI(SHIP_CONSTRICTOR_EDGES - SHIP_CONSTRICTOR)   \ Edges data offset (high)
     EQUB HI(SHIP_CONSTRICTOR_FACES - SHIP_CONSTRICTOR)   \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00110100         \ Laser power              = 6
                            \ Missiles                 = 4
    
    .SHIP_CONSTRICTOR_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   20,   -7,   80,     2,      0,    9,     9,         31    \ Vertex 0
     VERTEX  -20,   -7,   80,     1,      0,    9,     9,         31    \ Vertex 1
     VERTEX  -54,   -7,   40,     4,      1,    9,     9,         31    \ Vertex 2
     VERTEX  -54,   -7,  -40,     5,      4,    9,     8,         31    \ Vertex 3
     VERTEX  -20,   13,  -40,     6,      5,    8,     8,         31    \ Vertex 4
     VERTEX   20,   13,  -40,     7,      6,    8,     8,         31    \ Vertex 5
     VERTEX   54,   -7,  -40,     7,      3,    9,     8,         31    \ Vertex 6
     VERTEX   54,   -7,   40,     3,      2,    9,     9,         31    \ Vertex 7
     VERTEX   20,   13,    5,    15,     15,   15,    15,         31    \ Vertex 8
     VERTEX  -20,   13,    5,    15,     15,   15,    15,         31    \ Vertex 9
     VERTEX   20,   -7,   62,     9,      9,    9,     9,         18    \ Vertex 10
     VERTEX  -20,   -7,   62,     9,      9,    9,     9,         18    \ Vertex 11
     VERTEX   25,   -7,  -25,     9,      9,    9,     9,         18    \ Vertex 12
     VERTEX  -25,   -7,  -25,     9,      9,    9,     9,         18    \ Vertex 13
     VERTEX   15,   -7,  -15,     9,      9,    9,     9,         10    \ Vertex 14
     VERTEX  -15,   -7,  -15,     9,      9,    9,     9,         10    \ Vertex 15
     VERTEX    0,   -7,    0,    15,      9,    1,     0,          0    \ Vertex 16
    
    .SHIP_CONSTRICTOR_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     9,     0,         31    \ Edge 0
     EDGE       1,       2,     9,     1,         31    \ Edge 1
     EDGE       1,       9,     1,     0,         31    \ Edge 2
     EDGE       0,       8,     2,     0,         31    \ Edge 3
     EDGE       0,       7,     9,     2,         31    \ Edge 4
     EDGE       7,       8,     3,     2,         31    \ Edge 5
     EDGE       2,       9,     4,     1,         31    \ Edge 6
     EDGE       2,       3,     9,     4,         31    \ Edge 7
     EDGE       6,       7,     9,     3,         31    \ Edge 8
     EDGE       6,       8,     7,     3,         31    \ Edge 9
     EDGE       5,       8,     7,     6,         31    \ Edge 10
     EDGE       4,       9,     6,     5,         31    \ Edge 11
     EDGE       3,       9,     5,     4,         31    \ Edge 12
     EDGE       3,       4,     8,     5,         31    \ Edge 13
     EDGE       4,       5,     8,     6,         31    \ Edge 14
     EDGE       5,       6,     8,     7,         31    \ Edge 15
     EDGE       3,       6,     9,     8,         31    \ Edge 16
     EDGE       8,       9,     6,     0,         31    \ Edge 17
     EDGE      10,      12,     9,     9,         18    \ Edge 18
     EDGE      12,      14,     9,     9,          5    \ Edge 19
     EDGE      14,      10,     9,     9,         10    \ Edge 20
     EDGE      11,      15,     9,     9,         10    \ Edge 21
     EDGE      13,      15,     9,     9,          5    \ Edge 22
     EDGE      11,      13,     9,     9,         18    \ Edge 23
    
    .SHIP_CONSTRICTOR_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       55,       15,         31      \ Face 0
     FACE      -24,       75,       20,         31      \ Face 1
     FACE       24,       75,       20,         31      \ Face 2
     FACE       44,       75,        0,         31      \ Face 3
     FACE      -44,       75,        0,         31      \ Face 4
     FACE      -44,       75,        0,         31      \ Face 5
     FACE        0,       53,        0,         31      \ Face 6
     FACE       44,       75,        0,         31      \ Face 7
     FACE        0,        0,     -160,         31      \ Face 8
     FACE        0,      -27,        0,         31      \ Face 9
    
    
    
    
           Name: SHIP_COUGAR                                             [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Cougar
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
                 [The elusive Cougar](https://elite.bbcelite.com/deep_dives/the_elusive_cougar.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_cougar.md)
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_COUGAR
    
    
    
    
    
    
    .SHIP_COUGAR
    
     EQUB 3                 \ Max. canisters on demise = 3
     EQUW 70 * 70           \ Targetable area          = 70 * 70
    
     EQUB LO(SHIP_COUGAR_EDGES - SHIP_COUGAR)          \ Edges data offset (low)
     EQUB LO(SHIP_COUGAR_FACES - SHIP_COUGAR)          \ Faces data offset (low)
    
     EQUB 105               \ Max. edge count          = (105 - 1) / 4 = 26
     EQUB 0                 \ Gun vertex               = 0
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 114               \ Number of vertices       = 114 / 6 = 19
     EQUB 25                \ Number of edges          = 25
     EQUW 0                 \ Bounty                   = 0
     EQUB 24                \ Number of faces          = 24 / 4 = 6
     EQUB 34                \ Visibility distance      = 34
     EQUB 252               \ Max. energy              = 252
     EQUB 40                \ Max. speed               = 40
    
     EQUB HI(SHIP_COUGAR_EDGES - SHIP_COUGAR)          \ Edges data offset (high)
     EQUB HI(SHIP_COUGAR_FACES - SHIP_COUGAR)          \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00110100         \ Laser power              = 6
                            \ Missiles                 = 4
    
    .SHIP_COUGAR_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    5,   67,     2,      0,    4,     4,         31    \ Vertex 0
     VERTEX  -20,    0,   40,     1,      0,    2,     2,         31    \ Vertex 1
     VERTEX  -40,    0,  -40,     1,      0,    5,     5,         31    \ Vertex 2
     VERTEX    0,   14,  -40,     4,      0,    5,     5,         30    \ Vertex 3
     VERTEX    0,  -14,  -40,     2,      1,    5,     3,         30    \ Vertex 4
     VERTEX   20,    0,   40,     3,      2,    4,     4,         31    \ Vertex 5
     VERTEX   40,    0,  -40,     4,      3,    5,     5,         31    \ Vertex 6
     VERTEX  -36,    0,   56,     1,      0,    1,     1,         31    \ Vertex 7
     VERTEX  -60,    0,  -20,     1,      0,    1,     1,         31    \ Vertex 8
     VERTEX   36,    0,   56,     4,      3,    4,     4,         31    \ Vertex 9
     VERTEX   60,    0,  -20,     4,      3,    4,     4,         31    \ Vertex 10
     VERTEX    0,    7,   35,     0,      0,    4,     4,         18    \ Vertex 11
     VERTEX    0,    8,   25,     0,      0,    4,     4,         20    \ Vertex 12
     VERTEX  -12,    2,   45,     0,      0,    0,     0,         20    \ Vertex 13
     VERTEX   12,    2,   45,     4,      4,    4,     4,         20    \ Vertex 14
     VERTEX  -10,    6,  -40,     5,      5,    5,     5,         20    \ Vertex 15
     VERTEX  -10,   -6,  -40,     5,      5,    5,     5,         20    \ Vertex 16
     VERTEX   10,   -6,  -40,     5,      5,    5,     5,         20    \ Vertex 17
     VERTEX   10,    6,  -40,     5,      5,    5,     5,         20    \ Vertex 18
    
    .SHIP_COUGAR_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     2,     0,         31    \ Edge 0
     EDGE       1,       7,     1,     0,         31    \ Edge 1
     EDGE       7,       8,     1,     0,         31    \ Edge 2
     EDGE       8,       2,     1,     0,         31    \ Edge 3
     EDGE       2,       3,     5,     0,         30    \ Edge 4
     EDGE       3,       6,     5,     4,         30    \ Edge 5
     EDGE       2,       4,     5,     1,         30    \ Edge 6
     EDGE       4,       6,     5,     3,         30    \ Edge 7
     EDGE       6,      10,     4,     3,         31    \ Edge 8
     EDGE      10,       9,     4,     3,         31    \ Edge 9
     EDGE       9,       5,     4,     3,         31    \ Edge 10
     EDGE       5,       0,     4,     2,         31    \ Edge 11
     EDGE       0,       3,     4,     0,         27    \ Edge 12
     EDGE       1,       4,     2,     1,         27    \ Edge 13
     EDGE       5,       4,     3,     2,         27    \ Edge 14
     EDGE       1,       2,     1,     0,         26    \ Edge 15
     EDGE       5,       6,     4,     3,         26    \ Edge 16
     EDGE      12,      13,     0,     0,         20    \ Edge 17
     EDGE      13,      11,     0,     0,         18    \ Edge 18
     EDGE      11,      14,     4,     4,         18    \ Edge 19
     EDGE      14,      12,     4,     4,         20    \ Edge 20
     EDGE      15,      16,     5,     5,         18    \ Edge 21
     EDGE      16,      18,     5,     5,         20    \ Edge 22
     EDGE      18,      17,     5,     5,         18    \ Edge 23
     EDGE      17,      15,     5,     5,         20    \ Edge 24
    
    .SHIP_COUGAR_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE      -16,       46,        4,         31      \ Face 0
     FACE      -16,      -46,        4,         31      \ Face 1
     FACE        0,      -27,        5,         31      \ Face 2
     FACE       16,      -46,        4,         31      \ Face 3
     FACE       16,       46,        4,         31      \ Face 4
     FACE        0,        0,     -160,         30      \ Face 5
    
    
    
    
           Name: SHIP_DODO                                               [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Dodecahedron ("Dodo") space station
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [on its own page](../game_data/variable/ship_dodo.md)
     Variations: See [code variations](../related/compare/main/variable/ship_dodo.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](../game_data/variable/xx21.md) uses SHIP_DODO
    
    
    
    
    
    
    .SHIP_DODO
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 180 * 180         \ Targetable area          = 180 * 180
    
     EQUB LO(SHIP_DODO_EDGES - SHIP_DODO)              \ Edges data offset (low)
     EQUB LO(SHIP_DODO_FACES - SHIP_DODO)              \ Faces data offset (low)
    
     EQUB 101               \ Max. edge count          = (101 - 1) / 4 = 25
     EQUB 0                 \ Gun vertex               = 0
     EQUB 54                \ Explosion count          = 12, as (4 * n) + 6 = 54
     EQUB 144               \ Number of vertices       = 144 / 6 = 24
     EQUB 34                \ Number of edges          = 34
     EQUW 0                 \ Bounty                   = 0
     EQUB 48                \ Number of faces          = 48 / 4 = 12
     EQUB 125               \ Visibility distance      = 125
     EQUB 240               \ Max. energy              = 240
     EQUB 0                 \ Max. speed               = 0
    
     EQUB HI(SHIP_DODO_EDGES - SHIP_DODO)              \ Edges data offset (high)
     EQUB HI(SHIP_DODO_FACES - SHIP_DODO)              \ Faces data offset (high)
    
     EQUB 0                 \ Normals are scaled by    = 2^0 = 1
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_DODO_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,  150,  196,     1,      0,    5,     5,         31    \ Vertex 0
     VERTEX  143,   46,  196,     1,      0,    2,     2,         31    \ Vertex 1
     VERTEX   88, -121,  196,     2,      0,    3,     3,         31    \ Vertex 2
     VERTEX  -88, -121,  196,     3,      0,    4,     4,         31    \ Vertex 3
     VERTEX -143,   46,  196,     4,      0,    5,     5,         31    \ Vertex 4
     VERTEX    0,  243,   46,     5,      1,    6,     6,         31    \ Vertex 5
     VERTEX  231,   75,   46,     2,      1,    7,     7,         31    \ Vertex 6
     VERTEX  143, -196,   46,     3,      2,    8,     8,         31    \ Vertex 7
     VERTEX -143, -196,   46,     4,      3,    9,     9,         31    \ Vertex 8
     VERTEX -231,   75,   46,     5,      4,   10,    10,         31    \ Vertex 9
     VERTEX  143,  196,  -46,     6,      1,    7,     7,         31    \ Vertex 10
     VERTEX  231,  -75,  -46,     7,      2,    8,     8,         31    \ Vertex 11
     VERTEX    0, -243,  -46,     8,      3,    9,     9,         31    \ Vertex 12
     VERTEX -231,  -75,  -46,     9,      4,   10,    10,         31    \ Vertex 13
     VERTEX -143,  196,  -46,     6,      5,   10,    10,         31    \ Vertex 14
     VERTEX   88,  121, -196,     7,      6,   11,    11,         31    \ Vertex 15
     VERTEX  143,  -46, -196,     8,      7,   11,    11,         31    \ Vertex 16
     VERTEX    0, -150, -196,     9,      8,   11,    11,         31    \ Vertex 17
     VERTEX -143,  -46, -196,    10,      9,   11,    11,         31    \ Vertex 18
     VERTEX  -88,  121, -196,    10,      6,   11,    11,         31    \ Vertex 19
     VERTEX  -16,   32,  196,     0,      0,    0,     0,         30    \ Vertex 20
     VERTEX  -16,  -32,  196,     0,      0,    0,     0,         30    \ Vertex 21
     VERTEX   16,   32,  196,     0,      0,    0,     0,         23    \ Vertex 22
     VERTEX   16,  -32,  196,     0,      0,    0,     0,         23    \ Vertex 23
    
    .SHIP_DODO_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     1,     0,         31    \ Edge 0
     EDGE       1,       2,     2,     0,         31    \ Edge 1
     EDGE       2,       3,     3,     0,         31    \ Edge 2
     EDGE       3,       4,     4,     0,         31    \ Edge 3
     EDGE       4,       0,     5,     0,         31    \ Edge 4
     EDGE       5,      10,     6,     1,         31    \ Edge 5
     EDGE      10,       6,     7,     1,         31    \ Edge 6
     EDGE       6,      11,     7,     2,         31    \ Edge 7
     EDGE      11,       7,     8,     2,         31    \ Edge 8
     EDGE       7,      12,     8,     3,         31    \ Edge 9
     EDGE      12,       8,     9,     3,         31    \ Edge 10
     EDGE       8,      13,     9,     4,         31    \ Edge 11
     EDGE      13,       9,    10,     4,         31    \ Edge 12
     EDGE       9,      14,    10,     5,         31    \ Edge 13
     EDGE      14,       5,     6,     5,         31    \ Edge 14
     EDGE      15,      16,    11,     7,         31    \ Edge 15
     EDGE      16,      17,    11,     8,         31    \ Edge 16
     EDGE      17,      18,    11,     9,         31    \ Edge 17
     EDGE      18,      19,    11,    10,         31    \ Edge 18
     EDGE      19,      15,    11,     6,         31    \ Edge 19
     EDGE       0,       5,     5,     1,         31    \ Edge 20
     EDGE       1,       6,     2,     1,         31    \ Edge 21
     EDGE       2,       7,     3,     2,         31    \ Edge 22
     EDGE       3,       8,     4,     3,         31    \ Edge 23
     EDGE       4,       9,     5,     4,         31    \ Edge 24
     EDGE      10,      15,     7,     6,         31    \ Edge 25
     EDGE      11,      16,     8,     7,         31    \ Edge 26
     EDGE      12,      17,     9,     8,         31    \ Edge 27
     EDGE      13,      18,    10,     9,         31    \ Edge 28
     EDGE      14,      19,    10,     6,         31    \ Edge 29
     EDGE      20,      21,     0,     0,         30    \ Edge 30
     EDGE      21,      23,     0,     0,         20    \ Edge 31
     EDGE      23,      22,     0,     0,         23    \ Edge 32
     EDGE      22,      20,     0,     0,         20    \ Edge 33
    
    .SHIP_DODO_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,        0,      196,         31      \ Face 0
     FACE      103,      142,       88,         31      \ Face 1
     FACE      169,      -55,       89,         31      \ Face 2
     FACE        0,     -176,       88,         31      \ Face 3
     FACE     -169,      -55,       89,         31      \ Face 4
     FACE     -103,      142,       88,         31      \ Face 5
     FACE        0,      176,      -88,         31      \ Face 6
     FACE      169,       55,      -89,         31      \ Face 7
     FACE      103,     -142,      -88,         31      \ Face 8
     FACE     -103,     -142,      -88,         31      \ Face 9
     FACE     -169,       55,      -89,         31      \ Face 10
     FACE        0,        0,     -196,         31      \ Face 11
    
    
    

[X]

Macro [EDGE](elite_data.md#header-edge) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](elite_data.md#header-face) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_ADDER](elite_data.md#header-ship_adder) (category: Drawing ships)

Ship blueprint for an Adder

[X]

Label [SHIP_ADDER_EDGES](elite_data.md#ship_adder_edges) in variable [SHIP_ADDER](elite_data.md#header-ship_adder)

[X]

Label [SHIP_ADDER_FACES](elite_data.md#ship_adder_faces) in variable [SHIP_ADDER](elite_data.md#header-ship_adder)

[X]

Variable [SHIP_ANACONDA](elite_data.md#header-ship_anaconda) (category: Drawing ships)

Ship blueprint for an Anaconda

[X]

Label [SHIP_ANACONDA_EDGES](elite_data.md#ship_anaconda_edges) in variable [SHIP_ANACONDA](elite_data.md#header-ship_anaconda)

[X]

Label [SHIP_ANACONDA_FACES](elite_data.md#ship_anaconda_faces) in variable [SHIP_ANACONDA](elite_data.md#header-ship_anaconda)

[X]

Variable [SHIP_ASP_MK_2](elite_data.md#header-ship_asp_mk_2) (category: Drawing ships)

Ship blueprint for an Asp Mk II

[X]

Label [SHIP_ASP_MK_2_EDGES](elite_data.md#ship_asp_mk_2_edges) in variable [SHIP_ASP_MK_2](elite_data.md#header-ship_asp_mk_2)

[X]

Label [SHIP_ASP_MK_2_FACES](elite_data.md#ship_asp_mk_2_faces) in variable [SHIP_ASP_MK_2](elite_data.md#header-ship_asp_mk_2)

[X]

Variable [SHIP_ASTEROID](elite_data.md#header-ship_asteroid) (category: Drawing ships)

Ship blueprint for an asteroid

[X]

Label [SHIP_ASTEROID_EDGES](elite_data.md#ship_asteroid_edges) in variable [SHIP_ASTEROID](elite_data.md#header-ship_asteroid)

[X]

Label [SHIP_ASTEROID_FACES](elite_data.md#ship_asteroid_faces) in variable [SHIP_ASTEROID](elite_data.md#header-ship_asteroid)

[X]

Variable [SHIP_BOA](elite_data.md#header-ship_boa) (category: Drawing ships)

Ship blueprint for a Boa

[X]

Label [SHIP_BOA_EDGES](elite_data.md#ship_boa_edges) in variable [SHIP_BOA](elite_data.md#header-ship_boa)

[X]

Label [SHIP_BOA_FACES](elite_data.md#ship_boa_faces) in variable [SHIP_BOA](elite_data.md#header-ship_boa)

[X]

Variable [SHIP_BOULDER](elite_data.md#header-ship_boulder) (category: Drawing ships)

Ship blueprint for a boulder

[X]

Label [SHIP_BOULDER_EDGES](elite_data.md#ship_boulder_edges) in variable [SHIP_BOULDER](elite_data.md#header-ship_boulder)

[X]

Label [SHIP_BOULDER_FACES](elite_data.md#ship_boulder_faces) in variable [SHIP_BOULDER](elite_data.md#header-ship_boulder)

[X]

Variable [SHIP_CANISTER](elite_data.md#header-ship_canister) (category: Drawing ships)

Ship blueprint for a cargo canister

[X]

Label [SHIP_CANISTER_EDGES](elite_data.md#ship_canister_edges) in variable [SHIP_CANISTER](elite_data.md#header-ship_canister)

[X]

Label [SHIP_CANISTER_FACES](elite_data.md#ship_canister_faces) in variable [SHIP_CANISTER](elite_data.md#header-ship_canister)

[X]

Variable [SHIP_COBRA_MK_1](elite_data.md#header-ship_cobra_mk_1) (category: Drawing ships)

Ship blueprint for a Cobra Mk I

[X]

Label [SHIP_COBRA_MK_1_EDGES](elite_data.md#ship_cobra_mk_1_edges) in variable [SHIP_COBRA_MK_1](elite_data.md#header-ship_cobra_mk_1)

[X]

Label [SHIP_COBRA_MK_1_FACES](elite_data.md#ship_cobra_mk_1_faces) in variable [SHIP_COBRA_MK_1](elite_data.md#header-ship_cobra_mk_1)

[X]

Variable [SHIP_COBRA_MK_3](elite_data.md#header-ship_cobra_mk_3) (category: Drawing ships)

Ship blueprint for a Cobra Mk III

[X]

Label [SHIP_COBRA_MK_3_EDGES](elite_data.md#ship_cobra_mk_3_edges) in variable [SHIP_COBRA_MK_3](elite_data.md#header-ship_cobra_mk_3)

[X]

Label [SHIP_COBRA_MK_3_FACES](elite_data.md#ship_cobra_mk_3_faces) in variable [SHIP_COBRA_MK_3](elite_data.md#header-ship_cobra_mk_3)

[X]

Variable [SHIP_COBRA_MK_3_P](elite_data.md#header-ship_cobra_mk_3_p) (category: Drawing ships)

Ship blueprint for a Cobra Mk III (pirate)

[X]

Variable [SHIP_CONSTRICTOR](elite_data.md#header-ship_constrictor) (category: Drawing ships)

Ship blueprint for a Constrictor

[X]

Label [SHIP_CONSTRICTOR_EDGES](elite_data.md#ship_constrictor_edges) in variable [SHIP_CONSTRICTOR](elite_data.md#header-ship_constrictor)

[X]

Label [SHIP_CONSTRICTOR_FACES](elite_data.md#ship_constrictor_faces) in variable [SHIP_CONSTRICTOR](elite_data.md#header-ship_constrictor)

[X]

Variable [SHIP_CORIOLIS](elite_data.md#header-ship_coriolis) (category: Drawing ships)

Ship blueprint for a Coriolis space station

[X]

Label [SHIP_CORIOLIS_EDGES](elite_data.md#ship_coriolis_edges) in variable [SHIP_CORIOLIS](elite_data.md#header-ship_coriolis)

[X]

Label [SHIP_CORIOLIS_FACES](elite_data.md#ship_coriolis_faces) in variable [SHIP_CORIOLIS](elite_data.md#header-ship_coriolis)

[X]

Variable [SHIP_COUGAR](elite_data.md#header-ship_cougar) (category: Drawing ships)

Ship blueprint for a Cougar

[X]

Label [SHIP_COUGAR_EDGES](elite_data.md#ship_cougar_edges) in variable [SHIP_COUGAR](elite_data.md#header-ship_cougar)

[X]

Label [SHIP_COUGAR_FACES](elite_data.md#ship_cougar_faces) in variable [SHIP_COUGAR](elite_data.md#header-ship_cougar)

[X]

Variable [SHIP_DODO](elite_data.md#header-ship_dodo) (category: Drawing ships)

Ship blueprint for a Dodecahedron ("Dodo") space station

[X]

Label [SHIP_DODO_EDGES](elite_data.md#ship_dodo_edges) in variable [SHIP_DODO](elite_data.md#header-ship_dodo)

[X]

Label [SHIP_DODO_FACES](elite_data.md#ship_dodo_faces) in variable [SHIP_DODO](elite_data.md#header-ship_dodo)

[X]

Variable [SHIP_ESCAPE_POD](elite_data.md#header-ship_escape_pod) (category: Drawing ships)

Ship blueprint for an escape pod

[X]

Label [SHIP_ESCAPE_POD_EDGES](elite_data.md#ship_escape_pod_edges) in variable [SHIP_ESCAPE_POD](elite_data.md#header-ship_escape_pod)

[X]

Label [SHIP_ESCAPE_POD_FACES](elite_data.md#ship_escape_pod_faces) in variable [SHIP_ESCAPE_POD](elite_data.md#header-ship_escape_pod)

[X]

Variable [SHIP_FER_DE_LANCE](elite_data.md#header-ship_fer_de_lance) (category: Drawing ships)

Ship blueprint for a Fer-de-Lance

[X]

Label [SHIP_FER_DE_LANCE_EDGES](elite_data.md#ship_fer_de_lance_edges) in variable [SHIP_FER_DE_LANCE](elite_data.md#header-ship_fer_de_lance)

[X]

Label [SHIP_FER_DE_LANCE_FACES](elite_data.md#ship_fer_de_lance_faces) in variable [SHIP_FER_DE_LANCE](elite_data.md#header-ship_fer_de_lance)

[X]

Variable [SHIP_GECKO](elite_data.md#header-ship_gecko) (category: Drawing ships)

Ship blueprint for a Gecko

[X]

Label [SHIP_GECKO_EDGES](elite_data.md#ship_gecko_edges) in variable [SHIP_GECKO](elite_data.md#header-ship_gecko)

[X]

Label [SHIP_GECKO_FACES](elite_data.md#ship_gecko_faces) in variable [SHIP_GECKO](elite_data.md#header-ship_gecko)

[X]

Variable [SHIP_KRAIT](elite_data.md#header-ship_krait) (category: Drawing ships)

Ship blueprint for a Krait

[X]

Label [SHIP_KRAIT_EDGES](elite_data.md#ship_krait_edges) in variable [SHIP_KRAIT](elite_data.md#header-ship_krait)

[X]

Label [SHIP_KRAIT_FACES](elite_data.md#ship_krait_faces) in variable [SHIP_KRAIT](elite_data.md#header-ship_krait)

[X]

Variable [SHIP_MAMBA](elite_data.md#header-ship_mamba) (category: Drawing ships)

Ship blueprint for a Mamba

[X]

Label [SHIP_MAMBA_EDGES](elite_data.md#ship_mamba_edges) in variable [SHIP_MAMBA](elite_data.md#header-ship_mamba)

[X]

Label [SHIP_MAMBA_FACES](elite_data.md#ship_mamba_faces) in variable [SHIP_MAMBA](elite_data.md#header-ship_mamba)

[X]

Variable [SHIP_MISSILE](elite_data.md#header-ship_missile) (category: Drawing ships)

Ship blueprint for a missile

[X]

Label [SHIP_MISSILE_EDGES](elite_data.md#ship_missile_edges) in variable [SHIP_MISSILE](elite_data.md#header-ship_missile)

[X]

Label [SHIP_MISSILE_FACES](elite_data.md#ship_missile_faces) in variable [SHIP_MISSILE](elite_data.md#header-ship_missile)

[X]

Variable [SHIP_MORAY](elite_data.md#header-ship_moray) (category: Drawing ships)

Ship blueprint for a Moray

[X]

Label [SHIP_MORAY_EDGES](elite_data.md#ship_moray_edges) in variable [SHIP_MORAY](elite_data.md#header-ship_moray)

[X]

Label [SHIP_MORAY_FACES](elite_data.md#ship_moray_faces) in variable [SHIP_MORAY](elite_data.md#header-ship_moray)

[X]

Variable [SHIP_PLATE](elite_data.md#header-ship_plate) (category: Drawing ships)

Ship blueprint for an alloy plate

[X]

Label [SHIP_PLATE_EDGES](elite_data.md#ship_plate_edges) in variable [SHIP_PLATE](elite_data.md#header-ship_plate)

[X]

Label [SHIP_PLATE_FACES](elite_data.md#ship_plate_faces) in variable [SHIP_PLATE](elite_data.md#header-ship_plate)

[X]

Variable [SHIP_PYTHON](elite_data.md#header-ship_python) (category: Drawing ships)

Ship blueprint for a Python

[X]

Label [SHIP_PYTHON_EDGES](elite_data.md#ship_python_edges) in variable [SHIP_PYTHON](elite_data.md#header-ship_python)

[X]

Label [SHIP_PYTHON_FACES](elite_data.md#ship_python_faces) in variable [SHIP_PYTHON](elite_data.md#header-ship_python)

[X]

Variable [SHIP_PYTHON_P](elite_data.md#header-ship_python_p) (category: Drawing ships)

Ship blueprint for a Python (pirate)

[X]

Variable [SHIP_ROCK_HERMIT](elite_data.md#header-ship_rock_hermit) (category: Drawing ships)

Ship blueprint for a rock hermit (asteroid)

[X]

Variable [SHIP_SHUTTLE](elite_data.md#header-ship_shuttle) (category: Drawing ships)

Ship blueprint for a Shuttle

[X]

Label [SHIP_SHUTTLE_EDGES](elite_data.md#ship_shuttle_edges) in variable [SHIP_SHUTTLE](elite_data.md#header-ship_shuttle)

[X]

Label [SHIP_SHUTTLE_FACES](elite_data.md#ship_shuttle_faces) in variable [SHIP_SHUTTLE](elite_data.md#header-ship_shuttle)

[X]

Variable [SHIP_SIDEWINDER](elite_data.md#header-ship_sidewinder) (category: Drawing ships)

Ship blueprint for a Sidewinder

[X]

Label [SHIP_SIDEWINDER_EDGES](elite_data.md#ship_sidewinder_edges) in variable [SHIP_SIDEWINDER](elite_data.md#header-ship_sidewinder)

[X]

Label [SHIP_SIDEWINDER_FACES](elite_data.md#ship_sidewinder_faces) in variable [SHIP_SIDEWINDER](elite_data.md#header-ship_sidewinder)

[X]

Variable [SHIP_SPLINTER](elite_data.md#header-ship_splinter) (category: Drawing ships)

Ship blueprint for a splinter

[X]

Label [SHIP_SPLINTER_FACES](elite_data.md#ship_splinter_faces) in variable [SHIP_SPLINTER](elite_data.md#header-ship_splinter)

[X]

Variable [SHIP_THARGOID](elite_data.md#header-ship_thargoid) (category: Drawing ships)

Ship blueprint for a Thargoid mothership

[X]

Label [SHIP_THARGOID_EDGES](elite_data.md#ship_thargoid_edges) in variable [SHIP_THARGOID](elite_data.md#header-ship_thargoid)

[X]

Label [SHIP_THARGOID_FACES](elite_data.md#ship_thargoid_faces) in variable [SHIP_THARGOID](elite_data.md#header-ship_thargoid)

[X]

Variable [SHIP_THARGON](elite_data.md#header-ship_thargon) (category: Drawing ships)

Ship blueprint for a Thargon

[X]

Label [SHIP_THARGON_FACES](elite_data.md#ship_thargon_faces) in variable [SHIP_THARGON](elite_data.md#header-ship_thargon)

[X]

Variable [SHIP_TRANSPORTER](elite_data.md#header-ship_transporter) (category: Drawing ships)

Ship blueprint for a Transporter

[X]

Label [SHIP_TRANSPORTER_EDGES](elite_data.md#ship_transporter_edges) in variable [SHIP_TRANSPORTER](elite_data.md#header-ship_transporter)

[X]

Label [SHIP_TRANSPORTER_FACES](elite_data.md#ship_transporter_faces) in variable [SHIP_TRANSPORTER](elite_data.md#header-ship_transporter)

[X]

Variable [SHIP_VIPER](elite_data.md#header-ship_viper) (category: Drawing ships)

Ship blueprint for a Viper

[X]

Label [SHIP_VIPER_EDGES](elite_data.md#ship_viper_edges) in variable [SHIP_VIPER](elite_data.md#header-ship_viper)

[X]

Label [SHIP_VIPER_FACES](elite_data.md#ship_viper_faces) in variable [SHIP_VIPER](elite_data.md#header-ship_viper)

[X]

Variable [SHIP_WORM](elite_data.md#header-ship_worm) (category: Drawing ships)

Ship blueprint for a Worm

[X]

Label [SHIP_WORM_EDGES](elite_data.md#ship_worm_edges) in variable [SHIP_WORM](elite_data.md#header-ship_worm)

[X]

Label [SHIP_WORM_FACES](elite_data.md#ship_worm_faces) in variable [SHIP_WORM](elite_data.md#header-ship_worm)

[X]

Macro [VERTEX](elite_data.md#header-vertex) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[X]

Configuration variable [ax](elite_data.md#ax) in macro [VERTEX](elite_data.md#header-vertex)

[X]

Configuration variable [ay](elite_data.md#ay) in macro [VERTEX](elite_data.md#header-vertex)

[X]

Configuration variable [az](elite_data.md#az) in macro [VERTEX](elite_data.md#header-vertex)

[X]

Configuration variable [f](elite_data.md#f) in macro [EDGE](elite_data.md#header-edge)

[X]

Configuration variable [f1](elite_data.md#f1) in macro [VERTEX](elite_data.md#header-vertex)

[X]

Configuration variable [f2](elite_data.md#f2) in macro [VERTEX](elite_data.md#header-vertex)

[X]

Configuration variable [s](elite_data.md#s) in macro [VERTEX](elite_data.md#header-vertex)

[X]

[X]

[X]

[Game data](elite_data.md "Previous routine")[Text tokens](text_tokens.md "Next routine")
