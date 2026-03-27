---
title: "The SPIN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/spin.html"
---

[Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md "Previous routine")[BOMBOFF](bomboff.md "Next routine")
    
    
           Name: SPIN                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Randomly spawn cargo from a destroyed ship
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-spin)
     Variations: See [code variations](../../related/compare/main/subroutine/spin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls SPIN
                 * [Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md) calls via oh
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls via SPIN2
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The type of cargo to consider spawning (typically #PLT
                            or #OIL)
    
    
    
    * * *
    
    
     Other entry points:
    
       oh                   Contains an RTS
    
       SPIN2                Remove any randomness: spawn cargo of a specific type
                            (given in X), and always spawn the number given in A
    
    
    
    
    .SPIN
    
     JSR DORND              \ Fetch a random number, and jump to oh if it is
     BPL oh                 \ positive (50% chance)
    
     TYA                    \ Copy the cargo type from Y into A and X
     TAX
    
     LDY #0                 \ Fetch the first byte of the hit ship's blueprint,
     AND (XX0),Y            \ which determines the maximum number of bits of
                            \ debris shown when the ship is destroyed, and AND
                            \ with the random number we just fetched
    
     AND #15                \ Reduce the random number in A to the range 0-15
    
    .SPIN2
    
     STA CNT                \ Store the result in CNT, so CNT contains a random
                            \ number between 0 and the maximum number of bits of
                            \ debris that this ship will release when destroyed
                            \ (to a maximum of 15 bits of debris)
    
    .spl
    
     BEQ oh                 \ We're going to go round a loop using CNT as a counter
                            \ so this checks whether the counter is zero and jumps
                            \ to oh when it gets there (which might be straight
                            \ away)
    
     LDA #0                 \ Call SFS1 to spawn the specified cargo from the now
     JSR SFS1               \ deceased parent ship, giving the spawned canister an
                            \ AI flag of 0 (no AI, zero aggression, no E.C.M.)
    
     DEC CNT                \ Decrease the loop counter
    
     BNE spl+2              \ Jump back up to the LDA &0 instruction above (this BPL
                            \ is effectively a JMP as CNT will never be negative)
    
    .oh
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [SFS1](sfs1.md) (category: Universe)

Spawn a child ship from the current (parent) ship

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Entry point [oh](spin.md#oh) in subroutine [SPIN](spin.md) (category: Universe)

Contains an RTS

[X]

Label [spl](spin.md#spl) is local to this routine

[X]

[Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md "Previous routine")[BOMBOFF](bomboff.md "Next routine")
