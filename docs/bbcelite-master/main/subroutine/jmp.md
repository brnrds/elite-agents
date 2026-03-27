---
title: "The jmp subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/jmp.html"
---

[Ghy](ghy.md "Previous routine")[ee3](ee3.md "Next routine")
    
    
           Name: jmp                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set the current system to the selected system
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-jmp)
     References: This subroutine is called as follows:
                 * [BR1 (Part 2 of 2)](br1_part_2_of_2.md) calls jmp
                 * [hyp1](hyp1.md) calls jmp
    
    
    
    
    
    * * *
    
    
     Returns:
    
       (QQ0, QQ1)           The galactic coordinates of the new system
    
    
    
    * * *
    
    
     Other entry points:
    
       hy5                  Contains an RTS
    
    
    
    
    .jmp
    
     LDA QQ9                \ Set the current system's galactic x-coordinate to the
     STA QQ0                \ x-coordinate of the selected system
    
     LDA QQ10               \ Set the current system's galactic y-coordinate to the
     STA QQ1                \ y-coordinate of the selected system
    
    .hy5
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [QQ0](../workspace/wp.md#qq0) in workspace [WP](../workspace/wp.md)

The current system's galactic x-coordinate (0-256)

[X]

Variable [QQ1](../workspace/wp.md#qq1) in workspace [WP](../workspace/wp.md)

The current system's galactic y-coordinate (0-256)

[X]

Variable [QQ10](../workspace/zp.md#qq10) in workspace [ZP](../workspace/zp.md)

The galactic y-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic y-coordinate)

[X]

Variable [QQ9](../workspace/zp.md#qq9) in workspace [ZP](../workspace/zp.md)

The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)

[Ghy](ghy.md "Previous routine")[ee3](ee3.md "Next routine")
