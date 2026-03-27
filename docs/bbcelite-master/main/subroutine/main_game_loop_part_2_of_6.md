---
title: "The Main game loop (Part 2 of 6) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_game_loop_part_2_of_6.html"
---

[Main game loop (Part 1 of 6)](main_game_loop_part_1_of_6.md "Previous routine")[Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md "Next routine")
    
    
           Name: Main game loop (Part 2 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Call the main flight loop, and potentially spawn a trader, an
                 asteroid, or a cargo canister
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-main-game-loop-part-2-of-6)
     Variations: See [code variations](../../related/compare/main/subroutine/main_game_loop_part_2_of_6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 1 of 6)](main_game_loop_part_1_of_6.md) calls via TT100
                 * [Main game loop (Part 6 of 6)](main_game_loop_part_6_of_6.md) calls via TT100
                 * [me2](me2.md) calls via me3
    
    
    
    
    
    * * *
    
    
     This section covers the following:
    
       * Call M% to do the main flight loop
    
       * Potentially spawn a trader, asteroid or cargo canister
    
    
    
    * * *
    
    
     Other entry points:
    
       TT100                The entry point for the start of the main game loop,
                            which calls the main flight loop and the moves into the
                            spawning routine
    
       me3                  Used by me2 to jump back into the main game loop after
                            printing an in-flight message
    
    
    
    
    .TT100
    
     JSR M%                 \ Call M% to iterate through the main flight loop
    
     DEC DLY                \ Decrement the delay counter in DLY, so any in-flight
                            \ messages get removed once the counter reaches zero
    
     BEQ me2                \ If DLY is now 0, jump to me2 to remove any in-flight
                            \ message from the space view, and once done, return to
                            \ me3 below, skipping the following two instructions
    
     BPL me3                \ If DLY is positive, jump to me3 to skip the next
                            \ instruction
    
     INC DLY                \ If we get here, DLY is negative, so we have gone too
                            \ and need to increment DLY back to 0
    
    .me3
    
     DEC MCNT               \ Decrement the main loop counter in MCNT
    
     BEQ P%+5               \ If the counter has reached zero, which it will do
                            \ every 256 main loops, skip the next JMP instruction
                            \ (or to put it another way, if the counter hasn't
                            \ reached zero, jump down to MLOOP, skipping all the
                            \ following checks)
    
    .ytq
    
     JMP MLOOP              \ Jump down to MLOOP to do some end-of-loop tidying and
                            \ restart the main loop
    
                            \ We only get here once every 256 iterations of the
                            \ main loop. If we aren't in witchspace and don't
                            \ already have 3 or more asteroids in our local bubble,
                            \ then this section has a 13% chance of spawning
                            \ something benign (the other 87% of the time we jump
                            \ down to consider spawning cops, pirates and bounty
                            \ hunters)
                            \
                            \ If we are in that 13%, then 50% of the time this will
                            \ be a trader, and the other 50% of the time it will
                            \ either be an asteroid (98.5% chance) or, very rarely,
                            \ a cargo canister (1.5% chance)
    
     LDA MJ                 \ If we are in witchspace following a mis-jump, skip the
     BNE ytq                \ following by jumping down to MLOOP (via ytq above)
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #35                \ If A >= 35 (87% chance), jump down to MTT1 to skip
     BCS MTT1               \ the spawning of an asteroid or cargo canister and
                            \ potentially spawn something else
    
     LDA JUNK               \ If we already have 3 or more bits of junk in the local
     CMP #3                 \ bubble, jump down to MTT1 to skip the following and
     BCS MTT1               \ potentially spawn something else
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     LDA #38                \ Set z_hi = 38 (far away)
     STA INWK+7
    
     JSR DORND              \ Set A, X and C flag to random numbers
    
     STA INWK               \ Set x_lo = random
    
     STX INWK+3             \ Set y_lo = random
                            \
                            \ Note that because we use the value of X returned by
                            \ DORND, and X contains the value of A returned by the
                            \ previous call to DORND, this does not set the new ship
                            \ to a totally random location
    
     AND #%10000000         \ Set x_sign = bit 7 of x_lo
     STA INWK+2
    
     TXA                    \ Set y_sign = bit 7 of y_lo
     AND #%10000000
     STA INWK+5
    
     ROL INWK+1             \ Set bit 1 of x_hi to the C flag, which is random, so
     ROL INWK+1             \ this randomly moves us off-centre by 512 (as if x_hi
                            \ is %00000010, then (x_hi x_lo) is 512 + x_lo)
    
     JSR DORND              \ Set A, X and V flag to random numbers
    
     BVS MTT4               \ If V flag is set (50% chance), jump up to MTT4 to
                            \ spawn a trader
    
     ORA #%01101111         \ Take the random number in A and set bits 0-3 and 5-6,
     STA INWK+29            \ so the result has a 50% chance of being positive or
                            \ negative, and a 50% chance of bits 0-6 being 127.
                            \ Storing this number in the roll counter therefore
                            \ gives our new ship a fast roll speed with a 50%
                            \ chance of having no damping, plus a 50% chance of
                            \ rolling clockwise or anti-clockwise
    
     LDA SSPR               \ If we are inside the space station safe zone, jump
     BNE MTT1               \ down to MTT1 to skip the following and potentially
                            \ spawn something else
    
     TXA                    \ Set A to the random X we set above, which we haven't
     BCS MTT2               \ used yet, and if the C flag is set (50% chance) jump
                            \ down to MTT2 to skip the following
    
     AND #31                \ Set the ship speed to our random number, set to a
     ORA #16                \ minimum of 16 and a maximum of 31
     STA INWK+27
    
     BCC MTT3               \ Jump down to MTT3, skipping the following (this BCC
                            \ is effectively a JMP as we know the C flag is clear,
                            \ having passed through the BCS above)
    
    .MTT2
    
     ORA #%01111111         \ Set bits 0-6 of A to 127, leaving bit 7 as random, so
     STA INWK+30            \ storing this number in the pitch counter means we have
                            \ full pitch with no damping, with a 50% chance of
                            \ pitching up or down
    
    .MTT3
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #252               \ If random A < 252 (98.8% of the time), jump to thongs
     BCC thongs             \ to skip the following
    
     LDA #HER               \ Set A to #HER so we spawn a rock hermit 1.2% of the
                            \ time
    
     STA INWK+32            \ Set byte #32 to %00001111 to give the rock hermit an
                            \ E.C.M.
    
     BNE whips              \ Jump to whips (this BNE is effectively a JMP as A will
                            \ never be zero)
    
    .thongs
    
     CMP #10                \ If random A >= 10 (96% of the time), set the C flag
    
     AND #1                 \ Reduce A to a random number that's 0 or 1
    
     ADC #OIL               \ Set A = #OIL + A + C, so there's a 2% chance of us
                            \ spawning a cargo canister (#OIL), a 50% chance of
                            \ us spawning a boulder (#OIL + 1), and a 48% chance of
                            \ us spawning an asteroid (#OIL + 2)
    
    .whips
    
     JSR NWSHP              \ Add our new asteroid or canister to the universe
    

[X]

Variable [DLY](../workspace/wp.md#dly) in workspace [WP](../workspace/wp.md)

In-flight message delay

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

Variable [JUNK](../workspace/wp.md#junk) in workspace [WP](../workspace/wp.md)

The amount of junk in the local bubble

[X]

Entry point [M%](main_flight_loop_part_1_of_16.md#m-per-cent) in subroutine [Main flight loop (Part 1 of 16)](main_flight_loop_part_1_of_16.md) (category: Main loop)

The entry point for the main flight loop

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Variable [MJ](../workspace/wp.md#mj) in workspace [WP](../workspace/wp.md)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Entry point [MLOOP](main_game_loop_part_5_of_6.md#mloop) in subroutine [Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md) (category: Main loop)

The entry point for the main game loop. This entry point comes after the call to the main flight loop and spawning routines, so it marks the start of the main game loop for when we are docked (as we don't need to call the main flight loop or spawning routines if we aren't in space)

[X]

Label [MTT1](main_game_loop_part_3_of_6.md#mtt1) in subroutine [Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md)

[X]

Label [MTT2](main_game_loop_part_2_of_6.md#mtt2) is local to this routine

[X]

Label [MTT3](main_game_loop_part_2_of_6.md#mtt3) is local to this routine

[X]

Label [MTT4](main_game_loop_part_1_of_6.md#mtt4) in subroutine [Main game loop (Part 1 of 6)](main_game_loop_part_1_of_6.md)

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Configuration variable [OIL](../../all/workspaces.md#oil) = 5

Ship type for a cargo canister

[X]

Variable [SSPR](../workspace/wp.md#sspr) in workspace [WP](../workspace/wp.md)

"Space station present" flag

[X]

Subroutine [ZINF](zinf.md) (category: Universe)

Reset the INWK workspace and orientation vectors

[X]

Subroutine [me2](me2.md) (category: Flight)

Remove an in-flight message from the space view

[X]

Entry point [me3](main_game_loop_part_2_of_6.md#me3) in subroutine [Main game loop (Part 2 of 6)](main_game_loop_part_2_of_6.md) (category: Main loop)

Used by me2 to jump back into the main game loop after printing an in-flight message

[X]

Label [thongs](main_game_loop_part_2_of_6.md#thongs) is local to this routine

[X]

Label [whips](main_game_loop_part_2_of_6.md#whips) is local to this routine

[X]

Label [ytq](main_game_loop_part_2_of_6.md#ytq) is local to this routine

[Main game loop (Part 1 of 6)](main_game_loop_part_1_of_6.md "Previous routine")[Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md "Next routine")
