---
title: "The SHIP_THARGON (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_thargon.html"
---

[SHIP_THARGOID (Game data)](ship_thargoid.md "Previous routine")[SHIP_CONSTRICTOR (Game data)](ship_constrictor.md "Next routine")
    
    
           Name: SHIP_THARGON                                            [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Thargon
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_thargon)
     Variations: See [code variations](../../related/compare/main/variable/ship_thargon.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_THARGON
    
    
    
    
    
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
    

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Label [SHIP_CANISTER_EDGES](ship_canister.md#ship_canister_edges) in variable [SHIP_CANISTER](ship_canister.md)

[X]

Variable [SHIP_THARGON](ship_thargon.md) (category: Drawing ships)

Ship blueprint for a Thargon

[X]

Label [SHIP_THARGON_FACES](ship_thargon.md#ship_thargon_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_THARGOID (Game data)](ship_thargoid.md "Previous routine")[SHIP_CONSTRICTOR (Game data)](ship_constrictor.md "Next routine")
