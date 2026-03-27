---
title: "The getzp subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/getzp.html"
---

[NMIRELEASE](nmirelease.md "Previous routine")[NMICLAIM](nmiclaim.md "Next routine")
    
    
           Name: getzp                                                   [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Swap zero page (&0090 to &00EF) with the buffer at &3000
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-getzp)
     References: This subroutine is called as follows:
                 * [CATS](cats.md) calls getzp
                 * [DELT](delt.md) calls getzp
                 * [GTDIR](gtdir.md) calls getzp
                 * [NEWBRK](newbrk.md) calls getzp
                 * [rfile](rfile.md) calls getzp
                 * [wfile](wfile.md) calls getzp
                 * [NMIRELEASE](nmirelease.md) calls via getzp+3
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       getzp+3              Restore the top part of zero page, but without first
                            claiming the NMI workspace (Master Compact variant only)
    
    
    
    
    .getzp
    
    IF _COMPACT
    
     JSR NMICLAIM           \ Claim the NMI workspace (&00A0 to &00A7) from the MOS
                            \ so the game can use it
    
    ENDIF
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDX #&90               \ We want to swap zero page from &0090 and up, so set an
                            \ index in X, starting from &90
    
    .sz2
    
     LDA ZP,X               \ Swap the X-th byte of ZP with the X-th byte of &3000
     LDY &3000,X
     STY ZP,X
     STA &3000,X
    
     INX                    \ Increment the loop counter
    
     CPX #&F0               \ Loop back until we have swapped up to location &00EF
     BNE sz2
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDA #6                 \ Set bits 0-3 of the ROM Select latch at SHEILA &30 to
     STA VIA+&30            \ 6, to switch sideways ROM bank 6 into &8000-&BFFF in
                            \ main memory (we already confirmed that this bank
                            \ contains RAM rather than ROM in the loader)
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [NMICLAIM](nmiclaim.md) (category: Utility routines)

Claim the NMI workspace (&00A0 to &00A7) back from the MOS so the game can use it once again

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Workspace [ZP](../workspace/zp.md) (category: Workspaces)

Lots of important variables are stored in the zero page workspace as it is quicker and more space-efficient to access memory here

[X]

Label [sz2](getzp.md#sz2) is local to this routine

[NMIRELEASE](nmirelease.md "Previous routine")[NMICLAIM](nmiclaim.md "Next routine")
