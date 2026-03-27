---
title: "The SHIP_PLATE (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_plate.html"
---

[SHIP_ESCAPE_POD (Game data)](ship_escape_pod.md "Previous routine")[SHIP_CANISTER (Game data)](ship_canister.md "Next routine")
    
    
           Name: SHIP_PLATE                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an alloy plate
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_plate)
     Variations: See [code variations](../../related/compare/main/variable/ship_plate.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_PLATE
    
    
    
    
    
    
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
    

[X]

Macro [EDGE](../macro/edge.md) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_PLATE](ship_plate.md) (category: Drawing ships)

Ship blueprint for an alloy plate

[X]

Label [SHIP_PLATE_EDGES](ship_plate.md#ship_plate_edges) is local to this routine

[X]

Label [SHIP_PLATE_FACES](ship_plate.md#ship_plate_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_ESCAPE_POD (Game data)](ship_escape_pod.md "Previous routine")[SHIP_CANISTER (Game data)](ship_canister.md "Next routine")
