---
title: "The PL9 (Part 2 of 3) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pl9_part_2_of_3.html"
---

[PL9 (Part 1 of 3)](pl9_part_1_of_3.md "Previous routine")[PL9 (Part 3 of 3)](pl9_part_3_of_3.md "Next routine")
    
    
           Name: PL9 (Part 2 of 3)                                       [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw the planet's equator and meridian
      Deep dive: [Drawing meridians and equators](https://elite.bbcelite.com/deep_dives/drawing_meridians_and_equators.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-pl9-part-2-of-3)
     Variations: See [code variations](../../related/compare/main/subroutine/pl9_part_2_of_3.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Draw the planet's equator and meridian.
    
    
    
    * * *
    
    
     Arguments:
    
       K(1 0)               The planet's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the planet
    
       K4(1 0)              Pixel y-coordinate of the centre of the planet
    
       INWK                 The planet's ship data block
    
    
    
    
     LDA K                  \ If the planet's radius is less than 6, the planet is
     CMP #6                 \ too small to show a meridian, so jump to PL20 to
     BCC PL20               \ return from the subroutine
    
     LDA INWK+14            \ Set P = -nosev_z_hi
     EOR #%10000000
     STA P
    
     LDA INWK+20            \ Set A = roofv_z_hi
    
     JSR PLS4               \ Call PLS4 to calculate the following:
                            \
                            \   CNT2 = arctan(P / A) / 4
                            \        = arctan(-nosev_z_hi / roofv_z_hi) / 4
                            \
                            \ and do the following if nosev_z_hi >= 0:
                            \
                            \   CNT2 = CNT2 + PI
    
     LDX #9                 \ Set X to 9 so the call to PLS1 divides nosev_x
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA K2                 \
     STY XX16               \   (XX16 K2) = nosev_x / z
                            \
                            \ and increment X to point to nosev_y for the next call
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA K2+1               \
     STY XX16+1             \   (XX16+1 K2+1) = nosev_y / z
    
     LDX #15                \ Set X to 15 so the call to PLS5 divides roofv_x
    
     JSR PLS5               \ Call PLS5 to calculate the following:
                            \
                            \   (XX16+2 K2+2) = roofv_x / z
                            \
                            \   (XX16+3 K2+3) = roofv_y / z
    
     JSR PLS2               \ Call PLS2 to draw the first meridian
    
     LDA INWK+14            \ Set P = -nosev_z_hi
     EOR #%10000000
     STA P
    
     LDA INWK+26            \ Set A = sidev_z_hi, so the second meridian will be at
                            \ 90 degrees to the first
    
     JSR PLS4               \ Call PLS4 to calculate the following:
                            \
                            \   CNT2 = arctan(P / A) / 4
                            \        = arctan(-nosev_z_hi / sidev_z_hi) / 4
                            \
                            \ and do the following if nosev_z_hi >= 0:
                            \
                            \   CNT2 = CNT2 + PI
    
     LDX #21                \ Set X to 21 so the call to PLS5 divides sidev_x
    
     JSR PLS5               \ Call PLS5 to calculate the following:
                            \
                            \   (XX16+2 K2+2) = sidev_x / z
                            \
                            \   (XX16+3 K2+3) = sidev_y / z
    
     JMP PLS2               \ Jump to PLS2 to draw the second meridian, returning
                            \ from the subroutine using a tail call
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [K2](../workspace/zp.md#k2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [PL20](pl9_part_1_of_3.md#pl20) in subroutine [PL9 (Part 1 of 3)](pl9_part_1_of_3.md)

[X]

Subroutine [PLS1](pls1.md) (category: Drawing planets)

Calculate (Y A) = nosev_x / z

[X]

Subroutine [PLS2](pls2.md) (category: Drawing planets)

Draw a half-ellipse

[X]

Subroutine [PLS4](pls4.md) (category: Drawing planets)

Calculate CNT2 = arctan(P / A) / 4

[X]

Subroutine [PLS5](pls5.md) (category: Drawing planets)

Calculate roofv_x / z and roofv_y / z

[X]

Variable [XX16](../workspace/zp.md#xx16) in workspace [ZP](../workspace/zp.md)

Temporary storage for a block of values, used in a number of places

[PL9 (Part 1 of 3)](pl9_part_1_of_3.md "Previous routine")[PL9 (Part 3 of 3)](pl9_part_3_of_3.md "Next routine")
