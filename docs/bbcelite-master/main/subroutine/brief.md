---
title: "The BRIEF subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/brief.html"
---

[TBRIEF](tbrief.md "Previous routine")[BRIS](bris.md "Next routine")
    
    
           Name: BRIEF                                                   [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Start mission 1 and show the mission briefing
      Deep dive: [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-brief)
     Variations: See [code variations](../../related/compare/main/subroutine/brief.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOENTRY](doentry.md) calls BRIEF
    
    
    
    
    
    * * *
    
    
     This routine does the following:
    
       * Clear the screen
       * Display "INCOMING MESSAGE" in the middle of the screen
       * Wait for 2 seconds
       * Clear the screen
       * Show the Constrictor rolling and pitching in the middle of the screen
       * Do this for 64 loop iterations
       * Move the ship away from us and up until it's near the top of the screen
       * Show the mission 1 briefing in extended token 10
    
     The mission briefing ends with a "{display ship, wait for key press}" token,
     which calls the PAUSE routine. This continues to display the rotating ship,
     waiting until a key is pressed, and then removes the ship from the screen.
    
    
    
    
    .BRIEF
    
     LSR TP                 \ Set bit 0 of TP to indicate that mission 1 is now in
     SEC                    \ progress
     ROL TP
    
     JSR BRIS               \ Call BRIS to clear the screen, display "INCOMING
                            \ MESSAGE" and wait for 2 seconds
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     LDA #CON               \ Set the ship type in TYPE to the Constrictor
     STA TYPE
    
     JSR NWSHP              \ Add a new Constrictor to the local bubble (in this
                            \ case, the briefing screen)
    
     LDA #1                 \ Move the text cursor to column 1
     JSR DOXC
    
     STA INWK+7             \ Set z_hi = 1, the distance at which we show the
                            \ rotating ship
    
     LDA #13                \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 13 (rotating
                            \ ship view)
    
     LDA #64                \ Set the main loop counter to 64, so the ship rotates
     STA MCNT               \ for 64 iterations through MVEIT
    
    .BRL1
    
     LDX #%01111111         \ Set the ship's roll counter to a positive roll that
     STX INWK+29            \ doesn't dampen (a clockwise roll)
    
     STX INWK+30            \ Set the ship's pitch counter to a positive pitch that
                            \ doesn't dampen (a diving pitch)
    
     JSR LL9                \ Draw the ship on screen
    
     JSR MVEIT              \ Call MVEIT to rotate the ship in space
    
     DEC MCNT               \ Decrease the counter in MCNT
    
     BNE BRL1               \ Loop back to keep moving the ship until we have done
                            \ all 64 iterations
    
    .BRL2
    
     LSR INWK               \ Halve x_lo so the Constrictor moves towards the centre
    
     INC INWK+6             \ Increment z_lo so the Constrictor moves away from us
    
     BEQ BR2                \ If z_lo = 0 (i.e. it just went past 255), jump to BR2
                            \ to show the briefing
    
     INC INWK+6             \ Increment z_lo so the Constrictor moves a bit further
                            \ away from us
    
     BEQ BR2                \ If z_lo = 0 (i.e. it just went past 255), jump out of
                            \ the loop to BR2 to stop moving the ship up the screen
                            \ and show the briefing
    
     LDX INWK+3             \ Set X = y_lo + 1
     INX
    
     CPX #120               \ If X < 120 then skip the next instruction
     BCC P%+4
    
     LDX #120               \ X is bigger than 120, so set X = 120 so that X has a
                            \ maximum value of 120
    
     STX INWK+3             \ Set y_lo = X
                            \          = y_lo + 1
                            \
                            \ so the ship moves up the screen (as space coordinates
                            \ have the y-axis going up)
    
     JSR LL9                \ Draw the ship on screen
    
     JSR MVEIT              \ Call MVEIT to move and rotate the ship in space
    
     DEC MCNT               \ Decrease the counter in MCNT
    
     JMP BRL2               \ Loop back to keep moving the ship up the screen and
                            \ away from us
    
    .BR2
    
     INC INWK+7             \ Increment z_hi, to keep the ship at the same distance
                            \ as we just incremented z_lo past 255
    
     JSR PAS1               \ Call PAS1 to display the rotating ship at space
                            \ coordinates (0, 112, 256) and scan the keyboard,
                            \ returning the ASCII code of the key in X (or 0 for no
                            \ key press)
    
     LDA #10                \ Set A = 10 so the call to BRP prints extended token 10
                            \ (the briefing for mission 1 where we find out all
                            \ about the stolen Constrictor)
    
     BNE BRPS               \ Jump to BRP via BRPS to print the extended token in A
                            \ and show the Status Mode screen, returning from the
                            \ subroutine using a tail call (this BNE is effectively
                            \ a JMP as A is never zero)
    

[X]

Label [BR2](brief.md#br2) is local to this routine

[X]

Subroutine [BRIS](bris.md) (category: Missions)

Clear the screen, display "INCOMING MESSAGE" and wait for 2 seconds

[X]

Label [BRL1](brief.md#brl1) is local to this routine

[X]

Label [BRL2](brief.md#brl2) is local to this routine

[X]

Entry point [BRPS](debrief.md#brps) in subroutine [DEBRIEF](debrief.md) (category: Missions)

Print the extended token in A, show the Status Mode screen and return from the subroutine

[X]

Configuration variable [CON](../../all/workspaces.md#con) = 31

Ship type for a Constrictor

[X]

Subroutine [DOXC](doxc.md) (category: Text)

Move the text cursor to a specific column

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Subroutine [MVEIT (Part 1 of 9)](mveit_part_1_of_9.md) (category: Moving)

Move current ship: Tidy the orientation vectors

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Subroutine [PAS1](pas1.md) (category: Missions)

Display a rotating ship at space coordinates (0, 120, 256) and scan the keyboard

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Subroutine [ZINF](zinf.md) (category: Universe)

Reset the INWK workspace and orientation vectors

[TBRIEF](tbrief.md "Previous routine")[BRIS](bris.md "Next routine")
