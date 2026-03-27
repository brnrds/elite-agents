---
title: "The SHIP_ADDER (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_adder.html"
---

[SHIP_KRAIT (Game data)](ship_krait.md "Previous routine")[SHIP_GECKO (Game data)](ship_gecko.md "Next routine")
    
    
           Name: SHIP_ADDER                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for an Adder
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_adder)
     Variations: See [code variations](../../related/compare/main/variable/ship_adder.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_ADDER
    
    
    
    
    
    
    .SHIP_ADDER
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 50 * 50           \ Targetable area          = 50 * 50
    
     EQUB LO(SHIP_ADDER_EDGES - SHIP_ADDER)            \ Edges data offset (low)
     EQUB LO(SHIP_ADDER_FACES - SHIP_ADDER)            \ Faces data offset (low)
    
     EQUB 101               \ Max. edge count          = (101 - 1) / 4 = 25
     EQUB 0                 \ Gun vertex               = 0
     EQUB 22                \ Explosion count          = 4, as (4 * n) + 6 = 22
     EQUB 108               \ Number of vertices       = 108 / 6 = 18
     EQUB 29                \ Number of edges          = 29
     EQUW 40                \ Bounty                   = 40
     EQUB 60                \ Number of faces          = 60 / 4 = 15
     EQUB 20                \ Visibility distance      = 20
     EQUB 85                \ Max. energy              = 85
     EQUB 24                \ Max. speed               = 24
    
     EQUB HI(SHIP_ADDER_EDGES - SHIP_ADDER)            \ Edges data offset (high)
     EQUB HI(SHIP_ADDER_FACES - SHIP_ADDER)            \ Faces data offset (high)
    
     EQUB 2                 \ Normals are scaled by    = 2^2 = 4
     EQUB %00010000         \ Laser power              = 2
                            \ Missiles                 = 0
    
    .SHIP_ADDER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -18,    0,   40,     1,      0,   12,    11,         31    \ Vertex 0
     VERTEX   18,    0,   40,     1,      0,    3,     2,         31    \ Vertex 1
     VERTEX   30,    0,  -24,     3,      2,    5,     4,         31    \ Vertex 2
     VERTEX   30,    0,  -40,     5,      4,    6,     6,         31    \ Vertex 3
     VERTEX   18,   -7,  -40,     6,      5,   14,     7,         31    \ Vertex 4
     VERTEX  -18,   -7,  -40,     8,      7,   14,    10,         31    \ Vertex 5
     VERTEX  -30,    0,  -40,     9,      8,   10,    10,         31    \ Vertex 6
     VERTEX  -30,    0,  -24,    10,      9,   12,    11,         31    \ Vertex 7
     VERTEX  -18,    7,  -40,     8,      7,   13,     9,         31    \ Vertex 8
     VERTEX   18,    7,  -40,     6,      4,   13,     7,         31    \ Vertex 9
     VERTEX  -18,    7,   13,     9,      0,   13,    11,         31    \ Vertex 10
     VERTEX   18,    7,   13,     2,      0,   13,     4,         31    \ Vertex 11
     VERTEX  -18,   -7,   13,    10,      1,   14,    12,         31    \ Vertex 12
     VERTEX   18,   -7,   13,     3,      1,   14,     5,         31    \ Vertex 13
     VERTEX  -11,    3,   29,     0,      0,    0,     0,          5    \ Vertex 14
     VERTEX   11,    3,   29,     0,      0,    0,     0,          5    \ Vertex 15
     VERTEX   11,    4,   24,     0,      0,    0,     0,          4    \ Vertex 16
     VERTEX  -11,    4,   24,     0,      0,    0,     0,          4    \ Vertex 17
    
    .SHIP_ADDER_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       1,     1,     0,         31    \ Edge 0
     EDGE       1,       2,     3,     2,          7    \ Edge 1
     EDGE       2,       3,     5,     4,         31    \ Edge 2
     EDGE       3,       4,     6,     5,         31    \ Edge 3
     EDGE       4,       5,    14,     7,         31    \ Edge 4
     EDGE       5,       6,    10,     8,         31    \ Edge 5
     EDGE       6,       7,    10,     9,         31    \ Edge 6
     EDGE       7,       0,    12,    11,          7    \ Edge 7
     EDGE       3,       9,     6,     4,         31    \ Edge 8
     EDGE       9,       8,    13,     7,         31    \ Edge 9
     EDGE       8,       6,     9,     8,         31    \ Edge 10
     EDGE       0,      10,    11,     0,         31    \ Edge 11
     EDGE       7,      10,    11,     9,         31    \ Edge 12
     EDGE       1,      11,     2,     0,         31    \ Edge 13
     EDGE       2,      11,     4,     2,         31    \ Edge 14
     EDGE       0,      12,    12,     1,         31    \ Edge 15
     EDGE       7,      12,    12,    10,         31    \ Edge 16
     EDGE       1,      13,     3,     1,         31    \ Edge 17
     EDGE       2,      13,     5,     3,         31    \ Edge 18
     EDGE      10,      11,    13,     0,         31    \ Edge 19
     EDGE      12,      13,    14,     1,         31    \ Edge 20
     EDGE       8,      10,    13,     9,         31    \ Edge 21
     EDGE       9,      11,    13,     4,         31    \ Edge 22
     EDGE       5,      12,    14,    10,         31    \ Edge 23
     EDGE       4,      13,    14,     5,         31    \ Edge 24
     EDGE      14,      15,     0,     0,          5    \ Edge 25
     EDGE      15,      16,     0,     0,          3    \ Edge 26
     EDGE      16,      17,     0,     0,          4    \ Edge 27
     EDGE      17,      14,     0,     0,          3    \ Edge 28
    
    .SHIP_ADDER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       39,       10,         31      \ Face 0
     FACE        0,      -39,       10,         31      \ Face 1
     FACE       69,       50,       13,         31      \ Face 2
     FACE       69,      -50,       13,         31      \ Face 3
     FACE       30,       52,        0,         31      \ Face 4
     FACE       30,      -52,        0,         31      \ Face 5
     FACE        0,        0,     -160,         31      \ Face 6
     FACE        0,        0,     -160,         31      \ Face 7
     FACE        0,        0,     -160,         31      \ Face 8
     FACE      -30,       52,        0,         31      \ Face 9
     FACE      -30,      -52,        0,         31      \ Face 10
     FACE      -69,       50,       13,         31      \ Face 11
     FACE      -69,      -50,       13,         31      \ Face 12
     FACE        0,       28,        0,         31      \ Face 13
     FACE        0,      -28,        0,         31      \ Face 14
    

[X]

Macro [EDGE](../macro/edge.md) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_ADDER](ship_adder.md) (category: Drawing ships)

Ship blueprint for an Adder

[X]

Label [SHIP_ADDER_EDGES](ship_adder.md#ship_adder_edges) is local to this routine

[X]

Label [SHIP_ADDER_FACES](ship_adder.md#ship_adder_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_KRAIT (Game data)](ship_krait.md "Previous routine")[SHIP_GECKO (Game data)](ship_gecko.md "Next routine")
