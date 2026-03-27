---
title: "The hyp subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hyp.html"
---

[dockEd](docked.md "Previous routine")[wW](ww.md "Next routine")
    
    
           Name: hyp                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Start the hyperspace process
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-hyp)
     Variations: See [code variations](../../related/compare/main/subroutine/hyp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](tt102.md) calls hyp
                 * [TTX110](ttx110.md) calls via TTX111
    
    
    
    
    
    * * *
    
    
     Called when "H" or CTRL-H is pressed during flight. Checks the following:
    
       * We are in space
    
       * We are not already in a hyperspace countdown
    
     If CTRL is being held down, we jump to Ghy to engage the galactic hyperdrive,
     otherwise we check that:
    
       * The selected system is not the current system
    
       * We have enough fuel to make the jump
    
     and if all the pre-jump checks are passed, we print the destination on-screen
     and start the countdown.
    
    
    
    * * *
    
    
     Other entry points:
    
       TTX111               Used to rejoin this routine from the call to TTX110
    
    
    
    
    .hyp
    
     LDA QQ12               \ If we are docked (QQ12 = &FF) then jump to dockEd to
     BNE dockEd             \ print an error message and return from the subroutine
                            \ using a tail call (as we can't hyperspace when docked)
    
     LDA QQ22+1             \ Fetch QQ22+1, which contains the number that's shown
                            \ on-screen during hyperspace countdown
    
     BEQ P%+3               \ If it is zero, skip the next instruction
    
     RTS                    \ The count is non-zero, so return from the subroutine
    
     LDA #CYAN              \ The count is zero, so switch to colour 3, which is
     STA COL                \ cyan in the space view
    
    IF _SNG47
    
     JSR CTRL               \ Scan the keyboard to see if CTRL is currently pressed
    
    ELIF _COMPACT
    
     JSR CTRLmc             \ Scan the keyboard to see if CTRL is currently pressed
    
    ENDIF
    
     BMI Ghy                \ If it is, then the galactic hyperdrive has been
                            \ activated, so jump to Ghy to process it
    
     LDA QQ11               \ If the current view is 0 (i.e. the space view) then
     BEQ TTX110             \ jump to TTX110, which calls TT111 to set the current
                            \ system to the nearest system to (QQ9, QQ10), and jumps
                            \ back into this routine at TTX111 below
    
     AND #%11000000         \ If either bit 6 or 7 of the view type is set - so
     BNE P%+3               \ this is either the Short-range or Long-range Chart -
                            \ then skip the following instruction
    
     RTS                    \ This is not a chart view, so return from the
                            \ subroutine
    
     JSR hm                 \ This is a chart view, so call hm to redraw the chart
                            \ crosshairs
    
    .TTX111
    
                            \ If we get here then the current view is either the
                            \ space view or a chart
    
     LDA QQ8                \ If either byte of the distance to the selected system
     ORA QQ8+1              \ in QQ8 are zero, skip the next instruction to make a
     BNE P%+3               \ copy of the destination seeds in safehouse
    
     RTS                    \ The selected system is the same as the current system,
                            \ so return from the subroutine
    
     LDX #5                 \ We now want to copy those seeds into safehouse, so we
                            \ so set a counter in X to copy 6 bytes
    
    .sob
    
     LDA QQ15,X             \ Copy the X-th byte of QQ15 into the X-th byte of
     STA safehouse,X        \ safehouse
    
     DEX                    \ Decrement the loop counter
    
     BPL sob                \ Loop back to copy the next byte until we have copied
                            \ all six seed bytes
    
     LDA #7                 \ Move the text cursor to column 7, row 22 (in the
     STA XC                 \ middle of the bottom text row)
     LDA #22
     STA YC
    
     LDA #0                 \ Set QQ17 = 0 to switch to ALL CAPS
     STA QQ17
    
     LDA #189               \ Print recursive token 29 ("HYPERSPACE ")
     JSR TT27
    
     LDA QQ8+1              \ If the high byte of the distance to the selected
     BNE goTT147            \ system in QQ8 is > 0, then it is definitely too far to
                            \ jump (as our maximum range is 7.0 light years, or a
                            \ value of 70 in QQ8(1 0)), so jump to TT147 via goTT147
                            \ to print "RANGE?" and return from the subroutine using
                            \ a tail call
    
     LDA QQ14               \ Fetch our current fuel level from Q114 into A
    
     CMP QQ8                \ If our fuel reserves are greater than or equal to the
     BCS P%+5               \ distance to the selected system, then we have enough
                            \ fuel for this jump, so skip the following instruction
                            \ to start the hyperspace countdown
    
    .goTT147
    
     JMP TT147              \ We don't have enough fuel to reach the destination, so
                            \ jump to TT147 to print "RANGE?" and return from the
                            \ subroutine using a tail call
    
     LDA #'-'               \ Print a hyphen
     JSR TT27
    
     JSR cpl                \ Call cpl to print the name of the selected system
    
                            \ Fall through into wW to start the hyperspace countdown
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [CTRL](ctrl.md) (category: Keyboard)

Scan the keyboard to see if CTRL is currently pressed

[X]

Subroutine [CTRLmc](ctrlmc.md) (category: Keyboard)

Scan the Master Compact keyboard to see if CTRL is currently pressed

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Subroutine [Ghy](ghy.md) (category: Flight)

Perform a galactic hyperspace jump

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Variable [QQ12](../workspace/zp.md#qq12) in workspace [ZP](../workspace/zp.md)

Our "docked" status

[X]

Variable [QQ14](../workspace/wp.md#qq14) in workspace [WP](../workspace/wp.md)

Our current fuel level (0-70)

[X]

Variable [QQ15](../workspace/zp.md#qq15) in workspace [ZP](../workspace/zp.md)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ17](../workspace/zp.md#qq17) in workspace [ZP](../workspace/zp.md)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Variable [QQ22](../workspace/zp.md#qq22) in workspace [ZP](../workspace/zp.md)

The two hyperspace countdown counters

[X]

Variable [QQ8](../workspace/zp.md#qq8) in workspace [ZP](../workspace/zp.md)

The distance from the current system to the selected system in light years * 10, stored as a 16-bit number

[X]

Subroutine [TT147](tt147.md) (category: Flight)

Print an error when a system is out of hyperspace range

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[X]

Subroutine [TTX110](ttx110.md) (category: Flight)

Set the current system to the nearest system and return to hyp

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Subroutine [cpl](cpl.md) (category: Universe)

Print the selected system name

[X]

Subroutine [dockEd](docked.md) (category: Flight)

Print a message to say there is no hyperspacing allowed inside the station

[X]

Label [goTT147](hyp.md#gott147) is local to this routine

[X]

Subroutine [hm](hm.md) (category: Charts)

Select the closest system and redraw the chart crosshairs

[X]

Variable [safehouse](../workspace/wp.md#safehouse) in workspace [WP](../workspace/wp.md)

Backup storage for the seeds for the selected system

[X]

Label [sob](hyp.md#sob) is local to this routine

[dockEd](docked.md "Previous routine")[wW](ww.md "Next routine")
