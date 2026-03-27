---
title: "The LOIN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/loin.html"
---

[SCAN](scan.md "Previous routine")[TWOS](../variable/twos.md "Next routine")
    
    
           Name: LOIN                                                    [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a one-segment line
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-loin)
     Variations: See [code variations](../../related/compare/main/subroutine/ll30-loin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BLINE](bline.md) calls LOIN
                 * [BOMBOFF](bomboff.md) calls LOIN
                 * [DVLOIN](dvloin.md) calls LOIN
                 * [LASLI](lasli.md) calls LOIN
                 * [LL9 (Part 12 of 12)](ll9_part_12_of_12.md) calls LOIN
                 * [LSPUT](lsput.md) calls LOIN
                 * [TT15](tt15.md) calls LOIN
                 * [WPLS2](wpls2.md) calls LOIN
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       X1                   The screen x-coordinate of the start of the line
    
       Y1                   The screen y-coordinate of the start of the line
    
       X2                   The screen x-coordinate of the end of the line
    
       Y2                   The screen y-coordinate of the end of the line
    
    
    
    
    .LOIN
    
     STY YSAV               \ Store Y in YSAV so we can retrieve it below
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     JSR LOINQ              \ Draw a line from (X1, Y1) to (X2, Y2)
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDY YSAV               \ Retrieve the value of Y we stored above
    
     RTS                    \ Return from the subroutine
    

[X]

Entry point [LOINQ](loinq_part_1_of_7.md#loinq) in subroutine [LOINQ (Part 1 of 7)](loinq_part_1_of_7.md) (category: Drawing lines)

Draw a one-segment line from (X1, Y1) to (X2, Y2)

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [YSAV](../workspace/zp.md#ysav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the Y register, used in a number of places

[SCAN](scan.md "Previous routine")[TWOS](../variable/twos.md "Next routine")
