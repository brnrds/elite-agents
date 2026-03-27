---
title: "The Main flight loop (Part 11 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_11_of_16.html"
---

[Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md "Previous routine")[Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 11 of 16)                        [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Process missile lock and firing our laser
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Flipping axes between space views](https://elite.bbcelite.com/deep_dives/flipping_axes_between_space_views.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-11-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_11_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * If this is not the front space view, flip the axes of the ship's
            coordinates in INWK
    
         * Process missile lock
    
         * Process our laser firing
    
    
    
    
    .MA26
    
     LDA NEWB               \ If bit 7 of the ship's NEWB flags is clear, skip the
     BPL P%+5               \ following instruction
    
     JSR SCAN               \ Bit 7 of the ship's NEWB flags is set, which means the
                            \ ship has docked or been scooped, so we draw the ship
                            \ on the scanner, which has the effect of removing it
    
     LDA QQ11               \ If this is not a space view, jump to MA15 to skip
     BNE MA15               \ missile and laser locking
    
     JSR PLUT               \ Call PLUT to update the geometric axes in INWK to
                            \ match the view (front, rear, left, right)
    
     JSR HITCH              \ Call HITCH to see if this ship is in the crosshairs,
     BCC MA8                \ in which case the C flag will be set (so if there is
                            \ no missile or laser lock, we jump to MA8 to skip the
                            \ following)
    
     LDA MSAR               \ We have missile lock, so check whether the leftmost
     BEQ MA47               \ missile is currently armed, and if not, jump to MA47
                            \ to process laser fire, as we can't lock an unarmed
                            \ missile
    
     JSR BEEP               \ We have missile lock and an armed missile, so call
                            \ the BEEP subroutine to make a short, high beep
    
     LDX XSAV               \ Call ABORT2 to store the details of this missile
     LDY #RED2              \ lock, with the targeted ship's slot number in X
     JSR ABORT2             \ (which we stored in XSAV at the start of this ship's
                            \ loop at MAL1), and set the colour of the missile
                            \ indicator to the colour in Y (red = &0E)
    
    .MA47
    
                            \ If we get here then the ship is in our sights, but
                            \ we didn't lock a missile, so let's see if we're
                            \ firing the laser
    
     LDA LAS                \ If we are firing the laser then LAS will contain the
     BEQ MA8                \ laser power (which we set in MA68 above), so if this
                            \ is zero, jump down to MA8 to skip the following
    
     LDX #15                \ We are firing our laser and the ship in INWK is in
     JSR EXNO               \ the crosshairs, so call EXNO to make the sound of
                            \ us making a laser strike on another ship
    
     LDA TYPE               \ Did we just hit the space station? If so, jump to
     CMP #SST               \ MA14+2 to make the station hostile, skipping the
     BEQ MA14+2             \ following as we can't destroy a space station
    
     CMP #CON               \ If the ship we hit is less than #CON - i.e. it's not
     BCC BURN               \ a Constrictor, Cougar, Dodo station or the Elite logo,
                            \ jump to BURN to skip the following
    
     LDA LAS                \ Set A to the power of the laser we just used to hit
                            \ the ship (i.e. the laser in the current view)
    
     CMP #(Armlas AND 127)  \ If the laser is not a military laser, jump to MA14+2
     BNE MA14+2             \ to skip the following, as only military lasers have
                            \ any effect on the Constrictor or Cougar
    
     LSR LAS                \ Divide the laser power of the current view by 4, so
     LSR LAS                \ the damage inflicted on the super-ship is a quarter of
                            \ the damage our military lasers would inflict on a
                            \ normal ship
    
    .BURN
    
     LDA INWK+35            \ Fetch the hit ship's energy from byte #35 and subtract
     SEC                    \ our current laser power, and if the result is greater
     SBC LAS                \ than zero, the other ship has survived the hit, so
     BCS MA14               \ jump down to MA14 to make it angry
    
     ASL INWK+31            \ Set bit 7 of the ship byte #31 to indicate that it has
     SEC                    \ now been killed
     ROR INWK+31
    
     LDA TYPE               \ Did we just kill an asteroid? If not, jump to nosp,
     CMP #AST               \ otherwise keep going
     BNE nosp
    
     LDA LAS                \ Did we kill the asteroid using mining lasers? If not,
     CMP #Mlas              \ jump to nosp, otherwise keep going
     BNE nosp
    
     JSR DORND              \ Set A and X to random numbers
    
     LDX #SPL               \ Set X to the ship type for a splinter
    
     AND #3                 \ Reduce the random number in A to the range 0-3
    
     JSR SPIN2              \ Call SPIN2 to spawn A items of type X (i.e. spawn
                            \ 0-3 splinters)
    
    .nosp
    
     LDY #PLT               \ Randomly spawn some alloy plates
     JSR SPIN
    
     LDY #OIL               \ Randomly spawn some cargo canisters
     JSR SPIN
    
     LDX TYPE               \ Set X to the type of the ship that was killed so the
                            \ following call to EXNO2 can award us the correct
                            \ number of fractional kill points
    
     JSR EXNO2              \ Call EXNO2 to process the fact that we have killed a
                            \ ship (so increase the kill tally, make an explosion
                            \ sound and so on)
    
    .MA14
    
     STA INWK+35            \ Store the hit ship's updated energy in ship byte #35
    
     LDA TYPE               \ Call ANGRY to make the target ship or station hostile,
     JSR ANGRY              \ and if this is a ship, wake up its AI and give it a
                            \ kick of speed
    

[X]

Subroutine [ABORT2](abort2.md) (category: Dashboard)

Set/unset the lock target for a missile and update the dashboard

[X]

Subroutine [ANGRY](angry.md) (category: Tactics)

Make a ship or station hostile, and if this is a ship then enable the ship's AI and give it a kick of speed

[X]

Configuration variable [AST](../../all/workspaces.md#ast) = 7

Ship type for an asteroid

[X]

Configuration variable [Armlas](../../all/workspaces.md#armlas) = INT(128.5 + 1.5*POW)

Military laser power

[X]

Subroutine [BEEP](beep.md) (category: Sound)

Make a short, high beep

[X]

Label [BURN](main_flight_loop_part_11_of_16.md#burn) is local to this routine

[X]

Configuration variable [CON](../../all/workspaces.md#con) = 31

Ship type for a Constrictor

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [EXNO](exno.md) (category: Sound)

Make the sound of a laser strike on another ship or a ship explosion

[X]

Subroutine [EXNO2](exno2.md) (category: Status)

Process us making a kill

[X]

Subroutine [HITCH](hitch.md) (category: Tactics)

Work out if the ship in INWK is in our crosshairs

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [LAS](../workspace/zp.md#las) in workspace [ZP](../workspace/zp.md)

Contains the laser power of the laser fitted to the current space view (or 0 if there is no laser fitted to the current view)

[X]

Label [MA14](main_flight_loop_part_11_of_16.md#ma14) is local to this routine

[X]

[X]

Label [MA15](main_flight_loop_part_12_of_16.md#ma15) in subroutine [Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md)

[X]

Label [MA47](main_flight_loop_part_11_of_16.md#ma47) is local to this routine

[X]

Label [MA8](main_flight_loop_part_12_of_16.md#ma8) in subroutine [Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md)

[X]

Variable [MSAR](../workspace/wp.md#msar) in workspace [WP](../workspace/wp.md)

The targeting state of our leftmost missile

[X]

Configuration variable [Mlas](../../all/workspaces.md#mlas) = 50

Mining laser power

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Configuration variable [OIL](../../all/workspaces.md#oil) = 5

Ship type for a cargo canister

[X]

Configuration variable [PLT](../../all/workspaces.md#plt) = 4

Ship type for an alloy plate

[X]

Subroutine [PLUT](plut.md) (category: Flight)

Flip the coordinate axes for the four different views

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Configuration variable [RED2](../../all/workspaces.md#red2) = %00000011

Two mode 2 pixels of colour 1 (red)

[X]

Subroutine [SCAN](scan.md) (category: Dashboard)

Display the current ship on the scanner

[X]

Subroutine [SPIN](spin.md) (category: Universe)

Randomly spawn cargo from a destroyed ship

[X]

Entry point [SPIN2](spin.md#spin2) in subroutine [SPIN](spin.md) (category: Universe)

Remove any randomness: spawn cargo of a specific type (given in X), and always spawn the number given in A

[X]

Configuration variable [SPL](../../all/workspaces.md#spl) = 8

Ship type for a splinter

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Variable [XSAV](../workspace/zp.md#xsav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Label [nosp](main_flight_loop_part_11_of_16.md#nosp) is local to this routine

[Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md "Previous routine")[Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md "Next routine")
