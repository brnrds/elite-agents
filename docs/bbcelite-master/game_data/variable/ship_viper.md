---
title: "The SHIP_VIPER (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_viper.html"
---

[SHIP_ROCK_HERMIT (Game data)](ship_rock_hermit.md "Previous routine")[SHIP_SIDEWINDER (Game data)](ship_sidewinder.md "Next routine")
    
    
           Name: SHIP_VIPER                                              [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Viper
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_viper)
     Variations: See [code variations](../../related/compare/main/variable/ship_viper.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_VIPER
    
    
    
    
    
    
    .SHIP_VIPER
    
     EQUB 0                 \ Max. canisters on demise = 0
     EQUW 75 * 75           \ Targetable area          = 75 * 75
    
     EQUB LO(SHIP_VIPER_EDGES - SHIP_VIPER)            \ Edges data offset (low)
     EQUB LO(SHIP_VIPER_FACES - SHIP_VIPER)            \ Faces data offset (low)
    
     EQUB 81                \ Max. edge count          = (81 - 1) / 4 = 20
     EQUB 0                 \ Gun vertex               = 0
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 90                \ Number of vertices       = 90 / 6 = 15
     EQUB 20                \ Number of edges          = 20
     EQUW 0                 \ Bounty                   = 0
     EQUB 28                \ Number of faces          = 28 / 4 = 7
     EQUB 23                \ Visibility distance      = 23
     EQUB 140               \ Max. energy              = 140
     EQUB 32                \ Max. speed               = 32
    
     EQUB HI(SHIP_VIPER_EDGES - SHIP_VIPER)            \ Edges data offset (high)
     EQUB HI(SHIP_VIPER_FACES - SHIP_VIPER)            \ Faces data offset (high)
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00010001         \ Laser power              = 2
                            \ Missiles                 = 1
    
    .SHIP_VIPER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,   72,     1,      2,    3,     4,         31    \ Vertex 0
     VERTEX    0,   16,   24,     0,      1,    2,     2,         30    \ Vertex 1
     VERTEX    0,  -16,   24,     3,      4,    5,     5,         30    \ Vertex 2
     VERTEX   48,    0,  -24,     2,      4,    6,     6,         31    \ Vertex 3
     VERTEX  -48,    0,  -24,     1,      3,    6,     6,         31    \ Vertex 4
     VERTEX   24,  -16,  -24,     4,      5,    6,     6,         30    \ Vertex 5
     VERTEX  -24,  -16,  -24,     5,      3,    6,     6,         30    \ Vertex 6
     VERTEX   24,   16,  -24,     0,      2,    6,     6,         31    \ Vertex 7
     VERTEX  -24,   16,  -24,     0,      1,    6,     6,         31    \ Vertex 8
     VERTEX  -32,    0,  -24,     6,      6,    6,     6,         19    \ Vertex 9
     VERTEX   32,    0,  -24,     6,      6,    6,     6,         19    \ Vertex 10
     VERTEX    8,    8,  -24,     6,      6,    6,     6,         19    \ Vertex 11
     VERTEX   -8,    8,  -24,     6,      6,    6,     6,         19    \ Vertex 12
     VERTEX   -8,   -8,  -24,     6,      6,    6,     6,         18    \ Vertex 13
     VERTEX    8,   -8,  -24,     6,      6,    6,     6,         18    \ Vertex 14
    
    .SHIP_VIPER_EDGES
    
        \ vertex1, vertex2, face1, face2, visibility
     EDGE       0,       3,     2,     4,         31    \ Edge 0
     EDGE       0,       1,     1,     2,         30    \ Edge 1
     EDGE       0,       2,     3,     4,         30    \ Edge 2
     EDGE       0,       4,     1,     3,         31    \ Edge 3
     EDGE       1,       7,     0,     2,         30    \ Edge 4
     EDGE       1,       8,     0,     1,         30    \ Edge 5
     EDGE       2,       5,     4,     5,         30    \ Edge 6
     EDGE       2,       6,     3,     5,         30    \ Edge 7
     EDGE       7,       8,     0,     6,         31    \ Edge 8
     EDGE       5,       6,     5,     6,         30    \ Edge 9
     EDGE       4,       8,     1,     6,         31    \ Edge 10
     EDGE       4,       6,     3,     6,         30    \ Edge 11
     EDGE       3,       7,     2,     6,         31    \ Edge 12
     EDGE       3,       5,     6,     4,         30    \ Edge 13
     EDGE       9,      12,     6,     6,         19    \ Edge 14
     EDGE       9,      13,     6,     6,         18    \ Edge 15
     EDGE      10,      11,     6,     6,         19    \ Edge 16
     EDGE      10,      14,     6,     6,         18    \ Edge 17
     EDGE      11,      14,     6,     6,         16    \ Edge 18
     EDGE      12,      13,     6,     6,         16    \ Edge 19
    
    .SHIP_VIPER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE        0,       32,        0,         31      \ Face 0
     FACE      -22,       33,       11,         31      \ Face 1
     FACE       22,       33,       11,         31      \ Face 2
     FACE      -22,      -33,       11,         31      \ Face 3
     FACE       22,      -33,       11,         31      \ Face 4
     FACE        0,      -32,        0,         31      \ Face 5
     FACE        0,        0,      -48,         31      \ Face 6
    

[X]

Macro [EDGE](../macro/edge.md) (category: Drawing ships)

Macro definition for adding edges to ship blueprints

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Variable [SHIP_VIPER](ship_viper.md) (category: Drawing ships)

Ship blueprint for a Viper

[X]

Label [SHIP_VIPER_EDGES](ship_viper.md#ship_viper_edges) is local to this routine

[X]

Label [SHIP_VIPER_FACES](ship_viper.md#ship_viper_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_ROCK_HERMIT (Game data)](ship_rock_hermit.md "Previous routine")[SHIP_SIDEWINDER (Game data)](ship_sidewinder.md "Next routine")
