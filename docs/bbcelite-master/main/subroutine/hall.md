---
title: "The HALL subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hall.html"
---

[HATB](../variable/hatb.md "Previous routine")[HAS1](has1.md "Next routine")
    
    
           Name: HALL                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw the ships in the ship hangar, then draw the hangar
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-hall)
     Variations: See [code variations](../../related/compare/main/subroutine/hall.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOENTRY](doentry.md) calls HALL
    
    
    
    
    
    * * *
    
    
     Half the time this will draw one of the four pre-defined ship hangar groups in
     HATB, and half the time this will draw a solitary Sidewinder, Mamba, Krait or
     Adder on a random position. In all cases, the ships will be randomly spun
     around on the ground so they can face in any direction, and larger ships are
     drawn higher up off the ground than smaller ships.
    
    
    
    
    .HALL
    
     LDA #0                 \ Switch to the mode 1 palette for the space view,
     JSR DOVDU19            \ which is yellow (colour 1), red (colour 2) and cyan
                            \ (colour 3)
    
     LDA #0                 \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 0 (space
                            \ view)
    
     JSR DORND              \ Set A and X to random numbers
    
     BPL HA7                \ Jump to HA7 if A is positive (50% chance)
    
     AND #3                 \ Reduce A to a random number in the range 0-3
    
     STA T                  \ Set X = A * 8 + A
     ASL A                  \       = 9 * A
     ASL A                  \
     ASL A                  \ so X is a random number, either 0, 9, 18 or 27
     ADC T
     TAX
    
                            \ The following double loop calls the HAS1 routine three
                            \ times to display three ships on screen. For each call,
                            \ the values passed to HAS1 in XX15+2 to XX15 are taken
                            \ from the HATB table, depending on the value in X, as
                            \ follows:
                            \
                            \   * If X = 0,  pass bytes #0 to #2 of HATB to HAS1
                            \                then bytes #3 to #5
                            \                then bytes #6 to #8
                            \
                            \   * If X = 9,  pass bytes  #9 to #11 of HATB to HAS1
                            \                then bytes #12 to #14
                            \                then bytes #15 to #17
                            \
                            \   * If X = 18, pass bytes #18 to #20 of HATB to HAS1
                            \                then bytes #21 to #23
                            \                then bytes #24 to #26
                            \
                            \   * If X = 27, pass bytes #27 to #29 of HATB to HAS1
                            \                then bytes #30 to #32
                            \                then bytes #33 to #35
                            \
                            \ Note that the values are passed in reverse, so for the
                            \ first call, for example, where we pass bytes #0 to #2
                            \ of HATB to HAS1, we call HAS1 with:
                            \
                            \   XX15   = HATB+2
                            \   XX15+1 = HATB+1
                            \   XX15+2 = HATB
    
     LDY #3                 \ Set CNT2 = 3 to act as an outer loop counter going
     STY CNT2               \ from 3 to 1, so the HAL8 loop is run 3 times
    
    .HAL8
    
     LDY #2                 \ Set Y = 2 to act as an inner loop counter going from
                            \ 2 to 0
    
    .HAL9
    
     LDA HATB,X             \ Copy the X-th byte of HATB to the Y-th byte of XX15,
     STA XX15,Y             \ as described above
    
     INX                    \ Increment X to point to the next byte in HATB
    
     DEY                    \ Decrement Y to point to the previous byte in XX15
    
     BPL HAL9               \ Loop back to copy the next byte until we have copied
                            \ three of them (i.e. Y was 3 before the DEY)
    
     TXA                    \ Store X on the stack so we can retrieve it after the
     PHA                    \ call to HAS1 (as it contains the index of the next
                            \ byte in HATB
    
     JSR HAS1               \ Call HAS1 to draw this ship in the hangar
    
     PLA                    \ Restore the value of X, so X points to the next byte
     TAX                    \ in HATB after the three bytes we copied into XX15
    
     DEC CNT2               \ Decrement the outer loop counter in CNT2
    
     BNE HAL8               \ Loop back to HAL8 to do it 3 times, once for each ship
                            \ in the HATB table
    
     LDY #128               \ Set Y = 128 to send as byte #2 of the parameter block
                            \ to the OSWORD 248 command below, to tell the I/O
                            \ processor that there are multiple ships in the hangar
    
     BNE HA9                \ Jump to HA9 to display the ship hangar (this BNE is
                            \ effectively a JMP as Y is never zero)
    
    .HA7
    
                            \ If we get here, A is a positive random number in the
                            \ range 0-127
    
     LSR A                  \ Set XX15+1 = A / 2 (random number 0-63)
     STA XX15+1
    
     JSR DORND              \ Set XX15 = random number 0-255
     STA XX15
    
     JSR DORND              \ Set XX15+2 = #SH3 + random number 0-3
     AND #3                 \
     ADC #SH3               \ which is the ship type of a Sidewinder, Mamba, Krait
     STA XX15+2             \ or Adder
    
     JSR HAS1               \ Call HAS1 to draw this ship in the hangar, with the
                            \ following properties:
                            \
                            \   * Random x-coordinate from -63 to +63
                            \
                            \   * Randomly chosen Sidewinder, Mamba, Krait or Adder
                            \
                            \   * Random z-coordinate from +256 to +639
    
     LDY #0                 \ Set Y = 0 to use in the following instruction, to tell
                            \ the hangar-drawing routine that there is just one ship
                            \ in the hangar, so it knows not to draw between the
                            \ ships
    
    .HA9
    
     STY HANGFLAG           \ Store Y in HANGFLAG to specify whether there are
                            \ multiple ships in the hangar
    
     JMP HANGER             \ Call HANGER to draw the hangar background and return
                            \ from the subroutine using a tail call
    

[X]

Variable [CNT2](../workspace/zp.md#cnt2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [DOVDU19](dovdu19.md) (category: Drawing the screen)

Change the mode 1 palette

[X]

Label [HA7](hall.md#ha7) is local to this routine

[X]

Label [HA9](hall.md#ha9) is local to this routine

[X]

Label [HAL8](hall.md#hal8) is local to this routine

[X]

Label [HAL9](hall.md#hal9) is local to this routine

[X]

Subroutine [HANGER](hanger.md) (category: Ship hangar)

Display the ship hangar

[X]

Variable [HANGFLAG](../variable/hangflag.md) (category: Ship hangar)

The number of ships being displayed in the ship hangar

[X]

Subroutine [HAS1](has1.md) (category: Ship hangar)

Draw a ship in the ship hangar

[X]

Variable [HATB](../variable/hatb.md) (category: Ship hangar)

Ship hangar group table

[X]

Configuration variable [SH3](../../all/workspaces.md#sh3) = 17

Ship type for a Sidewinder

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[HATB](../variable/hatb.md "Previous routine")[HAS1](has1.md "Next routine")
