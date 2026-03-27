---
title: "The TACTICS (Part 1 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tactics_part_1_of_7.html"
---

[HAS1](has1.md "Previous routine")[TACTICS (Part 2 of 7)](tactics_part_2_of_7.md "Next routine")
    
    
           Name: TACTICS (Part 1 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Process missiles, both enemy missiles and our own
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tactics-part-1-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/tactics_part_1_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section implements missile tactics and is entered at TA18 from the main
     entry point below, if the current ship is a missile. Specifically:
    
       * If E.C.M. is active, destroy the missile
    
       * If the missile is hostile towards us, then check how close it is. If it
         hasn't reached us, jump to part 3 so it can streak towards us, otherwise
         we've been hit, so process a large amount of damage to our ship
    
       * Otherwise see how close the missile is to its target. If it has not yet
         reached its target, give the target a chance to activate its E.C.M. if it
         has one, otherwise jump to TA19 with K3 set to the vector from the target
         to the missile
    
       * If it has reached its target and the target is the space station, destroy
         the missile, potentially damaging us if we are nearby
    
       * If it has reached its target and the target is a ship, destroy the missile
         and the ship, potentially damaging us if we are nearby
    
    
    
    
    .TA352
    
                            \ If we get here, the missile has been destroyed by
                            \ E.C.M. or by the space station
    
     LDA INWK               \ Set A = x_lo OR y_lo OR z_lo of the missile
     ORA INWK+3
     ORA INWK+6
    
     BNE TA872              \ If A is non-zero then the missile is not near our
                            \ ship, so skip the next two instructions to avoid
                            \ damaging our ship
    
     LDA #80                \ Otherwise the missile just got destroyed near us, so
     JSR OOPS               \ call OOPS to damage the ship by 80, which is nowhere
                            \ near as bad as the 250 damage from a missile slamming
                            \ straight into us, but it's still pretty nasty
    
    .TA872
    
     LDX #PLT               \ Set X to the ship type for plate alloys, so we get
                            \ awarded the kill points for the missile scraps in TA87
    
     BNE TA353              \ Jump to TA353 to process the missile kill tally and
                            \ make an explosion sound
    
    .TA34
    
                            \ If we get here, the missile is hostile
    
     LDA #0                 \ Set A to x_hi OR y_hi OR z_hi
     JSR MAS4
    
     BEQ P%+5               \ If A = 0 then the missile is very close to our ship,
                            \ so skip the following instruction
    
     JMP TN4                \ Jump down to part 3 to set up the vectors and skip
                            \ straight to aggressive manoeuvring
    
     JSR TA873              \ The missile has hit our ship, so call TA873 to set
                            \ bit 7 of the missile's byte #31, which marks the
                            \ missile as being killed
    
     JSR EXNO3              \ Make the sound of the missile exploding
    
     LDA #250               \ Call OOPS to damage the ship by 250, which is a pretty
     JMP OOPS               \ big hit, and return from the subroutine using a tail
                            \ call
    
    .TA18
    
                            \ This is the entry point for missile tactics and is
                            \ called from the main TACTICS routine below
    
     LDA ECMA               \ If an E.C.M. is currently active (either ours or an
     BNE TA352              \ opponent's), jump to TA352 to destroy this missile
    
     LDA INWK+32            \ Fetch the AI flag from byte #32 and if bit 6 is set
     ASL A                  \ (i.e. missile is hostile), jump up to TA34 to check
     BMI TA34               \ whether the missile has hit us
    
     LSR A                  \ Otherwise shift A right again. We know bits 6 and 7
                            \ are now clear, so this leaves bits 0-5. Bits 1-5
                            \ contain the target's slot number, and bit 0 is cleared
                            \ in FRMIS when a missile is launched, so A contains
                            \ the slot number shifted left by 1 (i.e. doubled) so we
                            \ can use it as an index for the two-byte address table
                            \ at UNIV
    
     TAX                    \ Copy the address of the target ship's data block from
     LDA UNIV,X             \ UNIV(X+1 X) to (A V)
     STA V
     LDA UNIV+1,X
    
     JSR VCSUB              \ Calculate vector K3 as follows:
                            \
                            \ K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate of
                            \ target ship
                            \
                            \ K3(5 4 3) = (y_sign y_hi z_lo) - y-coordinate of
                            \ target ship
                            \
                            \ K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate of
                            \ target ship
    
                            \ So K3 now contains the vector from the target ship to
                            \ the missile
    
     LDA K3+2               \ Set A = OR of all the sign and high bytes of the
     ORA K3+5               \ above, clearing bit 7 (i.e. ignore the signs)
     ORA K3+8
     AND #%01111111
     ORA K3+1
     ORA K3+4
     ORA K3+7
    
     BNE TA64               \ If the result is non-zero, then the missile is some
                            \ distance from the target, so jump down to TA64 see if
                            \ the target activates its E.C.M.
    
     LDA INWK+32            \ Fetch the AI flag from byte #32 and if only bits 7 and
     CMP #%10000010         \ 1 are set (AI is enabled and the target is slot 1, the
     BEQ TA352              \ space station), jump to TA352 to destroy this missile,
                            \ as the space station ain't kidding around
    
     LDY #31                \ Fetch byte #31 (the exploding flag) of the target ship
     LDA (V),Y              \ into A
    
     BIT M32+1              \ M32 contains an LDY #32 instruction, so M32+1 contains
                            \ 32, so this instruction tests A with %00100000, which
                            \ checks bit 5 of A (the "already exploding?" bit)
    
     BNE TA35               \ If the target ship is already exploding, jump to TA35
                            \ to destroy this missile
    
     ORA #%10000000         \ Otherwise set bit 7 of the target's byte #31 to mark
     STA (V),Y              \ the ship as having been killed, so it explodes
    
    .TA35
    
     LDA INWK               \ Set A = x_lo OR y_lo OR z_lo of the missile
     ORA INWK+3
     ORA INWK+6
    
     BNE P%+7               \ If A is non-zero then the missile is not near our
                            \ ship, so skip the next two instructions to avoid
                            \ damaging our ship
    
     LDA #80                \ Otherwise the missile just got destroyed near us, so
     JSR OOPS               \ call OOPS to damage the ship by 80, which is nowhere
                            \ near as bad as the 250 damage from a missile slamming
                            \ straight into us, but it's still pretty nasty
    
    .TA87
    
     LDA INWK+32            \ Set X to bits 1-6 of the missile's AI flag in ship
     AND #%01111111         \ byte #32, so that bits 0-4 of X are the target's slot
     LSR A                  \ number, and bit 5 is clear (as the missile is ours)
     TAX                    \
                            \ So X contains the slot number of the target ship
                            \
                            \ We should now fetch the ship type from slot X, so we
                            \ can pass the ship type to EXNO2 to add the correct
                            \ number of kill points to award for this type of ship,
                            \ but instead we're passing the slot number to EXNO2
                            \
                            \ This is a bug that means we will be allocated a fairly
                            \ random number of kill points when destroying a ship
                            \ with a missile; this bug was fixed in the NES version,
                            \ but it affects the Commodore 64, Apple II and BBC
                            \ Master versions of Elite
    
    .TA353
    
     JSR EXNO2              \ Call EXNO2 to process the fact that we have killed a
                            \ missile (so increase the kill tally, make an explosion
                            \ sound and so on)
    
    .TA873
    
     ASL INWK+31            \ Set bit 7 of the missile's byte #31 flag to mark it as
     SEC                    \ having been killed, so it explodes
     ROR INWK+31
    
    .TA1
    
     RTS                    \ Return from the subroutine
    
    .TA64
    
                            \ If we get here then the missile has not reached the
                            \ target
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #16                \ If A >= 16 (94% chance), jump down to TA19S with the
     BCS TA19S              \ vector from the target to the missile in K3
    
    .M32
    
     LDY #32                \ Fetch byte #32 for the target and shift bit 0 (E.C.M.)
     LDA (V),Y              \ into the C flag
     LSR A
    
     BCS P%+5               \ If the C flag is set then the target has E.C.M.
                            \ fitted, so skip the next instruction
    
    .TA19S
    
     JMP TA19               \ The target does not have E.C.M. fitted, so jump down
                            \ to TA19 with the vector from the target to the missile
                            \ in K3
    
     JMP ECBLB2             \ The target has E.C.M., so jump to ECBLB2 to set it
                            \ off, returning from the subroutine using a tail call
    

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [ECBLB2](ecblb2.md) (category: Dashboard)

Start up the E.C.M. (light up the indicator, start the countdown and make the E.C.M. sound)

[X]

Variable [ECMA](../workspace/zp.md#ecma) in workspace [ZP](../workspace/zp.md)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Subroutine [EXNO2](exno2.md) (category: Status)

Process us making a kill

[X]

Subroutine [EXNO3](exno3.md) (category: Sound)

Make an explosion sound

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [M32](tactics_part_1_of_7.md#m32) is local to this routine

[X]

Subroutine [MAS4](mas4.md) (category: Maths (Geometry))

Calculate a cap on the maximum distance to a ship

[X]

Subroutine [OOPS](oops.md) (category: Flight)

Take some damage

[X]

Configuration variable [PLT](../../all/workspaces.md#plt) = 4

Ship type for an alloy plate

[X]

Label [TA19](tactics_part_3_of_7.md#ta19) in subroutine [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md)

[X]

Label [TA19S](tactics_part_1_of_7.md#ta19s) is local to this routine

[X]

Label [TA34](tactics_part_1_of_7.md#ta34) is local to this routine

[X]

Label [TA35](tactics_part_1_of_7.md#ta35) is local to this routine

[X]

Label [TA352](tactics_part_1_of_7.md#ta352) is local to this routine

[X]

Label [TA353](tactics_part_1_of_7.md#ta353) is local to this routine

[X]

Label [TA64](tactics_part_1_of_7.md#ta64) is local to this routine

[X]

Label [TA872](tactics_part_1_of_7.md#ta872) is local to this routine

[X]

Label [TA873](tactics_part_1_of_7.md#ta873) is local to this routine

[X]

Label [TN4](tactics_part_3_of_7.md#tn4) in subroutine [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md)

[X]

Variable [UNIV](../variable/univ.md) (category: Universe)

Table of pointers to the local universe's ship data blocks

[X]

Variable [V](../workspace/zp.md#v) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing an address pointer

[X]

Subroutine [VCSUB](vcsub.md) (category: Maths (Arithmetic))

Calculate vector K3(8 0) = [x y z] - coordinates in (A V)

[HAS1](has1.md "Previous routine")[TACTICS (Part 2 of 7)](tactics_part_2_of_7.md "Next routine")
