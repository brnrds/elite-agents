---
title: "The ECBLB subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ecblb.html"
---

[ECBLB2](ecblb2.md "Previous routine")[SPBLB](spblb.md "Next routine")
    
    
           Name: ECBLB                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Light up the E.C.M. indicator bulb ("E") on the dashboard
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-ecblb)
     Variations: See [code variations](../../related/compare/main/subroutine/ecblb.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ECMOF](ecmof.md) calls ECBLB
    
    
    
    
    
    
    .ECBLB
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA #8*14              \ The E.C.M. bulb is in character block number 14 with
     STA SC                 \ each character taking 8 bytes, so this sets the low
                            \ byte of the screen address of the character block we
                            \ want to draw to
    
     LDA #&7A               \ Set the high byte of SC(1 0) to &7A, as the bulbs are
     STA SC+1               \ both in the character row from &7A00 to &7BFF, and the
                            \ E.C.M. bulb is in the left half, which is from &7A00
                            \ to &7AFF
    
     LDY #15                \ Now to poke the bulb bitmap into screen memory, and
                            \ there are two character blocks' worth, each with eight
                            \ lines of one byte, so set a counter in Y for 16 bytes
    
    .BULL1
    
     LDA ECBT,Y             \ Fetch the Y-th byte of the bulb bitmap
    
     EOR (SC),Y             \ EOR the byte with the current contents of screen
                            \ memory, so drawing the bulb when it is already
                            \ on-screen will erase it
    
     STA (SC),Y             \ Store the Y-th byte of the bulb bitmap in screen
                            \ memory
    
     DEY                    \ Decrement the loop counter
    
     BPL BULL1              \ Loop back to poke the next byte until we have done
                            \ all 16 bytes across two character blocks
    
     BMI away               \ Jump to away to switch main memory back into
                            \ &3000-&7FFF and return from the subroutine (this BMI
                            \ is effectively a JMP as we just passed through the BPL
                            \ above)
    

[X]

Label [BULL1](ecblb.md#bull1) is local to this routine

[X]

Variable [ECBT](../variable/ecbt.md) (category: Dashboard)

The character bitmap for the E.C.M. indicator bulb

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Entry point [away](spblb.md#away) in subroutine [SPBLB](spblb.md) (category: Dashboard)

Switch main memory back into &3000-&7FFF and return from the subroutine

[ECBLB2](ecblb2.md "Previous routine")[SPBLB](spblb.md "Next routine")
