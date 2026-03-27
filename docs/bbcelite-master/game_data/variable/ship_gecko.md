---
title: "The SHIP_GECKO (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_gecko.html"
---

[SHIP_ADDER (Game data)](ship_adder.md "Previous routine")[SHIP_COBRA_MK_1 (Game data)](ship_cobra_mk_1.md "Next routine")
    
    
           Name: SHIP_GECKO                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Gecko
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_gecko)
     Variations: See [code variations](../../related/compare/main/variable/ship_gecko.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_GECKO
    
    
    
    
    
    
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
    

[X]

Macro [EDGE](../macro/edge.md) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_GECKO](ship_gecko.md) (category: Drawing ships)

Ship blueprint for a Gecko

[X]

Label [SHIP_GECKO_EDGES](ship_gecko.md#ship_gecko_edges) is local to this routine

[X]

Label [SHIP_GECKO_FACES](ship_gecko.md#ship_gecko_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_ADDER (Game data)](ship_adder.md "Previous routine")[SHIP_COBRA_MK_1 (Game data)](ship_cobra_mk_1.md "Next routine")
