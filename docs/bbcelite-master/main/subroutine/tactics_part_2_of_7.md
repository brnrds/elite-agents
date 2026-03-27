---
title: "The TACTICS (Part 2 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tactics_part_2_of_7.html"
---

[TACTICS (Part 1 of 7)](tactics_part_1_of_7.md "Previous routine")[TACTICS (Part 3 of 7)](tactics_part_3_of_7.md "Next routine")
    
    
           Name: TACTICS (Part 2 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Escape pod, station, lone Thargon, safe-zone pirate
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tactics-part-2-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/tactics_part_2_of_7.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MVEIT (Part 2 of 9)](mveit_part_2_of_9.md) calls TACTICS
    
    
    
    
    
    * * *
    
    
     This section contains the main entry point at TACTICS, which is called from
     part 2 of MVEIT for ships that have the AI flag set (i.e. bit 7 of byte #32).
     This part does the following:
    
       * If this is a missile, jump up to the missile code in part 1
    
       * If this is the space station and it is hostile, consider spawning a cop
         (6.2% chance, up to a maximum of seven) and we're done
    
       * If this is the space station and it is not hostile, consider spawning
         (0.8% chance if there are no Transporters around) a Transporter or Shuttle
         (equal odds of each type) and we're done
    
       * If this is a rock hermit, consider spawning (22% chance) a highly
         aggressive and hostile Sidewinder, Mamba, Krait, Adder or Gecko (equal
         odds of each type) and we're done
    
       * Recharge the ship's energy banks by 1
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The ship type
    
    
    
    
    .TACTICS
    
     LDA #3                 \ Set RAT = 3, which is the magnitude we set the pitch
     STA RAT                \ or roll counter to in part 7 when turning a ship
                            \ towards a vector (a higher value giving a longer
                            \ turn). This value is not changed in the TACTICS
                            \ routine, but it is set to different values by the
                            \ DOCKIT routine
    
     LDA #4                 \ Set RAT2 = 4, which is the threshold below which we
     STA RAT2               \ don't apply pitch and roll to the ship (so a lower
                            \ value means we apply pitch and roll more often, and a
                            \ value of 0 means we always apply them). The value is
                            \ compared with double the high byte of sidev . XX15,
                            \ where XX15 is the vector from the ship to the enemy
                            \ or planet. This value is set to different values by
                            \ both the TACTICS and DOCKIT routines
    
     LDA #22                \ Set CNT2 = 22, which is the maximum angle beyond which
     STA CNT2               \ a ship will slow down to start turning towards its
                            \ prey (a lower value means a ship will start to slow
                            \ down even if its angle with the enemy ship is large,
                            \ which gives a tighter turn). This value is not changed
                            \ in the TACTICS routine, but it is set to different
                            \ values by the DOCKIT routine
    
     CPX #MSL               \ If this is a missile, jump up to TA18 to implement
     BEQ TA18               \ missile tactics
    
     CPX #SST               \ If this is not the space station, jump down to TA13
     BNE TA13
    
     LDA NEWB               \ This is the space station, so check whether bit 2 of
     AND #%00000100         \ the ship's NEWB flags is set, and if it is (i.e. the
     BNE TN5                \ station is hostile), jump to TN5 to spawn some cops
    
     LDA MANY+SHU+1         \ The station is not hostile, so check how many
     BNE TA1                \ Transporters there are in the vicinity, and if we
                            \ already have one, return from the subroutine (as TA1
                            \ contains an RTS)
    
                            \ If we get here then the station is not hostile, so we
                            \ can consider spawning a Transporter or Shuttle
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #253               \ If A < 253 (99.2% chance), return from the subroutine
     BCC TA1                \ (as TA1 contains an RTS)
    
     AND #1                 \ Set A = a random number that's either 0 or 1
    
     ADC #SHU-1             \ The C flag is set (as we didn't take the BCC above),
     TAX                    \ so this sets X to a value of either #SHU or #SHU + 1,
                            \ which is the ship type for a Shuttle or a Transporter
    
     BNE TN6                \ Jump to TN6 to spawn this ship type and return from
                            \ the subroutine using a tail call (this BNE is
                            \ effectively a JMP as A is never zero)
    
    .TN5
    
                            \ If we get here then this is the space station and it
                            \ is hostile, so we need to spawn some cops
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #240               \ If A < 240 (93.8% chance), return from the subroutine
     BCC TA1                \ (as TA1 contains an RTS)
    
     LDA MANY+COPS          \ Check how many cops there are in the vicinity already,
     CMP #6                 \ and if there are 6 or more, return from the subroutine
     BCS TA22               \ (as TA22 contains an RTS)
    
     LDX #COPS              \ Set X to the ship type for a cop
    
    .TN6
    
     LDA #%11110001         \ Set the AI flag to give the ship E.C.M., enable AI and
                            \ make it very aggressive (56 out of 63)
    
     JMP SFS1               \ Jump to SFS1 to spawn the ship, returning from the
                            \ subroutine using a tail call
    
    .TA13
    
     CPX #HER               \ If this is not a rock hermit, jump down to TA17
     BNE TA17
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #200               \ If A < 200 (78% chance), return from the subroutine
     BCC TA22               \ (as TA22 contains an RTS)
    
     LDX #0                 \ Set byte #32 to %00000000 to disable AI, zero the
     STX INWK+32            \ aggression level and remove E.C.M.
    
     LDX #%00100100         \ Set the ship's NEWB flags to %00100100 so the ship we
     STX NEWB               \ spawn below will inherit the default values from E% as
                            \ well as having bit 2 (hostile) and bit 5 (innocent
                            \ bystander) set
    
     AND #3                 \ Set A = a random number that's in the range 0-3
    
     ADC #SH3               \ The C flag is set (as we didn't take the BCC above),
     TAX                    \ so this sets X to a random value between #SH3 + 1 and
                            \ #SH3 + 4, so that's a Sidewinder, Mamba, Krait, Adder
                            \ or Gecko
    
     JSR TN6                \ Call TN6 to spawn this ship with E.C.M., AI and a high
                            \ aggression (56 out of 63), though we override this in
                            \ the next instructions
    
     LDA #0                 \ Set byte #32 to %00000000 to disable AI, zero the
     STA INWK+32            \ aggression level and remove E.C.M. (for the rock
                            \ hermit)
    
     RTS                    \ Return from the subroutine
    
    .TA17
    
     LDY #14                \ If the ship's energy is greater or equal to the
     LDA INWK+35            \ maximum value from the ship's blueprint pointed to by
     CMP (XX0),Y            \ XX0, then skip the next instruction
     BCS TA21
    
     INC INWK+35            \ The ship's energy is not at maximum, so recharge the
                            \ energy banks by 1
    

