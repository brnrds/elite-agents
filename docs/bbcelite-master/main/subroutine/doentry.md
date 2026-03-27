---
title: "The DOENTRY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/doentry.html"
---

[G%](../variable/g_per_cent.md "Previous routine")[TRIBDIR](../variable/tribdir.md "Next routine")
    
    
           Name: DOENTRY                                                 [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Dock at the space station, show the ship hangar and work out any
                 mission progression
      Deep dive: [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
                 [The Thargoid Plans mission](https://elite.bbcelite.com/deep_dives/the_thargoid_plans_mission.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-doentry)
     Variations: See [code variations](../../related/compare/main/subroutine/doentry.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md) calls DOENTRY
    
    
    
    
    
    
    .DOENTRY
    
     JSR RES2               \ Reset a number of flight variables and workspaces
    
     JSR LAUN               \ Show the space station docking tunnel
    
     LDA #0                 \ Reduce the speed to 0
     STA DELTA
    
     STA GNTMP              \ Cool down the lasers completely
    
     STA QQ22+1             \ Reset the on-screen hyperspace counter
    
     LDA #&FF               \ Recharge the forward and aft shields
     STA FSH
     STA ASH
    
     STA ENERGY             \ Recharge the energy banks
    
     JSR HALL               \ Show the ship hangar
    
     LDY #44                \ Wait for 44/50 of a second (0.88 seconds)
     JSR DELAY
    
     LDA TP                 \ Fetch bits 0 and 1 of TP, and if they are non-zero
     AND #%00000011         \ (i.e. mission 1 is either in progress or has been
     BNE EN1                \ completed), skip to EN1
    
     LDA TALLY+1            \ If the high byte of TALLY is zero (so we have a combat
     BEQ EN4                \ rank below Competent, or we are Competent but have not
                            \ yet earned a grand total of at least 256 kill points),
                            \ jump to EN4 as we are not yet good enough to qualify
                            \ for a mission
    
     LDA GCNT               \ Fetch the galaxy number into A, and if any of bits 1-7
     LSR A                  \ are set (i.e. A > 1), jump to EN4 as mission 1 can
     BNE EN4                \ only be triggered in the first two galaxies
    
     JMP BRIEF              \ If we get here then mission 1 hasn't started, we have
                            \ reached a combat rank of at least Competent plus 128
                            \ kill points, and we are in galaxy 0 or 1 (shown
                            \ in-game as galaxy 1 or 2), so it's time to start
                            \ mission 1 by calling BRIEF
    
    .EN1
    
                            \ If we get here then mission 1 is either in progress or
                            \ has been completed
    
     CMP #%00000011         \ If bits 0 and 1 are not both set, then jump to EN2
     BNE EN2
    
     JMP DEBRIEF            \ Bits 0 and 1 are both set, so mission 1 is both in
                            \ progress and has been completed, which means we have
                            \ only just completed it, so jump to DEBRIEF to end the
                            \ mission get our reward
    
    .EN2
    
                            \ Mission 1 has been completed, so now to check for
                            \ mission 2
    
     LDA GCNT               \ Fetch the galaxy number into A
    
     CMP #2                 \ If this is not galaxy 2 (shown in-game as galaxy 3),
     BNE EN4                \ jump to EN4 as we can only start mission 2 in the
                            \ third galaxy
    
     LDA TP                 \ Extract bits 0-3 of TP into A
     AND #%00001111
    
     CMP #%00000010         \ If mission 1 is complete and no longer in progress,
     BNE EN3                \ and mission 2 is not yet started, then bits 0-3 of TP
                            \ will be %0010, so this jumps to EN3 if this is not the
                            \ case
    
     LDA TALLY+1            \ If the high byte of TALLY is < 5 (so we have a combat
     CMP #5                 \ rank that is less than 3/8 of the way from Dangerous
     BCC EN4                \ to Deadly), jump to EN4 as our rank isn't high enough
                            \ for mission 2
    
     JMP BRIEF2             \ If we get here, mission 1 is complete and no longer in
                            \ progress, mission 2 hasn't started, we have reached a
                            \ combat rank of 3/8 of the way from Dangerous to
                            \ Deadly, and we are in galaxy 2 (shown in-game as
                            \ galaxy 3), so it's time to start mission 2 by calling
                            \ BRIEF2
    
    .EN3
    
     CMP #%00000110         \ If mission 1 is complete and no longer in progress,
     BNE EN5                \ and mission 2 has started but we have not yet been
                            \ briefed and picked up the plans, then bits 0-3 of TP
                            \ will be %0110, so this jumps to EN5 if this is not the
                            \ case
    
     LDA QQ0                \ Set A = the current system's galactic x-coordinate
    
     CMP #215               \ If A <> 215 then jump to EN4
     BNE EN4
    
     LDA QQ1                \ Set A = the current system's galactic y-coordinate
    
     CMP #84                \ If A <> 84 then jump to EN4
     BNE EN4
    
     JMP BRIEF3             \ If we get here, mission 1 is complete and no longer in
                            \ progress, mission 2 has started but we have not yet
                            \ picked up the plans, and we have just arrived at
                            \ Ceerdi at galactic coordinates (215, 84), so we jump
                            \ to BRIEF3 to get a mission brief and pick up the plans
                            \ that we need to carry to Birera
    
    .EN5
    
     CMP #%00001010         \ If mission 1 is complete and no longer in progress,
     BNE EN4                \ and mission 2 has started and we have picked up the
                            \ plans, then bits 0-3 of TP will be %1010, so this
                            \ jumps to EN5 if this is not the case
    
     LDA QQ0                \ Set A = the current system's galactic x-coordinate
    
     CMP #63                \ If A <> 63 then jump to EN4
     BNE EN4
    
     LDA QQ1                \ Set A = the current system's galactic y-coordinate
    
     CMP #72                \ If A <> 72 then jump to EN4
     BNE EN4
    
     JMP DEBRIEF2           \ If we get here, mission 1 is complete and no longer in
                            \ progress, mission 2 has started and we have picked up
                            \ the plans, and we have just arrived at Birera at
                            \ galactic coordinates (63, 72), so we jump to DEBRIEF2
                            \ to end the mission and get our reward
    
    .EN4
    
    \LDA CASH+2             \ These instructions are commented out in the original
    \CMP #&C4               \ source
    \BCC EN6
    \
    \LDA TP
    \AND #&10
    \BNE EN6
    \
    \JMP TBRIEF
    \
    \.EN6
    
     JMP BAY                \ If we get here them we didn't start or any missions,
                            \ so jump to BAY to go to the docking bay (i.e. show the
                            \ Status Mode screen)
    

[X]

Variable [ASH](../workspace/zp.md#ash) in workspace [ZP](../workspace/zp.md)

Aft shield status

[X]

Subroutine [BAY](bay.md) (category: Status)

Go to the docking bay (i.e. show the Status Mode screen)

[X]

Subroutine [BRIEF](brief.md) (category: Missions)

Start mission 1 and show the mission briefing

[X]

Subroutine [BRIEF2](brief2.md) (category: Missions)

Start mission 2

[X]

Subroutine [BRIEF3](brief3.md) (category: Missions)

Receive the briefing and plans for mission 2

[X]

Subroutine [DEBRIEF](debrief.md) (category: Missions)

Finish mission 1

[X]

Subroutine [DEBRIEF2](debrief2.md) (category: Missions)

Finish mission 2

[X]

Subroutine [DELAY](delay.md) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Label [EN1](doentry.md#en1) is local to this routine

[X]

Label [EN2](doentry.md#en2) is local to this routine

[X]

Label [EN3](doentry.md#en3) is local to this routine

[X]

Label [EN4](doentry.md#en4) is local to this routine

[X]

Label [EN5](doentry.md#en5) is local to this routine

[X]

Variable [ENERGY](../workspace/zp.md#energy) in workspace [ZP](../workspace/zp.md)

Energy bank status

[X]

Variable [FSH](../workspace/zp.md#fsh) in workspace [ZP](../workspace/zp.md)

Forward shield status

[X]

Variable [GCNT](../workspace/wp.md#gcnt) in workspace [WP](../workspace/wp.md)

The number of the current galaxy (0-7)

[X]

Variable [GNTMP](../workspace/wp.md#gntmp) in workspace [WP](../workspace/wp.md)

Laser temperature (or "gun temperature")

[X]

Subroutine [HALL](hall.md) (category: Ship hangar)

Draw the ships in the ship hangar, then draw the hangar

[X]

Subroutine [LAUN](laun.md) (category: Drawing circles)

Make the launch sound and draw the launch tunnel

[X]

Variable [QQ0](../workspace/wp.md#qq0) in workspace [WP](../workspace/wp.md)

The current system's galactic x-coordinate (0-256)

[X]

Variable [QQ1](../workspace/wp.md#qq1) in workspace [WP](../workspace/wp.md)

The current system's galactic y-coordinate (0-256)

[X]

Variable [QQ22](../workspace/zp.md#qq22) in workspace [ZP](../workspace/zp.md)

The two hyperspace countdown counters

[X]

Subroutine [RES2](res2.md) (category: Start and end)

Reset a number of flight variables and workspaces

[X]

Variable [TALLY](../workspace/wp.md#tally) in workspace [WP](../workspace/wp.md)

Our combat rank

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[G%](../variable/g_per_cent.md "Previous routine")[TRIBDIR](../variable/tribdir.md "Next routine")
