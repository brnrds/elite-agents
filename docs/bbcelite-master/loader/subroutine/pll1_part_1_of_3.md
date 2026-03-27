---
title: "The PLL1 (Part 1 of 3) (Loader) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/subroutine/pll1_part_1_of_3.html"
---

[Elite loader (Loader)](elite_loader.md "Previous routine")[PLL1 (Part 2 of 3) (Loader)](pll1_part_2_of_3.md "Next routine")
    
    
           Name: PLL1 (Part 1 of 3)                                      [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw Saturn on the loading screen (draw the planet)
      Deep dive: [Drawing Saturn on the loading screen](https://elite.bbcelite.com/deep_dives/drawing_saturn_on_the_loading_screen.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/loader.md#header-pll1-part-1-of-3)
     Variations: See [code variations](../../related/compare/loader/subroutine/pll1_part_1_of_3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Elite loader](elite_loader.md) calls PLL1
    
    
    
    
    
    
    .PLL1
    
                            \ The following loop iterates CNT(1 0) times, i.e. &300
                            \ or 768 times, and draws the planet part of the
                            \ loading screen's Saturn
    
     STA RAND+1             \ Store A in RAND+1 among the hard-coded random seeds
                            \ in RAND. We set A to %00001111 before calling the PLL1
                            \ routine, so this sets the random number generator so
                            \ that it always generates the same numbers every time,
                            \ which is probably not what was intended (other
                            \ versions read the 6522 System VIA timer to use as a
                            \ seed, which is random). As a result, if you look at
                            \ the Saturn on the Master loading screen, it is always
                            \ exactly the same, every time you run the game
    
     JSR DORND              \ Set A and X to signed random numbers between -128 and
                            \ 127, so let's say A = r1
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = r1^2
    
     STA ZP+1               \ Set ZP(1 0) = (A P)
     LDA P                  \             = r1^2
     STA ZP
    
     JSR DORND              \ Set A and X to signed random numbers between -128 and
                            \ 127, so let's say A = r2
    
     STA YY                 \ Set YY = A
                            \        = r2
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = r2^2
    
     TAX                    \ Set (X P) = (A P)
                            \           = r2^2
    
     LDA P                  \ Set (A ZP) = (X P) + ZP(1 0)
     ADC ZP                 \
     STA ZP                 \ first adding the low bytes
    
     TXA                    \ And then adding the high bytes
     ADC ZP+1
    
     BCS PLC1               \ If the addition overflowed, jump down to PLC1 to skip
                            \ to the next pixel
    
     STA ZP+1               \ Set ZP(1 0) = (A ZP)
                            \             = r1^2 + r2^2
    
     LDA #1                 \ Set ZP(1 0) = &4001 - ZP(1 0) - (1 - C)
     SBC ZP                 \             = 128^2 - ZP(1 0)
     STA ZP                 \
                            \ (as the C flag is clear), first subtracting the low
                            \ bytes
    
     LDA #&40               \ And then subtracting the high bytes
     SBC ZP+1
     STA ZP+1
    
     BCC PLC1               \ If the subtraction underflowed, jump down to PLC1 to
                            \ skip to the next pixel
    
                            \ If we get here, then both calculations fitted into
                            \ 16 bits, and we have:
                            \
                            \   ZP(1 0) = 128^2 - (r1^2 + r2^2)
                            \
                            \ where ZP(1 0) >= 0
    
     JSR ROOT               \ Set ZP = SQRT(ZP(1 0))
    
     LDA ZP                 \ Set X = ZP >> 1
     LSR A                  \       = SQRT(128^2 - (a^2 + b^2)) / 2
     TAX
    
     LDA YY                 \ Set A = YY
                            \       = r2
    
     CMP #128               \ If YY >= 128, set the C flag (so the C flag is now set
                            \ to bit 7 of A)
    
     ROR A                  \ Rotate A and set the sign bit to the C flag, so A is
                            \ halved while retaining its sign
                            \
                            \ A is still a signed number from -128 to 127
    
     JSR PIX                \ Draw a pixel at screen coordinate (X + 128, A + 128),
                            \ where:
                            \
                            \   X = ZP / 2
                            \   A = r2 / 2
                            \   ZP = SQRT(128^2 - (r1^2 + r2^2))
                            \
                            \ So this is the same as plotting at (x, y) where:
                            \
                            \   r1 = random number from -128 to 127
                            \   r2 = random number from -128 to 127
                            \
                            \   (r1^2 + r2^2) < 128^2
                            \
                            \   x = (SQRT(128^2 - (r1^2 + r2^2)) / 2) + 128
                            \   y = (r2 / 2) + 128
                            \
                            \ which is what we want
    
    .PLC1
    
     DEC CNT                \ Decrement the counter in CNT (the low byte)
    
     BNE PLL1               \ Loop back to PLL1 until CNT = 0
    
     DEC CNT+1              \ Decrement the counter in CNT+1 (the high byte)
    
     BNE PLL1               \ Loop back to PLL1 until CNT+1 = 0
    

[X]

Variable [CNT](../variable/cnt.md) (category: Drawing planets)

A counter for use in drawing Saturn's planetary body

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PIX](pix.md) (category: Drawing pixels)

Draw a single pixel at a specific coordinate

[X]

Label [PLC1](pll1_part_1_of_3.md#plc1) is local to this routine

[X]

Subroutine [PLL1 (Part 1 of 3)](pll1_part_1_of_3.md) (category: Drawing planets)

Draw Saturn on the loading screen (draw the planet)

[X]

Variable [RAND](../variable/rand.md) (category: Drawing planets)

The random number seed used for drawing Saturn

[X]

Subroutine [ROOT](root.md) (category: Maths (Arithmetic))

Calculate ZP = SQRT(ZP(1 0))

[X]

Subroutine [SQUA2](squa2.md) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[X]

Variable [YY](../workspace/zp.md#yy) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Workspace [ZP](../workspace/zp.md) (category: Workspaces)

Important variables used by the loader

[Elite loader (Loader)](elite_loader.md "Previous routine")[PLL1 (Part 2 of 3) (Loader)](pll1_part_2_of_3.md "Next routine")
