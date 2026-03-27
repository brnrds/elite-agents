---
title: "The SHIP_PYTHON_P (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_python_p.html"
---

[SHIP_ASP_MK_2 (Game data)](ship_asp_mk_2.md "Previous routine")[SHIP_FER_DE_LANCE (Game data)](ship_fer_de_lance.md "Next routine")
    
    
           Name: SHIP_PYTHON_P                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Python (pirate)
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_python_p)
     Variations: See [code variations](../../related/compare/main/variable/ship_python_p.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_PYTHON_P
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the pirate Python reuses the edges and faces data from
     the non-pirate Python, so the edges and faces data offsets are negative.
    
    
    
    
    .SHIP_PYTHON_P
    
     EQUB 2                 \ Max. canisters on demise = 2
     EQUW 80 * 80           \ Targetable area          = 80 * 80
    
     EQUB LO(SHIP_PYTHON_EDGES - SHIP_PYTHON_P)        \ Edges from Python
     EQUB LO(SHIP_PYTHON_FACES - SHIP_PYTHON_P)        \ Faces from Python
    
     EQUB 89                \ Max. edge count          = (89 - 1) / 4 = 22
     EQUB 0                 \ Gun vertex               = 0
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 66                \ Number of vertices       = 66 / 6 = 11
     EQUB 26                \ Number of edges          = 26
     EQUW 200               \ Bounty                   = 200
     EQUB 52                \ Number of faces          = 52 / 4 = 13
     EQUB 40                \ Visibility distance      = 40
     EQUB 250               \ Max. energy              = 250
     EQUB 20                \ Max. speed               = 20
    
     EQUB HI(SHIP_PYTHON_EDGES - SHIP_PYTHON_P)        \ Edges from Python
     EQUB HI(SHIP_PYTHON_FACES - SHIP_PYTHON_P)        \ Faces from Python
    
     EQUB 0                 \ Normals are scaled by    = 2^0 = 1
     EQUB %00011011         \ Laser power              = 3
                            \ Missiles                 = 3
    
    .SHIP_PYTHON_P_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,    0,  224,     0,      1,    2,     3,         31    \ Vertex 0
     VERTEX    0,   48,   48,     0,      1,    4,     5,         31    \ Vertex 1
     VERTEX   96,    0,  -16,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX  -96,    0,  -16,    15,     15,   15,    15,         31    \ Vertex 3
     VERTEX    0,   48,  -32,     4,      5,    8,     9,         31    \ Vertex 4
     VERTEX    0,   24, -112,     9,      8,   12,    12,         31    \ Vertex 5
     VERTEX  -48,    0, -112,     8,     11,   12,    12,         31    \ Vertex 6
     VERTEX   48,    0, -112,     9,     10,   12,    12,         31    \ Vertex 7
     VERTEX    0,  -48,   48,     2,      3,    6,     7,         31    \ Vertex 8
     VERTEX    0,  -48,  -32,     6,      7,   10,    11,         31    \ Vertex 9
     VERTEX    0,  -24, -112,    10,     11,   12,    12,         31    \ Vertex 10
    

[X]

Label [SHIP_PYTHON_EDGES](ship_python.md#ship_python_edges) in variable [SHIP_PYTHON](ship_python.md)

[X]

Label [SHIP_PYTHON_FACES](ship_python.md#ship_python_faces) in variable [SHIP_PYTHON](ship_python.md)

[X]

Variable [SHIP_PYTHON_P](ship_python_p.md) (category: Drawing ships)

Ship blueprint for a Python (pirate)

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_ASP_MK_2 (Game data)](ship_asp_mk_2.md "Previous routine")[SHIP_FER_DE_LANCE (Game data)](ship_fer_de_lance.md "Next routine")
