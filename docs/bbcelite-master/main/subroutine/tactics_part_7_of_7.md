---
title: "The TACTICS (Part 7 of 7) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tactics_part_7_of_7.html"
---

[TACTICS (Part 6 of 7)](tactics_part_6_of_7.md "Previous routine")[DOCKIT](dockit.md "Next routine")
    
    
           Name: TACTICS (Part 7 of 7)                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Apply tactics: Set pitch, roll, and acceleration
      Deep dive: [Program flow of the tactics routine](https://elite.bbcelite.com/deep_dives/program_flow_of_the_tactics_routine.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tactics-part-7-of-7)
     Variations: See [code variations](../../related/compare/main/subroutine/tactics_part_7_of_7.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOCKIT](dockit.md) calls via TA151
                 * [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md) calls via TA151
                 * [TACTICS (Part 6 of 7)](tactics_part_6_of_7.md) calls via TA9-1
    
    
    
    
    
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
    

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [CNT2](../workspace/zp.md#cnt2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Configuration variable [MSL](../../all/workspaces.md#msl) = 1

Ship type for a missile

[X]

Variable [RAT](../workspace/zp.md#rat) in workspace [ZP](../workspace/zp.md)

Used to store different signs depending on the current space view, for use in calculating stardust movement

[X]

Variable [RAT2](../workspace/zp.md#rat2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the pitch and roll signs when moving objects and stardust

[X]

Label [TA10](tactics_part_7_of_7.md#ta10) is local to this routine

[X]

Label [TA11](tactics_part_7_of_7.md#ta11) is local to this routine

[X]

Label [TA12](tactics_part_7_of_7.md#ta12) is local to this routine

[X]

Label [TA15](tactics_part_7_of_7.md#ta15) is local to this routine

[X]

Label [TA152](tactics_part_7_of_7.md#ta152) is local to this routine

[X]

Label [TA5](tactics_part_7_of_7.md#ta5) is local to this routine

[X]

Label [TA6](tactics_part_7_of_7.md#ta6) is local to this routine

[X]

Label [TA9](tactics_part_7_of_7.md#ta9) is local to this routine

[X]

Subroutine [TAS3](tas3.md) (category: Maths (Geometry))

Calculate the dot product of XX15 and an orientation vector

[X]

Subroutine [TAS6](tas6.md) (category: Maths (Geometry))

Negate the vector in XX15 so it points in the opposite direction

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Label [ttt](tactics_part_7_of_7.md#ttt) is local to this routine

[TACTICS (Part 6 of 7)](tactics_part_6_of_7.md "Previous routine")[DOCKIT](dockit.md "Next routine")
