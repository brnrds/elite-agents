---
title: "The VERTEX (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/vertex.html"
---

[KWH% (Game data)](../variable/kwh_per_cent.md "Previous routine")[EDGE (Game data)](edge.md "Next routine")
    
    
           Name: VERTEX                                                  [Show more]
           Type: Macro
       Category: Drawing ships
        Summary: Macro definition for adding vertices to ship blueprints
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-vertex)
     References: This macro is used as follows:
                 * [SHIP_ADDER](../variable/ship_adder.md) uses VERTEX
                 * [SHIP_ANACONDA](../variable/ship_anaconda.md) uses VERTEX
                 * [SHIP_ASP_MK_2](../variable/ship_asp_mk_2.md) uses VERTEX
                 * [SHIP_ASTEROID](../variable/ship_asteroid.md) uses VERTEX
                 * [SHIP_BOA](../variable/ship_boa.md) uses VERTEX
                 * [SHIP_BOULDER](../variable/ship_boulder.md) uses VERTEX
                 * [SHIP_CANISTER](../variable/ship_canister.md) uses VERTEX
                 * [SHIP_COBRA_MK_1](../variable/ship_cobra_mk_1.md) uses VERTEX
                 * [SHIP_COBRA_MK_3](../variable/ship_cobra_mk_3.md) uses VERTEX
                 * [SHIP_COBRA_MK_3_P](../variable/ship_cobra_mk_3_p.md) uses VERTEX
                 * [SHIP_CONSTRICTOR](../variable/ship_constrictor.md) uses VERTEX
                 * [SHIP_CORIOLIS](../variable/ship_coriolis.md) uses VERTEX
                 * [SHIP_COUGAR](../variable/ship_cougar.md) uses VERTEX
                 * [SHIP_DODO](../variable/ship_dodo.md) uses VERTEX
                 * [SHIP_ESCAPE_POD](../variable/ship_escape_pod.md) uses VERTEX
                 * [SHIP_FER_DE_LANCE](../variable/ship_fer_de_lance.md) uses VERTEX
                 * [SHIP_GECKO](../variable/ship_gecko.md) uses VERTEX
                 * [SHIP_KRAIT](../variable/ship_krait.md) uses VERTEX
                 * [SHIP_MAMBA](../variable/ship_mamba.md) uses VERTEX
                 * [SHIP_MISSILE](../variable/ship_missile.md) uses VERTEX
                 * [SHIP_MORAY](../variable/ship_moray.md) uses VERTEX
                 * [SHIP_PLATE](../variable/ship_plate.md) uses VERTEX
                 * [SHIP_PYTHON](../variable/ship_python.md) uses VERTEX
                 * [SHIP_PYTHON_P](../variable/ship_python_p.md) uses VERTEX
                 * [SHIP_ROCK_HERMIT](../variable/ship_rock_hermit.md) uses VERTEX
                 * [SHIP_SHUTTLE](../variable/ship_shuttle.md) uses VERTEX
                 * [SHIP_SIDEWINDER](../variable/ship_sidewinder.md) uses VERTEX
                 * [SHIP_SPLINTER](../variable/ship_splinter.md) uses VERTEX
                 * [SHIP_THARGOID](../variable/ship_thargoid.md) uses VERTEX
                 * [SHIP_THARGON](../variable/ship_thargon.md) uses VERTEX
                 * [SHIP_TRANSPORTER](../variable/ship_transporter.md) uses VERTEX
                 * [SHIP_VIPER](../variable/ship_viper.md) uses VERTEX
                 * [SHIP_WORM](../variable/ship_worm.md) uses VERTEX
    
    
    
    
    
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
    

[X]

Configuration variable [ax](vertex.md#ax) is local to this routine

[X]

Configuration variable [ay](vertex.md#ay) is local to this routine

[X]

Configuration variable [az](vertex.md#az) is local to this routine

[X]

Configuration variable [f1](vertex.md#f1) is local to this routine

[X]

Configuration variable [f2](vertex.md#f2) is local to this routine

[X]

Configuration variable [s](vertex.md#s) is local to this routine

[KWH% (Game data)](../variable/kwh_per_cent.md "Previous routine")[EDGE (Game data)](edge.md "Next routine")
