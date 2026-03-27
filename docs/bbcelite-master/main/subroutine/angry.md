---
title: "The ANGRY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/angry.html"
---

[FRMIS](frmis.md "Previous routine")[FR1](fr1.md "Next routine")
    
    
           Name: ANGRY                                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Make a ship or station hostile, and if this is a ship then enable
                 the ship's AI and give it a kick of speed
      Deep dive: [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-angry)
     Variations: See [code variations](../../related/compare/main/subroutine/angry.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [FRMIS](frmis.md) calls ANGRY
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls ANGRY
    
    
    
    
    
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
    

[X]

Label [AN2](angry.md#an2) is local to this routine

[X]

Label [AN3](angry.md#an3) is local to this routine

[X]

Configuration variable [CYL](../../all/workspaces.md#cyl) = 11

Ship type for a Cobra Mk III

[X]

Entry point [HI1](hitch.md#hi1) in subroutine [HITCH](hitch.md) (category: Tactics)

Contains an RTS

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[FRMIS](frmis.md "Previous routine")[FR1](fr1.md "Next routine")
