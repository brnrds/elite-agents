---
title: "The PLL1 (Part 3 of 3) (Loader) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/subroutine/pll1_part_3_of_3.html"
---

[PLL1 (Part 2 of 3) (Loader)](pll1_part_2_of_3.md "Previous routine")[DORND (Loader)](dornd.md "Next routine")
    
    
           Name: PLL1 (Part 3 of 3)                                      [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw Saturn on the loading screen (draw the rings)
      Deep dive: [Drawing Saturn on the loading screen](https://elite.bbcelite.com/deep_dives/drawing_saturn_on_the_loading_screen.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/loader.md#header-pll1-part-3-of-3)
     Variations: See [code variations](../../related/compare/loader/subroutine/pll1_part_3_of_3.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
                            \ The following loop iterates CNT3(1 0) times, i.e. &333
                            \ or 819 times, and draws the rings around the loading
                            \ screen's Saturn
    
    .PLL3
    
     JSR DORND              \ Set A and X to signed random numbers between -128 and
                            \ 127, so let's say A = r5
    
     STA ZP                 \ Set ZP = r5
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = r5^2
    
     STA ZP+1               \ Set ZP+1 = A
                            \          = r5^2 / 256
    
     JSR DORND              \ Set A and X to signed random numbers between -128 and
                            \ 127, so let's say A = r6
    
     STA YY                 \ Set YY = r6
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = r6^2
    
     STA T                  \ Set T = A
                            \       = r6^2 / 256
    
     ADC ZP+1               \ Set ZP+1 = A + r5^2 / 256
     STA ZP+1               \          = r6^2 / 256 + r5^2 / 256
                            \          = (r5^2 + r6^2) / 256
    
     LDA ZP                 \ Set A = ZP
                            \       = r5
    
     CMP #128               \ If A >= 128, set the C flag (so the C flag is now set
                            \ to bit 7 of ZP, i.e. bit 7 of A)
    
     ROR A                  \ Rotate A and set the sign bit to the C flag, so bits
                            \ 6 and 7 are now the same
    
     CMP #128               \ If A >= 128, set the C flag (so again, the C flag is
                            \ set to bit 7 of A)
    
     ROR A                  \ Rotate A and set the sign bit to the C flag, so bits
                            \ 5-7 are now the same, i.e. A is a random number in one
                            \ of these ranges:
                            \
                            \   %00000000 - %00011111  = 0-31
                            \   %11100000 - %11111111  = 224-255
                            \
                            \ In terms of signed 8-bit integers, this is a random
                            \ number from -32 to 31. Let's call it r7
    
     ADC YY                 \ Set A = A + YY
                            \       = r7 + r6
    
     TAX                    \ Set X = A
                            \       = r6 + r7
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = (r6 + r7)^2
    
     TAY                    \ Set Y = A
                            \       = (r6 + r7)^2 / 256
    
     ADC ZP+1               \ Set A = A + ZP+1
                            \       = (r6 + r7)^2 / 256 + (r5^2 + r6^2) / 256
                            \       = ((r6 + r7)^2 + r5^2 + r6^2) / 256
    
     BCS PLC3               \ If the addition overflowed, jump down to PLC3 to skip
                            \ to the next pixel
    
     CMP #80                \ If A >= 80, jump down to PLC3 to skip to the next
     BCS PLC3               \ pixel
    
     CMP #32                \ If A < 32, jump down to PLC3 to skip to the next pixel
     BCC PLC3
    
     TYA                    \ Set A = Y + T
     ADC T                  \       = (r6 + r7)^2 / 256 + r6^2 / 256
                            \       = ((r6 + r7)^2 + r6^2) / 256
    
     CMP #16                \ If A >= 16, skip to PL1 to plot the pixel
     BCS PL1
    
     LDA ZP                 \ If ZP is positive (i.e. r5 < 128), jump down to PLC3
     BPL PLC3               \ to skip to the next pixel
    
    .PL1
    
                            \ If we get here then the following is true:
                            \
                            \   32 <= ((r6 + r7)^2 + r5^2 + r6^2) / 256 < 80
                            \
                            \ and either this is true:
                            \
                            \   ((r6 + r7)^2 + r6^2) / 256 >= 16
                            \
                            \ or both these are true:
                            \
                            \   ((r6 + r7)^2 + r6^2) / 256 < 16
                            \   r5 >= 128
    
     LDA YY                 \ Set A = YY
                            \       = r6
    
     JSR PIX                \ Draw a pixel at screen coordinate (X + 128, A + 128),
                            \ where:
                            \
                            \   X = (random -32 to 31) + r6
                            \   A = r6
                            \
                            \ So this is the same as plotting at (x, y) where:
                            \
                            \   r5 = random number from -128 to 127
                            \   r6 = random number from -128 to 127
                            \   r7 = r5, squashed into -32 to 31
                            \
                            \   32 <= ((r6 + r7)^2 + r5^2 + r6^2) / 256 < 80
                            \
                            \   Either: ((r6 + r7)^2 + r6^2) / 256 >= 16
                            \
                            \   Or:     ((r6 + r7)^2 + r6^2) / 256 <  16
                            \           r5 >= 128
                            \
                            \   x = r6 + r7 + 128
                            \   y = r6 + 128
                            \
                            \ which is what we want
    
    .PLC3
    
     DEC CNT3               \ Decrement the counter in CNT3 (the low byte)
    
     BNE PLL3               \ Loop back to PLL3 until CNT3 = 0
    
     DEC CNT3+1             \ Decrement the counter in CNT3+1 (the high byte)
    
     BNE PLL3               \ Loop back to PLL3 until CNT3+1 = 0
    

[X]

Variable [CNT3](../variable/cnt3.md) (category: Drawing planets)

A counter for use in drawing Saturn's rings

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [PIX](pix.md) (category: Drawing pixels)

Draw a single pixel at a specific coordinate

[X]

Label [PL1](pll1_part_3_of_3.md#pl1) is local to this routine

[X]

Label [PLC3](pll1_part_3_of_3.md#plc3) is local to this routine

[X]

Label [PLL3](pll1_part_3_of_3.md#pll3) is local to this routine

[X]

Subroutine [SQUA2](squa2.md) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [YY](../workspace/zp.md#yy) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Workspace [ZP](../workspace/zp.md) (category: Workspaces)

Important variables used by the loader

[PLL1 (Part 2 of 3) (Loader)](pll1_part_2_of_3.md "Previous routine")[DORND (Loader)](dornd.md "Next routine")
