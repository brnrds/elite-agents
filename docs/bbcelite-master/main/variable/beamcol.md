---
title: "The beamcol variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/beamcol.html"
---

[sightcol](sightcol.md "Previous routine")[TRIBTA](tribta.md "Next routine")
    
    
           Name: beamcol                                                 [Show more]
           Type: Variable
       Category: Drawing lines
        Summary: An unused table of laser colours
    
    
        Context: See this variable [in context in the source code](../../all/elite_h.md#header-beamcol)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .beamcol
    
     EQUB WHITE             \ These bytes appear to be unused - perhaps they were
     EQUB WHITE             \ going to be used to set different colours of laser
     EQUB WHITE             \ beam for the different lasers?
     EQUB WHITE
    

[X]

Configuration variable [WHITE](../../all/workspaces.md#white) = %11111010

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[sightcol](sightcol.md "Previous routine")[TRIBTA](tribta.md "Next routine")
