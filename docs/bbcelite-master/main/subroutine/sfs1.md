---
title: "The SFS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sfs1.html"
---

[SESCP](sescp.md "Previous routine")[SFS2](sfs2.md "Next routine")
    
    
           Name: SFS1                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Spawn a child ship from the current (parent) ship
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-sfs1)
     Variations: See [code variations](../../related/compare/main/subroutine/sfs1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SPIN](spin.md) calls SFS1
                 * [TACTICS (Part 2 of 7)](tactics_part_2_of_7.md) calls SFS1
                 * [TACTICS (Part 5 of 7)](tactics_part_5_of_7.md) calls SFS1
                 * [SFRMIS](sfrmis.md) calls via SFS1-2
    
    
    
    
    
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
    

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Label [FRL2](sfs1.md#frl2) is local to this routine

[X]

Label [FRL3](sfs1.md#frl3) is local to this routine

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Label [NOIL](sfs1.md#noil) is local to this routine

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Configuration variable [PLT](../../all/workspaces.md#plt) = 4

Ship type for an alloy plate

[X]

Subroutine [SFS2](sfs2.md) (category: Moving)

Move a ship in space along one of the coordinate axes

[X]

Configuration variable [SPL](../../all/workspaces.md#spl) = 8

Ship type for a splinter

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Workspace [XX3](../workspace/xx3.md) (category: Workspaces)

Temporary storage space for complex calculations

[X]

Label [rx](sfs1.md#rx) is local to this routine

[SESCP](sescp.md "Previous routine")[SFS2](sfs2.md "Next routine")
