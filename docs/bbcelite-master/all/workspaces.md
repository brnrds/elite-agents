---
title: "Workspaces and configuration in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/workspaces.html"
---

[Loader source](loader.md "Previous routine")[Elite A source](elite_a.md "Next routine")
    
    
     BBC MASTER ELITE MAIN GAME SOURCE
    
     BBC Master Elite was written by Ian Bell and David Braben and is copyright
     Acornsoft 1986
    
     The code in this file has been reconstructed from a disassembly of the version
     released on Ian Bell's personal website at <http://www.elitehomepage.org/>
    
     The commentary is copyright Mark Moxon, and any misunderstandings or mistakes
     in the documentation are entirely my fault
    
     The terminology and notations used in this commentary are explained at
     <https://elite.bbcelite.com/terminology>
    
     The deep dive articles referred to in this commentary can be found at
     <https://elite.bbcelite.com/deep_dives>
    
    
    
    * * *
    
    
     This source file contains the main game code for BBC Master Elite.
    
    
    
    * * *
    
    
     This source file produces the following binary file:
    
       * BCODE.bin
    
    
    
    
     INCLUDE "1-source-files/main-sources/elite-build-options.asm"
    
     CPU 1                  \ Switch to 65SC12 assembly, as this code runs on a
                            \ BBC Master
    
     _SNG47                 = (_VARIANT = 1)
     _COMPACT               = (_VARIANT = 2)
    
     GUARD &C000            \ Guard against assembling over MOS memory
    
    
    
     Configuration variables
    
    
    
    
     CODE% = &1300          \ The address where the code will be run
    
     LOAD% = &1300          \ The address where the code will be loaded
    
     Q% = _MAX_COMMANDER    \ Set Q% to TRUE to max out the default commander, FALSE
                            \ for the standard default commander
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NA2%](../main/variable/na2_per_cent.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     NOST = 20              \ The number of stardust particles in normal space (this
                            \ goes down to 3 in witchspace)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [WP](../main/workspace/wp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     NOSH = 12              \ The maximum number of ships in our local bubble of
                            \ universe
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [K%](../main/workspace/k_per_cent.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [WP](../main/workspace/wp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     NTY = 33               \ The number of different ship types
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [WP](../main/workspace/wp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     MSL = 1                \ Ship type for a missile
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FRMIS](../main/subroutine/frmis.md)
                            \   * [KS2](../main/subroutine/ks2.md)
                            \   * [Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md)
                            \   * [MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md)
                            \   * [SFRMIS](../main/subroutine/sfrmis.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SST = 2                \ Ship type for a Coriolis space station
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../main/subroutine/angry.md)
                            \   * [BEGIN](../main/subroutine/begin.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [NWSPS](../main/subroutine/nwsps.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [WP](../main/workspace/wp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     ESC = 3                \ Ship type for an escape pod
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [SESCP](../main/subroutine/sescp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     PLT = 4                \ Ship type for an alloy plate
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \   * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     OIL = 5                \ Ship type for a cargo canister
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [HATB](../main/variable/hatb.md)
                            \   * [Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     AST = 7                \ Ship type for an asteroid
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SPL = 8                \ Ship type for a splinter
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SHU = 9                \ Ship type for a Shuttle
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     CYL = 11               \ Ship type for a Cobra Mk III
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../main/subroutine/angry.md)
                            \   * [BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md)
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     ANA = 14               \ Ship type for an Anaconda
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     HER = 15               \ Ship type for a rock hermit (asteroid)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md)
                            \   * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     COPS = 16              \ Ship type for a Viper
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HATB](../main/variable/hatb.md)
                            \   * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SH3 = 17               \ Ship type for a Sidewinder
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HALL](../main/subroutine/hall.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     KRA = 19               \ Ship type for a Krait
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HATB](../main/variable/hatb.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     ADA = 20               \ Ship type for an Adder
    
     WRM = 23               \ Ship type for a Worm
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     CYL2 = 24              \ Ship type for a Cobra Mk III (pirate)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     ASP = 25               \ Ship type for an Asp Mk II
    
     THG = 29               \ Ship type for a Thargoid
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [GTHG](../main/subroutine/gthg.md)
                            \   * [Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [MJP](../main/subroutine/mjp.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
                            \   * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     TGL = 30               \ Ship type for a Thargon
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [GTHG](../main/subroutine/gthg.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
                            \   * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     CON = 31               \ Ship type for a Constrictor
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BRIEF](../main/subroutine/brief.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     COU = 32               \ Ship type for a Cougar
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     DOD = 33               \ Ship type for a Dodecahedron ("Dodo") space station
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NWSPS](../main/subroutine/nwsps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     JL = ESC               \ Junk is defined as starting from the escape pod
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     JH = SHU+2             \ Junk is defined as ending before the Cobra Mk III
                            \
                            \ So junk is defined as the following: escape pod,
                            \ alloy plate, cargo canister, asteroid, splinter,
                            \ Shuttle or Transporter
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     PACK = SH3             \ The first of the eight pack-hunter ships, which tend
                            \ to spawn in groups. With the default value of PACK the
                            \ pack-hunters are the Sidewinder, Mamba, Krait, Adder,
                            \ Gecko, Cobra Mk I, Worm and Cobra Mk III (pirate)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     POW = 15               \ Pulse laser power
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [NA2%](../main/variable/na2_per_cent.md)
                            \   * [refund](../main/subroutine/refund.md)
                            \   * [SIGHT](../main/subroutine/sight.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     Mlas = 50              \ Mining laser power
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [NA2%](../main/variable/na2_per_cent.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     Armlas = INT(128.5 + 1.5*POW)  \ Military laser power
                                    \
                                    \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [NA2%](../main/variable/na2_per_cent.md)
                            \   * [refund](../main/subroutine/refund.md)
                            \   * [SIGHT](../main/subroutine/sight.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     NI% = 37               \ The number of bytes in each ship's data block (as
                            \ stored in INWK and K%)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../main/subroutine/angry.md)
                            \   * [DCS1](../main/subroutine/dcs1.md)
                            \   * [K%](../main/workspace/k_per_cent.md)
                            \   * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 6 of 16)](../main/subroutine/main_flight_loop_part_6_of_16.md)
                            \   * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \   * [SPS4](../main/subroutine/sps4.md)
                            \   * [TAS4](../main/subroutine/tas4.md)
                            \   * [UNIV](../main/variable/univ.md)
                            \   * [VCSU1](../main/subroutine/vcsu1.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \   * [ZINF](../main/subroutine/zinf.md)
                            \   * [ZP](../main/workspace/zp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     X = 128                \ The centre x-coordinate of the 256 x 192 space view
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [PROJ](../main/subroutine/proj.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     Y = 96                 \ The centre y-coordinate of the 256 x 192 space view
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [FLFLLS](../main/subroutine/flflls.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [LL118](../main/subroutine/ll118.md)
                            \   * [LL145 (Part 1 of 4)](../main/subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 2 of 4)](../main/subroutine/ll145_part_2_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [PIXEL2](../main/subroutine/pixel2.md)
                            \   * [PROJ](../main/subroutine/proj.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [SHPPT](../main/subroutine/shppt.md)
                            \   * [SIGHT](../main/subroutine/sight.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \   * [WPLS](../main/subroutine/wpls.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     GCYT = 24              \ The y-coordinate of the top of the Long-range Chart
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DVLOIN](../main/subroutine/dvloin.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT15](../main/subroutine/tt15.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     GCYB = GCYT + 128      \ The y-coordinate of the bottom of the Long-range chart
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DVLOIN](../main/subroutine/dvloin.md)
                            \   * [TT15](../main/subroutine/tt15.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f0 = &80               \ Internal key number for red key f0 (Launch, Front)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f1 = &81               \ Internal key number for red key f1 (Buy Cargo, Rear)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f2 = &82               \ Internal key number for red key f2 (Sell Cargo, Left)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f3 = &83               \ Internal key number for red key f3 (Equip Ship, Right)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f4 = &84               \ Internal key number for red key f4 (Long-range Chart)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f5 = &85               \ Internal key number for red key f5 (Short-range Chart)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f6 = &86               \ Internal key number for red key f6 (Data on System)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f7 = &87               \ Internal key number for red key f7 (Market Price)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f8 = &88               \ Internal key number for red key f8 (Status Mode)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BAY](../main/subroutine/bay.md)
                            \   * [TT102](../main/subroutine/tt102.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     f9 = &89               \ Internal key number for red key f9 (Inventory)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT102](../main/subroutine/tt102.md)
                            \   * [TT219](../main/subroutine/tt219.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     YELLOW  = %00001111    \ Four mode 1 pixels of colour 1 (yellow)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [coltabl](../main/variable/coltabl.md)
                            \   * [me1](../main/subroutine/me1.md)
                            \   * [MESS](../main/subroutine/mess.md)
                            \   * [NLIN2](../main/subroutine/nlin2.md)
                            \   * [shpcol](../main/variable/shpcol.md)
                            \   * [sightcol](../main/variable/sightcol.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     RED     = %11110000    \ Four mode 1 pixels of colour 2 (red, magenta or white)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [coltabl](../main/variable/coltabl.md)
                            \   * [dockEd](../main/subroutine/docked.md)
                            \   * [ee3](../main/subroutine/ee3.md)
                            \   * [HAL3](../main/subroutine/hal3.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HAS2](../main/subroutine/has2.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [MT26](../main/subroutine/mt26.md)
                            \   * [shpcol](../main/variable/shpcol.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT128](../main/subroutine/tt128.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     CYAN    = %11111111    \ Four mode 1 pixels of colour 3 (cyan or white)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BOMBOFF](../main/subroutine/bomboff.md)
                            \   * [BRP](../main/subroutine/brp.md)
                            \   * [coltabl](../main/variable/coltabl.md)
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [gnum](../main/subroutine/gnum.md)
                            \   * [HME2](../main/subroutine/hme2.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [MT29](../main/subroutine/mt29.md)
                            \   * [NLIN2](../main/subroutine/nlin2.md)
                            \   * [shpcol](../main/variable/shpcol.md)
                            \   * [sightcol](../main/variable/sightcol.md)
                            \   * [TRADEMODE](../main/subroutine/trademode.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     GREEN   = %10101111    \ Four mode 1 pixels of colour 3, 1, 3, 1 (cyan/yellow)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [PLANET](../main/subroutine/planet.md)
                            \   * [TT103](../main/subroutine/tt103.md)
                            \   * [TT105](../main/subroutine/tt105.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     WHITE   = %11111010    \ Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [beamcol](../main/variable/beamcol.md)
                            \   * [PXCL](../main/variable/pxcl.md)
                            \   * [shpcol](../main/variable/shpcol.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     MAGENTA = RED          \ Four mode 1 pixels of colour 2 (red, magenta or white)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [gnum](../main/subroutine/gnum.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     DUST    = WHITE        \ Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [STARS](../main/subroutine/stars.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     RED2    = %00000011    \ Two mode 2 pixels of colour 1    (red)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [PZW](../main/subroutine/pzw.md)
                            \   * [scacol](../main/variable/scacol.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     GREEN2  = %00001100    \ Two mode 2 pixels of colour 2    (green)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [msblob](../main/subroutine/msblob.md)
                            \   * [PZW](../main/subroutine/pzw.md)
                            \   * [scacol](../main/variable/scacol.md)
                            \   * [SP2](../main/subroutine/sp2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     YELLOW2 = %00001111    \ Two mode 2 pixels of colour 3    (yellow)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [DOT](../main/subroutine/dot.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [scacol](../main/variable/scacol.md)
                            \   * [SP2](../main/subroutine/sp2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     BLUE2   = %00110000    \ Two mode 2 pixels of colour 4    (blue)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [scacol](../main/variable/scacol.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     MAG2    = %00110011    \ Two mode 2 pixels of colour 5    (magenta)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [scacol](../main/variable/scacol.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     CYAN2   = %00111100    \ Two mode 2 pixels of colour 6    (cyan)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [scacol](../main/variable/scacol.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     WHITE2  = %00111111    \ Two mode 2 pixels of colour 7    (white)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIL2](../main/subroutine/dil2.md)
                            \   * [PZW2](../main/subroutine/pzw2.md)
                            \   * [scacol](../main/variable/scacol.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     STRIPE  = %00100011    \ Two mode 2 pixels of colour 5, 1 (magenta/red)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [PZW](../main/subroutine/pzw.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     soboop  = 0            \ Sound 0  = Long, low beep
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BOOP](../main/subroutine/boop.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     sobeep  = 1            \ Sound 1  = Short, high beep
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BEEP](../main/subroutine/beep.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     soclick = 2            \ Sound 2  = This sound is not used
    
     solaser = 3            \ Sound 3  = Lasers fired by us 1
    
     soexpl  = 4            \ Sound 4  = We died / Collision / Being hit by lasers 2
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [EXNO3](../main/subroutine/exno3.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     solas2  = 5            \ Sound 5  = Lasers fired by us 2
    
     sohit   = 6            \ Sound 6  = We made a hit/kill / Other ship exploding
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EXNO](../main/subroutine/exno.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     sobomb  = 6            \ Sound 6  = Energy bomb
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BOMBEFF2](../main/subroutine/bombeff2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     soecm   = 7            \ Sound 7  = E.C.M. on
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     solaun  = 8            \ Sound 8  = Missile launched / Ship launch
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FRMIS](../main/subroutine/frmis.md)
                            \   * [LAUN](../main/subroutine/laun.md)
                            \   * [SFRMIS](../main/subroutine/sfrmis.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
                            \ Sound 9  = Being hit by lasers 1 (no variable defined
                            \            in original source code)
    
     sohyp   = 10           \ Sound 10 = Hyperspace drive engaged 1
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL164](../main/subroutine/ll164.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     sohyp2  = 11           \ Sound 11 = Hyperspace drive engaged 2
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL164](../main/subroutine/ll164.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     NRU% = 0               \ The number of planetary systems with extended system
                            \ description overrides in the RUTOK table
                            \
                            \ NRU% is set to 0 in the original source, but this is a
                            \ bug, as it should match the number of entries in the
                            \ RUGAL table
                            \
                            \ This bug causes the Data on System screen to crash the
                            \ game for a small number of systems - for example, the
                            \ game will freeze if you bring up the Data on System
                            \ screen after docking at Biarge in the first galaxy
                            \ during the Constrictor mission
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [PDESC](../main/subroutine/pdesc.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     RE = &23               \ The obfuscation byte used to hide the recursive tokens
                            \ table from crackers viewing the binary code
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ex](../main/subroutine/ex.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     VE = &57               \ The obfuscation byte used to hide the extended tokens
                            \ table from crackers viewing the binary code
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DETOK](../main/subroutine/detok.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     KEY1 = &19             \ The seed for encrypting the game code from DOENTRY to
                            \ F%
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEEOR](../main/subroutine/deeor.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     KEY2 = &62             \ The seed for encrypting the game data from XX21 to
                            \ &BBFF
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEEOR](../main/subroutine/deeor.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     LL = 30                \ The length of lines (in characters) of justified text
                            \ in the extended tokens system
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT26](../main/subroutine/tt26.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     BRKV = &0202           \ The break vector that we intercept to enable us to
                            \ handle and display system errors
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [COLD](../main/subroutine/cold.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     IRQ1V = &0204          \ The IRQ1V vector that we intercept to implement the
                            \ split-screen mode
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [SETINTS](../main/subroutine/setints.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     WRCHV = &020E          \ The WRCHV vector that we intercept with our custom
                            \ text printing routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [COLD](../main/subroutine/cold.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     LS% = &0800            \ The start of the descending ship line heap
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [RES2](../main/subroutine/res2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     TAP% = LS% - 111       \ The staging area where we copy files after loading and
                            \ before saving (though this isn't actually used in this
                            \ version, and is left-over Commodore 64 code)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LOD](../main/subroutine/lod.md)
                            \   * [rfile](../main/subroutine/rfile.md)
                            \   * [SVE](../main/subroutine/sve.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     commbuf = &0E7E        \ The file buffer where we load and save commander files
                            \ (this shares a location with LSX2 and is the address
                            \ used in the *SAVE and *LOAD OS commands)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [rfile](../main/subroutine/rfile.md)
                            \   * [wfile](../main/subroutine/wfile.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     XX21 = &8000           \ The address of the ship blueprints lookup table, as
                            \ set in elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [XX21 (Game data)](../game_data/variable/xx21.md)
    
     E% = &8042             \ The address of the default NEWB ship bytes, as set in
                            \ elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [E% (Game data)](../game_data/variable/e_per_cent.md)
    
     KWL% = &8063           \ The address of the kill tally fraction table, as set
                            \ in elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [KWL% (Game data)](../game_data/variable/kwl_per_cent.md)
    
     KWH% = &8084           \ The address of the kill tally integer table, as set in
                            \ elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [KWH% (Game data)](../game_data/variable/kwh_per_cent.md)
    
     QQ18 = &A000           \ The address of the text token table, as set in
                            \ elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [QQ18 (Game data)](../game_data/variable/qq18.md)
    
     SNE = &A3C0            \ The address of the sine lookup table, as set in
                            \ elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [SNE (Game data)](../game_data/variable/sne.md)
    
     ACT = &A3E0            \ The address of the arctan lookup table, as set in
                            \ elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [ACT (Game data)](../game_data/variable/act.md)
    
     TKN1 = &A400           \ The address of the extended token table, as set in
                            \ elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [TKN1 (Game data)](../game_data/variable/tkn1.md)
    
    IF _SNG47
    
     RUPLA = &AF48          \ The address of the extended system description system
                            \ number table, as set in elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [RUPLA (Game data)](../game_data/variable/rupla.md)
    
     RUGAL = &AF62          \ The address of the extended system description galaxy
                            \ number table, as set in elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [RUGAL (Game data)](../game_data/variable/rugal.md)
    
     RUTOK = &AF7C          \ The address of the extended system description token
                            \ table, as set in elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [RUTOK (Game data)](../game_data/variable/rutok.md)
    
    ELIF _COMPACT
    
     RUPLA = &AF43          \ The address of the extended system description system
                            \ number table, as set in elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [RUPLA (Game data)](../game_data/variable/rupla.md)
    
     RUGAL = &AF5D          \ The address of the extended system description galaxy
                            \ number table, as set in elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [RUGAL (Game data)](../game_data/variable/rugal.md)
    
     RUTOK = &AF77          \ The address of the extended system description token
                            \ table, as set in elite-data.asm
                            \
                            \ This configuration variable points to this code:
                            \
                            \   * [RUTOK (Game data)](../game_data/variable/rutok.md)
    
    ENDIF
    
     VIA = &FE00            \ Memory-mapped space for accessing internal hardware,
                            \ such as the video ULA, 6845 CRTC and 6522 VIAs (also
                            \ known as SHEILA)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [CLYNS](../main/subroutine/clyns.md)
                            \   * [DET1](../main/subroutine/det1.md)
                            \   * [DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md)
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [DKS4mc](../main/subroutine/dks4mc.md)
                            \   * [DKS5](../main/subroutine/dks5.md)
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [DOT](../main/subroutine/dot.md)
                            \   * [ECBLB](../main/subroutine/ecblb.md)
                            \   * [FILLKL](../main/subroutine/fillkl.md)
                            \   * [getzp](../main/subroutine/getzp.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [IRQ1](../main/subroutine/irq1.md)
                            \   * [LOIN](../main/subroutine/loin.md)
                            \   * [MSBAR](../main/subroutine/msbar.md)
                            \   * [PIXEL](../main/subroutine/pixel.md)
                            \   * [RDFIRE](../main/subroutine/rdfire.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SETINTS](../main/subroutine/setints.md)
                            \   * [setzp](../main/subroutine/setzp.md)
                            \   * [SOUS1](../main/subroutine/sous1.md)
                            \   * [SPBLB](../main/subroutine/spblb.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT17X](../main/subroutine/tt17x.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     OSBYTE = &FFF4         \ The address for the OSBYTE routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NMICLAIM](../main/subroutine/nmiclaim.md)
                            \   * [NMIRELEASE](../main/subroutine/nmirelease.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     OSCLI = &FFF7          \ The address for the OSCLI routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CATS](../main/subroutine/cats.md)
                            \   * [DELT](../main/subroutine/delt.md)
                            \   * [GTDIR](../main/subroutine/gtdir.md)
                            \   * [rfile](../main/subroutine/rfile.md)
                            \   * [wfile](../main/subroutine/wfile.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    
    
    
           Name: ZP                                                      [Show more]
           Type: Workspace
        Address: &0000 to &00E3
       Category: Workspaces
        Summary: Lots of important variables are stored in the zero page workspace
                 as it is quicker and more space-efficient to access memory here
    
    
        Context: See this workspace [on its own page](../main/workspace/zp.md)
     Variations: See [code variations](../related/compare/main/workspace/zp.md) for this workspace in the different versions
     References: This workspace is used as follows:
                 * [getzp](../main/subroutine/getzp.md) uses ZP
                 * [setzp](../main/subroutine/setzp.md) uses ZP
                 * [SWAPPZERO](../main/subroutine/swappzero.md) uses ZP
    
    
    
    
    
    
     ORG &0000              \ Set the assembly address to &0000
    
    .ZP
    
     SKIP 2                 \ These bytes appear to be unused
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [getzp](../main/subroutine/getzp.md)
                            \   * [setzp](../main/subroutine/setzp.md)
                            \   * [SWAPPZERO](../main/subroutine/swappzero.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    IF _COMPACT
    
    .MOS
    
     SKIP 1                 \ Determines whether we are running on a Master Compact
                            \
                            \   * 0 = This is a Master Compact
                            \
                            \   * &FF = This is not a Master Compact
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [IRQ1](../main/subroutine/irq1.md)
                            \   * [RDFIRE](../main/subroutine/rdfire.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \   * [TT17](../main/subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    ENDIF
    
    .RAND
    
     SKIP 4                 \ Four 8-bit seeds for the random number generation
                            \ system implemented in the DORND routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DORND](../main/subroutine/dornd.md)
                            \   * [Main flight loop (Part 1 of 16)](../main/subroutine/main_flight_loop_part_1_of_16.md)
                            \   * [PDESC](../main/subroutine/pdesc.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .T1
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../main/subroutine/add.md)
                            \   * [ADDK](../main/subroutine/addk.md)
                            \   * [ARCTAN](../main/subroutine/arctan.md)
                            \   * [DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md)
                            \   * [DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [DILX](../main/subroutine/dilx.md)
                            \   * [gnum](../main/subroutine/gnum.md)
                            \   * [LCASH](../main/subroutine/lcash.md)
                            \   * [MLS1](../main/subroutine/mls1.md)
                            \   * [MULT1](../main/subroutine/mult1.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [PIXEL](../main/subroutine/pixel.md)
                            \   * [refund](../main/subroutine/refund.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \   * [TIS1](../main/subroutine/tis1.md)
                            \   * [TT102](../main/subroutine/tt102.md)
                            \   * [TT219](../main/subroutine/tt219.md)
                            \   * [Ze](../main/subroutine/ze.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .T2
    
     SKIP 1                 \ This byte appears to be unused
    
    .T3
    
     SKIP 1                 \ This byte appears to be unused
    
    .T4
    
     SKIP 1                 \ This byte appears to be unused
    
    .SC
    
     SKIP 1                 \ Screen address (low byte)
                            \
                            \ Elite draws on-screen by poking bytes directly into
                            \ screen memory, and SC(1 0) is typically set to the
                            \ address of the character block containing the pixel
                            \ we want to draw
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [CLYNS](../main/subroutine/clyns.md)
                            \   * [CPIXK](../main/subroutine/cpixk.md)
                            \   * [DEEORS](../main/subroutine/deeors.md)
                            \   * [DETOK2](../main/subroutine/detok2.md)
                            \   * [DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md)
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [DIL2](../main/subroutine/dil2.md)
                            \   * [DILX](../main/subroutine/dilx.md)
                            \   * [ECBLB](../main/subroutine/ecblb.md)
                            \   * [HAL3](../main/subroutine/hal3.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HAS2](../main/subroutine/has2.md)
                            \   * [HAS3](../main/subroutine/has3.md)
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [KS2](../main/subroutine/ks2.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)
                            \   * [MSBAR](../main/subroutine/msbar.md)
                            \   * [PIXEL](../main/subroutine/pixel.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SPBLB](../main/subroutine/spblb.md)
                            \   * [TT26](../main/subroutine/tt26.md)
                            \   * [ZES1](../main/subroutine/zes1.md)
                            \   * [ZES2](../main/subroutine/zes2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SCH
    
     SKIP 1                 \ Screen address (high byte)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [MSBAR](../main/subroutine/msbar.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .P
    
     SKIP 4                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../main/subroutine/add.md)
                            \   * [ADDK](../main/subroutine/addk.md)
                            \   * [ARCTAN](../main/subroutine/arctan.md)
                            \   * [CHKON](../main/subroutine/chkon.md)
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)
                            \   * [DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DVID3B2](../main/subroutine/dvid3b2.md)
                            \   * [DVID4](../main/subroutine/dvid4.md)
                            \   * [DVID4K](../main/subroutine/dvid4k.md)
                            \   * [DVIDT](../main/subroutine/dvidt.md)
                            \   * [FMLTU](../main/subroutine/fmltu.md)
                            \   * [GC2](../main/subroutine/gc2.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HITCH](../main/subroutine/hitch.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [KS3](../main/subroutine/ks3.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \   * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)
                            \   * [MLS1](../main/subroutine/mls1.md)
                            \   * [MLTU2](../main/subroutine/mltu2.md)
                            \   * [MLU2](../main/subroutine/mlu2.md)
                            \   * [MU1](../main/subroutine/mu1.md)
                            \   * [MU11](../main/subroutine/mu11.md)
                            \   * [MU6](../main/subroutine/mu6.md)
                            \   * [MULT1](../main/subroutine/mult1.md)
                            \   * [MULT12](../main/subroutine/mult12.md)
                            \   * [MULT3](../main/subroutine/mult3.md)
                            \   * [MUT3](../main/subroutine/mut3.md)
                            \   * [MV40](../main/subroutine/mv40.md)
                            \   * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)
                            \   * [MVS4](../main/subroutine/mvs4.md)
                            \   * [MVS5](../main/subroutine/mvs5.md)
                            \   * [MVT6](../main/subroutine/mvt6.md)
                            \   * [NORM](../main/subroutine/norm.md)
                            \   * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md)
                            \   * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)
                            \   * [PLANET](../main/subroutine/planet.md)
                            \   * [PLS1](../main/subroutine/pls1.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [PLS3](../main/subroutine/pls3.md)
                            \   * [PROJ](../main/subroutine/proj.md)
                            \   * [SPS2](../main/subroutine/sps2.md)
                            \   * [SQUA2](../main/subroutine/squa2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TIS3](../main/subroutine/tis3.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT151](../main/subroutine/tt151.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT219](../main/subroutine/tt219.md)
                            \   * [TT24](../main/subroutine/tt24.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XC
    
     SKIP 1                 \ The x-coordinate of the text cursor (i.e. the text
                            \ column), which can be from 0 to 32
                            \
                            \ A value of 0 denotes the leftmost column and 32 the
                            \ rightmost column, but because the top part of the
                            \ screen (the space view) has a border box that
                            \ clashes with columns 0 and 32, text is only shown
                            \ in columns 1-31
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md)
                            \   * [CATS](../main/subroutine/cats.md)
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [dockEd](../main/subroutine/docked.md)
                            \   * [DOXC](../main/subroutine/doxc.md)
                            \   * [ee3](../main/subroutine/ee3.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [MESS](../main/subroutine/mess.md)
                            \   * [MT9](../main/subroutine/mt9.md)
                            \   * [plf2](../main/subroutine/plf2.md)
                            \   * [qv](../main/subroutine/qv.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT151](../main/subroutine/tt151.md)
                            \   * [TT167](../main/subroutine/tt167.md)
                            \   * [TT208](../main/subroutine/tt208.md)
                            \   * [TT213](../main/subroutine/tt213.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .COL
    
     SKIP 1                 \ Temporary storage, used to store colour information
                            \ when drawing pixels in the dashboard
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BOMBOFF](../main/subroutine/bomboff.md)
                            \   * [BRP](../main/subroutine/brp.md)
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [CPIXK](../main/subroutine/cpixk.md)
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [DILX](../main/subroutine/dilx.md)
                            \   * [dockEd](../main/subroutine/docked.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DOT](../main/subroutine/dot.md)
                            \   * [ee3](../main/subroutine/ee3.md)
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [gnum](../main/subroutine/gnum.md)
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [HME2](../main/subroutine/hme2.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)
                            \   * [me1](../main/subroutine/me1.md)
                            \   * [MESS](../main/subroutine/mess.md)
                            \   * [MT26](../main/subroutine/mt26.md)
                            \   * [MT29](../main/subroutine/mt29.md)
                            \   * [NLIN2](../main/subroutine/nlin2.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [PIXEL](../main/subroutine/pixel.md)
                            \   * [PLANET](../main/subroutine/planet.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SIGHT](../main/subroutine/sight.md)
                            \   * [STARS](../main/subroutine/stars.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TRADEMODE](../main/subroutine/trademode.md)
                            \   * [TT103](../main/subroutine/tt103.md)
                            \   * [TT105](../main/subroutine/tt105.md)
                            \   * [TT128](../main/subroutine/tt128.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .YC
    
     SKIP 1                 \ The y-coordinate of the text cursor (i.e. the text
                            \ row), which can be from 0 to 23
                            \
                            \ The screen actually has 31 character rows if you
                            \ include the dashboard, but the text printing routines
                            \ only work on the top part (the space view), so the
                            \ text cursor only goes up to a maximum of 23, the row
                            \ just before the screen splits
                            \
                            \ A value of 0 denotes the top row, but because the
                            \ top part of the screen has a border box that clashes
                            \ with row 0, text is always shown at row 1 or greater
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [CLYNS](../main/subroutine/clyns.md)
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [DOYC](../main/subroutine/doyc.md)
                            \   * [ee3](../main/subroutine/ee3.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [INCYC](../main/subroutine/incyc.md)
                            \   * [MESS](../main/subroutine/mess.md)
                            \   * [MT29](../main/subroutine/mt29.md)
                            \   * [NLIN](../main/subroutine/nlin.md)
                            \   * [qv](../main/subroutine/qv.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT146](../main/subroutine/tt146.md)
                            \   * [TT167](../main/subroutine/tt167.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \   * [TTX69](../main/subroutine/ttx69.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ17
    
     SKIP 1                 \ Contains a number of flags that affect how text tokens
                            \ are printed, particularly capitalisation:
                            \
                            \   * If all bits are set (255) then text printing is
                            \     disabled
                            \
                            \   * Bit 7: 0 = ALL CAPS
                            \            1 = Sentence Case, bit 6 determines the
                            \                case of the next letter to print
                            \
                            \   * Bit 6: 0 = print the next letter in upper case
                            \            1 = print the next letter in lower case
                            \
                            \   * Bits 0-5: If any of bits 0-5 are set, print in
                            \               lower case
                            \
                            \ So:
                            \
                            \   * QQ17 = 0 means case is set to ALL CAPS
                            \
                            \   * QQ17 = %10000000 means Sentence Case, currently
                            \            printing upper case
                            \
                            \   * QQ17 = %11000000 means Sentence Case, currently
                            \            printing lower case
                            \
                            \   * QQ17 = %11111111 means printing is disabled
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [CLYNS](../main/subroutine/clyns.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [MESS](../main/subroutine/mess.md)
                            \   * [MT17](../main/subroutine/mt17.md)
                            \   * [MT6](../main/subroutine/mt6.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT102](../main/subroutine/tt102.md)
                            \   * [TT167](../main/subroutine/tt167.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT219](../main/subroutine/tt219.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \   * [TT27](../main/subroutine/tt27.md)
                            \   * [TT41](../main/subroutine/tt41.md)
                            \   * [TT46](../main/subroutine/tt46.md)
                            \   * [TT69](../main/subroutine/tt69.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K3
    
     SKIP 0                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHKON](../main/subroutine/chkon.md)
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \   * [CIRCLE2](../main/subroutine/circle2.md)
                            \   * [DCS1](../main/subroutine/dcs1.md)
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [PROJ](../main/subroutine/proj.md)
                            \   * [SHPPT](../main/subroutine/shppt.md)
                            \   * [SPS3](../main/subroutine/sps3.md)
                            \   * [SPS4](../main/subroutine/sps4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [SUN (Part 4 of 4)](../main/subroutine/sun_part_4_of_4.md)
                            \   * [SWAPPZERO](../main/subroutine/swappzero.md)
                            \   * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
                            \   * [TAS1](../main/subroutine/tas1.md)
                            \   * [TAS2](../main/subroutine/tas2.md)
                            \   * [TT128](../main/subroutine/tt128.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX2
    
     SKIP 14                \ Temporary storage, used to store the visibility of the
                            \ ship's faces during the ship-drawing routine at LL9
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 4 of 12)](../main/subroutine/ll9_part_4_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K4
    
     SKIP 2                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [CHKON](../main/subroutine/chkon.md)
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)
                            \   * [PROJ](../main/subroutine/proj.md)
                            \   * [SHPPT](../main/subroutine/shppt.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [TT128](../main/subroutine/tt128.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX16
    
     SKIP 18                \ Temporary storage for a block of values, used in a
                            \ number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL51](../main/subroutine/ll51.md)
                            \   * [LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md)
                            \   * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [PLS5](../main/subroutine/pls5.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX0
    
     SKIP 2                 \ Temporary storage, used to store the address of a ship
                            \ blueprint. For example, it is used when we add a new
                            \ ship to the local bubble in routine NWSHP, and it
                            \ contains the address of the current ship's blueprint
                            \ as we loop through all the nearby ships in the main
                            \ flight loop
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HAS1](../main/subroutine/has1.md)
                            \   * [HITCH](../main/subroutine/hitch.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 4 of 12)](../main/subroutine/ll9_part_4_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \   * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [MVEIT (Part 4 of 9)](../main/subroutine/mveit_part_4_of_9.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \   * [SPIN](../main/subroutine/spin.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \   * [TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .INF
    
     SKIP 2                 \ Temporary storage, typically used for storing the
                            \ address of a ship's data block, so it can be copied
                            \ to and from the internal workspace at INWK
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../main/subroutine/angry.md)
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [GINF](../main/subroutine/ginf.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 6 of 16)](../main/subroutine/main_flight_loop_part_6_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [OOPS](../main/subroutine/oops.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \   * [WPSHPS](../main/subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .V
    
     SKIP 2                 \ Temporary storage, typically used for storing an
                            \ address pointer
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DETOK](../main/subroutine/detok.md)
                            \   * [DETOK2](../main/subroutine/detok2.md)
                            \   * [DETOK3](../main/subroutine/detok3.md)
                            \   * [ex](../main/subroutine/ex.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)
                            \   * [TAS1](../main/subroutine/tas1.md)
                            \   * [VCSU1](../main/subroutine/vcsu1.md)
                            \   * [VCSUB](../main/subroutine/vcsub.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX
    
     SKIP 2                 \ Temporary storage, typically used for storing a 16-bit
                            \ x-coordinate
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [MLS2](../main/subroutine/mls2.md)
                            \   * [MUT1](../main/subroutine/mut1.md)
                            \   * [MUT2](../main/subroutine/mut2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .YY
    
     SKIP 2                 \ Temporary storage, typically used for storing a 16-bit
                            \ y-coordinate
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EDGES](../main/subroutine/edges.md)
                            \   * [PIX1](../main/subroutine/pix1.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [SUN (Part 4 of 4)](../main/subroutine/sun_part_4_of_4.md)
                            \   * [WPLS](../main/subroutine/wpls.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SUNX
    
     SKIP 2                 \ The 16-bit x-coordinate of the vertical centre axis
                            \ of the sun (which might be off-screen)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [SUN (Part 4 of 4)](../main/subroutine/sun_part_4_of_4.md)
                            \   * [WPLS](../main/subroutine/wpls.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BETA
    
     SKIP 1                 \ The current pitch angle beta, which is reduced from
                            \ JSTY to a sign-magnitude value between -8 and +8
                            \
                            \ This describes how fast we are pitching our ship, and
                            \ determines how fast the universe pitches around us
                            \
                            \ The sign bit is also stored in BET2, while the
                            \ opposite sign is stored in BET2+1
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MV40](../main/subroutine/mv40.md)
                            \   * [MVS4](../main/subroutine/mvs4.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [RESET](../main/subroutine/reset.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BET1
    
     SKIP 1                 \ The magnitude of the pitch angle beta, i.e. |beta|,
                            \ which is a positive value between 0 and 8
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ22
    
     SKIP 2                 \ The two hyperspace countdown counters
                            \
                            \ Before a hyperspace jump, both QQ22 and QQ22+1 are
                            \ set to 15
                            \
                            \ QQ22 is an internal counter that counts down by 1
                            \ each time TT102 is called, which happens every
                            \ iteration of the main game loop. When it reaches
                            \ zero, the on-screen counter in QQ22+1 gets
                            \ decremented, and QQ22 gets set to 5 and the countdown
                            \ continues (so the first tick of the hyperspace counter
                            \ takes 15 iterations to happen, but subsequent ticks
                            \ take 5 iterations each)
                            \
                            \ QQ22+1 contains the number that's shown on-screen
                            \ during the countdown. It counts down from 15 to 1, and
                            \ when it hits 0, the hyperspace engines kick in
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [TT102](../main/subroutine/tt102.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \   * [wW](../main/subroutine/ww.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ECMA
    
     SKIP 1                 \ The E.C.M. countdown timer, which determines whether
                            \ an E.C.M. system is currently operating:
                            \
                            \   * 0 = E.C.M. is off
                            \
                            \   * Non-zero = E.C.M. is on and is counting down
                            \
                            \ The counter starts at 32 when an E.C.M. is activated,
                            \ either by us or by an opponent, and it decreases by 1
                            \ in each iteration of the main flight loop until it
                            \ reaches zero, at which point the E.C.M. switches off.
                            \ Only one E.C.M. can be active at any one time, so
                            \ there is only one counter
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ECBLB2](../main/subroutine/ecblb2.md)
                            \   * [ECMOF](../main/subroutine/ecmof.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)
                            \   * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)
                            \   * [TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ALP1
    
     SKIP 1                 \ Magnitude of the roll angle alpha, i.e. |alpha|,
                            \ which is a positive value between 0 and 31
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MLS1](../main/subroutine/mls1.md)
                            \   * [MUT3](../main/subroutine/mut3.md)
                            \   * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ALP2
    
     SKIP 2                 \ Bit 7 of ALP2 = sign of the roll angle in ALPHA
                            \
                            \ Bit 7 of ALP2+1 = opposite sign to ALP2 and ALPHA
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX15
    
     SKIP 0                 \ Temporary storage, typically used for storing screen
                            \ coordinates in line-drawing routines
                            \
                            \ There are six bytes of storage, from XX15 TO XX15+5.
                            \ The first four bytes have the following aliases:
                            \
                            \   X1 = XX15
                            \   Y1 = XX15+1
                            \   X2 = XX15+2
                            \   Y2 = XX15+3
                            \
                            \ These are typically used for describing lines in terms
                            \ of screen coordinates, i.e. (X1, Y1) to (X2, Y2)
                            \
                            \ The last two bytes of XX15 do not have aliases
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [BPRNT](../main/subroutine/bprnt.md)
                            \   * [DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [HALL](../main/subroutine/hall.md)
                            \   * [HAS1](../main/subroutine/has1.md)
                            \   * [LL118](../main/subroutine/ll118.md)
                            \   * [LL120](../main/subroutine/ll120.md)
                            \   * [LL145 (Part 1 of 4)](../main/subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 2 of 4)](../main/subroutine/ll145_part_2_of_4.md)
                            \   * [LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)
                            \   * [LL51](../main/subroutine/ll51.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../main/subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \   * [LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md)
                            \   * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [NORM](../main/subroutine/norm.md)
                            \   * [SP2](../main/subroutine/sp2.md)
                            \   * [TAS2](../main/subroutine/tas2.md)
                            \   * [TAS3](../main/subroutine/tas3.md)
                            \   * [TAS4](../main/subroutine/tas4.md)
                            \   * [TAS6](../main/subroutine/tas6.md)
                            \   * [TIDY](../main/subroutine/tidy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .X1
    
     SKIP 1                 \ Temporary storage, typically used for x-coordinates in
                            \ the line-drawing routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [BOMBOFF](../main/subroutine/bomboff.md)
                            \   * [CPIXK](../main/subroutine/cpixk.md)
                            \   * [DOT](../main/subroutine/dot.md)
                            \   * [DVLOIN](../main/subroutine/dvloin.md)
                            \   * [EDGES](../main/subroutine/edges.md)
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [LSPUT](../main/subroutine/lsput.md)
                            \   * [NLIN2](../main/subroutine/nlin2.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [PIXEL2](../main/subroutine/pixel2.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SHPPT](../main/subroutine/shppt.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TT15](../main/subroutine/tt15.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \   * [WPLS2](../main/subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Y1
    
     SKIP 1                 \ Temporary storage, typically used for y-coordinates in
                            \ line-drawing routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [BOMBOFF](../main/subroutine/bomboff.md)
                            \   * [CPIXK](../main/subroutine/cpixk.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DOT](../main/subroutine/dot.md)
                            \   * [DVLOIN](../main/subroutine/dvloin.md)
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [HLOIN2](../main/subroutine/hloin2.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [LSPUT](../main/subroutine/lsput.md)
                            \   * [MLU1](../main/subroutine/mlu1.md)
                            \   * [NLIN2](../main/subroutine/nlin2.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [PIXEL2](../main/subroutine/pixel2.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SHPPT](../main/subroutine/shppt.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TT15](../main/subroutine/tt15.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \   * [WPLS2](../main/subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .X2
    
     SKIP 1                 \ Temporary storage, typically used for x-coordinates in
                            \ the line-drawing routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [BOMBOFF](../main/subroutine/bomboff.md)
                            \   * [DVLOIN](../main/subroutine/dvloin.md)
                            \   * [EDGES](../main/subroutine/edges.md)
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [LSPUT](../main/subroutine/lsput.md)
                            \   * [NLIN2](../main/subroutine/nlin2.md)
                            \   * [SHPPT](../main/subroutine/shppt.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TT15](../main/subroutine/tt15.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \   * [WPLS2](../main/subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Y2
    
     SKIP 1                 \ Temporary storage, typically used for y-coordinates in
                            \ line-drawing routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [BOMBOFF](../main/subroutine/bomboff.md)
                            \   * [DVLOIN](../main/subroutine/dvloin.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [LSPUT](../main/subroutine/lsput.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SHPPT](../main/subroutine/shppt.md)
                            \   * [TT15](../main/subroutine/tt15.md)
                            \   * [TTX66](../main/subroutine/ttx66.md)
                            \   * [WPLS2](../main/subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SKIP 2                 \ The last two bytes of the XX15 block
    
    .XX12
    
     SKIP 6                 \ Temporary storage for a block of values, used in a
                            \ number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [BOMBOFF](../main/subroutine/bomboff.md)
                            \   * [LL129](../main/subroutine/ll129.md)
                            \   * [LL145 (Part 1 of 4)](../main/subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 2 of 4)](../main/subroutine/ll145_part_2_of_4.md)
                            \   * [LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)
                            \   * [LL51](../main/subroutine/ll51.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../main/subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \   * [LSPUT](../main/subroutine/lsput.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K
    
     SKIP 4                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BPRNT](../main/subroutine/bprnt.md)
                            \   * [CHKON](../main/subroutine/chkon.md)
                            \   * [CIRCLE](../main/subroutine/circle.md)
                            \   * [csh](../main/subroutine/csh.md)
                            \   * [DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md)
                            \   * [DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [DILX](../main/subroutine/dilx.md)
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [DVID3B2](../main/subroutine/dvid3b2.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [FMLTU2](../main/subroutine/fmltu2.md)
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [MAS1](../main/subroutine/mas1.md)
                            \   * [MU5](../main/subroutine/mu5.md)
                            \   * [MULT3](../main/subroutine/mult3.md)
                            \   * [MV40](../main/subroutine/mv40.md)
                            \   * [MVS5](../main/subroutine/mvs5.md)
                            \   * [MVT3](../main/subroutine/mvt3.md)
                            \   * [PL9 (Part 1 of 3)](../main/subroutine/pl9_part_1_of_3.md)
                            \   * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md)
                            \   * [PLANET](../main/subroutine/planet.md)
                            \   * [PLS1](../main/subroutine/pls1.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [PLS3](../main/subroutine/pls3.md)
                            \   * [PLS6](../main/subroutine/pls6.md)
                            \   * [PROJ](../main/subroutine/proj.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [SVE](../main/subroutine/sve.md)
                            \   * [TAS1](../main/subroutine/tas1.md)
                            \   * [TT11](../main/subroutine/tt11.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LAS
    
     SKIP 1                 \ Contains the laser power of the laser fitted to the
                            \ current space view (or 0 if there is no laser fitted
                            \ to the current view)
                            \
                            \ This gets set to bits 0-6 of the laser power byte from
                            \ the commander data block, which contains the laser's
                            \ power (bit 7 doesn't denote laser power, just whether
                            \ or not the laser pulses, so that is not stored here)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .MSTG
    
     SKIP 1                 \ The current missile lock target
                            \
                            \   * &FF = no target
                            \
                            \   * 1-12 = the slot number of the ship that our
                            \            missile is locked onto
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ABORT2](../main/subroutine/abort2.md)
                            \   * [FRMIS](../main/subroutine/frmis.md)
                            \   * [FRS1](../main/subroutine/frs1.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DL
    
     SKIP 1                 \ Vertical sync flag
                            \
                            \ DL gets set to 30 every time we reach vertical sync on
                            \ the video system, which happens 50 times a second
                            \ (50Hz). The WSCAN routine uses this to pause until the
                            \ vertical sync, by setting DL to 0 and then monitoring
                            \ its value until it changes to 30
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [IRQ1](../main/subroutine/irq1.md)
                            \   * [WSCAN](../main/subroutine/wscan.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSP
    
     SKIP 1                 \ The ball line heap pointer, which contains the number
                            \ of the first free byte after the end of the LSX2 and
                            \ LSY2 heaps
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [TT128](../main/subroutine/tt128.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \   * [WP1](../main/subroutine/wp1.md)
                            \   * [WPLS2](../main/subroutine/wpls2.md)
                            \   * [WPSHPS](../main/subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ15
    
     SKIP 6                 \ The three 16-bit seeds for the selected system, i.e.
                            \ the one in the crosshairs in the Short-range Chart
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [cpl](../main/subroutine/cpl.md)
                            \   * [Ghy](../main/subroutine/ghy.md)
                            \   * [HME2](../main/subroutine/hme2.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [PDESC](../main/subroutine/pdesc.md)
                            \   * [SOLAR](../main/subroutine/solar.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \   * [TT24](../main/subroutine/tt24.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \   * [TT54](../main/subroutine/tt54.md)
                            \   * [TT81](../main/subroutine/tt81.md)
                            \   * [ypl](../main/subroutine/ypl.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K5
    
     SKIP 0                 \ Temporary storage used to store segment coordinates
                            \ across successive calls to BLINE, the ball line
                            \ routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX18
    
     SKIP 4                 \ Temporary storage used to store coordinates in the
                            \ LL9 ship-drawing routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K6
    
     SKIP 5                 \ Temporary storage, typically used for storing
                            \ coordinates during vector calculations
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [CIRCLE2](../main/subroutine/circle2.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ19
    
     SKIP 6                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [cpl](../main/subroutine/cpl.md)
                            \   * [GVL](../main/subroutine/gvl.md)
                            \   * [SIGHT](../main/subroutine/sight.md)
                            \   * [TT103](../main/subroutine/tt103.md)
                            \   * [TT105](../main/subroutine/tt105.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT123](../main/subroutine/tt123.md)
                            \   * [TT128](../main/subroutine/tt128.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT15](../main/subroutine/tt15.md)
                            \   * [TT151](../main/subroutine/tt151.md)
                            \   * [TT152](../main/subroutine/tt152.md)
                            \   * [TT16](../main/subroutine/tt16.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \   * [var](../main/subroutine/var.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BET2
    
     SKIP 2                 \ Bit 7 of BET2 = sign of the pitch angle in BETA
                            \
                            \ Bit 7 of BET2+1 = opposite sign to BET2 and BETA
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DELTA
    
     SKIP 1                 \ Our current speed, in the range 1-40
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md)
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [DV41](../main/subroutine/dv41.md)
                            \   * [FRS1](../main/subroutine/frs1.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [Main flight loop (Part 10 of 16)](../main/subroutine/main_flight_loop_part_10_of_16.md)
                            \   * [MVEIT (Part 6 of 9)](../main/subroutine/mveit_part_6_of_9.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT110](../main/subroutine/tt110.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DELT4
    
     SKIP 2                 \ Our current speed * 64 as a 16-bit value
                            \
                            \ This is stored as DELT4(1 0), so the high byte in
                            \ DELT4+1 therefore contains our current speed / 4
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .U
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../main/subroutine/add.md)
                            \   * [ADDK](../main/subroutine/addk.md)
                            \   * [BPRNT](../main/subroutine/bprnt.md)
                            \   * [csh](../main/subroutine/csh.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [LL61](../main/subroutine/ll61.md)
                            \   * [LL62](../main/subroutine/ll62.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../main/subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [PLS3](../main/subroutine/pls3.md)
                            \   * [TAS1](../main/subroutine/tas1.md)
                            \   * [TT11](../main/subroutine/tt11.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Q
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ARCTAN](../main/subroutine/arctan.md)
                            \   * [DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)
                            \   * [DIL2](../main/subroutine/dil2.md)
                            \   * [DILX](../main/subroutine/dilx.md)
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DV41](../main/subroutine/dv41.md)
                            \   * [DVID3B2](../main/subroutine/dvid3b2.md)
                            \   * [DVID4](../main/subroutine/dvid4.md)
                            \   * [DVID4K](../main/subroutine/dvid4k.md)
                            \   * [DVIDT](../main/subroutine/dvidt.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [FMLTU](../main/subroutine/fmltu.md)
                            \   * [FMLTU2](../main/subroutine/fmltu2.md)
                            \   * [gnum](../main/subroutine/gnum.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HAS1](../main/subroutine/has1.md)
                            \   * [LL120](../main/subroutine/ll120.md)
                            \   * [LL123](../main/subroutine/ll123.md)
                            \   * [LL129](../main/subroutine/ll129.md)
                            \   * [LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md)
                            \   * [LL28](../main/subroutine/ll28.md)
                            \   * [LL38](../main/subroutine/ll38.md)
                            \   * [LL5](../main/subroutine/ll5.md)
                            \   * [LL51](../main/subroutine/ll51.md)
                            \   * [LL61](../main/subroutine/ll61.md)
                            \   * [LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [MLTU2](../main/subroutine/mltu2.md)
                            \   * [MULT1](../main/subroutine/mult1.md)
                            \   * [MULT3](../main/subroutine/mult3.md)
                            \   * [MULTU](../main/subroutine/multu.md)
                            \   * [MV40](../main/subroutine/mv40.md)
                            \   * [MVEIT (Part 3 of 9)](../main/subroutine/mveit_part_3_of_9.md)
                            \   * [MVS4](../main/subroutine/mvs4.md)
                            \   * [MVS5](../main/subroutine/mvs5.md)
                            \   * [NORM](../main/subroutine/norm.md)
                            \   * [OUTK](../main/subroutine/outk.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [PLS3](../main/subroutine/pls3.md)
                            \   * [PLS4](../main/subroutine/pls4.md)
                            \   * [SPS2](../main/subroutine/sps2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TAS3](../main/subroutine/tas3.md)
                            \   * [TAS4](../main/subroutine/tas4.md)
                            \   * [TIDY](../main/subroutine/tidy.md)
                            \   * [TIS1](../main/subroutine/tis1.md)
                            \   * [TIS2](../main/subroutine/tis2.md)
                            \   * [TIS3](../main/subroutine/tis3.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT219](../main/subroutine/tt219.md)
                            \   * [TT24](../main/subroutine/tt24.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .R
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../main/subroutine/add.md)
                            \   * [ADDK](../main/subroutine/addk.md)
                            \   * [ARCTAN](../main/subroutine/arctan.md)
                            \   * [CPIXK](../main/subroutine/cpixk.md)
                            \   * [DCS1](../main/subroutine/dcs1.md)
                            \   * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)
                            \   * [DILX](../main/subroutine/dilx.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DVID3B2](../main/subroutine/dvid3b2.md)
                            \   * [DVID4](../main/subroutine/dvid4.md)
                            \   * [gnum](../main/subroutine/gnum.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HAS1](../main/subroutine/has1.md)
                            \   * [HITCH](../main/subroutine/hitch.md)
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [LL118](../main/subroutine/ll118.md)
                            \   * [LL120](../main/subroutine/ll120.md)
                            \   * [LL123](../main/subroutine/ll123.md)
                            \   * [LL129](../main/subroutine/ll129.md)
                            \   * [LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)
                            \   * [LL28](../main/subroutine/ll28.md)
                            \   * [LL38](../main/subroutine/ll38.md)
                            \   * [LL5](../main/subroutine/ll5.md)
                            \   * [LL51](../main/subroutine/ll51.md)
                            \   * [LL61](../main/subroutine/ll61.md)
                            \   * [LL62](../main/subroutine/ll62.md)
                            \   * [LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [MAS3](../main/subroutine/mas3.md)
                            \   * [MLS2](../main/subroutine/mls2.md)
                            \   * [MULT12](../main/subroutine/mult12.md)
                            \   * [MULT3](../main/subroutine/mult3.md)
                            \   * [MUT1](../main/subroutine/mut1.md)
                            \   * [MVEIT (Part 3 of 9)](../main/subroutine/mveit_part_3_of_9.md)
                            \   * [MVEIT (Part 6 of 9)](../main/subroutine/mveit_part_6_of_9.md)
                            \   * [MVS4](../main/subroutine/mvs4.md)
                            \   * [MVS5](../main/subroutine/mvs5.md)
                            \   * [MVT1](../main/subroutine/mvt1.md)
                            \   * [NORM](../main/subroutine/norm.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SFS2](../main/subroutine/sfs2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TAS3](../main/subroutine/tas3.md)
                            \   * [TAS4](../main/subroutine/tas4.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT219](../main/subroutine/tt219.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .S
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../main/subroutine/add.md)
                            \   * [ADDK](../main/subroutine/addk.md)
                            \   * [BPRNT](../main/subroutine/bprnt.md)
                            \   * [DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DVID3B2](../main/subroutine/dvid3b2.md)
                            \   * [gnum](../main/subroutine/gnum.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HITCH](../main/subroutine/hitch.md)
                            \   * [LL118](../main/subroutine/ll118.md)
                            \   * [LL120](../main/subroutine/ll120.md)
                            \   * [LL123](../main/subroutine/ll123.md)
                            \   * [LL129](../main/subroutine/ll129.md)
                            \   * [LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)
                            \   * [LL38](../main/subroutine/ll38.md)
                            \   * [LL5](../main/subroutine/ll5.md)
                            \   * [LL51](../main/subroutine/ll51.md)
                            \   * [LL61](../main/subroutine/ll61.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)
                            \   * [MLS2](../main/subroutine/mls2.md)
                            \   * [MULT12](../main/subroutine/mult12.md)
                            \   * [MUT2](../main/subroutine/mut2.md)
                            \   * [MVS4](../main/subroutine/mvs4.md)
                            \   * [MVS5](../main/subroutine/mvs5.md)
                            \   * [MVT1](../main/subroutine/mvt1.md)
                            \   * [MVT3](../main/subroutine/mvt3.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [TAS3](../main/subroutine/tas3.md)
                            \   * [TAS4](../main/subroutine/tas4.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .T
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../main/subroutine/add.md)
                            \   * [ADDK](../main/subroutine/addk.md)
                            \   * [ARCTAN](../main/subroutine/arctan.md)
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [BPRNT](../main/subroutine/bprnt.md)
                            \   * [BUMP2](../main/subroutine/bump2.md)
                            \   * [CIRCLE2](../main/subroutine/circle2.md)
                            \   * [cpl](../main/subroutine/cpl.md)
                            \   * [DEEORS](../main/subroutine/deeors.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DVID3B2](../main/subroutine/dvid3b2.md)
                            \   * [DVIDT](../main/subroutine/dvidt.md)
                            \   * [EDGES](../main/subroutine/edges.md)
                            \   * [gnum](../main/subroutine/gnum.md)
                            \   * [HALL](../main/subroutine/hall.md)
                            \   * [HANGER](../main/subroutine/hanger.md)
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [LL120](../main/subroutine/ll120.md)
                            \   * [LL123](../main/subroutine/ll123.md)
                            \   * [LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md)
                            \   * [LL5](../main/subroutine/ll5.md)
                            \   * [LL51](../main/subroutine/ll51.md)
                            \   * [LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../main/subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \   * [MLS1](../main/subroutine/mls1.md)
                            \   * [MSBAR](../main/subroutine/msbar.md)
                            \   * [MU11](../main/subroutine/mu11.md)
                            \   * [MULT1](../main/subroutine/mult1.md)
                            \   * [MULT3](../main/subroutine/mult3.md)
                            \   * [MV40](../main/subroutine/mv40.md)
                            \   * [MVS5](../main/subroutine/mvs5.md)
                            \   * [MVT1](../main/subroutine/mvt1.md)
                            \   * [MVT3](../main/subroutine/mvt3.md)
                            \   * [NORM](../main/subroutine/norm.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [OOPS](../main/subroutine/oops.md)
                            \   * [PIXEL2](../main/subroutine/pixel2.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [REDU2](../main/subroutine/redu2.md)
                            \   * [SP2](../main/subroutine/sp2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)
                            \   * [TIS1](../main/subroutine/tis1.md)
                            \   * [TIS2](../main/subroutine/tis2.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XSAV
    
     SKIP 1                 \ Temporary storage for saving the value of the X
                            \ register, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HAS1](../main/subroutine/has1.md)
                            \   * [KS1](../main/subroutine/ks1.md)
                            \   * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [MVEIT (Part 1 of 9)](../main/subroutine/mveit_part_1_of_9.md)
                            \   * [MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \   * [WPSHPS](../main/subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .YSAV
    
     SKIP 1                 \ Temporary storage for saving the value of the Y
                            \ register, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HLOIN](../main/subroutine/hloin.md)
                            \   * [LOIN](../main/subroutine/loin.md)
                            \   * [TT217](../main/subroutine/tt217.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX17
    
     SKIP 1                 \ Temporary storage, used in BPRNT to store the number
                            \ of characters to print, and as the edge counter in the
                            \ main ship-drawing routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BPRNT](../main/subroutine/bprnt.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .W
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../main/subroutine/chpr.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ11
    
     SKIP 1                 \ The type of the current view:
                            \
                            \   0   = Space view
                            \         Death screen
                            \   1   = Data on System screen (red key f6)
                            \         Get commander name ("@", save/load commander)
                            \         In-system jump just arrived ("J")
                            \   2   = Buy Cargo screen (red key f1)
                            \   3   = Mis-jump just arrived (witchspace)
                            \   4   = Sell Cargo screen (red key f2)
                            \   8   = Status Mode screen (red key f8)
                            \         Inventory screen (red key f9)
                            \   13  = Rotating ship view (title or mission screen)
                            \   16  = Market Price screen (red key f7)
                            \   32  = Equip Ship screen (red key f3)
                            \   64  = Long-range Chart (red key f4)
                            \   128 = Short-range Chart (red key f5)
                            \   255 = Launch view
                            \
                            \ This value is typically set by calling routine TT66
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BOMBOFF](../main/subroutine/bomboff.md)
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [DFAULT](../main/subroutine/dfault.md)
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [LOOK1](../main/subroutine/look1.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)
                            \   * [Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md)
                            \   * [me2](../main/subroutine/me2.md)
                            \   * [MESS](../main/subroutine/mess.md)
                            \   * [NWSTARS](../main/subroutine/nwstars.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TT102](../main/subroutine/tt102.md)
                            \   * [TT103](../main/subroutine/tt103.md)
                            \   * [TT110](../main/subroutine/tt110.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT15](../main/subroutine/tt15.md)
                            \   * [TT17](../main/subroutine/tt17.md)
                            \   * [TT18](../main/subroutine/tt18.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT66](../main/subroutine/tt66.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ZZ
    
     SKIP 1                 \ Temporary storage, typically used for distance values
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [PDESC](../main/subroutine/pdesc.md)
                            \   * [PIXEL](../main/subroutine/pixel.md)
                            \   * [refund](../main/subroutine/refund.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX13
    
     SKIP 1                 \ Temporary storage, typically used in the line-drawing
                            \ routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [LL145 (Part 1 of 4)](../main/subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 2 of 4)](../main/subroutine/ll145_part_2_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .MCNT
    
     SKIP 1                 \ The main loop counter
                            \
                            \ This counter determines how often certain actions are
                            \ performed within the main loop
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BRIEF](../main/subroutine/brief.md)
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)
                            \   * [MVEIT (Part 1 of 9)](../main/subroutine/mveit_part_1_of_9.md)
                            \   * [MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md)
                            \   * [PZW](../main/subroutine/pzw.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .TYPE
    
     SKIP 1                 \ The current ship type
                            \
                            \ This is where we store the current ship type for when
                            \ we are iterating through the ships in the local bubble
                            \ as part of the main flight loop. See the table at XX21
                            \ for information about ship types
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../main/subroutine/angry.md)
                            \   * [BRIEF](../main/subroutine/brief.md)
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [HITCH](../main/subroutine/hitch.md)
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md)
                            \   * [MVEIT (Part 6 of 9)](../main/subroutine/mveit_part_6_of_9.md)
                            \   * [PL2](../main/subroutine/pl2.md)
                            \   * [PL9 (Part 1 of 3)](../main/subroutine/pl9_part_1_of_3.md)
                            \   * [PLANET](../main/subroutine/planet.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \   * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [WPSHPS](../main/subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ALPHA
    
     SKIP 1                 \ The current roll angle alpha, which is reduced from
                            \ JSTX to a sign-magnitude value between -31 and +31
                            \
                            \ This describes how fast we are rolling our ship, and
                            \ determines how fast the universe rolls around us
                            \
                            \ The sign bit is also stored in ALP2, while the
                            \ opposite sign is stored in ALP2+1
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MV40](../main/subroutine/mv40.md)
                            \   * [MVS4](../main/subroutine/mvs4.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ12
    
     SKIP 1                 \ Our "docked" status
                            \
                            \   * 0 = we are not docked
                            \
                            \   * &FF = we are docked
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BAY](../main/subroutine/bay.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [Main game loop (Part 6 of 6)](../main/subroutine/main_game_loop_part_6_of_6.md)
                            \   * [PDESC](../main/subroutine/pdesc.md)
                            \   * [RESET](../main/subroutine/reset.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \   * [TT102](../main/subroutine/tt102.md)
                            \   * [TT110](../main/subroutine/tt110.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .TGT
    
     SKIP 1                 \ Temporary storage, typically used as a target value
                            \ for counters when drawing explosion clouds and partial
                            \ circles
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)
                            \   * [PLS2](../main/subroutine/pls2.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .FLAG
    
     SKIP 1                 \ A flag that's used to define whether this is the first
                            \ call to the ball line routine in BLINE, so it knows
                            \ whether to wait for the second call before storing
                            \ segment data in the ball line heap
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [CIRCLE2](../main/subroutine/circle2.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .CNT
    
     SKIP 1                 \ Temporary storage, typically used for storing the
                            \ number of iterations required when looping
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [CIRCLE2](../main/subroutine/circle2.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [SPIN](../main/subroutine/spin.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
                            \   * [TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .CNT2
    
     SKIP 1                 \ Temporary storage, used in the planet-drawing routine
                            \ to store the segment number where the arc of a partial
                            \ circle should start
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [HALL](../main/subroutine/hall.md)
                            \   * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [PLS4](../main/subroutine/pls4.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .STP
    
     SKIP 1                 \ The step size for drawing circles
                            \
                            \ Circles in Elite are split up into 64 points, and the
                            \ step size determines how many points to skip with each
                            \ straight-line segment, so the smaller the step size,
                            \ the smoother the circle. The values used are:
                            \
                            \   * 2 for big planets and the circles on the charts
                            \   * 4 for medium planets and the launch tunnel
                            \   * 8 for small planets and the hyperspace tunnel
                            \
                            \ As the step size increases we move from smoother
                            \ circles at the top to more polygonal at the bottom.
                            \ See the CIRCLE2 routine for more details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [CIRCLE](../main/subroutine/circle.md)
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [TT128](../main/subroutine/tt128.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX4
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [GVL](../main/subroutine/gvl.md)
                            \   * [HFS2](../main/subroutine/hfs2.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [KS2](../main/subroutine/ks2.md)
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 4 of 12)](../main/subroutine/ll9_part_4_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX20
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HME2](../main/subroutine/hme2.md)
                            \   * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSNUM
    
     SKIP 1                 \ The pointer to the current position in the ship line
                            \ heap as we work our way through the new ship's edges
                            \ (and the corresponding old ship's edges) when drawing
                            \ the ship in the main ship-drawing routine at LL9
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)
                            \   * [LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md)
                            \   * [LSPUT](../main/subroutine/lsput.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSNUM2
    
     SKIP 1                 \ The size of the existing ship line heap for the ship
                            \ we are drawing in LL9, i.e. the number of lines in the
                            \ old ship that is currently shown on-screen and which
                            \ we need to erase
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md)
                            \   * [LSPUT](../main/subroutine/lsput.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .RAT
    
     SKIP 1                 \ Used to store different signs depending on the current
                            \ space view, for use in calculating stardust movement
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [PLUT](../main/subroutine/plut.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .RAT2
    
     SKIP 1                 \ Temporary storage, used to store the pitch and roll
                            \ signs when moving objects and stardust
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [HAS1](../main/subroutine/has1.md)
                            \   * [MVEIT (Part 8 of 9)](../main/subroutine/mveit_part_8_of_9.md)
                            \   * [MVS5](../main/subroutine/mvs5.md)
                            \   * [PLUT](../main/subroutine/plut.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K2
    
     SKIP 4                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [MV40](../main/subroutine/mv40.md)
                            \   * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)
                            \   * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md)
                            \   * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)
                            \   * [PLS22](../main/subroutine/pls22.md)
                            \   * [PLS5](../main/subroutine/pls5.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .widget
    
     SKIP 1                 \ Temporary storage, used to store the original argument
                            \ in A in the logarithmic FMLTU and LL28 routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DVID4](../main/subroutine/dvid4.md)
                            \   * [FMLTU](../main/subroutine/fmltu.md)
                            \   * [LL28](../main/subroutine/ll28.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .dontclip
    
     SKIP 1                 \ This is set to 0 in the RES2 routine, but the value is
                            \ never actually read (this is left over from the
                            \ Commodore 64 version of Elite)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [RES2](../main/subroutine/res2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Yx2M1
    
     SKIP 1                 \ This is used to store the number of pixel rows in the
                            \ space view minus 1, which is also the y-coordinate of
                            \ the bottom pixel row of the space view (it is set to
                            \ 191 in the RES2 routine)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHKON](../main/subroutine/chkon.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .messXC
    
     SKIP 1                 \ Temporary storage, used to store the text column
                            \ of the in-flight message in MESS, so it can be erased
                            \ from the screen at the correct time
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [MESS](../main/subroutine/mess.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .newzp
    
     SKIP 1                 \ This is used by the STARS2 routine for storing the
                            \ stardust particle's delta_x value
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX1
    
     SKIP 0                 \ This is an alias for INWK that is used in the main
                            \ ship-drawing routine at LL9
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../main/subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)
                            \   * [SHPPT](../main/subroutine/shppt.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .INWK
    
     SKIP 33                \ The zero-page internal workspace for the current ship
                            \ data block
                            \
                            \ As operations on zero page locations are faster and
                            \ have smaller opcodes than operations on the rest of
                            \ the addressable memory, Elite tends to store oft-used
                            \ data here. A lot of the routines in Elite need to
                            \ access and manipulate ship data, so to make this an
                            \ efficient exercise, the ship data is first copied from
                            \ the ship data blocks at K% into INWK (or, when new
                            \ ships are spawned, from the blueprints at XX21)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BRIEF](../main/subroutine/brief.md)
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [DELT](../main/subroutine/delt.md)
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [DVID3B2](../main/subroutine/dvid3b2.md)
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [FAROF2](../main/subroutine/farof2.md)
                            \   * [FRS1](../main/subroutine/frs1.md)
                            \   * [GTDIR](../main/subroutine/gtdir.md)
                            \   * [GTHG](../main/subroutine/gthg.md)
                            \   * [GTNMEW](../main/subroutine/gtnmew.md)
                            \   * [HAS1](../main/subroutine/has1.md)
                            \   * [HITCH](../main/subroutine/hitch.md)
                            \   * [HME2](../main/subroutine/hme2.md)
                            \   * [KS4](../main/subroutine/ks4.md)
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [Main flight loop (Part 6 of 16)](../main/subroutine/main_flight_loop_part_6_of_16.md)
                            \   * [Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md)
                            \   * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [Main flight loop (Part 10 of 16)](../main/subroutine/main_flight_loop_part_10_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md)
                            \   * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \   * [MAS1](../main/subroutine/mas1.md)
                            \   * [MAS4](../main/subroutine/mas4.md)
                            \   * [MT26](../main/subroutine/mt26.md)
                            \   * [MV40](../main/subroutine/mv40.md)
                            \   * [MVEIT (Part 1 of 9)](../main/subroutine/mveit_part_1_of_9.md)
                            \   * [MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md)
                            \   * [MVEIT (Part 3 of 9)](../main/subroutine/mveit_part_3_of_9.md)
                            \   * [MVEIT (Part 4 of 9)](../main/subroutine/mveit_part_4_of_9.md)
                            \   * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)
                            \   * [MVEIT (Part 8 of 9)](../main/subroutine/mveit_part_8_of_9.md)
                            \   * [MVEIT (Part 9 of 9)](../main/subroutine/mveit_part_9_of_9.md)
                            \   * [MVS4](../main/subroutine/mvs4.md)
                            \   * [MVS5](../main/subroutine/mvs5.md)
                            \   * [MVT1](../main/subroutine/mvt1.md)
                            \   * [MVT3](../main/subroutine/mvt3.md)
                            \   * [MVT6](../main/subroutine/mvt6.md)
                            \   * [NwS1](../main/subroutine/nws1.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [NWSPS](../main/subroutine/nwsps.md)
                            \   * [PAS1](../main/subroutine/pas1.md)
                            \   * [PAUSE](../main/subroutine/pause.md)
                            \   * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md)
                            \   * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)
                            \   * [PLANET](../main/subroutine/planet.md)
                            \   * [PLS1](../main/subroutine/pls1.md)
                            \   * [PLS4](../main/subroutine/pls4.md)
                            \   * [PLUT](../main/subroutine/plut.md)
                            \   * [PROJ](../main/subroutine/proj.md)
                            \   * [rfile](../main/subroutine/rfile.md)
                            \   * [RLINE](../main/variable/rline.md)
                            \   * [SCAN](../main/subroutine/scan.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \   * [SOLAR](../main/subroutine/solar.md)
                            \   * [SOS1](../main/subroutine/sos1.md)
                            \   * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \   * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)
                            \   * [TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)
                            \   * [TAS3](../main/subroutine/tas3.md)
                            \   * [TIDY](../main/subroutine/tidy.md)
                            \   * [TIS3](../main/subroutine/tis3.md)
                            \   * [TITLE](../main/subroutine/title.md)
                            \   * [TR1](../main/subroutine/tr1.md)
                            \   * [TRNME](../main/subroutine/trnme.md)
                            \   * [TT110](../main/subroutine/tt110.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \   * [WPSHPS](../main/subroutine/wpshps.md)
                            \   * [Ze](../main/subroutine/ze.md)
                            \   * [ZINF](../main/subroutine/zinf.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX19
    
     SKIP NI% - 34          \ XX19(1 0) shares its location with INWK(34 33), which
                            \ contains the address of the ship line heap
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md)
                            \   * [LSPUT](../main/subroutine/lsput.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .NEWB
    
     SKIP 1                 \ The ship's "new byte flags" (or NEWB flags)
                            \
                            \ Contains details about the ship's type and associated
                            \ behaviour, such as whether they are a trader, a bounty
                            \ hunter, a pirate, currently hostile, in the process of
                            \ docking, inside the hold having been scooped, and so
                            \ on. The default values for each ship type are taken
                            \ from the table at E%
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)
                            \   * [Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [NWSPS](../main/subroutine/nwsps.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
                            \   * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTX
    
     SKIP 1                 \ Our current roll rate
                            \
                            \ This value is shown in the dashboard's RL indicator,
                            \ and determines the rate at which we are rolling
                            \
                            \ The value ranges from 1 to 255 with 128 as the centre
                            \ point, so 1 means roll is decreasing at the maximum
                            \ rate, 128 means roll is not changing, and 255 means
                            \ roll is increasing at the maximum rate
                            \
                            \ This value is updated by "<" and ">" key presses, or
                            \ if joysticks are enabled, from the joystick. If
                            \ keyboard damping is enabled (which it is by default),
                            \ the value is slowly moved towards the centre value of
                            \ 128 (no roll) if there are no key presses or joystick
                            \ movement
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \   * [TT17](../main/subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTY
    
     SKIP 1                 \ Our current pitch rate
                            \
                            \ This value is shown in the dashboard's DC indicator,
                            \ and determines the rate at which we are pitching
                            \
                            \ The value ranges from 1 to 255 with 128 as the centre
                            \ point, so 1 means pitch is decreasing at the maximum
                            \ rate, 128 means pitch is not changing, and 255 means
                            \ pitch is increasing at the maximum rate
                            \
                            \ This value is updated by "S" and "X" key presses, or
                            \ if joysticks are enabled, from the joystick. If
                            \ keyboard damping is enabled (which it is by default),
                            \ the value is slowly moved towards the centre value of
                            \ 128 (no pitch) if there are no key presses or joystick
                            \ movement
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [TT17](../main/subroutine/tt17.md)
                            \   * [ZEKTRAN](../main/subroutine/zektran.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KL
    
     SKIP 1                 \ The following bytes implement a key logger that
                            \ enables Elite to scan for concurrent key presses of
                            \ the primary flight keys, plus a secondary flight key
                            \
                            \ If a key is being pressed that is not in the keyboard
                            \ table at KYTB, it can be stored here (as seen in
                            \ routine DK4, for example)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../main/subroutine/dk4.md)
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [FILLKL](../main/subroutine/fillkl.md)
                            \   * [RDKEY](../main/subroutine/rdkey.md)
                            \   * [TT102](../main/subroutine/tt102.md)
                            \   * [TT17](../main/subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY17
    
     SKIP 1                 \ "E" is being pressed (activate E.C.M.)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FILLKL](../main/subroutine/fillkl.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY14
    
     SKIP 1                 \ "T" is being pressed (target missile)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY15
    
     SKIP 1                 \ "U" is being pressed (unarm missile)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY20
    
     SKIP 1                 \ "P" is being pressed (deactivate docking computer)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY7
    
     SKIP 1                 \ "A" is being pressed (fire lasers)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ This is also set when the joystick fire button has
                            \ been pressed
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \   * [TT17](../main/subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY5
    
     SKIP 1                 \ "X" is being pressed (pull up)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY18
    
     SKIP 1                 \ "J" is being pressed (in-system jump)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY6
    
     SKIP 1                 \ "S" is being pressed (pitch down)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [FILLKL](../main/subroutine/fillkl.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY19
    
     SKIP 1                 \ "C" is being pressed (activate docking computer)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FILLKL](../main/subroutine/fillkl.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY12
    
     SKIP 1                 \ TAB is being pressed (energy bomb)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY2
    
     SKIP 1                 \ Space is being pressed (speed up)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY16
    
     SKIP 1                 \ "M" is being pressed (fire missile)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY3
    
     SKIP 1                 \ "<" is being pressed (roll left)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY4
    
     SKIP 1                 \ ">" is being pressed (roll right)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY1
    
     SKIP 1                 \ "?" is being pressed (slow down)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY13
    
     SKIP 1                 \ ESCAPE is being pressed (launch escape pod)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSX
    
     SKIP 1                 \ LSX contains the status of the sun line heap at LSO
                            \
                            \   * &FF indicates the sun line heap is empty
                            \
                            \   * Otherwise the LSO heap contains the line data for
                            \     the sun
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FLFLLS](../main/subroutine/flflls.md)
                            \   * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)
                            \   * [WPLS](../main/subroutine/wpls.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .FSH
    
     SKIP 1                 \ Forward shield status
                            \
                            \   * 0 = empty
                            \
                            \   * &FF = full
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [OOPS](../main/subroutine/oops.md)
                            \   * [RESET](../main/subroutine/reset.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ASH
    
     SKIP 1                 \ Aft shield status
                            \
                            \   * 0 = empty
                            \
                            \   * &FF = full
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [OOPS](../main/subroutine/oops.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ENERGY
    
     SKIP 1                 \ Energy bank status
                            \
                            \   * 0 = empty
                            \
                            \   * &FF = full
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DENGY](../main/subroutine/dengy.md)
                            \   * [DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [OOPS](../main/subroutine/oops.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ3
    
     SKIP 1                 \ The selected system's economy (0-7)
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
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
                            \   * [TT24](../main/subroutine/tt24.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ4
    
     SKIP 1                 \ The selected system's government (0-7)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
                            \   * [TT24](../main/subroutine/tt24.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ5
    
     SKIP 1                 \ The selected system's tech level (0-14)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
                            \   * [TT24](../main/subroutine/tt24.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ6
    
     SKIP 2                 \ The selected system's population in billions * 10
                            \ (1-71), so the maximum population is 7.1 billion
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT24](../main/subroutine/tt24.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ7
    
     SKIP 2                 \ The selected system's productivity in M CR (96-62480)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT24](../main/subroutine/tt24.md)
                            \   * [TT25](../main/subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ8
    
     SKIP 2                 \ The distance from the current system to the selected
                            \ system in light years * 10, stored as a 16-bit number
                            \
                            \ The distance will be 0 if the selected system is the
                            \ current system
                            \
                            \ The galaxy chart is 102.4 light years wide and 51.2
                            \ light years tall (see the intra-system distance
                            \ calculations in routine TT111 for details), which
                            \ equates to 1024 x 512 in terms of QQ8
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Ghy](../main/subroutine/ghy.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [PDESC](../main/subroutine/pdesc.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT146](../main/subroutine/tt146.md)
                            \   * [TT18](../main/subroutine/tt18.md)
                            \   * [TT23](../main/subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ9
    
     SKIP 1                 \ The galactic x-coordinate of the crosshairs in the
                            \ galaxy chart (and, most of the time, the selected
                            \ system's galactic x-coordinate)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Ghy](../main/subroutine/ghy.md)
                            \   * [HME2](../main/subroutine/hme2.md)
                            \   * [jmp](../main/subroutine/jmp.md)
                            \   * [ping](../main/subroutine/ping.md)
                            \   * [TT103](../main/subroutine/tt103.md)
                            \   * [TT105](../main/subroutine/tt105.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT16](../main/subroutine/tt16.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ10
    
     SKIP 1                 \ The galactic y-coordinate of the crosshairs in the
                            \ galaxy chart (and, most of the time, the selected
                            \ system's galactic y-coordinate)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Ghy](../main/subroutine/ghy.md)
                            \   * [HME2](../main/subroutine/hme2.md)
                            \   * [jmp](../main/subroutine/jmp.md)
                            \   * [TT103](../main/subroutine/tt103.md)
                            \   * [TT105](../main/subroutine/tt105.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT16](../main/subroutine/tt16.md)
                            \   * [TT22](../main/subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .NOSTM
    
     SKIP 1                 \ The number of stardust particles shown on screen,
                            \ which is 20 (#NOST) for normal space, and 3 for
                            \ witchspace
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [MJP](../main/subroutine/mjp.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     PRINT "ZP workspace from ", ~ZP, "to ", ~P%-1, "inclusive"
    
    
    
    
           Name: XX3                                                     [Show more]
           Type: Workspace
        Address: &0100 to the top of the descending stack
       Category: Workspaces
        Summary: Temporary storage space for complex calculations
    
    
        Context: See this workspace [on its own page](../main/workspace/xx3.md)
     References: This workspace is used as follows:
                 * [DOEXP](../main/subroutine/doexp.md) uses XX3
                 * [LL62](../main/subroutine/ll62.md) uses XX3
                 * [LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md) uses XX3
                 * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md) uses XX3
                 * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md) uses XX3
                 * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md) uses XX3
                 * [SFS1](../main/subroutine/sfs1.md) uses XX3
    
    
    
    
    
    * * *
    
    
     Used as heap space for storing temporary data during calculations. Shared with
     the descending 6502 stack, which works down from &01FF.
    
    
    
    
     ORG &0100              \ Set the assembly address to &0100
    
    .XX3
    
     SKIP 256               \ Temporary storage, typically used for storing tables
                            \ of values such as screen coordinates or ship data
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [LL62](../main/subroutine/ll62.md)
                            \   * [LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)
                            \   * [SFS1](../main/subroutine/sfs1.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    
    
    
           Name: K%                                                      [Show more]
           Type: Workspace
        Address: &0400 to &05BB
       Category: Workspaces
        Summary: Ship data blocks and ship line heaps
      Deep dive: [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [The local bubble of universe](https://elite.bbcelite.com/deep_dives/the_local_bubble_of_universe.html)
    
    
        Context: See this workspace [on its own page](../main/workspace/k_per_cent.md)
     Variations: See [code variations](../related/compare/main/workspace/k_per_cent.md) for this workspace in the different versions
     References: This workspace is used as follows:
                 * [ANGRY](../main/subroutine/angry.md) uses K%
                 * [DCS1](../main/subroutine/dcs1.md) uses K%
                 * [DOEXP](../main/subroutine/doexp.md) uses K%
                 * [Main flight loop (Part 1 of 16)](../main/subroutine/main_flight_loop_part_1_of_16.md) uses K%
                 * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md) uses K%
                 * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md) uses K%
                 * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md) uses K%
                 * [MAS2](../main/subroutine/mas2.md) uses K%
                 * [MAS3](../main/subroutine/mas3.md) uses K%
                 * [SPS3](../main/subroutine/sps3.md) uses K%
                 * [SPS4](../main/subroutine/sps4.md) uses K%
                 * [TAS4](../main/subroutine/tas4.md) uses K%
                 * [UNIV](../main/variable/univ.md) uses K%
                 * [VCSU1](../main/subroutine/vcsu1.md) uses K%
                 * [WARP](../main/subroutine/warp.md) uses K%
    
    
    
    
    
    * * *
    
    
     Contains ship data for all the ships, planets, suns and space stations in our
     local bubble of universe.
    
     The blocks are pointed to by the lookup table at location UNIV. The first 444
     bytes of the K% workspace hold ship data on up to 12 ships, with 37 (NI%)
     bytes per ship.
    
    
    
    
     ORG &0400              \ Set the assembly address to &0400
    
    .K%
    
     SKIP NOSH * NI%        \ Ship data blocks and ship line heap
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../main/subroutine/angry.md)
                            \   * [DCS1](../main/subroutine/dcs1.md)
                            \   * [DOEXP](../main/subroutine/doexp.md)
                            \   * [Main flight loop (Part 1 of 16)](../main/subroutine/main_flight_loop_part_1_of_16.md)
                            \   * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \   * [MAS2](../main/subroutine/mas2.md)
                            \   * [MAS3](../main/subroutine/mas3.md)
                            \   * [SPS3](../main/subroutine/sps3.md)
                            \   * [SPS4](../main/subroutine/sps4.md)
                            \   * [TAS4](../main/subroutine/tas4.md)
                            \   * [UNIV](../main/variable/univ.md)
                            \   * [VCSU1](../main/subroutine/vcsu1.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     PRINT "K% workspace from ", ~K%, "to ", ~P%-1, "inclusive"
    
    
    
    
           Name: WP                                                      [Show more]
           Type: Workspace
        Address: &0E41 to &12A9
       Category: Workspaces
        Summary: Ship slots, variables
    
    
        Context: See this workspace [on its own page](../main/workspace/wp.md)
     Variations: See [code variations](../related/compare/main/workspace/wp.md) for this workspace in the different versions
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
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [DEEOR](../main/subroutine/deeor.md)
                            \   * [DEEORS](../main/subroutine/deeors.md)
                            \   * [FRMIS](../main/subroutine/frmis.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [KS2](../main/subroutine/ks2.md)
                            \   * [KS4](../main/subroutine/ks4.md)
                            \   * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [NWSPS](../main/subroutine/nwsps.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \   * [WPSHPS](../main/subroutine/wpshps.md)
                            \   * [ZERO](../main/subroutine/zero.md)
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
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \   * [MJP](../main/subroutine/mjp.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
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
                            \   * [COMPAS](../main/subroutine/compas.md)
                            \   * [DOCKIT](../main/subroutine/dockit.md)
                            \   * [KS4](../main/subroutine/ks4.md)
                            \   * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)
                            \   * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
                            \   * [WARP](../main/subroutine/warp.md)
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
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \   * [WARP](../main/subroutine/warp.md)
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
                            \   * [cntr](../main/subroutine/cntr.md)
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
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
                            \   * [ECMOF](../main/subroutine/ecmof.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)
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
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)
                            \   * [MJP](../main/subroutine/mjp.md)
                            \   * [TT151](../main/subroutine/tt151.md)
                            \   * [WARP](../main/subroutine/warp.md)
                            \   * [ypl](../main/subroutine/ypl.md)
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
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
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
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
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
                            \   * [ABORT2](../main/subroutine/abort2.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)
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
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [LOOK1](../main/subroutine/look1.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [PLUT](../main/subroutine/plut.md)
                            \   * [SIGHT](../main/subroutine/sight.md)
                            \   * [STARS](../main/subroutine/stars.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \   * [WARP](../main/subroutine/warp.md)
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
                            \   * [DEATH](../main/subroutine/death.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)
                            \   * [Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md)
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
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [LASLI](../main/subroutine/lasli.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md)
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
                            \   * [IRQ1](../main/subroutine/irq1.md)
                            \   * [LL164](../main/subroutine/ll164.md)
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
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \   * [WARP](../main/subroutine/warp.md)
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
                            \   * [CLYNS](../main/subroutine/clyns.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)
                            \   * [me1](../main/subroutine/me1.md)
                            \   * [me2](../main/subroutine/me2.md)
                            \   * [MESS](../main/subroutine/mess.md)
                            \   * [OUCH](../main/subroutine/ouch.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
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
                            \   * [CLYNS](../main/subroutine/clyns.md)
                            \   * [mes9](../main/subroutine/mes9.md)
                            \   * [MESS](../main/subroutine/mess.md)
                            \   * [OUCH](../main/subroutine/ouch.md)
                            \   * [TTX66K](../main/subroutine/ttx66k.md)
                            \   * [ZERO](../main/subroutine/zero.md)
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
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [CIRCLE](../main/subroutine/circle.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [WP1](../main/subroutine/wp1.md)
                            \   * [WPLS2](../main/subroutine/wpls2.md)
                            \   * [WPSHPS](../main/subroutine/wpshps.md)
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
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [RES2](../main/subroutine/res2.md)
                            \   * [WPLS2](../main/subroutine/wpls2.md)
                            \   * [WPSHPS](../main/subroutine/wpshps.md)
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
                            \   * [EDGES](../main/subroutine/edges.md)
                            \   * [FLFLLS](../main/subroutine/flflls.md)
                            \   * [HLOIN2](../main/subroutine/hloin2.md)
                            \   * [NWSPS](../main/subroutine/nwsps.md)
                            \   * [SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)
                            \   * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)
                            \   * [SUN (Part 4 of 4)](../main/subroutine/sun_part_4_of_4.md)
                            \   * [WPLS](../main/subroutine/wpls.md)
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
                            \   * [HME2](../main/subroutine/hme2.md)
                            \   * [MT17](../main/subroutine/mt17.md)
                            \   * [TT26](../main/subroutine/tt26.md)
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
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
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
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
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
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [MLU1](../main/subroutine/mlu1.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
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
                            \   * [PIX1](../main/subroutine/pix1.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
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
                            \   * [DV42](../main/subroutine/dv42.md)
                            \   * [FLIP](../main/subroutine/flip.md)
                            \   * [nWq](../main/subroutine/nwq.md)
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS2](../main/subroutine/stars2.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
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
                            \   * [STARS1](../main/subroutine/stars1.md)
                            \   * [STARS6](../main/subroutine/stars6.md)
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
                            \   * [LASLI](../main/subroutine/lasli.md)
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
                            \   * [LASLI](../main/subroutine/lasli.md)
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
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
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
                            \   * [BLINE](../main/subroutine/bline.md)
                            \   * [LL145 (Part 1 of 4)](../main/subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)
                            \   * [LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)
                            \   * [WPLS2](../main/subroutine/wpls2.md)
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
                            \   * [TITLE](../main/subroutine/title.md)
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
                            \   * [NMICLAIM](../main/subroutine/nmiclaim.md)
                            \   * [NMIRELEASE](../main/subroutine/nmirelease.md)
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
                            \   * [cmn](../main/subroutine/cmn.md)
                            \   * [DFAULT](../main/subroutine/dfault.md)
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
                            \   * [BRIEF](../main/subroutine/brief.md)
                            \   * [BRIEF2](../main/subroutine/brief2.md)
                            \   * [BRIEF3](../main/subroutine/brief3.md)
                            \   * [DEBRIEF](../main/subroutine/debrief.md)
                            \   * [DEBRIEF2](../main/subroutine/debrief2.md)
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
                            \   * [PDESC](../main/subroutine/pdesc.md)
                            \   * [SVE](../main/subroutine/sve.md)
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
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [jmp](../main/subroutine/jmp.md)
                            \   * [ping](../main/subroutine/ping.md)
                            \   * [THERE](../main/subroutine/there.md)
                            \   * [TT105](../main/subroutine/tt105.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT23](../main/subroutine/tt23.md)
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
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [jmp](../main/subroutine/jmp.md)
                            \   * [MJP](../main/subroutine/mjp.md)
                            \   * [THERE](../main/subroutine/there.md)
                            \   * [TT105](../main/subroutine/tt105.md)
                            \   * [TT111](../main/subroutine/tt111.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT23](../main/subroutine/tt23.md)
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
                            \   * [Ghy](../main/subroutine/ghy.md)
                            \   * [TT81](../main/subroutine/tt81.md)
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
                            \   * [csh](../main/subroutine/csh.md)
                            \   * [LCASH](../main/subroutine/lcash.md)
                            \   * [MCASH](../main/subroutine/mcash.md)
                            \   * [SVE](../main/subroutine/sve.md)
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
                            \   * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [fwl](../main/subroutine/fwl.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [TT14](../main/subroutine/tt14.md)
                            \   * [TT18](../main/subroutine/tt18.md)
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
                            \   * [DFAULT](../main/subroutine/dfault.md)
                            \   * [MJP](../main/subroutine/mjp.md)
                            \   * [SVE](../main/subroutine/sve.md)
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
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [Ghy](../main/subroutine/ghy.md)
                            \   * [MT28](../main/subroutine/mt28.md)
                            \   * [PDESC](../main/subroutine/pdesc.md)
                            \   * [tal](../main/subroutine/tal.md)
                            \   * [THERE](../main/subroutine/there.md)
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
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [refund](../main/subroutine/refund.md)
                            \   * [SIGHT](../main/subroutine/sight.md)
                            \   * [STATUS](../main/subroutine/status.md)
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
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [tnpr](../main/subroutine/tnpr.md)
                            \   * [TT213](../main/subroutine/tt213.md)
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
                            \   * [BAD](../main/subroutine/bad.md)
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [OUCH](../main/subroutine/ouch.md)
                            \   * [SOLAR](../main/subroutine/solar.md)
                            \   * [tnpr](../main/subroutine/tnpr.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT219](../main/subroutine/tt219.md)
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
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [STATUS](../main/subroutine/status.md)
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
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [STATUS](../main/subroutine/status.md)
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
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [LOOK1](../main/subroutine/look1.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [STATUS](../main/subroutine/status.md)
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
                            \   * [DEBRIEF2](../main/subroutine/debrief2.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)
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
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
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
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [Ghy](../main/subroutine/ghy.md)
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
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [IRQ1](../main/subroutine/irq1.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [STATUS](../main/subroutine/status.md)
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
                            \   * [SOLAR](../main/subroutine/solar.md)
                            \   * [tnpr](../main/subroutine/tnpr.md)
                            \   * [TT210](../main/subroutine/tt210.md)
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
                            \   * [EXNO2](../main/subroutine/exno2.md)
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
                            \   * [ABORT2](../main/subroutine/abort2.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [FRMIS](../main/subroutine/frmis.md)
                            \   * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [msblob](../main/subroutine/msblob.md)
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
                            \   * [ESCAPE](../main/subroutine/escape.md)
                            \   * [Ghy](../main/subroutine/ghy.md)
                            \   * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md)
                            \   * [SOLAR](../main/subroutine/solar.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \   * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)
                            \   * [TT110](../main/subroutine/tt110.md)
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
                            \   * [GVL](../main/subroutine/gvl.md)
                            \   * [TT151](../main/subroutine/tt151.md)
                            \   * [TT219](../main/subroutine/tt219.md)
                            \   * [var](../main/subroutine/var.md)
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
                            \   * [GVL](../main/subroutine/gvl.md)
                            \   * [TT151](../main/subroutine/tt151.md)
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
                            \   * [DEBRIEF2](../main/subroutine/debrief2.md)
                            \   * [DOENTRY](../main/subroutine/doentry.md)
                            \   * [EXNO2](../main/subroutine/exno2.md)
                            \   * [KILLSHP](../main/subroutine/killshp.md)
                            \   * [STATUS](../main/subroutine/status.md)
                            \   * [SVE](../main/subroutine/sve.md)
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
                            \   * [SVE](../main/subroutine/sve.md)
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
                            \   * [me1](../main/subroutine/me1.md)
                            \   * [me2](../main/subroutine/me2.md)
                            \   * [MESS](../main/subroutine/mess.md)
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
                            \   * [DOT](../main/subroutine/dot.md)
                            \   * [SP2](../main/subroutine/sp2.md)
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
                            \   * [DOT](../main/subroutine/dot.md)
                            \   * [SP2](../main/subroutine/sp2.md)
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
                            \   * [TT151](../main/subroutine/tt151.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT219](../main/subroutine/tt219.md)
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
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [gnum](../main/subroutine/gnum.md)
                            \   * [TT151](../main/subroutine/tt151.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT219](../main/subroutine/tt219.md)
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
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
                            \   * [var](../main/subroutine/var.md)
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
                            \   * [Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [NWDAV4](../main/subroutine/nwdav4.md)
                            \   * [tnpr](../main/subroutine/tnpr.md)
                            \   * [tnpr1](../main/subroutine/tnpr1.md)
                            \   * [TT167](../main/subroutine/tt167.md)
                            \   * [TT210](../main/subroutine/tt210.md)
                            \   * [TT219](../main/subroutine/tt219.md)
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
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
                            \   * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)
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
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [EQSHP](../main/subroutine/eqshp.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
                            \   * [NWSPS](../main/subroutine/nwsps.md)
                            \   * [qv](../main/subroutine/qv.md)
                            \   * [SOS1](../main/subroutine/sos1.md)
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
                            \   * [KS3](../main/subroutine/ks3.md)
                            \   * [NWSHP](../main/subroutine/nwshp.md)
                            \   * [RES2](../main/subroutine/res2.md)
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
                            \   * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
                            \   * [ypl](../main/subroutine/ypl.md)
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
                            \   * [Ghy](../main/subroutine/ghy.md)
                            \   * [hyp](../main/subroutine/hyp.md)
                            \   * [hyp1](../main/subroutine/hyp1.md)
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
                            \   * [DOEXP](../main/subroutine/doexp.md)
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
                            \   * [DOKEY](../main/subroutine/dokey.md)
                            \   * [IRQ1](../main/subroutine/irq1.md)
                            \   * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [RDJOY](../main/subroutine/rdjoy.md)
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

Configuration variable [NI%](workspaces.md#ni-per-cent)

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Configuration variable [NOSH](workspaces.md#nosh)

The maximum number of ships in our local bubble of universe

[X]

Configuration variable [NOST](workspaces.md#nost)

The number of stardust particles in normal space (this goes down to 3 in witchspace)

[X]

Configuration variable [NTY](workspaces.md#nty)

The number of different ship types

[X]

Configuration variable [SST](workspaces.md#sst)

Ship type for a Coriolis space station

[Loader source](loader.md "Previous routine")[Elite A source](elite_a.md "Next routine")
