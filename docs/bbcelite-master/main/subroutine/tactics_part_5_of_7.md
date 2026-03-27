---
title: "The TACTICS (Part 5 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tactics_part_5_of_7.html"
---

[TACTICS (Part 4 of 7)](tactics_part_4_of_7.md "Previous routine")[TACTICS (Part 6 of 7)](tactics_part_6_of_7.md "Next routine")
    
    
           Name: TACTICS (Part 5 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Consider whether to launch a missile at us
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tactics-part-5-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/tactics_part_5_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section considers whether to launch a missile. Specifically:
    
       * If the ship doesn't have any missiles, skip to the next part
    
       * If an E.C.M. is firing, skip to the next part
    
       * Randomly decide whether to fire a missile (or, in the case of Thargoids,
         release a Thargon), and if we do, we're done
    
    
    
    
    .ta3
    
                            \ If we get here then the ship has less than half energy
                            \ so there may not be enough juice for lasers, but let's
                            \ see if we can fire a missile
    
     LDA INWK+31            \ Set A = bits 0-2 of byte #31, the number of missiles
     AND #%00000111         \ the ship has left
    
     BEQ TA3                \ If it doesn't have any missiles, jump to TA3
    
     STA T                  \ Store the number of missiles in T
    
     JSR DORND              \ Set A and X to random numbers
    
     AND #31                \ Restrict A to a random number in the range 0-31
    
     CMP T                  \ If A >= T, which is quite likely, though less likely
     BCS TA3                \ with higher numbers of missiles, jump to TA3 to skip
                            \ firing a missile
    
     LDA ECMA               \ If an E.C.M. is currently active (either ours or an
     BNE TA3                \ opponent's), jump to TA3 to skip firing a missile
    
     DEC INWK+31            \ We're done with the checks, so it's time to fire off a
                            \ missile, so reduce the missile count in byte #31 by 1
    
     LDA TYPE               \ Fetch the ship type into A
    
     CMP #THG               \ If this is not a Thargoid, jump down to TA16 to launch
     BNE TA16               \ a missile
    
     LDX #TGL               \ This is a Thargoid, so instead of launching a missile,
     LDA INWK+32            \ the mothership launches a Thargon, so call SFS1 to
     JMP SFS1               \ spawn a Thargon from the parent ship, and return from
                            \ the subroutine using a tail call
    
    .TA16
    
     JMP SFRMIS             \ Jump to SFRMIS to spawn a missile as a child of the
                            \ current ship, make a noise and print a message warning
                            \ of incoming missiles, and return from the subroutine
                            \ using a tail call
    

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [ECMA](../workspace/zp.md#ecma) in workspace [ZP](../workspace/zp.md)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [SFRMIS](sfrmis.md) (category: Tactics)

Add an enemy missile to our local bubble of universe

[X]

Subroutine [SFS1](sfs1.md) (category: Universe)

Spawn a child ship from the current (parent) ship

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [TA16](tactics_part_5_of_7.md#ta16) is local to this routine

[X]

Label [TA3](tactics_part_6_of_7.md#ta3) in subroutine [TACTICS (Part 6 of 7)](tactics_part_6_of_7.md)

[X]

Configuration variable [TGL](../../all/workspaces.md#tgl) = 30

Ship type for a Thargon

[X]

Configuration variable [THG](../../all/workspaces.md#thg) = 29

Ship type for a Thargoid

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[TACTICS (Part 4 of 7)](tactics_part_4_of_7.md "Previous routine")[TACTICS (Part 6 of 7)](tactics_part_6_of_7.md "Next routine")