[X]

Variable [CNT2](../workspace/zp.md#cnt2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Configuration variable [COPS](../../all/workspaces.md#cops) = 16

Ship type for a Viper

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Configuration variable [HER](../../all/workspaces.md#her) = 15

Ship type for a rock hermit (asteroid)

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [MANY](../workspace/wp.md#many) in workspace [WP](../workspace/wp.md)

The number of ships of each type in the local bubble of universe

[X]

Configuration variable [MSL](../../all/workspaces.md#msl) = 1

Ship type for a missile

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Variable [RAT](../workspace/zp.md#rat) in workspace [ZP](../workspace/zp.md)

Used to store different signs depending on the current space view, for use in calculating stardust movement

[X]

Variable [RAT2](../workspace/zp.md#rat2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the pitch and roll signs when moving objects and stardust

[X]

Subroutine [SFS1](sfs1.md) (category: Universe)

Spawn a child ship from the current (parent) ship

[X]

Configuration variable [SH3](../../all/workspaces.md#sh3) = 17

Ship type for a Sidewinder

[X]

Configuration variable [SHU](../../all/workspaces.md#shu) = 9

Ship type for a Shuttle

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[X]

Label [TA1](tactics_part_1_of_7.md#ta1) in subroutine [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md)

[X]

Label [TA13](tactics_part_2_of_7.md#ta13) is local to this routine

[X]

Label [TA17](tactics_part_2_of_7.md#ta17) is local to this routine

[X]

Label [TA18](tactics_part_1_of_7.md#ta18) in subroutine [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md)

[X]

Label [TA21](tactics_part_3_of_7.md#ta21) in subroutine [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md)

[X]

Label [TA22](tactics_part_3_of_7.md#ta22) in subroutine [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md)

[X]

Label [TN5](tactics_part_2_of_7.md#tn5) is local to this routine

[X]

Label [TN6](tactics_part_2_of_7.md#tn6) is local to this routine

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[TACTICS (Part 1 of 7)](tactics_part_1_of_7.md "Previous routine")[TACTICS (Part 3 of 7)](tactics_part_3_of_7.md "Next routine")
