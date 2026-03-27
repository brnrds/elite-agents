---
title: "The SHIP_SPLINTER (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_splinter.html"
---

[SHIP_ASTEROID (Game data)](ship_asteroid.md "Previous routine")[SHIP_SHUTTLE (Game data)](ship_shuttle.md "Next routine")
    
    
           Name: SHIP_SPLINTER                                           [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a splinter
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_splinter)
     Variations: See [code variations](../../related/compare/main/variable/ship_splinter.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_SPLINTER
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the splinter reuses the edges data from the escape pod,
     so the edges data offset is negative.
    
    
    
    
    .SHIP_SPLINTER
    
     EQUB 0 + (11 << 4)     \ Max. canisters on demise = 0
                            \ Market item when scooped = 11 + 1 = 12 (minerals)
     EQUW 16 * 16           \ Targetable area          = 16 * 16
    
     EQUB LO(SHIP_ESCAPE_POD_EDGES - SHIP_SPLINTER)    \ Edges from escape pod
     EQUB LO(SHIP_SPLINTER_FACES - SHIP_SPLINTER) + 24 \ Faces data offset (low)
    
     EQUB 29                \ Max. edge count          = (29 - 1) / 4 = 7
     EQUB 0                 \ Gun vertex               = 0
     EQUB 22                \ Explosion count          = 4, as (4 * n) + 6 = 22
     EQUB 24                \ Number of vertices       = 24 / 6 = 4
     EQUB 6                 \ Number of edges          = 6
     EQUW 0                 \ Bounty                   = 0
     EQUB 16                \ Number of faces          = 16 / 4 = 4
     EQUB 8                 \ Visibility distance      = 8
     EQUB 20                \ Max. energy              = 20
     EQUB 10                \ Max. speed               = 10
    
     EQUB HI(SHIP_ESCAPE_POD_EDGES - SHIP_SPLINTER)    \ Edges from escape pod
     EQUB HI(SHIP_SPLINTER_FACES - SHIP_SPLINTER)      \ Faces data offset (low)
    
     EQUB 5                 \ Normals are scaled by    = 2^5 = 32
     EQUB %00000000         \ Laser power              = 0
                            \ Missiles                 = 0
    
    .SHIP_SPLINTER_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX  -24,  -25,   16,     2,      1,    3,     3,         31    \ Vertex 0
     VERTEX    0,   12,  -10,     2,      0,    3,     3,         31    \ Vertex 1
     VERTEX   11,   -6,    2,     1,      0,    3,     3,         31    \ Vertex 2
     VERTEX   12,   42,    7,     1,      0,    2,     2,         31    \ Vertex 3
    
    .SHIP_SPLINTER_FACES
    
        \ normal_x, normal_y, normal_z, visibility
     FACE       35,        0,        4,         31      \ Face 0
     FACE        3,        4,        8,         31      \ Face 1
     FACE        1,        8,       12,         31      \ Face 2
     FACE       18,       12,        0,         31      \ Face 3
    

[X]

Macro [FACE](../macro/face.md) (category: Drawing ships)

Macro definition for adding faces to ship blueprints

[X]

Label [SHIP_ESCAPE_POD_EDGES](ship_escape_pod.md#ship_escape_pod_edges) in variable [SHIP_ESCAPE_POD](ship_escape_pod.md)

[X]

Variable [SHIP_SPLINTER](ship_splinter.md) (category: Drawing ships)

Ship blueprint for a splinter

[X]

Label [SHIP_SPLINTER_FACES](ship_splinter.md#ship_splinter_faces) is local to this routine

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_ASTEROID (Game data)](ship_asteroid.md "Previous routine")[SHIP_SHUTTLE (Game data)](ship_shuttle.md "Next routine")
