---
title: "The MVEIT (Part 3 of 9) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mveit_part_3_of_9.html"
---

[MVEIT (Part 2 of 9)](mveit_part_2_of_9.md "Previous routine")[MVEIT (Part 4 of 9)](mveit_part_4_of_9.md "Next routine")
    
    
           Name: MVEIT (Part 3 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Move ship forward according to its speed
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-mveit-part-3-of-9)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * Move the ship forward (along the vector pointing in the direction of
         travel) according to its speed:
    
         (x, y, z) += nosev_hi * speed / 64
    
    
    
    
     LDA INWK+27            \ Set Q = the ship's speed byte #27 * 4
     ASL A
     ASL A
     STA Q
    
     LDA INWK+10            \ Set A = |nosev_x_hi|
     AND #%01111111
    
     JSR FMLTU              \ Set R = A * Q / 256
     STA R                  \       = |nosev_x_hi| * speed / 64
    
     LDA INWK+10            \ If nosev_x_hi is positive, then:
     LDX #0                 \
     JSR MVT1-2             \   (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + R
                            \
                            \ If nosev_x_hi is negative, then:
                            \
                            \   (x_sign x_hi x_lo) = (x_sign x_hi x_lo) - R
                            \
                            \ So in effect, this does:
                            \
                            \   (x_sign x_hi x_lo) += nosev_x_hi * speed / 64
    
     LDA INWK+12            \ Set A = |nosev_y_hi|
     AND #%01111111
    
     JSR FMLTU              \ Set R = A * Q / 256
     STA R                  \       = |nosev_y_hi| * speed / 64
    
     LDA INWK+12            \ If nosev_y_hi is positive, then:
     LDX #3                 \
     JSR MVT1-2             \   (y_sign y_hi y_lo) = (y_sign y_hi y_lo) + R
                            \
                            \ If nosev_y_hi is negative, then:
                            \
                            \   (y_sign y_hi y_lo) = (y_sign y_hi y_lo) - R
                            \
                            \ So in effect, this does:
                            \
                            \   (y_sign y_hi y_lo) += nosev_y_hi * speed / 64
    
     LDA INWK+14            \ Set A = |nosev_z_hi|
     AND #%01111111
    
     JSR FMLTU              \ Set R = A * Q / 256
     STA R                  \       = |nosev_z_hi| * speed / 64
    
     LDA INWK+14            \ If nosev_y_hi is positive, then:
     LDX #6                 \
     JSR MVT1-2             \   (z_sign z_hi z_lo) = (z_sign z_hi z_lo) + R
                            \
                            \ If nosev_z_hi is negative, then:
                            \
                            \   (z_sign z_hi z_lo) = (z_sign z_hi z_lo) - R
                            \
                            \ So in effect, this does:
                            \
                            \   (z_sign z_hi z_lo) += nosev_z_hi * speed / 64
    

[X]

Subroutine [FMLTU](fmltu.md) (category: Maths (Arithmetic))

Calculate A = A * Q / 256

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Entry point [MVT1-2](mvt1.md#mvt1) in subroutine [MVT1](mvt1.md) (category: Moving)

Clear bits 0-6 of A before entering MVT1

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MVEIT (Part 2 of 9)](mveit_part_2_of_9.md "Previous routine")[MVEIT (Part 4 of 9)](mveit_part_4_of_9.md "Next routine")
