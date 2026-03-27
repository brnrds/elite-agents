---
title: "The TACTICS (Part 4 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tactics_part_4_of_7.html"
---

[TACTICS (Part 3 of 7)](tactics_part_3_of_7.md "Previous routine")[TACTICS (Part 5 of 7)](tactics_part_5_of_7.md "Next routine")
    
    
           Name: TACTICS (Part 4 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Check energy levels, maybe launch escape pod if low
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tactics-part-4-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/tactics_part_4_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section works out what kind of condition the ship is in. Specifically:
    
       * If this is an Anaconda, consider spawning (22% chance) a Worm (61% of the
         time) or a Sidewinder (39% of the time)
    
       * Rarely (2.5% chance) roll the ship by a noticeable amount
    
       * If the ship has at least half its energy banks full, jump to part 6 to
         consider firing the lasers
    
       * If the ship is not into the last 1/8th of its energy, jump to part 5 to
         consider firing a missile
    
       * If the ship is into the last 1/8th of its energy, and this ship type has
         an escape pod fitted, then rarely (10% chance) the ship launches an escape
         pod and is left drifting in space
    
    
    
    
     LDA TYPE               \ If this is not a missile, skip the following
     CMP #MSL               \ instruction
     BNE P%+5
    
     JMP TA20               \ This is a missile, so jump down to TA20 to get
                            \ straight into some aggressive manoeuvring
    
     CMP #ANA               \ If this is not an Anaconda, jump down to TN7 to skip
     BNE TN7                \ the following
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #200               \ If A < 200 (78% chance), jump down to TN7 to skip the
     BCC TN7                \ following
    
     JSR DORND              \ Set A and X to random numbers
    
     LDX #WRM               \ Set X to the ship type for a Worm
    
     CMP #100               \ If A >= 100 (61% chance), skip the following
     BCS P%+4               \ instruction
    
     LDX #SH3               \ Set X to the ship type for a Sidewinder
    
     JMP TN6                \ Jump to TN6 to spawn the Worm or Sidewinder and return
                            \ from the subroutine using a tail call
    
    .TN7
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #250               \ If A < 250 (97.5% chance), jump down to TA7 to skip
     BCC TA7                \ the following
    
     JSR DORND              \ Set A and X to random numbers
    
     ORA #104               \ Bump A up to at least 104 and store in the roll
     STA INWK+29            \ counter, to gives the ship a noticeable roll
    
    .TA7
    
     LDY #14                \ Set A = the ship's maximum energy / 2
     LDA (XX0),Y
     LSR A
    
     CMP INWK+35            \ If the ship's current energy in byte #35 > A, i.e. the
     BCC TA3                \ ship has at least half of its energy banks charged,
                            \ jump down to TA3
    
     LSR A                  \ If the ship's current energy in byte #35 > A / 4, i.e.
     LSR A                  \ the ship is not into the last 1/8th of its energy,
     CMP INWK+35            \ jump down to ta3 to consider firing a missile
     BCC ta3
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #230               \ If A < 230 (90% chance), jump down to ta3 to consider
     BCC ta3                \ firing a missile
    
     LDX TYPE               \ Fetch the ship blueprint's default NEWB flags from the
     LDA E%-1,X             \ table at E%, and if bit 7 is clear (i.e. this ship
     BPL ta3                \ does not have an escape pod), jump to ta3 to skip the
                            \ spawning of an escape pod
    
                            \ By this point, the ship has run out of both energy and
                            \ luck, so it's time to bail
    
     LDA NEWB               \ Clear bits 0-3 of the NEWB flags, so the ship is no
     AND #%11110000         \ longer a trader, a bounty hunter, hostile or a pirate
     STA NEWB               \ and the escape pod we are about to spawn won't inherit
                            \ any of these traits
    
     LDY #36                \ Update the NEWB flags in the ship's data block
     STA (INF),Y
    
     LDA #%00000000         \ Set the AI flag to 0 to disable AI, set aggression to
     STA INWK+32            \ zero and disable any E.C.M., so the ship's a sitting
                            \ duck
    
     JMP SESCP              \ Jump to SESCP to spawn an escape pod from the ship,
                            \ returning from the subroutine using a tail call
    

[X]

Configuration variable [ANA](../../all/workspaces.md#ana) = 14

Ship type for an Anaconda

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Configuration variable [E%](../../all/workspaces.md#e-per-cent) = &8042

The address of the default NEWB ship bytes, as set in elite-data.asm

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Configuration variable [MSL](../../all/workspaces.md#msl) = 1

Ship type for a missile

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Subroutine [SESCP](sescp.md) (category: Flight)

Spawn an escape pod from the current (parent) ship

[X]

Configuration variable [SH3](../../all/workspaces.md#sh3) = 17

Ship type for a Sidewinder

[X]

Label [TA20](tactics_part_7_of_7.md#ta20) in subroutine [TACTICS (Part 7 of 7)](tactics_part_7_of_7.md)

[X]

Label [TA3](tactics_part_6_of_7.md#ta3) in subroutine [TACTICS (Part 6 of 7)](tactics_part_6_of_7.md)

[X]

Label [TA7](tactics_part_4_of_7.md#ta7) is local to this routine

[X]

Label [TN6](tactics_part_2_of_7.md#tn6) in subroutine [TACTICS (Part 2 of 7)](tactics_part_2_of_7.md)

[X]

Label [TN7](tactics_part_4_of_7.md#tn7) is local to this routine

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Configuration variable [WRM](../../all/workspaces.md#wrm) = 23

Ship type for a Worm

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Label [ta3](tactics_part_5_of_7.md#ta3) in subroutine [TACTICS (Part 5 of 7)](tactics_part_5_of_7.md)

[TACTICS (Part 3 of 7)](tactics_part_3_of_7.md "Previous routine")[TACTICS (Part 5 of 7)](tactics_part_5_of_7.md "Next routine")
