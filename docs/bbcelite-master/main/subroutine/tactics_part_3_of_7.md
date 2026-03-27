---
title: "The TACTICS (Part 3 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tactics_part_3_of_7.html"
---

[TACTICS (Part 2 of 7)](tactics_part_2_of_7.md "Previous routine")[TACTICS (Part 4 of 7)](tactics_part_4_of_7.md "Next routine")
    
    
           Name: TACTICS (Part 3 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Calculate dot product to determine ship's aim
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tactics-part-3-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/tactics_part_3_of_7.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOCKIT](dockit.md) calls via GOPL
    
    
    
    
    
    * * *
    
    
     This section sets up some vectors and calculates dot products. Specifically:
    
       * If this is a lone Thargon without a mothership, set it adrift aimlessly
         and we're done
    
       * If this is a trader, 80% of the time we're done, 20% of the time the
         trader performs the same checks as the bounty hunter
    
       * If this is a bounty hunter (or one of the 20% of traders) and we have been
         really bad (i.e. a fugitive or serious offender), the ship becomes hostile
         (if it isn't already)
    
       * If the ship is not hostile, then either perform docking manoeuvres (if
         it's docking) or fly towards the planet (if it isn't docking) and we're
         done
    
       * If the ship is hostile, and a pirate, and we are within the space station
         safe zone, stop the pirate from attacking by removing all its aggression
    
       * Calculate the dot product of the ship's nose vector (i.e. the direction it
         is pointing) with the vector between us and the ship. This value will help
         us work out later on whether the enemy ship is pointing towards us, and
         therefore whether it can hit us with its lasers.
    
    
    
    * * *
    
    
     Other entry points:
    
       GOPL                 Make the ship head towards the planet
    
    
    
    
    .TA21
    
     CPX #TGL               \ If this is not a Thargon, jump down to TA14
     BNE TA14
    
     LDA MANY+THG           \ If there is at least one Thargoid in the vicinity,
     BNE TA14               \ jump down to TA14
    
     LSR INWK+32            \ This is a Thargon but there is no Thargoid mothership,
     ASL INWK+32            \ so clear bit 0 of the AI flag to disable its E.C.M.
    
     LSR INWK+27            \ And halve the Thargon's speed
    
    .TA22
    
     RTS                    \ Return from the subroutine
    
    .TA14
    
     JSR DORND              \ Set A and X to random numbers
    
     LDA NEWB               \ Extract bit 0 of the ship's NEWB flags into the C flag
     LSR A                  \ and jump to TN1 if it is clear (i.e. if this is not a
     BCC TN1                \ trader)
    
     CPX #50                \ This is a trader, so if X >= 50 (80% chance), return
     BCS TA22               \ from the subroutine (as TA22 contains an RTS)
    
    .TN1
    
     LSR A                  \ Extract bit 1 of the ship's NEWB flags into the C flag
     BCC TN2                \ and jump to TN2 if it is clear (i.e. if this is not a
                            \ bounty hunter)
    
     LDX FIST               \ This is a bounty hunter, so check whether our FIST
     CPX #40                \ rating is < 40 (where 50 is a fugitive), and jump to
     BCC TN2                \ TN2 if we are not 100% evil
    
     LDA NEWB               \ We are a fugitive or a bad offender, and this ship is
     ORA #%00000100         \ a bounty hunter, so set bit 2 of the ship's NEWB flags
     STA NEWB               \ to make it hostile
    
     LSR A                  \ Shift A right twice so the next test in TN2 will check
     LSR A                  \ bit 2
    
    .TN2
    
     LSR A                  \ Extract bit 2 of the ship's NEWB flags into the C flag
     BCS TN3                \ and jump to TN3 if it is set (i.e. if this ship is
                            \ hostile)
    
     LSR A                  \ The ship is not hostile, so extract bit 4 of the
     LSR A                  \ ship's NEWB flags into the C flag, and jump to GOPL if
     BCC GOPL               \ it is clear (i.e. if this ship is not docking)
    
     JMP DOCKIT             \ The ship is not hostile and is docking, so jump to
                            \ DOCKIT to apply the docking algorithm to this ship
    
    .GOPL
    
     JSR SPS1               \ The ship is not hostile and it is not docking, so call
                            \ SPS1 to calculate the vector to the planet and store
                            \ it in XX15
    
     JMP TA151              \ Jump to TA151 to make the ship head towards the planet
    
    .TN3
    
     LSR A                  \ Extract bit 3 of the ship's NEWB flags into the C flag
     BCC TN4                \ and jump to TN4 if it is clear (i.e. if this ship is
                            \ not a pirate)
    
     LDA SSPR               \ If we are not inside the space station safe zone, jump
     BEQ TN4                \ to TN4
    
                            \ If we get here then this is a pirate and we are inside
                            \ the space station safe zone
    
     LDA INWK+32            \ Clear bits 1 to 6 of the AI flag in byte #32 (to set
     AND #%10000001         \ the aggression level to zero)
     STA INWK+32
    
    .TN4
    
     LDX #8                 \ We now want to copy the ship's x, y and z coordinates
                            \ from INWK to K3, so set up a counter for 9 bytes
    
    .TAL1
    
     LDA INWK,X             \ Copy the X-th byte from INWK to the X-th byte of K3
     STA K3,X
    
     DEX                    \ Decrement the counter
    
     BPL TAL1               \ Loop back until we have copied all 9 bytes
    
    .TA19
    
                            \ If this is a missile that's heading for its target
                            \ (not us, one of the other ships), then the missile
                            \ routine at TA18 above jumps here after setting K3 to
                            \ the vector from the target to the missile
    
     JSR TAS2               \ Normalise the vector in K3 and store the normalised
                            \ version in XX15, so XX15 contains the normalised
                            \ vector from our ship to the ship we are applying AI
                            \ tactics to (or the normalised vector from the target
                            \ to the missile - in both cases it's the vector from
                            \ the potential victim to the attacker)
    
     LDY #10                \ Set (A X) = nosev . XX15
     JSR TAS3
    
     STA CNT                \ Store the high byte of the dot product in CNT. The
                            \ bigger the value, the more aligned the two ships are,
                            \ with a maximum magnitude of 36 (96 * 96 >> 8). If CNT
                            \ is positive, the ships are facing in a similar
                            \ direction, if it's negative they are facing in
                            \ opposite directions
    

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Subroutine [DOCKIT](dockit.md) (category: Flight)

