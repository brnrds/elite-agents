---
title: "The Main flight loop (Part 3 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_3_of_16.html"
---

[Main flight loop (Part 2 of 16)](main_flight_loop_part_2_of_16.md "Previous routine")[Main flight loop (Part 4 of 16)](main_flight_loop_part_4_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 3 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Scan for flight keys and process the results
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-3-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_3_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Scan for flight keys and process the results
    
     Flight keys are logged in the key logger at location KY1 onwards, with a
     non-zero value in the relevant location indicating a key press.
    
     The key presses that are processed are as follows:
    
       * Space and "?" to speed up and slow down
       * "U", "T" and "M" to disarm, arm and fire missiles
       * TAB to fire an energy bomb
       * ESCAPE to launch an escape pod
       * "J" to initiate an in-system jump
       * "E" to deploy E.C.M. anti-missile countermeasures
       * "C" to use the docking computer
       * "A" to fire lasers
    
    
    
    
    .BS2
    
     LDA KY2                \ If Space is being pressed, keep going, otherwise jump
     BEQ MA17               \ down to MA17 to skip the following
    
     LDA DELTA              \ The "go faster" key is being pressed, so first we
     CMP #40                \ fetch the current speed from DELTA into A, and if
     BCS MA17               \ A >= 40, we are already going at full pelt, so jump
                            \ down to MA17 to skip the following
    
     INC DELTA              \ We can go a bit faster, so increment the speed in
                            \ location DELTA
    
    .MA17
    
     LDA KY1                \ If "?" is being pressed, keep going, otherwise jump
     BEQ MA4                \ down to MA4 to skip the following
    
     DEC DELTA              \ The "slow down" key is being pressed, so we decrement
                            \ the current ship speed in DELTA
    
     BNE MA4                \ If the speed is still greater than zero, jump to MA4
    
     INC DELTA              \ Otherwise we just braked a little too hard, so bump
                            \ the speed back up to the minimum value of 1
    
    .MA4
    
     LDA KY15               \ If "U" is being pressed and the number of missiles
     AND NOMSL              \ in NOMSL is non-zero, keep going, otherwise jump down
     BEQ MA20               \ to MA20 to skip the following
    
     LDY #GREEN2            \ The "disarm missiles" key is being pressed, so call
     JSR ABORT              \ ABORT to disarm the missile and update the missile
                            \ indicators on the dashboard to green (Y = &EE)
    
     JSR BOOP               \ Call the BOOP routine to make a low, long beep to
                            \ indicate the missile is now disarmed
    
     LDA #0                 \ Set MSAR to 0 to indicate that no missiles are
     STA MSAR               \ currently armed
    
    .MA20
    
     LDA MSTG               \ If MSTG is positive (i.e. it does not have bit 7 set),
     BPL MA25               \ then it indicates we already have a missile locked on
                            \ a target (in which case MSTG contains the ship number
                            \ of the target), so jump to MA25 to skip targeting. Or
                            \ to put it another way, if MSTG = &FF, which means
                            \ there is no current target lock, keep going
    
     LDA KY14               \ If "T" is being pressed, keep going, otherwise jump
     BEQ MA25               \ down to MA25 to skip the following
    
     LDX NOMSL              \ If the number of missiles in NOMSL is zero, jump down
     BEQ MA25               \ to MA25 to skip the following
    
     STA MSAR               \ The "target missile" key is being pressed and we have
                            \ at least one missile, so set MSAR = &FF to denote that
                            \ our missile is currently armed (we know A has the
                            \ value &FF, as we just loaded it from MSTG and checked
                            \ that it was negative)
    
     LDY #YELLOW2           \ Change the leftmost missile indicator to yellow
     JSR MSBAR              \ on the missile bar (this call changes the leftmost
                            \ indicator because we set X to the number of missiles
                            \ in NOMSL above, and the indicators are numbered from
                            \ right to left, so X is the number of the leftmost
                            \ indicator)
    
    .MA25
    
     LDA KY16               \ If "M" is being pressed, keep going, otherwise jump
     BEQ MA24               \ down to MA24 to skip the following
    
     LDA MSTG               \ If MSTG = &FF then there is no target lock, so jump to
     BMI MA64               \ MA64 to skip the following (also skipping the checks
                            \ for TAB, ESCAPE, "J" and "E")
    
     JSR FRMIS              \ The "fire missile" key is being pressed and we have
                            \ a missile lock, so call the FRMIS routine to fire
                            \ the missile
    
    .MA24
    
     LDA KY12               \ If TAB is being pressed, keep going, otherwise jump
     BEQ MA76               \ down to MA76 to skip the following
    
     LDA BOMB               \ If we already set off our energy bomb, then BOMB is
     BMI MA76               \ negative, so this skips to MA76 if our energy bomb is
                            \ already going off
    
     ASL BOMB               \ The "energy bomb" key is being pressed, so double
                            \ the value in BOMB. If we have an energy bomb fitted,
                            \ BOMB will contain &7F (%01111111) before this shift
                            \ and will contain &FE (%11111110) after the shift; if
                            \ we don't have an energy bomb fitted, BOMB will still
                            \ contain 0. The bomb explosion is dealt with in the
                            \ MAL1 routine below - this just registers the fact that
                            \ we've set the bomb ticking
    
     BEQ MA76               \ If BOMB now contains 0, then the bomb is not going off
                            \ any more (or it never was), so skip the following
                            \ instruction
    
     JSR BOMBON             \ Call BOMBON to set up and display a new energy bomb
                            \ zig-zag lightning bolt
    
    .MA76
    
     LDA KY20               \ If "P" is being pressed, keep going, otherwise skip
     BEQ MA78               \ the next two instructions
    
     LDA #0                 \ The "cancel docking computer" key is bring pressed,
     STA auto               \ so turn it off by setting auto to 0
    
    \JSR stopbd             \ This instruction is commented out in the original
                            \ source
    
    .MA78
    
     LDA KY13               \ If ESCAPE is being pressed and we have an escape pod
     AND ESCP               \ fitted, keep going, otherwise jump to noescp to skip
     BEQ noescp             \ the following instructions
    
     LDA MJ                 \ If we are in witchspace, we can't launch our escape
     BNE noescp             \ pod, so jump down to noescp
    
     JMP ESCAPE             \ The button is being pressed to launch an escape pod
                            \ and we have an escape pod fitted, so jump to ESCAPE to
                            \ launch it, and exit the main flight loop using a tail
                            \ call
    
    .noescp
    
     LDA KY18               \ If "J" is being pressed, keep going, otherwise skip
     BEQ P%+5               \ the next instruction
    
     JSR WARP               \ Call the WARP routine to do an in-system jump
    
     LDA KY17               \ If "E" is being pressed and we have an E.C.M. fitted,
     AND ECM                \ keep going, otherwise jump down to MA64 to skip the
     BEQ MA64               \ following
    
     LDA ECMA               \ If ECMA is non-zero, that means an E.C.M. is already
     BNE MA64               \ operating and is counting down (this can be either
                            \ our E.C.M. or an opponent's), so jump down to MA64 to
                            \ skip the following (as we can't have two E.C.M.
                            \ systems operating at the same time)
    
     DEC ECMP               \ The E.C.M. button is being pressed and nobody else
                            \ is operating their E.C.M., so decrease the value of
                            \ ECMP to make it non-zero, to denote that our E.C.M.
                            \ is now on
    
     JSR ECBLB2             \ Call ECBLB2 to light up the E.C.M. indicator bulb on
                            \ the dashboard, set the E.C.M. countdown timer to 32,
                            \ and start making the E.C.M. sound
    
    .MA64
    
     LDA KY19               \ If "C" is being pressed, and we have a docking
     AND DKCMP              \ computer fitted, keep going, otherwise jump down to
     BEQ MA68               \ MA68 to skip the following
    
     STA auto               \ Set auto to the non-zero value of A, so the docking
                            \ computer is activated
    
    \EOR KLO+&29            \ These instructions are commented out in the original
    \BEQ MA68               \ source
    \
    \STA auto
    \
    \JSR startbd
    \
    \kill phantom Cs
    
    .MA68
    
     LDA #0                 \ Set LAS = 0, to switch the laser off while we do the
     STA LAS                \ following logic
    
     STA DELT4              \ Take the 16-bit value (DELTA 0) - i.e. a two-byte
     LDA DELTA              \ number with DELTA as the high byte and 0 as the low
     LSR A                  \ byte - and divide it by 4, storing the 16-bit result
     ROR DELT4              \ in DELT4(1 0). This has the effect of storing the
     LSR A                  \ current speed * 64 in the 16-bit location DELT4(1 0)
     ROR DELT4
     STA DELT4+1
    
     LDA LASCT              \ If LASCT is zero, keep going, otherwise the laser is
     BNE MA3                \ a pulse laser that is between pulses, so jump down to
                            \ MA3 to skip the following
    
     LDA KY7                \ If "A" is being pressed, keep going, otherwise jump
     BEQ MA3                \ down to MA3 to skip the following
    
     LDA GNTMP              \ If the laser temperature >= 242 then the laser has
     CMP #242               \ overheated, so jump down to MA3 to skip the following
     BCS MA3
    
     LDX VIEW               \ If the current space view has a laser fitted (i.e. the
     LDA LASER,X            \ laser power for this view is greater than zero), then
     BEQ MA3                \ keep going, otherwise jump down to MA3 to skip the
                            \ following
    
                            \ If we get here, then the "fire" button is being
                            \ pressed, our laser hasn't overheated and isn't already
                            \ being fired, and we actually have a laser fitted to
                            \ the current space view, so it's time to hit me with
                            \ those laser beams
    
     PHA                    \ Store the current view's laser power on the stack
    
     AND #%01111111         \ Set LAS and LAS2 to bits 0-6 of the laser power
     STA LAS
     STA LAS2
    
     JSR LASNO              \ Call the LASNO routine to make the sound of our laser
                            \ firing
    
     JSR LASLI              \ Call LASLI to draw the laser lines
    
     PLA                    \ Restore the current view's laser power into A
    
     BPL ma1                \ If the laser power has bit 7 set, then it's an "always
                            \ on" laser rather than a pulsing laser, so keep going,
                            \ otherwise jump down to ma1 to skip the following
                            \ instruction
    
     LDA #0                 \ This is an "always on" laser (i.e. a beam laser or a
                            \ military laser), so set A = 0, which will be stored in
                            \ LASCT to denote that this is not a pulsing laser
    
    .ma1
    
     AND #%11111010         \ LASCT will be set to 0 for beam lasers, and to the
     STA LASCT              \ laser power AND %11111010 for pulse lasers, which
                            \ comes to 10 for pulse lasers (as pulse lasers have a
                            \ power of 15) or 50 for mining lasers (as mining
                            \ lasers hava a power of 50). See MA23 in part 16 for
                            \ more on laser pulsing and LASCT
    

[X]

Subroutine [ABORT](abort.md) (category: Dashboard)

Disarm missiles and update the dashboard indicators

[X]

Variable [BOMB](../workspace/wp.md#bomb) in workspace [WP](../workspace/wp.md)

Energy bomb

[X]

Subroutine [BOMBON](bombon.md) (category: Drawing lines)

Randomise and draw the energy bomb's zig-zag lightning bolt lines

[X]

Subroutine [BOOP](boop.md) (category: Sound)

Make a long, low beep

[X]

Variable [DELT4](../workspace/zp.md#delt4) in workspace [ZP](../workspace/zp.md)

Our current speed * 64 as a 16-bit value

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Variable [DKCMP](../workspace/wp.md#dkcmp) in workspace [WP](../workspace/wp.md)

Docking computer

[X]

Subroutine [ECBLB2](ecblb2.md) (category: Dashboard)

Start up the E.C.M. (light up the indicator, start the countdown and make the E.C.M. sound)

[X]

Variable [ECM](../workspace/wp.md#ecm) in workspace [WP](../workspace/wp.md)

E.C.M. system

[X]

Variable [ECMA](../workspace/zp.md#ecma) in workspace [ZP](../workspace/zp.md)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Variable [ECMP](../workspace/wp.md#ecmp) in workspace [WP](../workspace/wp.md)

Our E.C.M. status

[X]

Subroutine [ESCAPE](escape.md) (category: Flight)

Launch our escape pod

[X]

Variable [ESCP](../workspace/wp.md#escp) in workspace [WP](../workspace/wp.md)

Escape pod

[X]

Subroutine [FRMIS](frmis.md) (category: Tactics)

Fire a missile from our ship

[X]

Variable [GNTMP](../workspace/wp.md#gntmp) in workspace [WP](../workspace/wp.md)

Laser temperature (or "gun temperature")

[X]

Configuration variable [GREEN2](../../all/workspaces.md#green2) = %00001100

Two mode 2 pixels of colour 2 (green)

[X]

Variable [KY1](../workspace/zp.md#ky1) in workspace [ZP](../workspace/zp.md)

"?" is being pressed (slow down)

[X]

Variable [KY12](../workspace/zp.md#ky12) in workspace [ZP](../workspace/zp.md)

TAB is being pressed (energy bomb)

[X]

Variable [KY13](../workspace/zp.md#ky13) in workspace [ZP](../workspace/zp.md)

ESCAPE is being pressed (launch escape pod)

[X]

Variable [KY14](../workspace/zp.md#ky14) in workspace [ZP](../workspace/zp.md)

"T" is being pressed (target missile)

[X]

Variable [KY15](../workspace/zp.md#ky15) in workspace [ZP](../workspace/zp.md)

"U" is being pressed (unarm missile)

[X]

Variable [KY16](../workspace/zp.md#ky16) in workspace [ZP](../workspace/zp.md)

"M" is being pressed (fire missile)

[X]

Variable [KY17](../workspace/zp.md#ky17) in workspace [ZP](../workspace/zp.md)

"E" is being pressed (activate E.C.M.)

[X]

Variable [KY18](../workspace/zp.md#ky18) in workspace [ZP](../workspace/zp.md)

"J" is being pressed (in-system jump)

[X]

Variable [KY19](../workspace/zp.md#ky19) in workspace [ZP](../workspace/zp.md)

"C" is being pressed (activate docking computer)

[X]

Variable [KY2](../workspace/zp.md#ky2) in workspace [ZP](../workspace/zp.md)

Space is being pressed (speed up)

[X]

Variable [KY20](../workspace/zp.md#ky20) in workspace [ZP](../workspace/zp.md)

"P" is being pressed (deactivate docking computer)

[X]

Variable [KY7](../workspace/zp.md#ky7) in workspace [ZP](../workspace/zp.md)

"A" is being pressed (fire lasers)

[X]

Variable [LAS](../workspace/zp.md#las) in workspace [ZP](../workspace/zp.md)

Contains the laser power of the laser fitted to the current space view (or 0 if there is no laser fitted to the current view)

[X]

Variable [LAS2](../workspace/wp.md#las2) in workspace [WP](../workspace/wp.md)

Laser power for the current laser

[X]

Variable [LASCT](../workspace/wp.md#lasct) in workspace [WP](../workspace/wp.md)

The laser pulse count for the current laser

[X]

Variable [LASER](../workspace/wp.md#laser) in workspace [WP](../workspace/wp.md)

The specifications of the lasers fitted to each of the four space views

[X]

Subroutine [LASLI](lasli.md) (category: Drawing lines)

Draw the laser lines for when we fire our lasers

[X]

Subroutine [LASNO](lasno.md) (category: Sound)

Make the sound of our laser firing

[X]

Label [MA17](main_flight_loop_part_3_of_16.md#ma17) is local to this routine

[X]

Label [MA20](main_flight_loop_part_3_of_16.md#ma20) is local to this routine

[X]

Label [MA24](main_flight_loop_part_3_of_16.md#ma24) is local to this routine

[X]

Label [MA25](main_flight_loop_part_3_of_16.md#ma25) is local to this routine

[X]

Label [MA3](main_flight_loop_part_4_of_16.md#ma3) in subroutine [Main flight loop (Part 4 of 16)](main_flight_loop_part_4_of_16.md)

[X]

Label [MA4](main_flight_loop_part_3_of_16.md#ma4) is local to this routine

[X]

Label [MA64](main_flight_loop_part_3_of_16.md#ma64) is local to this routine

[X]

Label [MA68](main_flight_loop_part_3_of_16.md#ma68) is local to this routine

[X]

Label [MA76](main_flight_loop_part_3_of_16.md#ma76) is local to this routine

[X]

Label [MA78](main_flight_loop_part_3_of_16.md#ma78) is local to this routine

[X]

Variable [MJ](../workspace/wp.md#mj) in workspace [WP](../workspace/wp.md)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Variable [MSAR](../workspace/wp.md#msar) in workspace [WP](../workspace/wp.md)

The targeting state of our leftmost missile

[X]

Subroutine [MSBAR](msbar.md) (category: Dashboard)

Draw a specific indicator in the dashboard's missile bar

[X]

Variable [MSTG](../workspace/zp.md#mstg) in workspace [ZP](../workspace/zp.md)

The current missile lock target

[X]

Variable [NOMSL](../workspace/wp.md#nomsl) in workspace [WP](../workspace/wp.md)

The number of missiles we have fitted (0-4)

[X]

Variable [VIEW](../workspace/wp.md#view) in workspace [WP](../workspace/wp.md)

The number of the current space view

[X]

Subroutine [WARP](warp.md) (category: Flight)

Perform an in-system jump

[X]

Configuration variable [YELLOW2](../../all/workspaces.md#yellow2) = %00001111

Two mode 2 pixels of colour 3 (yellow)

[X]

Variable [auto](../workspace/wp.md#auto) in workspace [WP](../workspace/wp.md)

Docking computer activation status

[X]

Label [ma1](main_flight_loop_part_3_of_16.md#ma1) is local to this routine

[X]

Label [noescp](main_flight_loop_part_3_of_16.md#noescp) is local to this routine

[Main flight loop (Part 2 of 16)](main_flight_loop_part_2_of_16.md "Previous routine")[Main flight loop (Part 4 of 16)](main_flight_loop_part_4_of_16.md "Next routine")
