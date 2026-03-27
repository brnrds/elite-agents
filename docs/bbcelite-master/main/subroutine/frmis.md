---
title: "The FRMIS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/frmis.html"
---

[FRS1](frs1.md "Previous routine")[ANGRY](angry.md "Next routine")
    
    
           Name: FRMIS                                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Fire a missile from our ship
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-frmis)
     Variations: See [code variations](../../related/compare/main/subroutine/frmis.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls FRMIS
    
    
    
    
    
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
    

[X]

Subroutine [ABORT](abort.md) (category: Dashboard)

Disarm missiles and update the dashboard indicators

[X]

Subroutine [ANGRY](angry.md) (category: Tactics)

Make a ship or station hostile, and if this is a ship then enable the ship's AI and give it a kick of speed

[X]

Subroutine [FR1](fr1.md) (category: Tactics)

Display the "missile jammed" message

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Subroutine [FRS1](frs1.md) (category: Tactics)

Launch a ship straight ahead of us, below the laser sights

[X]

Subroutine [GINF](ginf.md) (category: Universe)

Fetch the address of a ship's data block into INF

[X]

Configuration variable [MSL](../../all/workspaces.md#msl) = 1

Ship type for a missile

[X]

Variable [MSTG](../workspace/zp.md#mstg) in workspace [ZP](../workspace/zp.md)

The current missile lock target

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Variable [NOMSL](../workspace/wp.md#nomsl) in workspace [WP](../workspace/wp.md)

The number of missiles we have fitted (0-4)

[X]

Configuration variable [solaun](../../all/workspaces.md#solaun) = 8

Sound 8 = Missile launched / Ship launch

[FRS1](frs1.md "Previous routine")[ANGRY](angry.md "Next routine")
