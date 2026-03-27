---
title: "The SESCP subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sescp.html"
---

[FR1](fr1.md "Previous routine")[SFS1](sfs1.md "Next routine")
    
    
           Name: SESCP                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Spawn an escape pod from the current (parent) ship
      Deep dive: [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-sescp)
     References: This subroutine is called as follows:
                 * [TACTICS (Part 4 of 7)](tactics_part_4_of_7.md) calls SESCP
    
    
    
    
    
    * * *
    
    
     This is called when an enemy ship has run out of both energy and luck, so it's
     time to bail.
    
    
    
    
    .SESCP
    
     LDX #ESC               \ Set X to the ship type for an escape pod
    
     LDA #%11111110         \ Set A to use as an AI flag that has AI enabled, an
                            \ aggression level of 63 out of 63, and no E.C.M.
                            \
                            \ When spawning an escape pod, this high agression level
                            \ makes the pod turn towards the planet rather than
                            \ towards us
                            \
                            \ This instruction is also used as an entry point to
                            \ spawn missile (when calling via the SFS1-2 entry
                            \ point), in which case the missile has AI (bit 7 set),
                            \ is hostile (bit 6 set) and has been launched (bit 0
                            \ clear); the target slot number is set to 31, but this
                            \ is ignored as the hostile flag means we are the target
    
                            \ Fall through into SFS1 to spawn the escape pod or
                            \ missile
    

[X]

Configuration variable [ESC](../../all/workspaces.md#esc) = 3

Ship type for an escape pod

[FR1](fr1.md "Previous routine")[SFS1](sfs1.md "Next routine")
