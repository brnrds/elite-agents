---
title: "The PLS5 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pls5.html"
---

[PLS4](pls4.md "Previous routine")[PLS6](pls6.md "Next routine")
    
    
           Name: PLS5                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Calculate roofv_x / z and roofv_y / z
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-pls5)
     References: This subroutine is called as follows:
                 * [PL9 (Part 2 of 3)](pl9_part_2_of_3.md) calls PLS5
    
    
    
    
    
    * * *
    
    
     Calculate the following divisions of a specified value from one of the
     orientation vectors (in this example, roofv):
    
       (XX16+2 K2+2) = roofv_x / z
    
       (XX16+3 K2+3) = roofv_y / z
    
    
    
    * * *
    
    
     Arguments:
    
       X                    Determines which of the INWK orientation vectors to
                            divide:
    
                              * X = 15: divides roofv_x and roofv_y
    
                              * X = 21: divides sidev_x and sidev_y
    
       INWK                 The planet's ship data block
    
    
    
    
    .PLS5
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA K2+2               \
     STY XX16+2             \   K+2    = |roofv_x / z|
                            \   XX16+2 = sign of roofv_x / z
                            \
                            \ i.e. (XX16+2 K2+2) = roofv_x / z
                            \
                            \ and increment X to point to roofv_y for the next call
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA K2+3               \
     STY XX16+3             \   K+3    = |roofv_y / z|
                            \   XX16+3 = sign of roofv_y / z
                            \
                            \ i.e. (XX16+3 K2+3) = roofv_y / z
                            \
                            \ and increment X to point to roofv_z for the next call
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [K2](../workspace/zp.md#k2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PLS1](pls1.md) (category: Drawing planets)

Calculate (Y A) = nosev_x / z

[X]

Variable [XX16](../workspace/zp.md#xx16) in workspace [ZP](../workspace/zp.md)

Temporary storage for a block of values, used in a number of places

[PLS4](pls4.md "Previous routine")[PLS6](pls6.md "Next routine")
