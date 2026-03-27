---
title: "The TITLE subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/title.html"
---

[DFAULT](dfault.md "Previous routine")[CHECK](check.md "Next routine")
    
    
           Name: TITLE                                                   [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Display a title screen with a rotating ship and prompt
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-title)
     Variations: See [code variations](../../related/compare/main/subroutine/title.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 1 of 2)](br1_part_1_of_2.md) calls TITLE
                 * [BR1 (Part 2 of 2)](br1_part_2_of_2.md) calls TITLE
    
    
    
    
    
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
    

[X]

Variable [CNT2](../workspace/zp.md#cnt2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [DOVDU19](dovdu19.md) (category: Drawing the screen)

Change the mode 1 palette

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [JSTK](../workspace/up.md#jstk) in workspace [UP](../workspace/up.md)

Keyboard or joystick configuration setting

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

Variable [PATG](../workspace/up.md#patg) in workspace [UP](../workspace/up.md)

Configuration setting to show the author names on the start-up screen and enable manual hyperspace mis-jumps

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Variable [QQ17](../workspace/zp.md#qq17) in workspace [ZP](../workspace/zp.md)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Subroutine [RDFIRE](rdfire.md) (category: Keyboard)

Read the fire button on either the analogue or digital joystick

[X]

Subroutine [RDKEY](rdkey.md) (category: Keyboard)

Scan the keyboard for key presses and update the key logger

[X]

Configuration variable [RED](../../all/workspaces.md#red) = %11110000

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Subroutine [RESET](reset.md) (category: Start and end)

Reset most variables

[X]

Label [TL1](title.md#tl1) is local to this routine

[X]

Label [TL3](title.md#tl3) is local to this routine

[X]

Label [TLL2](title.md#tll2) is local to this routine

[X]

Subroutine [TT26](tt26.md) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Subroutine [ZEKTRAN](zektran.md) (category: Keyboard)

Clear the key logger

[X]

Subroutine [ZINF](zinf.md) (category: Universe)

Reset the INWK workspace and orientation vectors

[X]

Label [awe](title.md#awe) is local to this routine

[X]

Variable [distaway](../workspace/wp.md#distaway) in workspace [WP](../workspace/wp.md)

Used to store the nearest distance of the rotating ship on the title screen

[X]

Subroutine [plf](plf.md) (category: Text)

Print a text token followed by a newline

[DFAULT](dfault.md "Previous routine")[CHECK](check.md "Next routine")
