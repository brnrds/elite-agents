---
title: "The scacol variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/scacol.html"
---

[shpcol](shpcol.md "Previous routine")[UNIV](univ.md "Next routine")
    
    
           Name: scacol                                                  [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Ship colours on the scanner
      Deep dive: [The elusive Cougar](https://elite.bbcelite.com/deep_dives/the_elusive_cougar.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-scacol)
     Variations: See [code variations](../../related/compare/main/variable/scacol.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [SCAN](../subroutine/scan.md) uses scacol
    
    
    
    
    
    
    .scacol
    
     EQUB 0                 \ This byte appears to be unused
    
     EQUB YELLOW2           \ Missile
     EQUB GREEN2            \ Coriolis space station
     EQUB BLUE2             \ Escape pod
     EQUB BLUE2             \ Alloy plate
     EQUB BLUE2             \ Cargo canister
     EQUB RED2              \ Boulder
     EQUB RED2              \ Asteroid
     EQUB RED2              \ Splinter
     EQUB CYAN2             \ Shuttle
     EQUB CYAN2             \ Transporter
     EQUB CYAN2             \ Cobra Mk III
     EQUB MAG2              \ Python
     EQUB MAG2              \ Boa
     EQUB MAG2              \ Anaconda
     EQUB RED2              \ Rock hermit (asteroid)
     EQUB CYAN2             \ Viper
     EQUB CYAN2             \ Sidewinder
     EQUB CYAN2             \ Mamba
     EQUB CYAN2             \ Krait
     EQUB CYAN2             \ Adder
     EQUB CYAN2             \ Gecko
     EQUB CYAN2             \ Cobra Mk I
     EQUB BLUE2             \ Worm
     EQUB CYAN2             \ Cobra Mk III (pirate)
     EQUB CYAN2             \ Asp Mk II
     EQUB MAG2              \ Python (pirate)
     EQUB CYAN2             \ Fer-de-lance
     EQUB CYAN2             \ Moray
     EQUB WHITE2            \ Thargoid
     EQUB CYAN2             \ Thargon
     EQUB CYAN2             \ Constrictor
     EQUB 0                 \ Cougar
    
     EQUB CYAN2             \ These bytes appear to be unused
     EQUD 0
    

[X]

Configuration variable [BLUE2](../../all/workspaces.md#blue2) = %00110000

Two mode 2 pixels of colour 4 (blue)

[X]

Configuration variable [CYAN2](../../all/workspaces.md#cyan2) = %00111100

Two mode 2 pixels of colour 6 (cyan)

[X]

Configuration variable [GREEN2](../../all/workspaces.md#green2) = %00001100

Two mode 2 pixels of colour 2 (green)

[X]

Configuration variable [MAG2](../../all/workspaces.md#mag2) = %00110011

Two mode 2 pixels of colour 5 (magenta)

[X]

Configuration variable [RED2](../../all/workspaces.md#red2) = %00000011

Two mode 2 pixels of colour 1 (red)

[X]

Configuration variable [WHITE2](../../all/workspaces.md#white2) = %00111111

Two mode 2 pixels of colour 7 (white)

[X]

Configuration variable [YELLOW2](../../all/workspaces.md#yellow2) = %00001111

Two mode 2 pixels of colour 3 (yellow)

[shpcol](shpcol.md "Previous routine")[UNIV](univ.md "Next routine")
