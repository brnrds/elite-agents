---
title: "The SOUS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sous1.html"
---

[SOOFF](../variable/sooff.md "Previous routine")[SOFLUSH](soflush.md "Next routine")
    
    
           Name: SOUS1                                                   [Show more]
           Type: Subroutine
       Category: Sound
        Summary: Write sound data directly to the 76489 sound chip
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-sous1)
     References: This subroutine is called as follows:
                 * [SOFLUSH](soflush.md) calls SOUS1
                 * [SOINT](soint.md) calls SOUS1
                 * [NOISE](noise.md) calls via SOUR1
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The sound byte to send to the 76489 sound chip
    
    
    
    * * *
    
    
     Other entry points:
    
       SOUR1                Contains an RTS
    
    
    
    
    .SOUS1
    
     LDX #%11111111         \ Set 6522 System VIA data direction register DDRA
     STX VIA+&43            \ (SHEILA &43) to %11111111. This sets the ORA register
                            \ so that bits 0-7 of ORA will be sent to the 76489
                            \ sound chip
    
     STA VIA+&4F            \ Set 6522 System VIA output register ORA (SHEILA &4F)
                            \ to A, the sound data we want to send
    
     LDA #%00000000         \ Activate the sound chip by clearing bit 3 of the
     STA VIA+&40            \ 6522 System VIA output register ORB (SHEILA &40)
    
     PHA                    \ These instructions don't do anything apart from
     PLA                    \ keeping the sound chip activated for at least 8us,
     PHA                    \ which we need to do in order for the data to make
     PLA                    \ it to the chip
    
     LDA #%00001000         \ Deactivate the sound chip by setting bit 3 of the
     STA VIA+&40            \ 6522 System VIA output register ORB (SHEILA &40)
    
    .SOUR1
    
     RTS                    \ Return from the subroutine
    

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[SOOFF](../variable/sooff.md "Previous routine")[SOFLUSH](soflush.md "Next routine")
