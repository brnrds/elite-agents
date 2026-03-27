---
title: "The FACE (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/face.html"
---

[EDGE (Game data)](edge.md "Previous routine")[SHIP_MISSILE (Game data)](../variable/ship_missile.md "Next routine")
    
    
           Name: FACE                                                    [Show more]
           Type: Macro
       Category: Drawing ships
        Summary: Macro definition for adding faces to ship blueprints
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-face)
     References: This macro is used as follows:
                 * [SHIP_ADDER](../variable/ship_adder.md) uses FACE
                 * [SHIP_ANACONDA](../variable/ship_anaconda.md) uses FACE
                 * [SHIP_ASP_MK_2](../variable/ship_asp_mk_2.md) uses FACE
                 * [SHIP_ASTEROID](../variable/ship_asteroid.md) uses FACE
                 * [SHIP_BOA](../variable/ship_boa.md) uses FACE
                 * [SHIP_BOULDER](../variable/ship_boulder.md) uses FACE
                 * [SHIP_CANISTER](../variable/ship_canister.md) uses FACE
                 * [SHIP_COBRA_MK_1](../variable/ship_cobra_mk_1.md) uses FACE
                 * [SHIP_COBRA_MK_3](../variable/ship_cobra_mk_3.md) uses FACE
                 * [SHIP_CONSTRICTOR](../variable/ship_constrictor.md) uses FACE
                 * [SHIP_CORIOLIS](../variable/ship_coriolis.md) uses FACE
                 * [SHIP_COUGAR](../variable/ship_cougar.md) uses FACE
                 * [SHIP_DODO](../variable/ship_dodo.md) uses FACE
                 * [SHIP_ESCAPE_POD](../variable/ship_escape_pod.md) uses FACE
                 * [SHIP_FER_DE_LANCE](../variable/ship_fer_de_lance.md) uses FACE
                 * [SHIP_GECKO](../variable/ship_gecko.md) uses FACE
                 * [SHIP_KRAIT](../variable/ship_krait.md) uses FACE
                 * [SHIP_MAMBA](../variable/ship_mamba.md) uses FACE
                 * [SHIP_MISSILE](../variable/ship_missile.md) uses FACE
                 * [SHIP_MORAY](../variable/ship_moray.md) uses FACE
                 * [SHIP_PLATE](../variable/ship_plate.md) uses FACE
                 * [SHIP_PYTHON](../variable/ship_python.md) uses FACE
                 * [SHIP_SHUTTLE](../variable/ship_shuttle.md) uses FACE
                 * [SHIP_SIDEWINDER](../variable/ship_sidewinder.md) uses FACE
                 * [SHIP_SPLINTER](../variable/ship_splinter.md) uses FACE
                 * [SHIP_THARGOID](../variable/ship_thargoid.md) uses FACE
                 * [SHIP_THARGON](../variable/ship_thargon.md) uses FACE
                 * [SHIP_TRANSPORTER](../variable/ship_transporter.md) uses FACE
                 * [SHIP_VIPER](../variable/ship_viper.md) uses FACE
                 * [SHIP_WORM](../variable/ship_worm.md) uses FACE
    
    
    
    
    
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
    

[X]

Configuration variable [ax](vertex.md#ax) in macro [VERTEX](vertex.md)

[X]

Configuration variable [ay](vertex.md#ay) in macro [VERTEX](vertex.md)

[X]

Configuration variable [az](vertex.md#az) in macro [VERTEX](vertex.md)

[X]

Configuration variable [s](vertex.md#s) in macro [VERTEX](vertex.md)

[EDGE (Game data)](edge.md "Previous routine")[SHIP_MISSILE (Game data)](../variable/ship_missile.md "Next routine")
