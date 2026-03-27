---
title: "The SPBLB subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/spblb.html"
---

[ECBLB](ecblb.md "Previous routine")[SPBT](../variable/spbt.md "Next routine")
    
    
           Name: SPBLB                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Light up the space station indicator ("S") on the dashboard
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-spblb)
     Variations: See [code variations](../../related/compare/main/subroutine/spblb-dobulb.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KS4](ks4.md) calls SPBLB
                 * [NWSPS](nwsps.md) calls SPBLB
                 * [RES2](res2.md) calls SPBLB
                 * [ECBLB](ecblb.md) calls via away
                 * [HANGER](hanger.md) calls via away
                 * [MSBAR](msbar.md) calls via away
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       away                 Switch main memory back into &3000-&7FFF and return from
                            the subroutine
    
    
    
    
    .SPBLB
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA #16*8              \ The space station bulb is in character block number 48
     STA SC                 \ (counting from the left edge of the screen), with the
                            \ first half of the row in one page, and the second half
                            \ in another. We want to set the screen address to point
                            \ to the second part of the row, as the bulb is in that
                            \ half, so that's character block number 16 within that
                            \ second half (as the first half takes up 32 character
                            \ blocks, so given that each character block takes up 8
                            \ bytes, this sets the low byte of the screen address
                            \ of the character block we want to draw to
    
     LDA #&7B               \ Set the high byte of SC(1 0) to &7B, as the bulbs are
     STA SC+1               \ both in the character row from &7A00 to &7BFF, and the
                            \ space station bulb is in the right half, which is from
                            \ &7B00 to &7BFF
    
     LDY #15                \ Now to poke the bulb bitmap into screen memory, and
                            \ there are two character blocks' worth, each with eight
                            \ lines of one byte, so set a counter in Y for 16 bytes
    
    .BULL2
    
     LDA SPBT,Y             \ Fetch the Y-th byte of the bulb bitmap
    
     EOR (SC),Y             \ EOR the byte with the current contents of screen
                            \ memory, so drawing the bulb when it is already
                            \ on-screen will erase it
    
     STA (SC),Y             \ Store the Y-th byte of the bulb bitmap in screen
                            \ memory
    
     DEY                    \ Decrement the loop counter
    
     BPL BULL2              \ Loop back to poke the next byte until we have done
                            \ all 16 bytes across two character blocks
    
    .away
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    

[X]

Label [BULL2](spblb.md#bull2) is local to this routine

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [SPBT](../variable/spbt.md) (category: Dashboard)

The bitmap definition for the space station indicator bulb

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[ECBLB](ecblb.md "Previous routine")[SPBT](../variable/spbt.md "Next routine")