Apply docking manoeuvres to the ship in INWK

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [FIST](../workspace/wp.md#fist) in workspace [WP](../workspace/wp.md)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Entry point [GOPL](tactics_part_3_of_7.md#gopl) in subroutine [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md) (category: Tactics)

Make the ship head towards the planet

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [MANY](../workspace/wp.md#many) in workspace [WP](../workspace/wp.md)

The number of ships of each type in the local bubble of universe

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Subroutine [SPS1](sps1.md) (category: Maths (Geometry))

Calculate the vector to the planet and store it in XX15

[X]

Variable [SSPR](../workspace/wp.md#sspr) in workspace [WP](../workspace/wp.md)

"Space station present" flag

[X]

Label [TA14](tactics_part_3_of_7.md#ta14) is local to this routine

[X]

Entry point [TA151](tactics_part_7_of_7.md#ta151) in subroutine [TACTICS (Part 7 of 7)](tactics_part_7_of_7.md) (category: Tactics)

Make the ship head towards the planet

[X]

Label [TA22](tactics_part_3_of_7.md#ta22) is local to this routine

[X]

Label [TAL1](tactics_part_3_of_7.md#tal1) is local to this routine

[X]

Subroutine [TAS2](tas2.md) (category: Maths (Geometry))

Normalise the three-coordinate vector in K3

[X]

Subroutine [TAS3](tas3.md) (category: Maths (Geometry))

Calculate the dot product of XX15 and an orientation vector

[X]

Configuration variable [TGL](../../all/workspaces.md#tgl) = 30

Ship type for a Thargon

[X]

Configuration variable [THG](../../all/workspaces.md#thg) = 29

Ship type for a Thargoid

[X]

Label [TN1](tactics_part_3_of_7.md#tn1) is local to this routine

[X]

Label [TN2](tactics_part_3_of_7.md#tn2) is local to this routine

[X]

Label [TN3](tactics_part_3_of_7.md#tn3) is local to this routine

[X]

Label [TN4](tactics_part_3_of_7.md#tn4) is local to this routine

[TACTICS (Part 2 of 7)](tactics_part_2_of_7.md "Previous routine")[TACTICS (Part 4 of 7)](tactics_part_4_of_7.md "Next routine")
