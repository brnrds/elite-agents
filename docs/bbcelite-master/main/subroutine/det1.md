---
title: "The DET1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/det1.html"
---

[FLFLLS](flflls.md "Previous routine")[SHD](shd.md "Next routine")
    
    
           Name: DET1                                                    [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Show or hide the dashboard (for when we die)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-det1)
     Variations: See [code variations](../../related/compare/main/subroutine/det1-dodials.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](death.md) calls DET1
    
    
    
    
    
    * * *
    
    
     This routine sets the screen to show the number of text rows given in X.
    
     It is used when we are killed, as reducing the number of rows from the usual
     31 to 24 has the effect of hiding the dashboard, leaving a monochrome image
     of ship debris and explosion clouds. Increasing the rows back up to 31 makes
     the dashboard reappear, as the dashboard's screen memory doesn't get touched
     by this process.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The number of text rows to display on the screen (24
                            will hide the dashboard, 31 will make it reappear)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is set to 6
    
    
    
    
    .DET1
    
     LDA #6                 \ Set A to 6 so we can update 6845 register R6 below
    
     SEI                    \ Disable interrupts so we can update the 6845
    
     STA VIA+&00            \ Set 6845 register R6 to the value in X. Register R6
     STX VIA+&01            \ is the "vertical displayed" register, which sets the
                            \ number of rows shown on the screen
    
     CLI                    \ Re-enable interrupts
    
     RTS                    \ Return from the subroutine
    

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[FLFLLS](flflls.md "Previous routine")[SHD](shd.md "Next routine")
