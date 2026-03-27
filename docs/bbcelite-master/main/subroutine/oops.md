---
title: "The OOPS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/oops.html"
---

[SP2](sp2.md "Previous routine")[SPS3](sps3.md "Next routine")
    
    
           Name: OOPS                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Take some damage
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-oops)
     Variations: See [code variations](../../related/compare/main/subroutine/oops.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md) calls OOPS
                 * [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md) calls OOPS
                 * [TACTICS (Part 6 of 7)](tactics_part_6_of_7.md) calls OOPS
    
    
    
    
    
    * * *
    
    
     We just took some damage, so reduce the shields if we have any, or reduce the
     energy levels and potentially take some damage to the cargo if we don't.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The amount of damage to take
    
       INF                  The address of the ship block for the ship that attacked
                            us, or the ship that we just ran into
    
    
    
    
    .OOPS
    
     STA T                  \ Store the amount of damage in T
    
     LDX #0                 \ Fetch byte #8 (z_sign) for the ship attacking us, and
     LDY #8                 \ set X = 0
     LDA (INF),Y
    
     BMI OO1                \ If A is negative, then we got hit in the rear, so jump
                            \ to OO1 to process damage to the aft shield
    
     LDA FSH                \ Otherwise the forward shield was damaged, so fetch the
     SBC T                  \ shield strength from FSH and subtract the damage in T
    
     BCC OO2                \ If the C flag is clear then this amount of damage was
                            \ too much for the shields, so jump to OO2 to set the
                            \ shield level to 0 and start taking damage directly
                            \ from the energy banks
    
     STA FSH                \ Store the new value of the forward shield in FSH
    
     RTS                    \ Return from the subroutine
    
    .OO2
    
     LDX #0                 \ Set the forward shield to 0
     STX FSH
    
     BCC OO3                \ Jump to OO3 to start taking damage directly from the
                            \ energy banks (this BCC is effectively a JMP as the C
                            \ flag is clear, as we jumped to OO2 with a BCC)
    
    .OO1
    
     LDA ASH                \ The aft shield was damaged, so fetch the shield
     SBC T                  \ strength from ASH and subtract the damage in T
    
     BCC OO5                \ If the C flag is clear then this amount of damage was
                            \ too much for the shields, so jump to OO5 to set the
                            \ shield level to 0 and start taking damage directly
                            \ from the energy banks
    
     STA ASH                \ Store the new value of the aft shield in ASH
    
     RTS                    \ Return from the subroutine
    
    .OO5
    
     LDX #0                 \ Set the forward shield to 0
     STX ASH
    
    .OO3
    
     ADC ENERGY             \ A is negative and contains the amount by which the
     STA ENERGY             \ damage overwhelmed the shields, so this drains the
                            \ energy banks by that amount (and because the energy
                            \ banks are shown over four indicators rather than one,
                            \ but with the same value range of 0-255, energy will
                            \ appear to drain away four times faster than the
                            \ shields did)
    
     BEQ P%+4               \ If we have just run out of energy, skip the next
                            \ instruction to jump straight to our death
    
     BCS P%+5               \ If the C flag is set, then subtracting the damage from
                            \ the energy banks didn't underflow, so we had enough
                            \ energy to survive, and we can skip the next
                            \ instruction to make a sound and take some damage
    
     JMP DEATH              \ Otherwise our energy levels are either 0 or negative,
                            \ and in either case that means we jump to our DEATH,
                            \ returning from the subroutine using a tail call
    
     JSR EXNO3              \ We didn't die, so call EXNO3 to make the sound of a
                            \ collision
    
     JMP OUCH               \ And jump to OUCH to take damage and return from the
                            \ subroutine using a tail call
    

[X]

Variable [ASH](../workspace/zp.md#ash) in workspace [ZP](../workspace/zp.md)

Aft shield status

[X]

Subroutine [DEATH](death.md) (category: Start and end)

Display the death screen

[X]

Variable [ENERGY](../workspace/zp.md#energy) in workspace [ZP](../workspace/zp.md)

Energy bank status

[X]

Subroutine [EXNO3](exno3.md) (category: Sound)

Make an explosion sound

[X]

Variable [FSH](../workspace/zp.md#fsh) in workspace [ZP](../workspace/zp.md)

Forward shield status

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Label [OO1](oops.md#oo1) is local to this routine

[X]

Label [OO2](oops.md#oo2) is local to this routine

[X]

Label [OO3](oops.md#oo3) is local to this routine

[X]

Label [OO5](oops.md#oo5) is local to this routine

[X]

Subroutine [OUCH](ouch.md) (category: Flight)

Potentially lose cargo or equipment following damage

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[SP2](sp2.md "Previous routine")[SPS3](sps3.md "Next routine")
