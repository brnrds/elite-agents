---
title: "The JMTB variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/jmtb.html"
---

[VOWEL](../subroutine/vowel.md "Previous routine")[TKN2](tkn2.md "Next routine")
    
    
           Name: JMTB                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: The extended token table for jump tokens 1-32 (DETOK)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-jmtb)
     Variations: See [code variations](../../related/compare/main/variable/jmtb.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [DETOK2](../subroutine/detok2.md) uses JMTB
    
    
    
    
    
    
    .JMTB
    
     EQUW MT1               \ Token  1: Switch to ALL CAPS
     EQUW MT2               \ Token  2: Switch to Sentence Case
     EQUW TT27              \ Token  3: Print the selected system name
     EQUW TT27              \ Token  4: Print the commander's name
     EQUW MT5               \ Token  5: Switch to extended tokens
     EQUW MT6               \ Token  6: Switch to standard tokens, in Sentence Case
     EQUW DASC              \ Token  7: Beep
     EQUW MT8               \ Token  8: Tab to column 6
     EQUW MT9               \ Token  9: Clear screen, tab to column 1, view type = 1
     EQUW DASC              \ Token 10: Line feed
     EQUW NLIN4             \ Token 11: Draw box around title (line at pixel row 19)
     EQUW DASC              \ Token 12: Carriage return
     EQUW MT13              \ Token 13: Switch to lower case
     EQUW MT14              \ Token 14: Switch to justified text
     EQUW MT15              \ Token 15: Switch to left-aligned text
     EQUW MT16              \ Token 16: Print the character in DTW7 (drive number)
     EQUW MT17              \ Token 17: Print system name adjective in Sentence Case
     EQUW MT18              \ Token 18: Randomly print 1 to 4 two-letter tokens
     EQUW MT19              \ Token 19: Capitalise first letter of next word only
     EQUW DASC              \ Token 20: Unused
     EQUW CLYNS             \ Token 21: Clear the bottom few lines of the space view
     EQUW PAUSE             \ Token 22: Display ship and wait for key press
     EQUW MT23              \ Token 23: Move to row 10, white text, set lower case
     EQUW PAUSE2            \ Token 24: Wait for a key press
     EQUW BRIS              \ Token 25: Show incoming message screen, wait 2 seconds
     EQUW MT26              \ Token 26: Fetch line input from keyboard (filename)
     EQUW MT27              \ Token 27: Print mission captain's name (217-219)
     EQUW MT28              \ Token 28: Print mission 1 location hint (220-221)
     EQUW MT29              \ Token 29: Column 6, white text, lower case in words
     EQUW FILEPR            \ Token 30: Display currently selected media (disk/tape)
     EQUW OTHERFILEPR       \ Token 31: Display the non-selected media (disk/tape)
     EQUW DASC              \ Token 32: Unused
    

[X]

Subroutine [BRIS](../subroutine/bris.md) (category: Missions)

Clear the screen, display "INCOMING MESSAGE" and wait for 2 seconds

[X]

Subroutine [CLYNS](../subroutine/clyns.md) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Entry point [DASC](../subroutine/tt26.md#dasc) in subroutine [TT26](../subroutine/tt26.md) (category: Text)

DASC does exactly the same as TT26 and prints a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [FILEPR](../subroutine/filepr.md) (category: Save and load)

Display the currently selected media (disk or tape)

[X]

Subroutine [MT1](../subroutine/mt1.md) (category: Text)

Switch to ALL CAPS when printing extended tokens

[X]

Subroutine [MT13](../subroutine/mt13.md) (category: Text)

Switch to lower case when printing extended tokens

[X]

Subroutine [MT14](../subroutine/mt14.md) (category: Text)

Switch to justified text when printing extended tokens

[X]

Subroutine [MT15](../subroutine/mt15.md) (category: Text)

Switch to left-aligned text when printing extended tokens

[X]

Subroutine [MT16](../subroutine/mt16.md) (category: Text)

Print the character in variable DTW7

[X]

Subroutine [MT17](../subroutine/mt17.md) (category: Text)

Print the selected system's adjective, e.g. Lavian for Lave

[X]

Subroutine [MT18](../subroutine/mt18.md) (category: Text)

Print a random 1-8 letter word in Sentence Case

[X]

Subroutine [MT19](../subroutine/mt19.md) (category: Text)

Capitalise the next letter

[X]

Subroutine [MT2](../subroutine/mt2.md) (category: Text)

Switch to Sentence Case when printing extended tokens

[X]

Subroutine [MT23](../subroutine/mt23.md) (category: Text)

Move to row 10, switch to white text, and switch to lower case when printing extended tokens

[X]

Subroutine [MT26](../subroutine/mt26.md) (category: Text)

Fetch a line of text from the keyboard

[X]

Subroutine [MT27](../subroutine/mt27.md) (category: Text)

Print the captain's name during mission briefings

[X]

Subroutine [MT28](../subroutine/mt28.md) (category: Text)

Print the location hint during the mission 1 briefing

[X]

Subroutine [MT29](../subroutine/mt29.md) (category: Text)

Move to row 6, switch to white text, and switch to lower case when printing extended tokens

[X]

Subroutine [MT5](../subroutine/mt5.md) (category: Text)

Switch to extended tokens

[X]

Subroutine [MT6](../subroutine/mt6.md) (category: Text)

Switch to standard tokens in Sentence Case

[X]

Subroutine [MT8](../subroutine/mt8.md) (category: Text)

Tab to column 6 and start a new word when printing extended tokens

[X]

Subroutine [MT9](../subroutine/mt9.md) (category: Text)

Clear the screen and set the current view type to 1

[X]

Subroutine [NLIN4](../subroutine/nlin4.md) (category: Drawing lines)

Draw a horizontal line at pixel row 19 to box in a title

[X]

Subroutine [OTHERFILEPR](../subroutine/otherfilepr.md) (category: Save and load)

Display the non-selected media (disk or tape)

[X]

Subroutine [PAUSE](../subroutine/pause.md) (category: Missions)

Display a rotating ship, waiting until a key is pressed, then remove the ship from the screen

[X]

Subroutine [PAUSE2](../subroutine/pause2.md) (category: Keyboard)

Wait until a key is pressed, ignoring any existing key press

[X]

Subroutine [TT27](../subroutine/tt27.md) (category: Text)

Print a text token

[VOWEL](../subroutine/vowel.md "Previous routine")[TKN2](tkn2.md "Next routine")
