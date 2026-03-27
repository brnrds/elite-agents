---
title: "The WARP subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/warp.html"
---

[NORM](norm.md "Previous routine")[DKS3](dks3.md "Next routine")
    
    
           Name: WARP                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Perform an in-system jump
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-warp)
     Variations: See [code variations](../../related/compare/main/subroutine/warp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls WARP
    
    
    
    
    
    * * *
    
    
     This is called when we press "J" during flight. The following checks are
     performed:
    
       * Make sure we don't have any ships or space stations in the vicinity
    
       * Make sure we are not in witchspace
    
       * If we are facing the planet, make sure we aren't too close
    
       * If we are facing the sun, make sure we aren't too close
    
     If the above checks are passed, then we perform an in-system jump by moving
     the sun and planet in the opposite direction to travel, so we appear to jump
     in space. This means that any asteroids, cargo canisters or escape pods get
     dragged along for the ride.
    
    
    
    
    .WARP
    
     LDX JUNK               \ Set X to the total number of junk items in the
                            \ vicinity (e.g. asteroids, escape pods, cargo
                            \ canisters, Shuttles, Transporters and so on)
    
     LDA FRIN+2,X           \ If the slot at FRIN+2+X is non-zero, then we have
                            \ something else in the vicinity besides asteroids,
                            \ escape pods and cargo canisters, so to check whether
                            \ we can jump, we first grab the slot contents into A
    
     ORA SSPR               \ If there is a space station nearby, then SSPR will
                            \ be non-zero, so OR'ing with SSPR will produce a
                            \ non-zero result if either A or SSPR are non-zero
    
     ORA MJ                 \ If we are in witchspace, then MJ will be non-zero, so
                            \ OR'ing with MJ will produce a non-zero result if
                            \ either A or SSPR or MJ are non-zero
    
     BNE WA1                \ A is non-zero if we have either a ship or a space
                            \ station in the vicinity, or we are in witchspace, in
                            \ which case jump to WA1 to make a low beep to show that
                            \ we can't do an in-system jump
    
     LDY K%+8               \ Otherwise we can do an in-system jump, so now we fetch
                            \ the byte at K%+8, which contains the z_sign for the
                            \ first ship slot, i.e. the distance of the planet
    
     BMI WA3                \ If the planet's z_sign is negative, then the planet
                            \ is behind us, so jump to WA3 to skip the following
    
     TAY                    \ Set A = Y = 0 (as we didn't BNE above) so the call
                            \ to MAS2 measures the distance to the planet
    
     JSR MAS2               \ Call MAS2 to set A to the largest distance to the
                            \ planet in any of the three axes (we could also call
                            \ routine m to do the same thing, as A = 0)
    
     CMP #2                 \ If A < 2 then jump to WA1 to abort the in-system jump
     BCC WA1                \ with a low beep, as we are facing the planet and are
                            \ too close to jump in that direction
    
    .WA3
    
     LDY K%+NI%+8           \ Fetch the z_sign (byte #8) of the second ship in the
                            \ ship data workspace at K%, which is reserved for the
                            \ sun or the space station (in this case it's the
                            \ former, as we already confirmed there isn't a space
                            \ station in the vicinity)
    
     BMI WA2                \ If the sun's z_sign is negative, then the sun is
                            \ behind us, so jump to WA2 to skip the following
    
     LDY #NI%               \ Set Y to point to the offset of the ship data block
                            \ for the sun, which is NI% (as each block is NI% bytes
                            \ long, and the sun is the second block)
    
     JSR m                  \ Call m to set A to the largest distance to the sun
                            \ in any of the three axes
    
     CMP #2                 \ If A < 2 then jump to WA1 to abort the in-system jump
     BCC WA1                \ with a low beep, as we are facing the sun and are too
                            \ close to jump in that direction
    
    .WA2
    
                            \ If we get here, then we can do an in-system jump, as
                            \ we don't have any ships or space stations in the
                            \ vicinity, we are not in witchspace, and if we are
                            \ facing the planet or the sun, we aren't too close to
                            \ jump towards it
                            \
                            \ We do an in-system jump by moving the sun and planet,
                            \ rather than moving our own local bubble (this is why
                            \ in-system jumps drag asteroids, cargo canisters and
                            \ escape pods along for the ride). Specifically, we move
                            \ them in the z-axis by a fixed amount in the opposite
                            \ direction to travel, thus performing a jump towards
                            \ our destination
    
     LDA #&81               \ Set R = R = P = &81
     STA S
     STA R
     STA P
    
     LDA K%+8               \ Set A = z_sign for the planet
    
     JSR ADD                \ Set (A X) = (A P) + (S R)
                            \           = (z_sign &81) + &8181
                            \           = (z_sign &81) - &0181
                            \
                            \ This moves the planet against the direction of travel
                            \ by reducing z_sign by 1, as the above maths is:
                            \
                            \         z_sign 00000000
                            \   +   00000000 10000001
                            \   -   00000001 10000001
                            \
                            \ or:
                            \
                            \         z_sign 00000000
                            \   +   00000000 00000000
                            \   -   00000001 00000000
                            \
                            \ i.e. the high byte is z_sign - 1, making sure the sign
                            \ is preserved
    
     STA K%+8               \ Set the planet's z_sign to the high byte of the result
    
     LDA K%+NI%+8           \ Set A = z_sign for the sun
    
     JSR ADD                \ Set (A X) = (A P) + (S R)
                            \           = (z_sign &81) + &8181
                            \           = (z_sign &81) - &0181
                            \
                            \ which moves the sun against the direction of travel
                            \ by reducing z_sign by 1
    
     STA K%+NI%+8           \ Set the planet's z_sign to the high byte of the result
    
     LDA #1                 \ Temporarily set the view type to a non-zero value, so
     STA QQ11               \ the call to LOOK1 below clears the screen before
                            \ switching to the space view
    
     STA MCNT               \ Set the main loop counter to 1, so the next iteration
                            \ through the main loop will potentially spawn ships
                            \ (see part 2 of the main game loop at me3)
    
     LSR A                  \ Set EV, the extra vessels spawning counter, to 0
     STA EV                 \ (the LSR produces a 0 as A was previously 1)
    
     LDX VIEW               \ Set X to the current view (front, rear, left or right)
     JMP LOOK1              \ and jump to LOOK1 to initialise that view, returning
                            \ from the subroutine using a tail call
    
    .WA1
    
     JMP BOOP               \ Call the BOOP routine to make a long, low beep, and
                            \ return from the subroutine using a tail call
    
     RTS                    \ This instruction has no effect as we already returned
                            \ from the subroutine
    

[X]

Subroutine [ADD](add.md) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Subroutine [BOOP](boop.md) (category: Sound)

Make a long, low beep

[X]

Variable [EV](../workspace/wp.md#ev) in workspace [WP](../workspace/wp.md)

The "extra vessels" spawning counter

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Variable [JUNK](../workspace/wp.md#junk) in workspace [WP](../workspace/wp.md)

The amount of junk in the local bubble

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Subroutine [LOOK1](look1.md) (category: Flight)

Initialise the space view

[X]

Subroutine [MAS2](mas2.md) (category: Maths (Geometry))

Calculate a cap on the maximum distance to the planet or sun

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Variable [MJ](../workspace/wp.md#mj) in workspace [WP](../workspace/wp.md)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SSPR](../workspace/wp.md#sspr) in workspace [WP](../workspace/wp.md)

"Space station present" flag

[X]

Variable [VIEW](../workspace/wp.md#view) in workspace [WP](../workspace/wp.md)

The number of the current space view

[X]

Label [WA1](warp.md#wa1) is local to this routine

[X]

Label [WA2](warp.md#wa2) is local to this routine

[X]

Label [WA3](warp.md#wa3) is local to this routine

[X]

Entry point [m](mas2.md#m) in subroutine [MAS2](mas2.md) (category: Maths (Geometry))

Do not include A in the calculation

[NORM](norm.md "Previous routine")[DKS3](dks3.md "Next routine")
