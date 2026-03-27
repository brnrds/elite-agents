---
title: "The SHIP_COBRA_MK_3_P (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_cobra_mk_3_p.html"
---

[SHIP_WORM (Game data)](ship_worm.md "Previous routine")[SHIP_ASP_MK_2 (Game data)](ship_asp_mk_2.md "Next routine")
    
    
           Name: SHIP_COBRA_MK_3_P                                       [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a Cobra Mk III (pirate)
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_cobra_mk_3_p)
     Variations: See [code variations](../../related/compare/main/variable/ship_cobra_mk_3_p.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_COBRA_MK_3_P
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the pirate Cobra Mk III reuses the edges and faces data
     from the non-pirate Cobra Mk III, so the edges and faces data offsets are
     negative.
    
    
    
    
    .SHIP_COBRA_MK_3_P
    
     EQUB 1                 \ Max. canisters on demise = 1
     EQUW 95 * 95           \ Targetable area          = 95 * 95
    
     EQUB LO(SHIP_COBRA_MK_3_EDGES - SHIP_COBRA_MK_3_P)   \ Edges from Cobra Mk III
     EQUB LO(SHIP_COBRA_MK_3_FACES - SHIP_COBRA_MK_3_P)   \ Faces from Cobra Mk III
    
     EQUB 157               \ Max. edge count          = (157 - 1) / 4 = 39
     EQUB 84                \ Gun vertex               = 84 / 4 = 21
     EQUB 42                \ Explosion count          = 9, as (4 * n) + 6 = 42
     EQUB 168               \ Number of vertices       = 168 / 6 = 28
     EQUB 38                \ Number of edges          = 38
     EQUW 175               \ Bounty                   = 175
     EQUB 52                \ Number of faces          = 52 / 4 = 13
     EQUB 50                \ Visibility distance      = 50
     EQUB 150               \ Max. energy              = 150
     EQUB 28                \ Max. speed               = 28
    
     EQUB HI(SHIP_COBRA_MK_3_EDGES - SHIP_COBRA_MK_3_P)   \ Edges from Cobra Mk III
     EQUB HI(SHIP_COBRA_MK_3_FACES - SHIP_COBRA_MK_3_P)   \ Faces from Cobra Mk III
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00010010         \ Laser power              = 2
                            \ Missiles                 = 2
    
    .SHIP_COBRA_MK_3_P_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX   32,    0,   76,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX  -32,    0,   76,    15,     15,   15,    15,         31    \ Vertex 1
     VERTEX    0,   26,   24,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX -120,   -3,   -8,     3,      7,   10,    10,         31    \ Vertex 3
     VERTEX  120,   -3,   -8,     4,      8,   12,    12,         31    \ Vertex 4
     VERTEX  -88,   16,  -40,    15,     15,   15,    15,         31    \ Vertex 5
     VERTEX   88,   16,  -40,    15,     15,   15,    15,         31    \ Vertex 6
     VERTEX  128,   -8,  -40,     8,      9,   12,    12,         31    \ Vertex 7
     VERTEX -128,   -8,  -40,     7,      9,   10,    10,         31    \ Vertex 8
     VERTEX    0,   26,  -40,     5,      6,    9,     9,         31    \ Vertex 9
     VERTEX  -32,  -24,  -40,     9,     10,   11,    11,         31    \ Vertex 10
     VERTEX   32,  -24,  -40,     9,     11,   12,    12,         31    \ Vertex 11
     VERTEX  -36,    8,  -40,     9,      9,    9,     9,         20    \ Vertex 12
     VERTEX   -8,   12,  -40,     9,      9,    9,     9,         20    \ Vertex 13
     VERTEX    8,   12,  -40,     9,      9,    9,     9,         20    \ Vertex 14
     VERTEX   36,    8,  -40,     9,      9,    9,     9,         20    \ Vertex 15
     VERTEX   36,  -12,  -40,     9,      9,    9,     9,         20    \ Vertex 16
     VERTEX    8,  -16,  -40,     9,      9,    9,     9,         20    \ Vertex 17
     VERTEX   -8,  -16,  -40,     9,      9,    9,     9,         20    \ Vertex 18
     VERTEX  -36,  -12,  -40,     9,      9,    9,     9,         20    \ Vertex 19
     VERTEX    0,    0,   76,     0,     11,   11,    11,          6    \ Vertex 20
     VERTEX    0,    0,   90,     0,     11,   11,    11,         31    \ Vertex 21
     VERTEX  -80,   -6,  -40,     9,      9,    9,     9,          8    \ Vertex 22
     VERTEX  -80,    6,  -40,     9,      9,    9,     9,          8    \ Vertex 23
     VERTEX  -88,    0,  -40,     9,      9,    9,     9,          6    \ Vertex 24
     VERTEX   80,    6,  -40,     9,      9,    9,     9,          8    \ Vertex 25
     VERTEX   88,    0,  -40,     9,      9,    9,     9,          6    \ Vertex 26
     VERTEX   80,   -6,  -40,     9,      9,    9,     9,          8    \ Vertex 27
    

[X]

Label [SHIP_COBRA_MK_3_EDGES](ship_cobra_mk_3.md#ship_cobra_mk_3_edges) in variable [SHIP_COBRA_MK_3](ship_cobra_mk_3.md)

[X]

Label [SHIP_COBRA_MK_3_FACES](ship_cobra_mk_3.md#ship_cobra_mk_3_faces) in variable [SHIP_COBRA_MK_3](ship_cobra_mk_3.md)

[X]

Variable [SHIP_COBRA_MK_3_P](ship_cobra_mk_3_p.md) (category: Drawing ships)

Ship blueprint for a Cobra Mk III (pirate)

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_WORM (Game data)](ship_worm.md "Previous routine")[SHIP_ASP_MK_2 (Game data)](ship_asp_mk_2.md "Next routine")
