---
title: "The SETINTS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/setints.html"
---

[SFXVC](../variable/sfxvc.md "Previous routine")[TVT1](../variable/tvt1.md "Next routine")
    
    
           Name: SETINTS                                                 [Show more]
           Type: Subroutine
       Category: Loader
        Summary: Set the various vectors, interrupts and timers
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-setints)
     Variations: See [code variations](../../related/compare/main/subroutine/startup-setints.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [COLD](cold.md) calls SETINTS
    
    
    
    
    
    
    .SETINTS
    
     SEI                    \ Disable interrupts
    
     LDA #%00111001         \ Set 6522 System VIA interrupt enable register IER
     STA VIA+&4E            \ (SHEILA &4E) bits 0 and 3-5 (i.e. disable the Timer1,
                            \ CB1, CB2 and CA2 interrupts from the System VIA)
    
     LDA #%01111111         \ Set 6522 User VIA interrupt enable register IER
     STA &FE6E              \ (SHEILA &6E) bits 0-7 (i.e. disable all hardware
                            \ interrupts from the User VIA)
    
     LDA IRQ1V              \ Store the current IRQ1V vector in VEC, so VEC(1 0) now
     STA VEC                \ contains the original address of the IRQ1 handler
     LDA IRQ1V+1
     STA VEC+1
    
     LDA #LO(IRQ1)          \ Set the IRQ1V vector to IRQ1, so IRQ1 is now the
     STA IRQ1V              \ interrupt handler
     LDA #HI(IRQ1)
     STA IRQ1V+1
    
     LDA VSCAN              \ Set 6522 System VIA T1C-L timer 1 high-order counter
     STA VIA+&45            \ (SHEILA &45) to the contents of VSCAN (57) to start
                            \ the T1 counter counting down from 14592 at a rate of
                            \ 1 MHz
    
     CLI                    \ Enable interrupts again
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [IRQ1](irq1.md) (category: Drawing the screen)

The main screen-mode interrupt handler (IRQ1V points here)

[X]

Configuration variable [IRQ1V](../../all/workspaces.md#irq1v) = &0204

The IRQ1V vector that we intercept to implement the split-screen mode

[X]

Variable [VEC](../variable/vec.md) (category: Drawing the screen)

The original value of the IRQ1 vector

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [VSCAN](../variable/vscan.md) (category: Drawing the screen)

Defines the split position in the split-screen mode

[SFXVC](../variable/sfxvc.md "Previous routine")[TVT1](../variable/tvt1.md "Next routine")
