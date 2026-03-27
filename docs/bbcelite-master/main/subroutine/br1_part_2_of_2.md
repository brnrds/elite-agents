---
title: "The BR1 (Part 2 of 2) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/br1_part_2_of_2.html"
---

[BR1 (Part 1 of 2)](br1_part_1_of_2.md "Previous routine")[BAY](bay.md "Next routine")
    
    
           Name: BR1 (Part 2 of 2)                                       [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Show the "Press Fire or Space, Commander" screen and start the
                 game
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-br1-part-2-of-2)
     Variations: See [code variations](../../related/compare/main/subroutine/br1_part_2_of_2.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     BRKV is set to point to BR1 by the loading process.
    
    
    
    
     JSR msblob             \ Reset the dashboard's missile indicators so none of
                            \ them are targeted
    
     LDA #7                 \ Call TITLE to show a rotating Cougar (#COU) and token
     LDX #COU               \ 7 ("PRESS SPACE OR FIRE,{single cap}COMMANDER.{cr}
     LDY #100               \ {cr}"), with the ship at a distance of 100, returning
     JSR TITLE              \ with the internal number of the key pressed in A
    
     JSR ping               \ Set the target system coordinates (QQ9, QQ10) to the
                            \ current system coordinates (QQ0, QQ1) we just loaded
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     JSR jmp                \ Set the current system to the selected system
    
     LDX #5                 \ We now want to copy the seeds for the selected system
                            \ in QQ15 into QQ2, where we store the seeds for the
                            \ current system, so set up a counter in X for copying
                            \ 6 bytes (for three 16-bit seeds)
    
                            \ The label below is called likeTT112 because this code
                            \ is almost identical to the TT112 loop in the hyp1
                            \ routine
    
    .likeTT112
    
     LDA QQ15,X             \ Copy the X-th byte in QQ15 to the X-th byte in QQ2
     STA QQ2,X
    
     DEX                    \ Decrement the counter
    
     BPL likeTT112          \ Loop back to likeTT112 if we still have more bytes to
                            \ copy
    
     INX                    \ Set X = 0 (as we ended the above loop with X = &FF)
    
     STX EV                 \ Set EV, the extra vessels spawning counter, to 0, as
                            \ we are entering a new system with no extra vessels
                            \ spawned
    
     LDA QQ3                \ Set the current system's economy in QQ28 to the
     STA QQ28               \ selected system's economy from QQ3
    
     LDA QQ5                \ Set the current system's tech level in tek to the
     STA tek                \ selected system's economy from QQ5
    
     LDA QQ4                \ Set the current system's government in gov to the
     STA gov                \ selected system's government from QQ4
    
                            \ Fall through into the docking bay routine below
    

[X]

Configuration variable [COU](../../all/workspaces.md#cou) = 32

Ship type for a Cougar

[X]

Variable [EV](../workspace/wp.md#ev) in workspace [WP](../workspace/wp.md)

The "extra vessels" spawning counter

[X]

Variable [QQ15](../workspace/zp.md#qq15) in workspace [ZP](../workspace/zp.md)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ2](../workspace/wp.md#qq2) in workspace [WP](../workspace/wp.md)

The three 16-bit seeds for the current system, i.e. the one we are currently in

[X]

Variable [QQ28](../workspace/wp.md#qq28) in workspace [WP](../workspace/wp.md)

The current system's economy (0-7)

[X]

Variable [QQ3](../workspace/zp.md#qq3) in workspace [ZP](../workspace/zp.md)

The selected system's economy (0-7)

[X]

Variable [QQ4](../workspace/zp.md#qq4) in workspace [ZP](../workspace/zp.md)

The selected system's government (0-7)

[X]

Variable [QQ5](../workspace/zp.md#qq5) in workspace [ZP](../workspace/zp.md)

The selected system's tech level (0-14)

[X]

Subroutine [TITLE](title.md) (category: Start and end)

Display a title screen with a rotating ship and prompt

[X]

Subroutine [TT111](tt111.md) (category: Universe)

Set the current system to the nearest system to a point

[X]

Variable [gov](../workspace/wp.md#gov) in workspace [WP](../workspace/wp.md)

The current system's government type (0-7)

[X]

Subroutine [jmp](jmp.md) (category: Universe)

Set the current system to the selected system

[X]

Label [likeTT112](br1_part_2_of_2.md#likett112) is local to this routine

[X]

Subroutine [msblob](msblob.md) (category: Dashboard)

Display the dashboard's missile indicators in green

[X]

Subroutine [ping](ping.md) (category: Universe)

Set the selected system to the current system

[X]

Variable [tek](../workspace/wp.md#tek) in workspace [WP](../workspace/wp.md)

The current system's tech level (0-14)

[BR1 (Part 1 of 2)](br1_part_1_of_2.md "Previous routine")[BAY](bay.md "Next routine")
