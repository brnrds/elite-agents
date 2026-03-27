---
title: "The EDGE (Game data) macro - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/macro/edge.html"
---

[VERTEX (Game data)](vertex.md "Previous routine")[FACE (Game data)](face.md "Next routine")
    
    
           Name: EDGE                                                    [Show more]
           Type: Macro
       Category: Drawing ships
        Summary: Macro definition for adding edges to ship blueprints
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this macro [in context in the source code](../../all/elite_data.md#header-edge)
     References: This macro is used as follows:
                 * [SHIP_ADDER](../variable/ship_adder.md) uses EDGE
                 * [SHIP_ANACONDA](../variable/ship_anaconda.md) uses EDGE
                 * [SHIP_ASP_MK_2](../variable/ship_asp_mk_2.md) uses EDGE
                 * [SHIP_ASTEROID](../variable/ship_asteroid.md) uses EDGE
                 * [SHIP_BOA](../variable/ship_boa.md) uses EDGE
                 * [SHIP_BOULDER](../variable/ship_boulder.md) uses EDGE
                 * [SHIP_CANISTER](../variable/ship_canister.md) uses EDGE
                 * [SHIP_COBRA_MK_1](../variable/ship_cobra_mk_1.md) uses EDGE
                 * [SHIP_COBRA_MK_3](../variable/ship_cobra_mk_3.md) uses EDGE
                 * [SHIP_CONSTRICTOR](../variable/ship_constrictor.md) uses EDGE
                 * [SHIP_CORIOLIS](../variable/ship_coriolis.md) uses EDGE
                 * [SHIP_COUGAR](../variable/ship_cougar.md) uses EDGE
                 * [SHIP_DODO](../variable/ship_dodo.md) uses EDGE
                 * [SHIP_ESCAPE_POD](../variable/ship_escape_pod.md) uses EDGE
                 * [SHIP_FER_DE_LANCE](../variable/ship_fer_de_lance.md) uses EDGE
                 * [SHIP_GECKO](../variable/ship_gecko.md) uses EDGE
                 * [SHIP_KRAIT](../variable/ship_krait.md) uses EDGE
                 * [SHIP_MAMBA](../variable/ship_mamba.md) uses EDGE
                 * [SHIP_MISSILE](../variable/ship_missile.md) uses EDGE
                 * [SHIP_MORAY](../variable/ship_moray.md) uses EDGE
                 * [SHIP_PLATE](../variable/ship_plate.md) uses EDGE
                 * [SHIP_PYTHON](../variable/ship_python.md) uses EDGE
                 * [SHIP_SHUTTLE](../variable/ship_shuttle.md) uses EDGE
                 * [SHIP_SIDEWINDER](../variable/ship_sidewinder.md) uses EDGE
                 * [SHIP_THARGOID](../variable/ship_thargoid.md) uses EDGE
                 * [SHIP_TRANSPORTER](../variable/ship_transporter.md) uses EDGE
                 * [SHIP_VIPER](../variable/ship_viper.md) uses EDGE
                 * [SHIP_WORM](../variable/ship_worm.md) uses EDGE
    
    
    
    
    
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
    

[X]

Configuration variable [f](edge.md#f) is local to this routine

[X]

[X]

[X]

[VERTEX (Game data)](vertex.md "Previous routine")[FACE (Game data)](face.md "Next routine")
