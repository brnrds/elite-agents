---
title: "The DKS5 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dks5.html"
---

[CTRL](ctrl.md "Previous routine")[ZEKTRAN](zektran.md "Next routine")
    
    
           Name: DKS5                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard to see if a specific key is being pressed
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-dks5)
     Variations: See [code variations](../../related/compare/main/subroutine/dks4-dks5.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT17](tt17.md) calls DKS5
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The internal number of the key to check (see page 142 of
                            the "Advanced User Guide for the BBC Micro" by Bray,
                            Dickens and Holmes for a list of internal key numbers)
    
    
    
    * * *
    
    
     Returns:
    
       A                    If the key in A is being pressed, A contains the
                            original argument A, but with bit 7 set (i.e. A + 128).
                            If the key in A is not being pressed, the value in A is
                            unchanged
    
       X                    Contains the same as A
    
    
    
    
    IF _SNG47
    
    .DKS5
    
     LDX #3                 \ Set X to 3, so it's ready to send to SHEILA once
                            \ interrupts have been disabled
    
     SEI                    \ Disable interrupts so we can scan the keyboard
                            \ without being hijacked
    
     STX VIA+&40            \ Set 6522 System VIA output register ORB (SHEILA &40)
                            \ to %00000011 to stop auto scan of keyboard
    
     LDX #%01111111         \ Set 6522 System VIA data direction register DDRA
     STX VIA+&43            \ (SHEILA &43) to %01111111. This sets the A registers
                            \ (IRA and ORA) so that:
                            \
                            \   * Bits 0-6 of ORA will be sent to the keyboard
                            \
                            \   * Bit 7 of IRA will be read from the keyboard
    
     STA VIA+&4F            \ Set 6522 System VIA output register ORA (SHEILA &4F)
                            \ to A, the key we want to scan for; bits 0-6 will be
                            \ sent to the keyboard, of which bits 0-3 determine the
                            \ keyboard column, and bits 4-6 the keyboard row
    
     LDX VIA+&4F            \ Read 6522 System VIA output register IRA (SHEILA &4F)
                            \ into X; bit 7 is the only bit that will have changed.
                            \ If the key is pressed, then bit 7 will be set,
                            \ otherwise it will be clear
    
     LDA #%00001011         \ Set 6522 System VIA output register ORB (SHEILA &40)
     STA VIA+&40            \ to %00001011 to restart auto scan of keyboard
    
     CLI                    \ Allow interrupts again
    
     TXA                    \ Transfer X into A
    
     RTS                    \ Return from the subroutine
    
    ENDIF
    

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[CTRL](ctrl.md "Previous routine")[ZEKTRAN](zektran.md "Next routine")
