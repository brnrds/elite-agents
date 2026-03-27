---
title: "The sightcol variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/sightcol.html"
---

[SIGHT](../subroutine/sight.md "Previous routine")[beamcol](beamcol.md "Next routine")
    
    
           Name: sightcol                                                [Show more]
           Type: Variable
       Category: Drawing lines
        Summary: Colours for the crosshair sights on the different laser types
    
    
        Context: See this variable [in context in the source code](../../all/elite_h.md#header-sightcol)
     References: This variable is used as follows:
                 * [SIGHT](../subroutine/sight.md) uses sightcol
    
    
    
    
    
    
    .sightcol
    
     EQUB YELLOW            \ Pulse lasers have yellow sights
    
     EQUB CYAN              \ Beam lasers have cyan sights
    
     EQUB CYAN              \ Military lasers have cyan sights
    
     EQUB YELLOW            \ Mining lasers have yellow sights
    

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Configuration variable [YELLOW](../../all/workspaces.md#yellow) = %00001111

Four mode 1 pixels of colour 1 (yellow)

[SIGHT](../subroutine/sight.md "Previous routine")[beamcol](beamcol.md "Next routine")
