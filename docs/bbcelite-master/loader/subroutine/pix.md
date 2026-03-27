---
title: "The PIX (Loader) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/subroutine/pix.html"
---

[SQUA2 (Loader)](squa2.md "Previous routine")[TWOS (Loader)](../variable/twos.md "Next routine")
    
    
           Name: PIX                                                     [Show more]
           Type: Subroutine
       Category: Drawing pixels
        Summary: Draw a single pixel at a specific coordinate
    
    
        Context: See this subroutine [in context in the source code](../../all/loader.md#header-pix)
     Variations: See [code variations](../../related/compare/loader/subroutine/pix.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLL1 (Part 1 of 3)](pll1_part_1_of_3.md) calls PIX
                 * [PLL1 (Part 2 of 3)](pll1_part_2_of_3.md) calls PIX
                 * [PLL1 (Part 3 of 3)](pll1_part_3_of_3.md) calls PIX
    
    
    
    
    
    * * *
    
    
     Draw a pixel at screen coordinate (X + 128, A + 128). The routine uses the
     same approach as the PIXEL routine in the main game code, except it plots a
     single pixel from TWOS instead of a two pixel dash from TWOS2. This applies
     to the top part of the screen (the four-colour mode 1 space view).
    
     See the PIXEL routine in the main game code for more details.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The signed screen x-coordinate of the pixel to draw,
                            from -128 to 127, to be plotted relative to the origin
                            at (128, 128)
    
       A                    The signed screen y-coordinate of the pixel to draw,
                            from -128 to 127, to be plotted relative to the origin
                            at (128, 128)
    
    
    
    
    .PIX
    
     TAY                    \ Copy A into Y, for use later
    
     EOR #%10000000         \ Add 128 to A and treat this as an unsigned number from
                            \ now on
    
     LSR A                  \ Set ZP+1 = &40 + 2 * (A >> 3)
     LSR A
     LSR A
     ASL A
     ORA #&40
     STA ZP+1
    
     TXA                    \ Set (C ZP) = (X >> 2) * 8
     EOR #%10000000         \
     AND #%11111100         \ i.e. the C flag contains bit 8 of the calculation
     ASL A
     STA ZP
    
     BCC P%+4               \ If the C flag is set, i.e. bit 8 of the above
     INC ZP+1               \ calculation was a 1, increment ZP+1 so that ZP(1 0)
                            \ points to the second page in this character row (i.e.
                            \ the right half of the row)
    
     TYA                    \ Set Y = Y mod 8, which is the pixel row within the
     AND #7                 \ character block at which we want to draw our pixel
     TAY                    \ (as each character block has 8 rows)
    
     TXA                    \ Set X = X mod 8, which is the horizontal pixel number
     AND #7                 \ within the character block where the pixel lies (as
     TAX                    \ each pixel line in the character block is 8 pixels
                            \ wide)
    
     LDA TWOS,X             \ Fetch a pixel from TWOS and poke it into ZP+Y
     STA (ZP),Y
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [TWOS](../variable/twos.md) (category: Drawing pixels)

Ready-made single-pixel character row bytes for mode 1

[X]

Workspace [ZP](../workspace/zp.md) (category: Workspaces)

Important variables used by the loader

[SQUA2 (Loader)](squa2.md "Previous routine")[TWOS (Loader)](../variable/twos.md "Next routine")
