---
title: "The Main game loop (Part 4 of 6) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/main_game_loop_part_4_of_6.html"
---

[Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md "Previous routine")[Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md "Next routine")
    
    
           Name: Main game loop (Part 4 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Potentially spawn a lone bounty hunter, a Thargoid, or up to four
                 pirates
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
                 [The elusive Cougar](https://elite.bbcelite.com/deep_dives/the_elusive_cougar.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-main-game-loop-part-4-of-6)
     Variations: See [code variations](../../related/compare/main/subroutine/main_game_loop_part_4_of_6.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section covers the following:
    
       * Potentially spawn (35% chance) either a lone bounty hunter (a Cobra Mk
         III, Asp Mk II, Python or Fer-de-lance), a Thargoid, or a group of up to 4
         pirates (a mix of Sidewinders, Mambas, Kraits, Adders, Geckos, Cobras Mk I
         and III, and Worms)
    
       * Also potentially spawn a Constrictor if this is the mission 1 endgame, or
         Thargoids if mission 2 is in progress
    
    
    
    
     DEC EV                 \ Decrement EV, the extra vessels spawning delay, and if
     BPL MLOOPS             \ it is still positive, jump to MLOOPS to stop spawning,
                            \ so we only do the following when the EV counter runs
                            \ down
    
     INC EV                 \ EV is negative, so bump it up again, setting it back
                            \ to 0
    
     LDA TP                 \ Fetch bits 2 and 3 of TP, which contain the status of
     AND #%00001100         \ mission 2
    
     CMP #%00001000         \ If bit 3 is set and bit 2 is clear, keep going to
     BNE nopl               \ spawn a Thargoid as we are transporting the plans in
                            \ mission 2 and the Thargoids are trying to stop us,
                            \ otherwise jump to nopl to skip spawning a Thargoid
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #220               \ If the random number in A < 220 (86% chance), jump to
     BCC nopl               \ nopl to skip spawning a Thargoid
    
    .fothg2
    
     JSR GTHG               \ Call GTHG to spawn a Thargoid ship and a Thargon
                            \ companion
    
    .nopl
    
     JSR DORND              \ Set A and X to random numbers
    
     LDY gov                \ If the government of this system is 0 (anarchy), jump
     BEQ LABEL_2            \ straight to LABEL_2 to start spawning pirates or a
                            \ lone bounty hunter
    
     CMP #90                \ If the random number in A >= 90 (65% chance), jump to
     BCS MLOOPS             \ MLOOPS to stop spawning (so there's a 35% chance of
                            \ spawning pirates or a lone bounty hunter)
    
     AND #7                 \ Reduce the random number in A to the range 0-7, and
     CMP gov                \ if A is less than government of this system, jump
     BCC MLOOPS             \ to MLOOPS to stop spawning (so safer governments with
                            \ larger gov numbers have a greater chance of jumping
                            \ out, which is another way of saying that more
                            \ dangerous systems spawn pirates and bounty hunters
                            \ more often)
    
    .LABEL_2
    
                            \ Now to spawn a lone bounty hunter, a Thargoid or a
                            \ group of pirates
    
     JSR Ze                 \ Call Ze to initialise INWK to a fairly aggressive
                            \ ship, and set A and X to random values
                            \
                            \ Note that because Ze uses the value of X returned by
                            \ DORND, and X contains the value of A returned by the
                            \ previous call to DORND, this does not set the new ship
                            \ to a totally random location
    
     CMP #100               \ If the random number in A >= 100 (61% chance), jump
     BCS mt1                \ to mt1 to spawn pirates, otherwise keep going to
                            \ spawn a lone bounty hunter or a Thargoid
    
     INC EV                 \ Increase the extra vessels spawning counter, to
                            \ prevent the next attempt to spawn extra vessels
    
     AND #3                 \ Set A = random number in the range 0-3, which we
                            \ will now use to determine the type of ship
    
     ADC #CYL2              \ Add A to #CYL2 (we know the C flag is clear as we
                            \ passed through the BCS above), so A is now one of the
                            \ lone bounty hunter ships, i.e. Cobra Mk III (pirate),
                            \ Asp Mk II, Python (pirate) or Fer-de-lance
                            \
                            \ Interestingly, this logic means that the Moray, which
                            \ is the ship after the Fer-de-lance in the XX21 table,
                            \ never spawns, as the above logic chooses a blueprint
                            \ number in the range CYL2 to CYL2+3 (i.e. 24 to 27),
                            \ and the Moray is blueprint 28
                            \
                            \ No other code spawns the ship with blueprint 28, so
                            \ this means the Moray is never seen in Elite
                            \
                            \ This is presumably a bug, which could be very easily
                            \ fixed by inserting one of the following instructions
                            \ before the ADC #CYL2 instruction above:
                            \
                            \   * SEC would change the range to 25 to 28, which
                            \     would cover the Asp Mk II, Python (pirate),
                            \     Fer-de-lance and Moray
                            \
                            \   * LSR A would set the C flag to a random number to
                            \     give a range of 24 to 28, which would cover the
                            \     Cobra Mk III (pirate), Asp Mk II, Python (pirate),
                            \     Fer-de-lance and Moray
                            \
                            \ It's hard to know what the authors' original intent
                            \ was, but the second approach makes the Moray and Cobra
                            \ Mk III the rarest choices, with the Asp Mk II, Python
                            \ and Fer-de-Lance being more likely, and as the Moray
                            \ is described in the literature as a rare ship, and the
                            \ Cobra can already be spawned as part of a group of
                            \ pirates (see mt1 below), I tend to favour the LSR A
                            \ solution over the SEC approach
    
     TAY                    \ Copy the new ship type to Y
    
     JSR THERE              \ Call THERE to see if we are in the Constrictor's
                            \ system in mission 1
    
     BCC NOCON              \ If the C flag is clear then we are not in the
                            \ Constrictor's system, so skip to NOCON
    
     LDA #%11111001         \ Set the AI flag of this ship so that it has E.C.M.,
     STA INWK+32            \ has a very high aggression level of 60 out of 63, is
                            \ hostile, and has AI enabled - nasty stuff!
    
     LDA TP                 \ Fetch bits 0 and 1 of TP, which contain the status of
     AND #%00000011         \ mission 1
    
     LSR A                  \ Shift bit 0 into the C flag
    
     BCC NOCON              \ If bit 0 is clear, skip to NOCON as mission 1 is not
                            \ in progress
    
     ORA MANY+CON           \ Bit 0 of A now contains bit 1 of TP, so this will be
                            \ set if we have already completed mission 1, so this OR
                            \ will be non-zero if we have either completed mission
                            \ 1, or there is already a Constrictor in our local
                            \ bubble of universe (in which case MANY+CON will be
                            \ non-zero)
    
     BEQ YESCON             \ If A = 0 then mission 1 is in progress, we haven't
                            \ completed it yet, and there is no Constrictor in the
                            \ vicinity, so jump to YESCON to spawn the Constrictor
    
    .NOCON
    
     LDA #%00000100         \ Set bit 2 of the NEWB flags and clear all other bits,
     STA NEWB               \ so the ship we are about to spawn is hostile
    
                            \ We now build the AI flag for this ship in A
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #200               \ First, set the C flag if X >= 200 (22% chance)
    
     ROL A                  \ Set bit 0 of A to the C flag (i.e. there's a 22%
                            \ chance of this ship having E.C.M.)
    
     ORA #%11000000         \ Set bits 6 and 7 of A, so the ship has AI (bit 7) and
                            \ an aggression level of at least 32 out of 63
    
     STA INWK+32            \ Store A in the AI flag of this ship
    
     TYA                    \ Set A to the new ship type in Y
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &1F, or BIT &1FA9, which does nothing apart
                            \ from affect the flags
    
    .YESCON
    
     LDA #CON               \ If we jump straight here, we are in the mission 1
                            \ endgame and it's time to spawn the Constrictor, so
                            \ set A to the Constrictor's type
    
    .focoug
    
     JSR NWSHP              \ Spawn the new ship, whether it's a pirate, Thargoid,
                            \ Cougar or Constrictor
    
    .mj1
    
     JMP MLOOP              \ Jump down to MLOOP, as we are done spawning ships
    
    .fothg
    
     LDA K%+6               \ Fetch the z_lo coordinate of the first ship in the K%
     AND #%00111110         \ block (i.e. the planet) and extract bits 1-5
    
     BNE fothg2             \ If any of bits 1-5 are set (96.8% chance), jump up to
                            \ fothg2 to spawn a Thargoid
    
                            \ If we get here then we're going to spawn a Cougar, a
                            \ very rare event indeed. How rare? Well, all the
                            \ following have to happen in sequence:
                            \
                            \  * Main loop iteration = 0 (1 in 256 iterations)
                            \  * Skip asteroid spawning (87% chance)
                            \  * Skip cop spawning (0.4% chance)
                            \  * Skip Thargoid spawning (3.2% chance)
                            \
                            \ so the chances of spawning a Cougar on any single main
                            \ loop iteration are slim, to say the least
    
     LDA #18                \ Give the ship we're about to spawn a speed of 27
     STA INWK+27
    
     LDA #%01111001         \ Give it an E.C.M. and an aggression level of 60 out of
     STA INWK+32            \ 63, but don't enable its AI, so the ship will sit
                            \ still in space unless it is hit, at which point it
                            \ will defend itself vigorously
                            \
                            \ This ensures the Cougar behaves like a ship with a
                            \ cloaking device that hides it from the scanner, so it
                            \ minds its own business until it's discovered
    
     LDA #COU               \ Set the ship type to a Cougar and jump up to focoug
     BNE focoug             \ to spawn it
    
    .mt1
    
     AND #3                 \ It's time to spawn a group of pirates, so set A to a
                            \ random number in the range 0-3, which will be the
                            \ loop counter for spawning pirates below (so we will
                            \ spawn 1-4 pirates)
    
     STA EV                 \ Delay further spawnings by this number
    
     STA XX13               \ Store the number in XX13, the pirate counter
    
    .mt3
    
     JSR DORND              \ Set A and X to random numbers
    
     STA T                  \ Set T to a random number
    
     JSR DORND              \ Set A and X to random numbers
    
     AND T                  \ Set A to the AND of two random numbers, so each bit
                            \ has 25% chance of being set which makes the chances
                            \ of a smaller number higher
    
     AND #7                 \ Reduce A to a random number in the range 0-7, though
                            \ with a bigger chance of a smaller number in this range
    
     ADC #PACK              \ #PACK is set to #SH3, the ship type for a Sidewinder,
                            \ so this sets our new ship type to one of the pack
                            \ hunters, namely a Sidewinder, Mamba, Krait, Adder,
                            \ Gecko, Cobra Mk I, Worm or Cobra Mk III (pirate)
    
     JSR NWSHP              \ Try adding a new ship of type A to the local bubble
    
     DEC XX13               \ Decrement the pirate counter
    
     BPL mt3                \ If we need more pirates, loop back up to mt3,
                            \ otherwise we are done spawning, so fall through into
                            \ the end of the main loop at MLOOP
    

[X]

Configuration variable [CON](../../all/workspaces.md#con) = 31

Ship type for a Constrictor

[X]

Configuration variable [COU](../../all/workspaces.md#cou) = 32

Ship type for a Cougar

[X]

Configuration variable [CYL2](../../all/workspaces.md#cyl2) = 24

Ship type for a Cobra Mk III (pirate)

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [EV](../workspace/wp.md#ev) in workspace [WP](../workspace/wp.md)

The "extra vessels" spawning counter

[X]

Subroutine [GTHG](gthg.md) (category: Universe)

Spawn a Thargoid ship and a Thargon companion

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Label [LABEL_2](main_game_loop_part_4_of_6.md#label_2) is local to this routine

[X]

Variable [MANY](../workspace/wp.md#many) in workspace [WP](../workspace/wp.md)

The number of ships of each type in the local bubble of universe

[X]

Entry point [MLOOP](main_game_loop_part_5_of_6.md#mloop) in subroutine [Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md) (category: Main loop)

The entry point for the main game loop. This entry point comes after the call to the main flight loop and spawning routines, so it marks the start of the main game loop for when we are docked (as we don't need to call the main flight loop or spawning routines if we aren't in space)

[X]

Label [MLOOPS](main_game_loop_part_3_of_6.md#mloops) in subroutine [Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md)

[X]

Variable [NEWB](../workspace/zp.md#newb) in workspace [ZP](../workspace/zp.md)

The ship's "new byte flags" (or NEWB flags)

[X]

Label [NOCON](main_game_loop_part_4_of_6.md#nocon) is local to this routine

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Configuration variable [PACK](../../all/workspaces.md#pack) = SH3

The first of the eight pack-hunter ships, which tend to spawn in groups. With the default value of PACK the pack-hunters are the Sidewinder, Mamba, Krait, Adder, Gecko, Cobra Mk I, Worm and Cobra Mk III (pirate)

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [THERE](there.md) (category: Missions)

Check whether we are in the Constrictor's system in mission 1

[X]

Variable [TP](../workspace/wp.md#tp) in workspace [WP](../workspace/wp.md)

The current mission status

[X]

Variable [XX13](../workspace/zp.md#xx13) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used in the line-drawing routines

[X]

Label [YESCON](main_game_loop_part_4_of_6.md#yescon) is local to this routine

[X]

Subroutine [Ze](ze.md) (category: Universe)

Initialise the INWK workspace to a fairly aggressive ship

[X]

Label [focoug](main_game_loop_part_4_of_6.md#focoug) is local to this routine

[X]

Label [fothg2](main_game_loop_part_4_of_6.md#fothg2) is local to this routine

[X]

Variable [gov](../workspace/wp.md#gov) in workspace [WP](../workspace/wp.md)

The current system's government type (0-7)

[X]

Label [mt1](main_game_loop_part_4_of_6.md#mt1) is local to this routine

[X]

Label [mt3](main_game_loop_part_4_of_6.md#mt3) is local to this routine

[X]

Label [nopl](main_game_loop_part_4_of_6.md#nopl) is local to this routine

[Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md "Previous routine")[Main game loop (Part 5 of 6)](main_game_loop_part_5_of_6.md "Next routine")
