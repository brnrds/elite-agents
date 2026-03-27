---
title: "The SHIP_WORM (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_worm.html"
---

[SHIP_COBRA_MK_1 (Game data)](ship_cobra_mk_1.md "Previous routine")[SHIP_COBRA_MK_3_P (Game data)](ship_cobra_mk_3_p.md "Next routine")
    
    
           Name: SHIP_WORM                                               [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Worm
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_worm)
     Variations: See [code variations](../../related/compare/main/variable/ship_worm.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_WORM
    
    
    
    
    
    
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
    

[X]

Macro [EDGE](../macro/edge.md) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_WORM](ship_worm.md) (category: Drawing ships)

Ship blueprint for a Worm

[X]

Label [SHIP_WORM_EDGES](ship_worm.md#ship_worm_edges) is local to this routine

[X]

Label [SHIP_WORM_FACES](ship_worm.md#ship_worm_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_COBRA_MK_1 (Game data)](ship_cobra_mk_1.md "Previous routine")[SHIP_COBRA_MK_3_P (Game data)](ship_cobra_mk_3_p.md "Next routine")
