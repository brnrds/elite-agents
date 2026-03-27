---
title: "The DEATH subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/death.html"
---

[NEWBRK](newbrk.md "Previous routine")[spasto](../variable/spasto.md "Next routine")
    
    
           Name: DEATH                                                   [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Display the death screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-death)
     Variations: See [code variations](../../related/compare/main/subroutine/death.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md) calls DEATH
                 * [Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md) calls DEATH
                 * [OOPS](oops.md) calls DEATH
    
    
    
    
    
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
    

[X]

Entry point [BOX](ttx66.md#box) in subroutine [TTX66](ttx66.md) (category: Drawing the screen)

Just draw the border box along the top and sides

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Label [D1](death.md#d1) is local to this routine

[X]

Label [D2](death.md#d2) is local to this routine

[X]

Label [D3](death.md#d3) is local to this routine

[X]

Subroutine [DEATH2](death2.md) (category: Start and end)

Reset most of the game and restart from the title screen

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Subroutine [DET1](det1.md) (category: Drawing the screen)

Show or hide the dashboard (for when we die)

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [LASCT](../workspace/wp.md#lasct) in workspace [WP](../workspace/wp.md)

The laser pulse count for the current laser

[X]

Entry point [M%](main_flight_loop_part_1_of_16.md#m-per-cent) in subroutine [Main flight loop (Part 1 of 16)](main_flight_loop_part_1_of_16.md) (category: Main loop)

The entry point for the main flight loop

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Configuration variable [OIL](../../all/workspaces.md#oil) = 5

Ship type for a cargo canister

[X]

Configuration variable [PLT](../../all/workspaces.md#plt) = 4

Ship type for an alloy plate

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Subroutine [RES2](res2.md) (category: Start and end)

Reset a number of flight variables and workspaces

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Configuration variable [XX21](../../all/workspaces.md#xx21) = &8000

The address of the ship blueprints lookup table, as set in elite-data.asm

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Subroutine [Ze](ze.md) (category: Universe)

Initialise the INWK workspace to a fairly aggressive ship

[X]

Subroutine [ex](ex.md) (category: Text)

Print a recursive token

[X]

Entry point [fq1](frs1.md#fq1) in subroutine [FRS1](frs1.md) (category: Tactics)

Used to add a cargo canister to the universe

[X]

Subroutine [nWq](nwq.md) (category: Stardust)

Create a random cloud of stardust

[X]

Configuration variable [soexpl](../../all/workspaces.md#soexpl) = 4

Sound 4 = We died / Collision / Being hit by lasers 2

[NEWBRK](newbrk.md "Previous routine")[spasto](../variable/spasto.md "Next routine")
