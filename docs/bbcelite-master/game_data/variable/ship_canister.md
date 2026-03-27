---
title: "The SHIP_CANISTER (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_canister.html"
---

[SHIP_PLATE (Game data)](ship_plate.md "Previous routine")[SHIP_BOULDER (Game data)](ship_boulder.md "Next routine")
    
    
           Name: SHIP_CANISTER                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a cargo canister
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_canister)
     Variations: See [code variations](../../related/compare/main/variable/ship_canister.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_CANISTER
    
    
    
    
    
    
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
    

[X]

Macro [EDGE](../macro/edge.md) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_CANISTER](ship_canister.md) (category: Drawing ships)

Ship blueprint for a cargo canister

[X]

Label [SHIP_CANISTER_EDGES](ship_canister.md#ship_canister_edges) is local to this routine

[X]

Label [SHIP_CANISTER_FACES](ship_canister.md#ship_canister_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_PLATE (Game data)](ship_plate.md "Previous routine")[SHIP_BOULDER (Game data)](ship_boulder.md "Next routine")
