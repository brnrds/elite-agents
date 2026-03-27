---
title: "The coltabl variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/coltabl.html"
---

[exlook](exlook.md "Previous routine")[SOS1](../subroutine/sos1.md "Next routine")
    
    
           Name: coltabl                                                 [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Colours for ship explosions
    
    
        Context: See this variable [in context in the source code](../../all/elite_e.md#header-coltabl)
     References: This variable is used as follows:
                 * [DOEXP](../subroutine/doexp.md) uses coltabl
    
    
    
    
    
    
    .coltabl
    
     EQUB YELLOW
     EQUB RED
     EQUB YELLOW
     EQUB CYAN
    

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Configuration variable [RED](../../all/workspaces.md#red) = %11110000

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Configuration variable [YELLOW](../../all/workspaces.md#yellow) = %00001111

Four mode 1 pixels of colour 1 (yellow)

[exlook](exlook.md "Previous routine")[SOS1](../subroutine/sos1.md "Next routine")
