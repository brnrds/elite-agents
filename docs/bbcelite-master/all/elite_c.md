---
title: "Elite C source in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_c.html"
---

[Elite B source](elite_b.md "Previous routine")[Elite D source](elite_d.md "Next routine")
    
    
     ELITE C FILE
    
    
    
    
     CODE_C% = P%
    
     LOAD_C% = LOAD% +P% - CODE%
    
    
    
    
           Name: HATB                                                    [Show more]
           Type: Variable
       Category: Ship hangar
        Summary: Ship hangar group table
    
    
        Context: See this variable [on its own page](../main/variable/hatb.md)
     Variations: See [code variations](../related/compare/main/variable/hatb.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [HALL](../main/subroutine/hall.md) uses HATB
    
    
    
    
    
    * * *
    
    
     This table contains groups of ships to show in the ship hangar. A group of
     ships is shown half the time (the other half shows a solo ship), and each of
     the four groups is equally likely.
    
     The bytes for each ship in the group contain the following information:
    
       Byte #0             Non-zero = Ship type to draw
                           0        = don't draw anything
    
       Byte #1             Bits 0-7 = Ship's x_hi
                           Bit 0    = Ship's z_hi (1 if clear, or 2 if set)
    
       Byte #2             Bits 0-7 = Ship's z_lo
                           Bit 0    = Ship's x_sign
    
     The ship's y-coordinate is calculated in the has1 routine from the size of
     its targetable area. Ships of type 0 are not shown.
    
    
    
    
    .HATB
    
                            \ Hangar group for X = 0
                            \
                            \ Cobra Mk III (left)
    
     EQUB 11                \ Ship type = 11 = Cobra Mk III
     EQUB %01000100         \ x_hi = %01000100 = 68, z_hi   = 1     -> x = -68
     EQUB %00111011         \ z_lo = %00111011 = 59, x_sign = 1        z = +315
    
     EQUB 0                 \ No second ship
     EQUB %10000010         \ x_hi = %10000010 = 130, z_hi   = 1    -> x = +130
     EQUB %10110000         \ z_lo = %10110000 = 176, x_sign = 0       z = +432
    
     EQUB 0                 \ No third ship
     EQUB 0
     EQUB 0
    
                            \ Hangar group for X = 9
                            \
                            \ Three cargo canisters (left, far right and forward,
                            \ right)
    
     EQUB OIL               \ Ship type = OIL = Cargo canister
     EQUB %01010000         \ x_hi = %01010000 = 80, z_hi   = 1     -> x = -80
     EQUB %00010001         \ z_lo = %00010001 = 17, x_sign = 1        z = +273
    
     EQUB OIL               \ Ship type = OIL = Cargo canister
     EQUB %11010001         \ x_hi = %11010001 = 209, z_hi = 2      -> x = +209
     EQUB %00101000         \ z_lo = %00101000 =  40, x_sign = 0       z = +552
    
     EQUB OIL               \ Ship type = OIL = Cargo canister
     EQUB %01000000         \ x_hi = %01000000 = 64, z_hi   = 1     -> x = +64
     EQUB %00000110         \ z_lo = %00000110 = 6,  x_sign = 0        z = +262
    
                            \ Hangar group for X = 18
                            \
                            \ Viper (right) and Krait (left)
    
     EQUB COPS              \ Ship type = COPS = Viper
     EQUB %01100000         \ x_hi = %01100000 =  96, z_hi   = 1    -> x = +96
     EQUB %10010000         \ z_lo = %10010000 = 144, x_sign = 0       z = +400
    
     EQUB KRA               \ Ship type = KRA = Krait
     EQUB %00010000         \ x_hi = %00010000 =  16, z_hi   = 1    -> x = -16
     EQUB %11010001         \ z_lo = %11010001 = 209, x_sign = 1       z = +465
    
     EQUB 0                 \ No third ship
     EQUB 0
     EQUB 0
    
                            \ Hangar group for X = 27
                            \
                            \ Adder (right and forward) and Viper (left)
    
     EQUB 20                \ Ship type = 20 = Adder
     EQUB %01010001         \ x_hi = %01010001 =  81, z_hi  = 2     -> x = +81
     EQUB %11111000         \ z_lo = %11111000 = 248, x_sign = 0       z = +760
    
     EQUB 16                \ Ship type = 16 = Viper
     EQUB %01100000         \ x_hi = %01100000 = 96,  z_hi   = 1    -> x = -96
     EQUB %01110101         \ z_lo = %01110101 = 117, x_sign = 1       z = +373
    
     EQUB 0                 \ No third ship
     EQUB 0
     EQUB 0
    
    
    
    
           Name: HALL                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw the ships in the ship hangar, then draw the hangar
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hall.md)
     Variations: See [code variations](../related/compare/main/subroutine/hall.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOENTRY](../main/subroutine/doentry.md) calls HALL
    
    
    
    
    
    * * *
    
    
     Half the time this will draw one of the four pre-defined ship hangar groups in
     HATB, and half the time this will draw a solitary Sidewinder, Mamba, Krait or
     Adder on a random position. In all cases, the ships will be randomly spun
     around on the ground so they can face in any direction, and larger ships are
     drawn higher up off the ground than smaller ships.
    
    
    
    
    .HALL
    
     LDA #0                 \ Switch to the mode 1 palette for the space view,
     JSR DOVDU19            \ which is yellow (colour 1), red (colour 2) and cyan
                            \ (colour 3)
    
     LDA #0                 \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 0 (space
                            \ view)
    
     JSR DORND              \ Set A and X to random numbers
    
     BPL HA7                \ Jump to HA7 if A is positive (50% chance)
    
     AND #3                 \ Reduce A to a random number in the range 0-3
    
     STA T                  \ Set X = A * 8 + A
     ASL A                  \       = 9 * A
     ASL A                  \
     ASL A                  \ so X is a random number, either 0, 9, 18 or 27
     ADC T
     TAX
    
                            \ The following double loop calls the HAS1 routine three
                            \ times to display three ships on screen. For each call,
                            \ the values passed to HAS1 in XX15+2 to XX15 are taken
                            \ from the HATB table, depending on the value in X, as
                            \ follows:
                            \
                            \   * If X = 0,  pass bytes #0 to #2 of HATB to HAS1
                            \                then bytes #3 to #5
                            \                then bytes #6 to #8
                            \
                            \   * If X = 9,  pass bytes  #9 to #11 of HATB to HAS1
                            \                then bytes #12 to #14
                            \                then bytes #15 to #17
                            \
                            \   * If X = 18, pass bytes #18 to #20 of HATB to HAS1
                            \                then bytes #21 to #23
                            \                then bytes #24 to #26
                            \
                            \   * If X = 27, pass bytes #27 to #29 of HATB to HAS1
                            \                then bytes #30 to #32
                            \                then bytes #33 to #35
                            \
                            \ Note that the values are passed in reverse, so for the
                            \ first call, for example, where we pass bytes #0 to #2
                            \ of HATB to HAS1, we call HAS1 with:
                            \
                            \   XX15   = HATB+2
                            \   XX15+1 = HATB+1
                            \   XX15+2 = HATB
    
     LDY #3                 \ Set CNT2 = 3 to act as an outer loop counter going
     STY CNT2               \ from 3 to 1, so the HAL8 loop is run 3 times
    
    .HAL8
    
     LDY #2                 \ Set Y = 2 to act as an inner loop counter going from
                            \ 2 to 0
    
    .HAL9
    
     LDA HATB,X             \ Copy the X-th byte of HATB to the Y-th byte of XX15,
     STA XX15,Y             \ as described above
    
     INX                    \ Increment X to point to the next byte in HATB
    
     DEY                    \ Decrement Y to point to the previous byte in XX15
    
     BPL HAL9               \ Loop back to copy the next byte until we have copied
                            \ three of them (i.e. Y was 3 before the DEY)
    
     TXA                    \ Store X on the stack so we can retrieve it after the
     PHA                    \ call to HAS1 (as it contains the index of the next
                            \ byte in HATB
    
     JSR HAS1               \ Call HAS1 to draw this ship in the hangar
    
     PLA                    \ Restore the value of X, so X points to the next byte
     TAX                    \ in HATB after the three bytes we copied into XX15
    
     DEC CNT2               \ Decrement the outer loop counter in CNT2
    
     BNE HAL8               \ Loop back to HAL8 to do it 3 times, once for each ship
                            \ in the HATB table
    
     LDY #128               \ Set Y = 128 to send as byte #2 of the parameter block
                            \ to the OSWORD 248 command below, to tell the I/O
                            \ processor that there are multiple ships in the hangar
    
     BNE HA9                \ Jump to HA9 to display the ship hangar (this BNE is
                            \ effectively a JMP as Y is never zero)
    
    .HA7
    
                            \ If we get here, A is a positive random number in the
                            \ range 0-127
    
     LSR A                  \ Set XX15+1 = A / 2 (random number 0-63)
     STA XX15+1
    
     JSR DORND              \ Set XX15 = random number 0-255
     STA XX15
    
     JSR DORND              \ Set XX15+2 = #SH3 + random number 0-3
     AND #3                 \
     ADC #SH3               \ which is the ship type of a Sidewinder, Mamba, Krait
     STA XX15+2             \ or Adder
    
     JSR HAS1               \ Call HAS1 to draw this ship in the hangar, with the
                            \ following properties:
                            \
                            \   * Random x-coordinate from -63 to +63
                            \
                            \   * Randomly chosen Sidewinder, Mamba, Krait or Adder
                            \
                            \   * Random z-coordinate from +256 to +639
    
     LDY #0                 \ Set Y = 0 to use in the following instruction, to tell
                            \ the hangar-drawing routine that there is just one ship
                            \ in the hangar, so it knows not to draw between the
                            \ ships
    
    .HA9
    
     STY HANGFLAG           \ Store Y in HANGFLAG to specify whether there are
                            \ multiple ships in the hangar
    
     JMP HANGER             \ Call HANGER to draw the hangar background and return
                            \ from the subroutine using a tail call
    
    
    
    
           Name: HAS1                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw a ship in the ship hangar
    
    
        Context: See this subroutine [on its own page](../main/subroutine/has1.md)
     Variations: See [code variations](../related/compare/main/subroutine/has1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HALL](../main/subroutine/hall.md) calls HAS1
    
    
    
    
    
    * * *
    
    
     The ship's position within the hangar is determined by the arguments and the
     size of the ship's targetable area, as follows:
    
       * The x-coordinate is (x_sign x_hi 0) from the arguments, so the ship can be
         left of centre or right of centre
    
       * The y-coordinate is negative and is lower down the screen for smaller
         ships, so smaller ships are drawn closer to the ground (because they are)
    
       * The z-coordinate is positive, with both z_hi (which is 1 or 2) and z_lo
         coming from the arguments
    
    
    
    * * *
    
    
     Arguments:
    
       XX15                 Bits 0-7 = Ship's z_lo
                            Bit 0    = Ship's x_sign
    
       XX15+1               Bits 0-7 = Ship's x_hi
                            Bit 0    = Ship's z_hi (1 if clear, or 2 if set)
    
       XX15+2               Non-zero = Ship type to draw
                            0        = Don't draw anything
    
    
    
    
    .HAS1
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace and reset
                            \ the orientation vectors, with nosev pointing out of
                            \ the screen, so this puts the ship flat on the
                            \ horizontal deck (the y = 0 plane) with its nose
                            \ pointing towards us
    
     LDA XX15               \ Set z_lo = XX15
     STA INWK+6
    
     LSR A                  \ Set the sign bit of x_sign to bit 0 of A
     ROR INWK+2
    
     LDA XX15+1             \ Set x_hi = XX15+1
     STA INWK
    
     LSR A                  \ Set z_hi = 1 + bit 0 of XX15+1
     LDA #1
     ADC #0
     STA INWK+7
    
     LDA #%10000000         \ Set bit 7 of y_sign, so y is negative
     STA INWK+5
    
     STA RAT2               \ Set RAT2 = %10000000, so the yaw calls in HAL5 below
                            \ are negative
    
     LDA #&0B               \ Set the ship line heap pointer in INWK(34 33) to point
     STA INWK+34            \ to &0B00
    
     JSR DORND              \ We now perform a random number of small angle (3.6
     STA XSAV               \ degree) rotations to spin the ship on the deck while
                            \ keeping it flat on the deck (a bit like spinning a
                            \ bottle), so we set XSAV to a random number between 0
                            \ and 255 for the number of small yaw rotations to
                            \ perform, so the ship could be pointing in any
                            \ direction by the time we're done
    
    .HAL5
    
     LDX #21                \ Rotate (sidev_x, nosev_x) by a small angle (yaw)
     LDY #9
     JSR MVS5
    
     LDX #23                \ Rotate (sidev_y, nosev_y) by a small angle (yaw)
     LDY #11
     JSR MVS5
    
     LDX #25                \ Rotate (sidev_z, nosev_z) by a small angle (yaw)
     LDY #13
     JSR MVS5
    
     DEC XSAV               \ Decrement the yaw counter in XSAV
    
     BNE HAL5               \ Loop back to yaw a little more until we have yawed
                            \ by the number of times in XSAV
    
     LDY XX15+2             \ Set Y = XX15+2, the ship type of the ship we need to
                            \ draw
    
     BEQ HA1                \ If Y = 0, return from the subroutine (as HA1 contains
                            \ an RTS)
    
     TYA                    \ Set X = 2 * Y
     ASL A
     TAX
    
     LDA XX21-2,X           \ Set XX0(1 0) to the X-th address in the ship blueprint
     STA XX0                \ address lookup table at XX21, so XX0(1 0) now points
     LDA XX21-1,X           \ to the blueprint for the ship we need to draw
     STA XX0+1
    
     BEQ HA1                \ If the high byte of the blueprint address is 0, then
                            \ this is not a valid blueprint address, so return from
                            \ the subroutine (as HA1 contains an RTS)
    
     LDY #1                 \ Set Q = ship byte #1
     LDA (XX0),Y
     STA Q
    
     INY                    \ Set R = ship byte #2
     LDA (XX0),Y            \
     STA R                  \ so (R Q) contains the ship's targetable area, which is
                            \ a square number
    
     JSR LL5                \ Set Q = SQRT(R Q)
    
     LDA #100               \ Set y_lo = (100 - Q) / 2
     SBC Q                  \
     LSR A                  \ so the bigger the ship's targetable area, the smaller
     STA INWK+3             \ the magnitude of the y-coordinate, so because we set
                            \ y_sign to be negative above, this means smaller ships
                            \ are drawn lower down, i.e. closer to the ground, while
                            \ larger ships are drawn higher up, as you would expect
    
     JSR TIDY               \ Call TIDY to tidy up the orientation vectors, to
                            \ prevent the ship from getting elongated and out of
                            \ shape due to the imprecise nature of trigonometry
                            \ in assembly language
    
     JMP LL9                \ Jump to LL9 to display the ship and return from the
                            \ subroutine using a tail call
    
    .HA1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TACTICS (Part 1 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Process missiles, both enemy missiles and our own
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tactics_part_1_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/tactics_part_1_of_7.md) for this subroutine in the different versions
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
    
    
    
    
           Name: TACTICS (Part 2 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Escape pod, station, lone Thargon, safe-zone pirate
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tactics_part_2_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/tactics_part_2_of_7.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md) calls TACTICS
    
    
    
    
    
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
    
    
    
    
           Name: TACTICS (Part 3 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Calculate dot product to determine ship's aim
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tactics_part_3_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/tactics_part_3_of_7.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOCKIT](../main/subroutine/dockit.md) calls via GOPL
    
    
    
    
    
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
    
    
    
    
           Name: TACTICS (Part 4 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Check energy levels, maybe launch escape pod if low
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tactics_part_4_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/tactics_part_4_of_7.md) for this subroutine in the different versions
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
    
    
    
    
           Name: TACTICS (Part 5 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Consider whether to launch a missile at us
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tactics_part_5_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/tactics_part_5_of_7.md) for this subroutine in the different versions
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
    
    
    
    
           Name: TACTICS (Part 6 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Consider firing a laser at us, if aim is true
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tactics_part_6_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/tactics_part_6_of_7.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section looks at potentially firing the ship's laser at us. Specifically:
    
       * If the ship is not pointing at us, skip to the next part
    
       * If the ship is pointing at us but not accurately, fire its laser at us and
         skip to the next part
    
       * If we are in the ship's crosshairs, register some damage to our ship, slow
         down the attacking ship, make the noise of us being hit by laser fire, and
         move on to the next part to manoeuvre the attacking ship
    
    
    
    
    .TA3
    
                            \ If we get here then the ship either has plenty of
                            \ energy, or levels are low but it couldn't manage to
                            \ launch a missile, so maybe we can fire the laser?
    
     LDA #0                 \ Set A to x_hi OR y_hi OR z_hi
     JSR MAS4
    
     AND #%11100000         \ If any of the hi bytes have any of bits 5-7 set, then
     BNE TA4                \ jump to TA4 to skip the laser checks, as the ship is
                            \ too far away from us to hit us with a laser
    
     LDX CNT                \ Set X = the dot product set above in CNT. If this is
                            \ positive, this ship and our ship are facing in similar
                            \ directions, but if it's negative then we are facing
                            \ each other, so for us to be in the enemy ship's line
                            \ of fire, X needs to be negative. The value in X can
                            \ have a maximum magnitude of 36, which would mean we
                            \ were facing each other square on, so in the following
                            \ code we check X like this:
                            \
                            \   X = 0 to -31, we are not in the enemy ship's line
                            \       of fire, so they can't shoot at us
                            \
                            \   X = -32 to -34, we are in the enemy ship's line
                            \       of fire, so they can shoot at us, but they can't
                            \       hit us as we're not dead in their crosshairs
                            \
                            \   X = -35 to -36, we are bang in the middle of the
                            \       enemy ship's crosshairs, so they can not only
                            \       shoot us, they can hit us
    
    \BPL TA4                \ This instruction is commented out in the original
                            \ source
    
     CPX #160               \ If X < 160, i.e. X > -32, then we are not in the enemy
     BCC TA4                \ ship's line of fire, so jump to TA4 to skip the laser
                            \ checks
    
     LDY #19                \ Fetch the enemy ship's byte #19 from their ship's
     LDA (XX0),Y            \ blueprint into A
    
     AND #%11111000         \ Extract bits 3-7, which contain the enemy's laser
                            \ power
    
     BEQ TA4                \ If the enemy has no laser power, jump to TA4 to skip
                            \ the laser checks
    
     LDA INWK+31            \ Set bit 6 in byte #31 to denote that the ship is
     ORA #%01000000         \ firing its laser at us
     STA INWK+31
    
     CPX #163               \ If X < 163, i.e. X > -35, then we are not in the enemy
     BCC TA4                \ ship's crosshairs, so jump to TA4 to skip the laser
                            \ checks
    
    \LDY #19                \ This instruction is commented out in the original
                            \ source
    
     LDA (XX0),Y            \ Fetch the enemy ship's byte #19 from their ship's
                            \ blueprint into A
    
     LSR A                  \ Halve the enemy ship's byte #19 (which contains both
                            \ the laser power and number of missiles) to get the
                            \ amount of damage we should take
    
     JSR OOPS               \ Call OOPS to take some damage, which could do anything
                            \ from reducing the shields and energy, all the way to
                            \ losing cargo or dying (if the latter, we don't come
                            \ back from this subroutine)
    
     DEC INWK+28            \ Halve the attacking ship's acceleration in byte #28
    
     LDA ECMA               \ If an E.C.M. is currently active (either ours or an
     BNE TA9-1              \ opponent's), return from the subroutine without making
                            \ the laser-strike sound (as TA9-1 contains an RTS)
    
     JSR ELASNO             \ Call the ELASNO routine to make the sound of us
                            \ being hit by lasers
    
    
    
    
           Name: TACTICS (Part 7 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Set pitch, roll, and acceleration
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tactics_part_7_of_7.md)
     Variations: See [code variations](../related/compare/main/subroutine/tactics_part_7_of_7.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOCKIT](../main/subroutine/dockit.md) calls via TA151
                 * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md) calls via TA151
                 * [TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md) calls via TA9-1
    
    
    
    
    
    * * *
    
    
     This section looks at manoeuvring the ship. Specifically:
    
       * Work out which direction the ship should be moving, depending on the type
         of ship, where it is, which direction it is pointing, and how aggressive
         it is
    
       * Set the pitch and roll counters to head in that direction
    
       * Speed up or slow down, depending on where the ship is in relation to us
    
    
    
    * * *
    
    
     Other entry points:
    
       TA151                Make the ship head towards the planet
    
       TA9-1                Contains an RTS
    
    
    
    
    .TA4
    
     LDA INWK+7             \ If z_hi >= 3 then the ship is quite far away, so jump
     CMP #3                 \ down to TA5
     BCS TA5
    
     LDA INWK+1             \ Otherwise set A = x_hi OR y_hi and extract bits 1-7
     ORA INWK+4
     AND #%11111110
    
     BEQ TA15               \ If A = 0 then the ship is pretty close to us, so jump
                            \ to TA15 so it heads away from us
    
    .TA5
    
                            \ If we get here then the ship is quite far away
    
     JSR DORND              \ Set A and X to random numbers
    
     ORA #%10000000         \ Set bit 7 of A, so the following comparison ignores
                            \ the AI flag in bit 7 (as we already know bit 7 is set
                            \ in byte #32)
    
     CMP INWK+32            \ If A >= byte #32 (the ship's AI flag) then jump down
     BCS TA15               \ to TA15 so it heads away from us
    
                            \ We get here if byte #32 > A, where byte #32 is
                            \ composed of the following:
                            \
                            \   * Bit 7 set = AI is enabled
                            \
                            \   * Bits 1-6 = aggression level (0 to 63)
                            \
                            \   * Bit 0 set = ship has E.C.M.
                            \
                            \ We set bit 7 of A above, so if we get here we know the
                            \ ship has AI enabled, and the comparison then boils
                            \ down to the following:
                            \
                            \   Aggression level * 2 + E.C.M. > random number 0-127
                            \
                            \ In other words, higher aggression levels increase the
                            \ chances of a ship changing direction to head towards
                            \ us - or, to put it another way, ships with higher
                            \ aggression levels are spoiling for a fight, with
                            \ E.C.M. making them even more aggressive
                            \
                            \ Thargoids and missiles both have an aggression level
                            \ of 63 out of 63, which explains an awful lot
                            \
                            \ Interestingly, escape pods also have a maximum
                            \ agression level, but in this case it makes them fly
                            \ towards the planet rather than towards us
    
    .TA20
    
                            \ If this is a missile we will have jumped straight
                            \ here, but we also get here if the ship is either far
                            \ away and aggressive, or not too close
    
     JSR TAS6               \ Call TAS6 to negate the vector in XX15 so it points in
                            \ the opposite direction
    
     LDA CNT                \ Change the sign of the dot product in CNT, so now it's
     EOR #%10000000         \ positive if the ships are facing each other, and
                            \ negative if they are facing the same way
    
    .TA152
    
     STA CNT                \ Update CNT with the new value in A
    
    .TA15
    
                            \ If we get here, then one of the following is true:
                            \
                            \   * This is a trader and XX15 is pointing towards the
                            \     planet
                            \
                            \   * The ship is pretty close to us, or it's just not
                            \     very aggressive (though there is a random factor
                            \     at play here too). XX15 is still pointing from our
                            \     ship towards the enemy ship
                            \
                            \   * The ship is aggressive (though again, there's an
                            \     element of randomness here). XX15 is pointing from
                            \     the enemy ship towards our ship
                            \
                            \   * This is a missile heading for a target. XX15 is
                            \     pointing from the missile towards the target
                            \
                            \ We now want to move the ship in the direction of XX15,
                            \ which will make aggressive ships head towards us, and
                            \ ships that are too close turn away. Peaceful traders,
                            \ meanwhile, head off towards the planet in search of a
                            \ space station, and missiles home in on their targets
    
     LDY #16                \ Set (A X) = roofv . XX15
     JSR TAS3               \
                            \ This will be positive if XX15 is pointing in the same
                            \ direction as an arrow out of the top of the ship, in
                            \ other words if the ship should pull up to head in the
                            \ direction of XX15
    
     TAX                    \ Copy A into X so we can retrieve it below
    
     EOR #%10000000         \ Give the ship's pitch counter the opposite sign to the
     AND #%10000000         \ dot product result, with a value of 0
     STA INWK+30
    
     TXA                    \ Retrieve the original value of A from X
    
     ASL A                  \ Shift A left to double it and drop the sign bit
    
     CMP RAT2               \ If A < RAT2, skip to TA11 (so if RAT2 = 0, we always
     BCC TA11               \ set the pitch counter to RAT)
    
     LDA RAT                \ Set the magnitude of the ship's pitch counter to RAT
     ORA INWK+30            \ (we already set the sign above)
     STA INWK+30
    
    .TA11
    
     LDA INWK+29            \ Fetch the roll counter from byte #29 into A
    
     ASL A                  \ Shift A left to double it and drop the sign bit
    
     CMP #32                \ If A >= 32 then jump to TA6, as the ship is already
     BCS TA6                \ in the process of rolling
    
     LDY #22                \ Set (A X) = sidev . XX15
     JSR TAS3               \
                            \ This will be positive if XX15 is pointing in the same
                            \ direction as an arrow out of the right side of the
                            \ ship, in other words if the ship should roll right to
                            \ head in the direction of XX15
    
     TAX                    \ Copy A into X so we can retrieve it below
    
     EOR INWK+30            \ Give the ship's roll counter a positive sign
     AND #%10000000         \ (clockwise roll) if the pitch counter and dot product
     EOR #%10000000         \ have different signs, negative (anti-clockwise roll)
     STA INWK+29            \ if they have the same sign, with a value of 0
    
     TXA                    \ Retrieve the original value of A from X
    
     ASL A                  \ Shift A left to double it and drop the sign bit
    
     CMP RAT2               \ If A < RAT2, skip to TA12 (so if RAT2 = 0, we always
     BCC TA12               \ set the roll counter to RAT)
    
     LDA RAT                \ Set the magnitude of the ship's roll counter to RAT
     ORA INWK+29            \ (we already set the sign above)
     STA INWK+29
    
    .TA12
    
    .TA6
    
     LDA CNT                \ Fetch the dot product, and if it's negative jump to
     BMI TA9                \ TA9, as the ships are facing away from each other and
                            \ the ship might want to slow down to take another shot
    
     CMP CNT2               \ The dot product is positive, so the ships are facing
     BCC TA9                \ each other. If A < CNT2 then the ships are not heading
                            \ directly towards each other, so jump to TA9 to slow
                            \ down
    
    .PH10E
    
     LDA #3                 \ Otherwise set the acceleration in byte #28 to 3
     STA INWK+28
    
     RTS                    \ Return from the subroutine
    
    .TA9
    
     AND #%01111111         \ Clear the sign bit of the dot product in A
    
     CMP #18                \ If A < 18 then the ship is way off the XX15 vector, so
     BCC TA10               \ return from the subroutine (TA10 contains an RTS)
                            \ without slowing down, as it still has quite a bit of
                            \ turning to do to get on course
    
     LDA #&FF               \ Otherwise set A = -1
    
     LDX TYPE               \ If this is not a missile then skip the ASL instruction
     CPX #MSL
     BNE P%+3
    
     ASL A                  \ This is a missile, so set A = -2, as missiles are more
                            \ nimble and can brake more quickly
    
     STA INWK+28            \ Set the ship's acceleration to A
    
    .TA10
    
     RTS                    \ Return from the subroutine
    
    .TA151
    
                            \ This is called from part 3 with the vector to the
                            \ planet in XX15, when we want the ship to turn towards
                            \ the planet. It does the same dot product calculation
                            \ as part 3, but it can also change the value of RAT2
                            \ so that roll and pitch is always applied
    
     LDY #10                \ Set (A X) = nosev . XX15
     JSR TAS3               \
                            \ The bigger the value of the dot product, the more
                            \ aligned the two vectors are, with a maximum magnitude
                            \ in A of 36 (96 * 96 >> 8). If A is positive, the
                            \ vectors are facing in a similar direction, if it's
                            \ negative they are facing in opposite directions
    
     CMP #&98               \ If A is positive or A <= -24, jump to ttt
     BCC ttt
    
     LDX #0                 \ A > -24, which means the vectors are facing in
     STX RAT2               \ opposite directions but are quite aligned, so set
                            \ RAT2 = 0 instead of the default value of 4, so we
                            \ always apply roll and pitch when we turn the ship
                            \ towards the planet
    
    .ttt
    
     JMP TA152              \ Jump to TA152 to store A in CNT and move the ship in
                            \ the direction of XX15
    
    
    
    
           Name: DOCKIT                                                  [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Apply docking manoeuvres to the ship in INWK
      Deep dive: [The docking computer](https://elite.bbcelite.com/deep_dives/the_docking_computer.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dockit.md)
     Variations: See [code variations](../related/compare/main/subroutine/dockit.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOKEY](../main/subroutine/dokey.md) calls DOCKIT
                 * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md) calls DOCKIT
    
    
    
    
    
    
    .DOCKIT
    
     LDA #6                 \ Set RAT2 = 6, which is the threshold below which we
     STA RAT2               \ don't apply pitch and roll to the ship (so a lower
                            \ value means we apply pitch and roll more often, and a
                            \ value of 0 means we always apply them). The value is
                            \ compared with double the high byte of sidev . XX15,
                            \ where XX15 is the vector from the ship to the station
    
     LSR A                  \ Set RAT = 2, which is the magnitude we set the pitch
     STA RAT                \ or roll counter to in part 7 when turning a ship
                            \ towards a vector (a higher value giving a longer
                            \ turn)
    
     LDA #29                \ Set CNT2 = 29, which is the maximum angle beyond which
     STA CNT2               \ a ship will slow down to start turning towards its
                            \ prey (a lower value means a ship will start to slow
                            \ down even if its angle with the enemy ship is large,
                            \ which gives a tighter turn)
    
     LDA SSPR               \ If we are inside the space station safe zone, skip the
     BNE P%+5               \ next instruction
    
    .GOPLS
    
     JMP GOPL               \ Jump to GOPL to make the ship head towards the planet
    
     JSR VCSU1              \ If we get here then we are in the space station safe
                            \ zone, so call VCSU1 to calculate the following, where
                            \ the station is at coordinates (station_x, station_y,
                            \ station_z):
                            \
                            \   K3(2 1 0) = (x_sign x_hi x_lo) - station_x
                            \
                            \   K3(5 4 3) = (y_sign y_hi z_lo) - station_y
                            \
                            \   K3(8 7 6) = (z_sign z_hi z_lo) - station_z
                            \
                            \ so K3 contains the vector from the station to the ship
    
     LDA K3+2               \ If any of the top bytes of the K3 results above are
     ORA K3+5               \ non-zero (after removing the sign bits), jump to GOPL
     ORA K3+8               \ via GOPLS to make the ship head towards the planet, as
     AND #%01111111         \ this will aim the ship in the general direction of the
     BNE GOPLS              \ station (it's too far away for anything more accurate)
    
     JSR TA2                \ Call TA2 to calculate the length of the vector in K3
                            \ (ignoring the low coordinates), returning it in Q
    
     LDA Q                  \ Store the value of Q in K, so K now contains the
     STA K                  \ distance between station and the ship
    
     JSR TAS2               \ Call TAS2 to normalise the vector in K3, returning the
                            \ normalised version in XX15, so XX15 contains the unit
                            \ vector pointing from the station to the ship
    
     LDY #10                \ Call TAS4 to calculate:
     JSR TAS4               \
                            \   (A X) = nosev . XX15
                            \
                            \ where nosev is the nose vector of the space station,
                            \ so this is the dot product of the station to ship
                            \ vector with the station's nosev (which points straight
                            \ out into space, out of the docking slot), and because
                            \ both vectors are unit vectors, the following is also
                            \ true:
                            \
                            \   (A X) = cos(t)
                            \
                            \ where t is the angle between the two vectors
                            \
                            \ If the dot product is positive, that means the vector
                            \ from the station to the ship and the nosev sticking
                            \ out of the docking slot are facing in a broadly
                            \ similar direction (so the ship is essentially heading
                            \ for the slot, which is facing towards the ship), and
                            \ if it's negative they are facing in broadly opposite
                            \ directions (so the station slot is on the opposite
                            \ side of the station as the ship approaches)
    
     BMI PH1                \ If the dot product is negative, i.e. the station slot
                            \ is on the opposite side, jump to PH1 to fly towards
                            \ the ideal docking position, some way in front of the
                            \ slot
    
     CMP #35                \ If the dot product < 35, jump to PH1 to fly towards
     BCC PH1                \ the ideal docking position, some way in front of the
                            \ slot, as there is a large angle between the vector
                            \ from the station to the ship and the station's nosev,
                            \ so the angle of approach is not very optimal
                            \
                            \ Specifically, as the unit vector length is 96 in our
                            \ vector system,
                            \
                            \   (A X) = cos(t) < 35 / 96
                            \
                            \ so:
                            \
                            \   t > arccos(35 / 96) = 68.6 degrees
                            \
                            \ so the ship is coming in from the side of the station
                            \ at an angle between 68.6 and 90 degrees off the
                            \ optimal entry angle
    
                            \ If we get here, the slot is on the same side as the
                            \ ship and the angle of approach is less than 68.6
                            \ degrees, so we're heading in pretty much the correct
                            \ direction for a good approach to the docking slot
    
     LDY #10                \ Call TAS3 to calculate:
     JSR TAS3               \
                            \   (A X) = nosev . XX15
                            \
                            \ where nosev is the nose vector of the ship, so this is
                            \ the dot product of the station to ship vector with the
                            \ ship's nosev, and is a measure of how close to the
                            \ station the ship is pointing, with negative meaning it
                            \ is pointing at the station, and positive meaning it is
                            \ pointing away from the station
    
     CMP #&A2               \ If the dot product is in the range 0 to -34, jump to
     BCS PH3                \ PH3 to refine our approach, as we are pointing towards
                            \ the station
    
                            \ If we get here, then we are not pointing straight at
                            \ the station, so check how close we are
    
     LDA K                  \ Fetch the distance to the station into A
    
    \BEQ PH10               \ This instruction is commented out in the original
                            \ source
    
     CMP #157               \ If A < 157, jump to PH2 to turn away from the station,
     BCC PH2                \ as we are too close
    
     LDA TYPE               \ Fetch the ship type into A
    
     BMI PH3                \ If bit 7 is set, then that means the ship type was set
                            \ to -96 in the DOKEY routine when we switched on our
                            \ docking computer, so this is us auto-docking our
                            \ Cobra, so jump to PH3 to refine our approach
                            \
                            \ Otherwise this is an NPC trying to dock, so keep going
                            \ to turn away from the station
    
    .PH2
    
                            \ If we get here then we turn away from the station and
                            \ slow right down, effectively aborting this approach
                            \ attempt
    
     JSR TAS6               \ Call TAS6 to negate the vector in XX15 so it points in
                            \ the opposite direction, away from the station and
                            \ towards the ship
    
     JSR TA151              \ Call TA151 to make the ship head in the direction of
                            \ XX15, which makes the ship turn away from the station
    
    .PH22
    
                            \ If we get here then we slam on the brakes and slow
                            \ right down
    
     LDX #0                 \ Set the acceleration in byte #28 to 0
     STX INWK+28
    
     INX                    \ Set the speed in byte #28 to 1
     STX INWK+27
    
     RTS                    \ Return from the subroutine
    
    .PH1
    
                            \ If we get here then the slot is on the opposite side
                            \ of the station to the ship, or it's on the same side
                            \ and the approach angle is not optimal, so we just fly
                            \ towards the station, aiming for the ideal docking
                            \ position some distance in front of the slot
    
     JSR VCSU1              \ Call VCSU1 to set K3 to the vector from the station to
                            \ the ship
    
     JSR DCS1               \ Call DCS1 twice to calculate the vector from the ideal
     JSR DCS1               \ docking position to the ship, where the ideal docking
                            \ position is straight out of the docking slot at a
                            \ distance of 8 unit vectors from the centre of the
                            \ station
    
     JSR TAS2               \ Call TAS2 to normalise the vector in K3, returning the
                            \ normalised version in XX15
    
     JSR TAS6               \ Call TAS6 to negate the vector in XX15 so it points in
                            \ the opposite direction
    
     JMP TA151              \ Call TA151 to make the ship head in the direction of
                            \ XX15, which makes the ship turn towards the ideal
                            \ docking position, and return from the subroutine using
                            \ a tail call
    
    .TN11
    
                            \ If we get here, we accelerate and apply a full
                            \ clockwise roll (which matches the space station's
                            \ roll)
    
     INC INWK+28            \ Increment the acceleration in byte #28
    
     LDA #%01111111         \ Set the roll counter to a positive (clockwise) roll
     STA INWK+29            \ with no damping, to match the space station's roll
    
     BNE TN13               \ Jump down to TN13 (this BNE is effectively a JMP as
                            \ A will never be zero)
    
    .PH3
    
                            \ If we get here, we refine our approach using pitch and
                            \ roll to aim for the station
    
     LDX #0                 \ Set RAT2 = 0
     STX RAT2
    
     STX INWK+30            \ Set the pitch counter to 0 to stop any pitching
    
     LDA TYPE               \ If this is not our ship's docking computer, but is an
     BPL PH32               \ NPC ship trying to dock, jump to PH32
    
                            \ In the following, ship_x and ship_y are the x and
                            \ y-coordinates of XX15, the vector from the station to
                            \ the ship
    
     EOR XX15               \ A is negative, so this sets the sign of A to the same
     EOR XX15+1             \ as -XX15 * XX15+1, or -ship_x * ship_y
    
     ASL A                  \ Shift the sign bit into the C flag, so the C flag has
                            \ the following sign:
                            \
                            \   * Positive if ship_x and ship_y have different signs
                            \   * Negative if ship_x and ship_y have the same sign
    
     LDA #2                 \ Set A = +2 or -2, giving it the sign in the C flag,
     ROR A                  \ and store it in byte #29, the roll counter, so that
     STA INWK+29            \ the ship rolls towards the station
    
     LDA XX15               \ If |ship_x * 2| >= 12, i.e. |ship_x| >= 6, then jump
     ASL A                  \ to PH22 to slow right down and return from the
     CMP #12                \ subroutine, as the station is not in our sights
     BCS PH22
    
     LDA XX15+1             \ Set A = +2 or -2, giving it the same sign as ship_y,
     ASL A                  \ and store it in byte #30, the pitch counter, so that
     LDA #2                 \ the ship pitches towards the station
     ROR A
     STA INWK+30
    
     LDA XX15+1             \ If |ship_y * 2| >= 12, i.e. |ship_y| >= 6, then jump
     ASL A                  \ to PH22 to slow right down and return from the
     CMP #12                \ subroutine, as the station is not in our sights
     BCS PH22
    
    .PH32
    
                            \ If we get here, we try to match the station roll
    
     STX INWK+29            \ Set the roll counter to 0 to stop any pitching
    
     LDA INWK+22            \ Set XX15 = sidev_x_hi
     STA XX15
    
     LDA INWK+24            \ Set XX15+1 = sidev_y_hi
     STA XX15+1
    
     LDA INWK+26            \ Set XX15+2 = sidev_z_hi
     STA XX15+2             \
                            \ so XX15 contains the sidev vector of the ship
    
     LDY #16                \ Call TAS4 to calculate:
     JSR TAS4               \
                            \   (A X) = roofv . XX15
                            \
                            \ where roofv is the roof vector of the space station.
                            \ To dock with the slot horizontal, we want roofv to be
                            \ pointing off to the side, i.e. parallel to the ship's
                            \ sidev vector, which means we want the dot product to
                            \ be large (it can be positive or negative, as roofv can
                            \ point left or right - it just needs to be parallel to
                            \ the ship's sidev)
    
     ASL A                  \ If |A * 2| >= 66, i.e. |A| >= 33, then the ship is
     CMP #66                \ lined up with the slot, so jump to TN11 to accelerate
     BCS TN11               \ and roll clockwise (a positive roll) before jumping
                            \ down to TN13 to check if we're docked yet
    
     JSR PH22               \ Call PH22 to slow right down, as we haven't yet
                            \ matched the station's roll
    
    .TN13
    
                            \ If we get here, we check to see if we have docked
    
     LDA K3+10              \ If K3+10 is non-zero, skip to TNRTS, to return from
     BNE TNRTS              \ the subroutine
                            \
                            \ I have to say I have no idea what K3+10 contains, as
                            \ it isn't mentioned anywhere in the whole codebase
                            \ apart from here, but it does share a location with
                            \ XX2+10, so it will sometimes be non-zero (specifically
                            \ when face #10 in the ship we're drawing is visible,
                            \ which probably happens quite a lot). This would seem
                            \ to affect whether an NPC ship can dock, as that's the
                            \ code that gets skipped if K3+10 is non-zero, but as
                            \ to what this means... that's not yet clear
    
     ASL NEWB               \ Set bit 7 of the ship's NEWB flags to indicate that
     SEC                    \ the ship has now docked, which only has meaning if
     ROR NEWB               \ this is an NPC trying to dock
    
    .TNRTS
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: VCSU1                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate vector K3(8 0) = [x y z] - coordinates of the sun or
                 space station
    
    
        Context: See this subroutine [on its own page](../main/subroutine/vcsu1.md)
     References: This subroutine is called as follows:
                 * [DOCKIT](../main/subroutine/dockit.md) calls VCSU1
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate of the sun or space station
    
       K3(5 4 3) = (y_sign y_hi z_lo) - y-coordinate of the sun or space station
    
       K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate of the sun or space station
    
     where the first coordinate is from the ship data block in INWK, and the second
     coordinate is from the sun or space station's ship data block which they
     share.
    
    
    
    
    .VCSU1
    
     LDA #LO(K%+NI%)        \ Set the low byte of V(1 0) to point to the coordinates
     STA V                  \ of the sun or space station
    
     LDA #HI(K%+NI%)        \ Set A to the high byte of the address of the
                            \ coordinates of the sun or space station
    
                            \ Fall through into VCSUB to calculate:
                            \
                            \   K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate of sun
                            \               or space station
                            \
                            \   K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate of sun
                            \               or space station
                            \
                            \   K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate of sun
                            \               or space station
    
    
    
    
           Name: VCSUB                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate vector K3(8 0) = [x y z] - coordinates in (A V)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/vcsub.md)
     References: This subroutine is called as follows:
                 * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md) calls VCSUB
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate in (A V)
    
       K3(5 4 3) = (y_sign y_hi z_lo) - y-coordinate in (A V)
    
       K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate in (A V)
    
     where the first coordinate is from the ship data block in INWK, and the second
     coordinate is from the ship data block pointed to by (A V).
    
    
    
    
    .VCSUB
    
     STA V+1                \ Set the low byte of V(1 0) to A, so now V(1 0) = (A V)
    
     LDY #2                 \ K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate in data
     JSR TAS1               \ block at V(1 0)
    
     LDY #5                 \ K3(5 4 3) = (y_sign y_hi z_lo) - y-coordinate of data
     JSR TAS1               \ block at V(1 0)
    
     LDY #8                 \ Fall through into TAS1 to calculate the final result:
                            \
                            \ K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate of data
                            \ block at V(1 0)
    
    
    
    
           Name: TAS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate K3 = (x_sign x_hi x_lo) - V(1 0)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tas1.md)
     References: This subroutine is called as follows:
                 * [VCSUB](../main/subroutine/vcsub.md) calls TAS1
    
    
    
    
    
    * * *
    
    
     Calculate one of the following, depending on the value in Y:
    
       K3(2 1 0) = (x_sign x_hi x_lo) - x-coordinate in V(1 0)
    
       K3(5 4 3) = (y_sign y_hi z_lo) - y-coordinate in V(1 0)
    
       K3(8 7 6) = (z_sign z_hi z_lo) - z-coordinate in V(1 0)
    
     where the first coordinate is from the ship data block in INWK, and the second
     coordinate is from the ship data block pointed to by V(1 0).
    
    
    
    * * *
    
    
     Arguments:
    
       V(1 0)               The address of the ship data block to subtract
    
       Y                    The coordinate in the V(1 0) block to subtract:
    
                              * If Y = 2, subtract the x-coordinate and store the
                                result in K3(2 1 0)
    
                              * If Y = 5, subtract the y-coordinate and store the
                                result in K3(5 4 3)
    
                              * If Y = 8, subtract the z-coordinate and store the
                                result in K3(8 7 6)
    
    
    
    
    .TAS1
    
     LDA (V),Y              \ Copy the sign byte of the V(1 0) coordinate into K+3,
     EOR #%10000000         \ flipping it in the process
     STA K+3
    
     DEY                    \ Copy the high byte of the V(1 0) coordinate into K+2
     LDA (V),Y
     STA K+2
    
     DEY                    \ Copy the high byte of the V(1 0) coordinate into K+1,
     LDA (V),Y              \ so now:
     STA K+1                \
                            \   K(3 2 1) = - coordinate in V(1 0)
    
     STY U                  \ Copy the index (now 0, 3 or 6) into U and X
     LDX U
    
     JSR MVT3               \ Call MVT3 to add the same coordinates, but this time
                            \ from INWK, so this would look like this for the
                            \ x-axis:
                            \
                            \   K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)
                            \            = (x_sign x_hi x_lo) - coordinate in V(1 0)
    
     LDY U                  \ Restore the index into Y, though this instruction has
                            \ no effect, as Y is not used again, either here or
                            \ following calls to this routine
    
     STA K3+2,X             \ Store K(3 2 1) in K3+X(2 1 0), starting with the sign
                            \ byte
    
     LDA K+2                \ And then doing the high byte
     STA K3+1,X
    
     LDA K+1                \ And finally the low byte
     STA K3,X
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TAS4                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the dot product of XX15 and one of the space station's
                 orientation vectors
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tas4.md)
     References: This subroutine is called as follows:
                 * [DOCKIT](../main/subroutine/dockit.md) calls TAS4
    
    
    
    
    
    * * *
    
    
     Calculate the dot product of the vector in XX15 and one of the space station's
     orientation vectors, as determined by the value of Y. If vect is the space
     station orientation vector, we calculate this:
    
       (A X) = vect . XX15
             = vect_x * XX15 + vect_y * XX15+1 + vect_z * XX15+2
    
     Technically speaking, this routine can also calculate the dot product between
     XX15 and the sun's orientation vectors, as the sun and space station share the
     same ship data slot (the second ship data block at K%). However, the sun
     doesn't have orientation vectors, so this only gets called when that slot is
     being used for the space station.
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The space station's orientation vector:
    
                              * If Y = 10, calculate nosev . XX15
    
                              * If Y = 16, calculate roofv . XX15
    
                              * If Y = 22, calculate sidev . XX15
    
    
    
    * * *
    
    
     Returns:
    
       (A X)                The result of the dot product
    
    
    
    
    .TAS4
    
     LDX K%+NI%,Y           \ Set Q = the Y-th byte of K%+NI%, i.e. vect_x from the
     STX Q                  \ second ship data block at K%
    
     LDA XX15               \ Set A = XX15
    
     JSR MULT12             \ Set (S R) = Q * A
                            \           = vect_x * XX15
    
     LDX K%+NI%+2,Y         \ Set Q = the Y+2-th byte of K%+NI%, i.e. vect_y
     STX Q
    
     LDA XX15+1             \ Set A = XX15+1
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
                            \           = vect_y * XX15+1 + vect_x * XX15
    
     STA S                  \ Set (S R) = (A X)
     STX R
    
     LDX K%+NI%+4,Y         \ Set Q = the Y+2-th byte of K%+NI%, i.e. vect_z
     STX Q
    
     LDA XX15+2             \ Set A = XX15+2
    
     JMP MAD                \ Set:
                            \
                            \   (A X) = Q * A + (S R)
                            \           = vect_z * XX15+2 + vect_y * XX15+1 +
                            \             vect_x * XX15
                            \
                            \ and return from the subroutine using a tail call
    
    
    
    
           Name: TAS6                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Negate the vector in XX15 so it points in the opposite direction
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tas6.md)
     References: This subroutine is called as follows:
                 * [DOCKIT](../main/subroutine/dockit.md) calls TAS6
                 * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md) calls TAS6
    
    
    
    
    
    
    .TAS6
    
     LDA XX15               \ Reverse the sign of the x-coordinate of the vector in
     EOR #%10000000         \ XX15
     STA XX15
    
     LDA XX15+1             \ Then reverse the sign of the y-coordinate
     EOR #%10000000
     STA XX15+1
    
     LDA XX15+2             \ And then the z-coordinate, so now the XX15 vector is
     EOR #%10000000         \ pointing in the opposite direction
     STA XX15+2
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DCS1                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Calculate the vector from the ideal docking position to the ship
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dcs1.md)
     References: This subroutine is called as follows:
                 * [DOCKIT](../main/subroutine/dockit.md) calls DCS1
    
    
    
    
    
    * * *
    
    
     This routine is called by the docking computer routine in DOCKIT. It works out
     the vector between the ship and the ideal docking position, which is straight
     in front of the docking slot, but some distance away.
    
     Specifically, it calculates the following:
    
       * K3(2 1 0) = K3(2 1 0) - nosev_x_hi * 4
    
       * K3(5 4 3) = K3(5 4 3) - nosev_y_hi * 4
    
       * K3(8 7 6) = K3(8 7 6) - nosev_x_hi * 4
    
     where K3 is the vector from the station to the ship, and nosev is the nose
     vector for the space station.
    
     The nose vector points from the centre of the station through the slot, so
     -nosev * 4 is the vector from a point in front of the docking slot, but some
     way from the station, back to the centre of the station. Adding this to the
     vector from the station to the ship gives the vector from the point in front
     of the station to the ship.
    
     In practice, this routine is called twice, so the ideal docking position is
     actually at a distance of 8 unit vectors from the centre of the station.
    
     Back in DOCKIT, we flip this vector round to get the vector from the ship to
     the point in front of the station slot.
    
    
    
    * * *
    
    
     Arguments:
    
       K3                   The vector from the station to the ship
    
    
    
    * * *
    
    
     Returns:
    
       K3                   The vector from the ship to the ideal docking position
                            (4 unit vectors from the centre of the station for each
                            call to DCS1, so two calls will return the vector to a
                            point that's 8 unit vectors from the centre of the
                            station)
    
    
    
    
    .DCS1
    
     JSR P%+3               \ Run the following routine twice, so the subtractions
                            \ are all * 4
    
     LDA K%+NI%+10          \ Set A to the space station's byte #10, nosev_x_hi
    
     LDX #0                 \ Set K3(2 1 0) = K3(2 1 0) - A * 2
     JSR TAS7               \               = K3(2 1 0) - nosev_x_hi * 2
    
     LDA K%+NI%+12          \ Set A to the space station's byte #12, nosev_y_hi
    
     LDX #3                 \ Set K3(5 4 3) = K3(5 4 3) - A * 2
     JSR TAS7               \               = K3(5 4 3) - nosev_y_hi * 2
    
     LDA K%+NI%+14          \ Set A to the space station's byte #14, nosev_z_hi
    
     LDX #6                 \ Set K3(8 7 6) = K3(8 7 6) - A * 2
                            \               = K3(8 7 6) - nosev_x_hi * 2
    
    .TAS7
    
                            \ This routine subtracts A * 2 from one of the K3
                            \ coordinates, as determined by the value of X:
                            \
                            \   * X = 0, set K3(2 1 0) = K3(2 1 0) - A * 2
                            \
                            \   * X = 3, set K3(5 4 3) = K3(5 4 3) - A * 2
                            \
                            \   * X = 6, set K3(8 7 6) = K3(8 7 6) - A * 2
                            \
                            \ Let's document it for X = 0, i.e. K3(2 1 0)
    
     ASL A                  \ Shift A left one place and move the sign bit into the
                            \ C flag, so A = |A * 2|
    
     STA R                  \ Set R = |A * 2|
    
     LDA #0                 \ Rotate the sign bit of A from the C flag into the sign
     ROR A                  \ bit of A, so A is now just the sign bit from the
                            \ original value of A. This also clears the C flag
    
     EOR #%10000000         \ Flip the sign bit of A, so it has the sign of -A
    
     EOR K3+2,X             \ Give A the correct sign of K3(2 1 0) * -A
    
     BMI TS71               \ If the sign of K3(2 1 0) * -A is negative, jump to
                            \ TS71, as K3(2 1 0) and A have the same sign
    
                            \ If we get here then K3(2 1 0) and A have different
                            \ signs, so we can add them to do the subtraction
    
     LDA R                  \ Set K3(2 1 0) = K3(2 1 0) + R
     ADC K3,X               \               = K3(2 1 0) + |A * 2|
     STA K3,X               \
                            \ starting with the low bytes
    
     BCC TS72               \ If the above addition didn't overflow, we have the
                            \ result we want, so jump to TS72 to return from the
                            \ subroutine
    
     INC K3+1,X             \ The above addition overflowed, so increment the high
                            \ byte of K3(2 1 0)
    
    .TS72
    
     RTS                    \ Return from the subroutine
    
    .TS71
    
                            \ If we get here, then K3(2 1 0) and A have the same
                            \ sign
    
     LDA K3,X               \ Set K3(2 1 0) = K3(2 1 0) - R
     SEC                    \               = K3(2 1 0) - |A * 2|
     SBC R                  \
     STA K3,X               \ starting with the low bytes
    
     LDA K3+1,X             \ And then the high bytes
     SBC #0
     STA K3+1,X
    
     BCS TS72               \ If the subtraction didn't underflow, we have the
                            \ result we want, so jump to TS72 to return from the
                            \ subroutine
    
     LDA K3,X               \ Negate the result in K3(2 1 0) by flipping all the
     EOR #%11111111         \ bits and adding 1, i.e. using two's complement to
     ADC #1                 \ give it the opposite sign, starting with the low
     STA K3,X               \ bytes
    
     LDA K3+1,X             \ Then doing the high bytes
     EOR #%11111111
     ADC #0
     STA K3+1,X
    
     LDA K3+2,X             \ And finally, flipping the sign bit
     EOR #%10000000
     STA K3+2,X
    
     JMP TS72               \ Jump to TS72 to return from the subroutine
    
    
    
    
           Name: HITCH                                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Work out if the ship in INWK is in our crosshairs
      Deep dive: [In the crosshairs](https://elite.bbcelite.com/deep_dives/in_the_crosshairs.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hitch.md)
     Variations: See [code variations](../related/compare/main/subroutine/hitch.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls HITCH
                 * [ANGRY](../main/subroutine/angry.md) calls via HI1
    
    
    
    
    
    * * *
    
    
     This is called by the main flight loop to see if we have laser or missile lock
     on an enemy ship.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the ship is in our crosshairs, clear if it isn't
    
    
    
    * * *
    
    
     Other entry points:
    
       HI1                  Contains an RTS
    
    
    
    
    .HITCH
    
     CLC                    \ Clear the C flag so we can return with it cleared if
                            \ our checks fail
    
     LDA INWK+8             \ Set A = z_sign
    
     BNE HI1                \ If A is non-zero then the ship is behind us and can't
                            \ be in our crosshairs, so return from the subroutine
                            \ with the C flag clear (as HI1 contains an RTS)
    
     LDA TYPE               \ If the ship type has bit 7 set then it is the planet
     BMI HI1                \ or sun, which we can't target or hit with lasers, so
                            \ return from the subroutine with the C flag clear (as
                            \ HI1 contains an RTS)
    
     LDA INWK+31            \ Fetch bit 5 of byte #31 (the exploding flag) and OR
     AND #%00100000         \ with x_hi and y_hi
     ORA INWK+1
     ORA INWK+4
    
     BNE HI1                \ If this value is non-zero then either the ship is
                            \ exploding (so we can't target it), or the ship is too
                            \ far away from our line of fire to be targeted, so
                            \ return from the subroutine with the C flag clear (as
                            \ HI1 contains an RTS)
    
     LDA INWK               \ Set A = x_lo
    
     JSR SQUA2              \ Set (A P) = A * A = x_lo^2
    
     STA S                  \ Set (S R) = (A P) = x_lo^2
     LDA P
     STA R
    
     LDA INWK+3             \ Set A = y_lo
    
     JSR SQUA2              \ Set (A P) = A * A = y_lo^2
    
     TAX                    \ Store the high byte in X
    
     LDA P                  \ Add the two low bytes, so:
     ADC R                  \
     STA R                  \   R = P + R
    
     TXA                    \ Restore the high byte into A and add S to give the
     ADC S                  \ following:
                            \
                            \   (A R) = (S R) + (A P) = x_lo^2 + y_lo^2
    
     BCS TN10               \ If the addition just overflowed then there is no way
                            \ our crosshairs are within the ship's targetable area,
                            \ so return from the subroutine with the C flag clear
                            \ (as TN10 contains a CLC then an RTS)
    
     STA S                  \ Set (S R) = (A P) = x_lo^2 + y_lo^2
    
     LDY #2                 \ Fetch the ship's blueprint and set A to the high byte
     LDA (XX0),Y            \ of the targetable area of the ship
    
     CMP S                  \ We now compare the high bytes of the targetable area
                            \ and the calculation in (S R):
                            \
                            \   * If A >= S then then the C flag will be set
                            \
                            \   * If A < S then the C flag will be C clear
    
     BNE HI1                \ If A <> S we have just set the C flag correctly, so
                            \ return from the subroutine (as HI1 contains an RTS)
    
     DEY                    \ The high bytes were identical, so now we fetch the
     LDA (XX0),Y            \ low byte of the targetable area into A
    
     CMP R                  \ We now compare the low bytes of the targetable area
                            \ and the calculation in (S R):
                            \
                            \   * If A >= R then the C flag will be set
                            \
                            \   * If A < R then the C flag will be C clear
    
    .HI1
    
     RTS                    \ Return from the subroutine
    
    .TN10
    
     CLC                    \ Clear the C flag to indicate the ship is not in our
                            \ crosshairs
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: FRS1                                                    [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Launch a ship straight ahead of us, below the laser sights
    
    
        Context: See this subroutine [on its own page](../main/subroutine/frs1.md)
     References: This subroutine is called as follows:
                 * [ESCAPE](../main/subroutine/escape.md) calls FRS1
                 * [FRMIS](../main/subroutine/frmis.md) calls FRS1
                 * [DEATH](../main/subroutine/death.md) calls via fq1
    
    
    
    
    
    * * *
    
    
     This is used in three places:
    
       * When we launch a missile, in which case the missile is the ship that is
         launched ahead of us
    
       * When we launch our escape pod, in which case it's our abandoned Cobra Mk
         III that is launched ahead of us
    
       * The fq1 entry point is used to launch a bunch of cargo canisters ahead of
         us as part of the death screen
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The type of ship to launch ahead of us
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the ship was successfully launched, clear if it
                            wasn't (as there wasn't enough free memory)
    
    
    
    * * *
    
    
     Other entry points:
    
       fq1                  Used to add a cargo canister to the universe
    
    
    
    
    .FRS1
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     LDA #28                \ Set y_lo = 28
     STA INWK+3
    
     LSR A                  \ Set z_lo = 14, so the launched ship starts out
     STA INWK+6             \ ahead of us
    
     LDA #%10000000         \ Set y_sign to be negative, so the launched ship is
     STA INWK+5             \ launched just below our line of sight
    
     LDA MSTG               \ Set A to the missile lock target, shifted left so the
     ASL A                  \ slot number is in bits 1-5
    
     ORA #%10000000         \ Set bit 7 and store the result in byte #32, the AI
     STA INWK+32            \ flag launched ship for the launched ship. For missiles
                            \ this enables AI (bit 7), makes it friendly towards us
                            \ (bit 6), sets the target to the value of MSTG (bits
                            \ 1-5), and sets its lock status as launched (bit 0).
                            \ It doesn't matter what it does for our abandoned
                            \ Cobra, as the AI flag gets overwritten once we return
                            \ from the subroutine back to the ESCAPE routine that
                            \ called FRS1 in the first place
    
    .fq1
    
     LDA #&60               \ Set byte #14 (nosev_z_hi) to 1 (&60), so the launched
     STA INWK+14            \ ship is pointing away from us
    
     ORA #128               \ Set byte #22 (sidev_x_hi) to -1 (&D0), so the launched
     STA INWK+22            \ ship has the same orientation as spawned ships, just
                            \ pointing away from us (if we set sidev to +1 instead,
                            \ this ship would be a mirror image of all the other
                            \ ships, which are spawned with -1 in nosev and +1 in
                            \ sidev)
    
     LDA DELTA              \ Set byte #27 (speed) to 2 * DELTA, so the launched
     ROL A                  \ ship flies off at twice our speed
     STA INWK+27
    
     TXA                    \ Add a new ship of type X to our local bubble of
     JMP NWSHP              \ universe and return from the subroutine using a tail
                            \ call
    
    
    
    
           Name: FRMIS                                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Fire a missile from our ship
    
    
        Context: See this subroutine [on its own page](../main/subroutine/frmis.md)
     Variations: See [code variations](../related/compare/main/subroutine/frmis.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls FRMIS
    
    
    
    
    
    * * *
    
    
     We fired a missile, so send it streaking away from us to unleash mayhem and
     destruction on our sworn enemies.
    
    
    
    
    .FRMIS
    
     LDX #MSL               \ Call FRS1 to launch a missile straight ahead of us
     JSR FRS1
    
     BCC FR1                \ If FRS1 returns with the C flag clear, then there
                            \ isn't room in the universe for our missile, so jump
                            \ down to FR1 to display a "missile jammed" message
    
     LDX MSTG               \ Fetch the slot number of the missile's target
    
     JSR GINF               \ Get the address of the data block for the target ship
                            \ and store it in INF
    
     LDA FRIN,X             \ Fetch the ship type of the missile's target into A
    
     JSR ANGRY              \ Call ANGRY to make the target ship or station hostile,
                            \ and if this is a ship, wake up its AI and give it a
                            \ kick of speed
    
     LDY #0                 \ We have just launched a missile, so we need to remove
     JSR ABORT              \ missile lock and hide the leftmost indicator on the
                            \ dashboard by setting it to black (Y = 0)
    
     DEC NOMSL              \ Reduce the number of missiles we have by 1
    
     LDY #solaun            \ Call the NOISE routine with Y = 8 to make the sound
     JSR NOISE              \ of a missile launch
    
                            \ Fall through into ANGRY to make the missile target
                            \ angry, though as we already did this above, I'm not
                            \ entirely sure why we do this again
    
    
    
    
           Name: ANGRY                                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Make a ship or station hostile, and if this is a ship then enable
                 the ship's AI and give it a kick of speed
      Deep dive: [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/angry.md)
     Variations: See [code variations](../related/compare/main/subroutine/angry.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [FRMIS](../main/subroutine/frmis.md) calls ANGRY
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls ANGRY
    
    
    
    
    
    * * *
    
    
     This routine makes a ship or station angry by setting the hostile flag in
     NEWB, and for ships it also means enabling the ship's AI and giving it a kick
     of turning acceleration. Later calls to TACTICS may make the ship start to
     attack us if it has a high enough aggression level.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The type of ship we're going to irritate
    
       INF                  The address of the data block for the ship we're going
                            to infuriate
    
    
    
    
    .ANGRY
    
     CMP #SST               \ If this is the space station, jump to AN2 to make the
     BEQ AN2                \ space station hostile
    
     LDY #36                \ Fetch the ship's NEWB flags from byte #36
     LDA (INF),Y
    
     AND #%00100000         \ If bit 5 of the ship's NEWB flags is clear, skip the
     BEQ P%+5               \ following instruction, otherwise bit 5 is set, meaning
                            \ this ship is an innocent bystander, and attacking it
                            \ will annoy the space station
    
     JSR AN2                \ Call AN2 to make the space station hostile
    
     LDY #32                \ Fetch the ship's byte #32 (AI flag)
     LDA (INF),Y
    
     BEQ HI1                \ If the AI flag is zero then this ship has no AI and
                            \ zero aggression, so return from the subroutine (as
                            \ HI1 contains an RTS)
    
     ORA #%10000000         \ Otherwise set bit 7 (AI enabled) to ensure AI is
     STA (INF),Y            \ definitely enabled, so the ship can start acting
                            \ according to its aggression level
    
     LDY #28                \ Set the ship's byte #28 (acceleration) to 2, so it
     LDA #2                 \ speeds up
     STA (INF),Y
    
     ASL A                  \ Set the ship's byte #30 (pitch counter) to 4, so it
     LDY #30                \ starts diving
     STA (INF),Y
    
     LDA TYPE               \ If the ship's type is < #CYL (i.e. a missile, Coriolis
     CMP #CYL               \ space station, escape pod, plate, cargo canister,
     BCC AN3                \ boulder, asteroid, splinter, Shuttle or Transporter),
                            \ then jump to AN3 to skip the following
    
     LDY #36                \ Set bit 2 of the ship's NEWB flags in byte #36 to
     LDA (INF),Y            \ make this ship hostile
     ORA #%00000100
     STA (INF),Y
    
    .AN3
    
     RTS                    \ Return from the subroutine
    
    .AN2
    
     LDA K%+NI%+36          \ Set bit 2 of the NEWB flags in byte #36 of the second
     ORA #%00000100         \ ship in the ship data workspace at K%, which is
     STA K%+NI%+36          \ reserved for the sun or the space station (in this
                            \ case it's the latter), to make it hostile
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: FR1                                                     [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Display the "missile jammed" message
    
    
        Context: See this subroutine [on its own page](../main/subroutine/fr1.md)
     References: This subroutine is called as follows:
                 * [FRMIS](../main/subroutine/frmis.md) calls FR1
    
    
    
    
    
    * * *
    
    
     This is shown if there isn't room in the local bubble of universe for a new
     missile.
    
    
    
    * * *
    
    
     Other entry points:
    
       FR1-2                Clear the C flag and return from the subroutine
    
    
    
    
    .FR1
    
     LDA #201               \ Print recursive token 41 ("MISSILE JAMMED") as an
     JMP MESS               \ in-flight message and return from the subroutine using
                            \ a tail call
    
    
    
    
           Name: SESCP                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Spawn an escape pod from the current (parent) ship
      Deep dive: [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sescp.md)
     References: This subroutine is called as follows:
                 * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md) calls SESCP
    
    
    
    
    
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
    
    
    
    
           Name: SFS1                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Spawn a child ship from the current (parent) ship
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sfs1.md)
     Variations: See [code variations](../related/compare/main/subroutine/sfs1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SPIN](../main/subroutine/spin.md) calls SFS1
                 * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md) calls SFS1
                 * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md) calls SFS1
                 * [SFRMIS](../main/subroutine/sfrmis.md) calls via SFS1-2
    
    
    
    
    
    * * *
    
    
     If the parent is a space station then the child ship is spawned coming out of
     the slot, and if the child is a cargo canister, it is sent tumbling through
     space. Otherwise the child ship is spawned with the same ship data as the
     parent, just with damping disabled and the ship type and AI flag that are
     passed in A and X.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    AI flag for the new ship (see the documentation on ship
                            data byte #32 for details)
    
       X                    The ship type of the child to spawn
    
       INF                  Address of the parent's ship data block
    
       TYPE                 The type of the parent ship
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if ship successfully added, clear if it failed
    
       INF                  INF is preserved
    
       XX0                  XX0 is preserved
    
       INWK                 The whole INWK workspace is preserved
    
       X                    X is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       SFS1-2               Used to add a missile to the local bubble that that has
                            AI (bit 7 set), is hostile (bit 6 set) and has been
                            launched (bit 0 clear); the target slot number is set to
                            31, but this is ignored as the hostile flags means we
                            are the target
    
    
    
    
    .SFS1
    
     STA T1                 \ Store the child ship's AI flag in T1
    
                            \ Before spawning our child ship, we need to save the
                            \ INF and XX00 variables and the whole INWK workspace,
                            \ so we can restore them later when returning from the
                            \ subroutine
    
     TXA                    \ Store X, the ship type to spawn, on the stack so we
     PHA                    \ can preserve it through the routine
    
     LDA XX0                \ Store XX0(1 0) on the stack, so we can restore it
     PHA                    \ later when returning from the subroutine
     LDA XX0+1
     PHA
    
     LDA INF                \ Store INF(1 0) on the stack, so we can restore it
     PHA                    \ later when returning from the subroutine
     LDA INF+1
     PHA
    
     LDY #NI%-1             \ Now we want to store the current INWK data block in
                            \ temporary memory so we can restore it when we are
                            \ done, and we also want to copy the parent's ship data
                            \ into INWK, which we can do at the same time, so set up
                            \ a counter in Y for NI% bytes
    
    .FRL2
    
     LDA INWK,Y             \ Copy the Y-th byte of INWK to the Y-th byte of
     STA XX3,Y              \ temporary memory in XX3, so we can restore it later
                            \ when returning from the subroutine
    
     LDA (INF),Y            \ Copy the Y-th byte of the parent ship's data block to
     STA INWK,Y             \ the Y-th byte of INWK
    
     DEY                    \ Decrement the loop counter
    
     BPL FRL2               \ Loop back to copy the next byte until we have done
                            \ them all
    
                            \ INWK now contains the ship data for the parent ship,
                            \ so now we need to tweak the data before creating the
                            \ new child ship (in this way, the child inherits things
                            \ like location from the parent)
    
     LDA TYPE               \ Fetch the ship type of the parent into A
    
     CMP #SST               \ If the parent is not a space station, jump to rx to
     BNE rx                 \ skip the following
    
                            \ The parent is a space station, so the child needs to
                            \ launch out of the space station's slot. The space
                            \ station's nosev vector points out of the station's
                            \ slot, so we want to move the ship along this vector.
                            \ We do this by taking the unit vector in nosev and
                            \ doubling it, so we spawn our ship 2 units along the
                            \ vector from the space station's centre
    
     TXA                    \ Store the child's ship type in X on the stack
     PHA
    
     LDA #32                \ Set the child's byte #27 (speed) to 32
     STA INWK+27
    
     LDX #0                 \ Add 2 * nosev_x_hi to (x_lo, x_hi, x_sign) to get the
     LDA INWK+10            \ child's x-coordinate
     JSR SFS2
    
     LDX #3                 \ Add 2 * nosev_y_hi to (y_lo, y_hi, y_sign) to get the
     LDA INWK+12            \ child's y-coordinate
     JSR SFS2
    
     LDX #6                 \ Add 2 * nosev_z_hi to (z_lo, z_hi, z_sign) to get the
     LDA INWK+14            \ child's z-coordinate
     JSR SFS2
    
     PLA                    \ Restore the child's ship type from the stack into X
     TAX
    
    .rx
    
     LDA T1                 \ Restore the child ship's AI flag from T1 and store it
     STA INWK+32            \ in the child's byte #32 (AI)
    
     LSR INWK+29            \ Clear bit 0 of the child's byte #29 (roll counter) so
     ASL INWK+29            \ that its roll dampens (so if we are spawning from a
                            \ space station, for example, the spawned ship won't
                            \ keep rolling forever)
    
     TXA                    \ Copy the child's ship type from X into A
    
     CMP #SPL+1             \ If the type of the child we are spawning is less than
     BCS NOIL               \ #PLT or greater than #SPL - i.e. not an alloy plate,
     CMP #PLT               \ cargo canister, boulder, asteroid or splinter - then
     BCC NOIL               \ jump to NOIL to skip us setting up some pitch and roll
                            \ for it
    
     PHA                    \ Store the child's ship type on the stack so we can
                            \ retrieve it below
    
     JSR DORND              \ Set A and X to random numbers
    
     ASL A                  \ Set the child's byte #30 (pitch counter) to a random
     STA INWK+30            \ value, and at the same time set the C flag randomly
    
     TXA                    \ Set the child's byte #27 (speed) to a random value
     AND #%00001111         \ between 0 and 15
     STA INWK+27
    
     LDA #&FF               \ Set the child's byte #29 (roll counter) to a full
     ROR A                  \ roll with no damping (as bits 0 to 6 are set), so the
     STA INWK+29            \ canister tumbles through space, with the direction in
                            \ bit 7 set randomly, depending on the C flag from above
    
     PLA                    \ Retrieve the child's ship type from the stack
    
    .NOIL
    
     JSR NWSHP              \ Add a new ship of type A to the local bubble
    
                            \ We have now created our child ship, so we need to
                            \ restore all the variables we saved at the start of
                            \ the routine, so they are preserved when we return
                            \ from the subroutine
    
     PLA                    \ Restore INF(1 0) from the stack
     STA INF+1
     PLA
     STA INF
    
     LDX #NI%-1             \ Now to restore the INWK workspace that we saved into
                            \ XX3 above, so set a counter in X for NI% bytes
    
    .FRL3
    
     LDA XX3,X              \ Copy the Y-th byte of XX3 to the Y-th byte of INWK
     STA INWK,X
    
     DEX                    \ Decrement the loop counter
    
     BPL FRL3               \ Loop back to copy the next byte until we have done
                            \ them all
    
     PLA                    \ Restore XX0(1 0) from the stack
     STA XX0+1
     PLA
     STA XX0
    
     PLA                    \ Retrieve the ship type to spawn from the stack into X
     TAX                    \ so it is preserved through calls to this routine
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SFS2                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move a ship in space along one of the coordinate axes
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sfs2.md)
     References: This subroutine is called as follows:
                 * [SFS1](../main/subroutine/sfs1.md) calls SFS2
    
    
    
    
    
    * * *
    
    
     Move a ship's coordinates by a certain amount in the direction of one of the
     axes, where X determines the axis. Mathematically speaking, this routine
     translates the ship along a single axis by a signed delta.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The amount of movement, i.e. the signed delta
    
       X                    Determines which coordinate axis of INWK to move:
    
                              * X = 0 moves the ship along the x-axis
    
                              * X = 3 moves the ship along the y-axis
    
                              * X = 6 moves the ship along the z-axis
    
    
    
    
    .SFS2
    
     ASL A                  \ Set R = |A * 2|, with the C flag set to bit 7 of A
     STA R
    
     LDA #0                 \ Set bit 7 of A to the C flag, i.e. the sign bit from
     ROR A                  \ the original argument in A
    
     JMP MVT1               \ Add the delta R with sign A to (x_lo, x_hi, x_sign)
                            \ (or y or z, depending on the value in X) and return
                            \ from the subroutine using a tail call
    
    
    
    
           Name: LL164                                                   [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Make the hyperspace sound and draw the hyperspace tunnel
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ll164.md)
     Variations: See [code variations](../related/compare/main/subroutine/ll164.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MJP](../main/subroutine/mjp.md) calls LL164
                 * [TT18](../main/subroutine/tt18.md) calls LL164
    
    
    
    
    
    * * *
    
    
     See the IRQ1 routine for details on the multi-coloured effect that's used.
    
    
    
    
    .LL164
    
     LDY #sohyp             \ Call the NOISE routine with Y = 10 to make the first
     JSR NOISE              \ sound of the hyperspace drive being engaged
    
     LDY #sohyp2            \ Call the NOISE routine with Y = 11 to make the second
     JSR NOISE              \ sound of the hyperspace drive being engaged
    
     LDA #4                 \ Set the step size for the hyperspace rings to 4, so
                            \ there are more sections in the rings and they are
                            \ quite round (compared to the step size of 8 used in
                            \ the much more polygonal launch rings)
    
     STA HFX                \ Set HFX to 4, which switches the screen mode to a full
                            \ mode 2 screen, therefore making the hyperspace rings
                            \ multi-coloured and all zig-zaggy (see the IRQ1 routine
                            \ for details)
    
     JSR HFS2               \ Call HFS2 to draw the hyperspace tunnel rings
    
     STZ HFX                \ Set HFX back to 0, so we switch back to the normal
                            \ split-screen mode
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LAUN                                                    [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Make the launch sound and draw the launch tunnel
    
    
        Context: See this subroutine [on its own page](../main/subroutine/laun.md)
     Variations: See [code variations](../related/compare/main/subroutine/laun.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOENTRY](../main/subroutine/doentry.md) calls LAUN
                 * [TT110](../main/subroutine/tt110.md) calls LAUN
    
    
    
    
    
    * * *
    
    
     This is shown when launching from or docking with the space station.
    
    
    
    
    .LAUN
    
     LDY #solaun            \ Call the NOISE routine with Y = 8 to make the sound
     JSR NOISE              \ of the ship launching from the station
    
     LDA #8                 \ Set the step size for the launch tunnel rings to 8, so
                            \ there are fewer sections in the rings and they are
                            \ quite polygonal (compared to the step size of 4 used
                            \ in the much rounder hyperspace rings)
    
                            \ Fall through into HFS2 to draw the launch tunnel rings
    
    
    
    
           Name: HFS2                                                    [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw the launch or hyperspace tunnel
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hfs2.md)
     Variations: See [code variations](../related/compare/main/subroutine/hfs2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL164](../main/subroutine/ll164.md) calls HFS2
                 * [TT110](../main/subroutine/tt110.md) calls via HFS1
    
    
    
    
    
    * * *
    
    
     The animation gets drawn like this. First, we draw a circle of radius 8 at the
     centre, and then double the radius, draw another circle, double the radius
     again and draw a circle, and we keep doing this until the radius is bigger
     than 160 (which goes beyond the edge of the screen, which is 256 pixels wide,
     equivalent to a radius of 128). We then repeat this whole process for an
     initial circle of radius 9, then radius 10, all the way up to radius 15.
    
     This has the effect of making the tunnel appear to be racing towards us as we
     hurtle out into hyperspace or through the space station's docking tunnel.
    
     The hyperspace effect is done in a full mode 2 screen, which makes the rings
     all coloured and zig-zaggy, while the launch screen is in the normal
     four-colour mode 1 screen.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The step size of the straight lines making up the rings
                            (4 for launch, 8 for hyperspace)
    
    
    
    * * *
    
    
     Other entry points:
    
       HFS1                 Don't clear the screen, and draw 8 concentric rings
                            with the step size in STP
    
    
    
    
    .HFS2
    
     STA STP                \ Store the step size in A
    
     LDA QQ11               \ Store the current view type in QQ11 on the stack
     PHA
    
     LDA #0                 \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 0 (the space
                            \ view)
    
     PLA                    \ Restore the view type from the stack
     STA QQ11
    
    .HFS1
    
     LDX #X                 \ Set K3 = #X (the x-coordinate of the centre of the
     STX K3                 \ screen)
    
     LDX #Y                 \ Set K4 = #Y (the y-coordinate of the centre of the
     STX K4                 \ screen)
    
     LDX #0                 \ Set X = 0
    
     STX XX4                \ Set XX4 = 0, which we will use as a counter for
                            \ drawing eight concentric rings
    
     STX K3+1               \ Set the high bytes of K3(1 0) and K4(1 0) to 0
     STX K4+1
    
    .HFL5
    
     JSR HFL1               \ Call HFL1 below to draw a set of rings, with each one
                            \ twice the radius of the previous one, until they won't
                            \ fit on-screen
    
     INC XX4                \ Increment the counter and fetch it into X
     LDX XX4
    
     CPX #8                 \ If we haven't drawn 8 sets of rings yet, loop back to
     BNE HFL5               \ HFL5 to draw the next ring
    
     RTS                    \ Return from the subroutine
    
    .HFL1
    
     LDA XX4                \ Set K to the ring number in XX4 (0-7) + 8, so K has
     AND #7                 \ a value of 8 to 15, which we will use as the starting
     CLC                    \ radius for our next set of rings
     ADC #8
     STA K
    
    .HFL2
    
     LDA #1                 \ Set LSP = 1 to reset the ball line heap
     STA LSP
    
     JSR CIRCLE2            \ Call CIRCLE2 to draw a circle with the centre at
                            \ (K3(1 0), K4(1 0)) and radius K
    
     ASL K                  \ Double the radius in K
    
     BCS HF8                \ If the radius had a 1 in bit 7 before the above shift,
                            \ then doubling K will means the circle will no longer
                            \ fit on the screen (which is width 256), so jump to
                            \ HF8 to stop drawing circles
    
     LDA K                  \ If the radius in K <= 160, loop back to HFL2 to draw
     CMP #160               \ another one
     BCC HFL2
    
    .HF8
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: STARS2                                                  [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Process the stardust for the left or right view
      Deep dive: [Stardust in the side views](https://elite.bbcelite.com/deep_dives/stardust_in_the_side_views.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/stars2.md)
     Variations: See [code variations](../related/compare/main/subroutine/stars2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STARS](../main/subroutine/stars.md) calls STARS2
    
    
    
    
    
    * * *
    
    
     This moves the stardust sideways according to our speed and which side we are
     looking out of, and applies our current pitch and roll to each particle of
     dust, so the stardust moves correctly when we steer our ship.
    
     These are the calculations referred to in the commentary:
    
       1. delta_x = 8 * 256 * speed / z_hi
       2. x = x + delta_x
    
       3. x = x + beta * y
       4. y = y - beta * x
    
       5. x = x - alpha * x * y
       6. y = y + alpha * y * y + alpha
    
     For more information see the associated deep dive.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The view to process:
    
                              * X = 1 for left view
    
                              * X = 2 for right view
    
    
    
    
    .STARS2
    
     LDA #0                 \ Set A to 0 so we can use it to capture a sign bit
    
     CPX #2                 \ If X >= 2 then the C flag is set
    
     ROR A                  \ Roll the C flag into the sign bit of A and store in
     STA RAT                \ RAT, so:
                            \
                            \   * Left view, C is clear so RAT = 0 (positive)
                            \
                            \   * Right view, C is set so RAT = 128 (negative)
                            \
                            \ RAT represents the end of the x-axis where we want new
                            \ stardust particles to come from: positive for the left
                            \ view where new particles come in from the right,
                            \ negative for the right view where new particles come
                            \ in from the left
    
     EOR #%10000000         \ Set RAT2 to the opposite sign, so:
     STA RAT2               \
                            \   * Left view, RAT2 = 128 (negative)
                            \
                            \   * Right view, RAT2 = 0 (positive)
                            \
                            \ RAT2 represents the direction in which stardust
                            \ particles should move along the x-axis: negative for
                            \ the left view where particles go from right to left,
                            \ positive for the right view where particles go from
                            \ left to right
    
     JSR ST2                \ Call ST2 to flip the signs of the following if this is
                            \ the right view: ALPHA, ALP2, ALP2+1, BET2 and BET2+1
    
     LDY NOSTM              \ Set Y to the current number of stardust particles, so
                            \ we can use it as a counter through all the stardust
    
    .STL2
    
     LDA SZ,Y               \ Set A = ZZ = z_hi
    
     STA ZZ                 \ We also set ZZ to the original value of z_hi, which we
                            \ use below to remove the existing particle
    
     LSR A                  \ Set A = z_hi / 8
     LSR A
     LSR A
    
     JSR DV41               \ Call DV41 to set the following:
                            \
                            \   (P R) = 256 * DELTA / A
                            \         = 256 * speed / (z_hi / 8)
                            \         = 8 * 256 * speed / z_hi
                            \
                            \ This represents the distance we should move this
                            \ particle along the x-axis, let's call it delta_x
    
     LDA P                  \ Store the high byte of delta_x in newzp
     STA newzp
    
     EOR RAT2               \ Set S = P but with the sign from RAT2, so we now have
     STA S                  \ the distance delta_x with the correct sign in (S R):
                            \
                            \   (S R) = delta_x
                            \         = 8 * 256 * speed / z_hi
                            \
                            \ So (S R) is the delta, signed to match the direction
                            \ the stardust should move in, which is result 1 above
    
     LDA SXL,Y              \ Set (A P) = (x_hi x_lo)
     STA P                  \           = x
     LDA SX,Y
    
     STA X1                 \ Set X1 = A, so X1 contains the original value of x_hi,
                            \ which we use below to remove the existing particle
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = x + delta_x
    
     STA S                  \ Set (S R) = (A X)
     STX R                  \           = x + delta_x
    
     LDA SY,Y               \ Set A = y_hi
    
     STA Y1                 \ Set Y1 = A, so Y1 contains the original value of y_hi,
                            \ which we use below to remove the existing particle
    
     EOR BET2               \ Give A the correct sign of A * beta, i.e. y_hi * beta
    
     LDX BET1               \ Fetch |beta| from BET1, the pitch angle
    
     JSR MULTS-2            \ Call MULTS-2 to calculate:
                            \
                            \   (A P) = X * A
                            \         = beta * y_hi
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = beta * y + x + delta_x
    
     STX XX                 \ Set XX(1 0) = (A X), which gives us results 2 and 3
     STA XX+1               \ above, done at the same time:
                            \
                            \   x = x + delta_x + beta * y
    
     LDX SYL,Y              \ Set (S R) = (y_hi y_lo)
     STX R                  \           = y
     LDX Y1
     STX S
    
     LDX BET1               \ Fetch |beta| from BET1, the pitch angle
    
     EOR BET2+1             \ Give A the opposite sign to x * beta
    
     JSR MULTS-2            \ Call MULTS-2 to calculate:
                            \
                            \   (A P) = X * A
                            \         = -beta * x
    
     JSR ADD                \ Call ADD to calculate:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = -beta * x + y
    
     STX YY                 \ Set YY(1 0) = (A X), which gives us result 4 above:
     STA YY+1               \
                            \   y = y - beta * x
    
     LDX ALP1               \ Set X = |alpha| from ALP2, the roll angle
    
     EOR ALP2               \ Give A the correct sign of A * alpha, i.e. y_hi *
                            \ alpha
    
     JSR MULTS-2            \ Call MULTS-2 to calculate:
                            \
                            \   (A P) = X * A
                            \         = alpha * y
    
     STA Q                  \ Set Q = high byte of alpha * y
    
     LDA XX                 \ Set (S R) = XX(1 0)
     STA R                  \           = x
     LDA XX+1               \
     STA S                  \ and set A = y_hi at the same time
    
     EOR #%10000000         \ Flip the sign of A = -x_hi
    
     JSR MAD                \ Call MAD to calculate:
                            \
                            \   (A X) = Q * A + (S R)
                            \         = alpha * y * -x + x
    
     STA XX+1               \ Store the high byte A in XX+1
    
     TXA                    \ Store the low byte X in x_lo
     STA SXL,Y
    
                            \ So (XX+1 x_lo) now contains result 5 above:
                            \
                            \   x = x - alpha * x * y
    
     LDA YY                 \ Set (S R) = YY(1 0)
     STA R                  \           = y
     LDA YY+1               \
     STA S                  \ and set A = y_hi at the same time
    
     JSR MAD                \ Call MAD to calculate:
                            \
                            \   (A X) = Q * A + (S R)
                            \         = alpha * y * y_hi + y
    
     STA S                  \ Set (S R) = (A X)
     STX R                  \           = y + alpha * y * y
    
     LDA #0                 \ Set P = 0
     STA P
    
     LDA ALPHA              \ Set A = alpha, so:
                            \
                            \   (A P) = (alpha 0)
                            \         = alpha / 256
    
     JSR PIX1               \ Call PIX1 to calculate the following:
                            \
                            \   (YY+1 y_lo) = (A P) + (S R)
                            \               = alpha * 256 + y + alpha * y * y
                            \
                            \ i.e. y = y + alpha / 256 + alpha * y^2, which is
                            \ result 6 above
                            \
                            \ PIX1 also draws a particle at (X1, Y1) with distance
                            \ ZZ, which will remove the old stardust particle, as we
                            \ set X1, Y1 and ZZ to the original values for this
                            \ particle during the calculations above
    
                            \ We now have our newly moved stardust particle at
                            \ x-coordinate (XX+1 x_lo) and y-coordinate (YY+1 y_lo)
                            \ and distance z_hi, so we draw it if it's still on
                            \ screen, otherwise we recycle it as a new bit of
                            \ stardust and draw that
    
     LDA XX+1               \ Set X1 and x_hi to the high byte of XX in XX+1, so
     STA SX,Y               \ the new x-coordinate is in (x_hi x_lo) and the high
     STA X1                 \ byte is in X1
    
     AND #%01111111         \ Set A = ~|x_hi|, which is the same as -(x_hi + 1)
     EOR #%01111111         \ using two's complement
    
     CMP newzp              \ If newzp <= -(x_hi + 1), then the particle has been
     BCC KILL2              \ moved off the side of the screen and has wrapped
     BEQ KILL2              \ round to the other side, jump to KILL2 to recycle this
                            \ particle and rejoin at STC2 with the new particle
                            \
                            \ In the original BBC Micro versions, this test simply
                            \ checks whether |x_hi| >= 116, but this version using
                            \ newzp doesn't hard-code the screen width, so this is
                            \ presumably a change that was introduced to support
                            \ the different screen sizes of the other platforms
    
     LDA YY+1               \ Set Y1 and y_hi to the high byte of YY in YY+1, so
     STA SY,Y               \ the new x-coordinate is in (y_hi y_lo) and the high
     STA Y1                 \ byte is in Y1
    
     AND #%01111111         \ If |y_hi| >= 116 then jump to ST5 to recycle this
     CMP #116               \ particle, as it's gone off the top or bottom of the
     BCS ST5                \ screen, and rejoin at STC2 with the new particle
    
    .STC2
    
     JSR PIXEL2             \ Draw a stardust particle at (X1,Y1) with distance ZZ,
                            \ i.e. draw the newly moved particle at (x_hi, y_hi)
                            \ with distance z_hi
    
     DEY                    \ Decrement the loop counter to point to the next
                            \ stardust particle
    
     BEQ ST2                \ If we have just done the last particle, skip the next
                            \ instruction to return from the subroutine
    
     JMP STL2               \ We have more stardust to process, so jump back up to
                            \ STL2 for the next particle
    
                            \ Fall through into ST2 to restore the signs of the
                            \ following if this is the right view: ALPHA, ALP2,
                            \ ALP2+1, BET2 and BET2+1
    
    .ST2
    
     LDA ALPHA              \ If this is the right view, flip the sign of ALPHA
     EOR RAT
     STA ALPHA
    
     LDA ALP2               \ If this is the right view, flip the sign of ALP2
     EOR RAT
     STA ALP2
    
     EOR #%10000000         \ If this is the right view, flip the sign of ALP2+1
     STA ALP2+1
    
     LDA BET2               \ If this is the right view, flip the sign of BET2
     EOR RAT
     STA BET2
    
     EOR #%10000000         \ If this is the right view, flip the sign of BET2+1
     STA BET2+1
    
     RTS                    \ Return from the subroutine
    
    .KILL2
    
     JSR DORND              \ Set A and X to random numbers
    
     STA Y1                 \ Set y_hi and Y1 to random numbers, so the particle
     STA SY,Y               \ starts anywhere along the y-axis
    
     LDA #115               \ Make sure A is at least 115 and has the sign in RAT
     ORA RAT
    
     STA X1                 \ Set x_hi and X1 to A, so this particle starts on the
     STA SX,Y               \ correct edge of the screen for new particles
    
     BNE STF1               \ Jump down to STF1 to set the z-coordinate (this BNE is
                            \ effectively a JMP as A will never be zero)
    
    .ST5
    
     JSR DORND              \ Set A and X to random numbers
    
     STA X1                 \ Set x_hi and X1 to random numbers, so the particle
     STA SX,Y               \ starts anywhere along the x-axis
    
     LDA #110               \ Make sure A is at least 110 and has the sign in AL2+1,
     ORA ALP2+1             \ the flipped sign of the roll angle alpha
    
     STA Y1                 \ Set y_hi and Y1 to A, so the particle starts at the
     STA SY,Y               \ top or bottom edge, depending on the current roll
                            \ angle alpha
    
    .STF1
    
     JSR DORND              \ Set A and X to random numbers
    
     ORA #8                 \ Make sure A is at least 8 and store it in z_hi and
     STA ZZ                 \ ZZ, so the new particle starts at any distance from
     STA SZ,Y               \ us, but not too close
    
     BNE STC2               \ Jump up to STC2 to draw this new particle (this BNE is
                            \ effectively a JMP as A will never be zero)
    
    
    
    
           Name: MU5                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Set K(3 2 1 0) = (A A A A) and clear the C flag
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mu5.md)
     References: This subroutine is called as follows:
                 * [MULT3](../main/subroutine/mult3.md) calls MU5
    
    
    
    
    
    * * *
    
    
     In practice this is only called via a BEQ following an AND instruction, in
     which case A = 0, so this routine effectively does this:
    
       K(3 2 1 0) = 0
    
    
    
    
    .MU5
    
     STA K                  \ Set K(3 2 1 0) to (A A A A)
     STA K+1
     STA K+2
     STA K+3
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MULT3                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate K(3 2 1 0) = (A P+1 P) * Q
      Deep dive: [Shift-and-add multiplication](https://elite.bbcelite.com/deep_dives/shift-and-add_multiplication.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mult3.md)
     References: This subroutine is called as follows:
                 * [MV40](../main/subroutine/mv40.md) calls MULT3
    
    
    
    
    
    * * *
    
    
     Calculate the following multiplication between a signed 24-bit number and a
     signed 8-bit number, returning the result as a signed 32-bit number:
    
       K(3 2 1 0) = (A P+1 P) * Q
    
     The algorithm is the same shift-and-add algorithm as in routine MULT1, but
     extended to cope with more bits.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .MULT3
    
     STA R                  \ Store the high byte of (A P+1 P) in R
    
     AND #%01111111         \ Set K+2 to |A|, the high byte of K(2 1 0)
     STA K+2
    
     LDA Q                  \ Set A to bits 0-6 of Q, so A = |Q|
     AND #%01111111
    
     BEQ MU5                \ If |Q| = 0, jump to MU5 to set K(3 2 1 0) to 0,
                            \ returning from the subroutine using a tail call
    
     SEC                    \ Set T = |Q| - 1
     SBC #1
     STA T
    
                            \ We now use the same shift-and-add algorithm as MULT1
                            \ to calculate the following:
                            \
                            \ K(2 1 0) = K(2 1 0) * |Q|
                            \
                            \ so we start with the first shift right, in which we
                            \ take (K+2 P+1 P) and shift it right, storing the
                            \ result in K(2 1 0), ready for the multiplication loop
                            \ (so the multiplication loop actually calculates
                            \ (|A| P+1 P) * |Q|, as the following sets K(2 1 0) to
                            \ (|A| P+1 P) shifted right)
    
     LDA P+1                \ Set A = P+1
    
     LSR K+2                \ Shift the high byte in K+2 to the right
    
     ROR A                  \ Shift the middle byte in A to the right and store in
     STA K+1                \ K+1 (so K+1 contains P+1 shifted right)
    
     LDA P                  \ Shift the middle byte in P to the right and store in
     ROR A                  \ K, so K(2 1 0) now contains (|A| P+1 P) shifted right
     STA K
    
                            \ We now use the same shift-and-add algorithm as MULT1
                            \ to calculate the following:
                            \
                            \ K(2 1 0) = K(2 1 0) * |Q|
    
     LDA #0                 \ Set A = 0 so we can start building the answer in A
    
     LDX #24                \ Set up a counter in X to count the 24 bits in K(2 1 0)
    
    .MUL2
    
     BCC P%+4               \ If C (i.e. the next bit from K) is set, do the
     ADC T                  \ addition for this bit of K:
                            \
                            \   A = A + T + C
                            \     = A + |Q| - 1 + 1
                            \     = A + |Q|
    
     ROR A                  \ Shift A right by one place to catch the next digit
     ROR K+2                \ next digit of our result in the left end of K(2 1 0),
     ROR K+1                \ while also shifting K(2 1 0) right to fetch the next
     ROR K                  \ bit for the calculation into the C flag
                            \
                            \ On the last iteration of this loop, the bit falling
                            \ off the end of K will be bit 0 of the original A, as
                            \ we did one shift before the loop and we are doing 24
                            \ iterations. We set A to 0 before looping, so this
                            \ means the loop exits with the C flag clear
    
     DEX                    \ Decrement the loop counter
    
     BNE MUL2               \ Loop back for the next bit until K(2 1 0) has been
                            \ rotated all the way
    
                            \ The result (|A| P+1 P) * |Q| is now in (A K+2 K+1 K),
                            \ but it is positive and doesn't have the correct sign
                            \ of the final result yet
    
     STA T                  \ Save the high byte of the result into T
    
     LDA R                  \ Fetch the sign byte from the original (A P+1 P)
                            \ argument that we stored in R
    
     EOR Q                  \ EOR with Q so the sign bit is the same as that of
                            \ (A P+1 P) * Q
    
     AND #%10000000         \ Extract the sign bit
    
     ORA T                  \ Apply this to the high byte of the result in T, so
                            \ that A now has the correct sign for the result, and
                            \ (A K+2 K+1 K) therefore contains the correctly signed
                            \ result
    
     STA K+3                \ Store A in K+3, so K(3 2 1 0) now contains the result
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MLS2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (S R) = XX(1 0) and (A P) = A * ALP1
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mls2.md)
     Variations: See [code variations](../related/compare/main/subroutine/mls2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [STARS1](../main/subroutine/stars1.md) calls MLS2
                 * [STARS6](../main/subroutine/stars6.md) calls MLS2
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       (S R) = XX(1 0)
    
       (A P) = A * ALP1
    
     where ALP1 is the magnitude of the current roll angle alpha, in the range
     0-31.
    
    
    
    
    .MLS2
    
     LDX XX                 \ Set (S R) = XX(1 0), starting with the low bytes
     STX R
    
     LDX XX+1               \ And then doing the high bytes
     STX S
    
                            \ Fall through into MLS1 to calculate (A P) = A * ALP1
    
    
    
    
           Name: MLS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = ALP1 * A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mls1.md)
     References: This subroutine is called as follows:
                 * [STARS1](../main/subroutine/stars1.md) calls MLS1
                 * [STARS6](../main/subroutine/stars6.md) calls MLS1
                 * [STARS1](../main/subroutine/stars1.md) calls via MULTS-2
                 * [STARS2](../main/subroutine/stars2.md) calls via MULTS-2
                 * [STARS6](../main/subroutine/stars6.md) calls via MULTS-2
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       (A P) = ALP1 * A
    
     where ALP1 is the magnitude of the current roll angle alpha, in the range
     0-31.
    
     This routine uses an unrolled version of MU11. MU11 calculates P * X, so we
     use the same algorithm but with P set to ALP1 and X set to A. The unrolled
     version here can skip the bit tests for bits 5-7 of P as we know P < 32, so
     only 5 shifts with bit tests are needed (for bits 0-4), while the other 3
     shifts can be done without a test (for bits 5-7).
    
    
    
    * * *
    
    
     Other entry points:
    
       MULTS-2              Calculate (A P) = X * A
    
    
    
    
    .MLS1
    
     LDX ALP1               \ Set P to the roll angle alpha magnitude in ALP1
     STX P                  \ (0-31), so now we calculate P * A
    
    .MULTS
    
     TAX                    \ Set X = A, so now we can calculate P * X instead of
                            \ P * A to get our result, and we can use the algorithm
                            \ from MU11 to do that, just unrolled (as MU11 returns
                            \ P * X)
    
     AND #%10000000         \ Set T to the sign bit of A
     STA T
    
     TXA                    \ Set A = |A|
     AND #127
    
     BEQ MU6                \ If A = 0, jump to MU6 to set P(1 0) = 0 and return
                            \ from the subroutine using a tail call
    
     TAX                    \ Set T1 = X - 1
     DEX                    \
     STX T1                 \ We subtract 1 as the C flag will be set when we want
                            \ to do an addition in the loop below
    
     LDA #0                 \ Set A = 0 so we can start building the answer in A
    
     LSR P                  \ Set P = P >> 1
                            \ and C flag = bit 0 of P
    
                            \ We are now going to work our way through the bits of
                            \ P, and do a shift-add for any bits that are set,
                            \ keeping the running total in A, but instead of using a
                            \ loop like MU11, we just unroll it, starting with bit 0
    
     BCC P%+4               \ If C (i.e. the next bit from P) is set, do the
     ADC T1                 \ addition for this bit of P:
                            \
                            \   A = A + T1 + C
                            \     = A + X - 1 + 1
                            \     = A + X
    
     ROR A                  \ Shift A right to catch the next digit of our result,
                            \ which the next ROR sticks into the left end of P while
                            \ also extracting the next bit of P
    
     ROR P                  \ Add the overspill from shifting A to the right onto
                            \ the start of P, and shift P right to fetch the next
                            \ bit for the calculation into the C flag
    
     BCC P%+4               \ Repeat the shift-and-add loop for bit 1
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat the shift-and-add loop for bit 2
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat the shift-and-add loop for bit 3
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat the shift-and-add loop for bit 4
     ADC T1
     ROR A
     ROR P
    
     LSR A                  \ Just do the "shift" part for bit 5
     ROR P
    
     LSR A                  \ Just do the "shift" part for bit 6
     ROR P
    
     LSR A                  \ Just do the "shift" part for bit 7
     ROR P
    
     ORA T                  \ Give A the sign bit of the original argument A that
                            \ we put into T above
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MU6                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Set P(1 0) = (A A)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mu6.md)
     References: This subroutine is called as follows:
                 * [MLS1](../main/subroutine/mls1.md) calls MU6
    
    
    
    
    
    * * *
    
    
     In practice this is only called via a BEQ following an AND instruction, in
     which case A = 0, so this routine effectively does this:
    
       P(1 0) = 0
    
    
    
    
    .MU6
    
     STA P+1                \ Set P(1 0) = (A A)
     STA P
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SQUA                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Clear bit 7 of A and calculate (A P) = A * A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/squa.md)
     References: This subroutine is called as follows:
                 * [NORM](../main/subroutine/norm.md) calls SQUA
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of unsigned 8-bit numbers, after first
     clearing bit 7 of A:
    
       (A P) = A * A
    
    
    
    
    .SQUA
    
     AND #%01111111         \ Clear bit 7 of A and fall through into SQUA2 to set
                            \ (A P) = A * A
    
    
    
    
           Name: SQUA2                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = A * A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/squa2.md)
     References: This subroutine is called as follows:
                 * [HITCH](../main/subroutine/hitch.md) calls SQUA2
                 * [MAS3](../main/subroutine/mas3.md) calls SQUA2
                 * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md) calls SQUA2
                 * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md) calls SQUA2
                 * [TT111](../main/subroutine/tt111.md) calls SQUA2
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of unsigned 8-bit numbers:
    
       (A P) = A * A
    
    
    
    
    .SQUA2
    
     STA P                  \ Copy A into P and X
     TAX
    
     BNE MU11               \ If X = 0 fall through into MU1 to return a 0,
                            \ otherwise jump to MU11 to return P * X
    
    
    
    
           Name: MU1                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Copy X into P and A, and clear the C flag
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mu1.md)
     References: This subroutine is called as follows:
                 * [MULTU](../main/subroutine/multu.md) calls MU1
    
    
    
    
    
    * * *
    
    
     Used to return a 0 result quickly from MULTU below.
    
    
    
    
    .MU1
    
     CLC                    \ Clear the C flag
    
     STX P                  \ Copy X into P and A
     TXA
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MLU1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate Y1 = y_hi and (A P) = |y_hi| * Q for Y-th stardust
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mlu1.md)
     References: This subroutine is called as follows:
                 * [STARS1](../main/subroutine/stars1.md) calls MLU1
                 * [STARS6](../main/subroutine/stars6.md) calls MLU1
    
    
    
    
    
    * * *
    
    
     Do the following assignment, and multiply the Y-th stardust particle's
     y-coordinate with an unsigned number Q:
    
       Y1 = y_hi
    
       (A P) = |y_hi| * Q
    
    
    
    
    .MLU1
    
     LDA SY,Y               \ Set Y1 the Y-th byte of SY
     STA Y1
    
                            \ Fall through into MLU2 to calculate:
                            \
                            \   (A P) = |A| * Q
    
    
    
    
           Name: MLU2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = |A| * Q
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mlu2.md)
     References: This subroutine is called as follows:
                 * [STARS1](../main/subroutine/stars1.md) calls MLU2
                 * [STARS6](../main/subroutine/stars6.md) calls MLU2
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of a sign-magnitude 8-bit number P with an
     unsigned number Q:
    
       (A P) = |A| * Q
    
    
    
    
    .MLU2
    
     AND #%01111111         \ Clear the sign bit in P, so P = |A|
     STA P
    
                            \ Fall through into MULTU to calculate:
                            \
                            \   (A P) = P * Q
                            \         = |A| * Q
    
    
    
    
           Name: MULTU                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = P * Q
    
    
        Context: See this subroutine [on its own page](../main/subroutine/multu.md)
     References: This subroutine is called as follows:
                 * [GCASH](../main/subroutine/gcash.md) calls MULTU
                 * [PLS3](../main/subroutine/pls3.md) calls MULTU
                 * [TT24](../main/subroutine/tt24.md) calls MULTU
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of unsigned 8-bit numbers:
    
       (A P) = P * Q
    
    
    
    
    .MULTU
    
     LDX Q                  \ Set X = Q
    
     BEQ MU1                \ If X = Q = 0, jump to MU1 to copy X into P and A,
                            \ clear the C flag and return from the subroutine using
                            \ a tail call
    
                            \ Otherwise fall through into MU11 to set (A P) = P * X
    
    
    
    
           Name: MU11                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = P * X
      Deep dive: [Shift-and-add multiplication](https://elite.bbcelite.com/deep_dives/shift-and-add_multiplication.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mu11.md)
     Variations: See [code variations](../related/compare/main/subroutine/mu11.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SQUA2](../main/subroutine/squa2.md) calls MU11
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of two unsigned 8-bit numbers:
    
       (A P) = P * X
    
     This uses the same shift-and-add approach as MULT1, but it's simpler as we
     are dealing with unsigned numbers in P and X.
    
    
    
    
    .MU11
    
     DEX                    \ Set T = X - 1
     STX T                  \
                            \ We subtract 1 as the C flag will be set when we want
                            \ to do an addition in the loop below
    
     LDA #0                 \ Set A = 0 so we can start building the answer in A
    
    \LDX #8                 \ This instruction is commented out in the original
                            \ source
    
     TAX                    \ Copy A into X. There is a comment in the original
                            \ source here that says "just in case", which refers to
                            \ the MU11 routine in the BBC Micro cassette and disc
                            \ versions, which set X to 0 (as they use X as a loop
                            \ counter)
                            \
                            \ The version here doesn't use a loop, but this
                            \ instruction makes sure the unrolled version returns
                            \ the same results as the loop versions, just in case
                            \ something out there relies on MU11 returning X = 0
    
     LSR P                  \ Set P = P >> 1
                            \ and C flag = bit 0 of P
    
                            \ We now repeat the following four instruction block
                            \ eight times, one for each bit in P. In the BBC Micro
                            \ cassette and disc versions of Elite the following is
                            \ done with a loop, but it is marginally faster to
                            \ unroll the loop and have eight copies of the code,
                            \ though it does take up a bit more memory (though that
                            \ isn't a big concern when you have a 6502 Second
                            \ Processor)
    
     BCC P%+4               \ If C (i.e. bit 0 of P) is set, do the
     ADC T                  \ addition for this bit of P:
                            \
                            \   A = A + T + C
                            \     = A + X - 1 + 1
                            \     = A + X
    
     ROR A                  \ Shift A right to catch the next digit of our result,
                            \ which the next ROR sticks into the left end of P while
                            \ also extracting the next bit of P
    
     ROR P                  \ Add the overspill from shifting A to the right onto
                            \ the start of P, and shift P right to fetch the next
                            \ bit for the calculation into the C flag
    
     BCC P%+4               \ Repeat for the second time
     ADC T
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the third time
     ADC T
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the fourth time
     ADC T
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the fifth time
     ADC T
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the sixth time
     ADC T
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the seventh time
     ADC T
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the eighth time
     ADC T
     ROR A
     ROR P
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: FMLTU2                                                  [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate A = K * sin(A)
      Deep dive: [The sine, cosine and arctan tables](https://elite.bbcelite.com/deep_dives/the_sine_cosine_and_arctan_tables.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/fmltu2.md)
     References: This subroutine is called as follows:
                 * [CIRCLE2](../main/subroutine/circle2.md) calls FMLTU2
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       A = K * sin(A)
    
     Because this routine uses the sine lookup table SNE, we can also call this
     routine to calculate cosine multiplication. To calculate the following:
    
       A = K * cos(B)
    
     call this routine with B + 16 in the accumulator, as sin(B + 16) = cos(B).
    
    
    
    
    .FMLTU2
    
     AND #%00011111         \ Restrict A to bits 0-5 (so it's in the range 0-31)
    
     TAX                    \ Set Q = sin(A) * 256
     LDA SNE,X
     STA Q
    
     LDA K                  \ Set A to the radius in K
    
                            \ Fall through into FMLTU to do the following:
                            \
                            \   (A ?) = A * Q
                            \         = K * sin(A) * 256
                            \
                            \ which is equivalent to:
                            \
                            \   A = K * sin(A)
    
    
    
    
           Name: FMLTU                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate A = A * Q / 256
      Deep dive: [Multiplication and division using logarithms](https://elite.bbcelite.com/deep_dives/multiplication_and_division_using_logarithms.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/fmltu.md)
     Variations: See [code variations](../related/compare/main/subroutine/fmltu.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOEXP](../main/subroutine/doexp.md) calls FMLTU
                 * [LL51](../main/subroutine/ll51.md) calls FMLTU
                 * [LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md) calls FMLTU
                 * [MVEIT (Part 3 of 9)](../main/subroutine/mveit_part_3_of_9.md) calls FMLTU
                 * [PLS22](../main/subroutine/pls22.md) calls FMLTU
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of two unsigned 8-bit numbers, returning only
     the high byte of the result:
    
       (A ?) = A * Q
    
     or, to put it another way:
    
       A = A * Q / 256
    
     The advanced versions of Elite use logarithms to speed up the multiplication
     process.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is clear if A = 0, or set if we return a
                            result from one of the log tables
    
    
    
    
    .FMLTU
    
     STX P                  \ Store X in P so we can preserve it through the call to
                            \ FMLTU
    
     STA widget             \ Store A in widget, so now widget = argument A
    
     TAX                    \ Transfer A into X, so now X = argument A
    
     BEQ MU3                \ If A = 0, jump to MU3 to return a result of 0, as
                            \ 0 * Q / 256 is always 0
    
                            \ We now want to calculate La + Lq, first adding the low
                            \ bytes (from the logL table), and then the high bytes
                            \ (from the log table)
    
     LDA logL,X             \ Set A = low byte of La
                            \       = low byte of La (as we set X to A above)
    
     LDX Q                  \ Set X = Q
    
     BEQ MU3again           \ If X = 0, jump to MU3again to return a result of 0, as
                            \ A * 0 / 256 is always 0
    
     CLC                    \ Set A = A + low byte of Lq
     ADC logL,X             \       = low byte of La + low byte of Lq
    
     LDA log,X              \ Set A = high byte of Lq
    
     LDX widget             \ Set A = A + C + high byte of La
     ADC log,X              \       = high byte of Lq + high byte of La + C
                            \
                            \ so we now have:
                            \
                            \   A = high byte of (La + Lq)
    
     BCC MU3again           \ If the addition fitted into one byte and didn't carry,
                            \ then La + Lq < 256, so we jump to MU3again to return a
                            \ result of 0 and the C flag clear
    
                            \ If we get here then the C flag is set, ready for when
                            \ we return from the subroutine below
    
     TAX                    \ Otherwise La + Lq >= 256, so we return the A-th entry
     LDA alogh,X            \ from the antilog table
    
     LDX P                  \ Restore X from P so it is preserved
    
     RTS                    \ Return from the subroutine
    
    .MU3again
    
     LDA #0                 \ Set A = 0
    
    .MU3
    
                            \ If we get here then A (our result) is already 0
    
     LDX P                  \ Restore X from P so it is preserved
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MLTU2                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P+1 P) = (A ~P) * Q
      Deep dive: [Shift-and-add multiplication](https://elite.bbcelite.com/deep_dives/shift-and-add_multiplication.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mltu2.md)
     References: This subroutine is called as follows:
                 * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md) calls MLTU2
                 * [MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md) calls via MLTU2-2
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of an unsigned 16-bit number and an unsigned
     8-bit number:
    
       (A P+1 P) = (A ~P) * Q
    
     where ~P means P EOR %11111111 (i.e. P with all its bits flipped). In other
     words, if you wanted to calculate &1234 * &56, you would:
    
       * Set A to &12
       * Set P to &34 EOR %11111111 = &CB
       * Set Q to &56
    
     before calling MLTU2.
    
     This routine is like a mash-up of MU11 and FMLTU. It uses part of FMLTU's
     inverted argument trick to work out whether or not to do an addition, and like
     MU11 it sets up a counter in X to extract bits from (P+1 P). But this time we
     extract 16 bits from (P+1 P), so the result is a 24-bit number. The core of
     the algorithm is still the shift-and-add approach explained in MULT1, just
     with more bits.
    
    
    
    * * *
    
    
     Returns:
    
       Q                    Q is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       MLTU2-2              Set Q to X, so this calculates (A P+1 P) = (A ~P) * X
    
    
    
    
     STX Q                  \ Store X in Q
    
    .MLTU2
    
     EOR #%11111111         \ Flip the bits in A and rotate right, storing the
     LSR A                  \ result in P+1, so we now calculate (P+1 P) * Q
     STA P+1
    
     LDA #0                 \ Set A = 0 so we can start building the answer in A
    
     LDX #16                \ Set up a counter in X to count the 16 bits in (P+1 P)
    
     ROR P                  \ Set P = P >> 1 with bit 7 = bit 0 of A
                            \ and C flag = bit 0 of P
    
    .MUL7
    
     BCS MU21               \ If C (i.e. the next bit from P) is set, do not do the
                            \ addition for this bit of P, and instead skip to MU21
                            \ to just do the shifts
    
     ADC Q                  \ Do the addition for this bit of P:
                            \
                            \   A = A + Q + C
                            \     = A + Q
    
     ROR A                  \ Rotate (A P+1 P) to the right, so we capture the next
     ROR P+1                \ digit of the result in P+1, and extract the next digit
     ROR P                  \ of (P+1 P) in the C flag
    
     DEX                    \ Decrement the loop counter
    
     BNE MUL7               \ Loop back for the next bit until P has been rotated
                            \ all the way
    
     RTS                    \ Return from the subroutine
    
    .MU21
    
     LSR A                  \ Shift (A P+1 P) to the right, so we capture the next
     ROR P+1                \ digit of the result in P+1, and extract the next digit
     ROR P                  \ of (P+1 P) in the C flag
    
     DEX                    \ Decrement the loop counter
    
     BNE MUL7               \ Loop back for the next bit until P has been rotated
                            \ all the way
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MUT3                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: An unused routine that does the same as MUT2
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mut3.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine is never actually called, but it is identical to MUT2, as the
     extra instructions have no effect.
    
    
    
    
    .MUT3
    
     LDX ALP1               \ Set P = ALP1, though this gets overwritten by the
     STX P                  \ following, so this has no effect
    
                            \ Fall through into MUT2 to do the following:
                            \
                            \   (S R) = XX(1 0)
                            \   (A P) = Q * A
    
    
    
    
           Name: MUT2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (S R) = XX(1 0) and (A P) = Q * A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mut2.md)
     References: This subroutine is called as follows:
                 * [STARS1](../main/subroutine/stars1.md) calls MUT2
    
    
    
    
    
    * * *
    
    
     Do the following assignment, and multiplication of two signed 8-bit numbers:
    
       (S R) = XX(1 0)
       (A P) = Q * A
    
    
    
    
    .MUT2
    
     LDX XX+1               \ Set S = XX+1
     STX S
    
                            \ Fall through into MUT1 to do the following:
                            \
                            \   R = XX
                            \   (A P) = Q * A
    
    
    
    
           Name: MUT1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate R = XX and (A P) = Q * A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mut1.md)
     References: This subroutine is called as follows:
                 * [STARS6](../main/subroutine/stars6.md) calls MUT1
    
    
    
    
    
    * * *
    
    
     Do the following assignment, and multiplication of two signed 8-bit numbers:
    
       R = XX
       (A P) = Q * A
    
    
    
    
    .MUT1
    
     LDX XX                 \ Set R = XX
     STX R
    
                            \ Fall through into MULT1 to do the following:
                            \
                            \   (A P) = Q * A
    
    
    
    
           Name: MULT1                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A P) = Q * A
      Deep dive: [Shift-and-add multiplication](https://elite.bbcelite.com/deep_dives/shift-and-add_multiplication.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mult1.md)
     Variations: See [code variations](../related/compare/main/subroutine/mult1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MAD](../main/subroutine/mad.md) calls MULT1
                 * [MULT12](../main/subroutine/mult12.md) calls MULT1
    
    
    
    
    
    * * *
    
    
     Do the following multiplication of two 8-bit sign-magnitude numbers:
    
       (A P) = Q * A
    
    
    
    
    .MULT1
    
     TAX                    \ Store A in X
    
     AND #%01111111         \ Set P = |A| >> 1
     LSR A                  \ and C flag = bit 0 of A
     STA P
    
     TXA                    \ Restore argument A
    
     EOR Q                  \ Set bit 7 of A and T if Q and A have different signs,
     AND #%10000000         \ clear bit 7 if they have the same signs, 0 all other
     STA T                  \ bits, i.e. T contains the sign bit of Q * A
    
     LDA Q                  \ Set A = |Q|
     AND #%01111111
    
     BEQ mu10               \ If |Q| = 0 jump to mu10 (with A set to 0)
    
     TAX                    \ Set T1 = |Q| - 1
     DEX                    \
     STX T1                 \ We subtract 1 as the C flag will be set when we want
                            \ to do an addition in the loop below
    
                            \ We are now going to work our way through the bits of
                            \ P, and do a shift-add for any bits that are set,
                            \ keeping the running total in A. We already set up
                            \ the first shift at the start of this routine, as
                            \ P = |A| >> 1 and C = bit 0 of A, so we now need to set
                            \ up a loop to sift through the other 7 bits in P
    
     LDA #0                 \ Set A = 0 so we can start building the answer in A
    
     TAX                    \ Copy A into X. There is a comment in the original
                            \ source here that says "just in case", which refers to
                            \ the MULT1 routine in the BBC Micro cassette and disc
                            \ versions, which set X to 0 (as they use X as a loop
                            \ counter)
                            \
                            \ The version here doesn't use a loop, but this
                            \ instruction makes sure the unrolled version returns
                            \ the same results as the loop versions, just in case
                            \ something out there relies on MULT1 returning X = 0
    
    \.MUL4                  \ These instructions are commented out in the original
    \                       \ source. They contain the original loop version of the
    \BCC P%+4               \ code that's used in the BBC Micro cassette and disc
    \ADC T1                 \ versions
    \
    \ROR A
    \ROR P
    \
    \DEX
    \
    \BNE MUL4
    \
    \LSR A
    \ROR P
    \
    \ORA T
    \
    \RTS
    \
    \.mu10
    \
    \STA P
    \
    \RTS
    
                            \ We now repeat the following four instruction block
                            \ seven times, one for each remaining bit in P. In the
                            \ BBC Micro cassette and disc versions of Elite the
                            \ following is done with a loop, but it is marginally
                            \ faster to unroll the loop and have seven copies of
                            \ the code, though it does take up a bit more memory
    
     BCC P%+4               \ If C (i.e. the next bit from P) is set, do the
     ADC T1                 \ addition for this bit of P:
                            \
                            \   A = A + T1 + C
                            \     = A + |Q| - 1 + 1
                            \     = A + |Q|
    
     ROR A                  \ As mentioned above, this ROR shifts A right and
                            \ catches bit 0 in C - giving another digit for our
                            \ result - and the next ROR sticks that bit into the
                            \ left end of P while also extracting the next bit of P
                            \ for the next addition
    
     ROR P                  \ Add the overspill from shifting A to the right onto
                            \ the start of P, and shift P right to fetch the next
                            \ bit for the calculation
    
     BCC P%+4               \ Repeat for the second time
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the third time
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the fourth time
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the fifth time
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the sixth time
     ADC T1
     ROR A
     ROR P
    
     BCC P%+4               \ Repeat for the seventh time
     ADC T1
     ROR A
     ROR P
    
     LSR A                  \ Rotate (A P) once more to get the final result, as
     ROR P                  \ we only pushed 7 bits through the above process
    
     ORA T                  \ Set the sign bit of the result that we stored in T
    
     RTS                    \ Return from the subroutine
    
    .mu10
    
     STA P                  \ If we get here, the result is 0 and A = 0, so set
                            \ P = 0 so (A P) = 0
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MULT12                                                  [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (S R) = Q * A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mult12.md)
     References: This subroutine is called as follows:
                 * [TAS3](../main/subroutine/tas3.md) calls MULT12
                 * [TAS4](../main/subroutine/tas4.md) calls MULT12
                 * [TIDY](../main/subroutine/tidy.md) calls MULT12
                 * [TIS3](../main/subroutine/tis3.md) calls MULT12
    
    
    
    
    
    * * *
    
    
     Calculate:
    
       (S R) = Q * A
    
    
    
    
    .MULT12
    
     JSR MULT1              \ Set (A P) = Q * A
    
     STA S                  \ Set (S R) = (A P)
     LDA P                  \           = Q * A
     STA R
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TAS3                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the dot product of XX15 and an orientation vector
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tas3.md)
     Variations: See [code variations](../related/compare/main/subroutine/tas3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOCKIT](../main/subroutine/dockit.md) calls TAS3
                 * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md) calls TAS3
                 * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md) calls TAS3
    
    
    
    
    
    * * *
    
    
     Calculate the dot product of the vector in XX15 and one of the orientation
     vectors, as determined by the value of Y. If vect is the orientation vector,
     we calculate this:
    
       (A X) = vect . XX15
             = vect_x * XX15 + vect_y * XX15+1 + vect_z * XX15+2
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The orientation vector:
    
                              * If Y = 10, calculate nosev . XX15
    
                              * If Y = 16, calculate roofv . XX15
    
                              * If Y = 22, calculate sidev . XX15
    
    
    
    * * *
    
    
     Returns:
    
       (A X)                The result of the dot product
    
    
    
    
    .TAS3
    
     LDX INWK,Y             \ Set Q = the Y-th byte of INWK, i.e. vect_x
     STX Q
    
     LDA XX15               \ Set A = XX15
    
     JSR MULT12             \ Set (S R) = Q * A
                            \           = vect_x * XX15
    
     LDX INWK+2,Y           \ Set Q = the Y+2-th byte of INWK, i.e. vect_y
     STX Q
    
     LDA XX15+1             \ Set A = XX15+1
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
                            \           = vect_y * XX15+1 + vect_x * XX15
    
     STA S                  \ Set (S R) = (A X)
     STX R
    
     LDX INWK+4,Y           \ Set Q = the Y+2-th byte of INWK, i.e. vect_z
     STX Q
    
     LDA XX15+2             \ Set A = XX15+2
    
                            \ Fall through into MAD to set:
                            \
                            \   (A X) = Q * A + (S R)
                            \           = vect_z * XX15+2 + vect_y * XX15+1 +
                            \             vect_x * XX15
    
    
    
    
           Name: MAD                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A X) = Q * A + (S R)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mad.md)
     References: This subroutine is called as follows:
                 * [MVS4](../main/subroutine/mvs4.md) calls MAD
                 * [STARS2](../main/subroutine/stars2.md) calls MAD
                 * [TAS3](../main/subroutine/tas3.md) calls MAD
                 * [TAS4](../main/subroutine/tas4.md) calls MAD
                 * [TIS1](../main/subroutine/tis1.md) calls MAD
                 * [TIS3](../main/subroutine/tis3.md) calls MAD
    
    
    
    
    
    * * *
    
    
     Calculate
    
       (A X) = Q * A + (S R)
    
    
    
    
    .MAD
    
     JSR MULT1              \ Call MULT1 to set (A P) = Q * A
    
                            \ Fall through into ADD to do:
                            \
                            \   (A X) = (A P) + (S R)
                            \         = Q * A + (S R)
    
    
    
    
           Name: ADD                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A X) = (A P) + (S R)
      Deep dive: [Adding sign-magnitude numbers](https://elite.bbcelite.com/deep_dives/adding_sign-magnitude_numbers.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/add.md)
     References: This subroutine is called as follows:
                 * [MVS5](../main/subroutine/mvs5.md) calls ADD
                 * [PLS22](../main/subroutine/pls22.md) calls ADD
                 * [STARS1](../main/subroutine/stars1.md) calls ADD
                 * [STARS2](../main/subroutine/stars2.md) calls ADD
                 * [STARS6](../main/subroutine/stars6.md) calls ADD
                 * [WARP](../main/subroutine/warp.md) calls ADD
    
    
    
    
    
    * * *
    
    
     Add two 16-bit sign-magnitude numbers together, calculating:
    
       (A X) = (A P) + (S R)
    
    
    
    
    .ADD
    
     STA T1                 \ Store argument A in T1
    
     AND #%10000000         \ Extract the sign (bit 7) of A and store it in T
     STA T
    
     EOR S                  \ EOR bit 7 of A with S. If they have different bit 7s
     BMI MU8                \ (i.e. they have different signs) then bit 7 in the
                            \ EOR result will be 1, which means the EOR result is
                            \ negative. So the AND, EOR and BMI together mean "jump
                            \ to MU8 if A and S have different signs"
    
                            \ If we reach here, then A and S have the same sign, so
                            \ we can add them and set the sign to get the result
    
     LDA R                  \ Add the least significant bytes together into X:
     CLC                    \
     ADC P                  \   X = P + R
     TAX
    
     LDA S                  \ Add the most significant bytes together into A. We
     ADC T1                 \ stored the original argument A in T1 earlier, so we
                            \ can do this with:
                            \
                            \   A = A  + S + C
                            \     = T1 + S + C
    
     ORA T                  \ If argument A was negative (and therefore S was also
                            \ negative) then make sure result A is negative by
                            \ OR'ing the result with the sign bit from argument A
                            \ (which we stored in T)
    
     RTS                    \ Return from the subroutine
    
    .MU8
    
                            \ If we reach here, then A and S have different signs,
                            \ so we can subtract their absolute values and set the
                            \ sign to get the result
    
     LDA S                  \ Clear the sign (bit 7) in S and store the result in
     AND #%01111111         \ U, so U now contains |S|
     STA U
    
     LDA P                  \ Subtract the least significant bytes into X:
     SEC                    \
     SBC R                  \   X = P - R
     TAX
    
     LDA T1                 \ Restore the A of the argument (A P) from T1 and
     AND #%01111111         \ clear the sign (bit 7), so A now contains |A|
    
     SBC U                  \ Set A = |A| - |S|
    
                            \ At this point we have |A P| - |S R| in (A X), so we
                            \ need to check whether the subtraction above was the
                            \ right way round (i.e. that we subtracted the smaller
                            \ absolute value from the larger absolute value)
    
     BCS MU9                \ If |A| >= |S|, our subtraction was the right way
                            \ round, so jump to MU9 to set the sign
    
                            \ If we get here, then |A| < |S|, so our subtraction
                            \ above was the wrong way round (we actually subtracted
                            \ the larger absolute value from the smaller absolute
                            \ value). So let's subtract the result we have in (A X)
                            \ from zero, so that the subtraction is the right way
                            \ round
    
     STA U                  \ Store A in U
    
     TXA                    \ Set X = 0 - X using two's complement (to negate a
     EOR #&FF               \ number in two's complement, you can invert the bits
     ADC #1                 \ and add one - and we know the C flag is clear as we
     TAX                    \ didn't take the BCS branch above, so the ADC will do
                            \ the correct addition)
    
     LDA #0                 \ Set A = 0 - A, which we can do this time using a
     SBC U                  \ subtraction with the C flag clear
    
     ORA #%10000000         \ We now set the sign bit of A, so that the EOR on the
                            \ next line will give the result the opposite sign to
                            \ argument A (as T contains the sign bit of argument
                            \ A). This is the same as giving the result the same
                            \ sign as argument S (as A and S have different signs),
                            \ which is what we want, as S has the larger absolute
                            \ value
    
    .MU9
    
     EOR T                  \ If we get here from the BCS above, then |A| >= |S|,
                            \ so we want to give the result the same sign as
                            \ argument A, so if argument A was negative, we flip
                            \ the sign of the result with an EOR (to make it
                            \ negative)
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TIS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (A ?) = (-X * A + (S R)) / 96
      Deep dive: [Shift-and-subtract division](https://elite.bbcelite.com/deep_dives/shift-and-subtract_division.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tis1.md)
     References: This subroutine is called as follows:
                 * [TIDY](../main/subroutine/tidy.md) calls TIS1
    
    
    
    
    
    * * *
    
    
     Calculate the following expression between sign-magnitude numbers, ignoring
     the low byte of the result:
    
       (A ?) = (-X * A + (S R)) / 96
    
     This uses the same shift-and-subtract algorithm as TIS2, just with the
     quotient A hard-coded to 96.
    
    
    
    * * *
    
    
     Returns:
    
       Q                    Gets set to the value of argument X
    
    
    
    
    .TIS1
    
     STX Q                  \ Set Q = X
    
     EOR #%10000000         \ Flip the sign bit in A
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
                            \           = X * -A + (S R)
    
    .DVID96
    
     TAX                    \ Set T to the sign bit of the result
     AND #%10000000
     STA T
    
     TXA                    \ Set A to the high byte of the result with the sign bit
     AND #%01111111         \ cleared, so (A ?) = |X * A + (S R)|
    
                            \ The following is identical to TIS2, except Q is
                            \ hard-coded to 96, so this does A = A / 96
    
     LDX #254               \ Set T1 to have bits 1-7 set, so we can rotate through
     STX T1                 \ 7 loop iterations, getting a 1 each time, and then
                            \ getting a 0 on the 8th iteration... and we can also
                            \ use T1 to catch our result bits into bit 0 each time
    
    .DVL3
    
     ASL A                  \ Shift A to the left
    
     CMP #96                \ If A < 96 skip the following subtraction
     BCC DV4
    
     SBC #96                \ Set A = A - 96
                            \
                            \ Going into this subtraction we know the C flag is
                            \ set as we passed through the BCC above, and we also
                            \ know that A >= 96, so the C flag will still be set
                            \ once we are done
    
    .DV4
    
     ROL T1                 \ Rotate the counter in T1 to the left, and catch the
                            \ result bit into bit 0 (which will be a 0 if we didn't
                            \ do the subtraction, or 1 if we did)
    
     BCS DVL3               \ If we still have set bits in T1, loop back to DVL3 to
                            \ do the next iteration of 7
    
     LDA T1                 \ Fetch the result from T1 into A
    
     ORA T                  \ Give A the sign of the result that we stored above
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DV42                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (P R) = 256 * DELTA / z_hi
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dv42.md)
     References: This subroutine is called as follows:
                 * [STARS1](../main/subroutine/stars1.md) calls DV42
                 * [STARS6](../main/subroutine/stars6.md) calls DV42
    
    
    
    
    
    * * *
    
    
     Calculate the following division and remainder:
    
       P = DELTA / (the Y-th stardust particle's z_hi coordinate)
    
       R = remainder as a fraction of A, where 1.0 = 255
    
     Another way of saying the above is this:
    
       (P R) = 256 * DELTA / z_hi
    
     DELTA is a value between 1 and 40, and the minimum z_hi is 16 (dust particles
     are removed at lower values than this), so this means P is between 0 and 2
     (as 40 / 16 = 2.5, so the maximum result is P = 2 and R = 128.
    
     This uses the same shift-and-subtract algorithm as TIS2, but this time we
     keep the remainder.
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The number of the stardust particle to process
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .DV42
    
     LDA SZ,Y               \ Fetch the Y-th dust particle's z_hi coordinate into A
    
                            \ Fall through into DV41 to do:
                            \
                            \   (P R) = 256 * DELTA / A
                            \         = 256 * DELTA / Y-th stardust particle's z_hi
    
    
    
    
           Name: DV41                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (P R) = 256 * DELTA / A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dv41.md)
     References: This subroutine is called as follows:
                 * [STARS2](../main/subroutine/stars2.md) calls DV41
    
    
    
    
    
    * * *
    
    
     Calculate the following division and remainder:
    
       P = DELTA / A
    
       R = remainder as a fraction of A, where 1.0 = 255
    
     Another way of saying the above is this:
    
       (P R) = 256 * DELTA / A
    
     This uses the same shift-and-subtract algorithm as TIS2, but this time we
     keep the remainder.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .DV41
    
     STA Q                  \ Store A in Q
    
     LDA DELTA              \ Fetch the speed from DELTA into A
    
                            \ Fall through into DVID4 to do:
                            \
                            \   (P R) = 256 * A / Q
                            \         = 256 * DELTA / A
    
    
    
    
           Name: DVID4                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (P R) = 256 * A / Q
      Deep dive: [Shift-and-subtract division](https://elite.bbcelite.com/deep_dives/shift-and-subtract_division.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dvid4.md)
     Variations: See [code variations](../related/compare/main/subroutine/dvid4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOEXP](../main/subroutine/doexp.md) calls DVID4
                 * [SPS2](../main/subroutine/sps2.md) calls DVID4
    
    
    
    
    
    * * *
    
    
     Calculate the following division and remainder:
    
       P = A / Q
    
       R = remainder as a fraction of Q, where 1.0 = 255
    
     Another way of saying the above is this:
    
       (P R) = 256 * A / Q
    
     This uses the same shift-and-subtract algorithm as TIS2, but this time we
     keep the remainder and the loop is unrolled.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .DVID4
    
    \LDX #8                 \ This instruction is commented out in the original
                            \ source
    
     ASL A                  \ Shift A left and store in P (we will build the result
     STA P                  \ in P)
    
     LDA #0                 \ Set A = 0 for us to build a remainder
    
    \.DVL4                  \ This label is commented out in the original source
    
                            \ We now repeat the following five instruction block
                            \ eight times, one for each bit in P. In the BBC Micro
                            \ cassette and disc versions of Elite the following is
                            \ done with a loop, but it is marginally faster to
                            \ unroll the loop and have eight copies of the code,
                            \ though it does take up a bit more memory (though that
                            \ isn't a big concern when you have a 6502 Second
                            \ Processor)
    
     ROL A                  \ Shift A to the left
    
     CMP Q                  \ If A < Q skip the following subtraction
     BCC P%+4
    
     SBC Q                  \ A >= Q, so set A = A - Q
    
     ROL P                  \ Shift P to the left, pulling the C flag into bit 0
    
     ROL A                  \ Repeat for the second time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the third time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the fourth time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the fifth time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the sixth time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the seventh time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     ROL A                  \ Repeat for the eighth time
     CMP Q
     BCC P%+4
     SBC Q
     ROL P
    
     LDX #0                 \ Set X = 0 so this unrolled version of DVID4 also
                            \ returns X = 0
    
     STA widget             \ This contains the code from the LL28+4 routine, so
     TAX                    \ this section is exactly equivalent to a JMP LL28+4
     BEQ LLfix22            \ call, but is slightly faster as it's been inlined
     LDA logL,X             \ (so it converts the remainder in A into an integer
     LDX Q                  \ representation of the fractional value A / Q, in R,
     SEC                    \ where 1.0 = 255, and it also clears the C flag
     SBC logL,X
     LDX widget
     LDA log,X
     LDX Q
     SBC log,X
     BCS LL222
     TAX
     LDA alogh,X
    
    .LLfix22
    
     STA R                  \ This is also part of the inline LL28+4 routine
     RTS
    
    .LL222
    
     LDA #255               \ This is also part of the inline LL28+4 routine
     STA R
     RTS
    
    
    
    
           Name: DVID3B2                                                 [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)
      Deep dive: [Shift-and-subtract division](https://elite.bbcelite.com/deep_dives/shift-and-subtract_division.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dvid3b2.md)
     Variations: See [code variations](../related/compare/main/subroutine/dvid3b2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLANET](../main/subroutine/planet.md) calls DVID3B2
                 * [PLS1](../main/subroutine/pls1.md) calls DVID3B2
                 * [PLS6](../main/subroutine/pls6.md) calls DVID3B2
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)
    
     The actual division here is done as an 8-bit calculation using LL31, but this
     routine shifts both the numerator (the top part of the division) and the
     denominator (the bottom part of the division) around to get the multi-byte
     result we want.
    
     Specifically, it shifts both of them to the left as far as possible, keeping a
     tally of how many shifts get done in each one - and specifically, the
     difference in the number of shifts between the top and bottom (as shifting
     both of them once in the same direction won't change the result). It then
     divides the two highest bytes with the simple 8-bit routine in LL31, and
     shifts the result by the difference in the number of shifts, which acts as a
     scale factor to get the correct result.
    
    
    
    * * *
    
    
     Returns:
    
       K(3 2 1 0)           The result of the division
    
       X                    X is preserved
    
    
    
    
    .DVID3B2
    
     STA P+2                \ Set P+2 = A
    
     LDA INWK+6             \ Set Q = z_lo, making sure Q is at least 1
     ORA #1
     STA Q
    
     LDA INWK+7             \ Set R = z_hi
     STA R
    
     LDA INWK+8             \ Set S = z_sign
     STA S
    
    .DVID3B
    
                            \ Given the above assignments, we now want to calculate
                            \ the following to get the result we want:
                            \
                            \   K(3 2 1 0) = P(2 1 0) / (S R Q)
    
     LDA P                  \ Make sure P(2 1 0) is at least 1
     ORA #1
     STA P
    
     LDA P+2                \ Set T to the sign of P+2 * S (i.e. the sign of the
     EOR S                  \ result) and store it in T
     AND #%10000000
     STA T
    
     LDY #0                 \ Set Y = 0 to store the scale factor
    
     LDA P+2                \ Clear the sign bit of P+2, so the division can be done
     AND #%01111111         \ with positive numbers and we'll set the correct sign
                            \ below, once all the maths is done
                            \
                            \ This also leaves A = P+2, which we use below
    
    .DVL9
    
                            \ We now shift (A P+1 P) left until A >= 64, counting
                            \ the number of shifts in Y. This makes the top part of
                            \ the division as large as possible, thus retaining as
                            \ much accuracy as we can.  When we come to return the
                            \ final result, we shift the result by the number of
                            \ places in Y, and in the correct direction
    
     CMP #64                \ If A >= 64, jump down to DV14
     BCS DV14
    
     ASL P                  \ Shift (A P+1 P) to the left
     ROL P+1
     ROL A
    
     INY                    \ Increment the scale factor in Y
    
     BNE DVL9               \ Loop up to DVL9 (this BNE is effectively a JMP, as Y
                            \ will never be zero)
    
    .DV14
    
                            \ If we get here, A >= 64 and contains the highest byte
                            \ of the numerator, scaled up by the number of left
                            \ shifts in Y
    
     STA P+2                \ Store A in P+2, so we now have the scaled value of
                            \ the numerator in P(2 1 0)
    
     LDA S                  \ Set A = |S|
     AND #%01111111
    
    \BMI DV9                \ This label is commented out in the original source
    
    .DVL6
    
                            \ We now shift (S R Q) left until bit 7 of S is set,
                            \ reducing Y by the number of shifts. This makes the
                            \ bottom part of the division as large as possible, thus
                            \ retaining as much accuracy as we can. When we come to
                            \ return the final result, we shift the result by the
                            \ total number of places in Y, and in the correct
                            \ direction, to give us the correct result
                            \
                            \ We set A to |S| above, so the following actually
                            \ shifts (A R Q)
    
     DEY                    \ Decrement the scale factor in Y
    
     ASL Q                  \ Shift (A R Q) to the left
     ROL R
     ROL A
    
     BPL DVL6               \ Loop up to DVL6 to do another shift, until bit 7 of A
                            \ is set and we can't shift left any further
    
    .DV9
    
                            \ We have now shifted both the numerator and denominator
                            \ left as far as they will go, keeping a tally of the
                            \ overall scale factor of the various shifts in Y. We
                            \ can now divide just the two highest bytes to get our
                            \ result
    
     STA Q                  \ Set Q = A, the highest byte of the denominator
    
     LDA #254               \ Set R to have bits 1-7 set, so we can pass this to
     STA R                  \ LL31 to act as the bit counter in the division
    
     LDA P+2                \ Set A to the highest byte of the numerator
    
    .LL31new
    
     ASL A                  \ This contains the code from the LL31 routine, so
     BCS LL29new            \ this section is exactly equivalent to a JSR LL31
     CMP Q                  \ call, but is slightly faster as it's been inlined,
     BCC P%+4               \ so it calculates:
     SBC Q                  \
     ROL R                  \   R = 256 * A / Q
     BCS LL31new            \     = 256 * numerator / denominator
     JMP LL312new
    
    .LL29new
    
     SBC Q                  \ This is also part of the inline LL31 routine
     SEC
     ROL R
     BCS LL31new
     LDA R
    
    .LL312new
    
                            \ The result of our division is now in R, so we just
                            \ need to shift it back by the scale factor in Y
    
     LDA #0                 \ Set K(3 2 1) = 0 to hold the result (we populate K
     STA K+1                \ next)
     STA K+2
     STA K+3
    
     TYA                    \ If Y is positive, jump to DV12
     BPL DV12
    
                            \ If we get here then Y is negative, so we need to shift
                            \ the result R to the left by Y places, and then set the
                            \ correct sign for the result
    
     LDA R                  \ Set A = R
    
    .DVL8
    
     ASL A                  \ Shift (K+3 K+2 K+1 A) left
     ROL K+1
     ROL K+2
     ROL K+3
    
     INY                    \ Increment the scale factor in Y
    
     BNE DVL8               \ Loop back to DVL8 until we have shifted left by Y
                            \ places
    
     STA K                  \ Store A in K so the result is now in K(3 2 1 0)
    
     LDA K+3                \ Set K+3 to the sign in T, which we set above to the
     ORA T                  \ correct sign for the result
     STA K+3
    
     RTS                    \ Return from the subroutine
    
    .DV13
    
                            \ If we get here then Y is zero, so we don't need to
                            \ shift the result R, we just need to set the correct
                            \ sign for the result
    
     LDA R                  \ Store R in K so the result is now in K(3 2 1 0)
     STA K
    
     LDA T                  \ Set K+3 to the sign in T, which we set above to the
     STA K+3                \ correct sign for the result
    
     RTS                    \ Return from the subroutine
    
    .DV12
    
     BEQ DV13               \ We jumped here having set A to the scale factor in Y,
                            \ so this jumps up to DV13 if Y = 0
    
                            \ If we get here then Y is positive and non-zero, so we
                            \ need to shift the result R to the right by Y places
                            \ and then set the correct sign for the result. We also
                            \ know that K(3 2 1) will stay 0, as we are shifting the
                            \ lowest byte to the right, so no set bits will make
                            \ their way into the top three bytes
    
     LDA R                  \ Set A = R
    
    .DVL10
    
     LSR A                  \ Shift A right
    
     DEY                    \ Decrement the scale factor in Y
    
     BNE DVL10              \ Loop back to DVL10 until we have shifted right by Y
                            \ places
    
     STA K                  \ Store the shifted A in K so the result is now in
                            \ K(3 2 1 0)
    
     LDA T                  \ Set K+3 to the sign in T, which we set above to the
     STA K+3                \ correct sign for the result
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: cntr                                                    [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Apply damping to the pitch or roll dashboard indicator
    
    
        Context: See this subroutine [on its own page](../main/subroutine/cntr.md)
     Variations: See [code variations](../related/compare/main/subroutine/cntr.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md) calls cntr
    
    
    
    
    
    * * *
    
    
     Apply damping to the value in X, where X ranges from 1 to 255 with 128 as the
     centre point (so X represents a position on a centre-based dashboard slider,
     such as pitch or roll). If the value is in the left-hand side of the slider
     (1-127) then it bumps the value up by 1 so it moves towards the centre, and
     if it's in the right-hand side, it reduces it by 1, also moving it towards the
     centre.
    
    
    
    
    .cntr
    
     LDA auto               \ If the docking computer is currently activated, jump
     BNE cnt2               \ to cnt2 to skip the following as we always want to
                            \ enable damping for the docking computer
    
     LDA DAMP               \ If DAMP is non-zero, then keyboard damping is not
     BNE RE1                \ enabled, so jump to RE1 to return from the subroutine
    
    .cnt2
    
     TXA                    \ If X < 128, then it's in the left-hand side of the
     BPL BUMP               \ dashboard slider, so jump to BUMP to bump it up by 1,
                            \ to move it closer to the centre
    
     DEX                    \ Otherwise X >= 128, so it's in the right-hand side
     BMI ARSR1              \ of the dashboard slider, so decrement X by 1, and if
                            \ it's still >= 128, jump to ARSR1 to return from the
                            \ subroutine, otherwise fall through to BUMP to undo
                            \ the bump and then return
    
    .BUMP
    
     INX                    \ Bump X up by 1, and if it hasn't overshot the end of
     BNE RE1                \ the dashboard slider, jump to RE1 to return from the
                            \ subroutine, otherwise fall through to REDU to drop
                            \ it down by 1 again
    
    .REDU
    
     DEX                    \ Reduce X by 1, and if we have reached 0 jump up to
     BEQ BUMP               \ BUMP to add 1, because we need the value to be in the
                            \ range 1 to 255
    
    .RE1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: BUMP2                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Bump up the value of the pitch or roll dashboard indicator
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bump2.md)
     Variations: See [code variations](../related/compare/main/subroutine/bump2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOKEY](../main/subroutine/dokey.md) calls BUMP2
                 * [REDU2](../main/subroutine/redu2.md) calls via RE2+2
    
    
    
    
    
    * * *
    
    
     Increase ("bump up") X by A, where X is either the current rate of pitch or
     the current rate of roll.
    
     The rate of pitch or roll ranges from 1 to 255 with 128 as the centre point.
     This is the amount by which the pitch or roll is currently changing, so 1
     means it is decreasing at the maximum rate, 128 means it is not changing,
     and 255 means it is increasing at the maximum rate. These values correspond
     to the line on the DC or RL indicators on the dashboard, with 1 meaning full
     left, 128 meaning the middle, and 255 meaning full right.
    
     If bumping up X would push it past 255, then X is set to 255.
    
     If keyboard auto-recentre is configured and the result is less than 128, we
     bump X up to the mid-point, 128. This is the equivalent of having a roll or
     pitch in the left half of the indicator, when increasing the roll or pitch
     should jump us straight to the mid-point.
    
    
    
    * * *
    
    
     Other entry points:
    
       RE2+2                Restore A from T and return from the subroutine
    
    
    
    
    .BUMP2
    
     STA T                  \ Store argument A in T so we can restore it later
    
     TXA                    \ Copy argument X into A
    
     CLC                    \ Clear the C flag so we can do addition without the
                            \ C flag affecting the result
    
     ADC T                  \ Set X = A = argument X + argument A
     TAX
    
     BCC RE2                \ If the C flag is clear, then we didn't overflow, so
                            \ jump to RE2 to auto-recentre and return the result
    
     LDX #255               \ We have an overflow, so set X to the maximum possible
                            \ value of 255
    
    .RE2
    
     BPL djd1               \ If X has bit 7 clear (i.e. the result < 128), then
                            \ jump to djd1 in routine REDU2 to do an auto-recentre,
                            \ if configured, because the result is on the left side
                            \ of the centre point of 128
    
                            \ Jumps to RE2+2 end up here
    
     LDA T                  \ Restore the original argument A from T into A
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: REDU2                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Reduce the value of the pitch or roll dashboard indicator
    
    
        Context: See this subroutine [on its own page](../main/subroutine/redu2.md)
     Variations: See [code variations](../related/compare/main/subroutine/redu2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOKEY](../main/subroutine/dokey.md) calls REDU2
                 * [BUMP2](../main/subroutine/bump2.md) calls via djd1
    
    
    
    
    
    * * *
    
    
     Reduce X by A, where X is either the current rate of pitch or the current
     rate of roll.
    
     The rate of pitch or roll ranges from 1 to 255 with 128 as the centre point.
     This is the amount by which the pitch or roll is currently changing, so 1
     means it is decreasing at the maximum rate, 128 means it is not changing,
     and 255 means it is increasing at the maximum rate. These values correspond
     to the line on the DC or RL indicators on the dashboard, with 1 meaning full
     left, 128 meaning the middle, and 255 meaning full right.
    
     If reducing X would bring it below 1, then X is set to 1.
    
     If keyboard auto-recentre is configured and the result is greater than 128, we
     reduce X down to the mid-point, 128. This is the equivalent of having a roll
     or pitch in the right half of the indicator, when decreasing the roll or pitch
     should jump us straight to the mid-point.
    
    
    
    * * *
    
    
     Other entry points:
    
       djd1                 Auto-recentre the value in X, if keyboard auto-recentre
                            is configured
    
    
    
    
    .REDU2
    
     STA T                  \ Store argument A in T so we can restore it later
    
     TXA                    \ Copy argument X into A
    
     SEC                    \ Set the C flag so we can do subtraction without the
                            \ C flag affecting the result
    
     SBC T                  \ Set X = A = argument X - argument A
     TAX
    
     BCS RE3                \ If the C flag is set, then we didn't underflow, so
                            \ jump to RE3 to auto-recentre and return the result
    
     LDX #1                 \ We have an underflow, so set X to the minimum possible
                            \ value, 1
    
    .RE3
    
     BPL RE2+2              \ If X has bit 7 clear (i.e. the result < 128), then
                            \ jump to RE2+2 above to return the result as is,
                            \ because the result is on the left side of the centre
                            \ point of 128, so we don't need to auto-centre
    
    .djd1
    
                            \ If we get here, then we need to apply auto-recentre,
                            \ if it is configured
    
     LDA DJD                \ If keyboard auto-recentre is disabled, then
     BNE RE2+2              \ jump to RE2+2 to restore A and return
    
     LDX #128               \ If we get here then keyboard auto-recentre is enabled,
     BMI RE2+2              \ so set X to 128 (the middle of our range) and jump to
                            \ RE2+2 to restore A and return from the subroutine
                            \ (this BMI is effectively a JMP as bit 7 of X is always
                            \ set)
    
    
    
    
           Name: ARCTAN                                                  [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate A = arctan(P / Q)
      Deep dive: [The sine, cosine and arctan tables](https://elite.bbcelite.com/deep_dives/the_sine_cosine_and_arctan_tables.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/arctan.md)
     Variations: See [code variations](../related/compare/main/subroutine/arctan.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLS4](../main/subroutine/pls4.md) calls ARCTAN
                 * [cntr](../main/subroutine/cntr.md) calls via ARSR1
                 * [LASLI](../main/subroutine/lasli.md) calls via ARSR1
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       A = arctan(P / Q)
    
     In other words, this finds the angle in the right-angled triangle where the
     opposite side to angle A is length P and the adjacent side to angle A has
     length Q, so:
    
       tan(A) = P / Q
    
     The result in A is an integer representing the angle in radians. The routine
     returns values in the range 0 to 128, which covers 0 to 180 degrees (or 0 to
     PI radians).
    
    
    
    * * *
    
    
     Other entry points:
    
       ARSR1                Contains an RTS
    
    
    
    
    .ARCTAN
    
     LDA P                  \ Set T1 = P EOR Q, which will have the sign of P * Q
     EOR Q                  \
    \AND #%10000000         \ The AND is commented out in the original source
     STA T1
    
     LDA Q                  \ If Q = 0, jump to AR2 to return a right angle
     BEQ AR2
    
     ASL A                  \ Set Q = |Q| * 2 (this is a quick way of clearing the
     STA Q                  \ sign bit, and we don't need to shift right again as we
                            \ only ever use this value in the division with |P| * 2,
                            \ which we set next)
    
     LDA P                  \ Set A = |P| * 2
     ASL A
    
     CMP Q                  \ If A >= Q, i.e. |P| > |Q|, jump to AR1 to swap P
     BCS AR1                \ and Q around, so we can still use the lookup table
    
     JSR ARS1               \ Call ARS1 to set the following from the lookup table:
                            \
                            \   A = arctan(A / Q)
                            \     = arctan(|P / Q|)
    
     SEC                    \ Set the C flag so the SBC instruction in AR3 will be
                            \ correct, should we jump there
    
    .AR4
    
     LDX T1                 \ If T1 is negative, i.e. P and Q have different signs,
     BMI AR3                \ jump down to AR3 to return arctan(-|P / Q|)
    
     RTS                    \ Otherwise P and Q have the same sign, so our result is
                            \ correct and we can return from the subroutine
    
    .AR1
    
                            \ We want to calculate arctan(t) where |t| > 1, so we
                            \ can use the calculation described in the documentation
                            \ for the ACT table, i.e. 64 - arctan(1 / t)
    
     LDX Q                  \ Swap the values in Q and P, using the fact that we
     STA Q                  \ called AR1 with A = P
     STX P                  \
     TXA                    \ This also sets A = P (which now contains the original
                            \ argument |Q|)
    
     JSR ARS1               \ Call ARS1 to set the following from the lookup table:
                            \
                            \   A = arctan(A / Q)
                            \     = arctan(|Q / P|)
                            \     = arctan(1 / |P / Q|)
    
     STA T                  \ Set T = 64 - T
     LDA #64
     SBC T
    
     BCS AR4                \ Jump to AR4 to continue the calculation (this BCS is
                            \ effectively a JMP as the subtraction will never
                            \ underflow, as ARS1 returns values in the range 0-31)
    
    .AR2
    
                            \ If we get here then Q = 0, so tan(A) = infinity and
                            \ A is a right angle, or 0.25 of a circle. We allocate
                            \ 255 to a full circle, so we should return 63 for a
                            \ right angle
    
     LDA #63                \ Set A to 63, to represent a right angle
    
     RTS                    \ Return from the subroutine
    
    .AR3
    
                            \ A contains arctan(|P / Q|) but P and Q have different
                            \ signs, so we need to return arctan(-|P / Q|), using
                            \ the calculation described in the documentation for the
                            \ ACT table, i.e. 128 - A
    
     STA T                  \ Set A = 128 - A
     LDA #128               \
    \SEC                    \ The SEC instruction is commented out in the original
     SBC T                  \ source, and isn't required as we did a SEC before
                            \ calling AR3
    
     RTS                    \ Return from the subroutine
    
    .ARS1
    
                            \ This routine fetches arctan(A / Q) from the ACT table,
                            \ so A will be set to an integer in the range 0 to 31
                            \ that represents an angle from 0 to 45 degrees (or 0 to
                            \ PI / 4 radians)
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
    
     LDA R                  \ Set X = R / 8
     LSR A                  \       = 32 * A / Q
     LSR A                  \
     LSR A                  \ so X has the value t * 32 where t = A / Q, which is
     TAX                    \ what we need to look up values in the ACT table
    
     LDA ACT,X              \ Fetch ACT+X from the ACT table into A, so now:
                            \
                            \   A = value in ACT + X
                            \     = value in ACT + (32 * A / Q)
                            \     = arctan(A / Q)
    
    .ARSR1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LASLI                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw the laser lines for when we fire our lasers
    
    
        Context: See this subroutine [on its own page](../main/subroutine/lasli.md)
     Variations: See [code variations](../related/compare/main/subroutine/lasli.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls LASLI
                 * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md) calls via LASLI2
    
    
    
    
    
    * * *
    
    
     Draw the laser lines, aiming them to slightly different place each time so
     they appear to flicker and dance. Also heat up the laser temperature and drain
     some energy.
    
    
    
    * * *
    
    
     Other entry points:
    
       LASLI2               Just draw the current laser lines without moving the
                            centre point, draining energy or heating up. This has
                            the effect of removing the lines from the screen
    
       LASLI-1              Contains an RTS
    
    
    
    
    .LASLI
    
     JSR DORND              \ Set A and X to random numbers
    
     AND #7                 \ Restrict A to a random value in the range 0 to 7
    
     ADC #Y-4               \ Set LASY to four pixels above the centre of the
     STA LASY               \ screen (#Y), plus our random number, so the laser
                            \ dances above and below the centre point
    
     JSR DORND              \ Set A and X to random numbers
    
     AND #7                 \ Restrict A to a random value in the range 0 to 7
    
     ADC #X-4               \ Set LASX to four pixels left of the centre of the
     STA LASX               \ screen (#X), plus our random number, so the laser
                            \ dances to the left and right of the centre point
    
     LDA GNTMP              \ Add 8 to the laser temperature in GNTMP
     ADC #8
     STA GNTMP
    
     JSR DENGY              \ Call DENGY to deplete our energy banks by 1
    
    .LASLI2
    
     LDA QQ11               \ If this is not a space view (i.e. QQ11 is non-zero)
     BNE ARSR1              \ then jump to MA9 to return from the main flight loop
                            \ (as ARSR1 is an RTS)
    
     LDA #RED               \ Switch to colour 2, which is red in the space view
     STA COL
    
     LDA #32                \ Set A = 32 and Y = 224 for the first set of laser
     LDY #224               \ lines (the wider pair of lines)
    
     DEC LASY               \ Decrement the y-coordinate of the centre point to move
     DEC LASY               \ it up the screen by two pixels for the top set of
                            \ lines, so the wider set of lines aim slightly higher
                            \ than the narrower set
    
     JSR las                \ Call las below to draw the first set of laser lines
    
     INC LASY               \ Increment the y-coordinate of the centre point to put
     INC LASY               \ it back to the original position
    
     LDA #48                \ Fall through into las with A = 48 and Y = 208 to draw
     LDY #208               \ a second set of lines (the narrower pair)
    
                            \ The following routine draws two laser lines, one from
                            \ the centre point down to point A on the bottom row,
                            \ and the other from the centre point down to point Y
                            \ on the bottom row. We therefore get lines from the
                            \ centre point to points 32, 48, 208 and 224 along the
                            \ bottom row, giving us the triangular laser effect
                            \ we're after
    
    .las
    
     STA X2                 \ Set X2 = A
    
     LDA LASX               \ Set (X1, Y1) to the random centre point we set above
     STA X1
     LDA LASY
     STA Y1
    
     LDA #2*Y-1             \ Set Y2 = 2 * #Y - 1. The constant #Y is 96, the
     STA Y2                 \ y-coordinate of the mid-point of the space view, so
                            \ this sets Y2 to 191, the y-coordinate of the bottom
                            \ pixel row of the space view
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2), so that's from
                            \ the centre point to (A, 191)
    
     LDA LASX               \ Set (X1, Y1) to the random centre point we set above
     STA X1
     LDA LASY
     STA Y1
    
     STY X2                 \ Set X2 = Y
    
     LDA #2*Y-1             \ Set Y2 = 2 * #Y - 1, the y-coordinate of the bottom
     STA Y2                 \ pixel row of the space view (as before)
    
     JMP LOIN               \ Draw a line from (X1, Y1) to (X2, Y2), so that's from
                            \ the centre point to (Y, 191), and return from
                            \ the subroutine using a tail call
    
    
    
    
           Name: PDESC                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Print the system's extended description or a mission 1 directive
      Deep dive: [Extended system descriptions](https://elite.bbcelite.com/deep_dives/extended_system_descriptions.html)
                 [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pdesc.md)
     Variations: See [code variations](../related/compare/main/subroutine/pdesc.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT25](../main/subroutine/tt25.md) calls PDESC
    
    
    
    
    
    * * *
    
    
     This prints a specific system's extended description. This is called the "pink
     volcanoes string" in a comment in the original source, and the "goat soup"
     recipe by Ian Bell on his website (where he also refers to the species string
     as the "pink felines" string).
    
     For some special systems, when you are docked at them, the procedurally
     generated extended description is overridden and a text token from the RUTOK
     table is shown instead. If mission 1 is in progress, then a number of systems
     along the route of that mission's story will show custom mission-related
     directives in place of that system's normal "goat soup" phrase.
    
    
    
    * * *
    
    
     Arguments:
    
       ZZ                   The system number (0-255)
    
    
    
    
    .PDESC
    
     LDA QQ8                \ If either byte in QQ18(1 0) is non-zero, meaning that
     ORA QQ8+1              \ the distance from the current system to the selected
     BNE PD1                \ is non-zero, jump to PD1 to show the standard "goat
                            \ soup" description
    
     LDA QQ12               \ If QQ12 does not have bit 7 set, which means we are
     BPL PD1                \ not docked, jump to PD1 to show the standard "goat
                            \ soup" description
    
                            \ If we get here, then the current system is the same as
                            \ the selected system and we are docked, so now to check
                            \ whether there is a special override token for this
                            \ system
    
     LDY #NRU%              \ Set Y as a loop counter as we work our way through the
                            \ system numbers in RUPLA, starting at NRU% (which is
                            \ the number of entries in RUPLA, 26) and working our
                            \ way down to 1
    
    .PDL1
    
     LDA RUPLA-1,Y          \ Fetch the Y-th byte from RUPLA-1 into A (we use
                            \ RUPLA-1 because Y is looping from 26 to 1)
    
     CMP ZZ                 \ If A doesn't match the system whose description we
     BNE PD2                \ are printing (in ZZ), jump to PD2 to keep looping
                            \ through the system numbers in RUPLA
    
                            \ If we get here we have found a match for this system
                            \ number in RUPLA
    
     LDA RUGAL-1,Y          \ Fetch the Y-th byte from RUGAL-1 into A
    
     AND #%01111111         \ Extract bits 0-6 of A
    
     CMP GCNT               \ If the result does not equal the current galaxy
     BNE PD2                \ number, jump to PD2 to keep looping through the system
                            \ numbers in RUPLA
    
     LDA RUGAL-1,Y          \ Fetch the Y-th byte from RUGAL-1 into A, once again
    
     BMI PD3                \ If bit 7 is set, jump to PD3 to print the extended
                            \ token in A from the second table in RUTOK
    
     LDA TP                 \ Fetch bit 0 of TP into the C flag, and skip to PD1 if
     LSR A                  \ it is clear (i.e. if mission 1 is not in progress) to
     BCC PD1                \ print the "goat soup" extended description
    
                            \ If we get here then mission 1 is in progress, so we
                            \ print out the corresponding token from RUTOK
    
     JSR MT14               \ Call MT14 to switch to justified text
    
     LDA #1                 \ Set A = 1 so that extended token 1 (an empty string)
                            \ gets printed below instead of token 176, followed by
                            \ the Y-th token in RUTOK
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &B0, or BIT &B0A9, which does nothing apart
                            \ from affect the flags
    
    .PD3
    
     LDA #176               \ Print extended token 176 ("{lower case}{justify}
     JSR DETOK2             \ {single cap}")
    
     TYA                    \ Print the extended token in Y from the second table
     JSR DETOK3             \ in RUTOK
    
     LDA #177               \ Set A = 177 so when we jump to PD4 in the next
                            \ instruction, we print token 177 (".{cr}{left align}")
    
     BNE PD4                \ Jump to PD4 to print the extended token in A and
                            \ return from the subroutine using a tail call
    
    .PD2
    
     DEY                    \ Decrement the byte counter in Y
    
     BNE PDL1               \ Loop back to check the next byte in RUPLA until we
                            \ either find a match for the system in ZZ, or we fall
                            \ through into the "goat soup" extended description
                            \ routine
    
    .PD1
    
                            \ We now print the "goat soup" extended description
    
     LDX #3                 \ We now want to seed the random number generator with
                            \ the s1 and s2 16-bit seeds from the current system, so
                            \ we get the same extended description for each system
                            \ every time we call PDESC, so set a counter in X for
                            \ copying 4 bytes
    
    .PDL1K                  \ This label is a duplicate of the label above
                            \
                            \ In the original source this label is PDL1, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to PDL1K
    
     LDA QQ15+2,X           \ Copy QQ15+2 to QQ15+5 (s1 and s2) to RAND to RAND+3
     STA RAND,X
    
     DEX                    \ Decrement the loop counter
    
     BPL PDL1K              \ Loop back to PDL1K until we have copied all
    
     LDA #5                 \ Set A = 5, so we print extended token 5 in the next
                            \ instruction ("{lower case}{justify}{single cap}[86-90]
                            \ IS [140-144].{cr}{left align}"
    
    .PD4
    
     JMP DETOK              \ Print the extended token given in A, and return from
                            \ the subroutine using a tail call
    
    
    
    
           Name: BRIEF2                                                  [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Start mission 2
      Deep dive: [The Thargoid Plans mission](https://elite.bbcelite.com/deep_dives/the_thargoid_plans_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/brief2.md)
     References: This subroutine is called as follows:
                 * [DOENTRY](../main/subroutine/doentry.md) calls BRIEF2
    
    
    
    
    
    
    .BRIEF2
    
     LDA TP                 \ Set bit 2 of TP to indicate mission 2 is in progress
     ORA #%00000100         \ but plans have not yet been picked up
     STA TP
    
     LDA #11                \ Set A = 11 so the call to BRP prints extended token 11
                            \ (the initial contact at the start of mission 2, asking
                            \ us to head for Ceerdi for a mission briefing)
    
                            \ Fall through into BRP to print the extended token in A
                            \ and show the Status Mode screen
    
    
    
    
           Name: BRP                                                     [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Print an extended token and show the Status Mode screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/brp.md)
     Variations: See [code variations](../related/compare/main/subroutine/brp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF3](../main/subroutine/brief3.md) calls BRP
                 * [DEBRIEF](../main/subroutine/debrief.md) calls BRP
                 * [DEBRIEF2](../main/subroutine/debrief2.md) calls BRP
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       BAYSTEP              Go to the docking bay (i.e. show the Status Mode screen)
    
    
    
    
    .BRP
    
     LDX #CYAN              \ Switch to colour 3, which is white or cyan
     STX COL
    
     JSR DETOK              \ Print the extended token in A
    
    .BAYSTEP
    
     JMP BAY                \ Jump to BAY to go to the docking bay (i.e. show the
                            \ Status Mode screen) and return from the subroutine
                            \ using a tail call
    
    
    
    
           Name: BRIEF3                                                  [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Receive the briefing and plans for mission 2
      Deep dive: [The Thargoid Plans mission](https://elite.bbcelite.com/deep_dives/the_thargoid_plans_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/brief3.md)
     References: This subroutine is called as follows:
                 * [DOENTRY](../main/subroutine/doentry.md) calls BRIEF3
    
    
    
    
    
    
    .BRIEF3
    
     LDA TP                 \ Set bits 1 and 3 of TP to indicate that mission 1 is
     AND #%11110000         \ complete, and mission 2 is in progress and the plans
     ORA #%00001010         \ have been picked up
     STA TP
    
     LDA #222               \ Set A = 222 so the call to BRP prints extended token
                            \ 222 (the briefing for mission 2 where we pick up the
                            \ plans we need to take to Birera)
    
     BNE BRP                \ Jump to BRP to print the extended token in A and show
                            \ the Status Mode screen), returning from the subroutine
                            \ using a tail call (this BNE is effectively a JMP as A
                            \ is never zero)
    
    
    
    
           Name: DEBRIEF2                                                [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Finish mission 2
      Deep dive: [The Thargoid Plans mission](https://elite.bbcelite.com/deep_dives/the_thargoid_plans_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/debrief2.md)
     References: This subroutine is called as follows:
                 * [DOENTRY](../main/subroutine/doentry.md) calls DEBRIEF2
    
    
    
    
    
    
    .DEBRIEF2
    
     LDA TP                 \ Set bit 2 of TP to indicate mission 2 is complete (so
     ORA #%00000100         \ both bits 2 and 3 are now set)
     STA TP
    
     LDA #2                 \ Set ENGY to 2 so our energy banks recharge at a faster
     STA ENGY               \ rate, as our mission reward is a special navy energy
                            \ unit that recharges at a rate of 3 units of energy on
                            \ each iteration of the main loop, compared to a rate of
                            \ 2 units of energy for the standard energy unit
    
     INC TALLY+1            \ Award 256 kill points for completing the mission
    
     LDA #223               \ Set A = 223 so the call to BRP prints extended token
                            \ 223 (the thank you message at the end of mission 2)
    
     BNE BRP                \ Jump to BRP to print the extended token in A and show
                            \ the Status Mode screen), returning from the subroutine
                            \ using a tail call (this BNE is effectively a JMP as A
                            \ is never zero)
    
    
    
    
           Name: DEBRIEF                                                 [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Finish mission 1
      Deep dive: [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/debrief.md)
     Variations: See [code variations](../related/compare/main/subroutine/debrief.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOENTRY](../main/subroutine/doentry.md) calls DEBRIEF
                 * [BRIEF](../main/subroutine/brief.md) calls via BRPS
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       BRPS                 Print the extended token in A, show the Status Mode
                            screen and return from the subroutine
    
    
    
    
    .DEBRIEF
    
     LSR TP                 \ Clear bit 0 of TP to indicate that mission 1 is no
     ASL TP                 \ longer in progress, as we have completed it
    
    \INC TALLY+1            \ This instruction is commented out in the original
                            \ source
    
     LDX #LO(50000)         \ Increase our cash reserves by the generous mission
     LDY #HI(50000)         \ reward of 5,000 CR
     JSR MCASH
    
     LDA #15                \ Set A = 15 so the call to BRP prints extended token 15
                            \ (the thank you message at the end of mission 1)
    
    .BRPS
    
     BNE BRP                \ Jump to BRP to print the extended token in A and show
                            \ the Status Mode screen, returning from the subroutine
                            \ using a tail call (this BNE is effectively a JMP as A
                            \ is never zero)
    
    
    
    
           Name: TBRIEF                                                  [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Start mission 3
      Deep dive: [The Trumbles mission](https://elite.bbcelite.com/deep_dives/the_trumbles_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tbrief.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    \.TBRIEF                \ These instructions are commented out in the original
    \                       \ source (they are the checks for the Trumble mission,
    \LDA TP                 \ which is not present in the Master version)
    \ORA #%00010000
    \STA TP
    \
    \LDA #199
    \JSR DETOK
    \
    \JSR YESNO
    \
    \BCC BAYSTEP
    \
    \LDY #HI(50000)
    \LDX #LO(50000)
    \JSR LCASH
    \
    \INC TRIBBLE
    \
    \JMP BAY
    
    
    
    
           Name: BRIEF                                                   [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Start mission 1 and show the mission briefing
      Deep dive: [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/brief.md)
     Variations: See [code variations](../related/compare/main/subroutine/brief.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOENTRY](../main/subroutine/doentry.md) calls BRIEF
    
    
    
    
    
    * * *
    
    
     This routine does the following:
    
       * Clear the screen
       * Display "INCOMING MESSAGE" in the middle of the screen
       * Wait for 2 seconds
       * Clear the screen
       * Show the Constrictor rolling and pitching in the middle of the screen
       * Do this for 64 loop iterations
       * Move the ship away from us and up until it's near the top of the screen
       * Show the mission 1 briefing in extended token 10
    
     The mission briefing ends with a "{display ship, wait for key press}" token,
     which calls the PAUSE routine. This continues to display the rotating ship,
     waiting until a key is pressed, and then removes the ship from the screen.
    
    
    
    
    .BRIEF
    
     LSR TP                 \ Set bit 0 of TP to indicate that mission 1 is now in
     SEC                    \ progress
     ROL TP
    
     JSR BRIS               \ Call BRIS to clear the screen, display "INCOMING
                            \ MESSAGE" and wait for 2 seconds
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     LDA #CON               \ Set the ship type in TYPE to the Constrictor
     STA TYPE
    
     JSR NWSHP              \ Add a new Constrictor to the local bubble (in this
                            \ case, the briefing screen)
    
     LDA #1                 \ Move the text cursor to column 1
     JSR DOXC
    
     STA INWK+7             \ Set z_hi = 1, the distance at which we show the
                            \ rotating ship
    
     LDA #13                \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 13 (rotating
                            \ ship view)
    
     LDA #64                \ Set the main loop counter to 64, so the ship rotates
     STA MCNT               \ for 64 iterations through MVEIT
    
    .BRL1
    
     LDX #%01111111         \ Set the ship's roll counter to a positive roll that
     STX INWK+29            \ doesn't dampen (a clockwise roll)
    
     STX INWK+30            \ Set the ship's pitch counter to a positive pitch that
                            \ doesn't dampen (a diving pitch)
    
     JSR LL9                \ Draw the ship on screen
    
     JSR MVEIT              \ Call MVEIT to rotate the ship in space
    
     DEC MCNT               \ Decrease the counter in MCNT
    
     BNE BRL1               \ Loop back to keep moving the ship until we have done
                            \ all 64 iterations
    
    .BRL2
    
     LSR INWK               \ Halve x_lo so the Constrictor moves towards the centre
    
     INC INWK+6             \ Increment z_lo so the Constrictor moves away from us
    
     BEQ BR2                \ If z_lo = 0 (i.e. it just went past 255), jump to BR2
                            \ to show the briefing
    
     INC INWK+6             \ Increment z_lo so the Constrictor moves a bit further
                            \ away from us
    
     BEQ BR2                \ If z_lo = 0 (i.e. it just went past 255), jump out of
                            \ the loop to BR2 to stop moving the ship up the screen
                            \ and show the briefing
    
     LDX INWK+3             \ Set X = y_lo + 1
     INX
    
     CPX #120               \ If X < 120 then skip the next instruction
     BCC P%+4
    
     LDX #120               \ X is bigger than 120, so set X = 120 so that X has a
                            \ maximum value of 120
    
     STX INWK+3             \ Set y_lo = X
                            \          = y_lo + 1
                            \
                            \ so the ship moves up the screen (as space coordinates
                            \ have the y-axis going up)
    
     JSR LL9                \ Draw the ship on screen
    
     JSR MVEIT              \ Call MVEIT to move and rotate the ship in space
    
     DEC MCNT               \ Decrease the counter in MCNT
    
     JMP BRL2               \ Loop back to keep moving the ship up the screen and
                            \ away from us
    
    .BR2
    
     INC INWK+7             \ Increment z_hi, to keep the ship at the same distance
                            \ as we just incremented z_lo past 255
    
     JSR PAS1               \ Call PAS1 to display the rotating ship at space
                            \ coordinates (0, 112, 256) and scan the keyboard,
                            \ returning the ASCII code of the key in X (or 0 for no
                            \ key press)
    
     LDA #10                \ Set A = 10 so the call to BRP prints extended token 10
                            \ (the briefing for mission 1 where we find out all
                            \ about the stolen Constrictor)
    
     BNE BRPS               \ Jump to BRP via BRPS to print the extended token in A
                            \ and show the Status Mode screen, returning from the
                            \ subroutine using a tail call (this BNE is effectively
                            \ a JMP as A is never zero)
    
    
    
    
           Name: BRIS                                                    [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Clear the screen, display "INCOMING MESSAGE" and wait for 2
                 seconds
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bris.md)
     References: This subroutine is called as follows:
                 * [BRIEF](../main/subroutine/brief.md) calls BRIS
                 * [JMTB](../main/variable/jmtb.md) calls BRIS
    
    
    
    
    
    
    .BRIS
    
     LDA #216               \ Print extended token 216 ("{clear screen}{tab 6}{move
     JSR DETOK              \ to row 10, white, lower case}{white}{all caps}INCOMING
                            \ MESSAGE"
    
     LDY #100               \ Wait for 100/50 of a second (2 seconds) and return
     JMP DELAY              \ from the subroutine using a tail call
    
    
    
    
           Name: PAUSE                                                   [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Display a rotating ship, waiting until a key is pressed, then
                 remove the ship from the screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pause.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls PAUSE
    
    
    
    
    
    
    .PAUSE
    
     JSR PAS1               \ Call PAS1 to display the rotating ship at space
                            \ coordinates (0, 112, 256) and scan the keyboard,
                            \ returning the internal key number in X (or 0 for no
                            \ key press)
    
     BNE PAUSE              \ If a key was already being held down when we entered
                            \ this routine, keep looping back up to PAUSE, until
                            \ the key is released
    
    .PAL1
    
     JSR PAS1               \ Call PAS1 to display the rotating ship at space
                            \ coordinates (0, 112, 256) and scan the keyboard,
                            \ returning the internal key number in X (or 0 for no
                            \ key press)
    
     BEQ PAL1               \ Keep looping up to PAL1 until a key is pressed
    
     LDA #0                 \ Set the ship's AI flag to 0 (no AI) so it doesn't get
     STA INWK+31            \ any ideas of its own
    
     LDA #1                 \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 1
    
     JSR LL9                \ Draw the ship on screen to redisplay it
    
                            \ Fall through into MT23 to move to row 10, switch to
                            \ white text, and switch to lower case when printing
                            \ extended tokens
    
    
    
    
           Name: MT23                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Move to row 10, switch to white text, and switch to lower case
                 when printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt23.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT23
    
    
    
    
    
    
    .MT23
    
     LDA #10                \ Set A = 10, so when we fall through into MT29, the
                            \ text cursor gets moved to row 10
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &06, or BIT &06A9, which does nothing apart
                            \ from affect the flags
    
                            \ Fall through into MT29 to move to the row in A, switch
                            \ to white text, and switch to lower case
    
    
    
    
           Name: MT29                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Move to row 6, switch to white text, and switch to lower case when
                 printing extended tokens
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt29.md)
     Variations: See [code variations](../related/compare/main/subroutine/mt29.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls MT29
    
    
    
    
    
    
    .MT29
    
     LDA #6                 \ Move the text cursor to row 6
     STA YC
    
     LDA #CYAN              \ Set white text
     STA COL
    
     JMP MT13               \ Jump to MT13 to set bit 7 of DTW6 and bit 5 of DTW1,
                            \ returning from the subroutine using a tail call
    
    
    
    
           Name: PAS1                                                    [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Display a rotating ship at space coordinates (0, 120, 256) and
                 scan the keyboard
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pas1.md)
     Variations: See [code variations](../related/compare/main/subroutine/pas1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](../main/subroutine/brief.md) calls PAS1
                 * [PAUSE](../main/subroutine/pause.md) calls PAS1
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    If a key is being pressed, X contains the ASCII code of
                            the key being pressed, otherwise it contains 0
    
       A                    Contains the same as X
    
    
    
    
    .PAS1
    
     LDA #120               \ Set y_lo = 120
     STA INWK+3
    
     LDA #0                 \ Set x_lo = 0
     STA INWK
    
     STA INWK+6             \ Set z_lo = 0
    
     LDA #2                 \ Set z_hi = 1, so (z_hi z_lo) = 256
     STA INWK+7
    
     JSR LL9                \ Draw the ship on screen
    
     JSR MVEIT              \ Call MVEIT to move and rotate the ship in space
    
     JMP RDKEY              \ Scan the keyboard for a key press and return the
                            \ ASCII code of the key pressed in X (or 0 for no key
                            \ press), returning from the subroutine using a tail
                            \ call
    
    
    
    
           Name: PAUSE2                                                  [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Wait until a key is pressed, ignoring any existing key press
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pause2.md)
     Variations: See [code variations](../related/compare/main/subroutine/pause2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls PAUSE2
    
    
    
    
    
    * * *
    
    
     Returns:
    
       X                    The ASCII code of the key that was pressed
    
    
    
    
    .PAUSE2
    
     JSR RDKEY              \ Scan the keyboard for a key press and return the ASCII
                            \ code of the key pressed in A and X (or 0 for no key
                            \ press)
    
     BNE PAUSE2             \ If a key was already being held down when we entered
                            \ this routine, keep looping back up to PAUSE2, until
                            \ the key is released
    
     JSR RDKEY              \ Any pre-existing key press is now gone, so we can
                            \ start scanning the keyboard again, returning the
                            \ ASCII code of the key pressed in X (or 0 for no key
                            \ press)
    
     BEQ PAUSE2             \ Keep looping up to PAUSE2 until a key is pressed
    
    .newyearseve
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: GINF                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Fetch the address of a ship's data block into INF
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ginf.md)
     References: This subroutine is called as follows:
                 * [FRMIS](../main/subroutine/frmis.md) calls GINF
                 * [Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md) calls GINF
                 * [NWSHP](../main/subroutine/nwshp.md) calls GINF
                 * [WPSHPS](../main/subroutine/wpshps.md) calls GINF
    
    
    
    
    
    * * *
    
    
     Get the address of the data block for ship slot X and store it in INF. This
     address is fetched from the UNIV table, which stores the addresses of the 13
     ship data blocks in workspace K%.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The ship slot number for which we want the data block
                            address
    
    
    
    
    .GINF
    
     TXA                    \ Set Y = X * 2
     ASL A
     TAY
    
     LDA UNIV,Y             \ Get the high byte of the address of the X-th ship
     STA INF                \ from UNIV and store it in INF
    
     LDA UNIV+1,Y           \ Get the low byte of the address of the X-th ship
     STA INF+1              \ from UNIV and store it in INF
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: ping                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set the selected system to the current system
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ping.md)
     References: This subroutine is called as follows:
                 * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md) calls ping
                 * [TT102](../main/subroutine/tt102.md) calls ping
    
    
    
    
    
    
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
    
    
    
    
           Name: MTIN                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: Lookup table for random tokens in the extended token table (0-37)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [on its own page](../main/variable/mtin.md)
     References: This variable is used as follows:
                 * [DETOK2](../main/subroutine/detok2.md) uses MTIN
    
    
    
    
    
    * * *
    
    
     The ERND token type, which is part of the extended token system, takes an
     argument between 0 and 37, and returns a randomly chosen token in the range
     specified in this table. This is used to generate the extended description of
     each system.
    
     For example, the entry at position 13 in this table (counting from 0) is 66,
     so ERND 14 will expand into a random token in the range 66-70, i.e. one of
     "JUICE", "BRANDY", "WATER", "BREW" and "GARGLE BLASTERS".
    
    
    
    
    .MTIN
    
     EQUB 16                \ Token  0: a random extended token between 16 and 20
     EQUB 21                \ Token  1: a random extended token between 21 and 25
     EQUB 26                \ Token  2: a random extended token between 26 and 30
     EQUB 31                \ Token  3: a random extended token between 31 and 35
     EQUB 155               \ Token  4: a random extended token between 155 and 159
     EQUB 160               \ Token  5: a random extended token between 160 and 164
     EQUB 46                \ Token  6: a random extended token between 46 and 50
     EQUB 165               \ Token  7: a random extended token between 165 and 169
     EQUB 36                \ Token  8: a random extended token between 36 and 40
     EQUB 41                \ Token  9: a random extended token between 41 and 45
     EQUB 61                \ Token 10: a random extended token between 61 and 65
     EQUB 51                \ Token 11: a random extended token between 51 and 55
     EQUB 56                \ Token 12: a random extended token between 56 and 60
     EQUB 170               \ Token 13: a random extended token between 170 and 174
     EQUB 66                \ Token 14: a random extended token between 66 and 70
     EQUB 71                \ Token 15: a random extended token between 71 and 75
     EQUB 76                \ Token 16: a random extended token between 76 and 80
     EQUB 81                \ Token 17: a random extended token between 81 and 85
     EQUB 86                \ Token 18: a random extended token between 86 and 90
     EQUB 140               \ Token 19: a random extended token between 140 and 144
     EQUB 96                \ Token 20: a random extended token between 96 and 100
     EQUB 101               \ Token 21: a random extended token between 101 and 105
     EQUB 135               \ Token 22: a random extended token between 135 and 139
     EQUB 130               \ Token 23: a random extended token between 130 and 134
     EQUB 91                \ Token 24: a random extended token between 91 and 95
     EQUB 106               \ Token 25: a random extended token between 106 and 110
     EQUB 180               \ Token 26: a random extended token between 180 and 184
     EQUB 185               \ Token 27: a random extended token between 185 and 189
     EQUB 190               \ Token 28: a random extended token between 190 and 194
     EQUB 225               \ Token 29: a random extended token between 225 and 229
     EQUB 230               \ Token 30: a random extended token between 230 and 234
     EQUB 235               \ Token 31: a random extended token between 235 and 239
     EQUB 240               \ Token 32: a random extended token between 240 and 244
     EQUB 245               \ Token 33: a random extended token between 245 and 249
     EQUB 250               \ Token 34: a random extended token between 250 and 254
     EQUB 115               \ Token 35: a random extended token between 115 and 119
     EQUB 120               \ Token 36: a random extended token between 120 and 124
     EQUB 125               \ Token 37: a random extended token between 125 and 129
    
    
    
     Save ELTC.bin
    
    
    
    
     PRINT "ELITE C"
     PRINT "Assembled at ", ~CODE_C%
     PRINT "Ends at ", ~P%
     PRINT "Code size is ", ~(P% - CODE_C%)
     PRINT "Execute at ", ~LOAD%
     PRINT "Reload at ", ~LOAD_C%
    
     PRINT "S.ELTC ", ~CODE_C%, " ", ~P%, " ", ~LOAD%, " ", ~LOAD_C%
    \SAVE "3-assembled-output/ELTC.bin", CODE_C%, P%, LOAD%
    
    
    

[X]

Subroutine [ABORT](elite_e.md#header-abort) (category: Dashboard)

Disarm missiles and update the dashboard indicators

[X]

Configuration variable [ACT](workspaces.md#act)

The address of the arctan lookup table, as set in elite-data.asm

[X]

Subroutine [ADD](elite_c.md#header-add) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Variable [ALP1](workspaces.md#alp1) in workspace [ZP](workspaces.md#header-zp)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Variable [ALP2](workspaces.md#alp2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Variable [ALPHA](workspaces.md#alpha) in workspace [ZP](workspaces.md#header-zp)

The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31

[X]

Label [AN2](elite_c.md#an2) in subroutine [ANGRY](elite_c.md#header-angry)

[X]

Label [AN3](elite_c.md#an3) in subroutine [ANGRY](elite_c.md#header-angry)

[X]

Configuration variable [ANA](workspaces.md#ana)

Ship type for an Anaconda

[X]

Subroutine [ANGRY](elite_c.md#header-angry) (category: Tactics)

Make a ship or station hostile, and if this is a ship then enable the ship's AI and give it a kick of speed

[X]

Label [AR1](elite_c.md#ar1) in subroutine [ARCTAN](elite_c.md#header-arctan)

[X]

Label [AR2](elite_c.md#ar2) in subroutine [ARCTAN](elite_c.md#header-arctan)

[X]

Label [AR3](elite_c.md#ar3) in subroutine [ARCTAN](elite_c.md#header-arctan)

[X]

Label [AR4](elite_c.md#ar4) in subroutine [ARCTAN](elite_c.md#header-arctan)

[X]

Label [ARS1](elite_c.md#ars1) in subroutine [ARCTAN](elite_c.md#header-arctan)

[X]

Entry point [ARSR1](elite_c.md#arsr1) in subroutine [ARCTAN](elite_c.md#header-arctan) (category: Maths (Geometry))

Contains an RTS

[X]

Subroutine [BAY](elite_f.md#header-bay) (category: Status)

Go to the docking bay (i.e. show the Status Mode screen)

[X]

Variable [BET1](workspaces.md#bet1) in workspace [ZP](workspaces.md#header-zp)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BET2](workspaces.md#bet2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of BET2 = sign of the pitch angle in BETA

[X]

Label [BR2](elite_c.md#br2) in subroutine [BRIEF](elite_c.md#header-brief)

[X]

Subroutine [BRIS](elite_c.md#header-bris) (category: Missions)

Clear the screen, display "INCOMING MESSAGE" and wait for 2 seconds

[X]

Label [BRL1](elite_c.md#brl1) in subroutine [BRIEF](elite_c.md#header-brief)

[X]

Label [BRL2](elite_c.md#brl2) in subroutine [BRIEF](elite_c.md#header-brief)

[X]

Subroutine [BRP](elite_c.md#header-brp) (category: Missions)

Print an extended token and show the Status Mode screen

[X]

Entry point [BRPS](elite_c.md#brps) in subroutine [DEBRIEF](elite_c.md#header-debrief) (category: Missions)

Print the extended token in A, show the Status Mode screen and return from the subroutine

[X]

Label [BUMP](elite_c.md#bump) in subroutine [cntr](elite_c.md#header-cntr)

[X]

Subroutine [CIRCLE2](elite_e.md#header-circle2) (category: Drawing circles)

Draw a circle (for the planet or chart)

[X]

Variable [CNT](workspaces.md#cnt) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [CNT2](workspaces.md#cnt2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Variable [COL](workspaces.md#col) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CON](workspaces.md#con)

Ship type for a Constrictor

[X]

Configuration variable [COPS](workspaces.md#cops)

Ship type for a Viper

[X]

Configuration variable [CYAN](workspaces.md#cyan)

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Configuration variable [CYL](workspaces.md#cyl)

Ship type for a Cobra Mk III

[X]

Variable [DAMP](elite_a.md#damp) in workspace [UP](elite_a.md#header-up)

Keyboard damping configuration setting

[X]

Subroutine [DCS1](elite_c.md#header-dcs1) (category: Flight)

Calculate the vector from the ideal docking position to the ship

[X]

Subroutine [DELAY](elite_a.md#header-delay) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Variable [DELTA](workspaces.md#delta) in workspace [ZP](workspaces.md#header-zp)

Our current speed, in the range 1-40

[X]

Subroutine [DENGY](elite_e.md#header-dengy) (category: Flight)

Drain some energy from the energy banks

[X]

Subroutine [DETOK](elite_a.md#header-detok) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [DETOK2](elite_a.md#header-detok2) (category: Text)

Print an extended text token (1-255)

[X]

Subroutine [DETOK3](elite_a.md#header-detok3) (category: Text)

Print an extended recursive token from the RUTOK token table

[X]

Variable [DJD](elite_a.md#djd) in workspace [UP](elite_a.md#header-up)

Keyboard auto-recentre configuration setting

[X]

Subroutine [DOCKIT](elite_c.md#header-dockit) (category: Flight)

Apply docking manoeuvres to the ship in INWK

[X]

Subroutine [DORND](elite_f.md#header-dornd) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [DOVDU19](elite_a.md#header-dovdu19) (category: Drawing the screen)

Change the mode 1 palette

[X]

Subroutine [DOXC](elite_d.md#header-doxc) (category: Text)

Move the text cursor to a specific column

[X]

Label [DV12](elite_c.md#dv12) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Label [DV13](elite_c.md#dv13) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Label [DV14](elite_c.md#dv14) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Label [DV4](elite_c.md#dv4) in subroutine [TIS1](elite_c.md#header-tis1)

[X]

Subroutine [DV41](elite_c.md#header-dv41) (category: Maths (Arithmetic))

Calculate (P R) = 256 * DELTA / A

[X]

Label [DVL10](elite_c.md#dvl10) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Label [DVL3](elite_c.md#dvl3) in subroutine [TIS1](elite_c.md#header-tis1)

[X]

Label [DVL6](elite_c.md#dvl6) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Label [DVL8](elite_c.md#dvl8) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Label [DVL9](elite_c.md#dvl9) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Configuration variable [E%](workspaces.md#e-per-cent)

The address of the default NEWB ship bytes, as set in elite-data.asm

[X]

Subroutine [ECBLB2](elite_a.md#header-ecblb2) (category: Dashboard)

Start up the E.C.M. (light up the indicator, start the countdown and make the E.C.M. sound)

[X]

Variable [ECMA](workspaces.md#ecma) in workspace [ZP](workspaces.md#header-zp)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Subroutine [ELASNO](elite_a.md#header-elasno) (category: Sound)

Make the sound of us being hit

[X]

Variable [ENGY](workspaces.md#engy) in workspace [WP](workspaces.md#header-wp)

Energy unit

[X]

Configuration variable [ESC](workspaces.md#esc)

Ship type for an escape pod

[X]

Subroutine [EXNO2](elite_h.md#header-exno2) (category: Status)

Process us making a kill

[X]

Subroutine [EXNO3](elite_h.md#header-exno3) (category: Sound)

Make an explosion sound

[X]

Variable [FIST](workspaces.md#fist) in workspace [WP](workspaces.md#header-wp)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Subroutine [FR1](elite_c.md#header-fr1) (category: Tactics)

Display the "missile jammed" message

[X]

Variable [FRIN](workspaces.md#frin) in workspace [WP](workspaces.md#header-wp)

Slots for the ships in the local bubble of universe

[X]

Label [FRL2](elite_c.md#frl2) in subroutine [SFS1](elite_c.md#header-sfs1)

[X]

Label [FRL3](elite_c.md#frl3) in subroutine [SFS1](elite_c.md#header-sfs1)

[X]

Subroutine [FRS1](elite_c.md#header-frs1) (category: Tactics)

Launch a ship straight ahead of us, below the laser sights

[X]

Variable [GCNT](workspaces.md#gcnt) in workspace [WP](workspaces.md#header-wp)

The number of the current galaxy (0-7)

[X]

Subroutine [GINF](elite_c.md#header-ginf) (category: Universe)

Fetch the address of a ship's data block into INF

[X]

Variable [GNTMP](workspaces.md#gntmp) in workspace [WP](workspaces.md#header-wp)

Laser temperature (or "gun temperature")

[X]

Entry point [GOPL](elite_c.md#gopl) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7) (category: Tactics)

Make the ship head towards the planet

[X]

Label [GOPLS](elite_c.md#gopls) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Label [HA1](elite_c.md#ha1) in subroutine [HAS1](elite_c.md#header-has1)

[X]

Label [HA7](elite_c.md#ha7) in subroutine [HALL](elite_c.md#header-hall)

[X]

Label [HA9](elite_c.md#ha9) in subroutine [HALL](elite_c.md#header-hall)

[X]

Label [HAL5](elite_c.md#hal5) in subroutine [HAS1](elite_c.md#header-has1)

[X]

Label [HAL8](elite_c.md#hal8) in subroutine [HALL](elite_c.md#header-hall)

[X]

Label [HAL9](elite_c.md#hal9) in subroutine [HALL](elite_c.md#header-hall)

[X]

Subroutine [HANGER](elite_a.md#header-hanger) (category: Ship hangar)

Display the ship hangar

[X]

Variable [HANGFLAG](elite_a.md#header-hangflag) (category: Ship hangar)

The number of ships being displayed in the ship hangar

[X]

Subroutine [HAS1](elite_c.md#header-has1) (category: Ship hangar)

Draw a ship in the ship hangar

[X]

Variable [HATB](elite_c.md#header-hatb) (category: Ship hangar)

Ship hangar group table

[X]

Configuration variable [HER](workspaces.md#her)

Ship type for a rock hermit (asteroid)

[X]

Label [HF8](elite_c.md#hf8) in subroutine [HFS2](elite_c.md#header-hfs2)

[X]

Label [HFL1](elite_c.md#hfl1) in subroutine [HFS2](elite_c.md#header-hfs2)

[X]

Label [HFL2](elite_c.md#hfl2) in subroutine [HFS2](elite_c.md#header-hfs2)

[X]

Label [HFL5](elite_c.md#hfl5) in subroutine [HFS2](elite_c.md#header-hfs2)

[X]

Subroutine [HFS2](elite_c.md#header-hfs2) (category: Drawing circles)

Draw the launch or hyperspace tunnel

[X]

Variable [HFX](workspaces.md#hfx) in workspace [WP](workspaces.md#header-wp)

A flag that toggles the hyperspace colour effect

[X]

Entry point [HI1](elite_c.md#hi1) in subroutine [HITCH](elite_c.md#header-hitch) (category: Tactics)

Contains an RTS

[X]

Variable [INF](workspaces.md#inf) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](workspaces.md#inwk) in workspace [ZP](workspaces.md#header-zp)

The zero-page internal workspace for the current ship data block

[X]

Variable [K](workspaces.md#k) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Workspace [K%](workspaces.md#header-k-per-cent) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Variable [K3](workspaces.md#k3) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K4](workspaces.md#k4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Label [KILL2](elite_c.md#kill2) in subroutine [STARS2](elite_c.md#header-stars2)

[X]

Configuration variable [KRA](workspaces.md#kra)

Ship type for a Krait

[X]

Variable [LASX](workspaces.md#lasx) in workspace [WP](workspaces.md#header-wp)

The x-coordinate of the tip of the laser line

[X]

Variable [LASY](workspaces.md#lasy) in workspace [WP](workspaces.md#header-wp)

The y-coordinate of the tip of the laser line

[X]

Label [LL222](elite_c.md#ll222) in subroutine [DVID4](elite_c.md#header-dvid4)

[X]

Subroutine [LL28](elite_g.md#header-ll28) (category: Maths (Arithmetic))

Calculate R = 256 * A / Q

[X]

Label [LL29new](elite_c.md#ll29new) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Label [LL312new](elite_c.md#ll312new) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Label [LL31new](elite_c.md#ll31new) in subroutine [DVID3B2](elite_c.md#header-dvid3b2)

[X]

Subroutine [LL5](elite_g.md#header-ll5) (category: Maths (Arithmetic))

Calculate Q = SQRT(R Q)

[X]

Subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Label [LLfix22](elite_c.md#llfix22) in subroutine [DVID4](elite_c.md#header-dvid4)

[X]

Subroutine [LOIN](elite_a.md#header-loin) (category: Drawing lines)

Draw a one-segment line

[X]

Variable [LSP](workspaces.md#lsp) in workspace [ZP](workspaces.md#header-zp)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Label [M32](elite_c.md#m32) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Subroutine [MAD](elite_c.md#header-mad) (category: Maths (Arithmetic))

Calculate (A X) = Q * A + (S R)

[X]

Variable [MANY](workspaces.md#many) in workspace [WP](workspaces.md#header-wp)

The number of ships of each type in the local bubble of universe

[X]

Subroutine [MAS4](elite_f.md#header-mas4) (category: Maths (Geometry))

Calculate a cap on the maximum distance to a ship

[X]

Subroutine [MCASH](elite_d.md#header-mcash) (category: Maths (Arithmetic))

Add an amount of cash to the cash pot

[X]

Variable [MCNT](workspaces.md#mcnt) in workspace [ZP](workspaces.md#header-zp)

The main loop counter

[X]

Subroutine [MESS](elite_f.md#header-mess) (category: Flight)

Display an in-flight message

[X]

Configuration variable [MSL](workspaces.md#msl)

Ship type for a missile

[X]

Variable [MSTG](workspaces.md#mstg) in workspace [ZP](workspaces.md#header-zp)

The current missile lock target

[X]

Subroutine [MT13](elite_a.md#header-mt13) (category: Text)

Switch to lower case when printing extended tokens

[X]

Subroutine [MT14](elite_a.md#header-mt14) (category: Text)

Switch to justified text when printing extended tokens

[X]

Subroutine [MU1](elite_c.md#header-mu1) (category: Maths (Arithmetic))

Copy X into P and A, and clear the C flag

[X]

Subroutine [MU11](elite_c.md#header-mu11) (category: Maths (Arithmetic))

Calculate (A P) = P * X

[X]

Label [MU21](elite_c.md#mu21) in subroutine [MLTU2](elite_c.md#header-mltu2)

[X]

Label [MU3](elite_c.md#mu3) in subroutine [FMLTU](elite_c.md#header-fmltu)

[X]

Label [MU3again](elite_c.md#mu3again) in subroutine [FMLTU](elite_c.md#header-fmltu)

[X]

Subroutine [MU5](elite_c.md#header-mu5) (category: Maths (Arithmetic))

Set K(3 2 1 0) = (A A A A) and clear the C flag

[X]

Subroutine [MU6](elite_c.md#header-mu6) (category: Maths (Arithmetic))

Set P(1 0) = (A A)

[X]

Label [MU8](elite_c.md#mu8) in subroutine [ADD](elite_c.md#header-add)

[X]

Label [MU9](elite_c.md#mu9) in subroutine [ADD](elite_c.md#header-add)

[X]

Label [MUL2](elite_c.md#mul2) in subroutine [MULT3](elite_c.md#header-mult3)

[X]

Label [MUL7](elite_c.md#mul7) in subroutine [MLTU2](elite_c.md#header-mltu2)

[X]

Subroutine [MULT1](elite_c.md#header-mult1) (category: Maths (Arithmetic))

Calculate (A P) = Q * A

[X]

Subroutine [MULT12](elite_c.md#header-mult12) (category: Maths (Arithmetic))

Calculate (S R) = Q * A

[X]

Entry point [MULTS-2](elite_c.md#mults) in subroutine [MLS1](elite_c.md#header-mls1) (category: Maths (Arithmetic))

Calculate (A P) = X * A

[X]

Subroutine [MVEIT (Part 1 of 9)](elite_h.md#header-mveit-part-1-of-9) (category: Moving)

Move current ship: Tidy the orientation vectors

[X]

Subroutine [MVS5](elite_b.md#header-mvs5) (category: Moving)

Apply a 3.6 degree pitch or roll to an orientation vector

[X]

Subroutine [MVT1](elite_h.md#header-mvt1) (category: Moving)

Calculate (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (A R)

[X]

Subroutine [MVT3](elite_b.md#header-mvt3) (category: Moving)

Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)

[X]

Variable [NEWB](workspaces.md#newb) in workspace [ZP](workspaces.md#header-zp)

The ship's "new byte flags" (or NEWB flags)

[X]

Configuration variable [NI%](workspaces.md#ni-per-cent)

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Label [NOIL](elite_c.md#noil) in subroutine [SFS1](elite_c.md#header-sfs1)

[X]

Subroutine [NOISE](elite_a.md#header-noise) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Variable [NOMSL](workspaces.md#nomsl) in workspace [WP](workspaces.md#header-wp)

The number of missiles we have fitted (0-4)

[X]

Variable [NOSTM](workspaces.md#nostm) in workspace [ZP](workspaces.md#header-zp)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Configuration variable [NRU%](workspaces.md#nru-per-cent)

The number of planetary systems with extended system description overrides in the RUTOK table

[X]

Subroutine [NWSHP](elite_e.md#header-nwshp) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Configuration variable [OIL](workspaces.md#oil)

Ship type for a cargo canister

[X]

Subroutine [OOPS](elite_e.md#header-oops) (category: Flight)

Take some damage

[X]

Variable [P](workspaces.md#p) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Label [PAL1](elite_c.md#pal1) in subroutine [PAUSE](elite_c.md#header-pause)

[X]

Subroutine [PAS1](elite_c.md#header-pas1) (category: Missions)

Display a rotating ship at space coordinates (0, 120, 256) and scan the keyboard

[X]

Subroutine [PAUSE](elite_c.md#header-pause) (category: Missions)

Display a rotating ship, waiting until a key is pressed, then remove the ship from the screen

[X]

Subroutine [PAUSE2](elite_c.md#header-pause2) (category: Keyboard)

Wait until a key is pressed, ignoring any existing key press

[X]

Label [PD1](elite_c.md#pd1) in subroutine [PDESC](elite_c.md#header-pdesc)

[X]

Label [PD2](elite_c.md#pd2) in subroutine [PDESC](elite_c.md#header-pdesc)

[X]

Label [PD3](elite_c.md#pd3) in subroutine [PDESC](elite_c.md#header-pdesc)

[X]

Label [PD4](elite_c.md#pd4) in subroutine [PDESC](elite_c.md#header-pdesc)

[X]

Label [PDL1](elite_c.md#pdl1) in subroutine [PDESC](elite_c.md#header-pdesc)

[X]

Label [PDL1K](elite_c.md#pdl1k) in subroutine [PDESC](elite_c.md#header-pdesc)

[X]

Label [PH1](elite_c.md#ph1) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Label [PH2](elite_c.md#ph2) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Label [PH22](elite_c.md#ph22) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Label [PH3](elite_c.md#ph3) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Label [PH32](elite_c.md#ph32) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Subroutine [PIX1](elite_a.md#header-pix1) (category: Maths (Arithmetic))

Calculate (YY+1 SYL+Y) = (A P) + (S R) and draw stardust particle

[X]

Subroutine [PIXEL2](elite_a.md#header-pixel2) (category: Drawing pixels)

Draw a stardust particle relative to the screen centre

[X]

Configuration variable [PLT](workspaces.md#plt)

Ship type for an alloy plate

[X]

Variable [Q](workspaces.md#q) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [QQ0](workspaces.md#qq0) in workspace [WP](workspaces.md#header-wp)

The current system's galactic x-coordinate (0-256)

[X]

Variable [QQ11](workspaces.md#qq11) in workspace [ZP](workspaces.md#header-zp)

The type of the current view

[X]

Variable [QQ12](workspaces.md#qq12) in workspace [ZP](workspaces.md#header-zp)

Our "docked" status

[X]

Variable [QQ15](workspaces.md#qq15) in workspace [ZP](workspaces.md#header-zp)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ8](workspaces.md#qq8) in workspace [ZP](workspaces.md#header-zp)

The distance from the current system to the selected system in light years * 10, stored as a 16-bit number

[X]

Variable [QQ9](workspaces.md#qq9) in workspace [ZP](workspaces.md#header-zp)

The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)

[X]

Variable [R](workspaces.md#r) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [RAND](workspaces.md#rand) in workspace [ZP](workspaces.md#header-zp)

Four 8-bit seeds for the random number generation system implemented in the DORND routine

[X]

Variable [RAT](workspaces.md#rat) in workspace [ZP](workspaces.md#header-zp)

Used to store different signs depending on the current space view, for use in calculating stardust movement

[X]

Variable [RAT2](workspaces.md#rat2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the pitch and roll signs when moving objects and stardust

[X]

Subroutine [RDKEY](elite_h.md#header-rdkey) (category: Keyboard)

Scan the keyboard for key presses and update the key logger

[X]

Label [RE1](elite_c.md#re1) in subroutine [cntr](elite_c.md#header-cntr)

[X]

Label [RE2](elite_c.md#re2) in subroutine [BUMP2](elite_c.md#header-bump2)

[X]

Entry point [RE2+2](elite_c.md#re2) in subroutine [BUMP2](elite_c.md#header-bump2) (category: Dashboard)

Restore A from T and return from the subroutine

[X]

Label [RE3](elite_c.md#re3) in subroutine [REDU2](elite_c.md#header-redu2)

[X]

Configuration variable [RED](workspaces.md#red)

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Configuration variable [RUGAL](workspaces.md#rugal)

The address of the extended system description galaxy number table, as set in elite-data.asm

[X]

Configuration variable [RUPLA](workspaces.md#rupla)

The address of the extended system description system number table, as set in elite-data.asm

[X]

Variable [S](workspaces.md#s) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [SESCP](elite_c.md#header-sescp) (category: Flight)

Spawn an escape pod from the current (parent) ship

[X]

Subroutine [SFRMIS](elite_h.md#header-sfrmis) (category: Tactics)

Add an enemy missile to our local bubble of universe

[X]

Subroutine [SFS1](elite_c.md#header-sfs1) (category: Universe)

Spawn a child ship from the current (parent) ship

[X]

Subroutine [SFS2](elite_c.md#header-sfs2) (category: Moving)

Move a ship in space along one of the coordinate axes

[X]

Configuration variable [SH3](workspaces.md#sh3)

Ship type for a Sidewinder

[X]

Configuration variable [SHU](workspaces.md#shu)

Ship type for a Shuttle

[X]

Configuration variable [SNE](workspaces.md#sne)

The address of the sine lookup table, as set in elite-data.asm

[X]

Configuration variable [SPL](workspaces.md#spl)

Ship type for a splinter

[X]

Subroutine [SPS1](elite_f.md#header-sps1) (category: Maths (Geometry))

Calculate the vector to the planet and store it in XX15

[X]

Subroutine [SQUA2](elite_c.md#header-squa2) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[X]

Variable [SSPR](workspaces.md#sspr) in workspace [WP](workspaces.md#header-wp)

"Space station present" flag

[X]

Configuration variable [SST](workspaces.md#sst)

Ship type for a Coriolis space station

[X]

Label [ST2](elite_c.md#st2) in subroutine [STARS2](elite_c.md#header-stars2)

[X]

Label [ST5](elite_c.md#st5) in subroutine [STARS2](elite_c.md#header-stars2)

[X]

Label [STC2](elite_c.md#stc2) in subroutine [STARS2](elite_c.md#header-stars2)

[X]

Label [STF1](elite_c.md#stf1) in subroutine [STARS2](elite_c.md#header-stars2)

[X]

Label [STL2](elite_c.md#stl2) in subroutine [STARS2](elite_c.md#header-stars2)

[X]

Variable [STP](workspaces.md#stp) in workspace [ZP](workspaces.md#header-zp)

The step size for drawing circles

[X]

Variable [SX](workspaces.md#sx) in workspace [WP](workspaces.md#header-wp)

This is where we store the x_hi coordinates for all the stardust particles

[X]

Variable [SXL](workspaces.md#sxl) in workspace [WP](workspaces.md#header-wp)

This is where we store the x_lo coordinates for all the stardust particles

[X]

Variable [SY](workspaces.md#sy) in workspace [WP](workspaces.md#header-wp)

This is where we store the y_hi coordinates for all the stardust particles

[X]

Variable [SYL](workspaces.md#syl) in workspace [WP](workspaces.md#header-wp)

This is where we store the y_lo coordinates for all the stardust particles

[X]

Variable [SZ](workspaces.md#sz) in workspace [WP](workspaces.md#header-wp)

This is where we store the z_hi coordinates for all the stardust particles

[X]

Variable [T](workspaces.md#t) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [T1](workspaces.md#t1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Label [TA1](elite_c.md#ta1) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA10](elite_c.md#ta10) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Label [TA11](elite_c.md#ta11) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Label [TA12](elite_c.md#ta12) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Label [TA13](elite_c.md#ta13) in subroutine [TACTICS (Part 2 of 7)](elite_c.md#header-tactics-part-2-of-7)

[X]

Label [TA14](elite_c.md#ta14) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Label [TA15](elite_c.md#ta15) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Entry point [TA151](elite_c.md#ta151) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7) (category: Tactics)

Make the ship head towards the planet

[X]

Label [TA152](elite_c.md#ta152) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Label [TA16](elite_c.md#ta16) in subroutine [TACTICS (Part 5 of 7)](elite_c.md#header-tactics-part-5-of-7)

[X]

Label [TA17](elite_c.md#ta17) in subroutine [TACTICS (Part 2 of 7)](elite_c.md#header-tactics-part-2-of-7)

[X]

Label [TA18](elite_c.md#ta18) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA19](elite_c.md#ta19) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Label [TA19S](elite_c.md#ta19s) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Entry point [TA2](elite_f.md#ta2) in subroutine [TAS2](elite_f.md#header-tas2) (category: Maths (Geometry))

Calculate the length of the vector in XX15 (ignoring the low coordinates), returning it in Q

[X]

Label [TA20](elite_c.md#ta20) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Label [TA21](elite_c.md#ta21) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Label [TA22](elite_c.md#ta22) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Label [TA3](elite_c.md#ta3) in subroutine [TACTICS (Part 6 of 7)](elite_c.md#header-tactics-part-6-of-7)

[X]

Label [TA34](elite_c.md#ta34) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA35](elite_c.md#ta35) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA352](elite_c.md#ta352) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA353](elite_c.md#ta353) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA4](elite_c.md#ta4) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Label [TA5](elite_c.md#ta5) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Label [TA6](elite_c.md#ta6) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Label [TA64](elite_c.md#ta64) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA7](elite_c.md#ta7) in subroutine [TACTICS (Part 4 of 7)](elite_c.md#header-tactics-part-4-of-7)

[X]

Label [TA872](elite_c.md#ta872) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA873](elite_c.md#ta873) in subroutine [TACTICS (Part 1 of 7)](elite_c.md#header-tactics-part-1-of-7)

[X]

Label [TA9](elite_c.md#ta9) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Entry point [TA9-1](elite_c.md#ta9) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7) (category: Tactics)

Contains an RTS

[X]

Label [TAL1](elite_c.md#tal1) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Variable [TALLY](workspaces.md#tally) in workspace [WP](workspaces.md#header-wp)

Our combat rank

[X]

Subroutine [TAS1](elite_c.md#header-tas1) (category: Maths (Arithmetic))

Calculate K3 = (x_sign x_hi x_lo) - V(1 0)

[X]

Subroutine [TAS2](elite_f.md#header-tas2) (category: Maths (Geometry))

Normalise the three-coordinate vector in K3

[X]

Subroutine [TAS3](elite_c.md#header-tas3) (category: Maths (Geometry))

Calculate the dot product of XX15 and an orientation vector

[X]

Subroutine [TAS4](elite_c.md#header-tas4) (category: Maths (Geometry))

Calculate the dot product of XX15 and one of the space station's orientation vectors

[X]

Subroutine [TAS6](elite_c.md#header-tas6) (category: Maths (Geometry))

Negate the vector in XX15 so it points in the opposite direction

[X]

Label [TAS7](elite_c.md#tas7) in subroutine [DCS1](elite_c.md#header-dcs1)

[X]

Configuration variable [TGL](workspaces.md#tgl)

Ship type for a Thargon

[X]

Configuration variable [THG](workspaces.md#thg)

Ship type for a Thargoid

[X]

Subroutine [TIDY](elite_f.md#header-tidy) (category: Maths (Geometry))

Orthonormalise the orientation vectors for a ship

[X]

Label [TN1](elite_c.md#tn1) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Label [TN10](elite_c.md#tn10) in subroutine [HITCH](elite_c.md#header-hitch)

[X]

Label [TN11](elite_c.md#tn11) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Label [TN13](elite_c.md#tn13) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Label [TN2](elite_c.md#tn2) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Label [TN3](elite_c.md#tn3) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Label [TN4](elite_c.md#tn4) in subroutine [TACTICS (Part 3 of 7)](elite_c.md#header-tactics-part-3-of-7)

[X]

Label [TN5](elite_c.md#tn5) in subroutine [TACTICS (Part 2 of 7)](elite_c.md#header-tactics-part-2-of-7)

[X]

Label [TN6](elite_c.md#tn6) in subroutine [TACTICS (Part 2 of 7)](elite_c.md#header-tactics-part-2-of-7)

[X]

Label [TN7](elite_c.md#tn7) in subroutine [TACTICS (Part 4 of 7)](elite_c.md#header-tactics-part-4-of-7)

[X]

Label [TNRTS](elite_c.md#tnrts) in subroutine [DOCKIT](elite_c.md#header-dockit)

[X]

Variable [TP](workspaces.md#tp) in workspace [WP](workspaces.md#header-wp)

The current mission status

[X]

Label [TS71](elite_c.md#ts71) in subroutine [DCS1](elite_c.md#header-dcs1)

[X]

Label [TS72](elite_c.md#ts72) in subroutine [DCS1](elite_c.md#header-dcs1)

[X]

Subroutine [TT66](elite_h.md#header-tt66) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Variable [TYPE](workspaces.md#type) in workspace [ZP](workspaces.md#header-zp)

The current ship type

[X]

Variable [U](workspaces.md#u) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [UNIV](elite_b.md#header-univ) (category: Universe)

Table of pointers to the local universe's ship data blocks

[X]

Variable [V](workspaces.md#v) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing an address pointer

[X]

Subroutine [VCSU1](elite_c.md#header-vcsu1) (category: Maths (Arithmetic))

Calculate vector K3(8 0) = [x y z] - coordinates of the sun or space station

[X]

Subroutine [VCSUB](elite_c.md#header-vcsub) (category: Maths (Arithmetic))

Calculate vector K3(8 0) = [x y z] - coordinates in (A V)

[X]

Configuration variable [WRM](workspaces.md#wrm)

Ship type for a Worm

[X]

Configuration variable [X](workspaces.md#x)

The centre x-coordinate of the 256 x 192 space view

[X]

Variable [X1](workspaces.md#x1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](workspaces.md#x2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XSAV](workspaces.md#xsav) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX](workspaces.md#xx) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing a 16-bit x-coordinate

[X]

Variable [XX0](workspaces.md#xx0) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX15](workspaces.md#xx15) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Configuration variable [XX21](workspaces.md#xx21)

The address of the ship blueprints lookup table, as set in elite-data.asm

[X]

Workspace [XX3](workspaces.md#header-xx3) (category: Workspaces)

Temporary storage space for complex calculations

[X]

Variable [XX4](workspaces.md#xx4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [Y](workspaces.md#y)

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Y1](workspaces.md#y1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](workspaces.md#y2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [YC](workspaces.md#yc) in workspace [ZP](workspaces.md#header-zp)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Variable [YY](workspaces.md#yy) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing a 16-bit y-coordinate

[X]

Subroutine [ZINF](elite_f.md#header-zinf) (category: Universe)

Reset the INWK workspace and orientation vectors

[X]

Variable [ZZ](workspaces.md#zz) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for distance values

[X]

Variable [alogh](elite_a.md#header-alogh) (category: Maths (Arithmetic))

Binary antilogarithm table

[X]

Variable [auto](workspaces.md#auto) in workspace [WP](workspaces.md#header-wp)

Docking computer activation status

[X]

Label [cnt2](elite_c.md#cnt2) in subroutine [cntr](elite_c.md#header-cntr)

[X]

Entry point [djd1](elite_c.md#djd1) in subroutine [REDU2](elite_c.md#header-redu2) (category: Dashboard)

Auto-recentre the value in X, if keyboard auto-recentre is configured

[X]

Label [las](elite_c.md#las) in subroutine [LASLI](elite_c.md#header-lasli)

[X]

Variable [log](elite_a.md#header-log) (category: Maths (Arithmetic))

Binary logarithm table (high byte)

[X]

Variable [logL](elite_a.md#header-logl) (category: Maths (Arithmetic))

Binary logarithm table (low byte)

[X]

Label [mu10](elite_c.md#mu10) in subroutine [MULT1](elite_c.md#header-mult1)

[X]

Variable [newzp](workspaces.md#newzp) in workspace [ZP](workspaces.md#header-zp)

This is used by the STARS2 routine for storing the stardust particle's delta_x value

[X]

Label [pl1](elite_c.md#pl1) in subroutine [ping](elite_c.md#header-ping)

[X]

Label [rx](elite_c.md#rx) in subroutine [SFS1](elite_c.md#header-sfs1)

[X]

Configuration variable [sohyp](workspaces.md#sohyp)

Sound 10 = Hyperspace drive engaged 1

[X]

Configuration variable [sohyp2](workspaces.md#sohyp2)

Sound 11 = Hyperspace drive engaged 2

[X]

Configuration variable [solaun](workspaces.md#solaun)

Sound 8 = Missile launched / Ship launch

[X]

Label [ta3](elite_c.md#ta3) in subroutine [TACTICS (Part 5 of 7)](elite_c.md#header-tactics-part-5-of-7)

[X]

Label [ttt](elite_c.md#ttt) in subroutine [TACTICS (Part 7 of 7)](elite_c.md#header-tactics-part-7-of-7)

[X]

Variable [widget](workspaces.md#widget) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the original argument in A in the logarithmic FMLTU and LL28 routines

[Elite B source](elite_b.md "Previous routine")[Elite D source](elite_d.md "Next routine")
