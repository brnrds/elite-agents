---
title: "The SHIP_ROCK_HERMIT (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/ship_rock_hermit.html"
---

[SHIP_ANACONDA (Game data)](ship_anaconda.md "Previous routine")[SHIP_VIPER (Game data)](ship_viper.md "Next routine")
    
    
           Name: SHIP_ROCK_HERMIT                                        [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship blueprint for a rock hermit (asteroid)
      Deep dive: [Ship blueprints](https://elite.bbcelite.com/deep_dives/ship_blueprints.html)
                 [Comparing ship specifications](https://elite.bbcelite.com/deep_dives/comparing_ship_specifications.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-ship_rock_hermit)
     Variations: See [code variations](../../related/compare/main/variable/ship_rock_hermit.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [XX21](xx21.md) uses SHIP_ROCK_HERMIT
    
    
    
    
    
    * * *
    
    
     The ship blueprint for the rock hermit reuses the edges and faces data from
     the asteroid, so the edges and faces data offsets are negative.
    
    
    
    
    .SHIP_ROCK_HERMIT
    
     EQUB 7                 \ Max. canisters on demise = 7
     EQUW 80 * 80           \ Targetable area          = 80 * 80
    
     EQUB LO(SHIP_ASTEROID_EDGES - SHIP_ROCK_HERMIT)   \ Edges from asteroid
     EQUB LO(SHIP_ASTEROID_FACES - SHIP_ROCK_HERMIT)   \ Faces from asteroid
    
     EQUB 69                \ Max. edge count          = (69 - 1) / 4 = 17
     EQUB 0                 \ Gun vertex               = 0
     EQUB 50                \ Explosion count          = 11, as (4 * n) + 6 = 50
     EQUB 54                \ Number of vertices       = 54 / 6 = 9
     EQUB 21                \ Number of edges          = 21
     EQUW 0                 \ Bounty                   = 0
     EQUB 56                \ Number of faces          = 56 / 4 = 14
     EQUB 50                \ Visibility distance      = 50
     EQUB 180               \ Max. energy              = 180
     EQUB 30                \ Max. speed               = 30
    
     EQUB HI(SHIP_ASTEROID_EDGES - SHIP_ROCK_HERMIT)   \ Edges from asteroid
     EQUB HI(SHIP_ASTEROID_FACES - SHIP_ROCK_HERMIT)   \ Faces from asteroid
    
     EQUB 1                 \ Normals are scaled by    = 2^1 = 2
     EQUB %00000010         \ Laser power              = 0
                            \ Missiles                 = 2
    
    .SHIP_ROCK_HERMIT_VERTICES
    
          \    x,    y,    z, face1, face2, face3, face4, visibility
     VERTEX    0,   80,    0,    15,     15,   15,    15,         31    \ Vertex 0
     VERTEX  -80,  -10,    0,    15,     15,   15,    15,         31    \ Vertex 1
     VERTEX    0,  -80,    0,    15,     15,   15,    15,         31    \ Vertex 2
     VERTEX   70,  -40,    0,    15,     15,   15,    15,         31    \ Vertex 3
     VERTEX   60,   50,    0,     5,      6,   12,    13,         31    \ Vertex 4
     VERTEX   50,    0,   60,    15,     15,   15,    15,         31    \ Vertex 5
     VERTEX  -40,    0,   70,     0,      1,    2,     3,         31    \ Vertex 6
     VERTEX    0,   30,  -75,    15,     15,   15,    15,         31    \ Vertex 7
     VERTEX    0,  -50,  -60,     8,      9,   10,    11,         31    \ Vertex 8
    

[X]

Label [SHIP_ASTEROID_EDGES](ship_asteroid.md#ship_asteroid_edges) in variable [SHIP_ASTEROID](ship_asteroid.md)

[X]

Label [SHIP_ASTEROID_FACES](ship_asteroid.md#ship_asteroid_faces) in variable [SHIP_ASTEROID](ship_asteroid.md)

[X]

Variable [SHIP_ROCK_HERMIT](ship_rock_hermit.md) (category: Drawing ships)

Ship blueprint for a rock hermit (asteroid)

[X]

Macro [VERTEX](../macro/vertex.md) (category: Drawing ships)

Macro definition for adding vertices to ship blueprints

[SHIP_ANACONDA (Game data)](ship_anaconda.md "Previous routine")[SHIP_VIPER (Game data)](ship_viper.md "Next routine")
