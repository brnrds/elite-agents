---
title: "The ping subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ping.html"
---

[GINF](ginf.md "Previous routine")[MTIN](../variable/mtin.md "Next routine")
    
    
           Name: ping                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set the selected system to the current system
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-ping)
     References: This subroutine is called as follows:
                 * [BR1 (Part 2 of 2)](br1_part_2_of_2.md) calls ping
                 * [TT102](tt102.md) calls ping
    
    
    
    
    
    
    .ping
    
     LDX #1                 \ We want to copy the X- and Y-coordinates of the
                            \ current system in (QQ0, QQ1) to the selected system's
                            \ coordinates in (QQ9, QQ10), so set up a counter to
                            \ copy two bytes
    
    .pl1
    
     LDA QQ0,X              \ Load byte X from the current system in QQ0/QQ1
    
     STA QQ9,X              \ Store byte X in the selected system in QQ9/QQ10
    
     DEX                    \ Decrement the loop counter
    
     BPL pl1                \ Loop back for the next byte to copy
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [QQ0](../workspace/wp.md#qq0) in workspace [WP](../workspace/wp.md)

The current system's galactic x-coordinate (0-256)

[X]

Variable [QQ9](../workspace/zp.md#qq9) in workspace [ZP](../workspace/zp.md)

The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)

[X]

Label [pl1](ping.md#pl1) is local to this routine

[GINF](ginf.md "Previous routine")[MTIN](../variable/mtin.md "Next routine")
