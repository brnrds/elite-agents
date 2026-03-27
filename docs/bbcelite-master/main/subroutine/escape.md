---
title: "The ESCAPE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/escape.html"
---

[BELL](bell.md "Previous routine")[HME2](hme2.md "Next routine")
    
    
           Name: ESCAPE                                                  [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Launch our escape pod
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-escape)
     Variations: See [code variations](../../related/compare/main/subroutine/escape.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls ESCAPE
    
    
    
    
    
    * * *
    
    
     This routine displays our doomed Cobra Mk III disappearing off into the ether
     before arranging our replacement ship. Called when we press ESCAPE during
     flight and have an escape pod fitted.
    
    
    
    
    .ESCAPE
    
     JSR RES2               \ Reset a number of flight variables and workspaces
    
     LDX #CYL               \ Set the current ship type to a Cobra Mk III, so we
     STX TYPE               \ can show our ship disappear into the distance when we
                            \ eject in our pod
    
     JSR FRS1               \ Call FRS1 to launch the Cobra Mk III straight ahead,
                            \ like a missile launch, but with our ship instead
    
     BCS ES1                \ If the Cobra was successfully added to the local
                            \ bubble, jump to ES1 to skip the following instructions
    
     LDX #CYL2              \ The Cobra wasn't added to the local bubble for some
     JSR FRS1               \ reason, so try launching a pirate Cobra Mk III instead
    
    .ES1
    
     LDA #8                 \ Set the Cobra's byte #27 (speed) to 8
     STA INWK+27
    
     LDA #194               \ Set the Cobra's byte #30 (pitch counter) to 194, so it
     STA INWK+30            \ pitches up as we pull away
    
     LSR A                  \ Set the Cobra's byte #32 (AI flag) to %01100001, so it
     STA INWK+32            \ has no AI, and we can use this value as a counter to
                            \ do the following loop 97 times
    
    .ESL1
    
     JSR MVEIT              \ Call MVEIT to move the Cobra in space
    
     LDA QQ11               \ If either of QQ11 or VIEW is non-zero (i.e. this is
     ORA VIEW               \ not the front space view), skip the following
     BNE P%+5               \ instruction
    
     JSR LL9                \ Call LL9 to draw the Cobra on-screen
    
     DEC INWK+32            \ Decrement the counter in byte #32
    
     BNE ESL1               \ Loop back to keep moving the Cobra until the AI flag
                            \ is 0, which gives it time to drift away from our pod
    
     JSR SCAN               \ Call SCAN to remove the Cobra from the scanner (by
                            \ redrawing it)
    
     LDA #0                 \ Set A = 0 so we can use it to zero the contents of
                            \ the cargo hold
    
     LDX #16                \ We lose all our cargo when using our escape pod, so
                            \ up a counter in X so we can zero the 17 cargo slots
                            \ in QQ20
    
    .ESL2
    
     STA QQ20,X             \ Set the X-th byte of QQ20 to zero, so we no longer
                            \ have any of item type X in the cargo hold
    
     DEX                    \ Decrement the counter
    
     BPL ESL2               \ Loop back to ESL2 until we have emptied the entire
                            \ cargo hold
    
     STA FIST               \ Launching an escape pod also clears our criminal
                            \ record, so set our legal status in FIST to 0 ("clean")
    
     STA ESCP               \ The escape pod is a one-use item, so set ESCP to 0 so
                            \ we no longer have one fitted
    
    \LDA TRIBBLE            \ These instructions are commented out in the original
    \ORA TRIBBLE+1          \ source
    \BEQ nosurviv           \
    \                       \ They ensure that in games with the Trumble mission,
    \JSR DORND              \ at least one Trumble will hitch a ride in the escape
    \AND #7                 \ pod (so using an escape pod is not a solution to the
    \ORA #1                 \ trouble with Trumbles)
    \STA TRIBBLE            \
    \LDA #0                 \ This version of Elite does not contain the Trumble
    \STA TRIBBLE+1          \ mission, so the code is disabled
    \
    \.nosurviv
    
     LDA #70                \ Our replacement ship is delivered with a full tank of
     STA QQ14               \ fuel, so set the current fuel level in QQ14 to 70, or
                            \ 7.0 light years
    
     JMP GOIN               \ Go to the docking bay (i.e. show the ship hangar
                            \ screen) and return from the subroutine with a tail
                            \ call
    

[X]

Configuration variable [CYL](../../all/workspaces.md#cyl) = 11

Ship type for a Cobra Mk III

[X]

Configuration variable [CYL2](../../all/workspaces.md#cyl2) = 24

Ship type for a Cobra Mk III (pirate)

[X]

Label [ES1](escape.md#es1) is local to this routine

[X]

Variable [ESCP](../workspace/wp.md#escp) in workspace [WP](../workspace/wp.md)

Escape pod

[X]

Label [ESL1](escape.md#esl1) is local to this routine

[X]

Label [ESL2](escape.md#esl2) is local to this routine

[X]

Variable [FIST](../workspace/wp.md#fist) in workspace [WP](../workspace/wp.md)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Subroutine [FRS1](frs1.md) (category: Tactics)

Launch a ship straight ahead of us, below the laser sights

[X]

Entry point [GOIN](main_flight_loop_part_9_of_16.md#goin) in subroutine [Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md) (category: Main loop)

We jump here from part 3 of the main flight loop if the docking computer is activated by pressing "C"

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Subroutine [MVEIT (Part 1 of 9)](mveit_part_1_of_9.md) (category: Moving)

Move current ship: Tidy the orientation vectors

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Variable [QQ14](../workspace/wp.md#qq14) in workspace [WP](../workspace/wp.md)

Our current fuel level (0-70)

[X]

Variable [QQ20](../workspace/wp.md#qq20) in workspace [WP](../workspace/wp.md)

The contents of our cargo hold

[X]

Subroutine [RES2](res2.md) (category: Start and end)

Reset a number of flight variables and workspaces

[X]

Subroutine [SCAN](scan.md) (category: Dashboard)

Display the current ship on the scanner

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Variable [VIEW](../workspace/wp.md#view) in workspace [WP](../workspace/wp.md)

The number of the current space view

[BELL](bell.md "Previous routine")[HME2](hme2.md "Next routine")
