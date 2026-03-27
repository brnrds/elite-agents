---
title: "The SHIP_ESCAPE_POD (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_escape_pod.html"
---

[SHIP_CORIOLIS (Game data)](ship_coriolis.md "Previous routine")[SHIP_PLATE (Game data)](ship_plate.md "Next routine")
    
    
           Name: SHIP_ESCAPE_POD                                         [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an escape pod
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_escape_pod)
     Variations: See [code variations](../../related/compare/main/variable/ship_escape_pod.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_ESCAPE_POD
    
    
    
    
    
    
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
    

[X]

Macro [EDGE](../macro/edge.md) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_ESCAPE_POD](ship_escape_pod.md) (category: Drawing ships)

Ship blueprint for an escape pod

[X]

Label [SHIP_ESCAPE_POD_EDGES](ship_escape_pod.md#ship_escape_pod_edges) is local to this routine

[X]

Label [SHIP_ESCAPE_POD_FACES](ship_escape_pod.md#ship_escape_pod_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_CORIOLIS (Game data)](ship_coriolis.md "Previous routine")[SHIP_PLATE (Game data)](ship_plate.md "Next routine")
