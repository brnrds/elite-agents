---
title: "Elite F source in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_f.html"
---

[Elite E source](elite_e.md "Previous routine")[Elite G source](elite_g.md "Next routine")
    
    
     ELITE F FILE
    
    
    
    
     CODE_F% = P%
    
     LOAD_F% = LOAD% + P% - CODE%
    
    
    
    
           Name: DJOY                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard for cursor key or digital joystick movement
    
    
        Context: See this subroutine [on its own page](../main/subroutine/djoy.md)
     References: This subroutine is called as follows:
                 * [TT17](../main/subroutine/tt17.md) calls DJOY
    
    
    
    
    
    
    IF _COMPACT
    
    .DJOY
    
     JSR TT17X              \ Call TT17X to read the digital joystick and return
                            \ deltas as appropriate
    
     JMP TJ1                \ Jump to TJ1 in the TT17 routine to check for cursor
                            \ key presses and return the combined deltas for the
                            \ digital joystick and cursor keys, returning from the
                            \ subroutine using a tail call
    
    ENDIF
    
    
    
    
           Name: KS3                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set the SLSP ship line heap pointer after shuffling ship slots
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ks3.md)
     References: This subroutine is called as follows:
                 * [KS2](../main/subroutine/ks2.md) calls KS3
    
    
    
    
    
    * * *
    
    
     The final part of the KILLSHP routine, called after we have shuffled the ship
     slots and sorted out our missiles. This simply sets SLSP to the new bottom of
     the ship line heap.
    
    
    
    * * *
    
    
     Arguments:
    
       P(1 0)               Points to the ship line heap of the ship in the last
                            occupied slot (i.e. it points to the bottom of the
                            descending heap)
    
    
    
    
    .KS3
    
     LDA P                  \ After shuffling the ship slots, P(1 0) will point to
     STA SLSP               \ the new bottom of the ship line heap, so store this in
     LDA P+1                \ SLSP(1 0), which stores the bottom of the heap
     STA SLSP+1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: KS1                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Remove the current ship from our local bubble of universe
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ks1.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md) calls KS1
    
    
    
    
    
    * * *
    
    
     Part 12 of the main flight loop calls this routine to remove the ship that is
     currently being analysed by the flight loop. Once the ship is removed, it
     jumps back to MAL1 to rejoin the main flight loop, with X pointing to the
     same slot that we just cleared (and which now contains the next ship in the
     local bubble of universe).
    
    
    
    * * *
    
    
     Arguments:
    
       XX0                  The address of the blueprint for this ship
    
       INF                  The address of the data block for this ship
    
    
    
    
    .KS1
    
     LDX XSAV               \ Fetch the current ship's slot number from XSAV
    
     JSR KILLSHP            \ Call KILLSHP to remove the ship in slot X from our
                            \ local bubble of universe
    
     LDX XSAV               \ Restore the current ship's slot number from XSAV,
                            \ which now points to the next ship in the bubble
    
     JMP MAL1               \ Jump to MAL1 to rejoin the main flight loop at the
                            \ start of the ship analysis loop
    
    
    
    
           Name: KS4                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Remove the space station and replace it with the sun
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ks4.md)
     Variations: See [code variations](../related/compare/main/subroutine/ks4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KILLSHP](../main/subroutine/killshp.md) calls KS4
    
    
    
    
    
    
    .KS4
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     JSR FLFLLS             \ Reset the LSO block, returns with A = 0
    
     STA FRIN+1             \ Set the second slot in the FRIN table to 0, which
                            \ sets this slot to empty, so when we call NWSHP below
                            \ the new sun that gets created will go into FRIN+1
    
     STA SSPR               \ Set the "space station present" flag to 0, as we are
                            \ no longer in the space station's safe zone
    
     JSR SPBLB              \ Call SPBLB to redraw the space station bulb, which
                            \ will erase it from the dashboard
    
     LDA #6                 \ Set the sun's y_sign to 6
     STA INWK+5
    
     LDA #129               \ Set A = 129, the ship type for the sun
    
     JMP NWSHP              \ Call NWSHP to set up the sun's data block and add it
                            \ to FRIN, where it will get put in the second slot as
                            \ we just cleared out the second slot, and the first
                            \ slot is already taken by the planet
    
    
    
    
           Name: KS2                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Check the local bubble for missiles with target lock
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ks2.md)
     References: This subroutine is called as follows:
                 * [KILLSHP](../main/subroutine/killshp.md) calls KS2
    
    
    
    
    
    * * *
    
    
     Check the local bubble of universe to see if there are any missiles with
     target lock in the vicinity. If there are, then check their targets; if we
     just removed their target in the KILLSHP routine, then switch off their AI so
     they just drift in space, otherwise update their targets to reflect the newly
     shuffled slot numbers.
    
     This is called from KILLSHP once the slots have been shuffled down, following
     the removal of a ship.
    
    
    
    * * *
    
    
     Arguments:
    
       XX4                  The slot number of the ship we removed just before
                            calling this routine
    
    
    
    
    .KS2
    
     LDX #&FF               \ We want to go through the ships in our local bubble
                            \ and pick out all the missiles, so set X to &FF to
                            \ use as a counter
    
    .KSL4
    
     INX                    \ Increment the counter (so it starts at 0 on the first
                            \ iteration)
    
     LDA FRIN,X             \ If slot X is empty then we have worked our way through
     BEQ KS3                \ all the slots, so jump to KS3 to stop looking
    
     CMP #MSL               \ If the slot does not contain a missile, loop back to
     BNE KSL4               \ KSL4 to check the next slot
    
                            \ We have found a slot containing a missile, so now we
                            \ want to check whether it has target lock
    
     TXA                    \ Set Y = X * 2 and fetch the Y-th address from UNIV
     ASL A                  \ and store it in SC and SC+1 - in other words, set
     TAY                    \ SC(1 0) to point to the missile's ship data block
     LDA UNIV,Y
     STA SC
     LDA UNIV+1,Y
     STA SC+1
    
     LDY #32                \ Fetch byte #32 from the missile's ship data (AI)
     LDA (SC),Y
    
     BPL KSL4               \ If bit 7 of byte #32 is clear, then the missile is
                            \ dumb and has no AI, so loop back to KSL4 to move on
                            \ to the next slot
    
     AND #%01111111         \ Otherwise this missile has AI, so clear bit 7 and
     LSR A                  \ shift right to set the C flag to the missile's "is
                            \ locked" flag, and A to the target's slot number
    
     CMP XX4                \ If this missile's target is less than XX4, then the
     BCC KSL4               \ target's slot isn't being shuffled down, so jump to
                            \ KSL4 to move on to the next slot
    
     BEQ KS6                \ If this missile was locked onto the ship that we just
                            \ removed in KILLSHP, jump to KS6 to stop the missile
                            \ from continuing to hunt it down
    
     SBC #1                 \ Otherwise this missile is locked and has AI enabled,
                            \ and its target will have moved down a slot, so
                            \ subtract 1 from the target number (we know C is set
                            \ from the BCC above)
    
     ASL A                  \ Shift the target number left by 1, so it's in bits
                            \ 1-6 once again, and also set bit 0 to 1, as the C
                            \ flag is still set, so this makes sure the missile is
                            \ still set to being locked
    
     ORA #%10000000         \ Set bit 7, so the missile's AI is enabled
    
     STA (SC),Y             \ Update the missile's AI flag to the value in A
    
     BNE KSL4               \ Loop back to KSL4 to move on to the next slot (this
                            \ BNE is effectively a JMP as A will never be zero)
    
    .KS6
    
     LDA #0                 \ The missile's target lock just got removed, so set the
     STA (SC),Y             \ AI flag to 0 to make it dumb and not locked
    
     BEQ KSL4               \ Loop back to KSL4 to move on to the next slot (this
                            \ BEQ is effectively a JMP as A is always zero)
    
    
    
    
           Name: KILLSHP                                                 [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Remove a ship from our local bubble of universe
    
    
        Context: See this subroutine [on its own page](../main/subroutine/killshp.md)
     Variations: See [code variations](../related/compare/main/subroutine/killshp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KS1](../main/subroutine/ks1.md) calls KILLSHP
    
    
    
    
    
    * * *
    
    
     Remove the ship in slot X from our local bubble of universe. This happens
     when we kill a ship, collide with a ship and destroy it, or when a ship moves
     outside our local bubble.
    
     We also use this routine when we move out of range of the space station, in
     which case we replace it with the sun.
    
     When removing a ship, this creates a gap in the ship slots at FRIN, so we
     shuffle all the later slots down to close the gap. We also shuffle the ship
     data blocks at K% and ship line heap at WP, to reclaim all the memory that
     the removed ship used to occupy.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The slot number of the ship to remove
    
       XX0                  The address of the blueprint for the ship to remove
    
       INF                  The address of the data block for the ship to remove
    
    
    
    
    .KILLSHP
    
     STX XX4                \ Store the slot number of the ship to remove in XX4
    
     LDA MSTG               \ Check whether this slot matches the slot number in
     CMP XX4                \ MSTG, which is the target of our missile lock
    
     BNE KS5                \ If our missile is not locked on this ship, jump to KS5
    
     LDY #GREEN2            \ Otherwise we need to remove our missile lock, so call
     JSR ABORT              \ ABORT to disarm the missile and update the missile
                            \ indicators on the dashboard to green (Y = #GREEN2)
    
     LDA #200               \ Print recursive token 40 ("TARGET LOST") as an
     JSR MESS               \ in-flight message
    
    .KS5
    
     LDY XX4                \ Restore the slot number of the ship to remove into Y
    
     LDX FRIN,Y             \ Fetch the contents of the slot, which contains the
                            \ ship type
    
     CPX #SST               \ If this is the space station, then jump to KS4 to
     BEQ KS4                \ replace the space station with the sun
    
     CPX #CON               \ Did we just kill the Constrictor from mission 1? If
     BNE lll                \ not, jump to lll
    
     LDA TP                 \ We just killed the Constrictor from mission 1, so set
     ORA #%00000010         \ bit 1 of TP to indicate that we have successfully
     STA TP                 \ completed mission 1
    
     INC TALLY+1            \ Award 256 kill points for killing the Constrictor
    
    .lll
    
     CPX #HER               \ Did we just kill a rock hermit? If we did, jump to
     BEQ blacksuspenders    \ blacksuspenders to decrease the junk count
    
     CPX #JL                \ If JL <= X < JH, i.e. the type of ship we killed in X
     BCC KS7                \ is junk (escape pod, alloy plate, cargo canister,
     CPX #JH                \ asteroid, splinter, Shuttle or Transporter), then keep
     BCS KS7                \ going, otherwise jump to KS7
    
    .blacksuspenders
    
     DEC JUNK               \ We just killed junk, so decrease the junk counter
    
    .KS7
    
     DEC MANY,X             \ Decrease the number of this type of ship in our little
                            \ bubble, which is stored in MANY+X (where X is the ship
                            \ type)
    
     LDX XX4                \ Restore the slot number of the ship to remove into X
    
                            \ We now want to remove this ship and reclaim all the
                            \ memory that it uses. Removing the ship will leave a
                            \ gap in three places, which we need to close up:
                            \
                            \   * The ship slots in FRIN
                            \
                            \   * The ship data blocks in K%
                            \
                            \   * The descending ship line heap at WP down
                            \
                            \ The rest of this routine closes up these gaps by
                            \ looping through all the occupied ship slots after the
                            \ slot we are removing, one by one, and shuffling each
                            \ ship's slot, data block and line heap down to close
                            \ up the gaps left by the removed ship. As part of this,
                            \ we have to make sure we update any address pointers
                            \ so they point to the newly shuffled data blocks and
                            \ line heaps
                            \
                            \ In the following, when shuffling a ship's data down
                            \ into the preceding empty slot, we call the ship that
                            \ we are shuffling down the "source", and we call the
                            \ empty slot we are shuffling it into the "destination"
                            \
                            \ Before we start looping through the ships we need to
                            \ shuffle down, we need to set up some variables to
                            \ point to the source and destination line heaps
    
     LDY #5                 \ Fetch byte #5 of the removed ship's blueprint into A,
     LDA (XX0),Y            \ which gives the ship's maximum heap size for the ship
                            \ we are removing (i.e. the size of the gap in the heap
                            \ created by the ship removal)
    
                            \ INF currently contains the ship data for the ship we
                            \ are removing, and INF(34 33) contains the address of
                            \ the bottom of the ship's heap, so we can calculate
                            \ the address of the top of the heap by adding the heap
                            \ size to this address
    
     LDY #33                \ First we add A and the address in INF+33, to get the
     CLC                    \ low byte of the top of the heap, which we store in P
     ADC (INF),Y
     STA P
    
     INY                    \ And next we add A and the address in INF+34, with any
     LDA (INF),Y            \ carry from the previous addition, to get the high byte
     ADC #0                 \ of the top of the heap, which we store in P+1, so
     STA P+1                \ P(1 0) points to the top of this ship's heap
    
                            \ Now, we're ready to start looping through the ships
                            \ we want to move, moving the slots, data blocks and
                            \ line heap from the source to the destination. In the
                            \ following, we set up SC to point to the source data,
                            \ and INF (which currently points to the removed ship's
                            \ data that we can now overwrite) points to the
                            \ destination
                            \
                            \ So P(1 0) now points to the top of the line heap for
                            \ the destination
    
    .KSL1
    
     INX                    \ On entry, X points to the empty slot we want to
                            \ shuffle the next ship into (the destination), so
                            \ this increment points X to the next slot - i.e. the
                            \ source slot we want to shuffle down
    
     LDA FRIN,X             \ Copy the contents of the source slot into the
     STA FRIN-1,X           \ destination slot
    
     BNE P%+5               \ If the slot we just shuffled down is not empty, then
                            \ skip the following instruction
    
     JMP KS2                \ The source slot is empty and we are done shuffling,
                            \ so jump to KS2 to move on to processing missiles
    
     ASL A                  \ Otherwise we have a source ship to shuffle down into
     TAY                    \ the destination, so set Y = A * 2 so it can act as an
                            \ index into the two-byte ship blueprint lookup table
                            \ at XX21 for the source ship
    
     LDA XX21-2,Y           \ Set SC(0 1) to point to the blueprint data for the
     STA SC                 \ source ship
     LDA XX21-1,Y
     STA SC+1
    
     LDY #5                 \ Fetch blueprint byte #5 for the source ship, which
     LDA (SC),Y             \ gives us its maximum heap size, and store it in T
     STA T
    
                            \ We now subtract T from P(1 0), so P(1 0) will point to
                            \ the bottom of the line heap for the destination
                            \ (which we will use later when closing up the gap in
                            \ the heap space)
    
     LDA P                  \ First, we subtract the low bytes
     SEC
     SBC T
     STA P
    
     LDA P+1                \ And then we do the high bytes, for which we subtract
     SBC #0                 \ 0 to include any carry, so this is effectively doing
     STA P+1                \ P(1 0) = P(1 0) - (0 T)
    
                            \ Next, we want to set SC(1 0) to point to the source
                            \ ship's data block
    
     TXA                    \ Set Y = X * 2 so it can act as an index into the
     ASL A                  \ two-byte lookup table at UNIV, which contains the
     TAY                    \ addresses of the ship data blocks. In this case we are
                            \ multiplying X by 2, and X contains the source ship's
                            \ slot number so Y is now an index for the source ship's
                            \ entry in UNIV
    
     LDA UNIV,Y             \ Set SC(1 0) to the address of the data block for the
     STA SC                 \ source ship
     LDA UNIV+1,Y
     STA SC+1
    
                            \ We have now set up our variables as follows:
                            \
                            \   SC(1 0) points to the source's ship data block
                            \
                            \   INF(1 0) points to the destination's ship data block
                            \
                            \   P(1 0) points to the destination's line heap
                            \
                            \ so let's start copying data from the source to the
                            \ destination
    
     LDY #36                \ We are going to be using Y as a counter for the 37
                            \ bytes of ship data we want to copy from the source
                            \ to the destination, so we set it to 36 to start things
                            \ off, and will decrement Y for each byte we copy
    
     LDA (SC),Y             \ Fetch byte #36 of the source's ship data block at SC,
     STA (INF),Y            \ and store it in byte #36 of the destination's block
     DEY                    \ at INF, so that's the ship's NEWB flags copied from
                            \ the source to the destination. One down, quite a few
                            \ to go...
    
     LDA (SC),Y             \ Fetch byte #35 of the source's ship data block at SC,
     STA (INF),Y            \ and store it in byte #35 of the destination's block
                            \ at INF, so that's the ship's energy copied from the
                            \ source to the destination
    
     DEY                    \ Fetch byte #34 of the source ship, which is the
     LDA (SC),Y             \ high byte of the source ship's line heap, and store
     STA K+1                \ in K+1
    
     LDA P+1                \ Set the low byte of the destination's heap pointer
     STA (INF),Y            \ to P+1
    
     DEY                    \ Fetch byte #33 of the source ship, which is the
     LDA (SC),Y             \ low byte of the source ship's heap, and store in K
     STA K                  \ so now we have the following:
                            \
                            \   K(1 0) points to the source's line heap
    
     LDA P                  \ Set the low byte of the destination's heap pointer
     STA (INF),Y            \ to P, so now the destination's heap pointer is to
                            \ P(1 0), so that's the heap pointer in bytes #33 and
                            \ #34 done
    
     DEY                    \ Luckily, we can just copy the rest of the source's
                            \ ship data block into the destination, as there are no
                            \ more address pointers, so first we decrement our
                            \ counter in Y to point to the next byte (the AI flag)
                            \ in byte #32) and then start looping
    
    .KSL2
    
     LDA (SC),Y             \ Copy the Y-th byte of the source to the Y-th byte of
     STA (INF),Y            \ the destination
    
     DEY                    \ Decrement the counter
    
     BPL KSL2               \ Loop back to KSL2 to copy the next byte until we have
                            \ copied the whole block
    
                            \ We have now shuffled the ship's slot and the ship's
                            \ data block, so we only have the heap data itself to do
    
     LDA SC                 \ First, we copy SC into INF, so when we loop round
     STA INF                \ again, INF will correctly point to the destination for
     LDA SC+1               \ the next iteration
     STA INF+1
    
     LDY T                  \ Now we want to move the contents of the heap, as all
                            \ we did above was to update the pointers, so first
                            \ we set a counter in Y that is initially set to T
                            \ (which we set above to the maximum heap size for the
                            \ source ship)
                            \
                            \ As a reminder, we have already set the following:
                            \
                            \   K(1 0) points to the source's line heap
                            \
                            \   P(1 0) points to the destination's line heap
                            \
                            \ so we can move the heap data by simply copying the
                            \ correct number of bytes from K(1 0) to P(1 0)
    .KSL3
    
     DEY                    \ Decrement the counter
    
     LDA (K),Y              \ Copy the Y-th byte of the source heap at K(1 0) to
     STA (P),Y              \ the destination heap at P(1 0)
    
     TYA                    \ Loop back to KSL3 to copy the next byte, until we
     BNE KSL3               \ have done them all
    
     BEQ KSL1               \ We have now shuffled everything down one slot, so
                            \ jump back up to KSL1 to see if there is another slot
                            \ that needs shuffling down (this BEQ is effectively a
                            \ JMP as A will always be zero)
    
    
    
    
           Name: THERE                                                   [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Check whether we are in the Constrictor's system in mission 1
    
    
        Context: See this subroutine [on its own page](../main/subroutine/there.md)
     References: This subroutine is called as follows:
                 * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md) calls THERE
    
    
    
    
    
    * * *
    
    
     The stolen Constrictor is the target of mission 1. We finally track it down to
     the Orarra system in the second galaxy, which is at galactic coordinates
     (144, 33). This routine checks whether we are in this system and sets the C
     flag accordingly.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if we are in the Constrictor system, otherwise clear
    
    
    
    
    .THERE
    
     LDX GCNT               \ Set X = GCNT - 1
     DEX
    
     BNE THEX               \ If X is non-zero (i.e. GCNT is not 1, so we are not in
                            \ the second galaxy), then jump to THEX
    
     LDA QQ0                \ Set A = the current system's galactic x-coordinate
    
     CMP #144               \ If A <> 144 then jump to THEX
     BNE THEX
    
     LDA QQ1                \ Set A = the current system's galactic y-coordinate
    
     CMP #33                \ If A = 33 then set the C flag
    
     BEQ THEX+1             \ If A = 33 then jump to THEX+1, so we return from the
                            \ subroutine with the C flag set (otherwise we clear the
                            \ C flag with the next instruction)
    
    .THEX
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: NUMBOR                                                  [Show more]
           Type: Subroutine
       Category: Text
        Summary: An unused routine that prints a number in hexadecimal
    
    
        Context: See this subroutine [on its own page](../main/subroutine/numbor.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The number to print
    
    
    
    
    .NUMBOR
    
     PHA                    \ Store A on the stack so we can grab the low nibble
                            \ from it later
    
     LSR A                  \ Shift A right so that it contains the high nibble
     LSR A                  \ of the original argument
     LSR A
     LSR A
    
     JSR DIDGIT             \ Call DIDGIT below to print 0-F for the high nibble
    
     PLA                    \ Restore A from the stack
    
     AND #%00001111         \ Extract the low nibble and fall through into DIDGIT
                            \ to print 0-F for the low nibble
    
    .DIDGIT
    
     CMP #10                \ If A >= 10, skip the next three instructions
     BCS P%+7
    
     ADC #'0'               \ A < 10, so print the number in A as a digit 0-9 and
     JMP CHPR               \ return from the subroutine using a tail call
    
    .DIDGIT2
    
     ADC #'6'               \ A >= 10, so print the number in A as a digit A-F and
     JMP CHPR               \ return from the subroutine using a tail call
    
    
    
    
           Name: RESET                                                   [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Reset most variables
    
    
        Context: See this subroutine [on its own page](../main/subroutine/reset.md)
     Variations: See [code variations](../related/compare/main/subroutine/reset.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TITLE](../main/subroutine/title.md) calls RESET
                 * [TT170](../main/subroutine/tt170.md) calls RESET
    
    
    
    
    
    * * *
    
    
     Reset our ship and various controls, recharge shields and energy, and then
     fall through into RES2 to reset the stardust and the ship workspace at INWK.
    
     In this subroutine, this means zero-filling the following locations:
    
       * Pages &9, &A, &B, &C and &D
    
       * BETA to BETA+6, which covers the following:
    
         * BETA, BET1 - Set pitch to 0
    
         * XC, YC - Set text cursor to (0, 0)
    
         * QQ22 - Set hyperspace counters to 0
    
         * ECMA - Turn E.C.M. off
    
     It also sets QQ12 to &FF, to indicate we are docked, recharges the shields and
     energy banks, and then falls through into RES2.
    
    
    
    
    .RESET
    
     JSR ZERO               \ Reset the ship slots for the local bubble of universe,
                            \ and various flight and ship status variables
    
     LDX #6                 \ Set up a counter for zeroing BETA through BETA+6
    
    .SAL3
    
     STA BETA,X             \ Zero the X-th byte after BETA
    
     DEX                    \ Decrement the loop counter
    
     BPL SAL3               \ Loop back for the next byte to zero
    
     STX JSTGY              \ X is now negative - i.e. &FF - so this sets JSTGY to
                            \ &FF to set the joystick Y-channel to the default
                            \ direction
    
     TXA                    \ X is now negative - i.e. &FF - so this sets A and QQ12
     STA QQ12               \ to &FF to indicate we are docked
    
     LDX #2                 \ We're now going to recharge both shields and the
                            \ energy bank, which live in the three bytes at FSH,
                            \ ASH (FSH+1) and ENERGY (FSH+2), so set a loop counter
                            \ in X for 3 bytes
    
    .REL5
    
     STA FSH,X              \ Set the X-th byte of FSH to &FF to charge up that
                            \ shield/bank
    
     DEX                    \ Decrement the loop counter
    
     BPL REL5               \ Loop back to REL5 until we have recharged both shields
                            \ and the energy bank
    
                            \ Fall through into RES2 to reset the stardust and ship
                            \ workspace at INWK
    
    
    
    
           Name: RES2                                                    [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Reset a number of flight variables and workspaces
    
    
        Context: See this subroutine [on its own page](../main/subroutine/res2.md)
     Variations: See [code variations](../related/compare/main/subroutine/res2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](../main/subroutine/death.md) calls RES2
                 * [DEATH2](../main/subroutine/death2.md) calls RES2
                 * [DOENTRY](../main/subroutine/doentry.md) calls RES2
                 * [ESCAPE](../main/subroutine/escape.md) calls RES2
                 * [MJP](../main/subroutine/mjp.md) calls RES2
                 * [TT110](../main/subroutine/tt110.md) calls RES2
                 * [TT18](../main/subroutine/tt18.md) calls RES2
    
    
    
    
    
    * * *
    
    
     This is called after we launch from a space station, arrive in a new system
     after hyperspace, launch an escape pod, or die a cold, lonely death in the
     depths of space.
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is set to &FF
    
    
    
    
    .RES2
    
    \JSR stopbd             \ This instruction is commented out in the original
                            \ source
    
     LDA #NOST              \ Reset NOSTM, the number of stardust particles, to the
     STA NOSTM              \ maximum allowed (20)
    
     LDX #&FF               \ Reset LSX2 and LSY2, the ball line heaps used by the
     STX LSX2               \ BLINE routine for drawing circles, to &FF, to set the
     STX LSY2               \ heap to empty
    
     STX MSTG               \ Reset MSTG, the missile target, to &FF (no target)
    
     LDA #128               \ Set the current pitch rate to the mid-point, 128
     STA JSTY
    
     STA ALP2               \ Reset ALP2 (roll sign) and BET2 (pitch sign)
     STA BET2               \ to negative, i.e. pitch and roll negative
    
     ASL A                  \ This sets A to 0
    
     STA BETA               \ Reset BETA (pitch angle alpha) to 0
    
     STA BET1               \ Reset BET1 (magnitude of the pitch angle) to 0
    
     STA ALP2+1             \ Reset ALP2+1 (flipped roll sign) and BET2+1 (flipped
     STA BET2+1             \ pitch sign) to positive, i.e. pitch and roll negative
    
     STA MCNT               \ Reset MCNT (the main loop counter) to 0
    
    \STA TRIBCT             \ This instruction is commented out in the original
                            \ source; it is left over from the Commodore 64 version
                            \ of Elite and would reset the number of Trumbles
    
     LDA #3                 \ Reset DELTA (speed) to 3
     STA DELTA
    
     STA ALPHA              \ Reset ALPHA (roll angle alpha) to 3
    
     STA ALP1               \ Reset ALP1 (magnitude of roll angle alpha) to 3
    
    \LDA #&10               \ These instructions are commented out in the original
    \STA COL2               \ source
    
     LDA #0                 \ Set dontclip to 0 (though this variable is never used,
     STA dontclip           \ so this has no effect; it is left over from the
                            \ Commodore 64 version)
    
     LDA #2*Y-1             \ Set Yx2M1 to the number of pixel lines in the space
     STA Yx2M1              \ view
    
     LDA SSPR               \ Fetch the "space station present" flag, and if we are
     BEQ P%+5               \ not inside the safe zone, skip the next instruction
    
     JSR SPBLB              \ Light up the space station bulb on the dashboard
    
     LDA ECMA               \ Fetch the E.C.M. status flag, and if E.C.M. is off,
     BEQ yu                 \ skip the next instruction
    
     JSR ECMOF              \ Turn off the E.C.M. sound
    
    .yu
    
     JSR WPSHPS             \ Wipe all ships from the scanner
    
     JSR ZERO               \ Reset the ship slots for the local bubble of universe,
                            \ and various flight and ship status variables
    
     LDA #LO(LS%)           \ We have reset the ship line heap, so we now point
     STA SLSP               \ SLSP to LS% (the byte below the ship blueprints at D%)
     LDA #HI(LS%)           \ to indicate that the heap is empty
     STA SLSP+1
    
                            \ Finally, fall through into ZINF to reset the INWK
                            \ ship workspace
    
    
    
    
           Name: ZINF                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Reset the INWK workspace and orientation vectors
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/zinf.md)
     References: This subroutine is called as follows:
                 * [BRIEF](../main/subroutine/brief.md) calls ZINF
                 * [DOKEY](../main/subroutine/dokey.md) calls ZINF
                 * [FRS1](../main/subroutine/frs1.md) calls ZINF
                 * [HAS1](../main/subroutine/has1.md) calls ZINF
                 * [KS4](../main/subroutine/ks4.md) calls ZINF
                 * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md) calls ZINF
                 * [SOLAR](../main/subroutine/solar.md) calls ZINF
                 * [TITLE](../main/subroutine/title.md) calls ZINF
                 * [Ze](../main/subroutine/ze.md) calls ZINF
    
    
    
    
    
    * * *
    
    
     Zero-fill the INWK ship workspace and reset the orientation vectors, with
     nosev pointing out of the screen, towards us.
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is set to &FF
    
    
    
    
    .ZINF
    
     LDY #NI%-1             \ There are NI% bytes in the INWK workspace, so set a
                            \ counter in Y so we can loop through them
    
     LDA #0                 \ Set A to 0 so we can zero-fill the workspace
    
    .ZI1
    
     STA INWK,Y             \ Zero the Y-th byte of the INWK workspace
    
     DEY                    \ Decrement the loop counter
    
     BPL ZI1                \ Loop back for the next byte, ending when we have
                            \ zero-filled the last byte at INWK, which leaves Y
                            \ with a value of &FF
    
                            \ Finally, we reset the orientation vectors as follows:
                            \
                            \   sidev = (1,  0,  0)
                            \   roofv = (0,  1,  0)
                            \   nosev = (0,  0, -1)
                            \
                            \ 96 * 256 (&6000) represents 1 in the orientation
                            \ vectors, while -96 * 256 (&E000) represents -1. We
                            \ already set the vectors to zero above, so we just
                            \ need to set up the high bytes of the diagonal values
                            \ and we're done. The negative nosev makes the ship
                            \ point towards us, as the z-axis points into the screen
    
     LDA #96                \ Set A to represent a 1 (in vector terms)
    
     STA INWK+18            \ Set byte #18 = roofv_y_hi = 96 = 1
    
     STA INWK+22            \ Set byte #22 = sidev_x_hi = 96 = 1
    
     ORA #%10000000         \ Flip the sign of A to represent a -1
    
     STA INWK+14            \ Set byte #14 = nosev_z_hi = -96 = -1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: msblob                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Display the dashboard's missile indicators in green
    
    
        Context: See this subroutine [on its own page](../main/subroutine/msblob.md)
     Variations: See [code variations](../related/compare/main/subroutine/msblob.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md) calls msblob
                 * [EQSHP](../main/subroutine/eqshp.md) calls msblob
                 * [SOS1](../main/subroutine/sos1.md) calls msblob
    
    
    
    
    
    * * *
    
    
     Display the dashboard's missile indicators, with all the missiles reset to
     green (i.e. not armed or locked).
    
    
    
    
    .msblob
    
     LDX #4                 \ Set up a loop counter in X to count through all four
                            \ missile indicators
    
    .ss
    
     CPX NOMSL              \ If the counter is equal to the number of missiles,
     BEQ SAL8               \ jump down to SAL8 to draw the remaining missiles, as
                            \ the rest of them are present and should be drawn in
                            \ green
    
     LDY #0                 \ Draw the missile indicator at position X in black
     JSR MSBAR
    
     DEX                    \ Decrement the counter to point to the next missile
    
     BNE ss                 \ Loop back to ss if we still have missiles to draw
    
     RTS                    \ Return from the subroutine
    
    .SAL8
    
     LDY #GREEN2            \ Draw the missile indicator at position X in green
     JSR MSBAR
    
     DEX                    \ Decrement the counter to point to the next missile
    
     BNE SAL8               \ Loop back to SAL8 if we still have missiles to draw
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: me2                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Remove an in-flight message from the space view
    
    
        Context: See this subroutine [on its own page](../main/subroutine/me2.md)
     Variations: See [code variations](../related/compare/main/subroutine/me2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md) calls me2
    
    
    
    
    
    
    .me2
    
     LDA QQ11               \ If this is not the space view, jump down to clynsneed
     BNE clynsneed          \ to skip displaying the in-flight message
    
     LDA MCH                \ Fetch the token number of the current message into A
    
     JSR MESS               \ Call MESS to print the token, which will remove it
                            \ from the screen as printing uses EOR logic
    
     LDA #0                 \ Set the delay in DLY to 0, so any new in-flight
     STA DLY                \ messages will be shown instantly
    
     JMP me3                \ Jump back into the main spawning loop at me3
    
    .clynsneed
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
     JMP me3                \ Jump back into the main spawning loop at me3
    
    
    
    
           Name: Ze                                                      [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Initialise the INWK workspace to a fairly aggressive ship
      Deep dive: [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ze.md)
     Variations: See [code variations](../related/compare/main/subroutine/ze.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](../main/subroutine/death.md) calls Ze
                 * [GTHG](../main/subroutine/gthg.md) calls Ze
                 * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md) calls Ze
                 * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md) calls Ze
    
    
    
    
    
    * * *
    
    
     Specifically, this routine does the following:
    
       * Reset the INWK ship workspace
    
       * Set the ship to a fair distance away in all axes, in front of us but
         randomly up or down, left or right
    
       * Give the ship a 4% chance of having E.C.M.
    
       * Set the ship's aggression level to at least 32 out of 63, with AI enabled
    
     This routine also sets A, X, T1 and the C flag to random values.
    
     Note that because this routine uses the value of X returned by DORND, and X
     contains the value of A returned by the previous call to DORND, this routine
     does not necessarily set the new ship to a totally random location.
    
    
    
    
    .Ze
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     JSR DORND              \ Set A and X to random numbers
    
     STA T1                 \ Store A in T1
    
     AND #%10000000         \ Extract the sign of A and store in x_sign
     STA INWK+2
    
     TXA                    \ Extract the sign of X and store in y_sign
     AND #%10000000
     STA INWK+5
    
     LDA #25                \ Set x_hi = y_hi = z_hi = 25, a fair distance away
     STA INWK+1
     STA INWK+4
     STA INWK+7
    
     TXA                    \ Set the C flag if X >= 245 (4% chance)
     CMP #245
    
     ROL A                  \ Set bit 0 of A to the C flag (i.e. there's a 4%
                            \ chance of this ship having E.C.M.)
    
     ORA #%11000000         \ Set bits 6 and 7 of A, so the ship has AI (bit 7) and
                            \ an aggression level of at least 32 out of 63
    
     STA INWK+32            \ Store A in the AI flag of this ship
    
                            \ Fall through into DORND2 to set A, X and the C flag
                            \ randomly
    
    
    
    
           Name: DORND                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Generate random numbers
      Deep dive: [Generating random numbers](https://elite.bbcelite.com/deep_dives/generating_random_numbers.html)
                 [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dornd.md)
     References: This subroutine is called as follows:
                 * [BOMBON](../main/subroutine/bombon.md) calls DORND
                 * [DEATH](../main/subroutine/death.md) calls DORND
                 * [DETOK2](../main/subroutine/detok2.md) calls DORND
                 * [GVL](../main/subroutine/gvl.md) calls DORND
                 * [HALL](../main/subroutine/hall.md) calls DORND
                 * [HAS1](../main/subroutine/has1.md) calls DORND
                 * [LASLI](../main/subroutine/lasli.md) calls DORND
                 * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md) calls DORND
                 * [Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md) calls DORND
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls DORND
                 * [Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md) calls DORND
                 * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md) calls DORND
                 * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md) calls DORND
                 * [MT18](../main/subroutine/mt18.md) calls DORND
                 * [nWq](../main/subroutine/nwq.md) calls DORND
                 * [OUCH](../main/subroutine/ouch.md) calls DORND
                 * [SFS1](../main/subroutine/sfs1.md) calls DORND
                 * [SOLAR](../main/subroutine/solar.md) calls DORND
                 * [SPIN](../main/subroutine/spin.md) calls DORND
                 * [STARS1](../main/subroutine/stars1.md) calls DORND
                 * [STARS2](../main/subroutine/stars2.md) calls DORND
                 * [STARS6](../main/subroutine/stars6.md) calls DORND
                 * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md) calls DORND
                 * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md) calls DORND
                 * [TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md) calls DORND
                 * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md) calls DORND
                 * [TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md) calls DORND
                 * [TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md) calls DORND
                 * [TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md) calls DORND
                 * [TT18](../main/subroutine/tt18.md) calls DORND
                 * [TT210](../main/subroutine/tt210.md) calls DORND
                 * [Ze](../main/subroutine/ze.md) calls DORND
    
    
    
    
    
    * * *
    
    
     Set A and X to random numbers (though note that X is set to the random number
     that was returned in A the last time DORND was called).
    
     The C and V flags are also set randomly.
    
     If we want to generate a repeatable sequence of random numbers, when
     generating explosion clouds, for example, then we call DORND2 to ensure that
     the value of the C flag on entry doesn't affect the outcome, as otherwise we
     might not get the same sequence of numbers if the C flag changes.
    
    
    
    * * *
    
    
     Other entry points:
    
       DORND2               Make sure the C flag doesn't affect the outcome
    
    
    
    
    .DORND2
    
     CLC                    \ Clear the C flag so the value of the C flag on entry
                            \ doesn't affect the outcome
    
    .DORND
    
     LDA RAND               \ Calculate the next two values f2 and f3 in the feeder
     ROL A                  \ sequence:
     TAX                    \
     ADC RAND+2             \   * f2 = (f1 << 1) mod 256 + C flag on entry
     STA RAND               \   * f3 = f0 + f2 + (1 if bit 7 of f1 is set)
     STX RAND+2             \   * C flag is set according to the f3 calculation
    
     LDA RAND+1             \ Calculate the next value m2 in the main sequence:
     TAX                    \
     ADC RAND+3             \   * A = m2 = m0 + m1 + C flag from feeder calculation
     STA RAND+1             \   * X = m1
     STX RAND+3             \   * C and V flags set according to the m2 calculation
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: Main game loop (Part 1 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Spawn a trader (a Cobra Mk III, Python, Boa or Anaconda)
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_game_loop_part_1_of_6.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_game_loop_part_1_of_6.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This is part of the main game loop. This is where the core loop of the game
     lives, and it's in two parts. The shorter loop (just parts 5 and 6) is
     iterated when we are docked, while the entire loop from part 1 to 6 iterates
     if we are in space.
    
     This section covers the following:
    
       * Spawn a trader, i.e. a Cobra Mk III, Python, Boa or Anaconda, with a 50%
         chance of it having an E.C.M., a 50% chance of it docking, a random
         aggression level, a speed between 16 and 31, and a gentle clockwise roll
    
     We call this from within the main loop.
    
    
    
    
    .MTT4
    
     JSR DORND              \ Set A and X to random numbers
    
     LSR A                  \ Clear bit 7 of our random number in A and set the C
                            \ flag to bit 0 of A, which is random
    
     STA INWK+32            \ Store this in the ship's AI flag, so this ship does
                            \ not have AI
    
     STA INWK+29            \ Store A in the ship's roll counter, giving it a
                            \ clockwise roll (as bit 7 is clear), and a 1 in 127
                            \ chance of it having no damping
    
     ROL INWK+31            \ This instruction would appear to set bit 0 of the
                            \ ship's missile count randomly (as the C flag was set),
                            \ giving the ship either no missiles or one missile
                            \
                            \ However, INWK+31 is overwritten in the call to the
                            \ NWSHP routine below, where it is set to the number of
                            \ missiles from the ship blueprint, and the value of the
                            \ C flag is not used, so this instruction actually has
                            \ no effect
                            \
                            \ Interestingly, the original source code for the NWSPS
                            \ routine also has an instruction that sets INWK+31 and
                            \ which gets overwritten when it falls through into
                            \ NWSHP, but in this case the instruction is commented
                            \ out in the source. Perhaps the original version of
                            \ NWSHP didn't set the missile count and instead relied
                            \ on the calling code to set it, and when the authors
                            \ changed it, they commented out the INWK+31 instruction
                            \ in NWSPS and forgot about this one. Who knows?
    
     AND #31                \ Set the ship speed to our random number, set to a
     ORA #16                \ minimum of 16 and a maximum of 31
     STA INWK+27
    
     JSR DORND              \ Set A and X to random numbers, plus the C flag
    
     BMI nodo               \ If A is negative (50% chance), jump to nodo to skip
                            \ the following
    
                            \ If we get here then we are going to spawn a ship that
                            \ is minding its own business and trying to dock
    
     LDA INWK+32            \ Set bits 6 and 7 of A, so the ship has AI (bit 7) and
     ORA #%11000000         \ an aggression level of at least 32 out of 63 (this
     STA INWK+32            \ makes the ship more likely to turn towards its target,
                            \ which in this case is the space station, as we are
                            \ about to set the ship flags so it is docking)
    
     LDX #%00010000         \ Set bit 4 of the ship's NEWB flags, to indicate that
     STX NEWB               \ this ship is docking
    
    .nodo
    
     AND #2                 \ This reduces A to a random value of either 0 or 2
    
     ADC #CYL               \ Set A = A + C + #CYL
                            \
                            \ where A is 0 or 2 and C is 0 or 1, so this gives us a
                            \ ship type from the following: Cobra Mk III, Python,
                            \ Boa or Anaconda
    
     CMP #HER               \ If A is now the ship type of a rock hermit, jump to
     BEQ TT100              \ TT100 to skip the following instruction
    
     JSR NWSHP              \ Add a new ship of type A to the local bubble and fall
                            \ through into the main game loop again
    
    
    
    
           Name: Main game loop (Part 2 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Call the main flight loop, and potentially spawn a trader, an
                 asteroid, or a cargo canister
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_game_loop_part_2_of_6.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_game_loop_part_2_of_6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md) calls via TT100
                 * [Main game loop (Part 6 of 6)](../main/subroutine/main_game_loop_part_6_of_6.md) calls via TT100
                 * [me2](../main/subroutine/me2.md) calls via me3
    
    
    
    
    
    * * *
    
    
     This section covers the following:
    
       * Call M% to do the main flight loop
    
       * Potentially spawn a trader, asteroid or cargo canister
    
    
    
    * * *
    
    
     Other entry points:
    
       TT100                The entry point for the start of the main game loop,
                            which calls the main flight loop and the moves into the
                            spawning routine
    
       me3                  Used by me2 to jump back into the main game loop after
                            printing an in-flight message
    
    
    
    
    .TT100
    
     JSR M%                 \ Call M% to iterate through the main flight loop
    
     DEC DLY                \ Decrement the delay counter in DLY, so any in-flight
                            \ messages get removed once the counter reaches zero
    
     BEQ me2                \ If DLY is now 0, jump to me2 to remove any in-flight
                            \ message from the space view, and once done, return to
                            \ me3 below, skipping the following two instructions
    
     BPL me3                \ If DLY is positive, jump to me3 to skip the next
                            \ instruction
    
     INC DLY                \ If we get here, DLY is negative, so we have gone too
                            \ and need to increment DLY back to 0
    
    .me3
    
     DEC MCNT               \ Decrement the main loop counter in MCNT
    
     BEQ P%+5               \ If the counter has reached zero, which it will do
                            \ every 256 main loops, skip the next JMP instruction
                            \ (or to put it another way, if the counter hasn't
                            \ reached zero, jump down to MLOOP, skipping all the
                            \ following checks)
    
    .ytq
    
     JMP MLOOP              \ Jump down to MLOOP to do some end-of-loop tidying and
                            \ restart the main loop
    
                            \ We only get here once every 256 iterations of the
                            \ main loop. If we aren't in witchspace and don't
                            \ already have 3 or more asteroids in our local bubble,
                            \ then this section has a 13% chance of spawning
                            \ something benign (the other 87% of the time we jump
                            \ down to consider spawning cops, pirates and bounty
                            \ hunters)
                            \
                            \ If we are in that 13%, then 50% of the time this will
                            \ be a trader, and the other 50% of the time it will
                            \ either be an asteroid (98.5% chance) or, very rarely,
                            \ a cargo canister (1.5% chance)
    
     LDA MJ                 \ If we are in witchspace following a mis-jump, skip the
     BNE ytq                \ following by jumping down to MLOOP (via ytq above)
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #35                \ If A >= 35 (87% chance), jump down to MTT1 to skip
     BCS MTT1               \ the spawning of an asteroid or cargo canister and
                            \ potentially spawn something else
    
     LDA JUNK               \ If we already have 3 or more bits of junk in the local
     CMP #3                 \ bubble, jump down to MTT1 to skip the following and
     BCS MTT1               \ potentially spawn something else
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     LDA #38                \ Set z_hi = 38 (far away)
     STA INWK+7
    
     JSR DORND              \ Set A, X and C flag to random numbers
    
     STA INWK               \ Set x_lo = random
    
     STX INWK+3             \ Set y_lo = random
                            \
                            \ Note that because we use the value of X returned by
                            \ DORND, and X contains the value of A returned by the
                            \ previous call to DORND, this does not set the new ship
                            \ to a totally random location
    
     AND #%10000000         \ Set x_sign = bit 7 of x_lo
     STA INWK+2
    
     TXA                    \ Set y_sign = bit 7 of y_lo
     AND #%10000000
     STA INWK+5
    
     ROL INWK+1             \ Set bit 1 of x_hi to the C flag, which is random, so
     ROL INWK+1             \ this randomly moves us off-centre by 512 (as if x_hi
                            \ is %00000010, then (x_hi x_lo) is 512 + x_lo)
    
     JSR DORND              \ Set A, X and V flag to random numbers
    
     BVS MTT4               \ If V flag is set (50% chance), jump up to MTT4 to
                            \ spawn a trader
    
     ORA #%01101111         \ Take the random number in A and set bits 0-3 and 5-6,
     STA INWK+29            \ so the result has a 50% chance of being positive or
                            \ negative, and a 50% chance of bits 0-6 being 127.
                            \ Storing this number in the roll counter therefore
                            \ gives our new ship a fast roll speed with a 50%
                            \ chance of having no damping, plus a 50% chance of
                            \ rolling clockwise or anti-clockwise
    
     LDA SSPR               \ If we are inside the space station safe zone, jump
     BNE MTT1               \ down to MTT1 to skip the following and potentially
                            \ spawn something else
    
     TXA                    \ Set A to the random X we set above, which we haven't
     BCS MTT2               \ used yet, and if the C flag is set (50% chance) jump
                            \ down to MTT2 to skip the following
    
     AND #31                \ Set the ship speed to our random number, set to a
     ORA #16                \ minimum of 16 and a maximum of 31
     STA INWK+27
    
     BCC MTT3               \ Jump down to MTT3, skipping the following (this BCC
                            \ is effectively a JMP as we know the C flag is clear,
                            \ having passed through the BCS above)
    
    .MTT2
    
     ORA #%01111111         \ Set bits 0-6 of A to 127, leaving bit 7 as random, so
     STA INWK+30            \ storing this number in the pitch counter means we have
                            \ full pitch with no damping, with a 50% chance of
                            \ pitching up or down
    
    .MTT3
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #252               \ If random A < 252 (98.8% of the time), jump to thongs
     BCC thongs             \ to skip the following
    
     LDA #HER               \ Set A to #HER so we spawn a rock hermit 1.2% of the
                            \ time
    
     STA INWK+32            \ Set byte #32 to %00001111 to give the rock hermit an
                            \ E.C.M.
    
     BNE whips              \ Jump to whips (this BNE is effectively a JMP as A will
                            \ never be zero)
    
    .thongs
    
     CMP #10                \ If random A >= 10 (96% of the time), set the C flag
    
     AND #1                 \ Reduce A to a random number that's 0 or 1
    
     ADC #OIL               \ Set A = #OIL + A + C, so there's a 2% chance of us
                            \ spawning a cargo canister (#OIL), a 50% chance of
                            \ us spawning a boulder (#OIL + 1), and a 48% chance of
                            \ us spawning an asteroid (#OIL + 2)
    
    .whips
    
     JSR NWSHP              \ Add our new asteroid or canister to the universe
    
    
    
    
           Name: Main game loop (Part 3 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Potentially spawn a cop, particularly if we've been bad
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [Ship data blocks](https://elite.bbcelite.com/deep_dives/ship_data_blocks.html)
                 [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_game_loop_part_3_of_6.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_game_loop_part_3_of_6.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This section covers the following:
    
       * Potentially spawn a cop (in a Viper), very rarely if we have been good,
         more often if have been naughty, and very often if we have been properly
         bad
    
       * Very rarely, consider spawning a Thargoid, or vanishingly rarely, a Cougar
    
    
    
    
    .MTT1
    
     LDA SSPR               \ If we are outside the space station's safe zone, skip
     BEQ P%+5               \ the following instruction
    
    .MLOOPS
    
     JMP MLOOP              \ Jump to MLOOP to skip the following
    
     JSR BAD                \ Call BAD to work out how much illegal contraband we
                            \ are carrying in our hold (A is up to 40 for a
                            \ standard hold crammed with contraband, up to 70 for
                            \ an extended cargo hold full of narcotics and slaves)
    
     ASL A                  \ Double A to a maximum of 80 or 140
    
     LDX MANY+COPS          \ If there are no cops in the local bubble, skip the
     BEQ P%+5               \ next instruction
    
     ORA FIST               \ There are cops in the vicinity and we've got a hold
                            \ full of jail time, so OR the value in A with FIST to
                            \ get a new value that is at least as high as both
                            \ values, to reflect the fact that they have almost
                            \ certainly scanned our ship
    
     STA T                  \ Store our badness level in T
    
     JSR Ze                 \ Call Ze to initialise INWK to a fairly aggressive
                            \ ship, and set A and X to random values
                            \
                            \ Note that because Ze uses the value of X returned by
                            \ DORND, and X contains the value of A returned by the
                            \ previous call to DORND, this does not set the new ship
                            \ to a totally random location
    
     CMP #136               \ If the random number in A = 136 (0.4% chance), jump
     BEQ fothg              \ to fothg in part 4 to spawn either a Thargoid or, very
                            \ rarely, a Cougar
    
     CMP T                  \ If the random value in A >= our badness level, which
     BCS P%+7               \ will be the case unless we have been really, really
                            \ bad, then skip the following two instructions (so
                            \ if we are really bad, there's a higher chance of
                            \ spawning a cop, otherwise we got away with it, for
                            \ now)
    
     LDA #COPS              \ Add a new police ship to the local bubble
     JSR NWSHP
    
     LDA MANY+COPS          \ If we now have at least one cop in the local bubble,
     BNE MLOOPS             \ jump down to MLOOPS to stop spawning, otherwise fall
                            \ through into the next part to look at spawning
                            \ something else
    
    
    
    
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
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_game_loop_part_4_of_6.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_game_loop_part_4_of_6.md) for this subroutine in the different versions
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
    
    
    
    
           Name: Main game loop (Part 5 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Cool down lasers, make calls to update the dashboard
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
                 [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_game_loop_part_5_of_6.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_game_loop_part_5_of_6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md) calls via MLOOP
                 * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md) calls via MLOOP
                 * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md) calls via MLOOP
                 * [Main game loop (Part 6 of 6)](../main/subroutine/main_game_loop_part_6_of_6.md) calls via MLOOP
    
    
    
    
    
    * * *
    
    
     This is the first half of the minimal game loop, which we iterate when we are
     docked. This section covers the following:
    
       * Cool down lasers
    
       * Make calls to update the dashboard
    
    
    
    * * *
    
    
     Other entry points:
    
       MLOOP                The entry point for the main game loop. This entry point
                            comes after the call to the main flight loop and
                            spawning routines, so it marks the start of the main
                            game loop for when we are docked (as we don't need to
                            call the main flight loop or spawning routines if we
                            aren't in space)
    
    
    
    
    .MLOOP
    
     LDX #&FF               \ Set the stack pointer to &01FF, which is the standard
     TXS                    \ location for the 6502 stack, so this instruction
                            \ effectively resets the stack
    
     LDX GNTMP              \ If the laser temperature in GNTMP is non-zero,
     BEQ EE20               \ decrement it (i.e. cool it down a bit)
     DEC GNTMP
    
    .EE20
    
     LDX LASCT              \ Set X to the value of LASCT, the laser pulse count
    
     BEQ NOLASCT            \ If X = 0 then jump to NOLASCT to skip reducing LASCT,
                            \ as it can't be reduced any further
    
     DEX                    \ Decrement the value of LASCT in X
    
     BEQ P%+3               \ If X = 0, skip the next instruction
    
     DEX                    \ Decrement the value of LASCT in X again
    
     STX LASCT              \ Store the decremented value of X in LASCT, so LASCT
                            \ gets reduced by 2, but not into negative territory
    
    .NOLASCT
    
    \LDA QQ11               \ These instructions are commented out in the original
    \BNE P%+5               \ source
    
     JSR DIALS              \ Call DIALS to update the dashboard
    
     LDA QQ11               \ If this is a space view, jump to plus13 to skip the
     BEQ plus13             \ following five instructions
    
     AND PATG               \ If PATG = &FF (author names are shown on start-up)
     LSR A                  \ and bit 0 of QQ11 is 1 (the current view is type 1),
     BCS plus13             \ then skip the following two instructions
    
     LDY #2                 \ Wait for 2/50 of a second (0.04 seconds), to slow the
     JSR DELAY              \ main loop down a bit
    
    .plus13
    
     JSR TT17               \ Scan the keyboard for the cursor keys or joystick,
                            \ returning the cursor's delta values in X and Y and
                            \ the key pressed in A
    
    
    
    
           Name: Main game loop (Part 6 of 6)                            [Show more]
           Type: Subroutine
       Category: Main loop
        Summary: Process non-flight key presses (red function keys, docked keys)
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/main_game_loop_part_6_of_6.md)
     Variations: See [code variations](../related/compare/main/subroutine/main_game_loop_part_6_of_6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BAY](../main/subroutine/bay.md) calls via FRCE
                 * [TT219](../main/subroutine/tt219.md) calls via FRCE
    
    
    
    
    
    * * *
    
    
     This is the second half of the minimal game loop, which we iterate when we are
     docked. This section covers the following:
    
       * Process more key presses (red function keys, docked keys etc.)
    
     It also supports joining the main loop with a key already "pressed", so we can
     jump into the main game loop to perform a specific action. In practice, this
     is used when we enter the docking bay in BAY to display Status Mode (red key
     f8), and when we finish buying or selling cargo in BAY2 to jump to the
     Inventory (red key f9).
    
    
    
    * * *
    
    
     Other entry points:
    
       FRCE                 The entry point for the main game loop if we want to
                            jump straight to a specific screen, by pretending to
                            "press" a key, in which case A contains the internal key
                            number of the key we want to "press"
    
    
    
    
    .FRCE
    
     JSR TT102              \ Call TT102 to process the key pressed in A
    
     LDA QQ12               \ Fetch the docked flag from QQ12 into A
    
     BEQ P%+5               \ If we are docked, loop back up to MLOOP just above
     JMP MLOOP              \ to restart the main loop, but skipping all the flight
                            \ and spawning code in the top part of the main loop
    
     JMP TT100              \ Otherwise jump to TT100 to restart the main loop from
                            \ the start
    
    
    
    
           Name: TT102                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Process function key, save key, hyperspace and chart key presses
                 and update the hyperspace counter
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt102.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt102.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 6 of 6)](../main/subroutine/main_game_loop_part_6_of_6.md) calls TT102
                 * [HME2](../main/subroutine/hme2.md) calls via T95
    
    
    
    
    
    * * *
    
    
     Process function key presses, plus "@" (save commander), "H" (hyperspace),
     "D" (show distance to system) and "O" (move chart cursor back to current
     system). We can also pass cursor position deltas in X and Y to indicate that
     the cursor keys or joystick have been used (i.e. the values that are returned
     by routine TT17).
    
     This routine also checks for the "F" key press (search for a system), which
     applies to enhanced versions only.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The internal key number of the key pressed (see page 142
                            of the "Advanced User Guide for the BBC Micro" by Bray,
                            Dickens and Holmes for a list of internal key numbers)
    
       X                    The amount to move the crosshairs in the x-axis
    
       Y                    The amount to move the crosshairs in the y-axis
    
    
    
    * * *
    
    
     Other entry points:
    
       T95                  Print the distance to the selected system
    
    
    
    
    .TT102
    
     CMP #f8                \ If red key f8 was pressed, jump to STATUS to show the
     BNE P%+5               \ Status Mode screen, returning from the subroutine
     JMP STATUS             \ using a tail call
    
     CMP #f4                \ If red key f4 was pressed, jump to TT22 to show the
     BNE P%+5               \ Long-range Chart, returning from the subroutine using
     JMP TT22               \ a tail call
    
     CMP #f5                \ If red key f5 was pressed, jump to TT23 to show the
     BNE P%+5               \ Short-range Chart, returning from the subroutine using
     JMP TT23               \ a tail call
    
     CMP #f6                \ If red key f6 was pressed, call TT111 to select the
     BNE TT92               \ system nearest to galactic coordinates (QQ9, QQ10)
     JSR TT111              \ (the location of the chart crosshairs) and set ZZ to
     JMP TT25               \ the system number, and then jump to TT25 to show the
                            \ Data on System screen (along with an extended system
                            \ description for the system in ZZ if we're docked),
                            \ returning from the subroutine using a tail call
    
    .TT92
    
     CMP #f9                \ If red key f9 was pressed, jump to TT213 to show the
     BNE P%+5               \ Inventory screen, returning from the subroutine
     JMP TT213              \ using a tail call
    
     CMP #f7                \ If red key f7 was pressed, jump to TT167 to show the
     BNE P%+5               \ Market Price screen, returning from the subroutine
     JMP TT167              \ using a tail call
    
     CMP #f0                \ If red key f0 was pressed, jump to TT110 to launch our
     BNE fvw                \ ship (if docked), returning from the subroutine using
    \JSR CTRL               \ a tail call
    \BPL P%+5               \
    \JMP HALL               \ Three instructions are commented out in the original
     JMP TT110              \ source, which would show the ship hangar instead of
                            \ launching if CTRL is held down, so presumably they
                            \ were put in for testing
    
    .fvw
    
     BIT QQ12               \ If bit 7 of QQ12 is clear (i.e. we are not docked, but
     BPL INSP               \ in space), jump to INSP to skip the following checks
                            \ for f1-f3 and "@" (save commander file) key presses
    
     CMP #f3                \ If red key f3 was pressed, jump to EQSHP to show the
     BNE P%+5               \ Equip Ship screen, returning from the subroutine using
     JMP EQSHP              \ a tail call
    
     CMP #f1                \ If red key f1 was pressed, jump to TT219 to show the
     BNE P%+5               \ Buy Cargo screen, returning from the subroutine using
     JMP TT219              \ a tail call
    
     CMP #'@'               \ If "@" was not pressed, skip to nosave
     BNE nosave
    
     JSR SVE                \ "@" was pressed, so call SVE to show the disc access
                            \ menu
    
     BCC P%+5               \ If the C flag was set by SVE, then we loaded a new
     JMP QU5                \ commander file, so jump to QU5 to restart the game
                            \ with the newly loaded commander
    
     JMP BAY                \ Otherwise the C flag was clear, so jump to BAY to go
                            \ to the docking bay (i.e. show the Status Mode screen)
    
    .nosave
    
     CMP #f2                \ If red key f2 was pressed, jump to TT208 to show the
     BNE LABEL_3            \ Sell Cargo screen, returning from the subroutine using
     JMP TT208              \ a tail call
    
    .INSP
    
     CMP #f1                \ If red key f1 was pressed, jump to chview1
     BEQ chview1
    
     CMP #f2                \ If red key f2 was pressed, jump to chview2
     BEQ chview2
    
     CMP #f3                \ If red key f3 was not pressed, jump to LABEL_3 to keep
     BNE LABEL_3            \ checking for which key was pressed
    
     LDX #3                 \ Red key f3 was pressed, so set the view number in X to
                            \ 3 for the right view
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A2 &02, or BIT &02A2, which does nothing apart
                            \ from affect the flags
    
    .chview2
    
     LDX #2                 \ If we jump to here, red key f2 was pressed, so set the
                            \ view number in X to 2 for the left view
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A2 &01, or BIT &02A2, which does nothing apart
                            \ from affect the flags
    
    .chview1
    
     LDX #1                 \ If we jump to here, red key f1 was pressed, so set the
                            \ view number in X to 1 for the rear view
    
     JMP LOOK1              \ Jump to LOOK1 to switch to view X (rear, left or
                            \ right), returning from the subroutine using a tail
                            \ call
    
    .LABEL_3
    
     LDA KL                 \ If "H" was not pressed, jump to NWDAV5 to skip the
     CMP #'H'               \ following
     BNE NWDAV5
    
     JMP hyp                \ Jump to hyp to do a hyperspace jump (if we are in
                            \ space), returning from the subroutine using a tail
                            \ call
    
    .NWDAV5
    
     CMP #'D'               \ If "D" was pressed, jump to T95 to print the distance
     BEQ T95                \ to a system (if we are in one of the chart screens)
    
     CMP #'F'               \ If "F" was not pressed, jump down to HME1, otherwise
     BNE HME1               \ keep going to process searching for systems
    
     LDA QQ12               \ If QQ12 = 0 (we are not docked), we can't search for
     BEQ t95                \ systems, so return from the subroutine (as t95
                            \ contains an RTS)
    
     LDA QQ11               \ If the current view is a chart (QQ11 = 64 or 128),
     AND #%11000000         \ keep going, otherwise return from the subroutine (as
     BEQ t95                \ t95 contains an RTS)
    
     JMP HME2               \ Jump to HME2 to let us search for a system, returning
                            \ from the subroutine using a tail call
    
    .HME1
    
     STA T1                 \ Store A (the key that's been pressed) in T1
    
     LDA QQ11               \ If the current view is a chart (QQ11 = 64 or 128),
     AND #%11000000         \ keep going, otherwise jump down to TT107 to skip the
     BEQ TT107              \ following
    
     LDA QQ22+1             \ If the on-screen hyperspace counter is non-zero,
     BNE TT107              \ then we are already counting down, so jump to TT107
                            \ to skip the following
    
     LDA T1                 \ Restore the original value of A (the key that's been
                            \ pressed) from T1
    
     CMP #'O'               \ If "O" was pressed, do the following three jumps,
     BNE ee2                \ otherwise skip to ee2 to continue
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will erase the crosshairs currently there
    
     JSR ping               \ Set the target system to the current system (which
                            \ will move the location in (QQ9, QQ10) to the current
                            \ home system
    
     JMP TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will draw the crosshairs at our current home
                            \ system, and return from the subroutine using a tail
                            \ call
    
    .ee2
    
     JSR TT16               \ Call TT16 to move the crosshairs by the amount in X
                            \ and Y, which were passed to this subroutine as
                            \ arguments
    
    .TT107
    
     LDA QQ22+1             \ If the on-screen hyperspace counter is zero, return
     BEQ t95                \ from the subroutine (as t95 contains an RTS), as we
                            \ are not currently counting down to a hyperspace jump
    
     DEC QQ22               \ Decrement the internal hyperspace counter
    
     BNE t95                \ If the internal hyperspace counter is still non-zero,
                            \ then we are still counting down, so return from the
                            \ subroutine (as t95 contains an RTS)
    
                            \ If we get here then the internal hyperspace counter
                            \ has just reached zero and it wasn't zero before, so
                            \ we need to reduce the on-screen counter and update
                            \ the screen. We do this by first printing the next
                            \ number in the countdown sequence, and then printing
                            \ the old number, which will erase the old number
                            \ and display the new one because printing uses EOR
                            \ logic
    
     LDX QQ22+1             \ Set X = the on-screen hyperspace counter - 1
     DEX                    \ (i.e. the next number in the sequence)
    
     JSR ee3                \ Print the 8-bit number in X at text location (0, 1)
    
     LDA #5                 \ Reset the internal hyperspace counter to 5
     STA QQ22
    
     LDX QQ22+1             \ Set X = the on-screen hyperspace counter (i.e. the
                            \ current number in the sequence, which is already
                            \ shown on-screen)
    
     JSR ee3                \ Print the 8-bit number in X at text location (0, 1),
                            \ i.e. print the hyperspace countdown in the top-left
                            \ corner
    
     DEC QQ22+1             \ Decrement the on-screen hyperspace countdown
    
     BNE t95                \ If the countdown is not yet at zero, return from the
                            \ subroutine (as t95 contains an RTS)
    
     JMP TT18               \ Otherwise the countdown has finished, so jump to TT18
                            \ to do a hyperspace jump, returning from the subroutine
                            \ using a tail call
    
    .t95
    
     RTS                    \ Return from the subroutine
    
    .T95
    
                            \ If we get here, "D" was pressed, so we need to show
                            \ the distance to the selected system (if we are in a
                            \ chart view)
    
     LDA QQ11               \ If the current view is a chart (QQ11 = 64 or 128),
     AND #%11000000         \ keep going, otherwise return from the subroutine (as
     BEQ t95                \ t95 contains an RTS)
    
     JSR hm                 \ Call hm to move the crosshairs to the target system
                            \ in (QQ9, QQ10), returning with A = 0
    
    \STA QQ17               \ This instruction is commented out in the original
                            \ source
    
     JSR cpl                \ Print control code 3 (the selected system name)
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch to Sentence Case, with the
     STA QQ17               \ next letter in capitals
    
     LDA #12                \ Print a line feed to move the text cursor down a line
     JSR TT26
    
     JMP TT146              \ Print the distance to the selected system and return
                            \ from the subroutine using a tail call
    
    
    
    
           Name: BAD                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Calculate how bad we have been
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bad.md)
     References: This subroutine is called as follows:
                 * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md) calls BAD
                 * [TT110](../main/subroutine/tt110.md) calls BAD
    
    
    
    
    
    * * *
    
    
     Work out how bad we are from the amount of contraband in our hold. The
     formula is:
    
       (slaves + narcotics) * 2 + firearms
    
     so slaves and narcotics are twice as illegal as firearms. The value in FIST
     (our legal status) is set to at least this value whenever we launch from a
     space station, and a FIST of 50 or more gives us fugitive status, so leaving a
     station carrying 25 tonnes of slaves/narcotics, or 50 tonnes of firearms
     across multiple trips, is enough to make us a fugitive.
    
    
    
    * * *
    
    
     Returns:
    
       A                    A value that determines how bad we are from the amount
                            of contraband in our hold
    
    
    
    
    .BAD
    
     LDA QQ20+3             \ Set A to the number of tonnes of slaves in the hold
    
     CLC                    \ Clear the C flag so we can do addition without the
                            \ C flag affecting the result
    
     ADC QQ20+6             \ Add the number of tonnes of narcotics in the hold
    
     ASL A                  \ Double the result and add the number of tonnes of
     ADC QQ20+10            \ firearms in the hold
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: FAROF                                                   [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Compare x_hi, y_hi and z_hi with 224
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/farof.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md) calls FAROF
    
    
    
    
    
    * * *
    
    
     Compare x_hi, y_hi and z_hi with 224, and set the C flag if all three <= 224,
     otherwise clear the C flag.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if x_hi <= 224 and y_hi <= 224 and z_hi <= 224
    
                            Clear otherwise (i.e. if any one of them are bigger than
                            224)
    
    
    
    
    .FAROF
    
     LDA #224               \ Set A = 224 and fall through into FAROF2 to do the
                            \ comparison
    
    
    
    
           Name: FAROF2                                                  [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Compare x_hi, y_hi and z_hi with A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/farof2.md)
     Variations: See [code variations](../related/compare/main/subroutine/farof2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md) calls FAROF2
    
    
    
    
    
    * * *
    
    
     Compare x_hi, y_hi and z_hi with A, and set the C flag if all three <= A,
     otherwise clear the C flag.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if x_hi <= A and y_hi <= A and z_hi <= A
    
                            Clear otherwise (i.e. if any one of them are bigger than
                            A)
    
    
    
    
    .FAROF2
    
     CMP INWK+1             \ If A < x_hi, C will be clear so jump to FA1 to
     BCC FA1                \ return from the subroutine with C clear, otherwise
                            \ C will be set so move on to the next one
    
     CMP INWK+4             \ If A < y_hi, C will be clear so jump to FA1 to
     BCC FA1                \ return from the subroutine with C clear, otherwise
                            \ C will be set so move on to the next one
    
     CMP INWK+7             \ If A < z_hi, C will be clear, otherwise C will be set
    
    .FA1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MAS4                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate a cap on the maximum distance to a ship
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mas4.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md) calls MAS4
                 * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md) calls MAS4
                 * [TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md) calls MAS4
    
    
    
    
    
    * * *
    
    
     Logical OR the value in A with the high bytes of the ship's position (x_hi,
     y_hi and z_hi).
    
    
    
    * * *
    
    
     Returns:
    
       A                    A OR x_hi OR y_hi OR z_hi
    
    
    
    
    .MAS4
    
     ORA INWK+1             \ OR A with x_hi, y_hi and z_hi
     ORA INWK+4
     ORA INWK+7
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: stackpt                                                 [Show more]
           Type: Variable
       Category: Save and load
        Summary: Temporary storage for the stack pointer when jumping to the break
                 handler at NEWBRK
    
    
        Context: See this variable [on its own page](../main/variable/stackpt.md)
     Variations: See [code variations](../related/compare/main/variable/stack-stackpt.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [NEWBRK](../main/subroutine/newbrk.md) uses stackpt
                 * [SVE](../main/subroutine/sve.md) uses stackpt
    
    
    
    
    
    
    .stackpt
    
     EQUB &FF
    
    
    
    
           Name: NEWBRK                                                  [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: The standard BRKV handler for the game
    
    
        Context: See this subroutine [on its own page](../main/subroutine/newbrk.md)
     Variations: See [code variations](../related/compare/main/subroutine/brbr-newbrk.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [COLD](../main/subroutine/cold.md) calls NEWBRK
    
    
    
    
    
    * * *
    
    
     This routine is used to display error messages, before restarting the game.
     When called, it makes a beep and prints the system error message in the block
     pointed to by (&FD &FE), which is where the MOS will put any system errors. It
     then waits for a key press and restarts the game.
    
    
    
    
    .NEWBRK
    
     LDX stackpt            \ Set the stack pointer to the value that we stored in
     TXS                    \ location stack, so that's back to the value it had
                            \ before we change it in the SVE routine
    
     JSR getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000, as this will have been stored in
                            \ the buffer before performing the disc access that gave
                            \ the error we're processsing
    
     STZ CATF               \ Set the CATF flag to 0, so the TT26 routine reverts to
                            \ standard formatting
    
     LDY #0                 \ Set Y to 0, which we use as a loop counter below
    
     LDA #7                 \ Set A = 7 to generate a beep before we print the error
                            \ message
    
    .BRBRLOOP
    
     JSR CHPR               \ Print the character in A, which contains a line feed
                            \ on the first loop iteration, and then any non-zero
                            \ characters we fetch from the error message
    
     INY                    \ Increment the loop counter
    
     LDA (&FD),Y            \ Fetch the Y-th byte of the block pointed to by
                            \ (&FD &FE), so that's the Y-th character of the message
                            \ pointed to by the error message pointer
    
     BNE BRBRLOOP           \ If the fetched character is non-zero, loop back to the
                            \ JSR OSWRCH above to print the it, and keep looping
                            \ until we fetch a zero (which marks the end of the
                            \ message)
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     JMP SVE                \ Jump to SVE to display the disc access menu and return
                            \ from the subroutine using a tail call
    
    
    
    
           Name: DEATH                                                   [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Display the death screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/death.md)
     Variations: See [code variations](../related/compare/main/subroutine/death.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md) calls DEATH
                 * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md) calls DEATH
                 * [OOPS](../main/subroutine/oops.md) calls DEATH
    
    
    
    
    
    * * *
    
    
     We have been killed, so display the chaos of our destruction above a "GAME
     OVER" sign, and clean up the mess ready for the next attempt.
    
    
    
    
    .DEATH
    
     LDY #soexpl            \ Call the NOISE routine with Y = 4 to make the sound of
     JSR NOISE              \ us dying
    
     JSR RES2               \ Reset a number of flight variables and workspaces
    
     ASL DELTA              \ Divide our speed in DELTA by 4
     ASL DELTA
    
     LDX #24                \ Set the screen to only show 24 text rows, which hides
     JSR DET1               \ the dashboard, setting A to 6 in the process
    
     LDA #13                \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 13 (which
                            \ is not a space view, though I'm not quite sure why
                            \ this value is chosen, as it gets overwritten by the
                            \ next instruction anyway)
    
     STZ QQ11               \ Set QQ11 to 0, so from here on we are using a space
                            \ view
    
     JSR BOX                \ Call BOX to redraw the same border box (BOX is part
                            \ of TT66), which removes the border as it is drawn
                            \ using EOR logic
    
     JSR nWq                \ Create a cloud of stardust containing the correct
                            \ number of dust particles (i.e. NOSTM of them)
    
     LDA #CYAN              \ Change the current colour to cyan
     STA COL
    
     LDA #12                \ Move the text cursor to column 12 on row 12
     STA XC
     STA YC
    
     LDA #146               \ Print recursive token 146 ("{all caps}GAME OVER")
     JSR ex
    
    .D1
    
     JSR Ze                 \ Call Ze to initialise INWK to a fairly aggressive
                            \ ship, and set A and X to random values
    
     LSR A                  \ Set A = A / 4, so A is now between 0 and 63, and
     LSR A                  \ store in byte #0 (x_lo)
     STA INWK
    
     LDY #0                 \ Set the following to 0: x_hi, y_hi, z_hi and the AI
     STY INWK+1             \ flag (no AI or E.C.M. and zero aggression)
     STY INWK+4
     STY INWK+7
     STY INWK+32
    
     DEY                    \ Set Y = 255
    
     STY MCNT               \ Reset the main loop counter to 255, so all timer-based
                            \ calls will be stopped
    
     EOR #%00101010         \ Flip bits 1, 3 and 5 in A (x_lo) to get another number
     STA INWK+3             \ between 48 and 63, and store in byte #3 (y_lo)
    
     ORA #%01010000         \ Set bits 4 and 6 of A to bump it up to between 112 and
     STA INWK+6             \ 127, and store in byte #6 (z_lo)
    
     TXA                    \ Set A to the random number in X and keep bits 0-3 and
     AND #%10001111         \ the sign in bit 7 to get a number between -15 and +15,
     STA INWK+29            \ and store in byte #29 (roll counter) to give our ship
                            \ a gentle roll with damping
    
     LDY #64                \ Set the laser count to 64 to act as a counter in the
     STY LASCT              \ D2 loop below, so this setting determines how long the
                            \ death animation lasts (it's 64 * 2 iterations of the
                            \ main flight loop)
    
     SEC                    \ Set the C flag
    
     ROR A                  \ This sets A to a number between 0 and +7, which we
     AND #%10000111         \ store in byte #30 (the pitch counter) to give our ship
     STA INWK+30            \ a very gentle downwards pitch with damping
    
     LDX #OIL               \ Set X to #OIL, the ship type for a cargo canister
    
     LDA XX21-1+2*PLT       \ Fetch the byte from location XX21 - 1 + 2 * PLT, which
                            \ equates to XX21 + 7 (the high byte of the address of
                            \ SHIP_PLATE), which seems a bit odd. It might make more
                            \ sense to do LDA (XX21-2+2*PLT) as this would fetch the
                            \ first byte of the alloy plate's blueprint (which
                            \ determines what happens when alloys are destroyed),
                            \ but there aren't any brackets, so instead this always
                            \ returns &D0, which is never zero, so the following
                            \ BEQ is never true. (If the brackets were there, then
                            \ we could stop plates from spawning on death by setting
                            \ byte #0 of the blueprint to 0... but then scooping
                            \ plates wouldn't give us alloys, so who knows what this
                            \ is all about?)
    
     BEQ D3                 \ If A = 0, jump to D3 to skip the following instruction
    
     BCC D3                 \ If the C flag is clear, which will be random following
                            \ the above call to Ze, jump to D3 to skip the following
                            \ instruction
    
     DEX                    \ Decrement X, which sets it to #PLT, the ship type for
                            \ an alloy plate
    
    .D3
    
     JSR fq1                \ Call fq1 with X set to #OIL or #PLT, which adds a new
                            \ cargo canister or alloy plate to our local bubble of
                            \ universe and points it away from us with double DELTA
                            \ speed (i.e. 6, as DELTA was set to 3 by the call to
                            \ RES2 above). INF is set to point to the new arrival's
                            \ ship data block in K%
    
     JSR DORND              \ Set A and X to random numbers and extract bit 7 from A
     AND #%10000000
    
     LDY #31                \ Store this in byte #31 of the ship's data block, so it
     STA (INF),Y            \ has a 50% chance of marking our new arrival as being
                            \ killed (so it will explode)
    
     LDA FRIN+4             \ The call we made to RES2 before we entered the loop at
     BEQ D1                 \ D1 will have reset all the ship slots at FRIN, so this
                            \ checks to see if the fifth slot is empty, and if it
                            \ is we loop back to D1 to add another canister, until
                            \ we have added five of them
    
    \JSR U%                 \ This instruction is commented out in the original
                            \ source
    
     LDA #0                 \ Set our speed in DELTA to 0, as we aren't going
     STA DELTA              \ anywhere any more
    
     JSR M%                 \ Call the M% routine to do the main flight loop once,
                            \ which will display our exploding canister scene and
                            \ move everything about, as well as decrementing the
                            \ value in LASCT
    
    \JSR NOSPRITES          \ This instruction is commented out in the original
                            \ source
    
    .D2
    
     JSR M%                 \ Call the M% routine to do the main flight loop once,
                            \ which will display our exploding canister scene and
                            \ move everything about, as well as decrementing the
                            \ value in LASCT
    
     DEC LASCT              \ Decrement the counter in LASCT, which we set above,
                            \ so for each loop around D2, we decrement LASCT by 5
                            \ (the main loop decrements it by 4, and this one makes
                            \ it 5)
    
     BNE D2                 \ Loop back to call the main flight loop again, until we
                            \ have called it 127 times
    
     LDX #31                \ Set the screen to show all 31 text rows, which shows
     JSR DET1               \ the dashboard
    
     JMP DEATH2             \ Jump to DEATH2 to reset and restart the game
    
    
    
    
           Name: spasto                                                  [Show more]
           Type: Variable
       Category: Universe
        Summary: Contains the address of the Coriolis space station's ship
                 blueprint
    
    
        Context: See this variable [on its own page](../main/variable/spasto.md)
     References: This variable is used as follows:
                 * [BEGIN](../main/subroutine/begin.md) uses spasto
                 * [NWSPS](../main/subroutine/nwsps.md) uses spasto
    
    
    
    
    
    
    .spasto
    
     EQUW &8888             \ This variable is set by routine BEGIN to the address
                            \ of the Coriolis space station's ship blueprint
    
    
    
    
           Name: BEGIN                                                   [Show more]
           Type: Subroutine
       Category: Loader
        Summary: Initialise the configuration variables and start the game
    
    
        Context: See this subroutine [on its own page](../main/subroutine/begin.md)
     Variations: See [code variations](../related/compare/main/subroutine/begin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [S%](../main/subroutine/s_per_cent.md) calls BEGIN
    
    
    
    
    
    
    .BEGIN
    
     LDX #(DISK-COMC)       \ We start by zeroing all the configuration variables
                            \ between COMC and DISK, to set them to their default
                            \ values, so set a counter in X for DISK - COMC bytes
    
     LDA #0                 \ Set A = 0 so we can zero the variables
    
    .BEL1
    
     STA COMC,X             \ Zero the X-th configuration variable
    
     DEX                    \ Decrement the loop counter
    
     BPL BEL1               \ Loop back to BEL1 to zero the next byte, until we have
                            \ zeroed them all
    
     LDA XX21+SST*2-2       \ Set spasto(1 0) to the Coriolis space station entry
     STA spasto             \ from the ship blueprint lookup table at XX21 (so
     LDA XX21+SST*2-1       \ spasto(1 0) points to the Coriolis blueprint)
     STA spasto+1
    
     JSR JAMESON            \ Call JAMESON to set the last saved commander to the
                            \ default "JAMESON" commander
    
                            \ Fall through into TT170 to start the game
    
    
    
    
           Name: TT170                                                   [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Main entry point for the Elite game code
      Deep dive: [Program flow of the main game loop](https://elite.bbcelite.com/deep_dives/program_flow_of_the_main_game_loop.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt170.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt170.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This is the main entry point for the main game code.
    
    
    
    
    .TT170
    
     LDX #&FF               \ Set the stack pointer to &01FF, which is the standard
     TXS                    \ location for the 6502 stack, so this instruction
                            \ effectively resets the stack
    
     JSR RESET              \ Call RESET to initialise most of the game variables
    
                            \ Fall through into DEATH2 to start the game
    
    
    
    
           Name: DEATH2                                                  [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Reset most of the game and restart from the title screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/death2.md)
     Variations: See [code variations](../related/compare/main/subroutine/death2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](../main/subroutine/death.md) calls DEATH2
                 * [DK4](../main/subroutine/dk4.md) calls DEATH2
    
    
    
    
    
    * * *
    
    
     This routine is called following death, and when the game is quit by pressing
     ESCAPE when paused.
    
    
    
    
    .DEATH2
    
     LDX #&FF               \ Set the stack pointer to &01FF, which is the standard
     TXS                    \ location for the 6502 stack, so this instruction
                            \ effectively resets the stack
    
     JSR RES2               \ Reset a number of flight variables and workspaces
                            \ and fall through into the entry code for the game
                            \ to restart from the title screen
    
    
    
    
           Name: BR1 (Part 1 of 2)                                       [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Show the "Load New Commander (Y/N)?" screen and start the game
    
    
        Context: See this subroutine [on its own page](../main/subroutine/br1_part_1_of_2.md)
     Variations: See [code variations](../related/compare/main/subroutine/br1_part_1_of_2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls via QU5
    
    
    
    
    
    * * *
    
    
     BRKV is set to point to BR1 by the loading process.
    
    
    
    * * *
    
    
     Other entry points:
    
       QU5                  Restart the game using the last saved commander without
                            asking whether to load a new commander file
    
    
    
    
    .BR1
    
     JSR ZEKTRAN            \ Call ZEKTRAN to clear the key logger
    
     LDA #3                 \ Set XC = 3 (set text cursor to column 3)
     STA XC
    
    \JSR startat            \ This instruction is commented out in the original
                            \ source
    
     LDX #CYL               \ Call TITLE to show a rotating Cobra Mk III (#CYL) and
     LDA #6                 \ token 6 ("LOAD NEW {single cap}COMMANDER {all caps}
     LDY #200               \ (Y/N)?{sentence case}{cr}{cr}"), with the ship at a
     JSR TITLE              \ distance of 200, returning with the internal number
                            \ of the key pressed in A
    
     CPX #'Y'               \ Did we press "Y"? If not, jump to QU5, otherwise
     BNE QU5                \ continue on to load a new commander
    
    \JSR stopat             \ This instruction is commented out in the original
                            \ source
    
     JSR DFAULT             \ Call DFAULT to reset the current commander data block
                            \ to the last saved commander
    
     JSR SVE                \ Call SVE to load a new commander into the last saved
                            \ commander data block
    
    \JSR startat            \ This instruction is commented out in the original
                            \ source
    
    .QU5
    
     JSR DFAULT             \ Call DFAULT to reset the current commander data block
                            \ to the last saved commander
    
    
    
    
           Name: BR1 (Part 2 of 2)                                       [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Show the "Press Fire or Space, Commander" screen and start the
                 game
    
    
        Context: See this subroutine [on its own page](../main/subroutine/br1_part_2_of_2.md)
     Variations: See [code variations](../related/compare/main/subroutine/br1_part_2_of_2.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     BRKV is set to point to BR1 by the loading process.
    
    
    
    
     JSR msblob             \ Reset the dashboard's missile indicators so none of
                            \ them are targeted
    
     LDA #7                 \ Call TITLE to show a rotating Cougar (#COU) and token
     LDX #COU               \ 7 ("PRESS SPACE OR FIRE,{single cap}COMMANDER.{cr}
     LDY #100               \ {cr}"), with the ship at a distance of 100, returning
     JSR TITLE              \ with the internal number of the key pressed in A
    
     JSR ping               \ Set the target system coordinates (QQ9, QQ10) to the
                            \ current system coordinates (QQ0, QQ1) we just loaded
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     JSR jmp                \ Set the current system to the selected system
    
     LDX #5                 \ We now want to copy the seeds for the selected system
                            \ in QQ15 into QQ2, where we store the seeds for the
                            \ current system, so set up a counter in X for copying
                            \ 6 bytes (for three 16-bit seeds)
    
                            \ The label below is called likeTT112 because this code
                            \ is almost identical to the TT112 loop in the hyp1
                            \ routine
    
    .likeTT112
    
     LDA QQ15,X             \ Copy the X-th byte in QQ15 to the X-th byte in QQ2
     STA QQ2,X
    
     DEX                    \ Decrement the counter
    
     BPL likeTT112          \ Loop back to likeTT112 if we still have more bytes to
                            \ copy
    
     INX                    \ Set X = 0 (as we ended the above loop with X = &FF)
    
     STX EV                 \ Set EV, the extra vessels spawning counter, to 0, as
                            \ we are entering a new system with no extra vessels
                            \ spawned
    
     LDA QQ3                \ Set the current system's economy in QQ28 to the
     STA QQ28               \ selected system's economy from QQ3
    
     LDA QQ5                \ Set the current system's tech level in tek to the
     STA tek                \ selected system's economy from QQ5
    
     LDA QQ4                \ Set the current system's government in gov to the
     STA gov                \ selected system's government from QQ4
    
                            \ Fall through into the docking bay routine below
    
    
    
    
           Name: BAY                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Go to the docking bay (i.e. show the Status Mode screen)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/bay.md)
     Variations: See [code variations](../related/compare/main/subroutine/bay.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRP](../main/subroutine/brp.md) calls BAY
                 * [DOENTRY](../main/subroutine/doentry.md) calls BAY
                 * [EQSHP](../main/subroutine/eqshp.md) calls BAY
                 * [TT102](../main/subroutine/tt102.md) calls BAY
    
    
    
    
    
    * * *
    
    
     We end up here after the start-up process (load commander etc.), as well as
     after a successful save, an escape pod launch, a successful docking, the end
     of a cargo sell, and various errors (such as not having enough cash, entering
     too many items when buying, trying to fit an item to your ship when you
     already have it, running out of cargo space, and so on).
    
    
    
    
    .BAY
    
     LDA #&FF               \ Set QQ12 = &FF (the docked flag) to indicate that we
     STA QQ12               \ are docked
    
     LDA #f8                \ Jump into the main loop at FRCE, setting the key
     JMP FRCE               \ that's "pressed" to red key f8 (so we show the Status
                            \ Mode screen)
    
    
    
    
           Name: DFAULT                                                  [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Reset the current commander data block to the last saved commander
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dfault.md)
     Variations: See [code variations](../related/compare/main/subroutine/dfault-qu5.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md) calls DFAULT
                 * [SVE](../main/subroutine/sve.md) calls DFAULT
    
    
    
    
    
    
    .DFAULT
    
     LDX #NT%+8             \ The size of the last saved commander data block is NT%
                            \ bytes, and it is preceded by the 8 bytes of the
                            \ commander name (seven characters plus a carriage
                            \ return). The commander data block at NAME is followed
                            \ by the commander data block, so we need to copy the
                            \ name and data from the "last saved" buffer at NA% to
                            \ the current commander workspace at NAME. So we set up
                            \ a counter in X for the NT% + 8 bytes that we want to
                            \ copy
    
    .QUL1
    
     LDA NA%-1,X            \ Copy the X-th byte of NA%-1 to the X-th byte of
     STA NAME-1,X           \ NAME-1 (the -1 is because X is counting down from
                            \ NT% + 8 to 1)
    
     DEX                    \ Decrement the loop counter
    
     BNE QUL1               \ Loop back for the next byte of the commander data
                            \ block
    
     STX QQ11               \ X is 0 by the end of the above loop, so this sets QQ11
                            \ to 0, which means we will be showing a view without a
                            \ boxed title at the top (i.e. we're going to use the
                            \ screen layout of a space view in the following)
    
                            \ If the commander check below fails, we keep jumping
                            \ back to here to crash the game with an infinite loop
    
    .doitagain
    
     JSR CHECK              \ Call the CHECK subroutine to calculate the checksum
                            \ for the current commander block at NA%+8 and put it
                            \ in A
    
     CMP CHK                \ Test the calculated checksum against CHK
    
    IF _REMOVE_CHECKSUMS
    
     NOP                    \ If we have disabled checksums, then ignore the result
     NOP                    \ of the comparison and fall through into the next part
    
    ELSE
    
     BNE doitagain          \ If the calculated checksum does not match CHK, then
                            \ loop back to repeat the check - in other words, we
                            \ enter an infinite loop here, as the checksum routine
                            \ will keep returning the same incorrect value
    
    ENDIF
    
                            \ The checksum CHK is correct, so now we check whether
                            \ CHK2 = CHK EOR A9, and if this check fails, bit 7 of
                            \ the competition flags at COK gets set, to indicate
                            \ to Acornsoft via the competition code that there has
                            \ been some hacking going on with this competition entry
    
     EOR #&A9               \ X = checksum EOR &A9
     TAX
    
     LDA COK                \ Set A to the competition flags in COK
    
     CPX CHK2               \ If X = CHK2, then skip the next instruction
     BEQ tZ
    
     ORA #%10000000         \ Set bit 7 of A to indicate this commander file has
                            \ been tampered with
    
    .tZ
    
     ORA #%00001000         \ Set bit 3 of A to denote that this is the Master
                            \ version (which is the same flag as the Apple II
                            \ version)
    
     STA COK                \ Store the updated competition flags in COK
    
    \JSR CHECK2             \ These instructions are commented out in the original
    \                       \ source
    \CMP CHK3
    \BNE doitagain
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TITLE                                                   [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Display a title screen with a rotating ship and prompt
    
    
        Context: See this subroutine [on its own page](../main/subroutine/title.md)
     Variations: See [code variations](../related/compare/main/subroutine/title.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md) calls TITLE
                 * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md) calls TITLE
    
    
    
    
    
    * * *
    
    
     Display the title screen, with a rotating ship and a text token at the bottom
     of the screen.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The number of the extended token to show below the
                            rotating ship (see variable TKN1 for details of
                            recursive tokens)
    
       X                    The type of the ship to show (see variable XX21 for a
                            list of ship types)
    
       Y                    The distance to show the ship rotating, once it has
                            finished moving towards us
    
    
    
    * * *
    
    
     Returns:
    
       X                    If a key is being pressed, X contains the ASCII code
                            of the key pressed
    
    
    
    
    .TITLE
    
     STY distaway           \ Store the ship distance in distaway
    
     PHA                    \ Store the token number on the stack for later
    
     STX TYPE               \ Store the ship type in location TYPE
    
     JSR RESET              \ Reset our ship so we can use it for the rotating
                            \ title ship
    
     JSR ZEKTRAN            \ Call ZEKTRAN to clear the key logger
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     LDA #32                \ Set the mode 1 palette to yellow (colour 1), white
     JSR DOVDU19            \ (colour 2) and cyan (colour 3)
    
     LDA #13                \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 13 (rotating
                            \ ship view)
    
     LDA #RED               \ Switch to colour 2, which is white in the title screen
     STA COL
    
     LDA #0                 \ Set QQ11 to 0, so from here on we are using a space
     STA QQ11               \ view
    
     LDA #96                \ Set nosev_z hi = 96 (96 is the value of unity in the
     STA INWK+14            \ rotation vector)
    
     LDA #96                \ Set A = 96 as the distance that the ship starts at
    
     STA INWK+7             \ Set z_hi, the high byte of the ship's z-coordinate,
                            \ to 96, which is the distance at which the rotating
                            \ ship starts out before coming towards us
    
     LDX #127               \ Set roll counter = 127, so don't dampen the roll and
     STX INWK+29            \ make the roll direction clockwise
    
     STX INWK+30            \ Set pitch counter = 127, so don't dampen the pitch and
                            \ set the pitch direction to dive
    
     INX                    \ Set QQ17 to 128 (so bit 7 is set) to switch to
     STX QQ17               \ Sentence Case, with the next letter printing in upper
                            \ case
    
     LDA TYPE               \ Set up a new ship, using the ship type in TYPE
     JSR NWSHP
    
     LDA #6                 \ Move the text cursor to column 6
     STA XC
    
     LDA #30                \ Print recursive token 144 ("---- E L I T E ----")
     JSR plf                \ followed by a newline
    
     LDA #10                \ Print a line feed to move the text cursor down a line
     JSR TT26
    
     LDA #6                 \ Move the text cursor to column 6 again
     STA XC
    
     LDA PATG               \ If PATG = 0, skip the following two lines, which
     BEQ awe                \ print the author credits (PATG can be toggled by
                            \ pausing the game and pressing "X")
    
     LDA #13                \ Print extended token 13 ("BY D.BRABEN & I.BELL")
     JSR DETOK
    
    .awe
    
     LDY #0                 \ Set DELTA = 0 (i.e. ship speed = 0)
     STY DELTA
    
     STY JSTK               \ Set JSTK = 0 (i.e. keyboard, not joystick)
    
     LDA #20                \ Move the text cursor to row 20
     STA YC
    
     LDA #1                 \ Move the text cursor to column 1
     STA XC
    
     PLA                    \ Restore the recursive token number we stored on the
                            \ stack at the start of this subroutine
    
     JSR DETOK              \ Print the extended token in A
    
     LDA #7                 \ Move the text cursor to column 7
     STA XC
    
     LDA #12                \ Print extended token 12 ("({single cap}C) ACORNSOFT
     JSR DETOK              \ 1986")
    
     LDA #12                \ Set CNT2 = 12 as the outer loop counter for the loop
     STA CNT2               \ starting at TLL2
    
     LDA #5                 \ Set the main loop counter in MCNT to 5, to act as the
     STA MCNT               \ inner loop counter for the loop starting at TLL2
    
     STZ JSTK               \ Set JSTK = 0 (i.e. keyboard, not joystick)
    
    .TLL2
    
     LDA INWK+7             \ If z_hi (the ship's distance) is 1, jump to TL1 to
     CMP #1                 \ skip the following decrement
     BEQ TL1
    
     DEC INWK+7             \ Decrement the ship's distance, to bring the ship
                            \ a bit closer to us
    
    .TL1
    
     JSR MVEIT              \ Move the ship in space according to the orientation
                            \ vectors and the new value in z_hi
    
     LDX distaway           \ Set z_lo to the distance value we passed to the
     STX INWK+6             \ routine, so this is the closest the ship gets to us
    
     LDA #0                 \ Set x_lo = 0, so the ship remains in the screen centre
     STA INWK
    
     STA INWK+3             \ Set y_lo = 0, so the ship remains in the screen centre
    
     JSR LL9                \ Call LL9 to display the ship
    
     DEC MCNT               \ Decrement the main loop counter
    
    IF _SNG47
    
     LDA VIA+&40            \ Read 6522 System VIA input register IRB (SHEILA &40)
    
     AND #%00010000         \ Bit 4 of IRB (PB4) is clear if joystick 1's fire
                            \ button is pressed, otherwise it is set, so AND'ing
                            \ the value of IRB with %10000 extracts this bit
    
    ELIF _COMPACT
    
     JSR RDFIRE             \ Call RDFIRE to check whether the joystick's fire
                            \ button is being pressed
    
     BCS TL3                \ If the C flag is set then the joystick fire button
                            \ is being pressed, so jump to TL3
    
    ENDIF
    
    IF _SNG47
    
     BEQ TL3                \ If the joystick fire button is pressed, jump to TL3
    
    ENDIF
    
     JSR RDKEY              \ Scan the keyboard for a key press and return the ASCII
                            \ code of the key pressed in A and X (or 0 for no key
                            \ press)
    
     BEQ TLL2               \ If no key was pressed, loop back up to move/rotate
                            \ the ship and check again for a key press
    
     RTS                    \ Return from the subroutine
    
    .TL3
    
     DEC JSTK               \ Joystick fire button was pressed, so set JSTK to &FF
                            \ (it was set to 0 above), to disable keyboard and
                            \ enable joysticks
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: CHECK                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Calculate the checksum for the last saved commander data block
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/check.md)
     Variations: See [code variations](../related/compare/main/subroutine/check.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DFAULT](../main/subroutine/dfault.md) calls CHECK
                 * [SVE](../main/subroutine/sve.md) calls CHECK
    
    
    
    
    
    * * *
    
    
     The checksum for the last saved commander data block is saved as part of the
     commander file, in two places (CHK AND CHK2), to protect against file
     tampering. This routine calculates the checksum and returns it in A.
    
     This algorithm is also implemented in elite-checksum.py.
    
    
    
    * * *
    
    
     Returns:
    
       A                    The checksum for the last saved commander data block
    
    
    
    
    .CHECK
    
     LDX #NT%-3             \ Set X to the size of the commander data block, less
                            \ 3 (as there are two checksum bytes and the save count)
    
     CLC                    \ Clear the C flag so we can do addition without the
                            \ C flag affecting the result
    
     TXA                    \ Seed the checksum calculation by setting A to the
                            \ size of the commander data block, less 2
    
                            \ We now loop through the commander data block,
                            \ starting at the end and looping down to the start
                            \ (so at the start of this loop, the X-th byte is the
                            \ last byte of the commander data block, i.e. the save
                            \ count)
    
    .QUL2
    
     ADC NA%+7,X            \ Add the X-1-th byte of the data block to A, plus the
                            \ C flag
    
     EOR NA%+8,X            \ EOR A with the X-th byte of the data block
    
     DEX                    \ Decrement the loop counter
    
     BNE QUL2               \ Loop back for the next byte in the calculation, until
                            \ we have added byte #0 and EOR'd with byte #1 of the
                            \ data block
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: CHECK2                                                  [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Calculate the third checksum for the last saved commander data
                 block (Commodore 64 and Apple II versions only)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/check2.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .CHECK2
    
    \LDX #NT%-3             \ These instructions are commented out in the original
    \                       \ source (they are from the Commodore 64 and Apple II
    \CLC                    \ versions, and implement the third commander checksum
    \                       \ which the Master version doesn't have)
    \TXA
    \
    \.QU2L2
    \
    \STX T
    \EOR T
    \ROR A
    \
    \ADC NA%+7,X
    \
    \EOR NA%+8,X
    \
    \DEX
    \
    \BNE QU2L2
    \
    \RTS
    
    
    
    
           Name: JAMESON                                                 [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Restore the default JAMESON commander
    
    
        Context: See this subroutine [on its own page](../main/subroutine/jameson.md)
     References: This subroutine is called as follows:
                 * [BEGIN](../main/subroutine/begin.md) calls JAMESON
                 * [SVE](../main/subroutine/sve.md) calls JAMESON
    
    
    
    
    
    
    .JAMESON
    
     LDY #(NAEND%-NA2%)     \ We are going to copy the default commander at NA2%
                            \ over the top of the last saved commander at NA%, so
                            \ set a counter to copy all the bytes between NA2% and
                            \ NAEND%
    
    .JAMEL1
    
     LDA NA2%,Y             \ Copy the Y-th byte of NA2% to the Y-th byte of NA%
     STA NA%,Y
    
     DEY                    \ Decrement the loop counter
    
     BPL JAMEL1             \ Loop back until we have copied the whole commander
    
     LDY #7                 \ Set oldlong to 7, the length of the commander name
     STY oldlong            \ "JAMESON"
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TRNME                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Copy the last saved commander's name from INWK to NA%
    
    
        Context: See this subroutine [on its own page](../main/subroutine/trnme.md)
     Variations: See [code variations](../related/compare/main/subroutine/trnme.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SVE](../main/subroutine/sve.md) calls TRNME
    
    
    
    
    
    
    .TRNME
    
     LDX #7                 \ The commander's name can contain a maximum of 7
                            \ characters, and is terminated by a carriage return,
                            \ so set up a counter in X to copy 8 characters
    
     LDA thislong           \ Copy the length of the commander's name from thislong
     STA oldlong            \ to oldlong (though this is never used, so this
                            \ doesn't have any effect)
    
    .GTL1
    
     LDA INWK+5,X           \ Copy the X-th byte of INWK+5 to the X-th byte of NA%
     STA NA%,X
    
     DEX                    \ Decrement the loop counter
    
     BPL GTL1               \ Loop back until we have copied all 8 bytes
    
                            \ Fall through into TR1 to copy the name back from NA%
                            \ to INWK. This isn't necessary as the name is already
                            \ there, but it does save one byte, as we don't need an
                            \ RTS here
    
    
    
    
           Name: TR1                                                     [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Copy the last saved commander's name from NA% to INWK
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tr1.md)
     Variations: See [code variations](../related/compare/main/subroutine/tr1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [GTNMEW](../main/subroutine/gtnmew.md) calls TR1
    
    
    
    
    
    
    .TR1
    
     LDX #7                 \ The commander's name can contain a maximum of 7
                            \ characters, and is terminated by a carriage return,
                            \ so set up a counter in X to copy 8 characters
    
    .GTL2
    
     LDA NA%,X              \ Copy the X-th byte of NA% to the X-th byte of INWK+5
     STA INWK+5,X
    
     DEX                    \ Decrement the loop counter
    
     BPL GTL2               \ Loop back until we have copied all 8 bytes
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: GTNMEW                                                  [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Fetch the name of a commander file to save or load
    
    
        Context: See this subroutine [on its own page](../main/subroutine/gtnmew.md)
     Variations: See [code variations](../related/compare/main/subroutine/gtnme-gtnmew.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SVE](../main/subroutine/sve.md) calls GTNMEW
    
    
    
    
    
    * * *
    
    
     Get the commander's name for loading or saving a commander file. The name is
     stored in the INWK workspace and is terminated by a return character (13).
    
     If ESCAPE is pressed or a blank name is entered, then the name stored is set
     to the name from the last saved commander block.
    
    
    
    * * *
    
    
     Returns:
    
       INWK                 The full filename, including drive and directory, in
                            the form ":0.E.JAMESON", for example, terminated by a
                            return character (13)
    
    
    
    
    .GTNMEW
    
    .GTNME
    
     LDX #4                 \ First we want to copy the drive and directory part of
                            \ the commander file from NA%-5, so set a counter in X
                            \ for 5 bytes, as the string is of the form ":0.E."
    
    .GTL3
    
     LDA NA%-5,X            \ Copy the X-th byte from NA%-5 to INWK
     STA INWK,X
    
     DEX                    \ Decrement the loop counter
    
     BPL GTL3               \ Loop back until the whole drive and directory string
                            \ has been copied to INWK to INWK+4
    
     LDA #7                 \ The call to MT26 below uses the OSWORD block at RLINE
     STA RLINE+2            \ to fetch the line, and RLINE+2 defines the maximum
                            \ line length allowed, so this changes the maximum
                            \ length to 7 (as that's the longest commander name
                            \ allowed)
    
     LDA #8                 \ Print extended token 8 ("{single cap}COMMANDER'S
     JSR DETOK              \ NAME? ")
    
     JSR MT26               \ Call MT26 to fetch a line of text from the keyboard
                            \ to INWK+5, with the text length in Y, so INWK now
                            \ contains the full pathname of the file, as in
                            \ ":0.E.JAMESON", for example
    
     LDA #9                 \ Reset the maximum length in RLINE+2 to the original
     STA RLINE+2            \ value of 9
    
     TYA                    \ The OSWORD call returns the length of the commander's
                            \ name in Y, so transfer this to A
    
     BEQ TR1                \ If A = 0, no name was entered, so jump to TR1 to copy
                            \ the last saved commander's name from NA% to INWK
                            \ and return from the subroutine there
    
     STY thislong           \ Store the length of the length of the commander's that
                            \ was entered in thislong
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: MT26                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Fetch a line of text from the keyboard
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mt26.md)
     Variations: See [code variations](../related/compare/main/subroutine/mt26.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DELT](../main/subroutine/delt.md) calls MT26
                 * [GTDIR](../main/subroutine/gtdir.md) calls MT26
                 * [GTNMEW](../main/subroutine/gtnmew.md) calls MT26
                 * [JMTB](../main/variable/jmtb.md) calls MT26
    
    
    
    
    
    * * *
    
    
     Returns:
    
       Y                    The size of the entered text, or 0 if none was entered
    
       INWK+5               The entered text, terminated by a carriage return
    
       C flag               Set if ESCAPE was pressed
    
    
    
    
    .MT26
    
     LDA COL                \ Store the current colour on the stack
     PHA
    
     LDA #RED               \ Switch to colour 2, which is magenta in the trade view
     STA COL
    
     LDY #8                 \ Wait for 8/50 of a second (0.16 seconds)
     JSR DELAY
    
     JSR FLKB               \ Call FLKB to flush the keyboard buffer
    
     LDY #0                 \ Set Y = 0 to hold the length of the text entered
    
    .OSW0L
    
     JSR TT217              \ Scan the keyboard until a key is pressed, and return
                            \ the key's ASCII code in A (and X)
    
     CMP #13                \ If RETURN was pressed, jump to OSW03
     BEQ OSW03
    
     CMP #27                \ If ESCAPE was pressed, jump to OSW04
     BEQ OSW04
    
     CMP #127               \ If DELETE was pressed, jump to OSW05
     BEQ OSW05
    
    IF _COMPACT
    
     PHX                    \ Store X on the stack so we can retrieve it after the
                            \ call to SHIFT
    
     PHA                    \ Store the number of the key being pressed on the stack
    
     JSR SHIFT              \ If SHIFT is not being pressed, jump to noshift to skip
     BPL noshift            \ the following
    
     PLA                    \ SHIFT is being pressed, so fetch the number of the key
                            \ being pressed from the stack
    
     CMP #'@'               \ If A >= ASCII "@", then we are pressing a letter key,
     BCS P%+4               \ so skip the following instruction
    
     EOR #%00010000         \ We are pressing SHIFT and a number key, so flip bit 4
                            \ of the key number, which flips the letter between the
                            \ ASCII code of the number being pressed and the ASCII
                            \ code of the number being pressed when SHIFT is being
                            \ held down (so SHIFT-1 will enter !, SHIFT-2 will enter
                            \ ", and so on)
    
     PHA                    \ Push the updated key number onto the stack
    
    .noshift
    
     PLA                    \ Retrieve the values of X and A we stored on the stack
     PLX                    \ above
    
    ENDIF
    
     CPY RLINE+2            \ If Y >= RLINE+2 (the maximum line length from the
     BCS OSW01              \ OSWORD configuration block at RLINE), then jump to
                            \ OSW01 to give an error beep as we have reached the
                            \ character limit
    
     CMP RLINE+3            \ If the key pressed is less than the character in
     BCC OSW01              \ RLINE+3 (the lowest allowed character from the OSWORD
                            \ configuration block at RLINE), then jump to OSW01
                            \ to give an error beep as the key pressed is out of
                            \ range
    
     CMP RLINE+4            \ If the key pressed is greater than or equal to the
     BCS OSW01              \ character in RLINE+4 (the highest allowed character
                            \ from the OSWORD configuration block at RLINE), then
                            \ jump to OSW01 to give an error beep as the key
                            \ pressed is out of range
    
     STA INWK+5,Y           \ Store the key's ASCII code in the Y-th byte of INWK+5
    
     INY                    \ Increment Y to point to the next free byte in INWK+5
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &07, or BIT &07A9, which does nothing apart
                            \ from affect the flags
    
    .OSW01
    
     LDA #7                 \ Set A to the beep character, so the next instruction
                            \ makes a system beep
    
    .OSW06
    
     JSR CHPR               \ Print the character in A (and clear the C flag)
    
     BCC OSW0L              \ Loop back to OSW0L to fetch another key press (this
                            \ BCC is effectively a JMP as CHPR clears the C flag)
    
    .OSW03
    
     STA INWK+5,Y           \ Store the return character in the Y-th byte of INWK+5
    
     LDA #12                \ Print a newline
     JSR CHPR
    
     EQUB &24               \ Skip the next instruction by turning it into &24 &38,
                            \ or BIT &0038, which does nothing apart from affect the
                            \ flags
    
    .OSW04
    
     SEC                    \ Set the C flag as ESCAPE was pressed
    
     PLA                    \ Restore the original colour from the stack and set it
     STA COL                \ as the current colour
    
     RTS                    \ Return from the subroutine
    
    .OSW05
    
     TYA                    \ If the length of the line so far in Y is 0, then we
     BEQ OSW01              \ just pressed DELETE on an empty line, so jump to
                            \ OSW01 give an error beep
    
     DEY                    \ Otherwise we want to delete a character, so decrement
                            \ the length of the line so far in Y
    
     LDA #127               \ Set A = 127 and jump back to OSW06 to print the
     BNE OSW06              \ character in A (i.e. the DELETE character) and listen
                            \ for the next key press
    
    
    
    
           Name: RLINE                                                   [Show more]
           Type: Variable
       Category: Text
        Summary: The OSWORD configuration block used to fetch a line of text from
                 the keyboard
    
    
        Context: See this variable [on its own page](../main/variable/rline.md)
     Variations: See [code variations](../related/compare/main/variable/rline.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [GTDIR](../main/subroutine/gtdir.md) uses RLINE
                 * [GTNMEW](../main/subroutine/gtnmew.md) uses RLINE
                 * [MT26](../main/subroutine/mt26.md) uses RLINE
    
    
    
    
    
    
    .RLINE
    
     EQUW INWK+5            \ The address to store the input, so the text entered
                            \ will be stored in INWK+5 as it is typed
    
     EQUB 9                 \ Maximum line length = 9, as that's the maximum size
                            \ for a commander's name including a directory name
    
     EQUB '!'               \ Allow ASCII characters from "!" through to "{" in
     EQUB '{'               \ the input
    
    
    
    
           Name: FILEPR                                                  [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Display the currently selected media (disk or tape)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/filepr.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls FILEPR
    
    
    
    
    
    
    .FILEPR
    
     LDA #3                 \ Print extended token 3 + DISK, i.e. token 3 or 2 (as
     CLC                    \ DISK can be 0 or &FF). In other versions of the game,
     ADC DISK               \ such as the Commodore 64 version, token 2 is "disk"
     JMP DETOK              \ and token 3 is "tape", so this displays the currently
                            \ selected media, but this system is unused in the
                            \ Master version and tokens 2 and 3 contain different
                            \ text
    
    
    
    
           Name: OTHERFILEPR                                             [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Display the non-selected media (disk or tape)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/otherfilepr.md)
     References: This subroutine is called as follows:
                 * [JMTB](../main/variable/jmtb.md) calls OTHERFILEPR
    
    
    
    
    
    
    .OTHERFILEPR
    
     LDA #2                 \ Print extended token 2 - DISK, i.e. token 2 or 3 (as
     SEC                    \ DISK can be 0 or &FF). In other versions of the game,
     SBC DISK               \ such as the Commodore 64 version, token 2 is "disk"
     JMP DETOK              \ and token 3 is "tape", so this displays the other,
                            \ non-selected media, but this system is unused in the
                            \ Master version and tokens 2 and 3 contain different
                            \ text
    
    
    
    
           Name: ZERO                                                    [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Reset the local bubble of universe and ship status
    
    
        Context: See this subroutine [on its own page](../main/subroutine/zero.md)
     Variations: See [code variations](../related/compare/main/subroutine/zero.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [RES2](../main/subroutine/res2.md) calls ZERO
                 * [RESET](../main/subroutine/reset.md) calls ZERO
    
    
    
    
    
    * * *
    
    
     This resets the following workspaces to zero:
    
       * UP workspace variables from FRIN to de, which include the ship slots for
         the local bubble of universe, and various flight and ship status variables
    
    
    
    
    .ZERO
    
     LDX #(de-FRIN)         \ We're going to zero the UP workspace variables from
                            \ FRIN to de, so set a counter in X for the correct
                            \ number of bytes
    
     LDA #0                 \ Set A = 0 so we can zero the variables
    
    .ZEL2
    
     STA FRIN,X             \ Zero the X-th byte of FRIN to de
    
     DEX                    \ Decrement the loop counter
    
     BPL ZEL2               \ Loop back to zero the next variable until we have done
                            \ them all
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: GTDIR                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Fetch the name of an ADFS directory on the Master Compact and
                 change to that directory
    
    
        Context: See this subroutine [on its own page](../main/subroutine/gtdir.md)
     References: This subroutine is called as follows:
                 * [CATS](../main/subroutine/cats.md) calls GTDIR
    
    
    
    
    
    
    IF _COMPACT
    
    .GTDIR
    
     LDA #2                 \ Print extended token 2 ("{cr}WHICH DRIVE?")
     JSR DETOK
    
     LDA #19                \ The call to MT26 below uses the OSWORD block at RLINE
     STA RLINE+2            \ to fetch the line, and RLINE+2 defines the maximum
                            \ line length allowed, so this changes the maximum
                            \ length to 19 (as that's the longest directory name
                            \ allowed)
    
     JSR MT26               \ Call MT26 to fetch a line of text from the keyboard
                            \ to INWK+5, with the text length in Y, so INWK now
                            \ contains the full directory name
    
     LDA #9                 \ Reset the maximum length in RLINE+2 to the original
     STA RLINE+2            \ value of 9
    
     TYA                    \ The OSWORD call returns the length of the commander's
                            \ name in Y, so transfer this to A
    
     BEQ GTDIR-1            \ If A = 0, no name was entered, so jump to DIR-1 to
                            \ return from the subroutine (as GTDIR-1 contains an
                            \ RTS)
    
                            \ We now copy the entered filename from INWK to DIRI, so
                            \ that it overwrites the filename part of the string,
                            \ i.e. the "12345678901234567890" part of
                            \ "DIR 12345678901234567890"
    
     LDX #18                \ Set up a counter in X to count from 18 to 0, so that
                            \ we copy the string starting at INWK+5 to DIRI+4
                            \ onwards (i.e. "12345678901234567890")
    
    .DIRL
    
     LDA INWK+5,X           \ Copy the X-th byte of INWK+5 to the X-th byte of
     STA DIRI+4,X           \ DIRI+4
    
     DEX                    \ Decrement the loop counter
    
     BPL DIRL               \ Loop back to DIRL to copy the next character until we
                            \ have copied the whole filename
    
     JSR NMIRELEASE         \ Release the NMI workspace (&00A0 to &00A7) so the MOS
                            \ can use it, and store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
     LDX #LO(DIRI)          \ Set (Y X) to point to DIRI ("DIR <name entered>")
     LDY #HI(DIRI)
    
     JSR OSCLI              \ Call OSCLI to run the OS command in DIRI, which
                            \ changes the disc directory to the name entered
    
     JMP getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000 and return from the subroutine
                            \ using a tail call
    
    ENDIF
    
    
    
    
           Name: CATS                                                    [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Ask for a disc drive number and print a catalogue of that drive
    
    
        Context: See this subroutine [on its own page](../main/subroutine/cats.md)
     Variations: See [code variations](../related/compare/main/subroutine/cats.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DELT](../main/subroutine/delt.md) calls CATS
                 * [SVE](../main/subroutine/sve.md) calls CATS
    
    
    
    
    
    * * *
    
    
     This routine asks for a disc drive number, and if it is a valid number (0-3)
     it displays a catalogue of the disc in that drive. It also updates the OS
     command at CTLI so that when that command is run, it catalogues the correct
     drive.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Clear if a valid drive number was entered (0-3), set
                            otherwise
    
    
    
    
    .CATS
    
    IF _SNG47
    
     JSR GTDRV              \ Get an ASCII disc drive number from the keyboard in A,
                            \ setting the C flag if an invalid drive number was
                            \ entered
    
     BCS DELT-1             \ If the C flag is set, then an invalid drive number was
                            \ entered, so return from the subroutine (as DELT-1
                            \ contains an RTS)
    
     STA CTLI+4             \ Store the drive number in the fifth byte of the
                            \ command string at CTLI, so it overwrites the "1" in
                            \ "CAT 1" with the drive number to catalogue
    
     STA DTW7               \ Store the drive number in DTW7, so printing extended
                            \ token 4 will show the correct drive number (as token 4
                            \ contains the {drive number} jump code, which calls
                            \ MT16 to print the character in DTW7)
    
    ELIF _COMPACT
    
     JSR GTDIR              \ Get a directory name from the keyboard and change to
                            \ that directory
    
    ENDIF
    
     LDA #3                 \ Print extended token 3, which clears the screen and
     JSR DETOK              \ prints the boxed-out title "DRIVE {drive number}
                            \ CATALOGUE"
    
     LDA #1                 \ Set the CATF flag to 1, so that the TT26 routine will
     STA CATF               \ print out the disc catalogue correctly
    
     STA XC                 \ Move the text cursor to column 1
    
    IF _SNG47
    
     JSR getzp              \ Call getzp to store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ELIF _COMPACT
    
     JSR NMIRELEASE         \ Release the NMI workspace (&00A0 to &00A7) so the MOS
                            \ can use it, and store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ENDIF
    
     LDX #LO(CTLI)          \ Set (Y X) to point to the OS command at CTLI, which
     LDY #HI(CTLI)          \ contains a dot and the drive number, which is the
                            \ DFS command for cataloguing that drive (*. being short
                            \ for *CAT)
    
     JSR OSCLI              \ Call OSCLI to execute the OS command at (Y X), which
                            \ catalogues the disc
    
     JSR getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000
    
     STZ CATF               \ Set the CATF flag to 0, so the TT26 routine reverts to
                            \ standard formatting
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DELT                                                    [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Catalogue a disc, ask for a filename to delete, and delete the
                 file
    
    
        Context: See this subroutine [on its own page](../main/subroutine/delt.md)
     Variations: See [code variations](../related/compare/main/subroutine/delt.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SVE](../main/subroutine/sve.md) calls DELT
                 * [CATS](../main/subroutine/cats.md) calls via DELT-1
    
    
    
    
    
    * * *
    
    
     This routine asks for a disc drive number, and if it is a valid number (0-3)
     it displays a catalogue of the disc in that drive. It then asks for a filename
     to delete, updates the OS command at DELI so that when that command is run, it
     deletes the correct file, and then it does the deletion.
    
    
    
    * * *
    
    
     Other entry points:
    
       DELT-1               Contains an RTS
    
    
    
    
    .DELT
    
     JSR CATS               \ Call CATS to ask for a drive number (or a directory
                            \ name on the Master Compact) and catalogue that disc
                            \ or directory
    
     BCS SVE                \ If the C flag is set then an invalid drive number was
                            \ entered as part of the catalogue process, so jump to
                            \ SVE to display the disc access menu
    
    IF _SNG47
    
     LDA CTLI+4             \ The call to CATS above put the drive number into
     STA DELI+8             \ CTLI+4, so copy the drive number into DELI+8 so that
                            \ the drive number in the "DELETE :1.1234567" string
                            \ gets updated (i.e. the number after the colon)
    
    ENDIF
    
     LDA #8                 \ Print extended token 8 ("{single cap}COMMANDER'S
     JSR DETOK              \ NAME? ")
    
     JSR MT26               \ Call MT26 to fetch a line of text from the keyboard
                            \ to INWK+5, with the text length in Y
    
     TYA                    \ If no text was entered (Y = 0) then jump to SVE to
     BEQ SVE                \ display the disc access menu
    
    IF _SNG47
    
                            \ We now copy the entered filename from INWK to DELI, so
                            \ that it overwrites the filename part of the string,
                            \ i.e. the "E.1234567" part of "DELETE :1.1234567"
    
     LDX #9                 \ Set up a counter in X to count from 9 to 1, so that we
                            \ copy the string starting at INWK+4+1 (i.e. INWK+5) to
                            \ DELI+9+1 (i.e. DELI+10 onwards, or "1.1234567")
                            \
                            \ Note that this is a bug - X should be set to 8, as a
                            \ value of 9 overwrites the first character of the
                            \ "SAVE" command in savosc
                            \
                            \ This means that if you delete a file, it breaks the
                            \ save command, so you can't save a commander file if
                            \ you have previously deleted a file
    
    ELIF _COMPACT
    
                            \ We now copy the entered filename from INWK to DELI, so
                            \ that it overwrites the filename part of the string,
                            \ i.e. the "1234567890" part of "DELETE 1234567890"
    
     LDX #8                 \ Set up a counter in X to count from 8 to 0, so that we
                            \ copy the string starting at INWK+5+0 (i.e. INWK+5) to
                            \ DELI+7+0 (i.e. DELI+7 onwards, or "1234567890")
    
    ENDIF
    
    .DELL1
    
    IF _SNG47
    
     LDA INWK+4,X           \ Copy the X-th byte of INWK+4 to the X-th byte of
     STA DELI+9,X           \ DELI+9
    
     DEX                    \ Decrement the loop counter
    
     BNE DELL1              \ Loop back to DELL1 to copy the next character until we
                            \ have copied the whole filename
    
     JSR getzp              \ Call getzp to store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ELIF _COMPACT
    
     LDA INWK+5,X           \ Copy the X-th byte of INWK+5 to the X-th byte of
     STA DELI+7,X           \ DELI+7
    
     DEX                    \ Decrement the loop counter
    
     BPL DELL1              \ Loop back to DELL1 to copy the next character until we
                            \ have copied the whole filename
    
     JSR NMIRELEASE         \ Release the NMI workspace (&00A0 to &00A7) so the MOS
                            \ can use it, and store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ENDIF
    
     LDX #LO(DELI)          \ Set (Y X) to point to the OS command at DELI, which
     LDY #HI(DELI)          \ contains the DFS command for deleting this file
    
     JSR OSCLI              \ Call OSCLI to execute the OS command at (Y X), which
                            \ deletes the file
    
     JSR getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000
    
     JMP SVE                \ Jump to SVE to display the disc access menu and return
                            \ from the subroutine using a tail call
    
    
    
    
           Name: SVE                                                     [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Display the disk access menu and process saving of commander files
      Deep dive: [Commander save files](https://elite.bbcelite.com/deep_dives/commander_save_files.html)
                 [The competition code](https://elite.bbcelite.com/deep_dives/the_competition_code.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sve.md)
     Variations: See [code variations](../related/compare/main/subroutine/sve.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md) calls SVE
                 * [DELT](../main/subroutine/delt.md) calls SVE
                 * [LOD](../main/subroutine/lod.md) calls SVE
                 * [NEWBRK](../main/subroutine/newbrk.md) calls SVE
                 * [TT102](../main/subroutine/tt102.md) calls SVE
    
    
    
    
    
    
    .SVE
    
     TSX                    \ Transfer the stack pointer to X and store it in
     STX stackpt            \ stackpt, so we can restore it in the NEWBRK routine
    
     JSR TRADEMODE2         \ Set the palette for trading screens and switch the
                            \ current colour to white
    
     LDA #1                 \ Print extended token 1, the disc access menu, which
     JSR DETOK              \ presents these options:
                            \
                            \   1. Load New Commander
                            \   2. Save Commander {commander name}
                            \   3. Catalogue
                            \   4. Delete A File
                            \   5. Default JAMESON
                            \   6. Exit
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     CMP #'1'               \ Option 1 was chosen, so jump to loading to load a new
     BEQ loading            \ commander
    
     CMP #'2'               \ Option 2 was chosen, so jump to SV1 to save the
     BEQ SV1                \ current commander
    
     CMP #'3'               \ Option 3 was chosen, so jump to feb10 to catalogue a
     BEQ feb10              \ disc
    
     CMP #'4'               \ If option 4 wasn't chosen, skip the next two
     BNE jan18              \ instructions
    
     JSR DELT               \ Option 4 was chosen, so call DELT to delete a file
    
     JMP SVE                \ Jump to SVE to display the disc access menu again and
                            \ return from the subroutine using a tail call
    
    .jan18
    
     CMP #'5'               \ If option 5 wasn't chosen, skip to feb13 to exit the
     BNE feb13              \ menu
    
     LDA #224               \ Print extended token 224 ("ARE YOU SURE?")
     JSR DETOK
    
     JSR YESNO              \ Call YESNO to wait until either "Y" or "N" is pressed
    
     BCC feb13              \ If "N" was pressed, jump to feb13
    
     JSR JAMESON            \ Otherwise "Y" was pressed, so call JAMESON to set the
                            \ last saved commander to the default "JAMESON"
                            \ commander
    
     JMP DFAULT             \ Jump to DFAULT to reset the current commander data
                            \ block to the last saved commander, returning from the
                            \ subroutine using a tail call
    
    .feb13
    
     CLC                    \ Option 5 was chosen, so clear the C flag to indicate
                            \ that nothing was loaded
    
     RTS                    \ Return from the subroutine
    
    .feb10
    
     JSR CATS               \ Call CATS to ask for a drive number (or a directory
                            \ name on the Master Compact) and catalogue that disc
                            \ or directory
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     JMP SVE                \ Jump to SVE to display the disc access menu and return
                            \ from the subroutine using a tail call
    
    .loading
    
     JSR GTNMEW             \ If we get here then option 1 (load) was chosen, so
                            \ call GTNMEW to fetch the name of the commander file
                            \ to load (including drive number and directory) into
                            \ INWK
    
    IF _SNG47
    
     JSR GTDRV              \ Get an ASCII disc drive number from the keyboard in A,
                            \ setting the C flag if an invalid drive number was
                            \ entered
    
     BCS jan2186            \ If the C flag is set, then an invalid drive number was
                            \ entered, so return from the subroutine (as DELT-1
                            \ contains an RTS)
    
     STA lodosc+6           \ Store the ASCII drive number in lodosc+6, which is the
                            \ drive character of the load filename string ":1.E."
    
    ENDIF
    
     JSR LOD                \ Call LOD to load the commander file
    
     JSR TRNME              \ Transfer the commander filename from INWK to NA%
    
     SEC                    \ Set the C flag to indicate we loaded a new commander
    
    .jan2186
    
     RTS                    \ Return from the subroutine
    
    .SV1
    
     JSR GTNMEW             \ If we get here then option 2 (save) was chosen, so
                            \ call GTNMEW to fetch the name of the commander file
                            \ to save (including drive number and directory) into
                            \ INWK
    
     JSR TRNME              \ Transfer the commander filename from INWK to NA%
    
     LSR SVC                \ Halve the save count value in SVC
    
     LDA #4                 \ Print extended token 4, which is blank (this was where
     JSR DETOK              \ the competition number was printed in older versions,
                            \ but the competition was long gone by the time of the
                            \ BBC Master version
    
     LDX #NT%               \ We now want to copy the current commander data block
                            \ from location TP to the last saved commander block at
                            \ NA%+8, so set a counter in X to copy the NT% bytes in
                            \ the commander data block
    
    .SVL1
    
     LDA TP,X               \ Copy the X-th byte of TP to the X-th byte of NA%+8
    \STA &0B00,X            \
     STA NA%+8,X            \ The STA is commented out in the original source
    
     DEX                    \ Decrement the loop counter
    
     BPL SVL1               \ Loop back until we have copied all the bytes in the
                            \ commander data block
    
    \JSR CHECK2             \ These instructions are commented out in the original
    \                       \ source
    \STA CHK3
    
     JSR CHECK              \ Call CHECK to calculate the checksum for the last
                            \ saved commander and return it in A
    
     STA CHK                \ Store the checksum in CHK, which is at the end of the
                            \ last saved commander block
    
     PHA                    \ Store the checksum on the stack
    
     ORA #%10000000         \ Set K = checksum with bit 7 set
     STA K
    
     EOR COK                \ Set K+2 = K EOR COK (the competition flags)
     STA K+2
    
     EOR CASH+2             \ Set K+1 = K+2 EOR CASH+2 (the third cash byte)
     STA K+1
    
     EOR #&5A               \ Set K+3 = K+1 EOR &5A EOR TALLY+1 (the high byte of
     EOR TALLY+1            \ the kill tally)
     STA K+3
    
     CLC                    \ Clear the C flag so the call to BPRNT does not include
                            \ a decimal point
    
     JSR TT67               \ Call TT67 twice to print two newlines
     JSR TT67
    
     PLA                    \ Restore the checksum from the stack
    
     EOR #&A9               \ Store the checksum EOR &A9 in CHK2, the penultimate
     STA CHK2               \ byte of the last saved commander block
    
                            \ We now copy the current commander data block into the
                            \ TAP% staging area, though this has no effect as we
                            \ then ignore the result (this code is left over from
                            \ the Apple II version)
    
     LDY #NT%               \ Set a counter in X to copy the NT% bytes in the
                            \ commander data block
    
    .copyme2
    
     LDA NA%+8,Y            \ Copy the X-th byte of NA% to the X-th byte of TAP%
     STA TAP%,Y
    
     DEY                    \ Decrement the loop counter
    
     BPL copyme2            \ Loop back until we have copied all the bytes in the
                            \ commander data block
    
    IF _SNG47
    
     JSR GTDRV              \ Get an ASCII disc drive number from the keyboard in A,
                            \ setting the C flag if an invalid drive number was
                            \ entered
    
     BCS SVEX9              \ If the C flag is set, then an invalid drive number was
                            \ entered, so skip the next two instructions
    
     STA savosc+6           \ Store the ASCII drive number in savosc+6, which is the
                            \ drive character of the save filename string ":1.E."
    
    ENDIF
    
     JSR wfile              \ Call wfile to save the commander file
    
    .SVEX9
    
     JSR DFAULT             \ Call DFAULT to reset the current commander data
                            \ block to the last saved commander
    
    .SVEX
    
     CLC                    \ Clear the C flag to indicate that no new commander
                            \ file was loaded
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: thislong                                                [Show more]
           Type: Variable
       Category: Save and load
        Summary: Contains the length of the most recently entered commander name
    
    
        Context: See this variable [on its own page](../main/variable/thislong.md)
     References: This variable is used as follows:
                 * [GTNMEW](../main/subroutine/gtnmew.md) uses thislong
                 * [TRNME](../main/subroutine/trnme.md) uses thislong
    
    
    
    
    
    
    .thislong
    
     EQUB 7
    
    
    
    
           Name: oldlong                                                 [Show more]
           Type: Variable
       Category: Save and load
        Summary: Contains the length of the last saved commander name
    
    
        Context: See this variable [on its own page](../main/variable/oldlong.md)
     References: This variable is used as follows:
                 * [JAMESON](../main/subroutine/jameson.md) uses oldlong
                 * [TRNME](../main/subroutine/trnme.md) uses oldlong
    
    
    
    
    
    
    .oldlong
    
     EQUB 7
    
    
    
    
           Name: GTDRV                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Get an ASCII disc drive number from the keyboard
    
    
        Context: See this subroutine [on its own page](../main/subroutine/gtdrv.md)
     References: This subroutine is called as follows:
                 * [CATS](../main/subroutine/cats.md) calls GTDRV
                 * [SVE](../main/subroutine/sve.md) calls GTDRV
    
    
    
    
    
    * * *
    
    
     Returns:
    
       A                    The ASCII value of the entered drive number ("0" to "3")
    
       C flag               Clear if a valid drive number was entered (0-3), set
                            otherwise
    
    
    
    
    .GTDRV
    
     LDA #2                 \ Print extended token 2 ("{cr}WHICH DRIVE?")
     JSR DETOK
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     ORA #%00010000         \ Set bit 4 of A, perhaps to avoid printing any control
                            \ characters in the next instruction
    
     JSR CHPR               \ Print the character in A
    
     PHA                    \ Store A on the stack so we can retrieve it after the
                            \ call to FEED
    
     JSR FEED               \ Print a newline
    
     PLA                    \ Restore A from the stack
    
     CMP #'0'               \ If A < ASCII "0", then it is not a valid drive number,
     BCC LOR                \ so jump to LOR to set the C flag and return from the
                            \ subroutine
    
     CMP #'4'               \ If A >= ASCII "4", then it is not a valid drive
                            \ number, and this CMP sets the C flag, otherwise it is
                            \ a valid drive number in the range 0-3, so clear it
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: LOD                                                     [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Load a commander file
    
    
        Context: See this subroutine [on its own page](../main/subroutine/lod.md)
     Variations: See [code variations](../related/compare/main/subroutine/lod.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SVE](../main/subroutine/sve.md) calls LOD
                 * [GTDRV](../main/subroutine/gtdrv.md) calls via LOR
    
    
    
    
    
    * * *
    
    
     The filename should be stored at INWK, terminated with a carriage return (13).
    
    
    
    * * *
    
    
     Other entry points:
    
       LOR                  Set the C flag and return from the subroutine
    
    
    
    
    .LOD
    
     JSR rfile              \ Call rfile to load the commander file to the TAP%
                            \ staging area
    
     LDA TAP%               \ If the first byte of the loaded file has bit 7 set,
     BMI ELT2F              \ jump to ELT2F, as this is an invalid commander file
                            \
                            \ ELT2F contains a BRK instruction, which will force an
                            \ interrupt to call the address in BRKV, which will
                            \ print out the system error at ELT2F
    
     LDY #NT%               \ We have successfully loaded the commander file to the
                            \ TAP% staging area, so now we want to copy it to the
                            \ last saved commander data block at NA%+8, so we set up
                            \ a counter in Y to copy NT% bytes
    
    .copyme
    
     LDA TAP%,Y             \ Copy the Y-th byte of TAP% to the Y-th byte of NA%+8
     STA NA%+8,Y
    
     DEY                    \ Decrement the loop counter
    
     BPL copyme             \ Loop back until we have copied all NT% bytes
    
    .LOR
    
     SEC                    \ Set the C flag
    
     RTS                    \ Return from the subroutine
    
    .ELT2F
    
     LDA #9                 \ Print extended token 9 ("{cr}{all caps}ILLEGAL ELITE
     JSR DETOK              \ II FILE{sentence case}")
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     JMP SVE                \ Jump to SVE to display the disc access menu and return
                            \ from the subroutine using a tail call
    
    
    
    
           Name: backtonormal                                            [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Disable the keyboard, set the SVN flag to 0, and return with A = 0
    
    
        Context: See this subroutine [on its own page](../main/subroutine/backtonormal.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine is unused in this version of Elite (it is left over from the
     6502 Second Processor version).
    
    
    
    
    .backtonormal
    
     RTS                    \ Return from the subroutine, as backtonormal does
                            \ nothing in this version of Elite (it is left over from
                            \ the 6502 Second Processor version)
    
    
    
    
           Name: CLDELAY                                                 [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: Delay by iterating through 5 * 256 (1280) empty loops
    
    
        Context: See this subroutine [on its own page](../main/subroutine/cldelay.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine is unused in this version of Elite (it is left over from the
     6502 Second Processor version).
    
    
    
    
    .CLDELAY
    
     RTS                    \ Return from the subroutine, as CLDELAY does nothing in
                            \ this version of Elite (it is left over from the 6502
                            \ Second Processor version)
    
    
    
    
           Name: CTLI                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for cataloguing a disc
    
    
        Context: See this variable [on its own page](../main/variable/ctli.md)
     Variations: See [code variations](../related/compare/main/variable/ctli.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [CATS](../main/subroutine/cats.md) uses CTLI
                 * [DELT](../main/subroutine/delt.md) uses CTLI
    
    
    
    
    
    
    .CTLI
    
    IF _SNG47
    
     EQUS "CAT 1"           \ The "1" part of the string is overwritten with the
     EQUB 13                \ actual drive number by the CATS routine
    
    ELIF _COMPACT
    
     EQUS "CAT"             \ The Master Compact only has one drive, so the CAT
     EQUB 13                \ command always catalogues that drive
    
    ENDIF
    
    
    
    
           Name: DELI                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for deleting a file
    
    
        Context: See this variable [on its own page](../main/variable/deli.md)
     Variations: See [code variations](../related/compare/main/variable/deli.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DELT](../main/subroutine/delt.md) uses DELI
    
    
    
    
    
    
    .DELI
    
    IF _SNG47
    
     EQUS "DELETE :1.1234567"
     EQUB 13
    
    ELIF _COMPACT
    
     EQUS "DELETE 1234567890"
     EQUB 13
    
    ENDIF
    
    
    
    
           Name: DIRI                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for changing directory on the Master Compact
    
    
        Context: See this variable [on its own page](../main/variable/diri.md)
     References: This variable is used as follows:
                 * [GTDIR](../main/subroutine/gtdir.md) uses DIRI
    
    
    
    
    
    
    IF _COMPACT
    
    .DIRI
    
     EQUS "DIR 12345678901234567890"
     EQUB 13
    
    ENDIF
    
    
    
    
           Name: savosc                                                  [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for saving a commander file
    
    
        Context: See this variable [on its own page](../main/variable/savosc.md)
     References: This variable is used as follows:
                 * [SVE](../main/subroutine/sve.md) uses savosc
                 * [wfile](../main/subroutine/wfile.md) uses savosc
    
    
    
    
    
    
    .savosc
    
    IF _SNG47
    
     EQUS "SAVE :1.E.JAMESON  E7E +100 0 0"
     EQUB 13
    
    ELIF _COMPACT
    
     EQUS "SAVE JAMESON  E7E +100 0 0"
     EQUB 13
    
    ENDIF
    
    
    
    
           Name: lodosc                                                  [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for loading a commander file
    
    
        Context: See this variable [on its own page](../main/variable/lodosc.md)
     References: This variable is used as follows:
                 * [rfile](../main/subroutine/rfile.md) uses lodosc
                 * [SVE](../main/subroutine/sve.md) uses lodosc
    
    
    
    
    
    
    .lodosc
    
    IF _SNG47
    
     EQUS "LOAD :1.E.JAMESON  E7E"
     EQUB 13
    
    ELIF _COMPACT
    
     EQUS "LOAD JAMESON  E7E"
     EQUB 13
    
    ENDIF
    
    
    
    
           Name: wfile                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Save the commander file
    
    
        Context: See this subroutine [on its own page](../main/subroutine/wfile.md)
     References: This subroutine is called as follows:
                 * [SVE](../main/subroutine/sve.md) calls wfile
    
    
    
    
    
    * * *
    
    
     This routine copies a commander file into the commbuf file buffer at &0E7E,
     and then saves it.
    
    
    
    
    .wfile
    
     LDY #NT%               \ We first want to copy the current commander data block
                            \ to the commbuf file buffer so we can do a file save
                            \ operation, so we set a counter in Y to copy the NT%
                            \ bytes in the commander data block
    
    .wfileL1
    
     LDA NA%+8,Y            \ Copy the Y-th byte of NA%+8 to the Y-th byte of the
     STA commbuf,Y          \ commbuf file buffer
    
     DEY                    \ Decrement the loop counter
    
     BPL wfileL1            \ Loop back until we have copied all the bytes in the
                            \ commander data block
    
     LDA #0                 \ The save file is 256 bytes long but only NT% (76) of
                            \ those contain data, so before we save we need to zero
                            \ out the rest of the 256-byte block, so we set A = 0 to
                            \ do this
    
     LDY #NT%               \ Set an index in Y to point the byte after the end of
                            \ the first NT% bytes in the commander data block
    
    .wfileL2
    
     STA commbuf,Y          \ Zero the Y-th byte of the commbuf file buffer
    
     INY                    \ Increment the loop counter
    
     BNE wfileL2            \ Loop back until we have zeroed the rest of the
                            \ 256-byte block
    
     LDY #0                 \ Now we need to change the save command at savosc to
                            \ contain the commander name as the filename, so set an
                            \ index in Y so we can copy the commander name from NA%
                            \ into the savosc command
    
    .wfileL3
    
     LDA NA%,Y              \ Fetch the Y-th character of the commander name at NA%
    
     CMP #13                \ If the character is a carriage return then we have
     BEQ wfileL4            \ reached the end of the name, so jump to wfileL4 as we
                            \ have now copied the whole name
    
    IF _SNG47
    
     STA savosc+10,Y        \ Store the Y-th character of the commander name in the
                            \ Y-th character of savosc+10, where savosc+10 points to
                            \ the JAMESON part of the save command in savosc:
                            \
                            \   "SAVE :1.E.JAMESON  E7E +100 0 0"
    
    ELIF _COMPACT
    
     STA savosc+5,Y         \ Store the Y-th character of the commander name in the
                            \ Y-th character of savosc+5, where savosc+5 points to
                            \ the JAMESON part of the save command in savosc:
                            \
                            \   "SAVE JAMESON  E7E +100 0 0"
    
    ENDIF
    
     INY                    \ Increment the loop counter
    
     CPY #7                 \ If Y < 7 then there may be more characters in the
     BCC wfileL3            \ name, so loop back to wfileL3 to fetch the next one
    
    .wfileL4
    
     LDA #' '               \ We have copied the name into the savosc command
                            \ string, but the new name might be shorter than the
                            \ previous one, so we now need to blank out the rest
                            \ of the name with spaces, so we load the space
                            \ character into A
    
    .wfileL5
    
    IF _SNG47
    
     STA savosc+10,Y        \ Store the Y-th character of the commander name in the
                            \ Y-th character of savosc+10, which will be directly
                            \ after the last letter we copied above
    
    ELIF _COMPACT
    
     STA savosc+5,Y         \ Store the Y-th character of the commander name in the
                            \ Y-th character of savosc+5, which will be directly
                            \ after the last letter we copied above
    
    ENDIF
    
     INY                    \ Increment the loop counter
    
     CPY #7                 \ If Y < 7 then we haven't yet blanked out the whole
     BCC wfileL4            \ name, so loop back to wfileL4 to blank the next one
                            \ until the save string is ready for use
    
    IF _SNG47
    
     JSR getzp              \ Call getzp to store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ELIF _COMPACT
    
     JSR NMIRELEASE         \ Release the NMI workspace (&00A0 to &00A7) so the MOS
                            \ can use it, and store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ENDIF
    
     LDX #LO(savosc)        \ Set (Y X) to point to the OS command at savosc, which
     LDY #HI(savosc)        \ contains the DFS command for saving the commander file
    
     JSR OSCLI              \ Call OSCLI to execute the OS command at (Y X), which
                            \ saves the commander file
    
     JMP getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000 and return from the subroutine
                            \ using a tail call
    
    
    
    
           Name: rfile                                                   [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Load the commander file
    
    
        Context: See this subroutine [on its own page](../main/subroutine/rfile.md)
     References: This subroutine is called as follows:
                 * [LOD](../main/subroutine/lod.md) calls rfile
    
    
    
    
    
    * * *
    
    
     This routine loads a commander file into the commbuf file buffer at &0E7E, and
     then copies it to the TAP% staging area (though the latter is not used in this
     version, as it's left over from the Commodore 64 version).
    
    
    
    * * *
    
    
     Arguments:
    
       INWK+5               The filename, terminated by a carriage return
    
    
    
    
    .rfile
    
     LDY #0                 \ We start by changing the load command at lodosc to
                            \ contain the filename that was just entered by the
                            \ user, so we set an index in Y so we can copy the
                            \ filename from INWK+5 into the lodosc command
    
    .rfileL3
    
     LDA INWK+5,Y           \ Fetch the Y-th character of the filename
    
     CMP #13                \ If the character is a carriage return then we have
     BEQ rfileL4            \ reached the end of the filename, so jump to rfileL4 as
                            \ we have now copied the whole filename
    
    IF _SNG47
    
     STA lodosc+10,Y        \ Store the Y-th character of the filename in the Y-th
                            \ character of lodosc+10, where lodosc+10 points to the
                            \ JAMESON part of the load command in lodosc:
                            \
                            \   "LOAD :1.E.JAMESON  E7E"
    
    ELIF _COMPACT
    
     STA lodosc+5,Y         \ Store the Y-th character of the filename in the Y-th
                            \ character of lodosc+5, where lodosc+5 points to the
                            \ JAMESON part of the load command in lodosc:
                            \
                            \   "LOAD JAMESON  E7E"
    
    ENDIF
    
     INY                    \ Increment the loop counter
    
     CPY #7                 \ If Y < 7 then there may be more characters in the
     BCC rfileL3            \ name, so loop back to rfileL3 to fetch the next one
    
    .rfileL4
    
     LDA #' '               \ We have copied the name into the lodosc command
                            \ string, but the new name might be shorter than the
                            \ previous one, so we now need to blank out the rest of
                            \ the name with spaces, so we load the space character
                            \ into A
    
    .rfileL5
    
    IF _SNG47
    
     STA lodosc+10,Y        \ Store the Y-th character of the filename in the Y-th
                            \ character of lodosc+10, which will be directly after
                            \ the last letter we copied above
    
    ELIF _COMPACT
    
     STA lodosc+5,Y         \ Store the Y-th character of the filename in the Y-th
                            \ character of lodosc+5, which will be directly after
                            \ the last letter we copied above
    
    ENDIF
    
     INY                    \ Increment the loop counter
    
     CPY #7                 \ If Y < 7 then we haven't yet blanked out the whole
     BCC rfileL4            \ name, so loop back to rfileL4 to blank the next one
                            \ until the load string is ready for use
    
    IF _SNG47
    
     JSR getzp              \ Call getzp to store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ELIF _COMPACT
    
     JSR NMIRELEASE         \ Release the NMI workspace (&00A0 to &00A7) so the MOS
                            \ can use it, and store the top part of zero page in the
                            \ the buffer at &3000, as it gets corrupted by the MOS
                            \ during disc access
    
    ENDIF
    
     LDX #LO(lodosc)        \ Set (Y X) to point to the OS command at lodosc, which
     LDY #HI(lodosc)        \ contains the DFS command for loading the commander
                            \ file
    
     JSR OSCLI              \ Call OSCLI to execute the OS command at (Y X), which
                            \ loads the commander file
    
     JSR getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000
    
                            \ We now copy the newly loaded commander data block to
                            \ the TAP% staging area, though this has no effect as we
                            \ then ignore the result (this code is left over from
                            \ the Commodore 64 version)
    
     LDY #NT%               \ Set a counter in Y to copy the NT% bytes in the
                            \ commander data block
    
    .rfileL1
    
     LDA commbuf,Y          \ Copy the Y-th byte of the commbuf file buffer to the
     STA TAP%,Y             \ Y-th byte of the TAP% staging area
    
     DEY                    \ Decrement the loop counter
    
     BPL rfileL1            \ Loop back until we have copied all the bytes in the
                            \ newly loaded commander data block
    
     RTS                    \ Return from the subroutine
    
     RTS                    \ This instruction has no effect as we already returned
                            \ from the subroutine
    
    
    
    
           Name: SPS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the vector to the planet and store it in XX15
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sps1.md)
     References: This subroutine is called as follows:
                 * [COMPAS](../main/subroutine/compas.md) calls SPS1
                 * [Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md) calls SPS1
                 * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md) calls SPS1
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       SPS1+1               A BRK instruction
    
    
    
    
    .SPS1
    
     LDX #0                 \ Copy the two high bytes of the planet's x-coordinate
     JSR SPS3               \ into K3(2 1 0), separating out the sign bit into K3+2
    
     LDX #3                 \ Copy the two high bytes of the planet's y-coordinate
     JSR SPS3               \ into K3(5 4 3), separating out the sign bit into K3+5
    
     LDX #6                 \ Copy the two high bytes of the planet's z-coordinate
     JSR SPS3               \ into K3(8 7 6), separating out the sign bit into K3+8
    
                            \ Fall through into TAS2 to build XX15 from K3
    
    
    
    
           Name: TAS2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Normalise the three-coordinate vector in K3
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tas2.md)
     References: This subroutine is called as follows:
                 * [DOCKIT](../main/subroutine/dockit.md) calls TAS2
                 * [SPS4](../main/subroutine/sps4.md) calls TAS2
                 * [TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md) calls TAS2
                 * [DOCKIT](../main/subroutine/dockit.md) calls via TA2
    
    
    
    
    
    * * *
    
    
     Normalise the vector in K3, which has 16-bit values and separate sign bits,
     and store the normalised version in XX15 as a signed 8-bit vector.
    
     A normalised vector (also known as a unit vector) has length 1, so this
     routine takes an existing vector in K3 and scales it so the length of the
     new vector is 1. This is used in a number of places: when drawing the compass,
     when applying AI tactics to ships (so traders fly towards planets and missiles
     fly towards their targets, for example), and when implementing the docking
     computer in the enhanced versions of Elite.
    
     We do this in two stages. This stage shifts the 16-bit vector coordinates in
     K3 to the left as far as they will go without losing any bits off the end, so
     we can then take the high bytes and use them as the most accurate 8-bit vector
     to normalise. Then the next stage (in routine NORM) does the normalisation.
    
    
    
    * * *
    
    
     Arguments:
    
       K3(2 1 0)            The 16-bit x-coordinate as (x_sign x_hi x_lo), where
                            x_sign is just bit 7
    
       K3(5 4 3)            The 16-bit y-coordinate as (y_sign y_hi y_lo), where
                            y_sign is just bit 7
    
       K3(8 7 6)            The 16-bit z-coordinate as (z_sign z_hi z_lo), where
                            z_sign is just bit 7
    
    
    
    * * *
    
    
     Returns:
    
       XX15                 The normalised vector, with:
    
                              * The x-coordinate in XX15
    
                              * The y-coordinate in XX15+1
    
                              * The z-coordinate in XX15+2
    
    
    
    * * *
    
    
     Other entry points:
    
       TA2                  Calculate the length of the vector in XX15 (ignoring the
                            low coordinates), returning it in Q
    
    
    
    
    .TAS2
    
     LDA K3                 \ OR the three low bytes and 1 to get a byte that has
     ORA K3+3               \ a 1 wherever any of the three low bytes has a 1
     ORA K3+6               \ (as well as always having bit 0 set), and store in
     ORA #1                 \ K3+9
     STA K3+9
    
     LDA K3+1               \ OR the three high bytes to get a byte in A that has a
     ORA K3+4               \ 1 wherever any of the three high bytes has a 1
     ORA K3+7
    
                            \ (A K3+9) now has a 1 wherever any of the 16-bit
                            \ values in K3 has a 1
    .TAL2
    
     ASL K3+9               \ Shift (A K3+9) to the left, so bit 7 of the high byte
     ROL A                  \ goes into the C flag
    
     BCS TA2                \ If the left shift pushed a 1 out of the end, then we
                            \ know that at least one of the coordinates has a 1 in
                            \ this position, so jump to TA2 as we can't shift the
                            \ values in K3 any further to the left
    
     ASL K3                 \ Shift K3(1 0), the x-coordinate, to the left
     ROL K3+1
    
     ASL K3+3               \ Shift K3(4 3), the y-coordinate, to the left
     ROL K3+4
    
     ASL K3+6               \ Shift K3(6 7), the z-coordinate, to the left
     ROL K3+7
    
     BCC TAL2               \ Jump back to TAL2 to do another shift left (this BCC
                            \ is effectively a JMP as we know bit 7 of K3+7 is not a
                            \ 1, as otherwise bit 7 of A would have been a 1 and we
                            \ would have taken the BCS above)
    
    .TA2
    
     LDA K3+1               \ Fetch the high byte of the x-coordinate from our left-
     LSR A                  \ shifted K3, shift it right to clear bit 7, stick the
     ORA K3+2               \ sign bit in there from the x_sign part of K3, and
     STA XX15               \ store the resulting signed 8-bit x-coordinate in XX15
    
     LDA K3+4               \ Fetch the high byte of the y-coordinate from our left-
     LSR A                  \ shifted K3, shift it right to clear bit 7, stick the
     ORA K3+5               \ sign bit in there from the y_sign part of K3, and
     STA XX15+1             \ store the resulting signed 8-bit y-coordinate in
                            \ XX15+1
    
     LDA K3+7               \ Fetch the high byte of the z-coordinate from our left-
     LSR A                  \ shifted K3, shift it right to clear bit 7, stick the
     ORA K3+8               \ sign bit in there from the z_sign part of K3, and
     STA XX15+2             \ store the resulting signed 8-bit  z-coordinate in
                            \ XX15+2
    
                            \ Now we have a signed 8-bit version of the vector K3 in
                            \ XX15, so fall through into NORM to normalise it
    
    
    
    
           Name: NORM                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Normalise the three-coordinate vector in XX15
      Deep dive: [Tidying orthonormal vectors](https://elite.bbcelite.com/deep_dives/tidying_orthonormal_vectors.html)
                 [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/norm.md)
     References: This subroutine is called as follows:
                 * [TIDY](../main/subroutine/tidy.md) calls NORM
    
    
    
    
    
    * * *
    
    
     We do this by dividing each of the three coordinates by the length of the
     vector, which we can calculate using Pythagoras. Once normalised, 96 (&60) is
     used to represent a value of 1, and 96 with bit 7 set (&E0) is used to
     represent -1. This enables us to represent fractional values of less than 1
     using integers.
    
    
    
    * * *
    
    
     Arguments:
    
       XX15                 The vector to normalise, with:
    
                              * The x-coordinate in XX15
    
                              * The y-coordinate in XX15+1
    
                              * The z-coordinate in XX15+2
    
    
    
    * * *
    
    
     Returns:
    
       XX15                 The normalised vector
    
       Q                    The length of the original XX15 vector
    
    
    
    * * *
    
    
     Other entry points:
    
       NO1                  Contains an RTS
    
    
    
    
    .NORM
    
     LDA XX15               \ Fetch the x-coordinate into A
    
     JSR SQUA               \ Set (A P) = A * A = x^2
    
     STA R                  \ Set (R Q) = (A P) = x^2
     LDA P
     STA Q
    
     LDA XX15+1             \ Fetch the y-coordinate into A
    
     JSR SQUA               \ Set (A P) = A * A = y^2
    
     STA T                  \ Set (T P) = (A P) = y^2
    
     LDA P                  \ Set (R Q) = (R Q) + (T P) = x^2 + y^2
     ADC Q                  \
     STA Q                  \ First, doing the low bytes, Q = Q + P
    
     LDA T                  \ And then the high bytes, R = R + T
     ADC R
     STA R
    
     LDA XX15+2             \ Fetch the z-coordinate into A
    
     JSR SQUA               \ Set (A P) = A * A = z^2
    
     STA T                  \ Set (T P) = (A P) = z^2
    
     LDA P                  \ Set (R Q) = (R Q) + (T P) = x^2 + y^2 + z^2
     ADC Q                  \
     STA Q                  \ First, doing the low bytes, Q = Q + P
    
     LDA T                  \ And then the high bytes, R = R + T
     ADC R
     STA R
    
     JSR LL5                \ We now have the following:
                            \
                            \ (R Q) = x^2 + y^2 + z^2
                            \
                            \ so we can call LL5 to use Pythagoras to get:
                            \
                            \ Q = SQRT(R Q)
                            \   = SQRT(x^2 + y^2 + z^2)
                            \
                            \ So Q now contains the length of the vector (x, y, z),
                            \ and we can normalise the vector by dividing each of
                            \ the coordinates by this value, which we do by calling
                            \ routine TIS2. TIS2 returns the divided figure, using
                            \ 96 to represent 1 and 96 with bit 7 set for -1
    
     LDA XX15               \ Call TIS2 to divide the x-coordinate in XX15 by Q,
     JSR TIS2               \ with 1 being represented by 96
     STA XX15
    
     LDA XX15+1             \ Call TIS2 to divide the y-coordinate in XX15+1 by Q,
     JSR TIS2               \ with 1 being represented by 96
     STA XX15+1
    
     LDA XX15+2             \ Call TIS2 to divide the z-coordinate in XX15+2 by Q,
     JSR TIS2               \ with 1 being represented by 96
     STA XX15+2
    
    .NO1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: WARP                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Perform an in-system jump
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/warp.md)
     Variations: See [code variations](../related/compare/main/subroutine/warp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls WARP
    
    
    
    
    
    * * *
    
    
     This is called when we press "J" during flight. The following checks are
     performed:
    
       * Make sure we don't have any ships or space stations in the vicinity
    
       * Make sure we are not in witchspace
    
       * If we are facing the planet, make sure we aren't too close
    
       * If we are facing the sun, make sure we aren't too close
    
     If the above checks are passed, then we perform an in-system jump by moving
     the sun and planet in the opposite direction to travel, so we appear to jump
     in space. This means that any asteroids, cargo canisters or escape pods get
     dragged along for the ride.
    
    
    
    
    .WARP
    
     LDX JUNK               \ Set X to the total number of junk items in the
                            \ vicinity (e.g. asteroids, escape pods, cargo
                            \ canisters, Shuttles, Transporters and so on)
    
     LDA FRIN+2,X           \ If the slot at FRIN+2+X is non-zero, then we have
                            \ something else in the vicinity besides asteroids,
                            \ escape pods and cargo canisters, so to check whether
                            \ we can jump, we first grab the slot contents into A
    
     ORA SSPR               \ If there is a space station nearby, then SSPR will
                            \ be non-zero, so OR'ing with SSPR will produce a
                            \ non-zero result if either A or SSPR are non-zero
    
     ORA MJ                 \ If we are in witchspace, then MJ will be non-zero, so
                            \ OR'ing with MJ will produce a non-zero result if
                            \ either A or SSPR or MJ are non-zero
    
     BNE WA1                \ A is non-zero if we have either a ship or a space
                            \ station in the vicinity, or we are in witchspace, in
                            \ which case jump to WA1 to make a low beep to show that
                            \ we can't do an in-system jump
    
     LDY K%+8               \ Otherwise we can do an in-system jump, so now we fetch
                            \ the byte at K%+8, which contains the z_sign for the
                            \ first ship slot, i.e. the distance of the planet
    
     BMI WA3                \ If the planet's z_sign is negative, then the planet
                            \ is behind us, so jump to WA3 to skip the following
    
     TAY                    \ Set A = Y = 0 (as we didn't BNE above) so the call
                            \ to MAS2 measures the distance to the planet
    
     JSR MAS2               \ Call MAS2 to set A to the largest distance to the
                            \ planet in any of the three axes (we could also call
                            \ routine m to do the same thing, as A = 0)
    
     CMP #2                 \ If A < 2 then jump to WA1 to abort the in-system jump
     BCC WA1                \ with a low beep, as we are facing the planet and are
                            \ too close to jump in that direction
    
    .WA3
    
     LDY K%+NI%+8           \ Fetch the z_sign (byte #8) of the second ship in the
                            \ ship data workspace at K%, which is reserved for the
                            \ sun or the space station (in this case it's the
                            \ former, as we already confirmed there isn't a space
                            \ station in the vicinity)
    
     BMI WA2                \ If the sun's z_sign is negative, then the sun is
                            \ behind us, so jump to WA2 to skip the following
    
     LDY #NI%               \ Set Y to point to the offset of the ship data block
                            \ for the sun, which is NI% (as each block is NI% bytes
                            \ long, and the sun is the second block)
    
     JSR m                  \ Call m to set A to the largest distance to the sun
                            \ in any of the three axes
    
     CMP #2                 \ If A < 2 then jump to WA1 to abort the in-system jump
     BCC WA1                \ with a low beep, as we are facing the sun and are too
                            \ close to jump in that direction
    
    .WA2
    
                            \ If we get here, then we can do an in-system jump, as
                            \ we don't have any ships or space stations in the
                            \ vicinity, we are not in witchspace, and if we are
                            \ facing the planet or the sun, we aren't too close to
                            \ jump towards it
                            \
                            \ We do an in-system jump by moving the sun and planet,
                            \ rather than moving our own local bubble (this is why
                            \ in-system jumps drag asteroids, cargo canisters and
                            \ escape pods along for the ride). Specifically, we move
                            \ them in the z-axis by a fixed amount in the opposite
                            \ direction to travel, thus performing a jump towards
                            \ our destination
    
     LDA #&81               \ Set R = R = P = &81
     STA S
     STA R
     STA P
    
     LDA K%+8               \ Set A = z_sign for the planet
    
     JSR ADD                \ Set (A X) = (A P) + (S R)
                            \           = (z_sign &81) + &8181
                            \           = (z_sign &81) - &0181
                            \
                            \ This moves the planet against the direction of travel
                            \ by reducing z_sign by 1, as the above maths is:
                            \
                            \         z_sign 00000000
                            \   +   00000000 10000001
                            \   -   00000001 10000001
                            \
                            \ or:
                            \
                            \         z_sign 00000000
                            \   +   00000000 00000000
                            \   -   00000001 00000000
                            \
                            \ i.e. the high byte is z_sign - 1, making sure the sign
                            \ is preserved
    
     STA K%+8               \ Set the planet's z_sign to the high byte of the result
    
     LDA K%+NI%+8           \ Set A = z_sign for the sun
    
     JSR ADD                \ Set (A X) = (A P) + (S R)
                            \           = (z_sign &81) + &8181
                            \           = (z_sign &81) - &0181
                            \
                            \ which moves the sun against the direction of travel
                            \ by reducing z_sign by 1
    
     STA K%+NI%+8           \ Set the planet's z_sign to the high byte of the result
    
     LDA #1                 \ Temporarily set the view type to a non-zero value, so
     STA QQ11               \ the call to LOOK1 below clears the screen before
                            \ switching to the space view
    
     STA MCNT               \ Set the main loop counter to 1, so the next iteration
                            \ through the main loop will potentially spawn ships
                            \ (see part 2 of the main game loop at me3)
    
     LSR A                  \ Set EV, the extra vessels spawning counter, to 0
     STA EV                 \ (the LSR produces a 0 as A was previously 1)
    
     LDX VIEW               \ Set X to the current view (front, rear, left or right)
     JMP LOOK1              \ and jump to LOOK1 to initialise that view, returning
                            \ from the subroutine using a tail call
    
    .WA1
    
     JMP BOOP               \ Call the BOOP routine to make a long, low beep, and
                            \ return from the subroutine using a tail call
    
     RTS                    \ This instruction has no effect as we already returned
                            \ from the subroutine
    
    
    
    
           Name: DKS3                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Toggle a configuration setting and emit a beep
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dks3.md)
     Variations: See [code variations](../related/compare/main/subroutine/dks3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DK4](../main/subroutine/dk4.md) calls DKS3
    
    
    
    
    
    * * *
    
    
     This is called when the game is paused and a key is pressed that changes the
     game's configuration.
    
     Specifically, this routine toggles the configuration settings for the
     following keys:
    
       * CAPS LOCK toggles keyboard flight damping (0)
       * A toggles keyboard auto-recentre (1)
       * X toggles author names on start-up screen (2)
       * F toggles flashing console bars (3)
       * Y toggles reverse joystick Y channel (4)
       * J toggles reverse both joystick channels (5)
       * K toggles keyboard and joystick (6)
    
     The numbers in brackets are the configuration options that we pass in Y. We
     pass the ASCII code of the key that has been pressed in X, and the option to
     check it against in Y, so this routine is typically called in a loop that
     loops through the various configuration option.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The ASCII code of the key that's been pressed
    
       Y                    The number of the configuration option to check against
                            from the list above (i.e. Y must be from 0 to 6)
    
    
    
    
    .DKS3
    
     TXA                    \ Copy the ASCII code of the key that has been pressed
                            \ into A
    
     CMP TGINT,Y            \ If the pressed key doesn't match the configuration key
     BNE Dk3                \ for option Y (as listed in the TGINT table), then jump
                            \ to Dk3 to return from the subroutine
    
     LDA DAMP,Y             \ The configuration keys listed in TGINT correspond to
     EOR #&FF               \ the configuration option settings from DAMP onwards,
     STA DAMP,Y             \ so to toggle a setting, we fetch the existing byte
                            \ from DAMP+Y, invert it and put it back (0 means no
                            \ and &FF means yes in the configuration bytes, so
                            \ this toggles the setting)
    
     BPL P%+5               \ If the result has a clear bit 7 (i.e. we just turned
                            \ the option off), skip the following instruction
    
     JSR BELL               \ We just turned the option on, so make a standard
                            \ system beep, so in all we make two beeps
    
     JSR BELL               \ Make a beep sound so we know something has happened
    
     TYA                    \ Store Y and A on the stack so we can retrieve them
     PHA                    \ below
    
     LDY #20                \ Wait for 20/50 of a second (0.4 seconds)
     JSR DELAY
    
     PLA                    \ Restore A and Y from the stack
     TAY
    
    .Dk3
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DOKEY                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan for the seven primary flight controls and apply the docking
                 computer manoeuvring code
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
                 [The docking computer](https://elite.bbcelite.com/deep_dives/the_docking_computer.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dokey.md)
     Variations: See [code variations](../related/compare/main/subroutine/dokey.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT17](../main/subroutine/tt17.md) calls DOKEY
    
    
    
    
    
    * * *
    
    
     Scan for the seven primary flight controls (or the equivalent on joystick),
     pause and configuration keys, and secondary flight controls, and update the
     key logger accordingly. Specifically:
    
       * If we are on keyboard configuration, clear the key logger and update it
         for the seven primary flight controls, and update the pitch and roll
         rates accordingly.
    
       * If we are on joystick configuration, clear the key logger and jump to
         DKJ1, which reads the joystick equivalents of the primary flight
         controls.
    
     Both options end up at DK4 to scan for other keys, beyond the seven primary
     flight controls.
    
    
    
    
    .DOKEY
    
     JSR RDKEY-1            \ Scan the keyboard for a key press and return the
                            \ ASCII code of the key pressed in X (calling RDKEY-1
                            \ only scans the keyboard for valid BCD key numbers,
                            \ which speeds things up a bit)
    
     LDA auto               \ If auto is 0, then the docking computer is not
     BEQ DK15               \ currently activated, so jump to DK15 to skip the
                            \ docking computer manoeuvring code below
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     LDA #96                \ Set nosev_z_hi = 96
     STA INWK+14
    
     ORA #%10000000         \ Set sidev_x_hi = -96
     STA INWK+22
    
     STA TYPE               \ Set the ship type to -96, so the negative value will
                            \ let us check in the DOCKIT routine whether this is our
                            \ ship that is activating its docking computer, rather
                            \ than an NPC ship docking
    
     LDA DELTA              \ Set the ship speed to DELTA (our speed)
     STA INWK+27
    
     JSR DOCKIT             \ Call DOCKIT to calculate the docking computer's moves
                            \ and update INWK with the results
    
                            \ We now "press" the relevant flight keys, depending on
                            \ the results from DOCKIT, starting with the pitch keys
    
     LDA INWK+27            \ Fetch the updated ship speed from byte #27 into A
    
     CMP #22                \ If A < 22, skip the next instruction
     BCC P%+4
    
     LDA #22                \ Set A = 22, so the maximum speed during docking is 22
    
     STA DELTA              \ Update DELTA to the new value in A
    
     LDA #&FF               \ Set A = &FF, which we can insert into the key logger
                            \ to "fake" the docking computer working the keyboard
    
     LDX #15                \ Set X = 15, so we "press" KY+15, i.e. KY1, below
                            \ ("?", slow down)
    
     LDY INWK+28            \ If the updated acceleration in byte #28 is zero, skip
     BEQ DK11               \ to DK11
    
     BMI P%+4               \ If the updated acceleration is negative, skip the
                            \ following instruction
    
     LDX #11                \ Set X = 11, so we "press" KY+11, i.e. KY2, with the
                            \ next instruction (Space, speed up)
    
     STA KL,X               \ Store &FF in either KY1 or KY2 to "press" the relevant
                            \ key, depending on whether the updated acceleration is
                            \ negative (in which case we "press" KY1, "?", to slow
                            \ down) or positive (in which case we "press" KY2,
                            \ Space, to speed up)
    
    .DK11
    
                            \ We now "press" the relevant roll keys, depending on
                            \ the results from DOCKIT
    
     LDA #128               \ Set A = 128, which indicates no change in roll when
                            \ stored in JSTX (i.e. the centre of the roll indicator)
    
     LDX #13                \ Set X = 13, so we "press" KY+13, i.e. KY3, below
                            \ ("<", increase roll)
    
     ASL INWK+29            \ Shift ship byte #29 left, which shifts bit 7 of the
                            \ updated roll counter (i.e. the roll direction) into
                            \ the C flag
    
     BEQ DK12               \ If the remains of byte #29 is zero, then the updated
                            \ roll counter is zero, so jump to DK12 set JSTX to 128,
                            \ to indicate there's no change in the roll
    
     BCC P%+4               \ If the C flag is clear, skip the following instruction
    
     LDX #14                \ The C flag is set, i.e. the direction of the updated
                            \ roll counter is negative, so set X to 14 so we
                            \ "press" KY+14. i.e. KY4, below (">", decrease roll)
    
     BIT INWK+29            \ We shifted the updated roll counter to the left above,
     BPL DK14               \ so this tests bit 6 of the original value, and if it
                            \ is clear (i.e. the magnitude is less than 64), jump to
                            \ DK14 to "press" the key and leave JSTX unchanged
    
     LDA #64                \ The magnitude of the updated roll is 64 or more, so
     STA JSTX               \ set JSTX to 64 (so the roll decreases at half the
                            \ maximum rate)
    
     LDA #0                 \ And set A = 0 so we do not "press" any keys (so if the
                            \ docking computer needs to make a serious roll, it does
                            \ so by setting JSTX directly rather than by "pressing"
                            \ a key)
    
    .DK14
    
     STA KL,X               \ Store A in either KY3 or KY4, depending on whether
                            \ the updated roll rate is increasing (KY3) or
                            \ decreasing (KY4)
    
     LDA JSTX               \ Fetch A from JSTX so the next instruction has no
                            \ effect
    
    .DK12
    
     STA JSTX               \ Store A in JSTX to update the current roll rate
    
                            \ We now "press" the relevant pitch keys, depending on
                            \ the results from DOCKIT
    
     LDA #128               \ Set A = 128, which indicates no change in pitch when
                            \ stored in JSTX (i.e. the centre of the pitch
                            \ indicator)
    
     LDX #6                 \ Set X = 6, so we "press" KY+6, i.e. KY5, below
                            \ ("X", decrease pitch, pulling the nose up)
    
     ASL INWK+30            \ Shift ship byte #30 left, which shifts bit 7 of the
                            \ updated pitch counter (i.e. the pitch direction) into
                            \ the C flag
    
     BEQ DK13               \ If the remains of byte #30 is zero, then the updated
                            \ pitch counter is zero, so jump to DK13 set JSTY to
                            \ 128, to indicate there's no change in the pitch
    
     BCS P%+4               \ If the C flag is set, skip the following instruction
    
     LDX #8                 \ Set X = 8, so we "press" KY+8, i.e. KY6, with the next
                            \ instruction ("S", increase pitch, so the nose dives)
    
     STA KL,X               \ Store 128 in either KY5 or KY6 to "press" the relevant
                            \ key, depending on whether the pitch direction is
                            \ negative (in which case we "press" KY5, "X", to
                            \ decrease the pitch, pulling the nose up) or positive
                            \ (in which case we "press" KY6, "S", to increase the
                            \ pitch, pushing the nose down)
    
     LDA JSTY               \ Fetch A from JSTY so the next instruction has no
                            \ effect
    
    .DK13
    
     STA JSTY               \ Store A in JSTY to update the current pitch rate
    
    IF _COMPACT
    
     JMP DK152              \ Jump to DK152 to skip reading the joystick, as this is
                            \ a Master Compact that doesn't support an analogue
                            \ joystick (instead it supports a digital joystick,
                            \ which is read elsewhere)
    
    ENDIF
    
    .DK15
    
     LDA JSTK               \ If JSTK is zero, then we are configured to use the
     BEQ DK152              \ keyboard rather than the joystick, so jump to DK152 to
                            \ skip reading the joystick
    
    IF _SNG47
    
     LDA JOPOS              \ Fetch the high byte of the joystick X value
    
     EOR JSTE               \ The high byte A is now EOR'd with the value in
                            \ location JSTE, which contains &FF if both joystick
                            \ channels are reversed and 0 otherwise (so A now
                            \ contains the high byte but inverted, if that's what
                            \ the current settings say)
    
     ORA #1                 \ Ensure the value is at least 1
    
     STA JSTX               \ Store the resulting joystick X value in JSTX
    
     LDA JOPOS+1            \ Fetch the high byte of the joystick Y value
    
     EOR #&FF               \ This EOR is used in conjunction with the EOR JSTGY
                            \ below, as having a value of 0 in JSTGY means we have
                            \ to invert the joystick Y value, and this EOR does
                            \ that part
    
     EOR JSTE               \ The high byte A is now EOR'd with the value in
                            \ location JSTE, which contains &FF if both joystick
                            \ channels are reversed and 0 otherwise (so A now
                            \ contains the high byte but inverted, if that's what
                            \ the current settings say)
    
     EOR JSTGY              \ JSTGY will be 0 if the game is configured to reverse
                            \ the joystick Y channel, so this EOR along with the
                            \ EOR #&FF above does exactly that
    
     STA JSTY               \ Store the resulting joystick Y value in JSTY
    
     LDA VIA+&40            \ Read 6522 System VIA input register IRB (SHEILA &40)
    
     AND #%00010000         \ Bit 4 of IRB (PB4) is clear if joystick 1's fire
                            \ button is pressed, otherwise it is set, so AND'ing
                            \ the value of IRB with %10000 extracts this bit
    
     BNE DK4                \ If the joystick fire button is not being pressed,
                            \ jump to DK4 to scan for other keys
    
     LDA #&FF               \ Update the key logger at KY7 to "press" the "A" (fire)
     STA KY7                \ button
    
     BNE DK4                \ Jump to DK4 to scan for other keys (this BNE is
                            \ effectively a JMP as A is never 0)
    
    ELIF _COMPACT
    
     JSR RDJOY              \ Call RDJOY to read from either the analogue or digital
                            \ joystick
    
     BCC DK4                \ If we just read from the analogue joystick, the C flag
                            \ will be clear, so jump to DK4 to scan for other keys
    
                            \ Otherwise we just read the digital joystick, so we now
                            \ update the pitch and roll
    
    ENDIF
    
    .DK152
    
     LDX JSTX               \ Set X = JSTX, the current roll rate (as shown in the
                            \ RL indicator on the dashboard)
    
     LDA #7                 \ Set A to 7, which is the amount we want to alter the
                            \ roll rate by if the roll keys are being pressed
    
     LDY KY3                \ If the "<" key is not being pressed, skip the next
     BEQ P%+5               \ instruction
    
     JSR BUMP2              \ The "<" key is being pressed, so call the BUMP2
                            \ routine to increase the roll rate in X by A
    
     LDY KY4                \ If the ">" key is not being pressed, skip the next
     BEQ P%+5               \ instruction
    
     JSR REDU2              \ The "<" key is being pressed, so call the REDU2
                            \ routine to decrease the roll rate in X by A, taking
                            \ the keyboard auto re-centre setting into account
    
     STX JSTX               \ Store the updated roll rate in JSTX
    
     ASL A                  \ Double the value of A, to 14
    
     LDX JSTY               \ Set X = JSTY, the current pitch rate (as shown in the
                            \ DC indicator on the dashboard)
    
     LDY KY5                \ If the "X" key is not being pressed, skip the next
     BEQ P%+5               \ instruction
    
     JSR REDU2              \ The "X" key is being pressed, so call the REDU2
                            \ routine to decrease the pitch rate in X by A, taking
                            \ the keyboard auto re-centre setting into account
    
     LDY KY6                \ If the "S" key is not being pressed, skip the next
     BEQ P%+5               \ instruction
    
     JSR BUMP2              \ The "S" key is being pressed, so call the BUMP2
                            \ routine to increase the pitch rate in X by A
    
     STX JSTY               \ Store the updated roll rate in JSTY
    
                            \ Fall through into DK4 to scan for other keys
    
    
    
    
           Name: DK4                                                     [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan for pause, configuration and secondary flight keys
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dk4.md)
     Variations: See [code variations](../related/compare/main/subroutine/dk4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOKEY](../main/subroutine/dokey.md) calls DK4
    
    
    
    
    
    * * *
    
    
     Scan for pause and configuration keys, and if this is a space view, also scan
     for secondary flight controls.
    
     Specifically:
    
       * Scan for the pause button (COPY) and if it's pressed, pause the game and
         process any configuration key presses until the game is unpaused (DELETE)
    
       * If this is a space view, scan for secondary flight keys and update the
         relevant bytes in the key logger
    
    
    
    
    .DK4
    
     LDX KL                 \ Fetch the key pressed from byte #0 of the key logger
    
     CPX #&8B               \ If COPY is not being pressed, jump to DK2 below,
     BNE DK2                \ otherwise let's process the configuration keys
    
    .FREEZE
    
                            \ COPY is being pressed, so we enter a loop that
                            \ listens for configuration keys, and we keep looping
                            \ until we detect a DELETE key press. This effectively
                            \ pauses the game when COPY is pressed, and unpauses
                            \ it when DELETE is pressed
    
     JSR WSCAN              \ Call WSCAN to wait for the vertical sync, so the whole
                            \ screen gets drawn
    
     JSR RDKEY              \ Scan the keyboard for a key press and return the ASCII
                            \ code of the key pressed in A and X (or 0 for no key
                            \ press)
    
     CPX #'Q'               \ If "Q" is not being pressed, skip to DK6
     BNE DK6
    
     LDX #&FF               \ "Q" is being pressed, so set DNOIZ to &FF to turn the
     STX DNOIZ              \ sound off
    
     LDX #'Q'               \ Set X to the ASCII for "Q" once again, so it doesn't
                            \ get changed by the above
    
    .DK6
    
     LDY #0                 \ We now want to loop through the keys that toggle
                            \ various settings, so set a counter in Y to work our
                            \ way through them
    
    .DKL4
    
     JSR DKS3               \ Call DKS3 to scan for the key given in Y, and toggle
                            \ the relevant setting if it is pressed
    
     INY                    \ Increment Y to point to the next toggle key
    
     CPY #9                 \ Check to see whether we have reached the last toggle
                            \ key
    
     BNE DKL4               \ If not, loop back to check for the next toggle key
    
     LDA VOL                \ Fetch the current volume setting into A
    
     CPX #'.'               \ If "." is being pressed (i.e. the ">" key) then jump
     BEQ DOVOL1             \ to DOVOL1 to increase the volume
    
     CPX #','               \ If "," is not being pressed (i.e. the "<" key) then
     BNE DOVOL4             \ jump to DOVOL4 to skip the following
    
     DEC A                  \ The volume down key is being pressed, so decrement the
                            \ volume level in A
    
     EQUB &24               \ Skip the next instruction by turning it into &24 &1A,
                            \ or BIT &001A, which does nothing apart from affect the
                            \ flags
    
    .DOVOL1
    
     INC A                  \ The volume up key is being pressed, so increment the
                            \ volume level in A
    
     TAY                    \ Copy the new volume level to Y
    
     AND #%11111000         \ If any of bits 3-7 are set, skip to DOVOL3 as we have
     BNE DOVOL3             \ either increased the volume past the maximum volume of
                            \ 7, or we have decreased it below 0 to -1, and in
                            \ neither case do we want to change the volume as we are
                            \ already at the maximum or minimum level
    
     STY VOL                \ Store the new volume level in VOL
    
    .DOVOL3
    
     PHX                    \ Store X on the stack so we can retrieve it below after
                            \ making a beep
    
     JSR BEEP               \ Call the BEEP subroutine to make a short, high beep at
                            \ the new volume level
    
     LDY #10                \ Wait for 10/50 of a second (0.2 seconds)
     JSR DELAY
    
     PLX                    \ Restore the value of X we stored above
    
    .DOVOL4
    
     CPX #'B'               \ If "B" is not being pressed, skip to DOVOL2
     BNE DOVOL2
    
     LDA BSTK               \ Toggle the value of BSTK between 0 and &FF
     EOR #&FF
     STA BSTK
    
     STA JSTK               \ Configure JSTK to the same value, so when the Bitstik
                            \ is enabled, so is the joystick
    
     STA JSTE               \ Configure JSTE to the same value, so when the Bitstik
                            \ is enabled, the joystick is configured with reversed
                            \ channels
    
     BPL P%+5               \ If we just toggled the Bitstik off (i.e. to 0, which
                            \ is positive), then skip the first of these two
                            \ instructions, so we get two beeps for on and one beep
                            \ for off
    
     JSR BELL               \ We just enabled the Bitstik, so give two standard
                            \ system beeps (this being the first)
    
     JSR BELL               \ Make another system beep
    
    .DOVOL2
    
     CPX #'S'               \ If "S" is not being pressed, jump to DK7
     BNE DK7
    
     LDA #0                 \ "S" is being pressed, so set DNOIZ to 0 to turn the
     STA DNOIZ              \ sound on
    
    .DK7
    
     CPX #&1B               \ If ESCAPE is not being pressed, skip over the next
     BNE P%+5               \ instruction
    
     JMP DEATH2             \ ESCAPE is being pressed, so jump to DEATH2 to end
                            \ the game
    
     CPX #&7F               \ If DELETE is not being pressed, we are still paused,
     BNE FREEZE             \ so loop back up to keep listening for configuration
                            \ keys, otherwise fall through into the rest of the
                            \ key detection code, which unpauses the game
    
    .DK2
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT217                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard until a key is pressed
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt217.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt217.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [gnum](../main/subroutine/gnum.md) calls TT217
                 * [MT26](../main/subroutine/mt26.md) calls TT217
                 * [qv](../main/subroutine/qv.md) calls TT217
                 * [TT214](../main/subroutine/tt214.md) calls TT217
                 * [mes9](../main/subroutine/mes9.md) calls via out
                 * [OUCH](../main/subroutine/ouch.md) calls via out
                 * [GTDRV](../main/subroutine/gtdrv.md) calls via t
                 * [LOD](../main/subroutine/lod.md) calls via t
                 * [NEWBRK](../main/subroutine/newbrk.md) calls via t
                 * [SVE](../main/subroutine/sve.md) calls via t
                 * [YESNO](../main/subroutine/yesno.md) calls via t
    
    
    
    
    
    * * *
    
    
     Scan the keyboard until a key is pressed, and return the key's ASCII code.
     If, on entry, a key is already being held down, then wait until that key is
     released first (so this routine detects the first key down event following
     the subroutine call).
    
    
    
    * * *
    
    
     Returns:
    
       X                    The ASCII code of the key that was pressed
    
       A                    Contains the same as X
    
       Y                    Y is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       out                  Contains an RTS
    
       t                    As TT217 but don't preserve Y, set it to YSAV instead
    
    
    
    
    .TT217
    
     STY YSAV               \ Store Y in temporary storage, so we can restore it
                            \ later
    
    .t
    
     LDY #2                 \ Wait for 2/50 of a second (0.04 seconds) to implement
     JSR DELAY              \ a simple keyboard debounce and prevent multiple key
                            \ presses being recorded
    
     JSR RDKEY              \ Scan the keyboard for a key press and return the
                            \ ASCII code of the key pressed in A and X (or 0 for no
                            \ key press)
    
     BNE t                  \ If a key was already being held down when we entered
                            \ this routine, keep looping back up to t, until the
                            \ key is released
    
    .t2
    
     JSR RDKEY              \ Any pre-existing key press is now gone, so we can
                            \ start scanning the keyboard again, returning the
                            \ ASCII code of the key pressed in X (or 0 for no key
                            \ press)
    
     BEQ t2                 \ Keep looping up to t2 until a key is pressed
    
     LDY YSAV               \ Restore the original value of Y we stored above
    
     TAX                    \ Copy A into X
    
    .out
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: me1                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Erase an old in-flight message and display a new one
    
    
        Context: See this subroutine [on its own page](../main/subroutine/me1.md)
     Variations: See [code variations](../related/compare/main/subroutine/me1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [MESS](../main/subroutine/mess.md) calls me1
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
       X                    Must be set to 0
    
    
    
    
    .me1
    
     STX DLY                \ Set the message delay in DLY to 0, so any new
                            \ in-flight messages will be shown instantly
    
     PHA                    \ Store the new message token we want to print
    
     LDA #YELLOW            \ Switch to colour 1, which is yellow
     STA COL
    
     LDA MCH                \ Set A to the token number of the message that is
     JSR mes9               \ currently on-screen, and call mes9 to print it (which
                            \ will remove it from the screen, as printing is done
                            \ using EOR logic)
    
     PLA                    \ Restore the new message token
    
    
    
    
           Name: MESS                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Display an in-flight message
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mess.md)
     Variations: See [code variations](../related/compare/main/subroutine/mess.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EXNO2](../main/subroutine/exno2.md) calls MESS
                 * [FR1](../main/subroutine/fr1.md) calls MESS
                 * [Ghy](../main/subroutine/ghy.md) calls MESS
                 * [KILLSHP](../main/subroutine/killshp.md) calls MESS
                 * [Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md) calls MESS
                 * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md) calls MESS
                 * [Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md) calls MESS
                 * [me2](../main/subroutine/me2.md) calls MESS
                 * [ou2](../main/subroutine/ou2.md) calls MESS
                 * [ou3](../main/subroutine/ou3.md) calls MESS
                 * [OUCH](../main/subroutine/ouch.md) calls MESS
                 * [SFRMIS](../main/subroutine/sfrmis.md) calls MESS
    
    
    
    
    
    * * *
    
    
     Display an in-flight message in capitals at the bottom of the space view,
     erasing any existing in-flight message first.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .MESS
    
     PHA                    \ Store A on the stack so we can restore it after the
                            \ following
    
     LDX QQ11               \ If this is the space view, skip the following
     BEQ infrontvw          \ instruction
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
    .infrontvw
    
     LDA #21                \ Move the text cursor to row 21
     STA YC
    
     LDA #YELLOW            \ Switch to colour 1, which is yellow
     STA COL
    
     LDX #0                 \ Set QQ17 = 0 to switch to ALL CAPS
     STX QQ17
    
     LDA messXC             \ Move the text cursor to column messXC, in case we
     STA XC                 \ jump to me1 below to erase the current in-flight
                            \ message (whose column we stored in messXC when we
                            \ called MESS to put it there in the first place)
    
     PLA                    \ Restore A from the stack
    
     LDY #20                \ Set Y = 20 for setting the message delay below
    
     CPX DLY                \ If the message delay in DLY is not zero, jump up to
     BNE me1                \ me1 to erase the current message first (whose token
                            \ number will be in MCH)
    
     STY DLY                \ Set the message delay in DLY to 20
    
     STA MCH                \ Set MCH to the token we are about to display
    
                            \ Before we fall through into mes9 to print the token,
                            \ we need to work out the starting column for the
                            \ message we want to print, so it's centred on-screen,
                            \ so the following doesn't print anything, it just uses
                            \ the justified text mechanism to work out the number of
                            \ characters in the message we are going to print
    
     LDA #%11000000         \ Set the DTW4 flag to %11000000 (justify text, buffer
     STA DTW4               \ entire token including carriage returns)
    
     LDA de                 \ Set the C flag to bit 1 of the destruction flag in de
     LSR A
    
     LDA #0                 \ Set A = 0
    
     BCC P%+4               \ If the destruction flag in de is not set, skip the
                            \ following instruction
    
     LDA #10                \ Set A = 10
    
     STA DTW5               \ Store A in DTW5, so DTW5 (which holds the size of the
                            \ justified text buffer at BUF) is set to 0 if the
                            \ destruction flag is not set, or 10 if it is (10 being
                            \ the number of characters in the " DESTROYED" token)
    
     LDA MCH                \ Call TT27 to print the token in MCH into the buffer
     JSR TT27               \ (this doesn't print it on-screen, it just puts it into
                            \ the buffer and moves the DTW5 pointer along, so DTW5
                            \ now contains the size of the message we want to print,
                            \ including the " DESTROYED" part if that's going to be
                            \ included)
    
     LDA #32                \ Set A = (32 - DTW5) / 2
     SEC                    \
     SBC DTW5               \ so A now contains the column number we need to print
     LSR A                  \ our message at for it to be centred on-screen (as
                            \ there are 32 columns)
    
     STA messXC             \ Store A in messXC, so when we erase the message via
                            \ the branch to me1 above, messXC will tell us where to
                            \ print it
    
     STA XC                 \ Move the text cursor to column messXC
    
     JSR MT15               \ Call MT15 to switch to left-aligned text when printing
                            \ extended tokens disabling the justify text setting we
                            \ set above
    
     LDA MCH                \ Set MCH to the token we are about to display
    
                            \ Fall through into mes9 to print the token in A
    
    
    
    
           Name: mes9                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Print a text token, possibly followed by " DESTROYED"
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mes9.md)
     Variations: See [code variations](../related/compare/main/subroutine/mes9.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [me1](../main/subroutine/me1.md) calls mes9
    
    
    
    
    
    * * *
    
    
     Print a text token, followed by " DESTROYED" if the destruction flag is set
     (for when a piece of equipment is destroyed).
    
    
    
    
    .mes9
    
     JSR TT27               \ Call TT27 to print the text token in A
    
     LSR de                 \ If bit 0 of variable de is clear, return from the
     BCC out                \ subroutine (as out contains an RTS)
    
     LDA #253               \ Print recursive token 93 (" DESTROYED") and return
     JMP TT27               \ from the subroutine using a tail call
    
    
    
    
           Name: OUCH                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Potentially lose cargo or equipment following damage
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ouch.md)
     Variations: See [code variations](../related/compare/main/subroutine/ouch.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [OOPS](../main/subroutine/oops.md) calls OUCH
    
    
    
    
    
    * * *
    
    
     Our shields are dead and we are taking damage, so there is a small chance of
     losing cargo or equipment.
    
    
    
    
    .OUCH
    
     JSR DORND              \ Set A and X to random numbers
    
     BMI out                \ If A < 0 (50% chance), return from the subroutine
                            \ (as out contains an RTS)
    
     CPX #22                \ If X >= 22 (91% chance), return from the subroutine
     BCS out                \ (as out contains an RTS)
    
     LDA QQ20,X             \ If we do not have any of item QQ20+X, return from the
     BEQ out                \ subroutine (as out contains an RTS). X is in the range
                            \ 0-21, so this not only checks for cargo, but also for
                            \ E.C.M., fuel scoops, energy bomb, energy unit and
                            \ docking computer, all of which can be destroyed
    
     LDA DLY                \ If there is already an in-flight message on-screen,
     BNE out                \ return from the subroutine (as out contains an RTS)
    
     LDY #3                 \ Set bit 1 of de, the equipment destruction flag, so
     STY de                 \ that when we call MESS below, " DESTROYED" is appended
                            \ to the in-flight message
    
     STA QQ20,X             \ A is 0 (as we didn't branch with the BNE above), so
                            \ this sets QQ20+X to 0, which destroys any cargo or
                            \ equipment we have of that type
    
     CPX #17                \ If X >= 17 then we just lost a piece of equipment, so
     BCS ou1                \ jump to ou1 to print the relevant message
    
     TXA                    \ Print recursive token 48 + A as an in-flight token,
     ADC #208               \ which will be in the range 48 ("FOOD") to 64 ("ALIEN
     JMP MESS               \ ITEMS") as the C flag is clear, so this prints the
                            \ destroyed item's name, followed by " DESTROYED" (as we
                            \ set bit 1 of the de flag above), and returns from the
                            \ subroutine using a tail call
    
    .ou1
    
     BEQ ou2                \ If X = 17, jump to ou2 to print "E.C.M.SYSTEM
                            \ DESTROYED" and return from the subroutine using a tail
                            \ call
    
     CPX #18                \ If X = 18, jump to ou3 to print "FUEL SCOOPS
     BEQ ou3                \ DESTROYED" and return from the subroutine using a tail
                            \ call
    
     TXA                    \ Otherwise X is in the range 19 to 21 and the C flag is
     ADC #113-20            \ set (as we got here via a BCS to ou1), so we set A as
                            \ follows:
                            \
                            \   A = 113 - 20 + X + C
                            \     = 113 - 19 + X
                            \     = 113 to 115
    
     JMP MESS               \ Print recursive token A ("ENERGY BOMB", "ENERGY UNIT"
                            \ or "DOCKING COMPUTERS") as an in-flight message,
                            \ followed by " DESTROYED", and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: ou2                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Display "E.C.M.SYSTEM DESTROYED" as an in-flight message
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ou2.md)
     Variations: See [code variations](../related/compare/main/subroutine/ou2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [OUCH](../main/subroutine/ouch.md) calls ou2
    
    
    
    
    
    
    .ou2
    
     LDA #108               \ Set A to recursive token 108 ("E.C.M.SYSTEM")
    
     JMP MESS               \ Print recursive token A as an in-flight message,
                            \ followed by " DESTROYED", and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: ou3                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Display "FUEL SCOOPS DESTROYED" as an in-flight message
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ou3.md)
     Variations: See [code variations](../related/compare/main/subroutine/ou3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [OUCH](../main/subroutine/ouch.md) calls ou3
    
    
    
    
    
    
    .ou3
    
     LDA #111               \ Set A to recursive token 111 ("FUEL SCOOPS")
    
     JMP MESS               \ Print recursive token A as an in-flight message,
                            \ followed by " DESTROYED", and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: ITEM                                                    [Show more]
           Type: Macro
       Category: Market
        Summary: Macro definition for the market prices table
      Deep dive: [Market item prices and availability](https://elite.bbcelite.com/deep_dives/market_item_prices_and_availability.html)
    
    
        Context: See this macro [on its own page](../main/macro/item.md)
     References: This macro is used as follows:
                 * [QQ23](../main/variable/qq23.md) uses ITEM
    
    
    
    
    
    * * *
    
    
     The following macro is used to build the market prices table:
    
       ITEM price, factor, units, quantity, mask
    
     It inserts an item into the market prices table at QQ23.
    
    
    
    * * *
    
    
     Arguments:
    
       price                Base price
    
       factor               Economic factor
    
       units                Units: "t", "g" or "k"
    
       quantity             Base quantity
    
       mask                 Fluctuations mask
    
    
    
    
    MACRO ITEM price, factor, units, quantity, mask
    
     IF factor < 0
      s = 1 << 7
     ELSE
      s = 0
     ENDIF
    
     IF units = 't'
      u = 0
     ELIF units = 'k'
      u = 1 << 5
     ELSE
      u = 1 << 6
     ENDIF
    
     e = ABS(factor)
    
     EQUB price
     EQUB s + u + e
     EQUB quantity
     EQUB mask
    
    ENDMACRO
    
    
    
    
           Name: QQ23                                                    [Show more]
           Type: Variable
       Category: Market
        Summary: Market prices table
      Deep dive: [Market item prices and availability](https://elite.bbcelite.com/deep_dives/market_item_prices_and_availability.html)
    
    
        Context: See this variable [on its own page](../main/variable/qq23.md)
     Variations: See [code variations](../related/compare/main/variable/qq23.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [GVL](../main/subroutine/gvl.md) uses QQ23
                 * [TT151](../main/subroutine/tt151.md) uses QQ23
                 * [TT210](../main/subroutine/tt210.md) uses QQ23
    
    
    
    
    
    * * *
    
    
     Each item has four bytes of data, like this:
    
       Byte #0 = Base price
       Byte #1 = Economic factor in bits 0-4, with the sign in bit 7
                 Unit in bits 5-6
       Byte #2 = Base quantity
       Byte #3 = Mask to control price fluctuations
    
     To make it easier for humans to follow, I've defined a macro called ITEM
     that takes the following arguments and builds the four bytes for us:
    
       ITEM base price, economic factor, units, base quantity, mask
    
     So for food, we have the following, for example:
    
       * Base price = 19
       * Economic factor = -2
       * Unit = tonnes
       * Base quantity = 6
       * Mask = %00000001
    
    
    
    
    .QQ23
    
     ITEM 19,  -2, 't',   6, %00000001  \  0 = Food
     ITEM 20,  -1, 't',  10, %00000011  \  1 = Textiles
     ITEM 65,  -3, 't',   2, %00000111  \  2 = Radioactives
     ITEM 40,  -5, 't', 226, %00011111  \  3 = Slaves
     ITEM 83,  -5, 't', 251, %00001111  \  4 = Liquor/Wines
     ITEM 196,  8, 't',  54, %00000011  \  5 = Luxuries
     ITEM 235, 29, 't',   8, %01111000  \  6 = Narcotics
     ITEM 154, 14, 't',  56, %00000011  \  7 = Computers
     ITEM 117,  6, 't',  40, %00000111  \  8 = Machinery
     ITEM 78,   1, 't',  17, %00011111  \  9 = Alloys
     ITEM 124, 13, 't',  29, %00000111  \ 10 = Firearms
     ITEM 176, -9, 't', 220, %00111111  \ 11 = Furs
     ITEM 32,  -1, 't',  53, %00000011  \ 12 = Minerals
     ITEM 97,  -1, 'k',  66, %00000111  \ 13 = Gold
    
    \EQUD &360A118          \ This data is commented out in the original source
                            \
                            \ It would have inserted an item as follows:
                            \
                            \   ITEM 24, -1, 'k',  96, %00000011
                            \
                            \ So that's an item with a base price of 24 credits that
                            \ is slightly cheaper than average in agricultural
                            \ economies but closer to average in rich industrial
                            \ ones, with a base quantity of 96kg and a reasonably
                            \ stable price
                            \
                            \ I wonder what this mysterious item was going to be?
    
     ITEM 171, -2, 'k',  55, %00011111  \ 14 = Platinum
     ITEM 45,  -1, 'g', 250, %00001111  \ 15 = Gem-Stones
     ITEM 53,  15, 't', 192, %00000111  \ 16 = Alien items
    
    
    
    
           Name: TIDY                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Orthonormalise the orientation vectors for a ship
      Deep dive: [Tidying orthonormal vectors](https://elite.bbcelite.com/deep_dives/tidying_orthonormal_vectors.html)
                 [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tidy.md)
     References: This subroutine is called as follows:
                 * [HAS1](../main/subroutine/has1.md) calls TIDY
                 * [MVEIT (Part 1 of 9)](../main/subroutine/mveit_part_1_of_9.md) calls TIDY
    
    
    
    
    
    * * *
    
    
     This routine orthonormalises the orientation vectors for a ship. This means
     making the three orientation vectors orthogonal (perpendicular to each other),
     and normal (so each of the vectors has length 1).
    
     We do this because we use the small angle approximation to rotate these
     vectors in space. It is not completely accurate, so the three vectors tend
     to get stretched over time, so periodically we tidy the vectors with this
     routine to ensure they remain as orthonormal as possible.
    
    
    
    
    .TI2
    
                            \ Called from below with A = 0, X = 0, Y = 4 when
                            \ nosev_x and nosev_y are small, so we assume that
                            \ nosev_z is big
    
     TYA                    \ A = Y = 4
     LDY #2
     JSR TIS3               \ Call TIS3 with X = 0, Y = 2, A = 4, to set roofv_z =
     STA INWK+20            \ -(nosev_x * roofv_x + nosev_y * roofv_y) / nosev_z
    
     JMP TI3                \ Jump to TI3 to keep tidying
    
    .TI1
    
                            \ Called from below with A = 0, Y = 4 when nosev_x is
                            \ small
    
     TAX                    \ Set X = A = 0
    
     LDA XX15+1             \ Set A = nosev_y, and if the top two magnitude bits
     AND #%01100000         \ are both clear, jump to TI2 with A = 0, X = 0, Y = 4
     BEQ TI2
    
     LDA #2                 \ Otherwise nosev_y is big, so set up the index values
                            \ to pass to TIS3
    
     JSR TIS3               \ Call TIS3 with X = 0, Y = 4, A = 2, to set roofv_y =
     STA INWK+18            \ -(nosev_x * roofv_x + nosev_z * roofv_z) / nosev_y
    
     JMP TI3                \ Jump to TI3 to keep tidying
    
    .TIDY
    
     LDA INWK+10            \ Set (XX15, XX15+1, XX15+2) = nosev
     STA XX15
     LDA INWK+12
     STA XX15+1
     LDA INWK+14
     STA XX15+2
    
     JSR NORM               \ Call NORM to normalise the vector in XX15, i.e. nosev
    
     LDA XX15               \ Set nosev = (XX15, XX15+1, XX15+2)
     STA INWK+10
     LDA XX15+1
     STA INWK+12
     LDA XX15+2
     STA INWK+14
    
     LDY #4                 \ Set Y = 4
    
     LDA XX15               \ Set A = nosev_x, and if the top two magnitude bits
     AND #%01100000         \ are both clear, jump to TI1 with A = 0, Y = 4
     BEQ TI1
    
     LDX #2                 \ Otherwise nosev_x is big, so set up the index values
     LDA #0                 \ to pass to TIS3
    
     JSR TIS3               \ Call TIS3 with X = 2, Y = 4, A = 0, to set roofv_x =
     STA INWK+16            \ -(nosev_y * roofv_y + nosev_z * roofv_z) / nosev_x
    
    .TI3
    
     LDA INWK+16            \ Set (XX15, XX15+1, XX15+2) = roofv
     STA XX15
     LDA INWK+18
     STA XX15+1
     LDA INWK+20
     STA XX15+2
    
     JSR NORM               \ Call NORM to normalise the vector in XX15, i.e. roofv
    
     LDA XX15               \ Set roofv = (XX15, XX15+1, XX15+2)
     STA INWK+16
     LDA XX15+1
     STA INWK+18
     LDA XX15+2
     STA INWK+20
    
     LDA INWK+12            \ Set Q = nosev_y
     STA Q
    
     LDA INWK+20            \ Set A = roofv_z
    
     JSR MULT12             \ Set (S R) = Q * A = nosev_y * roofv_z
    
     LDX INWK+14            \ Set X = nosev_z
    
     LDA INWK+18            \ Set A = roofv_y
    
     JSR TIS1               \ Set (A ?) = (-X * A + (S R)) / 96
                            \        = (-nosev_z * roofv_y + nosev_y * roofv_z) / 96
                            \
                            \ This also sets Q = nosev_z
    
     EOR #%10000000         \ Set sidev_x = -A
     STA INWK+22            \        = (nosev_z * roofv_y - nosev_y * roofv_z) / 96
    
     LDA INWK+16            \ Set A = roofv_x
    
     JSR MULT12             \ Set (S R) = Q * A = nosev_z * roofv_x
    
     LDX INWK+10            \ Set X = nosev_x
    
     LDA INWK+20            \ Set A = roofv_z
    
     JSR TIS1               \ Set (A ?) = (-X * A + (S R)) / 96
                            \        = (-nosev_x * roofv_z + nosev_z * roofv_x) / 96
                            \
                            \ This also sets Q = nosev_x
    
     EOR #%10000000         \ Set sidev_y = -A
     STA INWK+24            \        = (nosev_x * roofv_z - nosev_z * roofv_x) / 96
    
     LDA INWK+18            \ Set A = roofv_y
    
     JSR MULT12             \ Set (S R) = Q * A = nosev_x * roofv_y
    
     LDX INWK+12            \ Set X = nosev_y
    
     LDA INWK+16            \ Set A = roofv_x
    
     JSR TIS1               \ Set (A ?) = (-X * A + (S R)) / 96
                            \        = (-nosev_y * roofv_x + nosev_x * roofv_y) / 96
    
     EOR #%10000000         \ Set sidev_z = -A
     STA INWK+26            \        = (nosev_y * roofv_x - nosev_x * roofv_y) / 96
    
     LDA #0                 \ Set A = 0 so we can clear the low bytes of the
                            \ orientation vectors
    
     LDX #14                \ We want to clear the low bytes, so start from sidev_y
                            \ at byte #9+14 (we clear all except sidev_z_lo, though
                            \ I suspect this is in error and that X should be 16)
    
    .TIL1
    
     STA INWK+9,X           \ Set the low byte in byte #9+X to zero
    
     DEX                    \ Set X = X - 2 to jump down to the next low byte
     DEX
    
     BPL TIL1               \ Loop back until we have zeroed all the low bytes
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TIS2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate A = A / Q
      Deep dive: [Shift-and-subtract division](https://elite.bbcelite.com/deep_dives/shift-and-subtract_division.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tis2.md)
     References: This subroutine is called as follows:
                 * [NORM](../main/subroutine/norm.md) calls TIS2
    
    
    
    
    
    * * *
    
    
     Calculate the following division, where A is a sign-magnitude number and Q is
     a positive integer:
    
       A = A / Q
    
     The value of A is returned as a sign-magnitude number with 96 representing 1,
     and the maximum value returned is 1 (i.e. 96). This routine is used when
     normalising vectors, where we represent fractions using integers, so this
     gives us an approximation to two decimal places.
    
    
    
    
    .TIS2
    
     TAY                    \ Store the argument A in Y
    
     AND #%01111111         \ Strip the sign bit from the argument, so A = |A|
    
     CMP Q                  \ If A >= Q then jump to TI4 to return a 1 with the
     BCS TI4                \ correct sign
    
     LDX #%11111110         \ Set T to have bits 1-7 set, so we can rotate through 7
     STX T                  \ loop iterations, getting a 1 each time, and then
                            \ getting a 0 on the 8th iteration... and we can also
                            \ use T to catch our result bits into bit 0 each time
    
    .TIL2
    
     ASL A                  \ Shift A to the left
    
     CMP Q                  \ If A < Q skip the following subtraction
     BCC P%+4
    
     SBC Q                  \ A >= Q, so set A = A - Q
                            \
                            \ Going into this subtraction we know the C flag is
                            \ set as we passed through the BCC above, and we also
                            \ know that A >= Q, so the C flag will still be set once
                            \ we are done
    
     ROL T                  \ Rotate the counter in T to the left, and catch the
                            \ result bit into bit 0 (which will be a 0 if we didn't
                            \ do the subtraction, or 1 if we did)
    
     BCS TIL2               \ If we still have set bits in T, loop back to TIL2 to
                            \ do the next iteration of 7
    
                            \ We've done the division and now have a result in the
                            \ range 0-255 here, which we need to reduce to the range
                            \ 0-96. We can do that by multiplying the result by 3/8,
                            \ as 256 * 3/8 = 96
    
     LDA T                  \ Set T = T / 4
     LSR A
     LSR A
     STA T
    
     LSR A                  \ Set T = T / 8 + T / 4
     ADC T                  \       = 3T / 8
     STA T
    
     TYA                    \ Fetch the sign bit of the original argument A
     AND #%10000000
    
     ORA T                  \ Apply the sign bit to T
    
     RTS                    \ Return from the subroutine
    
    .TI4
    
     TYA                    \ Fetch the sign bit of the original argument A
     AND #%10000000
    
     ORA #96                \ Apply the sign bit to 96 (which represents 1)
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TIS3                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate -(nosev_1 * roofv_1 + nosev_2 * roofv_2) / nosev_3
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tis3.md)
     References: This subroutine is called as follows:
                 * [TIDY](../main/subroutine/tidy.md) calls TIS3
    
    
    
    
    
    * * *
    
    
     Calculate the following expression:
    
       A = -(nosev_1 * roofv_1 + nosev_2 * roofv_2) / nosev_3
    
     where 1, 2 and 3 are x, y, or z, depending on the values of X, Y and A. This
     routine is called with the following values:
    
       X = 0, Y = 2, A = 4 ->
             A = -(nosev_x * roofv_x + nosev_y * roofv_y) / nosev_z
    
       X = 0, Y = 4, A = 2 ->
             A = -(nosev_x * roofv_x + nosev_z * roofv_z) / nosev_y
    
       X = 2, Y = 4, A = 0 ->
             A = -(nosev_y * roofv_y + nosev_z * roofv_z) / nosev_x
    
    
    
    * * *
    
    
     Arguments:
    
       X                    Index 1 (0 = x, 2 = y, 4 = z)
    
       Y                    Index 2 (0 = x, 2 = y, 4 = z)
    
       A                    Index 3 (0 = x, 2 = y, 4 = z)
    
    
    
    
    .TIS3
    
     STA P+2                \ Store P+2 in A for later
    
     LDA INWK+10,X          \ Set Q = nosev_x_hi (plus X)
     STA Q
    
     LDA INWK+16,X          \ Set A = roofv_x_hi (plus X)
    
     JSR MULT12             \ Set (S R) = Q * A
                            \           = nosev_x_hi * roofv_x_hi
    
     LDX INWK+10,Y          \ Set Q = nosev_x_hi (plus Y)
     STX Q
    
     LDA INWK+16,Y          \ Set A = roofv_x_hi (plus Y)
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
                            \           = (nosev_x,X * roofv_x,X) +
                            \             (nosev_x,Y * roofv_x,Y)
    
     STX P                  \ Store low byte of result in P, so result is now in
                            \ (A P)
    
     LDY P+2                \ Set Q = roofv_x_hi (plus argument A)
     LDX INWK+10,Y
     STX Q
    
     EOR #%10000000         \ Flip the sign of A
    
                            \ Fall through into DIVDT to do:
                            \
                            \   (P+1 A) = (A P) / Q
                            \
                            \     = -((nosev_x,X * roofv_x,X) +
                            \         (nosev_x,Y * roofv_x,Y))
                            \       / nosev_x,A
    
    
    
    
           Name: DVIDT                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (P+1 A) = (A P) / Q
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dvidt.md)
     Variations: See [code variations](../related/compare/main/subroutine/dvidt.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Calculate the following integer division between sign-magnitude numbers:
    
       (P+1 A) = (A P) / Q
    
     This uses the same shift-and-subtract algorithm as TIS2.
    
    
    
    
    .DVIDT
    
     STA P+1                \ Set P+1 = A, so P(1 0) = (A P)
    
     EOR Q                  \ Set T = the sign bit of A EOR Q, so it's 1 if A and Q
     AND #%10000000         \ have different signs, i.e. it's the sign of the result
     STA T                  \ of A / Q
    
     LDA #0                 \ Set A = 0 for us to build a result
    
     LDX #16                \ Set a counter in X to count the 16 bits in P(1 0)
    
     ASL P                  \ Shift P(1 0) left
     ROL P+1
    
     ASL Q                  \ Clear the sign bit of Q the C flag at the same time
     LSR Q
    
    .DVL2
    
     ROL A                  \ Shift A to the left
    
     CMP Q                  \ If A < Q skip the following subtraction
     BCC P%+4
    
     SBC Q                  \ Set A = A - Q
                            \
                            \ Going into this subtraction we know the C flag is
                            \ set as we passed through the BCC above, and we also
                            \ know that A >= Q, so the C flag will still be set once
                            \ we are done
    
     ROL P                  \ Rotate P(1 0) to the left, and catch the result bit
     ROL P+1                \ into the C flag (which will be a 0 if we didn't
                            \ do the subtraction, or 1 if we did)
    
     DEX                    \ Decrement the loop counter
    
     BNE DVL2               \ Loop back for the next bit until we have done all 16
                            \ bits of P(1 0)
    
     LDA P                  \ Set A = P so the low byte is in the result in A
    
     ORA T                  \ Set A to the correct sign bit that we set in T above
    
    .itsoff
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: KTRAN                                                   [Show more]
           Type: Variable
       Category: Keyboard
        Summary: An unused key logger buffer that's left over from the 6502 Second
                 Processor version of Elite
    
    
        Context: See this variable [on its own page](../main/variable/ktran.md)
     Variations: See [code variations](../related/compare/main/variable/ktran.md) for this variable in the different versions
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .buf
    
     EQUB 2                 \ Transmit 2 bytes as part of this command
    
     EQUB 15                \ Receive 15 bytes as part of this command
    
    .KTRAN
    
     EQUS "1234567890"      \ A 17-byte buffer to hold the key logger data from the
     EQUS "1234567"         \ KEYBOARD routine in the I/O processor (note that only
                            \ 12 of these bytes are actually updated by the KEYBOARD
                            \ routine)
    
    
    
     Save ELTF.bin
    
    
    
    
     PRINT "ELITE F"
     PRINT "Assembled at ", ~CODE_F%
     PRINT "Ends at ", ~P%
     PRINT "Code size is ", ~(P% - CODE_F%)
     PRINT "Execute at ", ~LOAD%
     PRINT "Reload at ", ~LOAD_F%
    
     PRINT "S.ELTF ", ~CODE_F%, " ", ~P%, " ", ~LOAD%, " ", ~LOAD_F%
    \SAVE "3-assembled-output/ELTF.bin", CODE_F%, P%, LOAD%
    
    
    

[X]

Subroutine [ABORT](elite_e.md#header-abort) (category: Dashboard)

Disarm missiles and update the dashboard indicators

[X]

Subroutine [ADD](elite_c.md#header-add) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Variable [ALP1](workspaces.md#alp1) in workspace [ZP](workspaces.md#header-zp)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Variable [ALP2](workspaces.md#alp2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Variable [ALPHA](workspaces.md#alpha) in workspace [ZP](workspaces.md#header-zp)

The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31

[X]

Subroutine [BAD](elite_f.md#header-bad) (category: Status)

Calculate how bad we have been

[X]

Subroutine [BAY](elite_f.md#header-bay) (category: Status)

Go to the docking bay (i.e. show the Status Mode screen)

[X]

Subroutine [BEEP](elite_a.md#header-beep) (category: Sound)

Make a short, high beep

[X]

Label [BEL1](elite_f.md#bel1) in subroutine [BEGIN](elite_f.md#header-begin)

[X]

Subroutine [BELL](elite_b.md#header-bell) (category: Sound)

Make a standard system beep

[X]

Variable [BET1](workspaces.md#bet1) in workspace [ZP](workspaces.md#header-zp)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BET2](workspaces.md#bet2) in workspace [ZP](workspaces.md#header-zp)

Bit 7 of BET2 = sign of the pitch angle in BETA

[X]

Variable [BETA](workspaces.md#beta) in workspace [ZP](workspaces.md#header-zp)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Subroutine [BOOP](elite_a.md#header-boop) (category: Sound)

Make a long, low beep

[X]

Entry point [BOX](elite_a.md#box) in subroutine [TTX66](elite_a.md#header-ttx66) (category: Drawing the screen)

Just draw the border box along the top and sides

[X]

Label [BRBRLOOP](elite_f.md#brbrloop) in subroutine [NEWBRK](elite_f.md#header-newbrk)

[X]

Variable [BSTK](elite_a.md#bstk) in workspace [UP](elite_a.md#header-up)

Bitstik configuration setting

[X]

Subroutine [BUMP2](elite_c.md#header-bump2) (category: Dashboard)

Bump up the value of the pitch or roll dashboard indicator

[X]

Variable [CASH](workspaces.md#cash) in workspace [WP](workspaces.md#header-wp)

Our current cash pot

[X]

Variable [CATF](elite_a.md#catf) in workspace [UP](elite_a.md#header-up)

The disc catalogue flag

[X]

Subroutine [CATS](elite_f.md#header-cats) (category: Save and load)

Ask for a disc drive number and print a catalogue of that drive

[X]

Subroutine [CHECK](elite_f.md#header-check) (category: Save and load)

Calculate the checksum for the last saved commander data block

[X]

Variable [CHK](elite_a.md#header-chk) (category: Save and load)

First checksum byte for the saved commander data file

[X]

Variable [CHK2](elite_a.md#header-chk2) (category: Save and load)

Second checksum byte for the saved commander data file

[X]

Subroutine [CHPR](elite_a.md#header-chpr) (category: Text)

Print a character at the text cursor by poking into screen memory

[X]

Subroutine [CLYNS](elite_a.md#header-clyns) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Variable [CNT2](workspaces.md#cnt2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Variable [COK](workspaces.md#cok) in workspace [WP](workspaces.md#header-wp)

Flags used to generate the competition code

[X]

Variable [COL](workspaces.md#col) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Variable [COMC](elite_a.md#comc) in workspace [UP](elite_a.md#header-up)

The colour of the dot on the compass

[X]

Configuration variable [CON](workspaces.md#con)

Ship type for a Constrictor

[X]

Configuration variable [COPS](workspaces.md#cops)

Ship type for a Viper

[X]

Configuration variable [COU](workspaces.md#cou)

Ship type for a Cougar

[X]

Variable [CTLI](elite_f.md#header-ctli) (category: Save and load)

The OS command string for cataloguing a disc

[X]

Configuration variable [CYAN](workspaces.md#cyan)

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Configuration variable [CYL](workspaces.md#cyl)

Ship type for a Cobra Mk III

[X]

Configuration variable [CYL2](workspaces.md#cyl2)

Ship type for a Cobra Mk III (pirate)

[X]

Label [D1](elite_f.md#d1) in subroutine [DEATH](elite_f.md#header-death)

[X]

Label [D2](elite_f.md#d2) in subroutine [DEATH](elite_f.md#header-death)

[X]

Label [D3](elite_f.md#d3) in subroutine [DEATH](elite_f.md#header-death)

[X]

Variable [DAMP](elite_a.md#damp) in workspace [UP](elite_a.md#header-up)

Keyboard damping configuration setting

[X]

Subroutine [DEATH2](elite_f.md#header-death2) (category: Start and end)

Reset most of the game and restart from the title screen

[X]

Subroutine [DELAY](elite_a.md#header-delay) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Variable [DELI](elite_f.md#header-deli) (category: Save and load)

The OS command string for deleting a file

[X]

Label [DELL1](elite_f.md#dell1) in subroutine [DELT](elite_f.md#header-delt)

[X]

Subroutine [DELT](elite_f.md#header-delt) (category: Save and load)

Catalogue a disc, ask for a filename to delete, and delete the file

[X]

Entry point [DELT-1](elite_f.md#delt) in subroutine [DELT](elite_f.md#header-delt) (category: Save and load)

Contains an RTS

[X]

Variable [DELTA](workspaces.md#delta) in workspace [ZP](workspaces.md#header-zp)

Our current speed, in the range 1-40

[X]

Subroutine [DET1](elite_e.md#header-det1) (category: Drawing the screen)

Show or hide the dashboard (for when we die)

[X]

Subroutine [DETOK](elite_a.md#header-detok) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [DFAULT](elite_f.md#header-dfault) (category: Start and end)

Reset the current commander data block to the last saved commander

[X]

Subroutine [DIALS (Part 1 of 4)](elite_a.md#header-dials-part-1-of-4) (category: Dashboard)

Update the dashboard: speed indicator

[X]

Label [DIDGIT](elite_f.md#didgit) in subroutine [NUMBOR](elite_f.md#header-numbor)

[X]

Variable [DIRI](elite_f.md#header-diri) (category: Save and load)

The OS command string for changing directory on the Master Compact

[X]

Label [DIRL](elite_f.md#dirl) in subroutine [GTDIR](elite_f.md#header-gtdir)

[X]

Variable [DISK](elite_a.md#disk) in workspace [UP](elite_a.md#header-up)

The configuration setting for toggle key "T", which isn't actually used but is still updated by pressing "T" while the game is paused. This is a configuration option from the Commodore 64 version of Elite that lets you switch between tape and disc

[X]

Label [DK11](elite_f.md#dk11) in subroutine [DOKEY](elite_f.md#header-dokey)

[X]

Label [DK12](elite_f.md#dk12) in subroutine [DOKEY](elite_f.md#header-dokey)

[X]

Label [DK13](elite_f.md#dk13) in subroutine [DOKEY](elite_f.md#header-dokey)

[X]

Label [DK14](elite_f.md#dk14) in subroutine [DOKEY](elite_f.md#header-dokey)

[X]

Label [DK15](elite_f.md#dk15) in subroutine [DOKEY](elite_f.md#header-dokey)

[X]

Label [DK152](elite_f.md#dk152) in subroutine [DOKEY](elite_f.md#header-dokey)

[X]

Label [DK2](elite_f.md#dk2) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Subroutine [DK4](elite_f.md#header-dk4) (category: Keyboard)

Scan for pause, configuration and secondary flight keys

[X]

Label [DK6](elite_f.md#dk6) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Label [DK7](elite_f.md#dk7) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Label [DKL4](elite_f.md#dkl4) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Subroutine [DKS3](elite_f.md#header-dks3) (category: Keyboard)

Toggle a configuration setting and emit a beep

[X]

Variable [DLY](workspaces.md#dly) in workspace [WP](workspaces.md#header-wp)

In-flight message delay

[X]

Variable [DNOIZ](elite_a.md#dnoiz) in workspace [UP](elite_a.md#header-up)

Sound on/off configuration setting

[X]

Subroutine [DOCKIT](elite_c.md#header-dockit) (category: Flight)

Apply docking manoeuvres to the ship in INWK

[X]

Subroutine [DORND](elite_f.md#header-dornd) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [DOVDU19](elite_a.md#header-dovdu19) (category: Drawing the screen)

Change the mode 1 palette

[X]

Label [DOVOL1](elite_f.md#dovol1) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Label [DOVOL2](elite_f.md#dovol2) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Label [DOVOL3](elite_f.md#dovol3) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Label [DOVOL4](elite_f.md#dovol4) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Variable [DTW4](elite_b.md#header-dtw4) (category: Text)

Flags that govern how justified extended text tokens are printed

[X]

Variable [DTW5](elite_b.md#header-dtw5) (category: Text)

The size of the justified text buffer at BUF

[X]

Configuration variable [DTW7](elite_b.md#dtw7) in subroutine [MT16](elite_b.md#header-mt16)

Point DTW7 to the second byte of the instruction above so that modifying DTW7 changes the value loaded into A

[X]

Label [DVL2](elite_f.md#dvl2) in subroutine [DVIDT](elite_f.md#header-dvidt)

[X]

Label [Dk3](elite_f.md#dk3) in subroutine [DKS3](elite_f.md#header-dks3)

[X]

Variable [ECMA](workspaces.md#ecma) in workspace [ZP](workspaces.md#header-zp)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Subroutine [ECMOF](elite_h.md#header-ecmof) (category: Dashboard)

Switch off the E.C.M. and turn off the dashboard bulb

[X]

Label [EE20](elite_f.md#ee20) in subroutine [Main game loop (Part 5 of 6)](elite_f.md#header-main-game-loop-part-5-of-6)

[X]

Label [ELT2F](elite_f.md#elt2f) in subroutine [LOD](elite_f.md#header-lod)

[X]

Subroutine [EQSHP](elite_d.md#header-eqshp) (category: Equipment)

Show the Equip Ship screen (red key f3)

[X]

Variable [EV](workspaces.md#ev) in workspace [WP](workspaces.md#header-wp)

The "extra vessels" spawning counter

[X]

Label [FA1](elite_f.md#fa1) in subroutine [FAROF2](elite_f.md#header-farof2)

[X]

Subroutine [FEED](elite_b.md#header-feed) (category: Text)

Print a newline

[X]

Variable [FIST](workspaces.md#fist) in workspace [WP](workspaces.md#header-wp)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Subroutine [FLFLLS](elite_e.md#header-flflls) (category: Drawing suns)

Reset the sun line heap

[X]

Subroutine [FLKB](elite_b.md#header-flkb) (category: Keyboard)

Flush the keyboard buffer

[X]

Entry point [FRCE](elite_f.md#frce) in subroutine [Main game loop (Part 6 of 6)](elite_f.md#header-main-game-loop-part-6-of-6) (category: Main loop)

The entry point for the main game loop if we want to jump straight to a specific screen, by pretending to "press" a key, in which case A contains the internal key number of the key we want to "press"

[X]

Label [FREEZE](elite_f.md#freeze) in subroutine [DK4](elite_f.md#header-dk4)

[X]

Variable [FRIN](workspaces.md#frin) in workspace [WP](workspaces.md#header-wp)

Slots for the ships in the local bubble of universe

[X]

Variable [FSH](workspaces.md#fsh) in workspace [ZP](workspaces.md#header-zp)

Forward shield status

[X]

Variable [GCNT](workspaces.md#gcnt) in workspace [WP](workspaces.md#header-wp)

The number of the current galaxy (0-7)

[X]

Variable [GNTMP](workspaces.md#gntmp) in workspace [WP](workspaces.md#header-wp)

Laser temperature (or "gun temperature")

[X]

Configuration variable [GREEN2](workspaces.md#green2)

Two mode 2 pixels of colour 2 (green)

[X]

Subroutine [GTDIR](elite_f.md#header-gtdir) (category: Save and load)

Fetch the name of an ADFS directory on the Master Compact and change to that directory

[X]

[X]

Subroutine [GTDRV](elite_f.md#header-gtdrv) (category: Save and load)

Get an ASCII disc drive number from the keyboard

[X]

Subroutine [GTHG](elite_d.md#header-gthg) (category: Universe)

Spawn a Thargoid ship and a Thargon companion

[X]

Label [GTL1](elite_f.md#gtl1) in subroutine [TRNME](elite_f.md#header-trnme)

[X]

Label [GTL2](elite_f.md#gtl2) in subroutine [TR1](elite_f.md#header-tr1)

[X]

Label [GTL3](elite_f.md#gtl3) in subroutine [GTNMEW](elite_f.md#header-gtnmew)

[X]

Subroutine [GTNMEW](elite_f.md#header-gtnmew) (category: Save and load)

Fetch the name of a commander file to save or load

[X]

Configuration variable [HER](workspaces.md#her)

Ship type for a rock hermit (asteroid)

[X]

Label [HME1](elite_f.md#hme1) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Subroutine [HME2](elite_b.md#header-hme2) (category: Charts)

Search the galaxy for a system

[X]

Variable [INF](workspaces.md#inf) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Label [INSP](elite_f.md#insp) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Variable [INWK](workspaces.md#inwk) in workspace [ZP](workspaces.md#header-zp)

The zero-page internal workspace for the current ship data block

[X]

Macro [ITEM](elite_f.md#header-item) (category: Market)

Macro definition for the market prices table

[X]

Label [JAMEL1](elite_f.md#jamel1) in subroutine [JAMESON](elite_f.md#header-jameson)

[X]

Subroutine [JAMESON](elite_f.md#header-jameson) (category: Save and load)

Restore the default JAMESON commander

[X]

Configuration variable [JH](workspaces.md#jh)

Junk is defined as ending before the Cobra Mk III

[X]

Configuration variable [JL](workspaces.md#jl)

Junk is defined as starting from the escape pod

[X]

Variable [JOPOS](workspaces.md#jopos) in workspace [WP](workspaces.md#header-wp)

Contains the high byte of the latest value from ADC channel 1 (the joystick X value), which gets updated regularly by the IRQ1 interrupt handler

[X]

Variable [JSTE](elite_a.md#jste) in workspace [UP](elite_a.md#header-up)

Reverse both joystick channels configuration setting

[X]

Variable [JSTGY](elite_a.md#jstgy) in workspace [UP](elite_a.md#header-up)

Reverse joystick Y-channel configuration setting

[X]

Variable [JSTK](elite_a.md#jstk) in workspace [UP](elite_a.md#header-up)

Keyboard or joystick configuration setting

[X]

Variable [JSTX](workspaces.md#jstx) in workspace [ZP](workspaces.md#header-zp)

Our current roll rate

[X]

Variable [JSTY](workspaces.md#jsty) in workspace [ZP](workspaces.md#header-zp)

Our current pitch rate

[X]

Variable [JUNK](workspaces.md#junk) in workspace [WP](workspaces.md#header-wp)

The amount of junk in the local bubble

[X]

Variable [K](workspaces.md#k) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Workspace [K%](workspaces.md#header-k-per-cent) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Variable [K3](workspaces.md#k3) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [KILLSHP](elite_f.md#header-killshp) (category: Universe)

Remove a ship from our local bubble of universe

[X]

Variable [KL](workspaces.md#kl) in workspace [ZP](workspaces.md#header-zp)

The following bytes implement a key logger that enables Elite to scan for concurrent key presses of the primary flight keys, plus a secondary flight key

[X]

Subroutine [KS2](elite_f.md#header-ks2) (category: Universe)

Check the local bubble for missiles with target lock

[X]

Subroutine [KS3](elite_f.md#header-ks3) (category: Universe)

Set the SLSP ship line heap pointer after shuffling ship slots

[X]

Subroutine [KS4](elite_f.md#header-ks4) (category: Universe)

Remove the space station and replace it with the sun

[X]

Label [KS5](elite_f.md#ks5) in subroutine [KILLSHP](elite_f.md#header-killshp)

[X]

Label [KS6](elite_f.md#ks6) in subroutine [KS2](elite_f.md#header-ks2)

[X]

Label [KS7](elite_f.md#ks7) in subroutine [KILLSHP](elite_f.md#header-killshp)

[X]

Label [KSL1](elite_f.md#ksl1) in subroutine [KILLSHP](elite_f.md#header-killshp)

[X]

Label [KSL2](elite_f.md#ksl2) in subroutine [KILLSHP](elite_f.md#header-killshp)

[X]

Label [KSL3](elite_f.md#ksl3) in subroutine [KILLSHP](elite_f.md#header-killshp)

[X]

Label [KSL4](elite_f.md#ksl4) in subroutine [KS2](elite_f.md#header-ks2)

[X]

Variable [KY3](workspaces.md#ky3) in workspace [ZP](workspaces.md#header-zp)

"<" is being pressed (roll left)

[X]

Variable [KY4](workspaces.md#ky4) in workspace [ZP](workspaces.md#header-zp)

">" is being pressed (roll right)

[X]

Variable [KY5](workspaces.md#ky5) in workspace [ZP](workspaces.md#header-zp)

"X" is being pressed (pull up)

[X]

Variable [KY6](workspaces.md#ky6) in workspace [ZP](workspaces.md#header-zp)

"S" is being pressed (pitch down)

[X]

Variable [KY7](workspaces.md#ky7) in workspace [ZP](workspaces.md#header-zp)

"A" is being pressed (fire lasers)

[X]

Label [LABEL_2](elite_f.md#label_2) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Label [LABEL_3](elite_f.md#label_3) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Variable [LASCT](workspaces.md#lasct) in workspace [WP](workspaces.md#header-wp)

The laser pulse count for the current laser

[X]

Subroutine [LL5](elite_g.md#header-ll5) (category: Maths (Arithmetic))

Calculate Q = SQRT(R Q)

[X]

Subroutine [LL9 (Part 1 of 12)](elite_g.md#header-ll9-part-1-of-12) (category: Drawing ships)

Draw ship: Check if ship is exploding, check if ship is in front

[X]

Subroutine [LOD](elite_f.md#header-lod) (category: Save and load)

Load a commander file

[X]

Subroutine [LOOK1](elite_h.md#header-look1) (category: Flight)

Initialise the space view

[X]

Entry point [LOR](elite_f.md#lor) in subroutine [LOD](elite_f.md#header-lod) (category: Save and load)

Set the C flag and return from the subroutine

[X]

Configuration variable [LS%](workspaces.md#ls-per-cent)

The start of the descending ship line heap

[X]

Variable [LSX2](workspaces.md#lsx2) in workspace [WP](workspaces.md#header-wp)

The ball line heap for storing x-coordinates

[X]

Variable [LSY2](workspaces.md#lsy2) in workspace [WP](workspaces.md#header-wp)

The ball line heap for storing y-coordinates

[X]

Entry point [M%](elite_a.md#m-per-cent) in subroutine [Main flight loop (Part 1 of 16)](elite_a.md#header-main-flight-loop-part-1-of-16) (category: Main loop)

The entry point for the main flight loop

[X]

Subroutine [MAD](elite_c.md#header-mad) (category: Maths (Arithmetic))

Calculate (A X) = Q * A + (S R)

[X]

Entry point [MAL1](elite_a.md#mal1) in subroutine [Main flight loop (Part 4 of 16)](elite_a.md#header-main-flight-loop-part-4-of-16) (category: Main loop)

Marks the beginning of the ship analysis loop, so we can jump back here from part 12 of the main flight loop to work our way through each ship in the local bubble. We also jump back here when a ship is removed from the bubble, so we can continue processing from the next ship

[X]

Variable [MANY](workspaces.md#many) in workspace [WP](workspaces.md#header-wp)

The number of ships of each type in the local bubble of universe

[X]

Subroutine [MAS2](elite_b.md#header-mas2) (category: Maths (Geometry))

Calculate a cap on the maximum distance to the planet or sun

[X]

Variable [MCH](workspaces.md#mch) in workspace [WP](workspaces.md#header-wp)

The text token number of the in-flight message that is currently being shown, and which will be removed by the me2 routine when the counter in DLY reaches zero

[X]

Variable [MCNT](workspaces.md#mcnt) in workspace [ZP](workspaces.md#header-zp)

The main loop counter

[X]

Subroutine [MESS](elite_f.md#header-mess) (category: Flight)

Display an in-flight message

[X]

Variable [MJ](workspaces.md#mj) in workspace [WP](workspaces.md#header-wp)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Entry point [MLOOP](elite_f.md#mloop) in subroutine [Main game loop (Part 5 of 6)](elite_f.md#header-main-game-loop-part-5-of-6) (category: Main loop)

The entry point for the main game loop. This entry point comes after the call to the main flight loop and spawning routines, so it marks the start of the main game loop for when we are docked (as we don't need to call the main flight loop or spawning routines if we aren't in space)

[X]

Label [MLOOPS](elite_f.md#mloops) in subroutine [Main game loop (Part 3 of 6)](elite_f.md#header-main-game-loop-part-3-of-6)

[X]

Subroutine [MSBAR](elite_a.md#header-msbar) (category: Dashboard)

Draw a specific indicator in the dashboard's missile bar

[X]

Configuration variable [MSL](workspaces.md#msl)

Ship type for a missile

[X]

Variable [MSTG](workspaces.md#mstg) in workspace [ZP](workspaces.md#header-zp)

The current missile lock target

[X]

Subroutine [MT15](elite_a.md#header-mt15) (category: Text)

Switch to left-aligned text when printing extended tokens

[X]

Subroutine [MT26](elite_f.md#header-mt26) (category: Text)

Fetch a line of text from the keyboard

[X]

Label [MTT1](elite_f.md#mtt1) in subroutine [Main game loop (Part 3 of 6)](elite_f.md#header-main-game-loop-part-3-of-6)

[X]

Label [MTT2](elite_f.md#mtt2) in subroutine [Main game loop (Part 2 of 6)](elite_f.md#header-main-game-loop-part-2-of-6)

[X]

Label [MTT3](elite_f.md#mtt3) in subroutine [Main game loop (Part 2 of 6)](elite_f.md#header-main-game-loop-part-2-of-6)

[X]

Label [MTT4](elite_f.md#mtt4) in subroutine [Main game loop (Part 1 of 6)](elite_f.md#header-main-game-loop-part-1-of-6)

[X]

Subroutine [MULT12](elite_c.md#header-mult12) (category: Maths (Arithmetic))

Calculate (S R) = Q * A

[X]

Subroutine [MVEIT (Part 1 of 9)](elite_h.md#header-mveit-part-1-of-9) (category: Moving)

Move current ship: Tidy the orientation vectors

[X]

Variable [NA%](elite_a.md#header-na-per-cent) (category: Save and load)

The data block for the last saved commander

[X]

Variable [NA2%](elite_a.md#header-na2-per-cent) (category: Save and load)

The data block for the default commander

[X]

Variable [NAEND%](elite_a.md#naend-per-cent) in variable [NA2%](elite_a.md#header-na2-per-cent)

These bytes appear to be unused

[X]

Variable [NAME](workspaces.md#name) in workspace [WP](workspaces.md#header-wp)

The current commander name

[X]

Variable [NEWB](workspaces.md#newb) in workspace [ZP](workspaces.md#header-zp)

The ship's "new byte flags" (or NEWB flags)

[X]

Configuration variable [NI%](workspaces.md#ni-per-cent)

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Subroutine [NMIRELEASE](elite_a.md#header-nmirelease) (category: Utility routines)

Release the NMI workspace (&00A0 to &00A7) so the MOS can use it and store the top part of zero page in the buffer at &3000

[X]

Label [NOCON](elite_f.md#nocon) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Subroutine [NOISE](elite_a.md#header-noise) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Label [NOLASCT](elite_f.md#nolasct) in subroutine [Main game loop (Part 5 of 6)](elite_f.md#header-main-game-loop-part-5-of-6)

[X]

Variable [NOMSL](workspaces.md#nomsl) in workspace [WP](workspaces.md#header-wp)

The number of missiles we have fitted (0-4)

[X]

Subroutine [NORM](elite_f.md#header-norm) (category: Maths (Geometry))

Normalise the three-coordinate vector in XX15

[X]

Configuration variable [NOST](workspaces.md#nost)

The number of stardust particles in normal space (this goes down to 3 in witchspace)

[X]

Variable [NOSTM](workspaces.md#nostm) in workspace [ZP](workspaces.md#header-zp)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Configuration variable [NT%](workspaces.md#nt-per-cent) in workspace [WP](workspaces.md#header-wp)

This sets the variable NT% to the size of the current commander data block, which starts at TP and ends at SVC+3 (inclusive), i.e. with the last checksum byte

[X]

Label [NWDAV5](elite_f.md#nwdav5) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Subroutine [NWSHP](elite_e.md#header-nwshp) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Configuration variable [OIL](workspaces.md#oil)

Ship type for a cargo canister

[X]

Configuration variable [OSCLI](workspaces.md#oscli)

The address for the OSCLI routine

[X]

Label [OSW01](elite_f.md#osw01) in subroutine [MT26](elite_f.md#header-mt26)

[X]

Label [OSW03](elite_f.md#osw03) in subroutine [MT26](elite_f.md#header-mt26)

[X]

Label [OSW04](elite_f.md#osw04) in subroutine [MT26](elite_f.md#header-mt26)

[X]

Label [OSW05](elite_f.md#osw05) in subroutine [MT26](elite_f.md#header-mt26)

[X]

Label [OSW06](elite_f.md#osw06) in subroutine [MT26](elite_f.md#header-mt26)

[X]

Label [OSW0L](elite_f.md#osw0l) in subroutine [MT26](elite_f.md#header-mt26)

[X]

Variable [P](workspaces.md#p) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [PACK](workspaces.md#pack)

The first of the eight pack-hunter ships, which tend to spawn in groups. With the default value of PACK the pack-hunters are the Sidewinder, Mamba, Krait, Adder, Gecko, Cobra Mk I, Worm and Cobra Mk III (pirate)

[X]

Variable [PATG](elite_a.md#patg) in workspace [UP](elite_a.md#header-up)

Configuration setting to show the author names on the start-up screen and enable manual hyperspace mis-jumps

[X]

Configuration variable [PLT](workspaces.md#plt)

Ship type for an alloy plate

[X]

Variable [Q](workspaces.md#q) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [QQ0](workspaces.md#qq0) in workspace [WP](workspaces.md#header-wp)

The current system's galactic x-coordinate (0-256)

[X]

Variable [QQ1](workspaces.md#qq1) in workspace [WP](workspaces.md#header-wp)

The current system's galactic y-coordinate (0-256)

[X]

Variable [QQ11](workspaces.md#qq11) in workspace [ZP](workspaces.md#header-zp)

The type of the current view

[X]

Variable [QQ12](workspaces.md#qq12) in workspace [ZP](workspaces.md#header-zp)

Our "docked" status

[X]

Variable [QQ15](workspaces.md#qq15) in workspace [ZP](workspaces.md#header-zp)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ17](workspaces.md#qq17) in workspace [ZP](workspaces.md#header-zp)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Variable [QQ2](workspaces.md#qq2) in workspace [WP](workspaces.md#header-wp)

The three 16-bit seeds for the current system, i.e. the one we are currently in

[X]

Variable [QQ20](workspaces.md#qq20) in workspace [WP](workspaces.md#header-wp)

The contents of our cargo hold

[X]

Variable [QQ22](workspaces.md#qq22) in workspace [ZP](workspaces.md#header-zp)

The two hyperspace countdown counters

[X]

Variable [QQ28](workspaces.md#qq28) in workspace [WP](workspaces.md#header-wp)

The current system's economy (0-7)

[X]

Variable [QQ3](workspaces.md#qq3) in workspace [ZP](workspaces.md#header-zp)

The selected system's economy (0-7)

[X]

Variable [QQ4](workspaces.md#qq4) in workspace [ZP](workspaces.md#header-zp)

The selected system's government (0-7)

[X]

Variable [QQ5](workspaces.md#qq5) in workspace [ZP](workspaces.md#header-zp)

The selected system's tech level (0-14)

[X]

Entry point [QU5](elite_f.md#qu5) in subroutine [BR1 (Part 1 of 2)](elite_f.md#header-br1-part-1-of-2) (category: Start and end)

Restart the game using the last saved commander without asking whether to load a new commander file

[X]

Label [QUL1](elite_f.md#qul1) in subroutine [DFAULT](elite_f.md#header-dfault)

[X]

Label [QUL2](elite_f.md#qul2) in subroutine [CHECK](elite_f.md#header-check)

[X]

Variable [R](workspaces.md#r) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [RAND](workspaces.md#rand) in workspace [ZP](workspaces.md#header-zp)

Four 8-bit seeds for the random number generation system implemented in the DORND routine

[X]

Subroutine [RDFIRE](elite_h.md#header-rdfire) (category: Keyboard)

Read the fire button on either the analogue or digital joystick

[X]

Subroutine [RDJOY](elite_h.md#header-rdjoy) (category: Keyboard)

Read from either the analogue or digital joystick

[X]

Subroutine [RDKEY](elite_h.md#header-rdkey) (category: Keyboard)

Scan the keyboard for key presses and update the key logger

[X]

Entry point [RDKEY-1](elite_h.md#rdkey) in subroutine [RDKEY](elite_h.md#header-rdkey) (category: Keyboard)

Only scan the keyboard for valid BCD key numbers

[X]

Configuration variable [RED](workspaces.md#red)

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Subroutine [REDU2](elite_c.md#header-redu2) (category: Dashboard)

Reduce the value of the pitch or roll dashboard indicator

[X]

Label [REL5](elite_f.md#rel5) in subroutine [RESET](elite_f.md#header-reset)

[X]

Subroutine [RES2](elite_f.md#header-res2) (category: Start and end)

Reset a number of flight variables and workspaces

[X]

Subroutine [RESET](elite_f.md#header-reset) (category: Start and end)

Reset most variables

[X]

Variable [RLINE](elite_f.md#header-rline) (category: Text)

The OSWORD configuration block used to fetch a line of text from the keyboard

[X]

Variable [S](workspaces.md#s) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Label [SAL3](elite_f.md#sal3) in subroutine [RESET](elite_f.md#header-reset)

[X]

Label [SAL8](elite_f.md#sal8) in subroutine [msblob](elite_f.md#header-msblob)

[X]

Variable [SC](workspaces.md#sc) in workspace [ZP](workspaces.md#header-zp)

Screen address (low byte)

[X]

Subroutine [SHIFT](elite_a.md#header-shift) (category: Keyboard)

Scan the keyboard to see if SHIFT is currently pressed

[X]

Variable [SLSP](workspaces.md#slsp) in workspace [WP](workspaces.md#header-wp)

The address of the bottom of the ship line heap

[X]

Subroutine [SPBLB](elite_a.md#header-spblb) (category: Dashboard)

Light up the space station indicator ("S") on the dashboard

[X]

Subroutine [SPS3](elite_e.md#header-sps3) (category: Maths (Geometry))

Copy a space coordinate from the K% block into K3

[X]

Subroutine [SQUA](elite_c.md#header-squa) (category: Maths (Arithmetic))

Clear bit 7 of A and calculate (A P) = A * A

[X]

Variable [SSPR](workspaces.md#sspr) in workspace [WP](workspaces.md#header-wp)

"Space station present" flag

[X]

Configuration variable [SST](workspaces.md#sst)

Ship type for a Coriolis space station

[X]

Subroutine [STATUS](elite_b.md#header-status) (category: Status)

Show the Status Mode screen (red key f8)

[X]

Label [SV1](elite_f.md#sv1) in subroutine [SVE](elite_f.md#header-sve)

[X]

Variable [SVC](workspaces.md#svc) in workspace [WP](workspaces.md#header-wp)

The save count

[X]

Subroutine [SVE](elite_f.md#header-sve) (category: Save and load)

Display the disk access menu and process saving of commander files

[X]

Label [SVEX9](elite_f.md#svex9) in subroutine [SVE](elite_f.md#header-sve)

[X]

Label [SVL1](elite_f.md#svl1) in subroutine [SVE](elite_f.md#header-sve)

[X]

Variable [T](workspaces.md#t) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [T1](workspaces.md#t1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Entry point [T95](elite_f.md#t95) in subroutine [TT102](elite_f.md#header-tt102) (category: Keyboard)

Print the distance to the selected system

[X]

Entry point [TA2](elite_f.md#ta2) in subroutine [TAS2](elite_f.md#header-tas2) (category: Maths (Geometry))

Calculate the length of the vector in XX15 (ignoring the low coordinates), returning it in Q

[X]

Label [TAL2](elite_f.md#tal2) in subroutine [TAS2](elite_f.md#header-tas2)

[X]

Variable [TALLY](workspaces.md#tally) in workspace [WP](workspaces.md#header-wp)

Our combat rank

[X]

Configuration variable [TAP%](workspaces.md#tap-per-cent)

The staging area where we copy files after loading and before saving (though this isn't actually used in this version, and is left-over Commodore 64 code)

[X]

Variable [TGINT](elite_a.md#header-tgint) (category: Keyboard)

The keys used to toggle configuration settings when the game is paused

[X]

Subroutine [THERE](elite_f.md#header-there) (category: Missions)

Check whether we are in the Constrictor's system in mission 1

[X]

Label [THEX](elite_f.md#thex) in subroutine [THERE](elite_f.md#header-there)

[X]

[X]

Label [TI1](elite_f.md#ti1) in subroutine [TIDY](elite_f.md#header-tidy)

[X]

Label [TI2](elite_f.md#ti2) in subroutine [TIDY](elite_f.md#header-tidy)

[X]

Label [TI3](elite_f.md#ti3) in subroutine [TIDY](elite_f.md#header-tidy)

[X]

Label [TI4](elite_f.md#ti4) in subroutine [TIS2](elite_f.md#header-tis2)

[X]

Label [TIL1](elite_f.md#til1) in subroutine [TIDY](elite_f.md#header-tidy)

[X]

Label [TIL2](elite_f.md#til2) in subroutine [TIS2](elite_f.md#header-tis2)

[X]

Subroutine [TIS1](elite_c.md#header-tis1) (category: Maths (Arithmetic))

Calculate (A ?) = (-X * A + (S R)) / 96

[X]

Subroutine [TIS2](elite_f.md#header-tis2) (category: Maths (Arithmetic))

Calculate A = A / Q

[X]

Subroutine [TIS3](elite_f.md#header-tis3) (category: Maths (Arithmetic))

Calculate -(nosev_1 * roofv_1 + nosev_2 * roofv_2) / nosev_3

[X]

Subroutine [TITLE](elite_f.md#header-title) (category: Start and end)

Display a title screen with a rotating ship and prompt

[X]

Entry point [TJ1](elite_e.md#tj1) in subroutine [TT17](elite_e.md#header-tt17) (category: Keyboard)

Check for cursor key presses and return the combined deltas for the digital joystick and cursor keys (Master Compact only)

[X]

Label [TL1](elite_f.md#tl1) in subroutine [TITLE](elite_f.md#header-title)

[X]

Label [TL3](elite_f.md#tl3) in subroutine [TITLE](elite_f.md#header-title)

[X]

Label [TLL2](elite_f.md#tll2) in subroutine [TITLE](elite_f.md#header-title)

[X]

Variable [TP](workspaces.md#tp) in workspace [WP](workspaces.md#header-wp)

The current mission status

[X]

Subroutine [TR1](elite_f.md#header-tr1) (category: Save and load)

Copy the last saved commander's name from NA% to INWK

[X]

Entry point [TRADEMODE2](elite_d.md#trademode2) in subroutine [TRADEMODE](elite_d.md#header-trademode) (category: Drawing the screen)

Set the palette for trading screens and switch the current colour to white

[X]

Subroutine [TRNME](elite_f.md#header-trnme) (category: Save and load)

Copy the last saved commander's name from INWK to NA%

[X]

Entry point [TT100](elite_f.md#tt100) in subroutine [Main game loop (Part 2 of 6)](elite_f.md#header-main-game-loop-part-2-of-6) (category: Main loop)

The entry point for the start of the main game loop, which calls the main flight loop and the moves into the spawning routine

[X]

Subroutine [TT102](elite_f.md#header-tt102) (category: Keyboard)

Process function key, save key, hyperspace and chart key presses and update the hyperspace counter

[X]

Subroutine [TT103](elite_d.md#header-tt103) (category: Charts)

Draw a small set of crosshairs on a chart

[X]

Label [TT107](elite_f.md#tt107) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Subroutine [TT110](elite_d.md#header-tt110) (category: Flight)

Launch from a station or show the front space view

[X]

Subroutine [TT111](elite_d.md#header-tt111) (category: Universe)

Set the current system to the nearest system to a point

[X]

Subroutine [TT146](elite_d.md#header-tt146) (category: Universe)

Print the distance to the selected system in light years

[X]

Subroutine [TT16](elite_d.md#header-tt16) (category: Charts)

Move the crosshairs on a chart

[X]

Subroutine [TT167](elite_d.md#header-tt167) (category: Market)

Show the Market Price screen (red key f7)

[X]

Subroutine [TT17](elite_e.md#header-tt17) (category: Keyboard)

Scan the keyboard for cursor key or joystick movement

[X]

Subroutine [TT17X](elite_h.md#header-tt17x) (category: Keyboard)

Scan the digital joystick for movement

[X]

Subroutine [TT18](elite_d.md#header-tt18) (category: Flight)

Try to initiate a jump into hyperspace

[X]

Subroutine [TT208](elite_d.md#header-tt208) (category: Market)

Show the Sell Cargo screen (red key f2)

[X]

Subroutine [TT213](elite_d.md#header-tt213) (category: Market)

Show the Inventory screen (red key f9)

[X]

Subroutine [TT217](elite_f.md#header-tt217) (category: Keyboard)

Scan the keyboard until a key is pressed

[X]

Subroutine [TT219](elite_d.md#header-tt219) (category: Market)

Show the Buy Cargo screen (red key f1)

[X]

Subroutine [TT22](elite_d.md#header-tt22) (category: Charts)

Show the Long-range Chart (red key f4)

[X]

Subroutine [TT23](elite_d.md#header-tt23) (category: Charts)

Show the Short-range Chart (red key f5)

[X]

Subroutine [TT25](elite_d.md#header-tt25) (category: Universe)

Show the Data on System screen (red key f6)

[X]

Subroutine [TT26](elite_b.md#header-tt26) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [TT27](elite_e.md#header-tt27) (category: Text)

Print a text token

[X]

Subroutine [TT66](elite_h.md#header-tt66) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Subroutine [TT67](elite_d.md#header-tt67) (category: Text)

Print a newline

[X]

Label [TT92](elite_f.md#tt92) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Variable [TYPE](workspaces.md#type) in workspace [ZP](workspaces.md#header-zp)

The current ship type

[X]

Variable [UNIV](elite_b.md#header-univ) (category: Universe)

Table of pointers to the local universe's ship data blocks

[X]

Configuration variable [VIA](workspaces.md#via)

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [VIEW](workspaces.md#view) in workspace [WP](workspaces.md#header-wp)

The number of the current space view

[X]

Variable [VOL](elite_a.md#vol) in workspace [UP](elite_a.md#header-up)

The volume level for the game's sound effects (0-7)

[X]

Label [WA1](elite_f.md#wa1) in subroutine [WARP](elite_f.md#header-warp)

[X]

Label [WA2](elite_f.md#wa2) in subroutine [WARP](elite_f.md#header-warp)

[X]

Label [WA3](elite_f.md#wa3) in subroutine [WARP](elite_f.md#header-warp)

[X]

Subroutine [WPSHPS](elite_e.md#header-wpshps) (category: Dashboard)

Clear the scanner, reset the ball line and sun line heaps

[X]

Subroutine [WSCAN](elite_a.md#header-wscan) (category: Drawing the screen)

Implement the #wscn command (wait for the vertical sync)

[X]

Variable [XC](workspaces.md#xc) in workspace [ZP](workspaces.md#header-zp)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [XSAV](workspaces.md#xsav) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX0](workspaces.md#xx0) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX13](workspaces.md#xx13) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used in the line-drawing routines

[X]

Variable [XX15](workspaces.md#xx15) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Configuration variable [XX21](workspaces.md#xx21)

The address of the ship blueprints lookup table, as set in elite-data.asm

[X]

Variable [XX4](workspaces.md#xx4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [Y](workspaces.md#y)

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [YC](workspaces.md#yc) in workspace [ZP](workspaces.md#header-zp)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Configuration variable [YELLOW](workspaces.md#yellow)

Four mode 1 pixels of colour 1 (yellow)

[X]

Label [YESCON](elite_f.md#yescon) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Subroutine [YESNO](elite_e.md#header-yesno) (category: Keyboard)

Wait until either "Y" or "N" is pressed

[X]

Variable [YSAV](workspaces.md#ysav) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for saving the value of the Y register, used in a number of places

[X]

Variable [Yx2M1](workspaces.md#yx2m1) in workspace [ZP](workspaces.md#header-zp)

This is used to store the number of pixel rows in the space view minus 1, which is also the y-coordinate of the bottom pixel row of the space view (it is set to 191 in the RES2 routine)

[X]

Subroutine [ZEKTRAN](elite_h.md#header-zektran) (category: Keyboard)

Clear the key logger

[X]

Label [ZEL2](elite_f.md#zel2) in subroutine [ZERO](elite_f.md#header-zero)

[X]

Subroutine [ZERO](elite_f.md#header-zero) (category: Utility routines)

Reset the local bubble of universe and ship status

[X]

Label [ZI1](elite_f.md#zi1) in subroutine [ZINF](elite_f.md#header-zinf)

[X]

Subroutine [ZINF](elite_f.md#header-zinf) (category: Universe)

Reset the INWK workspace and orientation vectors

[X]

Subroutine [Ze](elite_f.md#header-ze) (category: Universe)

Initialise the INWK workspace to a fairly aggressive ship

[X]

Variable [auto](workspaces.md#auto) in workspace [WP](workspaces.md#header-wp)

Docking computer activation status

[X]

Label [awe](elite_f.md#awe) in subroutine [TITLE](elite_f.md#header-title)

[X]

Label [blacksuspenders](elite_f.md#blacksuspenders) in subroutine [KILLSHP](elite_f.md#header-killshp)

[X]

Label [chview1](elite_f.md#chview1) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Label [chview2](elite_f.md#chview2) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Label [clynsneed](elite_f.md#clynsneed) in subroutine [me2](elite_f.md#header-me2)

[X]

Configuration variable [commbuf](workspaces.md#commbuf)

The file buffer where we load and save commander files (this shares a location with LSX2 and is the address used in the *SAVE and *LOAD OS commands)

[X]

Label [copyme](elite_f.md#copyme) in subroutine [LOD](elite_f.md#header-lod)

[X]

Label [copyme2](elite_f.md#copyme2) in subroutine [SVE](elite_f.md#header-sve)

[X]

Subroutine [cpl](elite_e.md#header-cpl) (category: Universe)

Print the selected system name

[X]

Variable [de](workspaces.md#de) in workspace [WP](workspaces.md#header-wp)

Equipment destruction flag

[X]

Variable [distaway](workspaces.md#distaway) in workspace [WP](workspaces.md#header-wp)

Used to store the nearest distance of the rotating ship on the title screen

[X]

Label [doitagain](elite_f.md#doitagain) in subroutine [DFAULT](elite_f.md#header-dfault)

[X]

Variable [dontclip](workspaces.md#dontclip) in workspace [ZP](workspaces.md#header-zp)

This is set to 0 in the RES2 routine, but the value is never actually read (this is left over from the Commodore 64 version of Elite)

[X]

Configuration variable [e](elite_f.md#e) in macro [ITEM](elite_f.md#header-item)

[X]

Label [ee2](elite_f.md#ee2) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Subroutine [ee3](elite_d.md#header-ee3) (category: Flight)

Print the hyperspace countdown in the top-left of the screen

[X]

Subroutine [ex](elite_e.md#header-ex) (category: Text)

Print a recursive token

[X]

Configuration variable [f0](workspaces.md#f0)

Internal key number for red key f0 (Launch, Front)

[X]

Configuration variable [f1](workspaces.md#f1)

Internal key number for red key f1 (Buy Cargo, Rear)

[X]

Configuration variable [f2](workspaces.md#f2)

Internal key number for red key f2 (Sell Cargo, Left)

[X]

Configuration variable [f3](workspaces.md#f3)

Internal key number for red key f3 (Equip Ship, Right)

[X]

Configuration variable [f4](workspaces.md#f4)

Internal key number for red key f4 (Long-range Chart)

[X]

Configuration variable [f5](workspaces.md#f5)

Internal key number for red key f5 (Short-range Chart)

[X]

Configuration variable [f6](workspaces.md#f6)

Internal key number for red key f6 (Data on System)

[X]

Configuration variable [f7](workspaces.md#f7)

Internal key number for red key f7 (Market Price)

[X]

Configuration variable [f8](workspaces.md#f8)

Internal key number for red key f8 (Status Mode)

[X]

Configuration variable [f9](workspaces.md#f9)

Internal key number for red key f9 (Inventory)

[X]

Label [feb10](elite_f.md#feb10) in subroutine [SVE](elite_f.md#header-sve)

[X]

Label [feb13](elite_f.md#feb13) in subroutine [SVE](elite_f.md#header-sve)

[X]

Label [focoug](elite_f.md#focoug) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Label [fothg](elite_f.md#fothg) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Label [fothg2](elite_f.md#fothg2) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Entry point [fq1](elite_c.md#fq1) in subroutine [FRS1](elite_c.md#header-frs1) (category: Tactics)

Used to add a cargo canister to the universe

[X]

Label [fvw](elite_f.md#fvw) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Subroutine [getzp](elite_a.md#header-getzp) (category: Utility routines)

Swap zero page (&0090 to &00EF) with the buffer at &3000

[X]

Variable [gov](workspaces.md#gov) in workspace [WP](workspaces.md#header-wp)

The current system's government type (0-7)

[X]

Subroutine [hm](elite_d.md#header-hm) (category: Charts)

Select the closest system and redraw the chart crosshairs

[X]

Subroutine [hyp](elite_d.md#header-hyp) (category: Flight)

Start the hyperspace process

[X]

Label [infrontvw](elite_f.md#infrontvw) in subroutine [MESS](elite_f.md#header-mess)

[X]

Label [jan18](elite_f.md#jan18) in subroutine [SVE](elite_f.md#header-sve)

[X]

Label [jan2186](elite_f.md#jan2186) in subroutine [SVE](elite_f.md#header-sve)

[X]

Subroutine [jmp](elite_d.md#header-jmp) (category: Universe)

Set the current system to the selected system

[X]

Label [likeTT112](elite_f.md#likett112) in subroutine [BR1 (Part 2 of 2)](elite_f.md#header-br1-part-2-of-2)

[X]

Label [lll](elite_f.md#lll) in subroutine [KILLSHP](elite_f.md#header-killshp)

[X]

Label [loading](elite_f.md#loading) in subroutine [SVE](elite_f.md#header-sve)

[X]

Variable [lodosc](elite_f.md#header-lodosc) (category: Save and load)

The OS command string for loading a commander file

[X]

Entry point [m](elite_b.md#m) in subroutine [MAS2](elite_b.md#header-mas2) (category: Maths (Geometry))

Do not include A in the calculation

[X]

[X]

Subroutine [me1](elite_f.md#header-me1) (category: Flight)

Erase an old in-flight message and display a new one

[X]

Subroutine [me2](elite_f.md#header-me2) (category: Flight)

Remove an in-flight message from the space view

[X]

Entry point [me3](elite_f.md#me3) in subroutine [Main game loop (Part 2 of 6)](elite_f.md#header-main-game-loop-part-2-of-6) (category: Main loop)

Used by me2 to jump back into the main game loop after printing an in-flight message

[X]

Subroutine [mes9](elite_f.md#header-mes9) (category: Flight)

Print a text token, possibly followed by " DESTROYED"

[X]

Variable [messXC](workspaces.md#messxc) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the text column of the in-flight message in MESS, so it can be erased from the screen at the correct time

[X]

Subroutine [msblob](elite_f.md#header-msblob) (category: Dashboard)

Display the dashboard's missile indicators in green

[X]

Label [mt1](elite_f.md#mt1) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Label [mt3](elite_f.md#mt3) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Subroutine [nWq](elite_e.md#header-nwq) (category: Stardust)

Create a random cloud of stardust

[X]

Label [nodo](elite_f.md#nodo) in subroutine [Main game loop (Part 1 of 6)](elite_f.md#header-main-game-loop-part-1-of-6)

[X]

Label [nopl](elite_f.md#nopl) in subroutine [Main game loop (Part 4 of 6)](elite_f.md#header-main-game-loop-part-4-of-6)

[X]

Label [nosave](elite_f.md#nosave) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Label [noshift](elite_f.md#noshift) in subroutine [MT26](elite_f.md#header-mt26)

[X]

Variable [oldlong](elite_f.md#header-oldlong) (category: Save and load)

Contains the length of the last saved commander name

[X]

Label [ou1](elite_f.md#ou1) in subroutine [OUCH](elite_f.md#header-ouch)

[X]

Subroutine [ou2](elite_f.md#header-ou2) (category: Flight)

Display "E.C.M.SYSTEM DESTROYED" as an in-flight message

[X]

Subroutine [ou3](elite_f.md#header-ou3) (category: Flight)

Display "FUEL SCOOPS DESTROYED" as an in-flight message

[X]

Entry point [out](elite_f.md#out) in subroutine [TT217](elite_f.md#header-tt217) (category: Keyboard)

Contains an RTS

[X]

Subroutine [ping](elite_c.md#header-ping) (category: Universe)

Set the selected system to the current system

[X]

Subroutine [plf](elite_e.md#header-plf) (category: Text)

Print a text token followed by a newline

[X]

Label [plus13](elite_f.md#plus13) in subroutine [Main game loop (Part 5 of 6)](elite_f.md#header-main-game-loop-part-5-of-6)

[X]

[X]

[X]

Subroutine [rfile](elite_f.md#header-rfile) (category: Save and load)

Load the commander file

[X]

Label [rfileL1](elite_f.md#rfilel1) in subroutine [rfile](elite_f.md#header-rfile)

[X]

Label [rfileL3](elite_f.md#rfilel3) in subroutine [rfile](elite_f.md#header-rfile)

[X]

Label [rfileL4](elite_f.md#rfilel4) in subroutine [rfile](elite_f.md#header-rfile)

[X]

Configuration variable [s](elite_f.md#s) in macro [ITEM](elite_f.md#header-item)

[X]

Variable [savosc](elite_f.md#header-savosc) (category: Save and load)

The OS command string for saving a commander file

[X]

Configuration variable [soexpl](workspaces.md#soexpl)

Sound 4 = We died / Collision / Being hit by lasers 2

[X]

Variable [spasto](elite_f.md#header-spasto) (category: Universe)

Contains the address of the Coriolis space station's ship blueprint

[X]

Label [ss](elite_f.md#ss) in subroutine [msblob](elite_f.md#header-msblob)

[X]

Variable [stackpt](elite_f.md#header-stackpt) (category: Save and load)

Temporary storage for the stack pointer when jumping to the break handler at NEWBRK

[X]

Entry point [t](elite_f.md#t) in subroutine [TT217](elite_f.md#header-tt217) (category: Keyboard)

As TT217 but don't preserve Y, set it to YSAV instead

[X]

Label [t2](elite_f.md#t2) in subroutine [TT217](elite_f.md#header-tt217)

[X]

Label [t95](elite_f.md#t95) in subroutine [TT102](elite_f.md#header-tt102)

[X]

Label [tZ](elite_f.md#tz) in subroutine [DFAULT](elite_f.md#header-dfault)

[X]

Variable [tek](workspaces.md#tek) in workspace [WP](workspaces.md#header-wp)

The current system's tech level (0-14)

[X]

Variable [thislong](elite_f.md#header-thislong) (category: Save and load)

Contains the length of the most recently entered commander name

[X]

Label [thongs](elite_f.md#thongs) in subroutine [Main game loop (Part 2 of 6)](elite_f.md#header-main-game-loop-part-2-of-6)

[X]

Configuration variable [u](elite_f.md#u) in macro [ITEM](elite_f.md#header-item)

[X]

Subroutine [wfile](elite_f.md#header-wfile) (category: Save and load)

Save the commander file

[X]

Label [wfileL1](elite_f.md#wfilel1) in subroutine [wfile](elite_f.md#header-wfile)

[X]

Label [wfileL2](elite_f.md#wfilel2) in subroutine [wfile](elite_f.md#header-wfile)

[X]

Label [wfileL3](elite_f.md#wfilel3) in subroutine [wfile](elite_f.md#header-wfile)

[X]

Label [wfileL4](elite_f.md#wfilel4) in subroutine [wfile](elite_f.md#header-wfile)

[X]

Label [whips](elite_f.md#whips) in subroutine [Main game loop (Part 2 of 6)](elite_f.md#header-main-game-loop-part-2-of-6)

[X]

Label [ytq](elite_f.md#ytq) in subroutine [Main game loop (Part 2 of 6)](elite_f.md#header-main-game-loop-part-2-of-6)

[X]

Label [yu](elite_f.md#yu) in subroutine [RES2](elite_f.md#header-res2)

[Elite E source](elite_e.md "Previous routine")[Elite G source](elite_g.md "Next routine")
