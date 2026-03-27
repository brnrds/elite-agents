---
title: "The MSBAR subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/msbar.html"
---

[ECBT](../variable/ecbt.md "Previous routine")[HANGFLAG](../variable/hangflag.md "Next routine")
    
    
           Name: MSBAR                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Draw a specific indicator in the dashboard's missile bar
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-msbar)
     Variations: See [code variations](../../related/compare/main/subroutine/msbar.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [ABORT2](abort2.md) calls MSBAR
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls MSBAR
                 * [msblob](msblob.md) calls MSBAR
    
    
    
    
    
    * * *
    
    
     Each indicator is a rectangle that's 3 pixels wide and 5 pixels high. If the
     indicator is set to black, this effectively removes a missile.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The number of the missile indicator to update (counting
                            from right to left, so indicator NOMSL is the leftmost
                            indicator)
    
       Y                    The colour of the missile indicator:
    
                              * &00 = black (no missile)
    
                              * #RED2 = red (armed and locked)
    
                              * #YELLOW2 = yellow/white (armed)
    
                              * #GREEN2 = green (disarmed)
    
    
    
    * * *
    
    
     Returns:
    
       X                    X is preserved
    
       Y                    Y is set to 0
    
    
    
    
    .MSBAR
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     TXA                    \ Store the value of X on the stack so we can preserve
     PHA                    \ it across the call to this subroutine
    
     ASL A                  \ Set T = A * 16
     ASL A
     ASL A
     ASL A
     STA T
    
     LDA #97                \ Set SC = 97 - T
     SBC T                  \        = 96 + 1 - (X * 16)
     STA SC
    
                            \ So the low byte of SC(1 0) contains the row address
                            \ for the rightmost missile indicator, made up as
                            \ follows:
                            \
                            \   * 96 (character block 14, as byte #14 * 8 = 96), the
                            \     character block of the rightmost missile
                            \
                            \   * 1 (so we start drawing on the second row of the
                            \     character block)
                            \
                            \   * Move left one character (8 bytes) for each count
                            \     of X, so when X = 0 we are drawing the rightmost
                            \     missile, for X = 1 we hop to the left by one
                            \     character, and so on
    
     LDA #&7C               \ Set the high byte of SC(1 0) to &7C, the character row
     STA SCH                \ that contains the missile indicators (i.e. the bottom
                            \ row of the screen)
    
     TYA                    \ Set A to the correct colour, which is a two-pixel wide
                            \ mode 2 character row byte in the specified colour
    
     LDY #5                 \ We now want to draw this line five times to do the
                            \ left two pixels of the indicator, so set a counter in
                            \ Y
    
    .MBL1
    
     STA (SC),Y             \ Draw the three-pixel row, and as we do not use EOR
                            \ logic, this will overwrite anything that is already
                            \ there (so drawing a black missile will delete what's
                            \ there)
    
     DEY                    \ Decrement the counter for the next row
    
     BNE MBL1               \ Loop back to MBL1 if have more rows to draw
    
     PHA                    \ Store the value of A on the stack so we can retrieve
                            \ it after the following addition
    
     LDA SC                 \ Set SC = SC + 8
     CLC                    \
     ADC #8                 \ so SC(1 0) now points to the next character block on
     STA SC                 \ the row (for the right half of the indicator)
    
     PLA                    \ Retrieve A from the stack
    
     AND #%10101010         \ Mask the character row to plot just the first pixel
                            \ in the next character block, as we already did a
                            \ two-pixel wide band in the previous character block,
                            \ so we need to plot just one more pixel, width-wise
    
     LDY #5                 \ We now want to draw this line five times, so set a
                            \ counter in Y
    
    .MBL2
    
     STA (SC),Y             \ Draw the one-pixel row, and as we do not use EOR
                            \ logic, this will overwrite anything that is already
                            \ there (so drawing a black missile will delete what's
                            \ there)
    
     DEY                    \ Decrement the counter for the next row
    
     BNE MBL2               \ Loop back to MBL2 if have more rows to draw
    
     PLX                    \ Restore X from the stack, so that it's preserved
    
    IF _SNG47
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     RTS                    \ Return from the subroutine
    
    ELIF _COMPACT
    
     JMP away               \ Jump to away to switch main memory back into
                            \ &3000-&7FFF and return from the subroutine
    
    ENDIF
    

[X]

Label [MBL1](msbar.md#mbl1) is local to this routine

[X]

Label [MBL2](msbar.md#mbl2) is local to this routine

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [SCH](../workspace/zp.md#sch) in workspace [ZP](../workspace/zp.md)

Screen address (high byte)

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Entry point [away](spblb.md#away) in subroutine [SPBLB](spblb.md) (category: Dashboard)

Switch main memory back into &3000-&7FFF and return from the subroutine

[ECBT](../variable/ecbt.md "Previous routine")[HANGFLAG](../variable/hangflag.md "Next routine")
