---
title: "The shpcol variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/shpcol.html"
---

[NA2%](na2_per_cent.md "Previous routine")[scacol](scacol.md "Next routine")
    
    
           Name: shpcol                                                  [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship colours
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-shpcol)
     Variations: See [code variations](../../related/compare/main/variable/shpcol.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md) uses shpcol
    
    
    
    
    
    
    .shpcol
    
     EQUB 0
    
     EQUB YELLOW            \ Missile
     EQUB CYAN              \ Coriolis space station
     EQUB CYAN              \ Escape pod
     EQUB CYAN              \ Alloy plate
     EQUB CYAN              \ Cargo canister
     EQUB RED               \ Boulder
     EQUB RED               \ Asteroid
     EQUB RED               \ Splinter
     EQUB CYAN              \ Shuttle
     EQUB CYAN              \ Transporter
     EQUB CYAN              \ Cobra Mk III
     EQUB CYAN              \ Python
     EQUB CYAN              \ Boa
     EQUB CYAN              \ Anaconda
     EQUB RED               \ Rock hermit (asteroid)
     EQUB CYAN              \ Viper
     EQUB CYAN              \ Sidewinder
     EQUB CYAN              \ Mamba
     EQUB CYAN              \ Krait
     EQUB CYAN              \ Adder
     EQUB CYAN              \ Gecko
     EQUB CYAN              \ Cobra Mk I
     EQUB CYAN              \ Worm
     EQUB CYAN              \ Cobra Mk III (pirate)
     EQUB CYAN              \ Asp Mk II
     EQUB CYAN              \ Python (pirate)
     EQUB CYAN              \ Fer-de-lance
     EQUB %11001001         \ Moray (colour 3, 2, 0, 1 = cyan/red/black/yellow)
     EQUB WHITE             \ Thargoid
     EQUB WHITE             \ Thargon
     EQUB CYAN              \ Constrictor
     EQUB CYAN              \ Cougar
    
     EQUB CYAN              \ This byte appears to be unused
    

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Configuration variable [RED](../../all/workspaces.md#red) = %11110000

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Configuration variable [WHITE](../../all/workspaces.md#white) = %11111010

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[X]

Configuration variable [YELLOW](../../all/workspaces.md#yellow) = %00001111

Four mode 1 pixels of colour 1 (yellow)

[NA2%](na2_per_cent.md "Previous routine")[scacol](scacol.md "Next routine")
