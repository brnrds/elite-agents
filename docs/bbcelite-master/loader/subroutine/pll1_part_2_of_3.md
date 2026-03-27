---
title: "The PLL1 (Part 2 of 3) (Loader) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/subroutine/pll1_part_2_of_3.html"
---

[PLL1 (Part 1 of 3) (Loader)](pll1_part_1_of_3.md "Previous routine")[PLL1 (Part 3 of 3) (Loader)](pll1_part_3_of_3.md "Next routine")
    
    
           Name: PLL1 (Part 2 of 3)                                      [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw Saturn on the loading screen (draw the stars)
      Deep dive: [Drawing Saturn on the loading screen](https://elite.bbcelite.com/deep_dives/drawing_saturn_on_the_loading_screen.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/loader.md#header-pll1-part-2-of-3)
     Variations: See [code variations](../../related/compare/loader/subroutine/pll1_part_2_of_3.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
                            \ The following loop iterates CNT2(1 0) times, i.e. &1DD
                            \ or 477 times, and draws the background stars on the
                            \ loading screen
    
    .PLL2
    
     JSR DORND              \ Set A and X to signed random numbers between -128 and
                            \ 127, so let's say A = r3
    
     TAX                    \ Set X = A
                            \       = r3
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = r3^2
    
     STA ZP+1               \ Set ZP+1 = A
                            \          = r3^2 / 256
    
     JSR DORND              \ Set A and X to signed random numbers between -128 and
                            \ 127, so let's say A = r4
    
     STA YY                 \ Set YY = r4
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = r4^2
    
     ADC ZP+1               \ Set A = A + r3^2 / 256
                            \       = r4^2 / 256 + r3^2 / 256
                            \       = (r3^2 + r4^2) / 256
    
     CMP #&11               \ If A < 17, jump down to PLC2 to skip to the next pixel
     BCC PLC2
    
     LDA YY                 \ Set A = r4
    
     JSR PIX                \ Draw a pixel at screen coordinate (X + 128, A + 128),
                            \ where:
                            \
                            \   X = r3
                            \   A = r4
                            \
                            \ So this is the same as plotting at (x, y) where:
                            \
                            \   r3 = random number from -128 to 127
                            \   r4 = random number from -128 to 127
                            \
                            \   (r3^2 + r4^2) / 256 >= 17
                            \
                            \   x = r3
                            \   y = r4
                            \
                            \ which is what we want
    
    .PLC2
    
     DEC CNT2               \ Decrement the counter in CNT2 (the low byte)
    
     BNE PLL2               \ Loop back to PLL2 until CNT2 = 0
    
     DEC CNT2+1             \ Decrement the counter in CNT2+1 (the high byte)
    
     BNE PLL2               \ Loop back to PLL2 until CNT2+1 = 0
    

[X]

Variable [CNT2](../variable/cnt2.md) (category: Drawing planets)

A counter for use in drawing Saturn's background stars

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [PIX](pix.md) (category: Drawing pixels)

Draw a single pixel at a specific coordinate

[X]

Label [PLC2](pll1_part_2_of_3.md#plc2) is local to this routine

[X]

Label [PLL2](pll1_part_2_of_3.md#pll2) is local to this routine

[X]

Subroutine [SQUA2](squa2.md) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[X]

Variable [YY](../workspace/zp.md#yy) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Workspace [ZP](../workspace/zp.md) (category: Workspaces)

Important variables used by the loader

[PLL1 (Part 1 of 3) (Loader)](pll1_part_1_of_3.md "Previous routine")[PLL1 (Part 3 of 3) (Loader)](pll1_part_3_of_3.md "Next routine")
