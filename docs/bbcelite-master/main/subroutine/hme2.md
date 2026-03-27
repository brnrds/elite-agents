---
title: "The HME2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hme2.html"
---

[ESCAPE](escape.md "Previous routine")[HATB](../variable/hatb.md "Next routine")
    
    
           Name: HME2                                                    [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Search the galaxy for a system
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-hme2)
     Variations: See [code variations](../../related/compare/main/subroutine/hme2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](tt102.md) calls HME2
    
    
    
    
    
    
    .HME2
    
     LDA #CYAN              \ Switch to colour 3, which is white in the chart view
     STA COL
    
     LDA #14                \ Print extended token 14 ("{clear bottom of screen}
     JSR DETOK              \ PLANET NAME?{fetch line input from keyboard}"). The
                            \ last token calls MT26, which puts the entered search
                            \ term in INWK+5 and the term length in Y
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will erase the crosshairs currently there
    
     JSR TT81               \ Set the seeds in QQ15 (the selected system) to those
                            \ of system 0 in the current galaxy (i.e. copy the seeds
                            \ from QQ21 to QQ15)
    
     LDA #0                 \ We now loop through the galaxy's systems in order,
     STA XX20               \ until we find a match, so set XX20 to act as a system
                            \ counter, starting with system 0
    
    .HME3
    
     JSR MT14               \ Switch to justified text when printing extended
                            \ tokens, so the call to cpl prints into the justified
                            \ text buffer at BUF instead of the screen, and DTW5
                            \ gets set to the length of the system name
    
     JSR cpl                \ Print the selected system name into the justified text
                            \ buffer
    
     LDX DTW5               \ Fetch DTW5 into X, so X is now equal to the length of
                            \ the selected system name
    
     LDA INWK+5,X           \ Fetch the X-th character from the entered search term
    
     CMP #13                \ If the X-th character is not a carriage return, then
     BNE HME6               \ the selected system name and the entered search term
                            \ are different lengths, so jump to HME6 to move on to
                            \ the next system
    
    .HME4
    
     DEX                    \ Decrement X so it points to the last letter of the
                            \ selected system name (and, when we loop back here, it
                            \ points to the next letter to the left)
    
     LDA INWK+5,X           \ Set A to the X-th character of the entered search term
    
     ORA #%00100000         \ Set bit 5 of the character to make it lower case
    
     CMP BUF,X              \ If the character in A matches the X-th character of
     BEQ HME4               \ the selected system name in BUF, loop back to HME4 to
                            \ check the next letter to the left
    
     TXA                    \ The last comparison didn't match, so copy the letter
     BMI HME5               \ number into A, and if it's negative, that means we
                            \ managed to go past the first letters of each term
                            \ before we failed to get a match, so the terms are the
                            \ same, so jump to HME5 to process a successful search
    
    .HME6
    
                            \ If we get here then the selected system name and the
                            \ entered search term did not match
    
     JSR TT20               \ We want to move on to the next system, so call TT20
                            \ to twist the three 16-bit seeds in QQ15
    
     INC XX20               \ Increment the system counter in XX20
    
     BNE HME3               \ If we haven't yet checked all 256 systems in the
                            \ current galaxy, loop back to HME3 to check the next
                            \ system
    
                            \ If we get here then the entered search term did not
                            \ match any systems in the current galaxy
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10), so we can put the crosshairs back where
                            \ they were before the search
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10)
    
     JSR BOOP               \ Call the BOOP routine to make a low, long beep to
                            \ indicate a failed search
    
     LDA #215               \ Print extended token 215 ("{left align} UNKNOWN
     JMP DETOK              \ PLANET"), which will print on-screen as the left align
                            \ code disables justified text, and return from the
                            \ subroutine using a tail call
    
    .HME5
    
                            \ If we get here then we have found a match for the
                            \ entered search
    
     LDA QQ15+3             \ The x-coordinate of the system described by the seeds
     STA QQ9                \ in QQ15 is in QQ15+3 (s1_hi), so we copy this to QQ9
                            \ as the x-coordinate of the search result
    
     LDA QQ15+1             \ The y-coordinate of the system described by the seeds
     STA QQ10               \ in QQ15 is in QQ15+1 (s0_hi), so we copy this to QQ10
                            \ as the y-coordinate of the search result
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10)
    
     JSR MT15               \ Switch to left-aligned text when printing extended
                            \ tokens so future tokens will print to the screen (as
                            \ this disables justified text)
    
     JMP T95                \ Jump to T95 to print the distance to the selected
                            \ system and return from the subroutine using a tail
                            \ call
    

[X]

Subroutine [BOOP](boop.md) (category: Sound)

Make a long, low beep

[X]

Variable [BUF](../workspace/wp.md#buf) in workspace [WP](../workspace/wp.md)

The line buffer used by DASC to print justified text

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Variable [DTW5](../variable/dtw5.md) (category: Text)

The size of the justified text buffer at BUF

[X]

Label [HME3](hme2.md#hme3) is local to this routine

[X]

Label [HME4](hme2.md#hme4) is local to this routine

[X]

Label [HME5](hme2.md#hme5) is local to this routine

[X]

Label [HME6](hme2.md#hme6) is local to this routine

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [MT14](mt14.md) (category: Text)

Switch to justified text when printing extended tokens

[X]

Subroutine [MT15](mt15.md) (category: Text)

Switch to left-aligned text when printing extended tokens

[X]

Variable [QQ10](../workspace/zp.md#qq10) in workspace [ZP](../workspace/zp.md)

The galactic y-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic y-coordinate)

[X]

Variable [QQ15](../workspace/zp.md#qq15) in workspace [ZP](../workspace/zp.md)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ9](../workspace/zp.md#qq9) in workspace [ZP](../workspace/zp.md)

The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)

[X]

Entry point [T95](tt102.md#t95) in subroutine [TT102](tt102.md) (category: Keyboard)

Print the distance to the selected system

[X]

Subroutine [TT103](tt103.md) (category: Charts)

Draw a small set of crosshairs on a chart

[X]

Subroutine [TT111](tt111.md) (category: Universe)

Set the current system to the nearest system to a point

[X]

Subroutine [TT20](tt20.md) (category: Universe)

Twist the selected system's seeds four times

[X]

Subroutine [TT81](tt81.md) (category: Universe)

Set the selected system's seeds to those of system 0

[X]

Variable [XX20](../workspace/zp.md#xx20) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [cpl](cpl.md) (category: Universe)

Print the selected system name

[ESCAPE](escape.md "Previous routine")[HATB](../variable/hatb.md "Next routine")
