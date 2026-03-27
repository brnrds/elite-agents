---
title: "The Main flight loop (Part 8 of 16) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_flight_loop_part_8_of_16.html"
---

[Main flight loop (Part 7 of 16)](main_flight_loop_part_7_of_16.md "Previous routine")[Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md "Next routine")
    
    
           Name: Main flight loop (Part 8 of 16)                         [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: For each nearby ship: Process us potentially scooping this item
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-main-flight-loop-part-8-of-16)
     Variations: See [code variations](../../related/compare/main/subroutine/main_flight_loop_part_8_of_16.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     The main flight loop covers most of the flight-specific aspects of Elite. This
     section covers the following:
    
       * Continue looping through all the ships in the local bubble, and for each
         one:
    
         * Process us potentially scooping this item
    
    
    
    
     CPX #OIL               \ If this is a cargo canister, jump to oily to randomly
     BEQ oily               \ decide the canister's contents
    
     LDY #0                 \ Fetch byte #0 of the ship's blueprint
     LDA (XX0),Y
    
     LSR A                  \ Shift it right four times, so A now contains the high
     LSR A                  \ nibble (i.e. bits 4-7)
     LSR A
     LSR A
    
     BEQ MA58               \ If A = 0, jump to MA58 to skip all the docking and
                            \ scooping checks
    
                            \ Only the Thargon, alloy plate, splinter and escape pod
                            \ have non-zero high nibbles in their blueprint byte #0
                            \ so if we get here, our ship is one of those, and the
                            \ high nibble gives the market item number of the item
                            \ when scooped, less 1
    
     ADC #1                 \ Add 1 to the high nibble to get the market item
                            \ number
    
     BNE slvy2              \ Skip to slvy2 so we scoop the ship as a market item
    
    .oily
    
     JSR DORND              \ Set A and X to random numbers and reduce A to a
     AND #7                 \ random number in the range 0-7
    
    .slvy2
    
                            \ By the time we get here, we are scooping, and A
                            \ contains the type of item we are scooping (a random
                            \ number 0-7 if we are scooping a cargo canister, 3 if
                            \ we are scooping an escape pod, or 16 if we are
                            \ scooping a Thargon). These numbers correspond to the
                            \ relevant market items (see QQ23 for a list), so a
                            \ cargo canister can contain anything from food to
                            \ computers, while escape pods contain slaves, and
                            \ Thargons become alien items when scooped
    
     JSR tnpr1              \ Call tnpr1 with the scooped cargo type stored in A
                            \ to work out whether we have room in the hold for one
                            \ tonne of this cargo (A is set to 1 by this call, and
                            \ the C flag contains the result)
    
     LDY #78                \ This instruction has no effect, so presumably it used
                            \ to do something, but didn't get removed
    
     BCS MA59               \ If the C flag is set then we have no room in the hold
                            \ for the scooped item, so jump down to MA59 make a
                            \ sound to indicate failure, before destroying the
                            \ canister
    
     LDY QQ29               \ Scooping was successful, so set Y to the type of
                            \ item we just scooped, which we stored in QQ29 above
    
     ADC QQ20,Y             \ Add A (which we set to 1 above) to the number of items
     STA QQ20,Y             \ of type Y in the cargo hold, as we just successfully
                            \ scooped one canister of type Y
    
     TYA                    \ Print recursive token 48 + Y as an in-flight token,
     ADC #208               \ which will be in the range 48 ("FOOD") to 64 ("ALIEN
     JSR MESS               \ ITEMS"), so this prints the scooped item's name
    
     ASL NEWB               \ The item has now been scooped, so set bit 7 of its
     SEC                    \ NEWB flags to indicate this
     ROR NEWB
    
    .MA65
    
     JMP MA26               \ If we get here, then the ship we are processing was
                            \ too far away to be scooped, docked or collided with,
                            \ so jump to MA26 to skip over the collision routines
                            \ and move on to missile targeting
    

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Label [MA26](main_flight_loop_part_11_of_16.md#ma26) in subroutine [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md)

[X]

Label [MA58](main_flight_loop_part_10_of_16.md#ma58) in subroutine [Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md)

[X]

Label [MA59](main_flight_loop_part_10_of_16.md#ma59) in subroutine [Main flight loop (Part 10 of 16)](main_flight_loop_part_10_of_16.md)

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Configuration variable [OIL](../../all/workspaces.md#oil) = 5

Ship type for a cargo canister

[X]

Variable [QQ20](../workspace/wp.md#qq20) in workspace [WP](../workspace/wp.md)

The contents of our cargo hold

[X]

Variable [QQ29](../workspace/wp.md#qq29) in workspace [WP](../workspace/wp.md)

Temporary storage, used in a number of places

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Label [oily](main_flight_loop_part_8_of_16.md#oily) is local to this routine

[X]

Label [slvy2](main_flight_loop_part_8_of_16.md#slvy2) is local to this routine

[X]

Subroutine [tnpr1](tnpr1.md) (category: Market)

Work out if we have space for one tonne of cargo

[Main flight loop (Part 7 of 16)](main_flight_loop_part_7_of_16.md "Previous routine")[Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md "Next routine")
