---
title: "The SHIP_KRAIT (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_krait.html"
---

[SHIP_MAMBA (Game data)](ship_mamba.md "Previous routine")[SHIP_ADDER (Game data)](ship_adder.md "Next routine")
    
    
           Name: SHIP_KRAIT                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Krait
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_krait)
     Variations: See [code variations](../../related/compare/main/variable/ship_krait.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_KRAIT
    
    
    
    
    
    
    .SHIP_KRAIT
    
     EQUB 1                 \ Max. canisters on demise = 1
     EQUW 60 * 60           \ Targetable area          = 60 * 60
    
     EQUB LO(SHIP_KRAIT_EDGES - SHIP_KRAIT)            \ Edges data offset (low)
     EQUB LO(SHIP_KRAIT_FACES - SHIP_KRAIT)            \ Faces data offset (low)
    
     EQUB 89                \ Max. edge count          = (89 - 1) / 4 = 22
     EQUB 0                 \ Gun vertex               = 0
     EQUB 18                \ Explosion count          = 3, as (4 * n) + 6 = 18
     EQUB 102               \ Number of vertices       = 102 / 6 = 17
     EQUB 21                \ Number of edges          = 21
     EQUW 100               \ Bounty                   = 100
     EQUB 24                \ Number of faces          = 24 / 4 = 6
     EQUB 20                \ Visibility distance      = 20
     EQUB 80                \ Max. energy              = 80
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_KRAIT_EDGES - SHIP_KRAIT)            \ Edges data offset (high)
     EQUB HI(SHIP_KRAIT_FACES - SHIP_KRAIT)            \ Faces data offset (high)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00010000         \ Laser power              = 2
                            \ Missiles                 = 0
    
    .SHIP_KRAIT_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,   96,     1,      0,    3,     2,         31    \ Vertex 0
     VERTEX    0,   18,  -48,     3,      0,    5,     4,         31    \ Vertex 1
     VERTEX    0,  -18,  -48,     2,      1,    5,     4,         31    \ Vertex 2
     VERTEX   90,    0,   -3,     1,      0,    4,     4,         31    \ Vertex 3
     VERTEX  -90,    0,   -3,     3,      2,    5,     5,         31    \ Vertex 4
     VERTEX   90,    0,   87,     1,      0,    1,     1,         30    \ Vertex 5
     VERTEX  -90,    0,   87,     3,      2,    3,     3,         30    \ Vertex 6
     VERTEX    0,    5,   53,     0,      0,    3,     3,          9    \ Vertex 7
     VERTEX    0,    7,   38,     0,      0,    3,     3,          6    \ Vertex 8
     VERTEX  -18,    7,   19,     3,      3,    3,     3,          9    \ Vertex 9
     VERTEX   18,    7,   19,     0,      0,    0,     0,          9    \ Vertex 10
     VERTEX   18,   11,  -39,     4,      4,    4,     4,          8    \ Vertex 11
     VERTEX   18,  -11,  -39,     4,      4,    4,     4,          8    \ Vertex 12
     VERTEX   36,    0,  -30,     4,      4,    4,     4,          8    \ Vertex 13
     VERTEX  -18,   11,  -39,     5,      5,    5,     5,          8    \ Vertex 14
     VERTEX  -18,  -11,  -39,     5,      5,    5,     5,          8    \ Vertex 15
     VERTEX  -36,    0,  -30,     5,      5,    5,     5,          8    \ Vertex 16
    
    .SHIP_KRAIT_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     3,     0,         31    \ Edge 0
     EDGE       0,       2,     2,     1,         31    \ Edge 1
     EDGE       0,       3,     1,     0,         31    \ Edge 2
     EDGE       0,       4,     3,     2,         31    \ Edge 3
     EDGE       1,       4,     5,     3,         31    \ Edge 4
     EDGE       4,       2,     5,     2,         31    \ Edge 5
     EDGE       2,       3,     4,     1,         31    \ Edge 6
     EDGE       3,       1,     4,     0,         31    \ Edge 7
     EDGE       3,       5,     1,     0,         30    \ Edge 8
     EDGE       4,       6,     3,     2,         30    \ Edge 9
     EDGE       1,       2,     5,     4,          8    \ Edge 10
     EDGE       7,      10,     0,     0,          9    \ Edge 11
     EDGE       8,      10,     0,     0,          6    \ Edge 12
     EDGE       7,       9,     3,     3,          9    \ Edge 13
     EDGE       8,       9,     3,     3,          6    \ Edge 14
     EDGE      11,      13,     4,     4,          8    \ Edge 15
     EDGE      13,      12,     4,     4,          8    \ Edge 16
     EDGE      12,      11,     4,     4,          7    \ Edge 17
     EDGE      14,      15,     5,     5,          7    \ Edge 18
     EDGE      15,      16,     5,     5,          8    \ Edge 19
     EDGE      16,      14,     5,     5,          8    \ Edge 20
    
    .SHIP_KRAIT_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        3,       24,        3,         31      \ Face 0
     FACE        3,      -24,        3,         31      \ Face 1
     FACE       -3,      -24,        3,         31      \ Face 2
     FACE       -3,       24,        3,         31      \ Face 3
     FACE       38,        0,      -77,         31      \ Face 4
     FACE      -38,        0,      -77,         31      \ Face 5
    

[X]

Macro [EDGE](../macro/edge.md) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_KRAIT](ship_krait.md) (category: Drawing ships)

Ship blueprint for a Krait

[X]

Label [SHIP_KRAIT_EDGES](ship_krait.md#ship_krait_edges) is local to this routine

[X]

Label [SHIP_KRAIT_FACES](ship_krait.md#ship_krait_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_MAMBA (Game data)](ship_mamba.md "Previous routine")[SHIP_ADDER (Game data)](ship_adder.md "Next routine")
