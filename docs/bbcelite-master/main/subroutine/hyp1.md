---
title: "The hyp1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hyp1.html"
---

[var](var.md "Previous routine")[GVL](gvl.md "Next routine")
    
    
           Name: hyp1                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Process a jump to the system closest to (QQ9, QQ10)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-hyp1)
     Variations: See [code variations](../../related/compare/main/subroutine/hyp1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT18](tt18.md) calls via hyp1+3
    
    
    
    
    
    * * *
    
    
     Do a hyperspace jump to the system closest to galactic coordinates
     (QQ9, QQ10), and set up the current system's state to those of the new system.
    
    
    
    * * *
    
    
     Returns:
    
       (QQ0, QQ1)           The galactic coordinates of the new system
    
       QQ2 to QQ2+6         The seeds of the new system
    
       EV                   Set to 0
    
       QQ28                 The new system's economy
    
       tek                  The new system's tech level
    
       gov                  The new system's government
    
    
    
    * * *
    
    
     Other entry points:
    
       hyp1+3               Jump straight to the system at (QQ9, QQ10) without
                            first calculating which system is closest. We do this
                            if we already know that (QQ9, QQ10) points to a system
    
    
    
    
    .hyp1
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     JSR jmp                \ Set the current system to the selected system
    
     LDX #5                 \ We now want to copy the seeds for the selected system
                            \ in QQ15 into QQ2, where we store the seeds for the
                            \ current system, so set up a counter in X for copying
                            \ 6 bytes (for three 16-bit seeds)
    
    .TT112
    
     LDA safehouse,X        \ Copy the X-th byte in safehouse to the X-th byte in
     STA QQ2,X              \ QQ2
    
     DEX                    \ Decrement the counter
    
     BPL TT112              \ Loop back to TT112 if we still have more bytes to
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
    
                            \ Fall through into GVL to calculate the availability of
                            \ market items in the new system
    

[X]

Variable [EV](../workspace/wp.md#ev) in workspace [WP](../workspace/wp.md)

The "extra vessels" spawning counter

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

Subroutine [TT111](tt111.md) (category: Universe)

Set the current system to the nearest system to a point

[X]

Label [TT112](hyp1.md#tt112) is local to this routine

[X]

Variable [gov](../workspace/wp.md#gov) in workspace [WP](../workspace/wp.md)

The current system's government type (0-7)

[X]

Subroutine [jmp](jmp.md) (category: Universe)

Set the current system to the selected system

[X]

Variable [safehouse](../workspace/wp.md#safehouse) in workspace [WP](../workspace/wp.md)

Backup storage for the seeds for the selected system

[X]

Variable [tek](../workspace/wp.md#tek) in workspace [WP](../workspace/wp.md)

The current system's tech level (0-14)

[var](var.md "Previous routine")[GVL](gvl.md "Next routine")
