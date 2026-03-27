---
title: "The RDFIRE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/rdfire.html"
---

[RDKEY](rdkey.md "Previous routine")[RDJOY](rdjoy.md "Next routine")
    
    
           Name: RDFIRE                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Read the fire button on either the analogue or digital joystick
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-rdfire)
     References: This subroutine is called as follows:
                 * [TITLE](title.md) calls RDFIRE
    
    
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the joystick fire button is being pressed, clear
                            if it isn't
    
    
    
    
    IF _COMPACT
    
    .RDFIRE
    
     LDA MOS                \ If MOS = 0 then this is a Master Compact, so jump to
     BEQ DFIRE              \ DFIRE to read the digital joystick rather than the
                            \ analogue joystick, as the Compact doesn't have the
                            \ latter
    
     CLC                    \ Clear the C flag
    
     LDA VIA+&40            \ Read 6522 System VIA input register IRB (SHEILA &40)
    
     AND #%00010000         \ Bit 4 of IRB (PB4) is clear if joystick 1's fire
                            \ button is pressed, otherwise it is set, so AND'ing
                            \ the value of IRB with %10000 extracts this bit
    
     BNE P%+3               \ If the joystick fire button is not being pressed,
                            \ jump skip the following to leave the C flag clear
    
     SEC                    \ Set the C flag to indicate the joystick fire button
                            \ is being pressed
    
     RTS                    \ Return from the subroutine
    
    .DFIRE
    
     LDA VIA+&60            \ Read the 6522 User VIA, which is where the Master
                            \ Compact's digital joystick is mapped to. The pins go
                            \ low when the joystick connection is made, and PB0 is
                            \ connected to the joystick fire button, so when PB0
                            \ is low, fire is being pressed
    
     EOR #%00000001         \ Flip PB0 so that it's 1 if fire is being pressed and
                            \ 0 if it isn't
    
     LSR A                  \ Shift PB0 into the C flag, so it's set if the fire
                            \ button is being pressed
    
     RTS                    \ Return from the subroutine
    
    ENDIF
    

[X]

Label [DFIRE](rdfire.md#dfire) is local to this routine

[X]

Variable [MOS](../workspace/zp.md#mos) in workspace [ZP](../workspace/zp.md)

Determines whether we are running on a Master Compact

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[RDKEY](rdkey.md "Previous routine")[RDJOY](rdjoy.md "Next routine")
