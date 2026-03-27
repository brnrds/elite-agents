---
title: "Elite D source in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_d.html"
---

[Elite C source](elite_c.md "Previous routine")[Elite E source](elite_e.md "Next routine")
    
    
     ELITE D FILE
    
    
    
    
     CODE_D% = P%
    
     LOAD_D% = LOAD% + P% - CODE%
    
    
    
    
           Name: SCALEY                                                  [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Scale the y-coordinate in A to 0.5 * A
    
    
        Context: See this subroutine [on its own page](../main/subroutine/scaley.md)
     References: This subroutine is called as follows:
                 * [TT103](../main/subroutine/tt103.md) calls SCALEY
                 * [TT14](../main/subroutine/tt14.md) calls SCALEY
                 * [TT22](../main/subroutine/tt22.md) calls SCALEY
    
    
    
    
    
    * * *
    
    
     This routine (and the related SCALEY2 and SCALEX routines) are called from
     various places in the code to scale the value in A. This code is different in
     the Apple II and BBC Master versions, and allows coordinates to be scaled
     correctly on different platforms.
    
     In the Master version, the only scaling routine that does anything is SCALEY,
     which halves the y-coordinates in the Long-range Chart (as the galaxy is half
     as tall as it is wide). The other routines are left over from the Apple II
     version, which uses them to scale the system charts to fit into its smaller
     screen size.
    
    
    
    
    .SCALEY
    
     LSR A                  \ Halve the value in A
    
                            \ Fall through into SCALEY2 to return from the
                            \ subroutine
    
    
    
    
           Name: SCALEY2                                                 [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Scale the y-coordinate in A (leave it unchanged)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/scaley2.md)
     References: This subroutine is called as follows:
                 * [TT105](../main/subroutine/tt105.md) calls SCALEY2
                 * [TT14](../main/subroutine/tt14.md) calls SCALEY2
                 * [TT23](../main/subroutine/tt23.md) calls SCALEY2
    
    
    
    
    
    * * *
    
    
     This routine (and the related SCALEY and SCALEX routines) are called from
     various places in the code to scale the value in A. This code is different in
     the Apple II and BBC Master versions, and allows coordinates to be scaled
     correctly on different platforms.
    
     The original source contains the comment "SCALE Scans by 3/4 to fit in".
    
     In the Master version, the only scaling routine that does anything is SCALEY,
     which halves the y-coordinates in the Long-range Chart (as the galaxy is half
     as tall as it is wide). The other routines are left over from the Apple II
     version, which uses them to scale the system charts to fit into its smaller
     screen size.
    
    
    
    
    .SCALEY2
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SCALEX                                                  [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Scale the x-coordinate in A (leave it unchanged)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/scalex.md)
     References: This subroutine is called as follows:
                 * [TT103](../main/subroutine/tt103.md) calls SCALEX
                 * [TT14](../main/subroutine/tt14.md) calls SCALEX
                 * [TT22](../main/subroutine/tt22.md) calls SCALEX
    
    
    
    
    
    * * *
    
    
     This routine (and the related SCALEY and SCALEY2 routines) are called from
     various places in the code to scale the value in A. This code is different in
     the Apple II and BBC Master versions, and allows coordinates to be scaled
     correctly on different platforms.
    
     In the Master version, the only scaling routine that does anything is SCALEY,
     which halves the y-coordinates in the Long-range Chart (as the galaxy is half
     as tall as it is wide). The other routines are left over from the Apple II
     version, which uses them to scale the system charts to fit into its smaller
     screen size.
    
    
    
    
    .SCALEX
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DVLOIN                                                  [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a vertical line from (A, GCYT) to (A, GCYB)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dvloin.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine is from the Apple II version of Elite and is not used in the BBC
     Master version.
    
    
    
    
    .DVLOIN
    
     STA X1                 \ Draw a vertical line from (A, GCYT) to (A, GCYB)
     STA X2
     LDA #GCYT
     STA Y1
     LDA #GCYB
     STA Y2
     JMP LOIN
    
    
    
    
           Name: tnpr1                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Work out if we have space for one tonne of cargo
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tnpr1.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md) calls tnpr1
    
    
    
    
    
    * * *
    
    
     Given a market item, work out whether there is room in the cargo hold for one
     tonne of this item.
    
     For standard tonne canisters, the limit is given by the type of cargo hold we
     have, with a standard cargo hold having a capacity of 20t and an extended
     cargo bay being 35t.
    
     For items measured in kg (gold, platinum), g (gem-stones) and alien items,
     the individual limit on each of these is 200 units.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The type of market item (see QQ23 for a list of market
                            item numbers)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A = 1
    
       C flag               Returns the result:
    
                              * Set if there is no room for this item
    
                              * Clear if there is room for this item
    
    
    
    
    .tnpr1
    
     STA QQ29               \ Store the type of market item in QQ29
    
     LDA #1                 \ Set the number of units of this market item to 1
    
                            \ Fall through into tnpr to work out whether there is
                            \ room in the cargo hold for A tonnes of the item of
                            \ type QQ29
    
    
    
    
           Name: tnpr                                                    [Show more]
           Type: Subroutine
       Category: Market
        Summary: Work out if we have space for a specific amount of cargo
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tnpr.md)
     Variations: See [code variations](../related/compare/main/subroutine/tnpr.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT219](../main/subroutine/tt219.md) calls tnpr
    
    
    
    
    
    * * *
    
    
     Given a market item and an amount, work out whether there is room in the
     cargo hold for this item.
    
     For standard tonne canisters, the limit is given by the type of cargo hold we
     have, with a standard cargo hold having a capacity of 20t and an extended
     cargo bay being 35t.
    
     For items measured in kg (gold, platinum), g (gem-stones) and alien items,
     the individual limit on each of these is 200 units.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The number of units of this market item
    
       QQ29                 The type of market item (see QQ23 for a list of market
                            item numbers)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       C flag               Returns the result:
    
                              * Set if there is no room for this item
    
                              * Clear if there is room for this item
    
    
    
    
    .tnpr
    
     PHA                    \ Store A on the stack
    
     LDX #12                \ If QQ29 > 12 then jump to kg below, as this cargo
     CPX QQ29               \ type is gold, platinum, gem-stones or alien items,
     BCC kg                 \ and they have different cargo limits to the standard
                            \ tonne canisters
    
    .Tml
    
                            \ Here we count the tonne canisters we have in the hold
                            \ and add to A to see if we have enough room for A more
                            \ tonnes of cargo, using X as the loop counter, starting
                            \ with X = 12
    
     ADC QQ20,X             \ Set A = A + the number of tonnes we have in the hold
                            \ of market item number X. Note that the first time we
                            \ go round this loop, the C flag is set (as we didn't
                            \ branch with the BCC above, so the effect of this loop
                            \ is to count the number of tonne canisters in the hold,
                            \ and add 1
    
     DEX                    \ Decrement the loop counter
    
     BPL Tml                \ Loop back to add in the next market item in the hold,
                            \ until we have added up all market items from 12
                            \ (minerals) down to 0 (food)
    
     ADC TRIBBLE+1          \ Add the high byte of the number of Trumbles in the
                            \ hold, as 256 Trumbles take up one tonne of cargo space
    
     CMP CRGO               \ If A < CRGO then the C flag will be clear (we have
                            \ room in the hold)
                            \
                            \ If A >= CRGO then the C flag will be set (we do not
                            \ have room in the hold)
                            \
                            \ This works because A contains the number of canisters
                            \ plus 1, while CRGO contains our cargo capacity plus 2,
                            \ so if we actually have "a" canisters and a capacity
                            \ of "c", then:
                            \
                            \ A < CRGO means: a+1 <  c+2
                            \                 a   <  c+1
                            \                 a   <= c
                            \
                            \ So this is why the value in CRGO is 2 higher than the
                            \ actual cargo bay size, i.e. it's 22 for the standard
                            \ 20-tonne bay, and 37 for the large 35-tonne bay
    
     PLA                    \ Restore A from the stack
    
     RTS                    \ Return from the subroutine
    
    .kg
    
                            \ Here we count the number of items of this type that
                            \ we already have in the hold, and add to A to see if
                            \ we have enough room for A more units
    
     LDY QQ29               \ Set Y to the item number we want to add
    
     ADC QQ20,Y             \ Set A = A + the number of units of this item that we
                            \ already have in the hold
    
     CMP #200               \ Is the result greater than 200 (the limit on
                            \ individual stocks of gold, platinum, gem-stones and
                            \ alien items)?
                            \
                            \ If so, this sets the C flag (no room)
                            \
                            \ Otherwise it is clear (we have room)
    
     PLA                    \ Restore A from the stack
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DOXC                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Move the text cursor to a specific column
    
    
        Context: See this subroutine [on its own page](../main/subroutine/doxc.md)
     Variations: See [code variations](../related/compare/main/subroutine/setxc-doxc.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](../main/subroutine/brief.md) calls DOXC
                 * [crlf](../main/subroutine/crlf.md) calls DOXC
                 * [MT8](../main/subroutine/mt8.md) calls DOXC
                 * [TT151](../main/subroutine/tt151.md) calls DOXC
                 * [TT163](../main/subroutine/tt163.md) calls DOXC
                 * [TT210](../main/subroutine/tt210.md) calls DOXC
                 * [TT219](../main/subroutine/tt219.md) calls DOXC
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text column
    
    
    
    
    .DOXC
    
     STA XC                 \ Store the new text column in XC
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DOYC                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Move the text cursor to a specific row
    
    
        Context: See this subroutine [on its own page](../main/subroutine/doyc.md)
     Variations: See [code variations](../related/compare/main/subroutine/setyc-doyc.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT219](../main/subroutine/tt219.md) calls DOYC
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text row
    
    
    
    
    .DOYC
    
     STA YC                 \ Store the new text row in YC
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: INCYC                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Move the text cursor to the next row
    
    
        Context: See this subroutine [on its own page](../main/subroutine/incyc.md)
     Variations: See [code variations](../related/compare/main/subroutine/incyc.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .INCYC
    
     INC YC                 \ Move the text cursor to the next row
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TRADEMODE                                               [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the screen and set up a trading screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/trademode.md)
     Variations: See [code variations](../related/compare/main/subroutine/trademode.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls TRADEMODE
                 * [STATUS](../main/subroutine/status.md) calls TRADEMODE
                 * [TT167](../main/subroutine/tt167.md) calls TRADEMODE
                 * [TT208](../main/subroutine/tt208.md) calls TRADEMODE
                 * [TT213](../main/subroutine/tt213.md) calls TRADEMODE
                 * [TT219](../main/subroutine/tt219.md) calls TRADEMODE
                 * [TT25](../main/subroutine/tt25.md) calls TRADEMODE
                 * [SVE](../main/subroutine/sve.md) calls via TRADEMODE2
    
    
    
    
    
    * * *
    
    
     Clear the top part of the screen, draw a border box, set the palette for
     trading screens, and set the current view type in QQ11 to A.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The type of the new current view (see QQ11 for a list of
                            view types)
    
    
    
    * * *
    
    
     Other entry points:
    
       TRADEMODE2           Set the palette for trading screens and switch the
                            current colour to white
    
    
    
    
    .TRADEMODE
    
     JSR TT66               \ Clear the top part of the screen, draw a border box,
                            \ and set the current view type in QQ11 to A
    
     JSR FLKB               \ Call FLKB to flush the keyboard buffer
    
    .TRADEMODE2
    
     LDA #48                \ Switch to the mode 1 palette for trading screens,
     JSR DOVDU19            \ which is yellow (colour 1), magenta (colour 2) and
                            \ white (colour 3)
    
     LDA #CYAN              \ Switch to colour 3, which is white in the trade view
     STA COL
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT20                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Twist the selected system's seeds four times
      Deep dive: [Twisting the system seeds](https://elite.bbcelite.com/deep_dives/twisting_the_system_seeds.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt20.md)
     References: This subroutine is called as follows:
                 * [HME2](../main/subroutine/hme2.md) calls TT20
                 * [TT111](../main/subroutine/tt111.md) calls TT20
                 * [TT22](../main/subroutine/tt22.md) calls TT20
                 * [TT23](../main/subroutine/tt23.md) calls TT20
    
    
    
    
    
    * * *
    
    
     Twist the three 16-bit seeds in QQ15 (selected system) four times, to
     generate the next system.
    
    
    
    
    .TT20
    
     JSR P%+3               \ This line calls the line below as a subroutine, which
                            \ does two twists before returning here, and then we
                            \ fall through to the line below for another two
                            \ twists, so the net effect of these two consecutive
                            \ JSR calls is four twists, not counting the ones
                            \ inside your head as you try to follow this process
    
     JSR P%+3               \ This line calls TT54 as a subroutine to do a twist,
                            \ and then falls through into TT54 to do another twist
                            \ before returning from the subroutine
    
    
    
    
           Name: TT54                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Twist the selected system's seeds
      Deep dive: [Twisting the system seeds](https://elite.bbcelite.com/deep_dives/twisting_the_system_seeds.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt54.md)
     References: This subroutine is called as follows:
                 * [cpl](../main/subroutine/cpl.md) calls TT54
    
    
    
    
    
    * * *
    
    
     This routine twists the three 16-bit seeds in QQ15 once.
    
     If we start with seeds s0, s1 and s2 and we want to work out their new values
     after we perform a twist (let's call the new values s0´, s1´ and s2´), then:
    
      s0´ = s1
      s1´ = s2
      s2´ = s0 + s1 + s2
    
     So given an existing set of seeds in s0, s1 and s2, we can get the new values
     s0´, s1´ and s2´ simply by doing the above sums. And if we want to do the
     above in-place without creating three new s´ variables, then we can do the
     following:
    
      tmp = s0 + s1
      s0 = s1
      s1 = s2
      s2 = tmp + s1
    
     So this is what we do in this routine, where each seed is a 16-bit number.
    
    
    
    
    .TT54
    
     LDA QQ15               \ X = tmp_lo = s0_lo + s1_lo
     CLC
     ADC QQ15+2
     TAX
    
     LDA QQ15+1             \ Y = tmp_hi = s1_hi + s1_hi + C
     ADC QQ15+3
     TAY
    
     LDA QQ15+2             \ s0_lo = s1_lo
     STA QQ15
    
     LDA QQ15+3             \ s0_hi = s1_hi
     STA QQ15+1
    
     LDA QQ15+5             \ s1_hi = s2_hi
     STA QQ15+3
    
     LDA QQ15+4             \ s1_lo = s2_lo
     STA QQ15+2
    
     CLC                    \ s2_lo = X + s1_lo
     TXA
     ADC QQ15+2
     STA QQ15+4
    
     TYA                    \ s2_hi = Y + s1_hi + C
     ADC QQ15+3
     STA QQ15+5
    
     RTS                    \ The twist is complete so return from the subroutine
    
    
    
    
           Name: TT146                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Print the distance to the selected system in light years
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt146.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt146.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT146
                 * [TT25](../main/subroutine/tt25.md) calls TT146
    
    
    
    
    
    * * *
    
    
     If it is non-zero, print the distance to the selected system in light years.
     If it is zero, just move the text cursor down a line.
    
     Specifically, if the distance in QQ8 is non-zero, print token 31 ("DISTANCE"),
     then a colon, then the distance to one decimal place, then token 35 ("LIGHT
     YEARS"). If the distance is zero, move the cursor down one line.
    
    
    
    
    .TT146
    
     LDA QQ8                \ Take the two bytes of the 16-bit value in QQ8 and
     ORA QQ8+1              \ OR them together to check whether there are any
     BNE TT63               \ non-zero bits, and if so, jump to TT63 to print the
                            \ distance
    
     INC YC                 \ The distance is zero, so we just move the text cursor
     RTS                    \ in YC down by one line and return from the subroutine
    
    .TT63
    
     LDA #191               \ Print recursive token 31 ("DISTANCE") followed by
     JSR TT68               \ a colon
    
     LDX QQ8                \ Load (Y X) from QQ8, which contains the 16-bit
     LDY QQ8+1              \ distance we want to show
    
     SEC                    \ Set the C flag so that the call to pr5 will include a
                            \ decimal point, and display the value as (Y X) / 10
    
     JSR pr5                \ Print (Y X) to 5 digits, including a decimal point
    
     LDA #195               \ Set A to the recursive token 35 (" LIGHT YEARS") and
                            \ fall through into TT60 to print the token followed
                            \ by a paragraph break
    
    
    
    
           Name: TT60                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token and a paragraph break
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt60.md)
     References: This subroutine is called as follows:
                 * [TT213](../main/subroutine/tt213.md) calls TT60
                 * [TT25](../main/subroutine/tt25.md) calls TT60
    
    
    
    
    
    * * *
    
    
     Print a text token (i.e. a character, control code, two-letter token or
     recursive token). Then print a paragraph break (a blank line between
     paragraphs) by moving the cursor down a line, setting Sentence Case, and then
     printing a newline.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .TT60
    
     JSR TT27               \ Print the text token in A and fall through into TTX69
                            \ to print the paragraph break
    
    
    
    
           Name: TTX69                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a paragraph break
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ttx69.md)
     Variations: See [code variations](../related/compare/main/subroutine/ttx69.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT25](../main/subroutine/tt25.md) calls TTX69
    
    
    
    
    
    * * *
    
    
     Print a paragraph break (a blank line between paragraphs) by moving the cursor
     down a line, setting Sentence Case, and then printing a newline.
    
    
    
    
    .TTX69
    
     INC YC                 \ Move the text cursor down a line
    
                            \ Fall through into TT69 to set Sentence Case and print
                            \ a newline
    
    
    
    
           Name: TT69                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Set Sentence Case and print a newline
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt69.md)
     References: This subroutine is called as follows:
                 * [TT210](../main/subroutine/tt210.md) calls TT69
    
    
    
    
    
    
    .TT69
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch to Sentence Case
     STA QQ17
    
                            \ Fall through into TT67 to print a newline
    
    
    
    
           Name: TT67                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a newline
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt67.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt67.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls TT67
                 * [NWDAV4](../main/subroutine/nwdav4.md) calls TT67
                 * [plf](../main/subroutine/plf.md) calls TT67
                 * [SVE](../main/subroutine/sve.md) calls TT67
                 * [TT208](../main/subroutine/tt208.md) calls TT67
                 * [TT219](../main/subroutine/tt219.md) calls TT67
    
    
    
    
    
    
    .TT67
    
    \INC YC                 \ This instruction is commented out in the original
                            \ source
    
     LDA #12                \ Load a newline character into A
    
     JMP TT27               \ Print the text token in A and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: TT70                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Display "MAINLY " and jump to TT72
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt70.md)
     References: This subroutine is called as follows:
                 * [TT25](../main/subroutine/tt25.md) calls TT70
    
    
    
    
    
    * * *
    
    
     This subroutine is called by TT25 when displaying a system's economy.
    
    
    
    
    .TT70
    
     LDA #173               \ Print recursive token 13 ("MAINLY ")
     JSR TT27
    
     JMP TT72               \ Jump to TT72 to continue printing system data as part
                            \ of routine TT25
    
    
    
    
           Name: spc                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token followed by a space
    
    
        Context: See this subroutine [on its own page](../main/subroutine/spc.md)
     References: This subroutine is called as follows:
                 * [dn](../main/subroutine/dn.md) calls spc
                 * [EQSHP](../main/subroutine/eqshp.md) calls spc
                 * [qv](../main/subroutine/qv.md) calls spc
                 * [STATUS](../main/subroutine/status.md) calls spc
                 * [TT25](../main/subroutine/tt25.md) calls spc
    
    
    
    
    
    * * *
    
    
     Print a text token (i.e. a character, control code, two-letter token or
     recursive token) followed by a space.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .spc
    
     JSR TT27               \ Print the text token in A
    
     JMP TT162              \ Print a space and return from the subroutine using a
                            \ tail call
    
    
    
    
           Name: TT25                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Show the Data on System screen (red key f6)
      Deep dive: [Generating system data](https://elite.bbcelite.com/deep_dives/generating_system_data.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt25.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt25.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT25
                 * [TT70](../main/subroutine/tt70.md) calls via TT72
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       TT72                 Used by TT70 to re-enter the routine after displaying
                            "MAINLY" for the economy type
    
    
    
    
    .TT25
    
     LDA #1                 \ Clear the top part of the screen, draw a border box,
     JSR TRADEMODE          \ and set up a trading screen with a view type in QQ11
                            \ of 1
    
     LDA #9                 \ Move the text cursor to column 9
     STA XC
    
     LDA #163               \ Print recursive token 3 ("DATA ON {selected system
     JSR NLIN3              \ name}" and draw a horizontal line at pixel row 19
                            \ to box in the title
    
     JSR TTX69              \ Print a paragraph break and set Sentence Case
    
     JSR TT146              \ If the distance to this system is non-zero, print
                            \ "DISTANCE", then the distance, "LIGHT YEARS" and a
                            \ paragraph break, otherwise just move the cursor down
                            \ a line
    
     LDA #194               \ Print recursive token 34 ("ECONOMY") followed by
     JSR TT68               \ a colon
    
     LDA QQ3                \ The system economy is determined by the value in QQ3,
                            \ so fetch it into A. First we work out the system's
                            \ prosperity as follows:
                            \
                            \   QQ3 = 0 or 5 = %000 or %101 = Rich
                            \   QQ3 = 1 or 6 = %001 or %110 = Average
                            \   QQ3 = 2 or 7 = %010 or %111 = Poor
                            \   QQ3 = 3 or 4 = %011 or %100 = Mainly
    
     CLC                    \ If (QQ3 + 1) >> 1 = %10, i.e. if QQ3 = %011 or %100
     ADC #1                 \ (3 or 4), then call TT70, which prints "MAINLY " and
     LSR A                  \ jumps down to TT72 to print the type of economy
     CMP #%00000010
     BEQ TT70
    
     LDA QQ3                \ If (QQ3 + 1) >> 1 < %10, i.e. if QQ3 = %000, %001 or
     BCC TT71               \ %010 (0, 1 or 2), then jump to TT71 with A set to the
                            \ original value of QQ3
    
     SBC #5                 \ Here QQ3 = %101, %110 or %111 (5, 6 or 7), so subtract
     CLC                    \ 5 to bring it down to 0, 1 or 2 (the C flag is already
                            \ set so the SBC will be correct)
    
    .TT71
    
     ADC #170               \ A is now 0, 1 or 2, so print recursive token 10 + A.
     JSR TT27               \ This means that:
                            \
                            \   QQ3 = 0 or 5 prints token 10 ("RICH ")
                            \   QQ3 = 1 or 6 prints token 11 ("AVERAGE ")
                            \   QQ3 = 2 or 7 prints token 12 ("POOR ")
    
    .TT72
    
     LDA QQ3                \ Now to work out the type of economy, which is
     LSR A                  \ determined by bit 2 of QQ3, as follows:
     LSR A                  \
                            \   QQ3 bit 2 = 0 = Industrial
                            \   QQ3 bit 2 = 1 = Agricultural
                            \
                            \ So we fetch QQ3 into A and set A = bit 2 of QQ3 using
                            \ two right shifts (which will work as QQ3 is only a
                            \ 3-bit number)
    
     CLC                    \ Print recursive token 8 + A, followed by a paragraph
     ADC #168               \ break and Sentence Case, so:
     JSR TT60               \
                            \   QQ3 bit 2 = 0 prints token 8 ("INDUSTRIAL")
                            \   QQ3 bit 2 = 1 prints token 9 ("AGRICULTURAL")
    
     LDA #162               \ Print recursive token 2 ("GOVERNMENT") followed by
     JSR TT68               \ a colon
    
     LDA QQ4                \ The system's government is determined by the value in
                            \ QQ4, so fetch it into A
    
     CLC                    \ Print recursive token 17 + A, followed by a paragraph
     ADC #177               \ break and Sentence Case, so:
     JSR TT60               \
                            \   QQ4 = 0 prints token 17 ("ANARCHY")
                            \   QQ4 = 1 prints token 18 ("FEUDAL")
                            \   QQ4 = 2 prints token 19 ("MULTI-GOVERNMENT")
                            \   QQ4 = 3 prints token 20 ("DICTATORSHIP")
                            \   QQ4 = 4 prints token 21 ("COMMUNIST")
                            \   QQ4 = 5 prints token 22 ("CONFEDERACY")
                            \   QQ4 = 6 prints token 23 ("DEMOCRACY")
                            \   QQ4 = 7 prints token 24 ("CORPORATE STATE")
    
     LDA #196               \ Print recursive token 36 ("TECH.LEVEL") followed by a
     JSR TT68               \ colon
    
     LDX QQ5                \ Fetch the tech level from QQ5 and increment it, as it
     INX                    \ is stored in the range 0-14 but the displayed range
                            \ should be 1-15
    
     CLC                    \ Call pr2 to print the technology level as a
     JSR pr2                \ three-digit number without a decimal point (by
                            \ clearing the C flag)
    
     JSR TTX69              \ Print a paragraph break and set Sentence Case
    
     LDA #192               \ Print recursive token 32 ("POPULATION") followed by a
     JSR TT68               \ colon
    
     SEC                    \ Call pr2 to print the population as a three-digit
     LDX QQ6                \ number with a decimal point (by setting the C flag),
     JSR pr2                \ so the number printed will be population / 10
    
     LDA #198               \ Print recursive token 38 (" BILLION"), followed by a
     JSR TT60               \ paragraph break and Sentence Case
    
     LDA #'('               \ Print an opening bracket
     JSR TT27
    
     LDA QQ15+4             \ Now to calculate the species, so first check bit 7 of
     BMI TT75               \ s2_lo, and if it is set, jump to TT75 as this is an
                            \ alien species
    
     LDA #188               \ Bit 7 of s2_lo is clear, so print recursive token 28
     JSR TT27               \ ("HUMAN COLONIAL")
    
     JMP TT76               \ Jump to TT76 to print "S)" and a paragraph break, so
                            \ the whole species string is "(HUMAN COLONIALS)"
    
    .TT75
    
     LDA QQ15+5             \ This is an alien species, and we start with the first
     LSR A                  \ adjective, so fetch bits 2-7 of s2_hi into A and push
     LSR A                  \ onto the stack so we can use this later
     PHA
    
     AND #%00000111         \ Set A = bits 0-2 of A (so that's bits 2-4 of s2_hi)
    
     CMP #3                 \ If A >= 3, jump to TT205 to skip the first adjective,
     BCS TT205
    
     ADC #227               \ Otherwise A = 0, 1 or 2, so print recursive token
     JSR spc                \ 67 + A, followed by a space, so:
                            \
                            \   A = 0 prints token 67 ("LARGE") and a space
                            \   A = 1 prints token 68 ("FIERCE") and a space
                            \   A = 2 prints token 69 ("SMALL") and a space
    
    .TT205
    
     PLA                    \ Now for the second adjective, so restore A to bits
     LSR A                  \ 2-7 of s2_hi, and throw away bits 2-4 to leave
     LSR A                  \ A = bits 5-7 of s2_hi
     LSR A
    
     CMP #6                 \ If A >= 6, jump to TT206 to skip the second adjective
     BCS TT206
    
     ADC #230               \ Otherwise A = 0 to 5, so print recursive token
     JSR spc                \ 70 + A, followed by a space, so:
                            \
                            \   A = 0 prints token 70 ("GREEN") and a space
                            \   A = 1 prints token 71 ("RED") and a space
                            \   A = 2 prints token 72 ("YELLOW") and a space
                            \   A = 3 prints token 73 ("BLUE") and a space
                            \   A = 4 prints token 74 ("BLACK") and a space
                            \   A = 5 prints token 75 ("HARMLESS") and a space
    
    .TT206
    
     LDA QQ15+3             \ Now for the third adjective, so EOR the high bytes of
     EOR QQ15+1             \ s0 and s1 and extract bits 0-2 of the result:
     AND #%00000111         \
     STA QQ19               \   A = (s0_hi EOR s1_hi) AND %111
                            \
                            \ storing the result in QQ19 so we can use it later
    
     CMP #6                 \ If A >= 6, jump to TT207 to skip the third adjective
     BCS TT207
    
     ADC #236               \ Otherwise A = 0 to 5, so print recursive token
     JSR spc                \ 76 + A, followed by a space, so:
                            \
                            \   A = 0 prints token 76 ("SLIMY") and a space
                            \   A = 1 prints token 77 ("BUG-EYED") and a space
                            \   A = 2 prints token 78 ("HORNED") and a space
                            \   A = 3 prints token 79 ("BONY") and a space
                            \   A = 4 prints token 80 ("FAT") and a space
                            \   A = 5 prints token 81 ("FURRY") and a space
    
    .TT207
    
     LDA QQ15+5             \ Now for the actual species, so take bits 0-1 of
     AND #%00000011         \ s2_hi, add this to the value of A that we used for
     CLC                    \ the third adjective, and take bits 0-2 of the result
     ADC QQ19
     AND #%00000111
    
     ADC #242               \ A = 0 to 7, so print recursive token 82 + A, so:
     JSR TT27               \
                            \   A = 0 prints token 82 ("RODENT")
                            \   A = 1 prints token 83 ("FROG")
                            \   A = 2 prints token 84 ("LIZARD")
                            \   A = 3 prints token 85 ("LOBSTER")
                            \   A = 4 prints token 86 ("BIRD")
                            \   A = 5 prints token 87 ("HUMANOID")
                            \   A = 6 prints token 88 ("FELINE")
                            \   A = 7 prints token 89 ("INSECT")
    
    .TT76
    
     LDA #'S'               \ Print an "S" to pluralise the species
     JSR TT27
    
     LDA #')'               \ And finally, print a closing bracket, followed by a
     JSR TT60               \ paragraph break and Sentence Case, to end the species
                            \ section
    
     LDA #193               \ Print recursive token 33 ("GROSS PRODUCTIVITY"),
     JSR TT68               \ followed by a colon
    
     LDX QQ7                \ Fetch the 16-bit productivity value from QQ7 into
     LDY QQ7+1              \ (Y X)
    
     JSR pr6                \ Print (Y X) to 5 digits with no decimal point
    
     JSR TT162              \ Print a space
    
     STZ QQ17               \ Set QQ17 = 0 to switch to ALL CAPS
    
     LDA #'M'               \ Print "M"
     JSR TT27
    
     LDA #226               \ Print recursive token 66 (" CR"), followed by a
     JSR TT60               \ paragraph break and Sentence Case
    
     LDA #250               \ Print recursive token 90 ("AVERAGE RADIUS"), followed
     JSR TT68               \ by a colon
    
                            \ The average radius is calculated like this:
                            \
                            \   ((s2_hi AND %1111) + 11) * 256 + s1_hi
                            \
                            \ or, in terms of memory locations:
                            \
                            \   ((QQ15+5 AND %1111) + 11) * 256 + QQ15+3
                            \
                            \ Because the multiplication is by 256, this is the
                            \ same as saying a 16-bit number, with high byte:
                            \
                            \   (QQ15+5 AND %1111) + 11
                            \
                            \ and low byte:
                            \
                            \   QQ15+3
                            \
                            \ so we can set this up in (Y X) and call the pr5
                            \ routine to print it out
    
     LDA QQ15+5             \ Set A = QQ15+5
     LDX QQ15+3             \ Set X = QQ15+3
    
     AND #%00001111         \ Set Y = (A AND %1111) + 11
     CLC
     ADC #11
     TAY
    
     JSR pr5                \ Print (Y X) to 5 digits, not including a decimal
                            \ point, as the C flag will be clear (as the maximum
                            \ radius will always fit into 16 bits)
    
     JSR TT162              \ Print a space
    
     LDA #'k'               \ Print "km"
     JSR TT26
     LDA #'m'
     JSR TT26
    
     JSR TTX69              \ Print a paragraph break and set Sentence Case
    
                            \ By this point, ZZ contains the current system number
                            \ which PDESC requires. It gets put there in the TT102
                            \ routine, which calls TT111 to populate ZZ before
                            \ calling TT25 (this routine)
    
     JMP PDESC              \ Jump to PDESC to print the system's extended
                            \ description, returning from the subroutine using a
                            \ tail call
    
    
    
    
           Name: TT24                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Calculate system data from the system seeds
      Deep dive: [Generating system data](https://elite.bbcelite.com/deep_dives/generating_system_data.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt24.md)
     References: This subroutine is called as follows:
                 * [TT111](../main/subroutine/tt111.md) calls TT24
    
    
    
    
    
    * * *
    
    
     Calculate system data from the seeds in QQ15 and store them in the relevant
     locations. Specifically, this routine calculates the following from the three
     16-bit seeds in QQ15 (using only s0_hi, s1_hi and s1_lo):
    
       QQ3 = economy (0-7)
       QQ4 = government (0-7)
       QQ5 = technology level (0-14)
       QQ6 = population * 10 (1-71)
       QQ7 = productivity (96-62480)
    
     The ranges of the various values are shown in brackets. Note that the radius
     and type of inhabitant are calculated on-the-fly in the TT25 routine when
     the system data gets displayed, so they aren't calculated here.
    
    
    
    
    .TT24
    
     LDA QQ15+1             \ Fetch s0_hi and extract bits 0-2 to determine the
     AND #%00000111         \ system's economy, and store in QQ3
     STA QQ3
    
     LDA QQ15+2             \ Fetch s1_lo and extract bits 3-5 to determine the
     LSR A                  \ system's government, and store in QQ4
     LSR A
     LSR A
     AND #%00000111
     STA QQ4
    
     LSR A                  \ If government isn't anarchy or feudal, skip to TT77,
     BNE TT77               \ as we need to fix the economy of anarchy and feudal
                            \ systems so they can't be rich
    
     LDA QQ3                \ Set bit 1 of the economy in QQ3 to fix the economy
     ORA #%00000010         \ for anarchy and feudal governments
     STA QQ3
    
    .TT77
    
     LDA QQ3                \ Now to work out the tech level, which we do like this:
     EOR #%00000111         \
     CLC                    \   flipped_economy + (s1_hi AND %11) + (government / 2)
     STA QQ5                \
                            \ or, in terms of memory locations:
                            \
                            \   QQ5 = (QQ3 EOR %111) + (QQ15+3 AND %11) + (QQ4 / 2)
                            \
                            \ We start by setting QQ5 = QQ3 EOR %111
    
     LDA QQ15+3             \ We then take the first 2 bits of s1_hi (QQ15+3) and
     AND #%00000011         \ add it into QQ5
     ADC QQ5
     STA QQ5
    
     LDA QQ4                \ And finally we add QQ4 / 2 and store the result in
     LSR A                  \ QQ5, using LSR then ADC to divide by 2, which rounds
     ADC QQ5                \ up the result for odd-numbered government types
     STA QQ5
    
     ASL A                  \ Now to work out the population, like so:
     ASL A                  \
     ADC QQ3                \   (tech level * 4) + economy + government + 1
     ADC QQ4                \
     ADC #1                 \ or, in terms of memory locations:
     STA QQ6                \
                            \   QQ6 = (QQ5 * 4) + QQ3 + QQ4 + 1
    
     LDA QQ3                \ Finally, we work out productivity, like this:
     EOR #%00000111         \
     ADC #3                 \  (flipped_economy + 3) * (government + 4)
     STA P                  \                        * population
     LDA QQ4                \                        * 8
     ADC #4                 \
     STA Q                  \ or, in terms of memory locations:
     JSR MULTU              \
                            \   QQ7 = (QQ3 EOR %111 + 3) * (QQ4 + 4) * QQ6 * 8
                            \
                            \ We do the first step by setting P to the first
                            \ expression in brackets and Q to the second, and
                            \ calling MULTU, so now (A P) = P * Q. The highest this
                            \ can be is 10 * 11 (as the maximum values of economy
                            \ and government are 7), so the high byte of the result
                            \ will always be 0, so we actually have:
                            \
                            \   P = P * Q
                            \     = (flipped_economy + 3) * (government + 4)
    
     LDA QQ6                \ We now take the result in P and multiply by the
     STA Q                  \ population to get the productivity, by setting Q to
     JSR MULTU              \ the population from QQ6 and calling MULTU again, so
                            \ now we have:
                            \
                            \   (A P) = P * population
    
     ASL P                  \ Next we multiply the result by 8, as a 16-bit number,
     ROL A                  \ so we shift both bytes to the left three times, using
     ASL P                  \ the C flag to carry bits from bit 7 of the low byte
     ROL A                  \ into bit 0 of the high byte
     ASL P
     ROL A
    
     STA QQ7+1              \ Finally, we store the productivity in two bytes, with
     LDA P                  \ the low byte in QQ7 and the high byte in QQ7+1
     STA QQ7
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT22                                                    [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Show the Long-range Chart (red key f4)
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt22.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt22.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT22
                 * [TT114](../main/subroutine/tt114.md) calls TT22
    
    
    
    
    
    
    .TT22
    
     LDA #64                \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 32 (Long-
                            \ range Chart)
    
     LDA #16                \ Switch to the mode 1 palette for the trade view, which
     JSR DOVDU19            \ is yellow (colour 1), magenta (colour 2) and white
                            \ (colour 3)
    
     LDA #CYAN              \ Switch to colour 3, which is white in the chart view
     STA COL
    
     LDA #7                 \ Move the text cursor to column 7
     STA XC
    
     JSR TT81               \ Set the seeds in QQ15 to those of system 0 in the
                            \ current galaxy (i.e. copy the seeds from QQ21 to QQ15)
    
     LDA #199               \ Print recursive token 39 ("GALACTIC CHART{galaxy
     JSR TT27               \ number right-aligned to width 3}")
    
     JSR NLIN               \ Draw a horizontal line at pixel row 23 to box in the
                            \ title and act as the top frame of the chart, and move
                            \ the text cursor down one line
    
     LDA #GCYB+1            \ Move the text cursor down one line and draw a
     JSR NLIN5              \ screen-wide horizontal line at pixel row GCYB+1 (153)
                            \ for the bottom edge of the chart, so the chart itself
                            \ is 128 pixels high, starting on row 24 and ending on
                            \ row 153
    
     JSR TT14               \ Call TT14 to draw a circle with crosshairs at the
                            \ current system's galactic coordinates
    
     LDX #0                 \ We're now going to plot each of the galaxy's systems,
                            \ so set up a counter in X for each system, starting at
                            \ 0 and looping through to 255
    
    .TT83
    
     STX XSAV               \ Store the counter in XSAV
    
     LDX QQ15+3             \ Fetch the s1_hi seed into X, which gives us the
                            \ galactic x-coordinate of this system
    
     LDY QQ15+4             \ Fetch the s2_lo seed and set bits 4 and 6, storing the
     TYA                    \ result in ZZ to give a random number between 80 and
     ORA #%01010000         \ (but which will always be the same for this system).
     STA ZZ                 \ We use this value to determine the size of the point
                            \ for this system on the chart by passing it as the
                            \ distance argument to the PIXEL routine below
    
     LDA #YELLOW            \ Switch to colour 1, which is yellow
     STA COL
    
     LDA QQ15+1             \ Fetch the s0_hi seed into A, which gives us the
                            \ galactic y-coordinate of this system
    
     JSR SCALEY             \ We halve the y-coordinate because the galaxy in
                            \ in Elite is rectangular rather than square, and is
                            \ twice as wide (x-axis) as it is high (y-axis), so the
                            \ chart is 256 pixels wide and 128 high
                            \
                            \ The call to SCALEY simply does an LSR A, but having
                            \ this call instruction here would enable different
                            \ scaling to be applied by altering the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     CLC                    \ Add GCYT to the halved y-coordinate in A (so the top
     ADC #GCYT              \ of the chart is on pixel row GCYT, just below the line
                            \ we drew on row 23 above)
    
     JSR PIXEL              \ Call PIXEL to draw a point at (X, A), with the size of
                            \ the point dependent on the distance specified in ZZ
                            \ (so a high value of ZZ will produce a one-pixel point,
                            \ a medium value will produce a two-pixel dash, and a
                            \ small value will produce a four-pixel square)
    
     JSR TT20               \ We want to move on to the next system, so call TT20
                            \ to twist the three 16-bit seeds in QQ15
    
     LDX XSAV               \ Restore the loop counter from XSAV
    
     INX                    \ Increment the counter
    
     BNE TT83               \ If X > 0 then we haven't done all 256 systems yet, so
                            \ loop back up to TT83
    
     LDA QQ9                \ Set QQ19 to the selected system's x-coordinate
     JSR SCALEX             \
     STA QQ19               \ The call to SCALEX has no effect as it only contains
                            \ an RTS, but having this call instruction here would
                            \ enable different scaling to be applied by altering
                            \ the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LDA QQ10               \ Set QQ19+1 to the selected system's y-coordinate,
     JSR SCALEY             \ halved to fit it into the chart
     STA QQ19+1             \
                            \ The call to SCALEY simply does an LSR A, but having
                            \ this call instruction here would enable different
                            \ scaling to be applied by altering the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LDA #4                 \ Set QQ19+2 to size 4 for the crosshairs size
     STA QQ19+2
    
     LDA #GREEN             \ Switch to stripe 3-1-3-1, which is white/yellow in the
     STA COL                \ chart view
    
                            \ Fall through into TT15 to draw crosshairs of size 4 at
                            \ the selected system's coordinates
    
    
    
    
           Name: TT15                                                    [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a set of crosshairs
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt15.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt15.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SIGHT](../main/subroutine/sight.md) calls TT15
                 * [TT103](../main/subroutine/tt103.md) calls TT15
                 * [TT105](../main/subroutine/tt105.md) calls TT15
                 * [TT14](../main/subroutine/tt14.md) calls TT15
    
    
    
    
    
    * * *
    
    
     For all views except the Short-range Chart, the centre is drawn 24 pixels to
     the right of the y-coordinate given.
    
    
    
    * * *
    
    
     Arguments:
    
       QQ19                 The pixel x-coordinate of the centre of the crosshairs
    
       QQ19+1               The pixel y-coordinate of the centre of the crosshairs
    
       QQ19+2               The size of the crosshairs
    
    
    
    
    .TT15
    
     LDA #GCYT              \ Set A to GCYT, which we will use as the minimum
                            \ screen indent for the crosshairs (i.e. the minimum
                            \ distance from the top-left corner of the screen)
    
     LDX QQ11               \ If the current view is not the Short-range Chart,
     BPL TT178              \ which is the only view with bit 7 set, then jump to
                            \ TT178 to skip the following instruction
    
     LDA #0                 \ This is the Short-range Chart, so set A to 0, so the
                            \ crosshairs can go right up against the screen edges
    
    .TT178
    
     STA QQ19+5             \ Set QQ19+5 to A, which now contains the correct indent
                            \ for this view
    
     LDA QQ19               \ Set A = crosshairs x-coordinate - crosshairs size
     SEC                    \ to get the x-coordinate of the left edge of the
     SBC QQ19+2             \ crosshairs
    
     BIT QQ11               \ If bit 7 of QQ11 is set, then this this is the
     BMI TT84               \ Short-range Chart, so jump to TT84
    
     BCC botchfix13         \ If the above subtraction underflowed, then A is
                            \ positive, so skip the next two instructions
    
     CMP #2                 \ If A >= 2, skip the next instruction
     BCS TT84
    
    .botchfix13
    
     LDA #2                 \ The subtraction underflowed or A < 2, so set A to 2
                            \ so the crosshairs don't spill out of the left of the
                            \ screen
    
    .TT84
    
     STA X1                 \ Set X1 = A (the x-coordinate of the left edge of the
                            \ crosshairs)
    
     LDA QQ19               \ Set A = crosshairs x-coordinate + crosshairs size
     CLC                    \ to get the x-coordinate of the right edge of the
     ADC QQ19+2             \ crosshairs
    
    \BIT QQ11               \ These instructions are commented out in the original
    \BMI TT85               \ source
    
     BCS botchfix12         \ If the above addition overflowed, skip the following
                            \ two instructions to set A = 254
    
     CMP #254               \ The addition didn't overflow, so if A < 254, jump to
     BCC TT85               \ TT85
    
    .botchfix12
    
     LDA #254               \ Set A = 254, so the crosshairs don't spill out of the
                            \ right of the screen
    
    .TT85
    
     STA X2                 \ Set X2 = A (the x-coordinate of the right edge of the
                            \ crosshairs)
    
     LDA QQ19+1             \ Set Y1 = crosshairs y-coordinate + indent to get the
     CLC                    \ y-coordinate of the centre of the crosshairs
     ADC QQ19+5
     STA Y1
    
     JSR HLOIN3             \ Call HLOIN3 to draw a line from (X1, Y1) to (X2, Y1)
    
     LDA QQ19+1             \ Set A = crosshairs y-coordinate - crosshairs size
     SEC                    \ to get the y-coordinate of the top edge of the
     SBC QQ19+2             \ crosshairs
    
     BCS TT86               \ If the above subtraction didn't underflow, then A is
                            \ correct, so skip the next instruction
    
     LDA #0                 \ The subtraction underflowed, so set A to 0 so the
                            \ crosshairs don't spill out of the top of the screen
    
    .TT86
    
     CLC                    \ Set Y1 = A + indent to get the y-coordinate of the top
     ADC QQ19+5             \ edge of the indented crosshairs
     STA Y1
    
     LDA QQ19+1             \ Set A = crosshairs y-coordinate + crosshairs size
     CLC                    \ + indent to get the y-coordinate of the bottom edge
     ADC QQ19+2             \ of the indented crosshairs
     ADC QQ19+5
    
     CMP #GCYB              \ If A < GCYB then skip the following, as the crosshairs
     BCC TT87               \ won't spill out of the bottom of the screen
    
     LDX QQ11               \ A >= 152, so we need to check whether this will fit in
                            \ this view, so fetch the view type
    
     BMI TT87               \ If this is the Short-range Chart then the y-coordinate
                            \ is fine, so skip to TT87
    
     LDA #GCYB              \ Otherwise this is the Long-range Chart, so we need to
                            \ clip the crosshairs at a maximum y-coordinate of GCYB
    
    .TT87
    
     STA Y2                 \ Set Y2 = A (the y-coordinate of the bottom edge of the
                            \ crosshairs)
    
     LDA QQ19               \ Set X1 = the x-coordinate of the centre of the
     STA X1                 \ crosshairs
    
     STA X2                 \ Set X2 = the x-coordinate of the centre of the
                            \ crosshairs
    
     JMP LOIN               \ Draw a vertical line from (X1, Y1) to (X2, Y2), which
                            \ will draw from the top edge of the crosshairs to the
                            \ bottom edge, through the centre of the crosshairs,
                            \ and returning from the subroutine using a tail call
    
    
    
    
           Name: TT14                                                    [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw a circle with crosshairs on a chart
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt14.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt14.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT22](../main/subroutine/tt22.md) calls TT14
                 * [TT23](../main/subroutine/tt23.md) calls TT14
    
    
    
    
    
    * * *
    
    
     Draw a circle with crosshairs at the current system's galactic coordinates.
    
    
    
    
    .TT126
    
     LDA #104               \ Set QQ19 = 104, for the x-coordinate of the centre of
     STA QQ19               \ the fixed circle on the Short-range Chart
    
     LDA #90                \ Set QQ19+1 = 90, for the y-coordinate of the centre of
     STA QQ19+1             \ the fixed circle on the Short-range Chart
    
     LDA #16                \ Set QQ19+2 = 16, the size of the crosshairs on the
     STA QQ19+2             \ Short-range Chart
    
     LDA #GREEN             \ Switch to stripe 3-1-3-1, which is white/yellow in the
     STA COL                \ chart view
    
     JSR TT15               \ Draw the set of crosshairs defined in QQ19, at the
                            \ exact coordinates as this is the Short-range Chart
    
     LDA QQ14               \ Set K to the fuel level from QQ14, so this can act as
     JSR SCALEY2            \ the circle's radius (70 being a full tank)
     STA K                  \
                            \ The call to SCALEY2 has no effect as it only contains
                            \ an RTS, but having this call instruction here would
                            \ enable different scaling to be applied by altering
                            \ the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     JMP TT128              \ Jump to TT128 to draw a circle with the centre at the
                            \ same coordinates as the crosshairs, (QQ19, QQ19+1),
                            \ and radius K that reflects the current fuel levels,
                            \ returning from the subroutine using a tail call
    
    .TT14
    
     LDA QQ11               \ If the current view is the Short-range Chart, which
     BMI TT126              \ is the only view with bit 7 set, then jump up to TT126
                            \ to draw the crosshairs and circle for that view
    
                            \ Otherwise this is the Long-range Chart, so we draw the
                            \ crosshairs and circle for that view instead
    
     LDA QQ14               \ Set K to the fuel level from QQ14 divided by 4, so
     LSR A                  \ this can act as the circle's radius (70 being a full
     JSR SCALEY             \ tank, which divides down to a radius of 17)
     STA K                  \
                            \ The call to SCALEY simply does an LSR A, but having
                            \ this call instruction here would enable different
                            \ scaling to be applied by altering the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LDA QQ0                \ Set QQ19 to the x-coordinate of the current system,
     JSR SCALEX             \ which will be the centre of the circle and crosshairs
     STA QQ19               \ we draw
                            \
                            \ The call to SCALEX has no effect as it only contains
                            \ an RTS, but having this call instruction here would
                            \ enable different scaling to be applied by altering
                            \ the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LDA QQ1                \ Set QQ19+1 to the y-coordinate of the current system,
     JSR SCALEY             \ halved because the galactic chart is half as high as
     STA QQ19+1             \ it is wide, which will again be the centre of the
                            \ circle and crosshairs we draw
                            \
                            \ Again, the call to SCALEY simply does an LSR A (see
                            \ the comment above)
    
     LDA #7                 \ Set QQ19+2 = 7, the size of the crosshairs on the
     STA QQ19+2             \ Long-range Chart
    
     LDA #CYAN              \ Switch to colour 3, which is white in the chart view
     STA COL
    
     JSR TT15               \ Draw the set of crosshairs defined in QQ19, which will
                            \ be drawn 24 pixels to the right of QQ19+1
    
     LDA QQ19+1             \ Add GCYT to the y-coordinate of the crosshairs in
     CLC                    \ QQ19+1 so that the centre of the circle matches the
     ADC #GCYT              \ centre of the crosshairs
     STA QQ19+1
    
                            \ Fall through into TT128 to draw a circle with the
                            \ centre at the same coordinates as the crosshairs,
                            \ (QQ19, QQ19+1), and radius K that reflects the
                            \ current fuel levels
    
    
    
    
           Name: TT128                                                   [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw a circle on a chart
      Deep dive: [Drawing circles](https://elite.bbcelite.com/deep_dives/drawing_circles.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt128.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt128.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT14](../main/subroutine/tt14.md) calls TT128
    
    
    
    
    
    * * *
    
    
     Draw a circle with the centre at (QQ19, QQ19+1) and radius K.
    
    
    
    * * *
    
    
     Arguments:
    
       QQ19                 The x-coordinate of the centre of the circle
    
       QQ19+1               The y-coordinate of the centre of the circle
    
       K                    The radius of the circle
    
    
    
    
    .TT128
    
     LDA QQ19               \ Set K3 = the x-coordinate of the centre
     STA K3
    
     LDA QQ19+1             \ Set K4 = the y-coordinate of the centre
     STA K4
    
     STZ K4+1               \ Set the high bytes of K3(1 0) and K4(1 0) to 0
     STZ K3+1
    
     LDX #1                 \ Set LSP = 1 to reset the ball line heap
     STX LSP
    
     INX                    \ Set STP = 2, the step size for the circle
     STX STP
    
     LDA #RED               \ Switch to colour 2, which is red in the chart view
     STA COL
    
     JMP CIRCLE2            \ Jump to CIRCLE2 to draw a circle with the centre at
                            \ (K3(1 0), K4(1 0)) and radius K, returning from the
                            \ subroutine using a tail call
    
    
    
    
           Name: TT219                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Show the Buy Cargo screen (red key f1)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt219.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt219.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT219
                 * [gnum](../main/subroutine/gnum.md) calls via BAY2
                 * [TT210](../main/subroutine/tt210.md) calls via BAY2
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       BAY2                 Jump into the main loop at FRCE, setting the key
                            "pressed" to red key f9 (so we show the Inventory
                            screen)
    
    
    
    
    .TT219
    
     LDA #2                 \ Clear the top part of the screen, draw a border box,
     JSR TRADEMODE          \ and set up a trading screen with a view type in QQ11
                            \ of 2 (Buy Cargo screen)
    
     JSR TT163              \ Print the column headers for the prices table
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch to Sentence Case, with the
     STA QQ17               \ next letter in capitals
    
     LDA #0                 \ We're going to loop through all the available market
     STA QQ29               \ items, so we set up a counter in QQ29 to denote the
                            \ current item and start it at 0
    
    .TT220
    
     JSR TT151              \ Call TT151 to print the item name, market price and
                            \ availability of the current item, and set QQ24 to the
                            \ item's price / 4, QQ25 to the quantity available and
                            \ QQ19+1 to byte #1 from the market prices table for
                            \ this item
    
     LDA QQ25               \ If there are some of the current item available, jump
     BNE TT224              \ to TT224 below to see if we want to buy any
    
     JMP TT222              \ Otherwise there are none available, so jump down to
                            \ TT222 to skip this item
    
    .TQ4
    
     LDY #176               \ Set Y to the recursive token 16 ("QUANTITY")
    
    .Tc
    
     JSR TT162              \ Print a space
    
     TYA                    \ Print the recursive token in Y followed by a question
     JSR prq                \ mark
    
    .TTX224
    
     JSR dn2                \ Call dn2 to make a short, high beep and delay for 1
                            \ second
    
    .TT224
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
     LDA #204               \ Print recursive token 44 ("QUANTITY OF ")
     JSR TT27
    
     LDA QQ29               \ Print recursive token 48 + QQ29, which will be in the
     CLC                    \ range 48 ("FOOD") to 64 ("ALIEN ITEMS"), so this
     ADC #208               \ prints the current item's name
     JSR TT27
    
     LDA #'/'               \ Print "/"
     JSR TT27
    
     JSR TT152              \ Print the unit ("t", "kg" or "g") for the current item
                            \ (as the call to TT151 above set QQ19+1 with the
                            \ appropriate value)
    
     LDA #'?'               \ Print "?"
     JSR TT27
    
     JSR TT67               \ Print a newline
    
     LDX #0                 \ These instructions have no effect, as they are
     STX R                  \ repeated at the start of gnum, which we call next.
     LDX #12                \ Perhaps they were left behind when code was moved from
     STX T1                 \ here into gnum, and weren't deleted?
    
    .TT223K                 \ This label is a duplicate of a label in the gnum
                            \ routine, so this could also be a remnant from code
                            \ that got moved into the gnum subroutine
                            \
                            \ In the original source this label is TT223, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to TT223K
    
     JSR gnum               \ Call gnum to get a number from the keyboard, which
                            \ will be the quantity of this item we want to purchase,
                            \ returning the number entered in A and R
    
     BCS TQ4                \ If gnum set the C flag, the number entered is greater
                            \ than the quantity available, so jump up to TQ4 to
                            \ display a "Quantity?" error, beep, clear the number
                            \ and try again
    
     STA P                  \ Otherwise we have a valid purchase quantity entered,
                            \ so store the amount we want to purchase in P
    
     JSR tnpr               \ Call tnpr to work out whether there is room in the
                            \ cargo hold for this item
    
     LDY #206               \ Set Y to recursive token 46 (" CARGO{sentence case}")
                            \ to pass to the Tc routine if we call it
    
     LDA R                  \ If R = 0, then we didn't enter a number above, so skip
     BEQ P%+4               \ the following instruction
    
     BCS Tc                 \ If the C flag is set, then there is no room in the
                            \ cargo hold, jump up to Tc to print a "Cargo?" error,
                            \ beep, clear the number and try again
    
     LDA QQ24               \ There is room in the cargo hold, so now to check
     STA Q                  \ whether we have enough cash, so fetch the item's
                            \ price / 4, which was returned in QQ24 by the call
                            \ to TT151 above and store it in Q
    
     JSR GCASH              \ Call GCASH to calculate:
                            \
                            \   (Y X) = P * Q * 4
                            \
                            \ which will be the total price of this transaction
                            \ (as P contains the purchase quantity and Q contains
                            \ the item's price / 4)
    
     JSR LCASH              \ Subtract (Y X) cash from the cash pot in CASH
    
     LDY #197               \ If the C flag is clear, we didn't have enough cash,
     BCC Tc                 \ so set Y to the recursive token 37 ("CASH") and jump
                            \ up to Tc to print a "Cash?" error, beep, clear the
                            \ number and try again
    
     LDY QQ29               \ Fetch the current market item number from QQ29 into Y
    
     LDA R                  \ Set A to the number of items we just purchased (this
                            \ was set by gnum above)
    
     PHA                    \ Store the quantity just purchased on the stack
    
     CLC                    \ Add the number purchased to the Y-th byte of QQ20,
     ADC QQ20,Y             \ which contains the number of items of this type in
     STA QQ20,Y             \ our hold (so this transfers the bought items into our
                            \ cargo hold)
    
     LDA AVL,Y              \ Subtract the number of items from the Y-th byte of
     SEC                    \ AVL, which contains the number of items of this type
     SBC R                  \ that are available on the market
     STA AVL,Y
    
     PLA                    \ Restore the quantity just purchased
    
     BEQ TT222              \ If we didn't buy anything, jump to TT222 to skip the
                            \ following instruction
    
     JSR dn                 \ Call dn to print the amount of cash left in the cash
                            \ pot, then make a short, high beep to confirm the
                            \ purchase, and delay for 1 second
    
    .TT222
    
     LDA QQ29               \ Move the text cursor to row QQ29 + 5 (where QQ29 is
     CLC                    \ the item number, starting from 0)
     ADC #5
     JSR DOYC
    
     LDA #0                 \ Move the text cursor to column 0
     JSR DOXC
    
     INC QQ29               \ Increment QQ29 to point to the next item
    
     LDA QQ29               \ If QQ29 >= 17 then jump to BAY2 as we have done the
     CMP #17                \ last item
     BCS BAY2
    
     JMP TT220              \ Otherwise loop back to TT220 to print the next market
                            \ item
    
    .BAY2
    
    \LDA #&10               \ These instructions are commented out in the original
    \STA COL2               \ source
    
     LDA #f9                \ Jump into the main loop at FRCE, setting the key
     JMP FRCE               \ "pressed" to red key f9 (so we show the Inventory
                            \ screen)
    
    
    
    
           Name: gnum                                                    [Show more]
           Type: Subroutine
       Category: Market
        Summary: Get a number from the keyboard
    
    
        Context: See this subroutine [on its own page](../main/subroutine/gnum.md)
     Variations: See [code variations](../related/compare/main/subroutine/gnum.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls gnum
                 * [TT210](../main/subroutine/tt210.md) calls gnum
                 * [TT219](../main/subroutine/tt219.md) calls gnum
                 * [OUTK](../main/subroutine/outk.md) calls via OUT
    
    
    
    
    
    * * *
    
    
     Get a number from the keyboard, up to the maximum number in QQ25, for the
     buying and selling of cargo and equipment.
    
     Pressing "Y" will return the maximum number (i.e. buy/sell all items), while
     pressing "N" will abort the sale and return a 0.
    
     Pressing a key with an ASCII code less than ASCII "0" will return a 0 in A (so
     that includes pressing Space or Return), while pressing a key with an ASCII
     code greater than ASCII "9" will jump to the Inventory screen (so that
     includes all letters and most punctuation).
    
    
    
    * * *
    
    
     Arguments:
    
       QQ25                 The maximum number allowed
    
    
    
    * * *
    
    
     Returns:
    
       A                    The number entered
    
       R                    Also contains the number entered
    
       C flag               Set if the number is too large (> QQ25), clear otherwise
    
    
    
    * * *
    
    
     Other entry points:
    
       OUT                  The OUTK routine jumps back here after printing the key
                            that was just pressed
    
    
    
    
    .gnum
    
     LDA #MAGENTA           \ Switch to colour 2, which is magenta in the trade view
     STA COL
    
     LDX #0                 \ We will build the number entered in R, so initialise
     STX R                  \ it with 0
    
     LDX #12                \ We will check for up to 12 key presses, so set a
     STX T1                 \ counter in T1
    
    .TT223
    
     JSR TT217              \ Scan the keyboard until a key is pressed, and return
                            \ the key's ASCII code in A (and X)
    
     LDX R                  \ If R is non-zero then skip to NWDAV2, as we are
     BNE NWDAV2             \ already building a number
    
     CMP #'Y'               \ If "Y" was pressed, jump to NWDAV1 to return the
     BEQ NWDAV1             \ maximum number allowed (i.e. buy/sell the whole stock)
    
     CMP #'N'               \ If "N" was pressed, jump to NWDAV3 to return from the
     BEQ NWDAV3             \ subroutine with a result of 0 (i.e. abort transaction)
    
    .NWDAV2
    
     STA Q                  \ Store the key pressed in Q
    
     SEC                    \ Subtract ASCII "0" from the key pressed, to leave the
     SBC #'0'               \ numeric value of the key in A (if it was a number key)
    
     BCC OUT                \ If A < 0, jump to OUT to load the current number and
                            \ return from the subroutine, as the key pressed was
                            \ RETURN (or some other character with a value less than
                            \ ASCII "0")
    
     CMP #10                \ If A >= 10, jump to BAY2 to display the Inventory
     BCS BAY2               \ screen, as the key pressed was a letter or other
                            \ non-digit and is greater than ASCII "9"
    
     STA S                  \ Store the numeric value of the key pressed in S
    
     LDA R                  \ Fetch the result so far into A
    
    \BEQ P%+4               \ This instruction is commented out in the original
                            \ source, and has a comment "tribs"
    
     CMP #26                \ If A >= 26, where A is the number entered so far, then
     BCS OUTK               \ adding a further digit will make it bigger than 256,
                            \ so jump to OUTK to print the key that was just pressed
                            \ before jumping to OUT below with the C flag still set
    
     ASL A                  \ Set A = (A * 2) + (A * 8) = A * 10
     STA T
     ASL A
     ASL A
     ADC T
    
     ADC S                  \ Add the pressed digit to A
    
     BCS OUTK               \ If the addition overflowed, then jump to OUTK to print
                            \ the key that was just pressed before jumping to OUT
                            \ below with the C flag still set
    
     STA R                  \ Otherwise store the result in R, so R now contains
                            \ its previous value with the new key press tacked onto
                            \ the end
    
     CMP QQ25               \ If the result in R = the maximum allowed in QQ25, jump
     BEQ TT226              \ to TT226 to print the key press and keep looping (the
                            \ BEQ is needed because the BCS below would jump to OUT
                            \ if R >= QQ25, which we don't want)
    
     BCS OUTK               \ If the result in R > QQ25, jump to OUTK to print
                            \ the key that was just pressed before jumping to OUT
                            \ below with the C flag still set
    
    .TT226
    
     LDA Q                  \ Print the character in Q (i.e. the key that was
     JSR TT26               \ pressed, as we stored the ASCII value in Q earlier)
    
     DEC T1                 \ Decrement the loop counter
    
     BNE TT223              \ Loop back to TT223 until we have checked for 12 digits
    
    .OUT
    
     LDA #CYAN              \ Switch to colour 3, which is white in the trade view
     STA COL
    
     LDA R                  \ Set A to the result we have been building in R
    
     RTS                    \ Return from the subroutine
    
    .NWDAV1
    
                            \ If we get here then "Y" was pressed, so we return the
                            \ maximum number allowed, which is in QQ25
    
     JSR TT26               \ Print the character for the key that was pressed
    
     LDA QQ25               \ Set R = QQ25, so we return the maximum value allowed
     STA R
    
     JMP OUT                \ Jump to OUT to return from the subroutine
    
    .NWDAV3
    
                            \ If we get here then "N" was pressed, so we return 0
    
     JSR TT26               \ Print the character for the key that was pressed
    
     STZ R                  \ Set R = 0, so we return 0
    
     JMP OUT                \ Jump to OUT to return from the subroutine
    
    
    
    
           Name: NWDAV4                                                  [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print an "ITEM?" error, make a beep and rejoin the TT210 routine
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nwdav4.md)
     References: This subroutine is called as follows:
                 * [TT210](../main/subroutine/tt210.md) calls NWDAV4
    
    
    
    
    
    
    .NWDAV4
    
     JSR TT67               \ Print a newline
    
     LDA #176               \ Print recursive token 127 ("ITEM") followed by a
     JSR prq                \ question mark
    
     JSR dn2                \ Call dn2 to make a short, high beep and delay for 1
                            \ second
    
     LDY QQ29               \ Fetch the item number we are selling from QQ29
    
     JMP NWDAVxx            \ Jump back into the TT210 routine that called NWDAV4
    
    
    
    
           Name: OUTK                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print the character in Q before returning to gnum
    
    
        Context: See this subroutine [on its own page](../main/subroutine/outk.md)
     References: This subroutine is called as follows:
                 * [gnum](../main/subroutine/gnum.md) calls OUTK
    
    
    
    
    
    
    .OUTK
    
     LDA Q                  \ Print the character in Q, which is the key that was
     JSR DASC               \ just pressed in the gnum routine
    
     SEC                    \ Set the C flag, as this routine is only called if the
                            \ key pressed makes the number too high
    
     JMP OUT                \ Jump back into the gnum routine to return the number
                            \ that has been built
    
    
    
    
           Name: TT208                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Show the Sell Cargo screen (red key f2)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt208.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt208.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT208
    
    
    
    
    
    
    .TT208
    
     LDA #4                 \ Clear the top part of the screen, draw a border box,
     JSR TRADEMODE          \ and set up a trading screen with a view type in QQ11
                            \ of 4 (Sell Cargo screen)
    
     LDA #10                \ Move the text cursor to column 10
     STA XC
    
     LDA #205               \ Print recursive token 45 ("SELL")
     JSR TT27
    
     LDA #206               \ Print recursive token 46 (" CARGO{sentence case}")
     JSR NLIN3              \ draw a horizontal line at pixel row 19 to box in the
                            \ title
    
     JSR TT67               \ Print a newline
    
                            \ Fall through into TT210 to show the Inventory screen
                            \ with the option to sell
    
    
    
    
           Name: TT210                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Show a list of current cargo in our hold, optionally to sell
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt210.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt210.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT213](../main/subroutine/tt213.md) calls TT210
                 * [NWDAV4](../main/subroutine/nwdav4.md) calls via NWDAVxx
    
    
    
    
    
    * * *
    
    
     Show a list of current cargo in our hold, either with the ability to sell (the
     Sell Cargo screen) or without (the Inventory screen), depending on the current
     view.
    
    
    
    * * *
    
    
     Arguments:
    
       QQ11                 The current view:
    
                                * 4 = Sell Cargo
    
                                * 8 = Inventory
    
    
    
    * * *
    
    
     Other entry points:
    
       NWDAVxx              Used to rejoin this routine from the call to NWDAV4
    
    
    
    
    .TT210
    
     LDY #0                 \ We're going to loop through all the available market
                            \ items and check whether we have any in the hold (and,
                            \ if we are in the Sell Cargo screen, whether we want
                            \ to sell any items), so we set up a counter in Y to
                            \ denote the current item and start it at 0
    
    .TT211
    
     STY QQ29               \ Store the current item number in QQ29
    
    .NWDAVxx
    
     LDX QQ20,Y             \ Fetch into X the amount of the current item that we
     BEQ TT212              \ have in our cargo hold, which is stored in QQ20+Y,
                            \ and if there are no items of this type in the hold,
                            \ jump down to TT212 to skip to the next item
    
     TYA                    \ Set Y = Y * 4, so this will act as an index into the
     ASL A                  \ market prices table at QQ23 for this item (as there
     ASL A                  \ are four bytes per item in the table)
     TAY
    
     LDA QQ23+1,Y           \ Fetch byte #1 from the market prices table for the
     STA QQ19+1             \ current item and store it in QQ19+1, for use by the
                            \ call to TT152 below
    
     TXA                    \ Store the amount of item in the hold (in X) on the
     PHA                    \ stack
    
     JSR TT69               \ Call TT69 to set Sentence Case and print a newline
    
     CLC                    \ Print recursive token 48 + QQ29, which will be in the
     LDA QQ29               \ range 48 ("FOOD") to 64 ("ALIEN ITEMS"), so this
     ADC #208               \ prints the current item's name
     JSR TT27
    
     LDA #14                \ Move the text cursor to column 14, for the item's
     JSR DOXC               \ quantity
    
     PLA                    \ Restore the amount of item in the hold into X
     TAX
    
     STA QQ25               \ Store the amount of this item in the hold in QQ25
    
     CLC                    \ Print the 8-bit number in X to 3 digits, without a
     JSR pr2                \ decimal point
    
     JSR TT152              \ Print the unit ("t", "kg" or "g") for the market item
                            \ whose byte #1 from the market prices table is in
                            \ QQ19+1 (which we set up above)
    
     LDA QQ11               \ If the current view type in QQ11 is not 4 (Sell Cargo
     CMP #4                 \ screen), jump to TT212 to skip the option to sell
     BNE TT212              \ items
    
    \JSR TT162              \ This instruction is commented out in the original
                            \ source
    
     LDA #205               \ Print recursive token 45 ("SELL")
     JSR TT27
    
     LDA #206               \ Print extended token 206 ("{all caps}(Y/N)?")
     JSR DETOK
    
     JSR gnum               \ Call gnum to get a number from the keyboard, which
                            \ will be the number of the item we want to sell,
                            \ returning the number entered in A and R, and setting
                            \ the C flag if the number is bigger than the available
                            \ amount of this item in QQ25
    
     BEQ TT212              \ If no number was entered, jump to TT212 to move on to
                            \ the next item
    
     BCS NWDAV4             \ If the number entered was too big, jump to NWDAV4 to
                            \ print an "ITEM?" error, make a beep and rejoin the
                            \ routine at NWDAVxx above
    
     LDA QQ29               \ We are selling this item, so fetch the item number
                            \ from QQ29
    
     LDX #255               \ Set QQ17 = 255 to disable printing
     STX QQ17
    
     JSR TT151              \ Call TT151 to set QQ24 to the item's price / 4 (the
                            \ routine doesn't print the item details, as we just
                            \ disabled printing)
    
     LDY QQ29               \ Subtract R (the number of items we just asked to buy)
     LDA QQ20,Y             \ from the available amount of this item in QQ20, as we
     SEC                    \ just bought them
     SBC R
     STA QQ20,Y
    
     LDA R                  \ Set P to the amount of this item we just bought
     STA P
    
     LDA QQ24               \ Set Q to the item's price / 4
     STA Q
    
     JSR GCASH              \ Call GCASH to calculate
                            \
                            \   (Y X) = P * Q * 4
                            \
                            \ which will be the total price we make from this sale
                            \ (as P contains the quantity we're selling and Q
                            \ contains the item's price / 4)
    
     JSR MCASH              \ Add (Y X) cash to the cash pot in CASH
    
     LDA #0                 \ We've made the sale, so set the amount
    
     STA QQ17               \ Set QQ17 = 0, which enables printing again
    
    .TT212
    
     LDY QQ29               \ Fetch the item number from QQ29 into Y, and increment
     INY                    \ Y to point to the next item
    
     CPY #17                \ Loop back to TT211 to print the next item in the hold
     BCC TT211              \ until Y = 17 (at which point we have done the last
                            \ item)
    
     LDA QQ11               \ If the current view type in QQ11 is not 4 (Sell Cargo
     CMP #4                 \ screen), skip the next two instructions and move on to
     BNE P%+8               \ printing the number of Trumbles
    
     JSR dn2                \ This is the Sell Cargo screen, so call dn2 to make a
                            \ short, high beep and delay for 1 second
    
     JMP BAY2               \ And then jump to BAY2 to display the Inventory
                            \ screen, as we have finished selling cargo
    
     JSR TT69               \ Call TT69 to set Sentence Case and print a newline
    
     LDA TRIBBLE            \ If there are any Trumbles in the hold, skip the
     ORA TRIBBLE+1          \ following RTS and continue on (in the Master version,
     BNE P%+3               \ there are never any Trumbles, so this value will
                            \ always be zero)
    
    .zebra
    
     RTS                    \ There are no Trumbles in the hold, so return from the
                            \ subroutine
    
                            \ If we get here then we have Trumbles in the hold, so
                            \ we print out the number (though we never get here in
                            \ the Master version)
    
     CLC                    \ Clear the C flag, so the call to TT11 below doesn't
                            \ include a decimal point
    
     LDA #0                 \ Set A = 0, for the call to TT11 below, so we don't pad
                            \ out the number of Trumbles
    
     LDX TRIBBLE            \ Fetch the number of Trumbles into (Y X)
     LDY TRIBBLE+1
    
     JSR TT11               \ Call TT11 to print the number of Trumbles in (Y X),
                            \ with no decimal point
    
     JSR DORND              \ Print out a random extended token from 111 to 114, all
     AND #3                 \ of which are blank in this version of Elite
     CLC
     ADC #111
     JSR DETOK
    
     LDA #198               \ Print extended token 198, which is blank, but would
     JSR DETOK              \ contain the text "LITTLE TRUMBLE" if the Trumbles
                            \ mission was enabled
    
     LDA TRIBBLE+1          \ If we have more than 256 Trumbles, skip to DOANS
     BNE DOANS
    
     LDX TRIBBLE            \ If we have exactly one Trumble, return from the
     DEX                    \ subroutine (as zebra contains an RTS)
     BEQ zebra
    
    .DOANS
    
     LDA #'s'               \ We have more than one Trumble, so print an 's' and
     JMP DASC               \ return from the subroutine using a tail call
    
    
    
    
           Name: TT213                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Show the Inventory screen (red key f9)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt213.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt213.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT213
    
    
    
    
    
    
    .TT213
    
     LDA #8                 \ Clear the top part of the screen, draw a border box,
     JSR TRADEMODE          \ and set up a trading screen with a view type in QQ11
                            \ of 4 (Inventory screen)
    
     LDA #11                \ Move the text cursor to column 11 to print the screen
     STA XC                 \ title
    
     LDA #164               \ Print recursive token 4 ("INVENTORY{crlf}") followed
     JSR TT60               \ by a paragraph break and Sentence Case
    
     JSR NLIN4              \ Draw a horizontal line at pixel row 19 to box in the
                            \ title. The authors could have used a call to NLIN3
                            \ instead and saved the above call to TT60, but you
                            \ just can't optimise everything
    
     JSR fwl                \ Call fwl to print the fuel and cash levels on two
                            \ separate lines
    
     LDA CRGO               \ If our ship's cargo capacity is < 26 (i.e. we do not
     CMP #26                \ have a cargo bay extension), skip the following two
     BCC P%+7               \ instructions
    
     LDA #107               \ We do have a cargo bay extension, so print recursive
     JSR TT27               \ token 107 ("LARGE CARGO{sentence case} BAY")
    
     JMP TT210              \ Jump to TT210 to print the contents of our cargo bay
                            \ and return from the subroutine using a tail call
    
    
    
    
           Name: TT214                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Ask a question with a "Y/N?" prompt and return the response
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt214.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt214.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to print before the "Y/N?" prompt
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the response was "yes", clear otherwise
    
    
    
    
    .TT214
    
    \.TT214                 \ These instructions are commented out in the original
    \                       \ source
    \PHA
    \JSR TT162
    \PLA
    
    .TT221
    
     JSR TT27               \ Print the text token in A
    
     LDA #206               \ Print extended token 206 ("{all caps}(Y/N)?")
     JSR DETOK
    
     JSR TT217              \ Scan the keyboard until a key is pressed, and return
                            \ the key's ASCII code in A and X
    
     ORA #%00100000         \ Set bit 5 in the value of the key pressed, which
                            \ converts it to lower case
    
     CMP #'y'               \ If "y" was pressed, jump to TT218
     BEQ TT218
    
     LDA #'n'               \ Otherwise jump to TT26 to print "n" and return from
     JMP TT26               \ the subroutine using a tail call (so all other
                            \ responses apart from "y" indicate a no)
    
    .TT218
    
     JSR TT26               \ Print the character in A, i.e. print "y"
    
     SEC                    \ Set the C flag to indicate a "yes" response
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT16                                                    [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Move the crosshairs on a chart
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt16.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt16.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT16
    
    
    
    
    
    * * *
    
    
     Move the chart crosshairs by the amount in X and Y.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The amount to move the crosshairs in the x-axis
    
       Y                    The amount to move the crosshairs in the y-axis
    
    
    
    
    .TT16
    
     TXA                    \ Push the change in X onto the stack (let's call this
     PHA                    \ the x-delta)
    
     DEY                    \ Negate the change in Y and push it onto the stack
     TYA                    \ (let's call this the y-delta)
     EOR #&FF
     PHA
    
     JSR WSCAN              \ Call WSCAN to wait for the vertical sync, so the whole
                            \ screen gets drawn and we can move the crosshairs with
                            \ no screen flicker
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will erase the crosshairs currently there
    
     PLA                    \ Store the y-delta in QQ19+3 and fetch the current
     STA QQ19+3             \ y-coordinate of the crosshairs from QQ10 into A, ready
     LDA QQ10               \ for the call to TT123
    
     JSR TT123              \ Call TT123 to move the selected system's galactic
                            \ y-coordinate by the y-delta, putting the new value in
                            \ QQ19+4
    
     LDA QQ19+4             \ Store the updated y-coordinate in QQ10 (the current
     STA QQ10               \ y-coordinate of the crosshairs)
    
     STA QQ19+1             \ This instruction has no effect, as QQ19+1 is
                            \ overwritten below, both in TT103 and TT105
    
     PLA                    \ Store the x-delta in QQ19+3 and fetch the current
     STA QQ19+3             \ x-coordinate of the crosshairs from QQ10 into A, ready
     LDA QQ9                \ for the call to TT123
    
     JSR TT123              \ Call TT123 to move the selected system's galactic
                            \ x-coordinate by the x-delta, putting the new value in
                            \ QQ19+4
    
     LDA QQ19+4             \ Store the updated x-coordinate in QQ9 (the current
     STA QQ9                \ x-coordinate of the crosshairs)
    
     STA QQ19               \ This instruction has no effect, as QQ19 is overwritten
                            \ below, both in TT103 and TT105
    
                            \ Now we've updated the coordinates of the crosshairs,
                            \ fall through into TT103 to redraw them at their new
                            \ location
    
    
    
    
           Name: TT103                                                   [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Draw a small set of crosshairs on a chart
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt103.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt103.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hm](../main/subroutine/hm.md) calls TT103
                 * [HME2](../main/subroutine/hme2.md) calls TT103
                 * [TT102](../main/subroutine/tt102.md) calls TT103
                 * [TT16](../main/subroutine/tt16.md) calls TT103
                 * [TT23](../main/subroutine/tt23.md) calls TT103
    
    
    
    
    
    * * *
    
    
     Draw a small set of crosshairs on a galactic chart at the coordinates in
     (QQ9, QQ10).
    
    
    
    
    .TT103
    
     LDA #GREEN             \ Switch to stripe 3-1-3-1, which is white/yellow in the
     STA COL                \ chart view
    
     LDA QQ11               \ Fetch the current view type into A
    
     BMI TT105              \ If this is the Short-range Chart screen, jump to TT105
    
     LDA QQ9                \ Store the crosshairs x-coordinate in QQ19
     JSR SCALEX             \
     STA QQ19               \ The call to SCALEX has no effect as it only contains
                            \ an RTS, but having this call instruction here would
                            \ enable different scaling to be applied by altering
                            \ the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LDA QQ10               \ Halve the crosshairs y-coordinate and store it in QQ19
     JSR SCALEY             \ (we halve it because the Long-range Chart is half as
     STA QQ19+1             \ high as it is wide)
                            \
                            \ The call to SCALEY simply does an LSR A, but having
                            \ this call instruction here would enable different
                            \ scaling to be applied by altering the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LDA #4                 \ Set QQ19+2 to 4 denote crosshairs of size 4
     STA QQ19+2
    
     JMP TT15               \ Jump to TT15 to draw crosshairs of size 4 at the
                            \ crosshairs coordinates, returning from the subroutine
                            \ using a tail call
    
    
    
    
           Name: TT123                                                   [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Move galactic coordinates by a signed delta
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt123.md)
     References: This subroutine is called as follows:
                 * [TT16](../main/subroutine/tt16.md) calls TT123
                 * [TT105](../main/subroutine/tt105.md) calls via TT180
    
    
    
    
    
    * * *
    
    
     Move an 8-bit galactic coordinate by a certain distance in either direction
     (i.e. a signed 8-bit delta), but only if it doesn't cause the coordinate to
     overflow. The coordinate is in a single axis, so it's either an x-coordinate
     or a y-coordinate.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The galactic coordinate to update
    
       QQ19+3               The delta (can be positive or negative)
    
    
    
    * * *
    
    
     Returns:
    
       QQ19+4               The updated coordinate after moving by the delta (this
                            will be the same as A if moving by the delta overflows)
    
    
    
    * * *
    
    
     Other entry points:
    
       TT180                Contains an RTS
    
    
    
    
    .TT123
    
     STA QQ19+4             \ Store the original coordinate in temporary storage at
                            \ QQ19+4
    
     CLC                    \ Set A = A + QQ19+3, so A now contains the original
     ADC QQ19+3             \ coordinate, moved by the delta
    
     LDX QQ19+3             \ If the delta is negative, jump to TT124
     BMI TT124
    
     BCC TT125              \ If the C flag is clear, then the above addition didn't
                            \ overflow, so jump to TT125 to return the updated value
    
     RTS                    \ Otherwise the C flag is set and the above addition
                            \ overflowed, so do not update the return value
    
    .TT124
    
     BCC TT180              \ If the C flag is clear, then because the delta is
                            \ negative, this indicates the addition (which is
                            \ effectively a subtraction) underflowed, so jump to
                            \ TT180 to return from the subroutine without updating
                            \ the return value
    
    .TT125
    
     STA QQ19+4             \ Store the updated coordinate in QQ19+4
    
    .TT180
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT105                                                   [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Draw crosshairs on the Short-range Chart, with clipping
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt105.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt105.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT103](../main/subroutine/tt103.md) calls TT105
    
    
    
    
    
    * * *
    
    
     Check whether the crosshairs are close enough to the current system to appear
     on the Short-range Chart, and if so, draw them.
    
    
    
    
    .TT105
    
     LDA QQ9                \ Set A = QQ9 - QQ0, the horizontal distance between the
     SEC                    \ crosshairs (QQ9) and the current system (QQ0)
     SBC QQ0
    
     BCS P%+6               \ If the subtraction didn't underflow, skip the next two
                            \ instructions
    
     EOR #&FF               \ The subtraction underflowed, so negate the result
     ADC #1                 \ using two's complement so that it is positive, i.e.
                            \ A = |QQ9 - QQ0|, the absolute horizontal distance
    
     CMP #29                \ If the absolute horizontal distance in A >= 29, then
     BCS TT180              \ the crosshairs are too far from the current system to
                            \ appear in the Short-range Chart, so jump to TT180 to
                            \ return from the subroutine (as TT180 contains an RTS)
    
     LDA QQ9                \ Set A = QQ9 - QQ0, the horizontal distance between the
     SEC                    \ crosshairs (QQ9) and the current system (QQ0)
     SBC QQ0
    
     BPL TT179              \ If the horizontal distance in A is positive, then skip
                            \ the next two instructions
    
     CMP #233               \ If the horizontal distance in A < -23, then the
     BCC TT180              \ crosshairs are too far from the current system to
                            \ appear in the Short-range Chart, so jump to TT180 to
                            \ return from the subroutine (as TT180 contains an RTS)
    
    .TT179
    
     ASL A                  \ Set QQ19 = 104 + A * 4
     ASL A                  \
     CLC                    \ 104 is the x-coordinate of the centre of the chart,
     ADC #104               \ so this sets QQ19 to the screen pixel x-coordinate
     JSR SCALEY2            \
     STA QQ19               \ The call to SCALEY2 has no effect as it only contains
                            \ an RTS, but having this call instruction here would
                            \ enable different scaling to be applied by altering
                            \ the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LDA QQ10               \ Set A = QQ10 - QQ1, the vertical distance between the
     SEC                    \ crosshairs (QQ10) and the current system (QQ1)
     SBC QQ1
    
     BCS P%+6               \ If the subtraction didn't underflow, skip the next two
                            \ instructions
    
     EOR #&FF               \ The subtraction underflowed, so negate the result
     ADC #1                 \ using two's complement so that it is positive, i.e.
                            \ A = |QQ10 - QQ0|, the absolute vertical distance
    
     CMP #35                \ If the absolute vertical distance in A >= 35, then
     BCS TT180              \ the crosshairs are too far from the current system to
                            \ appear in the Short-range Chart, so jump to TT180 to
                            \ return from the subroutine (as TT180 contains an RTS)
    
     LDA QQ10               \ Set A = QQ10 - QQ1, the vertical distance between the
     SEC                    \ crosshairs (QQ10) and the current system (QQ1)
     SBC QQ1
    
     ASL A                  \ Set QQ19+1 = 90 + A * 2
     CLC                    \
     ADC #90                \ 90 is the y-coordinate of the centre of the chart,
     JSR SCALEY2            \ so this sets QQ19+1 to the screen pixel x-coordinate
     STA QQ19+1             \ of the crosshairs
                            \
                            \ The call to SCALEY2 has no effect as it only contains
                            \ an RTS, but having this call instruction here would
                            \ enable different scaling to be applied by altering
                            \ the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LDA #8                 \ Set QQ19+2 to 8 denote crosshairs of size 8
     STA QQ19+2
    
     LDA #GREEN             \ Switch to stripe 3-1-3-1, which is white/yellow in the
     STA COL                \ chart view
    
     JMP TT15               \ Jump to TT15 to draw crosshairs of size 8 at the
                            \ crosshairs coordinates, returning from the subroutine
                            \ using a tail call
    
    
    
    
           Name: TT23                                                    [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Show the Short-range Chart (red key f5)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt23.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt23.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT23
                 * [TT114](../main/subroutine/tt114.md) calls TT23
    
    
    
    
    
    
    .TT23
    
     LDA #128               \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 128 (Short-
                            \ range Chart)
    
     LDA #16                \ Switch to the mode 1 palette for the trade view, which
     JSR DOVDU19            \ is yellow (colour 1), magenta (colour 2) and white
                            \ (colour 3)
    
     LDA #CYAN              \ Switch to colour 3, which is white in the chart view
     STA COL
    
     LDA #7                 \ Move the text cursor to column 7
     STA XC
    
     LDA #190               \ Print recursive token 30 ("SHORT RANGE CHART") and
     JSR NLIN3              \ draw a horizontal line at pixel row 19 to box in the
                            \ title
    
     JSR TT14               \ Call TT14 to draw a circle with crosshairs at the
                            \ current system's galactic coordinates
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ i.e. at the selected system
    
     JSR TT81               \ Set the seeds in QQ15 to those of system 0 in the
                            \ current galaxy (i.e. copy the seeds from QQ21 to QQ15)
    
     LDA #CYAN              \ Switch to colour 3, which is white in the chart view
     STA COL
    
     LDA #0                 \ Set A = 0, which we'll use below to zero out the INWK
                            \ workspace
    
     STA XX20               \ We're about to start working our way through each of
                            \ the galaxy's systems, so set up a counter in XX20 for
                            \ each system, starting at 0 and looping through to 255
    
     LDX #24                \ First, though, we need to zero out the 25 bytes at
                            \ INWK so we can use them to work out which systems have
                            \ room for a label, so set a counter in X for 25 bytes
    
    .EE3
    
     STA INWK,X             \ Set the X-th byte of INWK to zero
    
     DEX                    \ Decrement the counter
    
     BPL EE3                \ Loop back to EE3 for the next byte until we've zeroed
                            \ all 25 bytes
    
                            \ We now loop through every single system in the galaxy
                            \ and check the distance from the current system whose
                            \ coordinates are in (QQ0, QQ1). We get the galactic
                            \ coordinates of each system from the system's seeds,
                            \ like this:
                            \
                            \   x = s1_hi (which is stored in QQ15+3)
                            \   y = s0_hi (which is stored in QQ15+1)
                            \
                            \ so the following loops through each system in the
                            \ galaxy in turn and calculates the distance between
                            \ (QQ0, QQ1) and (s1_hi, s0_hi) to find the closest one
    
    .TT182
    
     LDA QQ15+3             \ Set A = s1_hi - QQ0, the horizontal distance between
     SEC                    \ (s1_hi, s0_hi) and (QQ0, QQ1)
     SBC QQ0
    
     BCS TT184              \ If a borrow didn't occur, i.e. s1_hi >= QQ0, then the
                            \ result is positive, so jump to TT184 and skip the
                            \ following two instructions
    
     EOR #&FF               \ Otherwise negate the result in A, so A is always
     ADC #1                 \ positive (i.e. A = |s1_hi - QQ0|)
    
    .TT184
    
     CMP #29                \ If the horizontal distance in A is >= 29, then this
     BCS TT187s             \ system is too far away from the current system to
                            \ appear in the Short-range Chart, so jump to TT187 via
                            \ TT187s to move on to the next system
    
     LDA QQ15+1             \ Set A = s0_hi - QQ1, the vertical distance between
     SEC                    \ (s1_hi, s0_hi) and (QQ0, QQ1)
     SBC QQ1
    
     BCS TT186              \ If a borrow didn't occur, i.e. s0_hi >= QQ1, then the
                            \ result is positive, so jump to TT186 and skip the
                            \ following two instructions
    
     EOR #&FF               \ Otherwise negate the result in A, so A is always
     ADC #1                 \ positive (i.e. A = |s0_hi - QQ1|)
    
    .TT186
    
     CMP #40                \ If the vertical distance in A is >= 40, then this
     BCS TT187s             \ system is too far away from the current system to
                            \ appear in the Short-range Chart, so jump to TT187 via
                            \ TT187s to move on to the next system
    
                            \ This system should be shown on the Short-range Chart,
                            \ so now we need to work out where the label should go,
                            \ and set up the various variables we need to draw the
                            \ system's filled circle on the chart
    
     LDA QQ15+3             \ Set A = s1_hi - QQ0, the horizontal distance between
     SEC                    \ this system and the current system, where |A| < 20.
     SBC QQ0                \ Let's call this the x-delta, as it's the horizontal
                            \ difference between the current system at the centre of
                            \ the chart, and this system (and this time we keep the
                            \ sign of A, so it can be negative if it's to the left
                            \ of the chart's centre, or positive if it's to the
                            \ right)
    
     ASL A                  \ Set XX12 = 104 + x-delta * 4
     ASL A                  \
     ADC #104               \ 104 is the x-coordinate of the centre of the chart,
     JSR SCALEY2            \ so this sets XX12 to the centre 104 +/- 76, the pixel
     STA XX12               \ x-coordinate of this system
                            \
                            \ The call to SCALEY2 has no effect as it only contains
                            \ an RTS, but having this call instruction here would
                            \ enable different scaling to be applied by altering
                            \ the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LSR A                  \ Move the text cursor to column x-delta / 2 + 1
     LSR A                  \ which will be in the range 1-10
     LSR A
     INC A
     STA XC
    
     LDA QQ15+1             \ Set A = s0_hi - QQ1, the vertical distance between
     SEC                    \ this system and the current system, where |A| < 38.
     SBC QQ1                \ Let's call this the y-delta, as it's the vertical
                            \ difference between the current system at the centre of
                            \ the chart, and this system (and this time we keep the
                            \ sign of A, so it can be negative if it's above the
                            \ chart's centre, or positive if it's below)
    
     ASL A                  \ Set K4 = 90 + y-delta * 2
     ADC #90                \
     JSR SCALEY2            \ 90 is the y-coordinate of the centre of the chart,
     STA K4                 \ so this sets K4 to the centre 90 +/- 74, the pixel
                            \ y-coordinate of this system
                            \
                            \ The call to SCALEY2 has no effect as it only contains
                            \ an RTS, but having this call instruction here would
                            \ enable different scaling to be applied by altering
                            \ the SCALE routines
                            \
                            \ This code is left over from the Apple II version,
                            \ where the scale factor is different
    
     LSR A                  \ Set Y = A >> 3
     LSR A                  \       = K4 div 8
     LSR A                  \
     TAY                    \ So Y now contains the number of the character row
                            \ that contains this system
    
                            \ Now to see if there is room for this system's label.
                            \ Ideally we would print the system name on the same
                            \ text row as the system, but we only want to print one
                            \ label per row, to prevent overlap, so now we check
                            \ this system's row, and if that's already occupied,
                            \ the row above, and if that's already occupied, the
                            \ row below... and if that's already occupied, we give
                            \ up and don't print a label for this system
    
     LDX INWK,Y             \ If the value in INWK+Y is 0 (i.e. the text row
     BEQ EE4                \ containing this system does not already have another
                            \ system's label on it), jump to EE4 to store this
                            \ system's label on this row
    
     INY                    \ If the value in INWK+Y+1 is 0 (i.e. the text row below
     LDX INWK,Y             \ the one containing this system does not already have
     BEQ EE4                \ another system's label on it), jump to EE4 to store
                            \ this system's label on this row
    
     DEY                    \ If the value in INWK+Y-1 is 0 (i.e. the text row above
     DEY                    \ the one containing this system does not already have
     LDX INWK,Y             \ another system's label on it), fall through into to
     BNE ee1                \ EE4 to store this system's label on this row,
                            \ otherwise jump to ee1 to skip printing a label for
                            \ this system (as there simply isn't room)
    
    .EE4
    
     STY YC                 \ Now to print the label, so move the text cursor to row
                            \ Y (which contains the row where we can print this
                            \ system's label)
    
     CPY #3                 \ If Y < 3, then the system would clash with the chart
     BCC TT187              \ title, so jump to TT187 to skip showing the system
    
     CPY #21                \ If Y > 21, then the label will be off the bottom of
     BCS TT187              \ the chart, so jump to TT187 to skip showing the system
    
     TYA                    \ Store Y on the stack so it can be preserved across the
     PHA                    \ call to readdistnce
    
     LDA QQ15+3             \ Set A = s1_hi, so A contains the galactic x-coordinate
                            \ of the system we are displaying on the chart
    
     JSR readdistnce        \ Call readdistnce to calculate the distance between the
                            \ system with galactic coordinates (A, QQ15+1) - i.e.
                            \ the system we are displaying - and the current system
                            \ at (QQ0, QQ1), returning the result in QQ8(1 0)
    
     PLA                    \ Restore Y from the stack
     TAY
    
     LDA QQ8+1              \ If the high byte of the distance in QQ8(1 0) is
     BNE TT187              \ non-zero, jump to TT187 to skip showing the system as
                            \ it is too far away from the current system
    
     LDA QQ8                \ If the low byte of the distance in QQ8(1 0) is >= 70,
     CMP #70                \ jump to TT187 to skip showing the system as it is too
                            \ far away from the current system
    
    .TT187s
    
     BCS TT187              \ If we get here from the instruction above, we jump to
                            \ TT187 if QQ8(1 0) >= 70, so we only show systems that
                            \ are within distance 70 (i.e. 7 light years) of the
                            \ current system
                            \
                            \ If we jump here from elsewhere with a BCS TT187s, we
                            \ jump straight on to TT187
    
     LDA #&FF               \ Store &FF in INWK+Y, to denote that this row is now
     STA INWK,Y             \ occupied so we don't try to print another system's
                            \ label on this row
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch to Sentence Case
     STA QQ17
    
     JSR cpl                \ Call cpl to print out the system name for the seeds
                            \ in QQ15 (which now contains the seeds for the current
                            \ system)
    
    .ee1
    
     LDA #0                 \ Now to plot the star, so set the high bytes of K, K3
     STA K3+1               \ and K4 to 0
     STA K4+1
     STA K+1
    
     LDA XX12               \ Set the low byte of K3 to XX12, the pixel x-coordinate
     STA K3                 \ of this system
    
     LDA QQ15+5             \ Fetch s2_hi for this system from QQ15+5, extract bit 0
     AND #1                 \ and add 2 to get the size of the star, which we store
     ADC #2                 \ in K. This will be either 2, 3 or 4, depending on the
     STA K                  \ value of bit 0, and whether the C flag is set (which
                            \ will vary depending on what happens in the above call
                            \ to cpl). Incidentally, the planet's average radius
                            \ also uses s2_hi, bits 0-3 to be precise, but that
                            \ doesn't mean the two sizes affect each other
    
                            \ We now have the following:
                            \
                            \   K(1 0)  = radius of star (2, 3 or 4)
                            \
                            \   K3(1 0) = pixel x-coordinate of system
                            \
                            \   K4(1 0) = pixel y-coordinate of system
                            \
                            \ which we can now pass to the SUN routine to draw a
                            \ small "sun" on the Short-range Chart for this system
    
     JSR FLFLLS             \ Call FLFLLS to reset the LSO block
    
     JSR SUN                \ Call SUN to plot a sun with radius K at pixel
                            \ coordinate (K3, K4)
    
     JSR FLFLLS             \ Call FLFLLS to reset the LSO block
    
     LDA #CYAN              \ Switch to colour 3, which is white in the chart view
     STA COL
    
    .TT187
    
     JSR TT20               \ We want to move on to the next system, so call TT20
                            \ to twist the three 16-bit seeds in QQ15
    
     INC XX20               \ Increment the counter
    
     BEQ P%+5               \ If X = 0 then we have done all 256 systems, so skip
                            \ the next instruction to return from the subroutine
    
     JMP TT182              \ Otherwise jump back up to TT182 to process the next
                            \ system
    
    \LDA #0                 \ These instructions are commented out in the original
    \STA dontclip           \ source
    \
    \LDA #2*Y-1
    \STA Yx2M1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT81                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set the selected system's seeds to those of system 0
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt81.md)
     References: This subroutine is called as follows:
                 * [HME2](../main/subroutine/hme2.md) calls TT81
                 * [TT111](../main/subroutine/tt111.md) calls TT81
                 * [TT22](../main/subroutine/tt22.md) calls TT81
                 * [TT23](../main/subroutine/tt23.md) calls TT81
    
    
    
    
    
    * * *
    
    
     Copy the three 16-bit seeds for the current galaxy's system 0 (QQ21) into the
     seeds for the selected system (QQ15) - in other words, set the selected
     system's seeds to those of system 0.
    
    
    
    
    .TT81
    
     LDX #5                 \ Set up a counter in X to copy six bytes (for three
                            \ 16-bit numbers)
    
     LDA QQ21,X             \ Copy the X-th byte in QQ21 to the X-th byte in QQ15
     STA QQ15,X
    
     DEX                    \ Decrement the counter
    
     BPL TT81+2             \ Loop back up to the LDA instruction if we still have
                            \ more bytes to copy
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT111                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set the current system to the nearest system to a point
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt111.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt111.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md) calls TT111
                 * [Ghy](../main/subroutine/ghy.md) calls TT111
                 * [hm](../main/subroutine/hm.md) calls TT111
                 * [HME2](../main/subroutine/hme2.md) calls TT111
                 * [hyp1](../main/subroutine/hyp1.md) calls TT111
                 * [STATUS](../main/subroutine/status.md) calls TT111
                 * [TT102](../main/subroutine/tt102.md) calls TT111
                 * [TT110](../main/subroutine/tt110.md) calls TT111
                 * [TTX110](../main/subroutine/ttx110.md) calls TT111
                 * [TT23](../main/subroutine/tt23.md) calls via readdistnce
    
    
    
    
    
    * * *
    
    
     Given a set of galactic coordinates in (QQ9, QQ10), find the nearest system
     to this point in the galaxy, and set this as the currently selected system.
    
    
    
    * * *
    
    
     Arguments:
    
       QQ9                  The x-coordinate near which we want to find a system
    
       QQ10                 The y-coordinate near which we want to find a system
    
    
    
    * * *
    
    
     Returns:
    
       QQ8(1 0)             The distance from the current system to the nearest
                            system to the original coordinates
    
       QQ9                  The x-coordinate of the nearest system to the original
                            coordinates
    
       QQ10                 The y-coordinate of the nearest system to the original
                            coordinates
    
       QQ15 to QQ15+5       The three 16-bit seeds of the nearest system to the
                            original coordinates
    
       ZZ                   The system number of the nearest system
    
    
    
    * * *
    
    
     Other entry points:
    
       TT111-1              Contains an RTS
    
       readdistnce          Calculate the distance between the system with galactic
                            coordinates (A, QQ15+1) and the system at (QQ0, QQ1),
                            returning the result in QQ8(1 0)
    
    
    
    
    .TT111
    
     JSR TT81               \ Set the seeds in QQ15 to those of system 0 in the
                            \ current galaxy (i.e. copy the seeds from QQ21 to QQ15)
    
                            \ We now loop through every single system in the galaxy
                            \ and check the distance from (QQ9, QQ10). We get the
                            \ galactic coordinates of each system from the system's
                            \ seeds, like this:
                            \
                            \   x = s1_hi (which is stored in QQ15+3)
                            \   y = s0_hi (which is stored in QQ15+1)
                            \
                            \ so the following loops through each system in the
                            \ galaxy in turn and calculates the distance between
                            \ (QQ9, QQ10) and (s1_hi, s0_hi) to find the closest one
    
     LDY #127               \ Set Y = T = 127 to hold the shortest distance we've
     STY T                  \ found so far, which we initially set to half the
                            \ distance across the galaxy, or 127, as our coordinate
                            \ system ranges from (0,0) to (255, 255)
    
     LDA #0                 \ Set A = U = 0 to act as a counter for each system in
     STA U                  \ the current galaxy, which we start at system 0 and
                            \ loop through to 255, the last system
    
    .TT130
    
     LDA QQ15+3             \ Set A = s1_hi - QQ9, the horizontal distance between
     SEC                    \ (s1_hi, s0_hi) and (QQ9, QQ10)
     SBC QQ9
    
     BCS TT132              \ If a borrow didn't occur, i.e. s1_hi >= QQ9, then the
                            \ result is positive, so jump to TT132 and skip the
                            \ following two instructions
    
     EOR #&FF               \ Otherwise negate the result in A, so A is always
     ADC #1                 \ positive (i.e. A = |s1_hi - QQ9|)
    
    .TT132
    
     LSR A                  \ Set S = A / 2
     STA S                  \       = |s1_hi - QQ9| / 2
    
     LDA QQ15+1             \ Set A = s0_hi - QQ10, the vertical distance between
     SEC                    \ (s1_hi, s0_hi) and (QQ9, QQ10)
     SBC QQ10
    
     BCS TT134              \ If a borrow didn't occur, i.e. s0_hi >= QQ10, then the
                            \ result is positive, so jump to TT134 and skip the
                            \ following two instructions
    
     EOR #&FF               \ Otherwise negate the result in A, so A is always
     ADC #1                 \ positive (i.e. A = |s0_hi - QQ10|)
    
    .TT134
    
     LSR A                  \ Set A = S + A / 2
     CLC                    \       = |s1_hi - QQ9| / 2 + |s0_hi - QQ10| / 2
     ADC S                  \
                            \ So A now contains the sum of the horizontal and
                            \ vertical distances, both divided by 2 so the result
                            \ fits into one byte, and although this doesn't contain
                            \ the actual distance between the systems, it's a good
                            \ enough approximation to use for comparing distances
    
     CMP T                  \ If A >= T, then this system's distance is bigger than
     BCS TT135              \ our "minimum distance so far" stored in T, so it's no
                            \ closer than the systems we have already found, so
                            \ skip to TT135 to move on to the next system
    
     STA T                  \ This system is the closest to (QQ9, QQ10) so far, so
                            \ update T with the new "distance" approximation
    
     LDX #5                 \ As this system is the closest we have found yet, we
                            \ want to store the system's seeds in case it ends up
                            \ being the closest of all, so we set up a counter in X
                            \ to copy six bytes (for three 16-bit numbers)
    
    .TT136
    
     LDA QQ15,X             \ Copy the X-th byte in QQ15 to the X-th byte in QQ19,
     STA QQ19,X             \ where QQ15 contains the seeds for the system we just
                            \ found to be the closest so far, and QQ19 is temporary
                            \ storage
    
     DEX                    \ Decrement the counter
    
     BPL TT136              \ Loop back to TT136 if we still have more bytes to
                            \ copy
    
     LDA U                  \ Store the system number U in ZZ, so when we are done
     STA ZZ                 \ looping through all the candidates, the winner's
                            \ number will be in ZZ
    
    .TT135
    
     JSR TT20               \ We want to move on to the next system, so call TT20
                            \ to twist the three 16-bit seeds in QQ15
    
     INC U                  \ Increment the system counter in U
    
     BNE TT130              \ If U > 0 then we haven't done all 256 systems yet, so
                            \ loop back up to TT130
    
                            \ We have now finished checking all the systems in the
                            \ galaxy, and the seeds for the closest system are in
                            \ QQ19, so now we want to copy these seeds to QQ15,
                            \ to set the selected system to this closest system
    
     LDX #5                 \ So we set up a counter in X to copy six bytes (for
                            \ three 16-bit numbers)
    
    .TT137
    
     LDA QQ19,X             \ Copy the X-th byte in QQ19 to the X-th byte in QQ15
     STA QQ15,X
    
     DEX                    \ Decrement the counter
    
     BPL TT137              \ Loop back to TT137 if we still have more bytes to
                            \ copy
    
     LDA QQ15+1             \ The y-coordinate of the system described by the seeds
     STA QQ10               \ in QQ15 is in QQ15+1 (s0_hi), so we copy this to QQ10
                            \ as this is where we store the selected system's
                            \ y-coordinate
    
     LDA QQ15+3             \ The x-coordinate of the system described by the seeds
     STA QQ9                \ in QQ15 is in QQ15+3 (s1_hi), so we copy this to QQ9
                            \ as this is where we store the selected system's
                            \ x-coordinate
    
                            \ We have now found the closest system to (QQ9, QQ10)
                            \ and have set it as the selected system, so now we
                            \ need to work out the distance between the selected
                            \ system and the current system
    
    .readdistnce
    
     SEC                    \ Set A = QQ9 - QQ0, the horizontal distance between
     SBC QQ0                \ the selected system's x-coordinate (QQ9) and the
                            \ current system's x-coordinate (QQ0)
    
     BCS TT139              \ If a borrow didn't occur, i.e. QQ9 >= QQ0, then the
                            \ result is positive, so jump to TT139 and skip the
                            \ following two instructions
    
     EOR #&FF               \ Otherwise negate the result in A, so A is always
     ADC #1                 \ positive (i.e. A = |QQ9 - QQ0|)
    
                            \ A now contains the difference between the two
                            \ systems' x-coordinates, with the sign removed. We
                            \ will refer to this as the x-delta ("delta" means
                            \ change or difference in maths)
    
    .TT139
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = |QQ9 - QQ0| ^ 2
                            \           = x_delta ^ 2
    
     STA K+1                \ Store (A P) in K(1 0)
     LDA P
     STA K
    
     LDA QQ15+1             \ Set A = QQ15+1 - QQ1, the vertical distance between
    \LDA QQ10               \ the selected system's y-coordinate (QQ15+1) and the
     SEC                    \ current system's y-coordinate (QQ1)
     SBC QQ1                \
                            \ The LDA instruction is commented out in the original
                            \ source
    
     BCS TT141              \ If a borrow didn't occur, i.e. QQ10 >= QQ1, then the
                            \ result is positive, so jump to TT141 and skip the
                            \ following two instructions
    
     EOR #&FF               \ Otherwise negate the result in A, so A is always
     ADC #1                 \ positive (i.e. A = |QQ10 - QQ1|)
    
    .TT141
    
     LSR A                  \ Set A = A / 2
    
                            \ A now contains the difference between the two
                            \ systems' y-coordinates, with the sign removed, and
                            \ halved. We halve the value because the galaxy in
                            \ in Elite is rectangular rather than square, and is
                            \ twice as wide (x-axis) as it is high (y-axis), so to
                            \ get a distance that matches the shape of the
                            \ long-range galaxy chart, we need to halve the
                            \ distance between the vertical y-coordinates. We will
                            \ refer to this as the y-delta
    
     JSR SQUA2              \ Set (A P) = A * A
                            \           = (|QQ10 - QQ1| / 2) ^ 2
                            \           = y_delta ^ 2
    
                            \ By this point we have the following results:
                            \
                            \   K(1 0) = x_delta ^ 2
                            \    (A P) = y_delta ^ 2
                            \
                            \ so to find the distance between the two points, we
                            \ can use Pythagoras - so first we need to add the two
                            \ results together, and then take the square root
    
     PHA                    \ Store the high byte of the y-axis value on the stack,
                            \ so we can use A for another purpose
    
     LDA P                  \ Set Q = P + K, which adds the low bytes of the two
     CLC                    \ calculated values
     ADC K
     STA Q
    
     PLA                    \ Restore the high byte of the y-axis value from the
                            \ stack into A again
    
     ADC K+1                \ Set A = A + K+1, which adds the high bytes of the two
                            \ calculated values
    
     BCC P%+4               \ If the above addition overflowed, set A = 255
     LDA #255
    
     STA R                  \ Store A in R, so we now have R = A + K+1, and:
                            \
                            \   (R Q) = K(1 0) + (A P)
                            \         = (x_delta ^ 2) + (y_delta ^ 2)
    
     JSR LL5                \ Set Q = SQRT(R Q), so Q now contains the distance
                            \ between the two systems, in terms of coordinates
    
                            \ We now store the distance to the selected system * 4
                            \ in the two-byte location QQ8, by taking (0 Q) and
                            \ shifting it left twice, storing it in QQ8(1 0)
    
     LDA Q                  \ First we shift the low byte left by setting
     ASL A                  \ A = Q * 2, with bit 7 of A going into the C flag
    
     LDX #0                 \ Now we set the high byte in QQ8+1 to 0 and rotate
     STX QQ8+1              \ the C flag into bit 0 of QQ8+1
     ROL QQ8+1
    
     ASL A                  \ And then we repeat the shift left of (QQ8+1 A)
     ROL QQ8+1
    
     STA QQ8                \ And store A in the low byte, QQ8, so QQ8(1 0) now
                            \ contains Q * 4. Given that the width of the galaxy is
                            \ 256 in coordinate terms, the width of the galaxy
                            \ would be 1024 in the units we store in QQ8
    
     JMP TT24               \ Call TT24 to calculate system data from the seeds in
                            \ QQ15 and store them in the relevant locations, so our
                            \ new selected system is fully set up, and return from
                            \ the subroutine using a tail call
    
    
    
    
           Name: dockEd                                                  [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Print a message to say there is no hyperspacing allowed inside the
                 station
    
    
        Context: See this subroutine [on its own page](../main/subroutine/docked.md)
     Variations: See [code variations](../related/compare/main/subroutine/hy6-docked.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hyp](../main/subroutine/hyp.md) calls dockEd
    
    
    
    
    
    * * *
    
    
     Print "Docked" at the bottom of the screen to indicate we can't hyperspace
     when docked.
    
    
    
    
    .dockEd
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
     LDA #15                \ Move the text cursor to column 15 (the middle of the
     STA XC                 \ screen), setting A to 15 at the same time for the
                            \ following call to TT27
    
     LDA #RED               \ Switch to colour 2, which is magenta in the trade view
     STA COL                \ or red in the chart view
    
     LDA #205               \ Print extended token 205 ("DOCKED") and return from
     JMP DETOK              \ the subroutine using a tail call
    
    
    
    
           Name: hyp                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Start the hyperspace process
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hyp.md)
     Variations: See [code variations](../related/compare/main/subroutine/hyp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls hyp
                 * [TTX110](../main/subroutine/ttx110.md) calls via TTX111
    
    
    
    
    
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
    
    
    
    
           Name: wW                                                      [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Start a hyperspace countdown
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ww.md)
     Variations: See [code variations](../related/compare/main/subroutine/ww.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Ghy](../main/subroutine/ghy.md) calls via wW2
    
    
    
    
    
    * * *
    
    
     Start the hyperspace countdown (for both inter-system hyperspace and the
     galactic hyperdrive).
    
    
    
    * * *
    
    
     Other entry points:
    
       wW2                  Start the hyperspace countdown, starting the countdown
                            from the value in A
    
    
    
    
    .wW
    
     LDA #15                \ The hyperspace countdown starts from 15, so set A to
                            \ 15 so we can set the two hyperspace counters
    
    .wW2
    
     STA QQ22+1             \ Set the number in QQ22+1 to A, which is the number
                            \ that's shown on-screen during the hyperspace countdown
    
     STA QQ22               \ Set the number in QQ22 to 15, which is the internal
                            \ counter that counts down by 1 each iteration of the
                            \ main game loop, and each time it reaches zero, the
                            \ on-screen counter gets decremented, and QQ22 gets set
                            \ to 5, so setting QQ22 to 15 here makes the first tick
                            \ of the hyperspace counter longer than subsequent ticks
    
     TAX                    \ Print the 8-bit number in X (i.e. 15) at text location
     JMP ee3                \ (0, 1), padded to 5 digits, so it appears in the top
                            \ left corner of the screen, and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: TTX110                                                  [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Set the current system to the nearest system and return to hyp
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ttx110.md)
     References: This subroutine is called as follows:
                 * [hyp](../main/subroutine/hyp.md) calls TTX110
    
    
    
    
    
    
    .TTX110
    
                            \ This routine is only called from the hyp routine, and
                            \ it jumps back into hyp at label TTX111
    
     JSR TT111              \ Call TT111 to set the current system to the nearest
                            \ system to (QQ9, QQ10), and put the seeds of the
                            \ nearest system into QQ15 to QQ15+5
    
     JMP TTX111             \ Return to TTX111 in the hyp routine
    
    
    
    
           Name: Ghy                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Perform a galactic hyperspace jump
      Deep dive: [Twisting the system seeds](https://elite.bbcelite.com/deep_dives/twisting_the_system_seeds.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ghy.md)
     Variations: See [code variations](../related/compare/main/subroutine/ghy.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hyp](../main/subroutine/hyp.md) calls Ghy
    
    
    
    
    
    * * *
    
    
     Engage the galactic hyperdrive. Called from the hyp routine above if CTRL-H is
     being pressed.
    
     This routine also updates the galaxy seeds to point to the next galaxy. Using
     a galactic hyperdrive rotates each seed byte to the left, rolling each byte
     left within itself like this:
    
       01234567 -> 12345670
    
     to get the seeds for the next galaxy. So after 8 galactic jumps, the seeds
     roll round to those of the first galaxy again.
    
     We always arrive in a new galaxy at galactic coordinates (96, 96), and then
     find the nearest system and set that as our location.
    
    
    
    * * *
    
    
     Other entry points:
    
       zZ+1                 Contains an RTS
    
    
    
    
    .Ghy
    
     LDX GHYP               \ Fetch GHYP, which tells us whether we own a galactic
     BEQ zZ+1               \ hyperdrive, and if it is zero, which means we don't,
                            \ return from the subroutine (as zZ+1 contains an RTS)
    
     INX                    \ We own a galactic hyperdrive, so X is &FF, so this
                            \ instruction sets X = 0
    
    \STX QQ8                \ These instructions are commented out in the original
    \STX QQ8+1              \ source
    
     STX GHYP               \ The galactic hyperdrive is a one-use item, so set GHYP
                            \ to 0 so we no longer have one fitted
    
     STX FIST               \ Changing galaxy also clears our criminal record, so
                            \ set our legal status in FIST to 0 ("clean")
    
     LDA #2                 \ Call wW2 with A = 2 to start the hyperspace countdown,
     JSR wW2                \ but starting the countdown from 2
    
     LDX #5                 \ To move galaxy, we rotate the galaxy's seeds left, so
                            \ set a counter in X for the 6 seed bytes
    
     INC GCNT               \ Increment the current galaxy number in GCNT
    
     LDA GCNT               \ Clear bit 3 of GCNT, so we jump from galaxy 7 back
     AND #%11110111         \ to galaxy 0 (shown in-game as going from galaxy 8 back
     STA GCNT               \ to the starting point in galaxy 1). We also retain any
                            \ set bits in the high nibble, so if the galaxy number
                            \ is manually set to 16 or higher, it will stay high
                            \ (though the high nibble doesn't seem to get set by
                            \ the game at any point, so it isn't clear what this is
                            \ for, though Lave in galaxy 16 does show a unique
                            \ system description override, so something is going on
                            \ here...)
    
    .G1
    
     LDA QQ21,X             \ Load the X-th seed byte into A
    
     ASL A                  \ Set the C flag to bit 7 of the seed
    
     ROL QQ21,X             \ Rotate the seed in memory, which will add bit 7 back
                            \ in as bit 0, so this rolls the seed around on itself
    
     DEX                    \ Decrement the counter
    
     BPL G1                 \ Loop back for the next seed byte, until we have
                            \ rotated them all
    
    \JSR DORND              \ This instruction is commented out in the original
                            \ source, and would set A and X to random numbers, so
                            \ perhaps the original plan was to arrive in each new
                            \ galaxy in a random place?
    
    .zZ
    
     LDA #96                \ Set (QQ9, QQ10) to (96, 96), which is where we always
     STA QQ9                \ arrive in a new galaxy (the selected system will be
     STA QQ10               \ set to the nearest actual system later on)
    
     JSR TT110              \ Call TT110 to show the front space view
    
     JSR TT111              \ Call TT111 to set the current system to the nearest
                            \ system to (QQ9, QQ10), and put the seeds of the
                            \ nearest system into QQ15 to QQ15+5
                            \
                            \ This call fixes a bug in the cassette version, where
                            \ the galactic hyperdrive will take us to coordinates
                            \ (96, 96) in the new galaxy, even if there isn't
                            \ actually a system there, so if we jump when we are
                            \ low on fuel, it is possible to get stuck in the
                            \ middle of nowhere when changing galaxy
                            \
                            \ This call sets the current system correctly, so we
                            \ always arrive at the nearest system to (96, 96)
    
     LDX #5                 \ We now want to copy those seeds into safehouse, so we
                            \ so set a counter in X to copy 6 bytes
    
    .dumdeedum
    
     LDA QQ15,X             \ Copy the X-th byte of QQ15 into the X-th byte of
     STA safehouse,X        \ safehouse
    
     DEX                    \ Decrement the loop counter
    
     BPL dumdeedum          \ Loop back to copy the next byte until we have copied
                            \ all six seed bytes
    
     LDX #0                 \ Set the distance to the selected system in QQ8(1 0)
     STX QQ8                \ to 0
     STX QQ8+1
    
     LDA #116               \ Print recursive token 116 (GALACTIC HYPERSPACE ")
     JSR MESS               \ as an in-flight message
    
                            \ Fall through into jmp to set the system to the
                            \ current system and return from the subroutine there
    
    
    
    
           Name: jmp                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set the current system to the selected system
    
    
        Context: See this subroutine [on its own page](../main/subroutine/jmp.md)
     References: This subroutine is called as follows:
                 * [BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md) calls jmp
                 * [hyp1](../main/subroutine/hyp1.md) calls jmp
    
    
    
    
    
    * * *
    
    
     Returns:
    
       (QQ0, QQ1)           The galactic coordinates of the new system
    
    
    
    * * *
    
    
     Other entry points:
    
       hy5                  Contains an RTS
    
    
    
    
    .jmp
    
     LDA QQ9                \ Set the current system's galactic x-coordinate to the
     STA QQ0                \ x-coordinate of the selected system
    
     LDA QQ10               \ Set the current system's galactic y-coordinate to the
     STA QQ1                \ y-coordinate of the selected system
    
    .hy5
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: ee3                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Print the hyperspace countdown in the top-left of the screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ee3.md)
     Variations: See [code variations](../related/compare/main/subroutine/ee3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls ee3
                 * [TTX66K](../main/subroutine/ttx66k.md) calls ee3
                 * [wW](../main/subroutine/ww.md) calls ee3
    
    
    
    
    
    * * *
    
    
     5 digits, left-padding with spaces for numbers with fewer than 3 digits (so
     numbers < 10000 are right-aligned), with no decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The number to print
    
    
    
    
    .ee3
    
     LDA #RED               \ Switch to colour 2, which is red in the space view
     STA COL
    
     LDA #1                 \ Move the text cursor to column 1
     STA XC
    
     STA YC                 \ Move the text cursor to row 1
    
     LDY #0                 \ Set Y = 0 for the high byte in pr6
    
     CLC                    \ Call TT11 to print X to 3 digits with no decimal point
     LDA #3                 \ and return from the subroutine using a tail call
     JMP TT11
    
    
    
    
           Name: pr6                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print 16-bit number, left-padded to 5 digits, no point
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pr6.md)
     References: This subroutine is called as follows:
                 * [TT25](../main/subroutine/tt25.md) calls pr6
    
    
    
    
    
    * * *
    
    
     Print the 16-bit number in (Y X) to 5 digits, left-padding with spaces for
     numbers with fewer than 3 digits (so numbers < 10000 are right-aligned),
     with no decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The low byte of the number to print
    
       Y                    The high byte of the number to print
    
    
    
    
    .pr6
    
     CLC                    \ Do not display a decimal point when printing
    
                            \ Fall through into pr5 to print X to 5 digits
    
    
    
    
           Name: pr5                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a 16-bit number, left-padded to 5 digits, and optional point
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pr5.md)
     References: This subroutine is called as follows:
                 * [TT146](../main/subroutine/tt146.md) calls pr5
                 * [TT151](../main/subroutine/tt151.md) calls pr5
                 * [TT25](../main/subroutine/tt25.md) calls pr5
    
    
    
    
    
    * * *
    
    
     Print the 16-bit number in (Y X) to 5 digits, left-padding with spaces for
     numbers with fewer than 3 digits (so numbers < 10000 are right-aligned).
     Optionally include a decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The low byte of the number to print
    
       Y                    The high byte of the number to print
    
       C flag               If set, include a decimal point
    
    
    
    
    .pr5
    
     LDA #5                 \ Set the number of digits to print to 5
    
     JMP TT11               \ Call TT11 to print (Y X) to 5 digits and return from
                            \ the subroutine using a tail call
    
    
    
    
           Name: TT147                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Print an error when a system is out of hyperspace range
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt147.md)
     References: This subroutine is called as follows:
                 * [hyp](../main/subroutine/hyp.md) calls TT147
    
    
    
    
    
    * * *
    
    
     Print "RANGE?" for when the hyperspace distance is too far
    
    
    
    
    .TT147
    
     LDA #202               \ Load A with token 42 ("RANGE") and fall through into
                            \ prq to print it, followed by a question mark
    
    
    
    
           Name: prq                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token followed by a question mark
    
    
        Context: See this subroutine [on its own page](../main/subroutine/prq.md)
     References: This subroutine is called as follows:
                 * [eq](../main/subroutine/eq.md) calls prq
                 * [EQSHP](../main/subroutine/eqshp.md) calls prq
                 * [NWDAV4](../main/subroutine/nwdav4.md) calls prq
                 * [qv](../main/subroutine/qv.md) calls prq
                 * [TT219](../main/subroutine/tt219.md) calls prq
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    * * *
    
    
     Other entry points:
    
       prq+3                Print a question mark
    
    
    
    
    .prq
    
     JSR TT27               \ Print the text token in A
    
     LDA #'?'               \ Print a question mark and return from the
     JMP TT27               \ subroutine using a tail call
    
    
    
    
           Name: TT151                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print the name, price and availability of a market item
      Deep dive: [Market item prices and availability](https://elite.bbcelite.com/deep_dives/market_item_prices_and_availability.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt151.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt151.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT167](../main/subroutine/tt167.md) calls TT151
                 * [TT210](../main/subroutine/tt210.md) calls TT151
                 * [TT219](../main/subroutine/tt219.md) calls TT151
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The number of the market item to print, 0-16 (see QQ23
                            for details of item numbers)
    
    
    
    * * *
    
    
     Returns:
    
       QQ19+1               Byte #1 from the market prices table for this item
    
       QQ24                 The item's price / 4
    
       QQ25                 The item's availability
    
    
    
    
    .TT151q
    
                            \ We jump here from below if we are in witchspace
    
     PLA                    \ Restore the item number from the stack
    
     RTS                    \ Return from the subroutine
    
    .TT151
    
     PHA                    \ Store the item number on the stack and in QQ19+4
     STA QQ19+4
    
     ASL A                  \ Store the item number * 4 in QQ19, so this will act as
     ASL A                  \ an index into the market prices table at QQ23 for this
     STA QQ19               \ item (as there are four bytes per item in the table)
    
     LDA MJ                 \ If we are in witchspace, we can't trade items, so jump
     BNE TT151q             \ up to TT151q to return from the subroutine
    
     LDA #1                 \ Move the text cursor to column 1, for the item's name
     JSR DOXC
    
     PLA                    \ Restore the item number
    
     ADC #208               \ Print recursive token 48 + A, which will be in the
     JSR TT27               \ range 48 ("FOOD") to 64 ("ALIEN ITEMS"), so this
                            \ prints the item's name
    
     LDA #14                \ Move the text cursor to column 14, for the price
     STA XC
    
     LDX QQ19               \ Fetch byte #1 from the market prices table (units and
     LDA QQ23+1,X           \ economic_factor) for this item and store in QQ19+1
     STA QQ19+1
    
     LDA QQ26               \ Fetch the random number for this system visit and
     AND QQ23+3,X           \ AND with byte #3 from the market prices table (mask)
                            \ to give:
                            \
                            \   A = random AND mask
    
     CLC                    \ Add byte #0 from the market prices table (base_price),
     ADC QQ23,X             \ so we now have:
     STA QQ24               \
                            \   A = base_price + (random AND mask)
    
     JSR TT152              \ Call TT152 to print the item's unit ("t", "kg" or
                            \ "g"), padded to a width of two characters
    
     JSR var                \ Call var to set QQ19+3 = economy * |economic_factor|
                            \ (and set the availability of alien items to 0)
    
     LDA QQ19+1             \ Fetch the byte #1 that we stored above and jump to
     BMI TT155              \ TT155 if it is negative (i.e. if the economic_factor
                            \ is negative)
    
     LDA QQ24               \ Set A = QQ24 + QQ19+3
     ADC QQ19+3             \
                            \       = base_price + (random AND mask)
                            \         + (economy * |economic_factor|)
                            \
                            \ which is the result we want, as the economic_factor
                            \ is positive
    
     JMP TT156              \ Jump to TT156 to multiply the result by 4
    
    .TT155
    
     LDA QQ24               \ Set A = QQ24 - QQ19+3
     SEC                    \
     SBC QQ19+3             \       = base_price + (random AND mask)
                            \         - (economy * |economic_factor|)
                            \
                            \ which is the result we want, as economic_factor
                            \ is negative
    
    .TT156
    
     STA QQ24               \ Store the result in QQ24 and P
     STA P
    
     LDA #0                 \ Set A = 0 and call GC2 to calculate (Y X) = (A P) * 4,
     JSR GC2                \ which is the same as (Y X) = P * 4 because A = 0
    
     SEC                    \ We now have our final price, * 10, so we can call pr5
     JSR pr5                \ to print (Y X) to 5 digits, including a decimal
                            \ point, as the C flag is set
    
     LDY QQ19+4             \ We now move on to availability, so fetch the market
                            \ item number that we stored in QQ19+4 at the start
    
     LDA #5                 \ Set A to 5 so we can print the availability to 5
                            \ digits (right-padded with spaces)
    
     LDX AVL,Y              \ Set X to the item's availability, which is given in
                            \ the AVL table
    
     STX QQ25               \ Store the availability in QQ25
    
     CLC                    \ Clear the C flag
    
     BEQ TT172              \ If none are available, jump to TT172 to print a tab
                            \ and a "-"
    
     JSR pr2+2              \ Otherwise print the 8-bit number in X to 5 digits,
                            \ right-aligned with spaces. This works because we set
                            \ A to 5 above, and we jump into the pr2 routine just
                            \ after the first instruction, which would normally
                            \ set the number of digits to 3
    
     JMP TT152              \ Print the unit ("t", "kg" or "g") for the market item,
                            \ with a following space if required to make it two
                            \ characters long, and return from the subroutine using
                            \ a tail call
    
    .TT172
    
     LDA #25                \ Move the text cursor to column 25
     JSR DOXC
    
     LDA #'-'               \ Print a "-" character by jumping to TT162+2, which
     BNE TT162+2            \ contains JMP TT27 (this BNE is effectively a JMP as A
                            \ will never be zero), and return from the subroutine
                            \ using a tail call
    
    
    
    
           Name: TT152                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print the unit ("t", "kg" or "g") for a market item
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt152.md)
     References: This subroutine is called as follows:
                 * [TT151](../main/subroutine/tt151.md) calls TT152
                 * [TT210](../main/subroutine/tt210.md) calls TT152
                 * [TT219](../main/subroutine/tt219.md) calls TT152
    
    
    
    
    
    * * *
    
    
     Print the unit ("t", "kg" or "g") for the market item whose byte #1 from the
     market prices table is in QQ19+1, right-padded with spaces to a width of two
     characters (so that's "t ", "kg" or "g ").
    
    
    
    
    .TT152
    
     LDA QQ19+1             \ Fetch the economic_factor from QQ19+1
    
     AND #96                \ If bits 5 and 6 are both clear, jump to TT160 to
     BEQ TT160              \ print "t" for tonne, followed by a space, and return
                            \ from the subroutine using a tail call
    
     CMP #32                \ If bit 5 is set, jump to TT161 to print "kg" for
     BEQ TT161              \ kilograms, and return from the subroutine using a tail
                            \ call
    
     JSR TT16a              \ Otherwise call TT16a to print "g" for grams, and fall
                            \ through into TT162 to print a space and return from
                            \ the subroutine
    
    
    
    
           Name: TT162                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a space
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt162.md)
     References: This subroutine is called as follows:
                 * [dn](../main/subroutine/dn.md) calls TT162
                 * [EQSHP](../main/subroutine/eqshp.md) calls TT162
                 * [spc](../main/subroutine/spc.md) calls TT162
                 * [TT160](../main/subroutine/tt160.md) calls TT162
                 * [TT219](../main/subroutine/tt219.md) calls TT162
                 * [TT25](../main/subroutine/tt25.md) calls TT162
                 * [TTX66K](../main/subroutine/ttx66k.md) calls TT162
                 * [TT151](../main/subroutine/tt151.md) calls via TT162+2
                 * [TT163](../main/subroutine/tt163.md) calls via TT162+2
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       TT162+2              Jump to TT27 to print the text token in A
    
    
    
    
    .TT162
    
     LDA #' '               \ Load a space character into A
    
     JMP TT27               \ Print the text token in A and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: TT160                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print "t" (for tonne) and a space
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt160.md)
     References: This subroutine is called as follows:
                 * [TT152](../main/subroutine/tt152.md) calls TT160
    
    
    
    
    
    
    .TT160
    
     LDA #'t'               \ Load a "t" character into A
    
     JSR TT26               \ Print the character, using TT216 so that it doesn't
                            \ change the character case
    
     BCC TT162              \ Jump to TT162 to print a space and return from the
                            \ subroutine using a tail call (this BCC is effectively
                            \ a JMP as the C flag is cleared by TT26)
    
    
    
    
           Name: TT161                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print "kg" (for kilograms)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt161.md)
     References: This subroutine is called as follows:
                 * [TT152](../main/subroutine/tt152.md) calls TT161
    
    
    
    
    
    
    .TT161
    
     LDA #'k'               \ Load a "k" character into A
    
     JSR TT26               \ Print the character, using TT216 so that it doesn't
                            \ change the character case, and fall through into
                            \ TT16a to print a "g" character
    
    
    
    
           Name: TT16a                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print "g" (for grams)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt16a.md)
     References: This subroutine is called as follows:
                 * [TT152](../main/subroutine/tt152.md) calls TT16a
    
    
    
    
    
    
    .TT16a
    
     LDA #'g'               \ Load a "g" character into A
    
     JMP TT26               \ Print the character, using TT216 so that it doesn't
                            \ change the character case, and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: TT163                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print the headers for the table of market prices
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt163.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt163.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT167](../main/subroutine/tt167.md) calls TT163
                 * [TT219](../main/subroutine/tt219.md) calls TT163
    
    
    
    
    
    * * *
    
    
     Print the column headers for the prices table in the Buy Cargo and Market
     Price screens.
    
    
    
    
    .TT163
    
     LDA #17                \ Move the text cursor in XC to column 17
     JSR DOXC
    
     LDA #255               \ Print recursive token 95 token ("UNIT  QUANTITY
     BNE TT162+2            \ {crlf} PRODUCT   UNIT PRICE FOR SALE{crlf}{lf}") by
                            \ jumping to TT162+2, which contains JMP TT27 (this BNE
                            \ is effectively a JMP as A will never be zero), and
                            \ return from the subroutine using a tail call
    
    
    
    
           Name: TT167                                                   [Show more]
           Type: Subroutine
       Category: Market
        Summary: Show the Market Price screen (red key f7)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt167.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt167.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT167
    
    
    
    
    
    
    .TT167
    
     LDA #16                \ Clear the top part of the screen, draw a border box,
     JSR TRADEMODE          \ and set up a trading screen with a view type in QQ11
                            \ of 32 (Market Price screen)
    
     LDA #5                 \ Move the text cursor to column 5
     STA XC
    
     LDA #167               \ Print recursive token 7 ("{current system name} MARKET
     JSR NLIN3              \ PRICES") and draw a horizontal line at pixel row 19
                            \ to box in the title
    
     LDA #3                 \ Move the text cursor to row 3
     STA YC
    
     JSR TT163              \ Print the column headers for the prices table
    
     LDA #6                 \ Move the text cursor to row 6
     STA YC
    
     LDA #0                 \ We're going to loop through all the available market
     STA QQ29               \ items, so we set up a counter in QQ29 to denote the
                            \ current item and start it at 0
    
    .TT168
    
     LDX #%10000000         \ Set bit 7 of QQ17 to switch to Sentence Case, with the
     STX QQ17               \ next letter in capitals
    
     JSR TT151              \ Call TT151 to print the item name, market price and
                            \ availability of the current item, and set QQ24 to the
                            \ item's price / 4, QQ25 to the quantity available and
                            \ QQ19+1 to byte #1 from the market prices table for
                            \ this item
    
     INC YC                 \ Move the text cursor down one row
    
     INC QQ29               \ Increment QQ29 to point to the next item
    
     LDA QQ29               \ If QQ29 >= 17 then jump to TT168 as we have done the
     CMP #17                \ last item
     BCC TT168
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: var                                                     [Show more]
           Type: Subroutine
       Category: Market
        Summary: Calculate QQ19+3 = economy * |economic_factor|
    
    
        Context: See this subroutine [on its own page](../main/subroutine/var.md)
     References: This subroutine is called as follows:
                 * [GVL](../main/subroutine/gvl.md) calls var
                 * [TT151](../main/subroutine/tt151.md) calls var
    
    
    
    
    
    * * *
    
    
     Set QQ19+3 = economy * |economic_factor|, given byte #1 of the market prices
     table for an item. Also sets the availability of alien items to 0.
    
     This routine forms part of the calculations for market item prices (TT151)
     and availability (GVL).
    
    
    
    * * *
    
    
     Arguments:
    
       QQ19+1               Byte #1 of the market prices table for this market item
                            (which contains the economic_factor in bits 0-5, and the
                            sign of the economic_factor in bit 7)
    
    
    
    
    .var
    
     LDA QQ19+1             \ Extract bits 0-5 from QQ19+1 into A, to get the
     AND #31                \ economic_factor without its sign, in other words:
                            \
                            \   A = |economic_factor|
    
     LDY QQ28               \ Set Y to the economy byte of the current system
    
     STA QQ19+2             \ Store A in QQ19+2
    
     CLC                    \ Clear the C flag so we can do additions below
    
     LDA #0                 \ Set AVL+16 (availability of alien items) to 0,
     STA AVL+16             \ setting A to 0 in the process
    
    .TT153
    
                            \ We now do the multiplication by doing a series of
                            \ additions in a loop, building the result in A. Each
                            \ loop adds QQ19+2 (|economic_factor|) to A, and it
                            \ loops the number of times given by the economy byte;
                            \ in other words, because A starts at 0, this sets:
                            \
                            \   A = economy * |economic_factor|
    
     DEY                    \ Decrement the economy in Y, exiting the loop when it
     BMI TT154              \ becomes negative
    
     ADC QQ19+2             \ Add QQ19+2 to A
    
     JMP TT153              \ Loop back to TT153 to do another addition
    
    .TT154
    
     STA QQ19+3             \ Store the result in QQ19+3
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: hyp1                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Process a jump to the system closest to (QQ9, QQ10)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hyp1.md)
     Variations: See [code variations](../related/compare/main/subroutine/hyp1.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT18](../main/subroutine/tt18.md) calls via hyp1+3
    
    
    
    
    
    * * *
    
    
     Do a hyperspace jump to the system closest to galactic coordinates
     (QQ9, QQ10), and set up the current system's state to those of the new system.
    
    
    
    * * *
    
    
     Returns:
    
       (QQ0, QQ1)           The galactic coordinates of the new system
    
       QQ2 to QQ2+6         The seeds of the new system
    
       EV                   Set to 0
    
       QQ28                 The new system's economy
    
       tek                  The new system's tech level
    
       gov                  The new system's government
    
    
    
    * * *
    
    
     Other entry points:
    
       hyp1+3               Jump straight to the system at (QQ9, QQ10) without
                            first calculating which system is closest. We do this
                            if we already know that (QQ9, QQ10) points to a system
    
    
    
    
    .hyp1
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     JSR jmp                \ Set the current system to the selected system
    
     LDX #5                 \ We now want to copy the seeds for the selected system
                            \ in QQ15 into QQ2, where we store the seeds for the
                            \ current system, so set up a counter in X for copying
                            \ 6 bytes (for three 16-bit seeds)
    
    .TT112
    
     LDA safehouse,X        \ Copy the X-th byte in safehouse to the X-th byte in
     STA QQ2,X              \ QQ2
    
     DEX                    \ Decrement the counter
    
     BPL TT112              \ Loop back to TT112 if we still have more bytes to
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
    
                            \ Fall through into GVL to calculate the availability of
                            \ market items in the new system
    
    
    
    
           Name: GVL                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Calculate the availability of market items
      Deep dive: [Market item prices and availability](https://elite.bbcelite.com/deep_dives/market_item_prices_and_availability.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/gvl.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Calculate the availability for each market item and store it in AVL. This is
     called on arrival in a new system.
    
    
    
    * * *
    
    
     Other entry points:
    
       hyR                  Contains an RTS
    
    
    
    
    .GVL
    
     JSR DORND              \ Set A and X to random numbers
    
     STA QQ26               \ Set QQ26 to the random byte that's used in the market
                            \ calculations
    
     LDX #0                 \ We are now going to loop through the market item
     STX XX4                \ availability table in AVL, so set a counter in XX4
                            \ (and X) for the market item number, starting with 0
    
    .hy9
    
     LDA QQ23+1,X           \ Fetch byte #1 from the market prices table (units and
     STA QQ19+1             \ economic_factor) for item number X and store it in
                            \ QQ19+1
    
     JSR var                \ Call var to set QQ19+3 = economy * |economic_factor|
                            \ (and set the availability of alien items to 0)
    
     LDA QQ23+3,X           \ Fetch byte #3 from the market prices table (mask) and
     AND QQ26               \ AND with the random number for this system visit
                            \ to give:
                            \
                            \   A = random AND mask
    
     CLC                    \ Add byte #2 from the market prices table
     ADC QQ23+2,X           \ (base_quantity) so we now have:
                            \
                            \   A = base_quantity + (random AND mask)
    
     LDY QQ19+1             \ Fetch the byte #1 that we stored above and jump to
     BMI TT157              \ TT157 if it is negative (i.e. if the economic_factor
                            \ is negative)
    
     SEC                    \ Set A = A - QQ19+3
     SBC QQ19+3             \
                            \       = base_quantity + (random AND mask)
                            \         - (economy * |economic_factor|)
                            \
                            \ which is the result we want, as the economic_factor
                            \ is positive
    
     JMP TT158              \ Jump to TT158 to skip TT157
    
    .TT157
    
     CLC                    \ Set A = A + QQ19+3
     ADC QQ19+3             \
                            \       = base_quantity + (random AND mask)
                            \         + (economy * |economic_factor|)
                            \
                            \ which is the result we want, as the economic_factor
                            \ is negative
    
    .TT158
    
     BPL TT159              \ If A < 0, then set A = 0, so we don't have negative
     LDA #0                 \ availability
    
    .TT159
    
     LDY XX4                \ Fetch the counter (the market item number) into Y
    
     AND #%00111111         \ Take bits 0-5 of A, i.e. A mod 64, and store this as
     STA AVL,Y              \ this item's availability in the Y=th byte of AVL, so
                            \ each item has a maximum availability of 63t
    
     INY                    \ Increment the counter into XX44, Y and A
     TYA
     STA XX4
    
     ASL A                  \ Set X = counter * 4, so that X points to the next
     ASL A                  \ item's entry in the four-byte market prices table,
     TAX                    \ ready for the next loop
    
     CMP #63                \ If A < 63, jump back up to hy9 to set the availability
     BCC hy9                \ for the next market item
    
    .hyR
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: GTHG                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Spawn a Thargoid ship and a Thargon companion
      Deep dive: [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/gthg.md)
     References: This subroutine is called as follows:
                 * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md) calls GTHG
                 * [MJP](../main/subroutine/mjp.md) calls GTHG
    
    
    
    
    
    
    .GTHG
    
     JSR Ze                 \ Call Ze to initialise INWK to a fairly aggressive
                            \ ship (though we increase this below)
                            \
                            \ Note that because Ze uses the value of X returned by
                            \ DORND, and X contains the value of A returned by the
                            \ previous call to DORND, this does not set the new ship
                            \ to a totally random location
    
     LDA #%11111111         \ Set the AI flag in byte #32 so that the ship has AI,
     STA INWK+32            \ an aggression level of 63 out of 63, and E.C.M.
    
     LDA #THG               \ Call NWSHP to add a new Thargoid ship to our local
     JSR NWSHP              \ bubble of universe
    
     LDA #TGL               \ Call NWSHP to add a new Thargon ship to our local
     JMP NWSHP              \ bubble of universe, and return from the subroutine
                            \ using a tail call
    
    
    
    
           Name: MJP                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Process a mis-jump into witchspace
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mjp.md)
     Variations: See [code variations](../related/compare/main/subroutine/mjp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT18](../main/subroutine/tt18.md) calls MJP
                 * [TT18](../main/subroutine/tt18.md) calls via ptg
                 * [TT18](../main/subroutine/tt18.md) calls via RTS111
    
    
    
    
    
    * * *
    
    
     Process a mis-jump into witchspace (which happens very rarely). Witchspace has
     a strange, almost dust-free aspect to it, and it is populated by aggressive
     Thargoids. Using our escape pod will be fatal, and our position on the
     galactic chart is in-between systems. It is a scary place...
    
     There is a 0.78% chance that this routine is called from TT18 instead of doing
     a normal hyperspace, or we can manually trigger a mis-jump by holding down
     CTRL after first enabling the "author display" configuration option ("X") when
     paused.
    
    
    
    * * *
    
    
     Other entry points:
    
       ptg                  Called when the user manually forces a mis-jump
    
       RTS111               Contains an RTS
    
    
    
    
    .ptg
    
     LSR COK                \ Set bit 0 of the competition flags in COK, so that the
     SEC                    \ competition code will include the fact that we have
     ROL COK                \ manually forced a mis-jump into witchspace
    
    .MJP
    
    \JSR CATLOD             \ This instruction is commented out in the original
                            \ source
    
     LDA #3                 \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 3
    
     JSR LL164              \ Call LL164 to show the hyperspace tunnel and make the
                            \ hyperspace sound for a second time (as we already
                            \ called LL164 in TT18)
    
     JSR RES2               \ Reset a number of flight variables and workspaces, as
                            \ well as setting Y to &FF
    
     STY MJ                 \ Set the mis-jump flag in MJ to &FF, to indicate that
                            \ we are now in witchspace
    
    .MJP1
    
     JSR GTHG               \ Call GTHG to spawn a Thargoid ship and a Thargon
                            \ companion
    
     LDA #2                 \ Fetch the number of Thargoid ships from MANY+THG, and
     CMP MANY+THG           \ if it is less than or equal to 2, loop back to MJP1 to
     BCS MJP1               \ spawn another one, until we have three Thargoids
    
     STA NOSTM              \ Set NOSTM (the maximum number of stardust particles)
                            \ to 3, so there are fewer bits of stardust in
                            \ witchspace (normal space has a maximum of 18)
    
     LDX #0                 \ Initialise the front space view
     JSR LOOK1
    
     LDA QQ1                \ Fetch the current system's galactic y-coordinate in
     EOR #%00011111         \ QQ1 and flip bits 0-5, so we end up somewhere in the
     STA QQ1                \ vicinity of our original destination, but above or
                            \ below it in the galactic chart
    
     RTS                    \ Return from the subroutine
    
    .RTS111
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT18                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Try to initiate a jump into hyperspace
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt18.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt18.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls TT18
    
    
    
    
    
    * * *
    
    
     Try to go through hyperspace. Called from TT102 in the main loop when the
     hyperspace countdown has finished.
    
    
    
    
    .TT18
    
     LDA QQ14               \ Subtract the distance to the selected system (in QQ8)
     SEC                    \ from the amount of fuel in our tank (in QQ14) into A
     SBC QQ8
    
     BCS P%+4               \ If the subtraction didn't overflow, skip the next
                            \ instruction
    
     LDA #0                 \ The subtraction overflowed, so set A = 0 so we don't
                            \ end up with a negative amount of fuel
    
     STA QQ14               \ Store the updated fuel amount in QQ14
    
     LDA QQ11               \ If the current view is not a space view, jump to ee5
     BNE ee5                \ to skip the following
    
     JSR TT66               \ Clear the top part of the screen, draw a border box,
                            \ and set the current view type in QQ11 to 0 (space
                            \ view)
    
     JSR LL164              \ Call LL164 to show the hyperspace tunnel and make the
                            \ hyperspace sound
    
    .ee5
    
    IF _SNG47
    
     JSR CTRL               \ Scan the keyboard to see if CTRL is currently pressed,
                            \ returning a negative value in A if it is
    
    ELIF _COMPACT
    
     JSR CTRLmc             \ Scan the keyboard to see if CTRL is currently pressed,
                            \ returning a negative value in A if it is
    
    ENDIF
    
     AND PATG               \ If the game is configured to show the author's names
                            \ on the start-up screen, then PATG will contain &FF,
                            \ otherwise it will be 0
    
     BMI ptg                \ By now, A will be negative if we are holding down CTRL
                            \ and author names are configured, which is what we have
                            \ to do in order to trigger a manual mis-jump, so jump
                            \ to ptg to do a mis-jump (ptg not only mis-jumps, but
                            \ updates the competition flags, so Acornsoft could tell
                            \ from the competition code whether this feature had
                            \ been used)
    
     JSR DORND              \ Set A and X to random numbers
    
     CMP #253               \ If A >= 253 (0.78% chance) then jump to MJP to trigger
     BCS MJP                \ a mis-jump into witchspace
    
    \JSR TT111              \ This instruction is commented out in the original
                            \ source. It finds the closest system to coordinates
                            \ (QQ9, QQ10), but we don't need to do this as the
                            \ crosshairs will already be on a system by this point
    
     JSR hyp1+3             \ Jump straight to the system at (QQ9, QQ10) without
                            \ first calculating which system is closest
    
     JSR RES2               \ Reset a number of flight variables and workspaces
    
     JSR SOLAR              \ Halve our legal status, update the missile indicators,
                            \ and set up data blocks and slots for the planet and
                            \ sun
    
    \JSR CATLOD             \ These instructions are commented out in the original
    \                       \ source
    \JSR LOMOD
    
     LDA QQ11               \ If the current view in QQ11 is not a space view (0) or
     AND #%00111111         \ one of the charts (64 or 128), return from the
     BNE RTS111             \ subroutine (as RTS111 contains an RTS)
    
     JSR TTX66              \ Otherwise clear the screen and draw a border box
    
     LDA QQ11               \ If the current view is one of the charts, jump to
     BNE TT114              \ TT114 (from which we jump to the correct routine to
                            \ display the chart)
    
     INC QQ11               \ This is a space view, so increment QQ11 to 1
    
                            \ Fall through into TT110 to show the front space view
    
    
    
    
           Name: TT110                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Launch from a station or show the front space view
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt110.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt110.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Ghy](../main/subroutine/ghy.md) calls TT110
                 * [TT102](../main/subroutine/tt102.md) calls TT110
    
    
    
    
    
    * * *
    
    
     Launch the ship (if we are docked), or show the front space view (if we are
     already in space).
    
     Called when red key f0 is pressed while docked (launch), after we arrive in a
     new galaxy, or after a hyperspace if the current view is a space view.
    
    
    
    
    .TT110
    
     LDX QQ12               \ If we are not docked (QQ12 = 0) then jump to NLUNCH
     BEQ NLUNCH             \ to skip the launch tunnel and setup process
    
     JSR LAUN               \ Show the space station launch tunnel
    
     JSR RES2               \ Reset a number of flight variables and workspaces
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     INC INWK+8             \ Increment z_sign ready for the call to SOS, so the
                            \ planet appears at a z_sign of 1 in front of us when
                            \ we launch
    
     JSR SOS1               \ Call SOS1 to set up the planet's data block and add it
                            \ to FRIN, where it will get put in the first slot as
                            \ it's the first one to be added to our local bubble of
                            \ universe following the call to RES2 above
    
     LDA #128               \ For the space station, set z_sign to &80, so it's
     STA INWK+8             \ behind us (&80 is negative)
    
     INC INWK+7             \ And increment z_hi, so it's only just behind us
    
     JSR NWSPS              \ Add a new space station to our local bubble of
                            \ universe
    
     LDA #12                \ Set our launch speed in DELTA to 12
     STA DELTA
    
     JSR BAD                \ Call BAD to work out how much illegal contraband we
                            \ are carrying in our hold (A is up to 40 for a
                            \ standard hold crammed with contraband, up to 70 for
                            \ an extended cargo hold full of narcotics and slaves)
    
     ORA FIST               \ OR the value in A with our legal status in FIST to
                            \ get a new value that is at least as high as both
                            \ values, to reflect the fact that launching with a
                            \ hold full of contraband can only make matters worse
    
     STA FIST               \ Update our legal status with the new value
    
     LDA #255               \ Set the view type in QQ11 to 255
     STA QQ11
    
     JSR HFS1               \ Call HFS1 to draw 8 concentric rings to remove the
                            \ launch tunnel that we drew above
    
    .NLUNCH
    
     LDX #0                 \ Set QQ12 to 0 to indicate we are not docked
     STX QQ12
    
     JMP LOOK1              \ Jump to LOOK1 to switch to the front view (X = 0),
                            \ returning from the subroutine using a tail call
    
    
    
    
           Name: TT114                                                   [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Display either the Long-range or Short-range Chart
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt114.md)
     References: This subroutine is called as follows:
                 * [TT18](../main/subroutine/tt18.md) calls TT114
    
    
    
    
    
    * * *
    
    
     Display either the Long-range or Short-range Chart, depending on the current
     view setting. Called from TT18 once we know the current view is one of the
     charts.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The current view, loaded from QQ11
    
    
    
    
    .TT114
    
     BMI TT115              \ If bit 7 of the current view is set (i.e. the view is
                            \ the Short-range Chart, 128), skip to TT115 below to
                            \ jump to TT23 to display the chart
    
     JMP TT22               \ Otherwise the current view is the Long-range Chart, so
                            \ jump to TT22 to display it
    
    .TT115
    
     JMP TT23               \ Jump to TT23 to display the Short-range Chart
    
    
    
    
           Name: LCASH                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Subtract an amount of cash from the cash pot
    
    
        Context: See this subroutine [on its own page](../main/subroutine/lcash.md)
     References: This subroutine is called as follows:
                 * [eq](../main/subroutine/eq.md) calls LCASH
                 * [TT219](../main/subroutine/tt219.md) calls LCASH
    
    
    
    
    
    * * *
    
    
     Subtract (Y X) cash from the cash pot in CASH, but only if there is enough
     cash in the pot. As CASH is a four-byte number, this calculates:
    
       CASH(0 1 2 3) = CASH(0 1 2 3) - (0 0 Y X)
    
    
    
    * * *
    
    
     Returns:
    
       C flag               If set, there was enough cash to do the subtraction
    
                            If clear, there was not enough cash to do the
                            subtraction
    
    
    
    
    .LCASH
    
     STX T1                 \ Subtract the least significant bytes:
     LDA CASH+3             \
     SEC                    \   CASH+3 = CASH+3 - X
     SBC T1
     STA CASH+3
    
     STY T1                 \ Then the second most significant bytes:
     LDA CASH+2             \
     SBC T1                 \   CASH+2 = CASH+2 - Y
     STA CASH+2
    
     LDA CASH+1             \ Then the third most significant bytes (which are 0):
     SBC #0                 \
     STA CASH+1             \   CASH+1 = CASH+1 - 0
    
     LDA CASH               \ And finally the most significant bytes (which are 0):
     SBC #0                 \
     STA CASH               \   CASH = CASH - 0
    
     BCS TT113              \ If the C flag is set then the subtraction didn't
                            \ underflow, so the value in CASH is correct and we can
                            \ jump to TT113 to return from the subroutine with the
                            \ C flag set to indicate success (as TT113 contains an
                            \ RTS)
    
                            \ Otherwise we didn't have enough cash in CASH to
                            \ subtract (Y X) from it, so fall through into
                            \ MCASH to reverse the sum and restore the original
                            \ value in CASH, and returning with the C flag clear
    
    
    
    
           Name: MCASH                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Add an amount of cash to the cash pot
    
    
        Context: See this subroutine [on its own page](../main/subroutine/mcash.md)
     References: This subroutine is called as follows:
                 * [DEBRIEF](../main/subroutine/debrief.md) calls MCASH
                 * [EQSHP](../main/subroutine/eqshp.md) calls MCASH
                 * [Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md) calls MCASH
                 * [refund](../main/subroutine/refund.md) calls MCASH
                 * [TT210](../main/subroutine/tt210.md) calls MCASH
                 * [LCASH](../main/subroutine/lcash.md) calls via TT113
    
    
    
    
    
    * * *
    
    
     Add (Y X) cash to the cash pot in CASH. As CASH is a four-byte number, this
     calculates:
    
       CASH(0 1 2 3) = CASH(0 1 2 3) + (Y X)
    
    
    
    * * *
    
    
     Other entry points:
    
       TT113                Contains an RTS
    
    
    
    
    .MCASH
    
     TXA                    \ Add the least significant bytes:
     CLC                    \
     ADC CASH+3             \   CASH+3 = CASH+3 + X
     STA CASH+3
    
     TYA                    \ Then the second most significant bytes:
     ADC CASH+2             \
     STA CASH+2             \   CASH+2 = CASH+2 + Y
    
     LDA CASH+1             \ Then the third most significant bytes (which are 0):
     ADC #0                 \
     STA CASH+1             \   CASH+1 = CASH+1 + 0
    
     LDA CASH               \ And finally the most significant bytes (which are 0):
     ADC #0                 \
     STA CASH               \   CASH = CASH + 0
    
     CLC                    \ Clear the C flag, so if the above was done following
                            \ a failed LCASH call, the C flag correctly indicates
                            \ failure
    
    .TT113
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: GCASH                                                   [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (Y X) = P * Q * 4
    
    
        Context: See this subroutine [on its own page](../main/subroutine/gcash.md)
     References: This subroutine is called as follows:
                 * [TT210](../main/subroutine/tt210.md) calls GCASH
                 * [TT219](../main/subroutine/tt219.md) calls GCASH
    
    
    
    
    
    * * *
    
    
     Calculate the following multiplication of unsigned 8-bit numbers:
    
       (Y X) = P * Q * 4
    
    
    
    
    .GCASH
    
     JSR MULTU              \ Call MULTU to calculate (A P) = P * Q
    
    
    
    
           Name: GC2                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (Y X) = (A P) * 4
    
    
        Context: See this subroutine [on its own page](../main/subroutine/gc2.md)
     References: This subroutine is called as follows:
                 * [TT151](../main/subroutine/tt151.md) calls GC2
    
    
    
    
    
    * * *
    
    
     Calculate the following multiplication of unsigned 16-bit numbers:
    
       (Y X) = (A P) * 4
    
    
    
    
    .GC2
    
     ASL P                  \ Set (A P) = (A P) * 4
     ROL A
     ASL P
     ROL A
    
     TAY                    \ Set (Y X) = (A P)
     LDX P
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: EQSHP                                                   [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Show the Equip Ship screen (red key f3)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/eqshp.md)
     Variations: See [code variations](../related/compare/main/subroutine/eqshp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](../main/subroutine/tt102.md) calls EQSHP
                 * [eq](../main/subroutine/eq.md) calls via err
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       err                  Beep, pause and go to the docking bay (i.e. show the
                            Status Mode screen)
    
       pres                 Given an item number A with the item name in recursive
                            token Y, show an error to say that the item is already
                            present, refund the cost of the item, and then beep and
                            exit to the docking bay (i.e. show the Status Mode
                            screen)
    
    
    
    
    .bay
    
     JMP BAY                \ Go to the docking bay (i.e. show the Status Mode
                            \ screen)
    
    .EQSHP
    
     LDA #32                \ Clear the top part of the screen, draw a border box,
     JSR TRADEMODE          \ and set up a printable trading screen with a view type
                            \ in QQ11 of 32 (Equip Ship screen)
    
     LDA #12                \ Move the text cursor to column 12
     STA XC
    
     LDA #207               \ Print recursive token 47 ("EQUIP") followed by a space
     JSR spc
    
     LDA #185               \ Print recursive token 25 ("SHIP") and draw a
     JSR NLIN3              \ horizontal line at pixel row 19 to box in the title
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch to Sentence Case, with the
     STA QQ17               \ next letter in capitals
    
     INC YC                 \ Move the text cursor down one line
    
     LDA tek                \ Fetch the tech level of the current system from tek
     CLC                    \ and add 3 (the tech level is stored as 0-14, so A is
     ADC #3                 \ now set to between 3 and 17)
    
     CMP #12                \ If A >= 12 then set A = 14, so A is now set to between
     BCC P%+4               \ 3 and 14
     LDA #14
    
     STA Q                  \ Set QQ25 = A (so QQ25 is in the range 3-14 and
     STA QQ25               \ represents number of the most advanced item available
     INC Q                  \ in this system, which we can pass to gnum below when
                            \ asking which item we want to buy)
                            \
                            \ Set Q = A + 1 (so Q is in the range 4-15 and contains
                            \ QQ25 + 1, i.e. the highest item number on sale + 1)
    
     LDA #70                \ Set A = 70 - QQ14, where QQ14 contains the current
     SEC                    \ fuel in light years * 10, so this leaves the amount
     SBC QQ14               \ of fuel we need to fill 'er up (in light years * 10)
    
     ASL A                  \ The price of fuel is always 2 Cr per light year, so we
     STA PRXS               \ double A and store it in PRXS, as the first price in
                            \ the price list (which is reserved for fuel), and
                            \ because the table contains prices as price * 10, it's
                            \ in the right format (so tank containing 7.0 light
                            \ years of fuel would be 14.0 Cr, or a PRXS value of
                            \ 140)
    
     LDX #1                 \ We are now going to work our way through the equipment
                            \ price list at PRXS, printing out the equipment that is
                            \ available at this station, so set a counter in X,
                            \ starting at 1, to hold the number of the current item
                            \ plus 1 (so the item number in X loops through 1-13)
    
    .EQL1
    
     STX XX13               \ Store the current item number + 1 in XX13
    
     JSR TT67               \ Print a newline
    
     LDX XX13               \ Print the current item number + 1 to 3 digits, left-
     CLC                    \ padding with spaces, and with no decimal point, so the
     JSR pr2                \ items are numbered from 1
    
     JSR TT162              \ Print a space
    
     LDA XX13               \ Print recursive token 104 + XX13, which will be in the
     CLC                    \ range 105 ("FUEL") to 116 ("GALACTIC HYPERSPACE ")
     ADC #104               \ so this prints the current item's name
     JSR TT27
    
     LDA XX13               \ Call prx-3 to set (Y X) to the price of the item with
     JSR prx-3              \ number XX13 - 1 (as XX13 contains the item number + 1)
    
     SEC                    \ Set the C flag so we will print a decimal point when
                            \ we print the price
    
     LDA #25                \ Move the text cursor to column 25
     STA XC
    
     LDA #6                 \ Print the number in (Y X) to 6 digits, left-padding
     JSR TT11               \ with spaces and including a decimal point, which will
                            \ be the correct price for this item as (Y X) contains
                            \ the price * 10, so the trailing zero will go after the
                            \ decimal point (i.e. 5250 will be printed as 525.0)
    
     LDX XX13               \ Increment the current item number in XX13
     INX
    
     CPX Q                  \ If X < Q, loop back up to print the next item on the
     BCC EQL1               \ list of equipment available at this station
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
     LDA #127               \ Print recursive token 127 ("ITEM") followed by a
     JSR prq                \ question mark
    
     JSR gnum               \ Call gnum to get a number from the keyboard, which
                            \ will be the number of the item we want to purchase,
                            \ returning the number entered in A and R, and setting
                            \ the C flag if the number is bigger than the highest
                            \ item number in QQ25
    
     BEQ bay                \ If no number was entered, jump up to bay to go to the
                            \ docking bay (i.e. show the Status Mode screen)
    
     BCS bay                \ If the number entered was too big, jump up to bay to
                            \ go to the docking bay (i.e. show the Status Mode
                            \ screen)
    
     SBC #0                 \ Set A to the number entered - 1 (because the C flag is
                            \ clear), which will be the actual item number we want
                            \ to buy
    
     PHA                    \ Store A on the stack so we can restore it after the
                            \ following call to DOXC
    
     LDA #2                 \ Move the text cursor to column 2
     STA XC
    
     INC YC                 \ Move the text cursor down one line
    
     PLA                    \ Restore A from the stack
    
     PHA                    \ While preserving the value in A, call eq to subtract
     JSR eq                 \ the price of the item we want to buy (which is in A)
     PLA                    \ from our cash pot, but only if we have enough cash in
                            \ the pot. If we don't have enough cash, exit to the
                            \ docking bay (i.e. show the Status Mode screen)
    
     BNE et0                \ If A is not 0 (i.e. the item we've just bought is not
                            \ fuel), skip to et0
    
     LDX #70                \ Set the current fuel level * 10 in QQ14 to 70, or 7.0
     STX QQ14               \ light years (a full tank)
    
    .et0
    
     CMP #1                 \ If A is not 1 (i.e. the item we've just bought is not
     BNE et1                \ a missile), skip to et1
    
     LDX NOMSL              \ Fetch the current number of missiles from NOMSL into X
    
     INX                    \ Increment X to the new number of missiles
    
     LDY #124               \ Set Y to recursive token 124 ("ALL")
    
     CPX #5                 \ If buying this missile would give us 5 missiles, this
     BCS pres               \ is more than the maximum of 4 missiles that we can
                            \ fit, so jump to pres to show the error "All Present",
                            \ beep and exit to the docking bay (i.e. show the Status
                            \ Mode screen)
    
     STX NOMSL              \ Otherwise update the number of missiles in NOMSL
    
     JSR msblob             \ Reset the dashboard's missile indicators so none of
                            \ them are targeted
    
     LDA #1                 \ Set A to 1 as the call to msblob will have overwritten
                            \ the original value, and we still need it set
                            \ correctly so we can continue through the conditional
                            \ statements for all the other equipment
    
    .et1
    
     LDY #107               \ Set Y to recursive token 107 ("LARGE CARGO{sentence
                            \ case} BAY")
    
     CMP #2                 \ If A is not 2 (i.e. the item we've just bought is not
     BNE et2                \ a large cargo bay), skip to et2
    
     LDX #37                \ If our current cargo capacity in CRGO is 37, then we
     CPX CRGO               \ already have a large cargo bay fitted, so jump to pres
     BEQ pres               \ to show the error "Large Cargo Bay Present", beep and
                            \ exit to the docking bay (i.e. show the Status Mode
                            \ screen)
    
     STX CRGO               \ Otherwise we just scored ourselves a large cargo bay,
                            \ so update our current cargo capacity in CRGO to 37
    
    .et2
    
     CMP #3                 \ If A is not 3 (i.e. the item we've just bought is not
     BNE et3                \ an E.C.M. system), skip to et3
    
     INY                    \ Increment Y to recursive token 108 ("E.C.M.SYSTEM")
    
     LDX ECM                \ If we already have an E.C.M. fitted (i.e. ECM is
     BNE pres               \ non-zero), jump to pres to show the error "E.C.M.
                            \ System Present", beep and exit to the docking bay
                            \ (i.e. show the Status Mode screen)
    
     DEC ECM                \ Otherwise we just took delivery of a brand new E.C.M.
                            \ system, so set ECM to &FF (as ECM was 0 before the DEC
                            \ instruction)
    
    .et3
    
     CMP #4                 \ If A is not 4 (i.e. the item we've just bought is not
     BNE et4                \ an extra pulse laser), skip to et4
    
     JSR qv                 \ Print a menu listing the four views, with a "View ?"
                            \ prompt, and ask for a view number, which is returned
                            \ in X (which now contains 0-3)
    
     LDA #POW               \ Call refund with A set to the power of the new pulse
     JSR refund             \ laser to install the new laser and process a refund if
                            \ we already have a laser fitted to this view
    
     LDA #4                 \ Set A to 4 as we just overwrote the original value,
                            \ and we still need it set correctly so we can continue
                            \ through the conditional statements for all the other
                            \ equipment
    
    .et4
    
     CMP #5                 \ If A is not 5 (i.e. the item we've just bought is not
     BNE et5                \ an extra beam laser), skip to et5
    
     JSR qv                 \ Print a menu listing the four views, with a "View ?"
                            \ prompt, and ask for a view number, which is returned
                            \ in X (which now contains 0-3)
    
     LDA #POW+128           \ Call refund with A set to the power of the new beam
     JSR refund             \ laser to install the new laser and process a refund if
                            \ we already have a laser fitted to this view
    
    .et5
    
     LDY #111               \ Set Y to recursive token 111 ("FUEL SCOOPS")
    
     CMP #6                 \ If A is not 6 (i.e. the item we've just bought is not
     BNE et6                \ a fuel scoop), skip to et6
    
     LDX BST                \ If we already have fuel scoops fitted (i.e. BST is
     BEQ ed9                \ zero), jump to ed9, otherwise fall through into pres
                            \ to show the error "Fuel Scoops Present", beep and
                            \ exit to the docking bay (i.e. show the Status Mode
                            \ screen)
    
    .pres
    
                            \ If we get here we need to show an error to say that
                            \ the item whose name is in recursive token Y is already
                            \ present, and then process a refund for the cost of
                            \ item number A
    
     STY K                  \ Store the item's name in K
    
     JSR prx                \ Call prx to set (Y X) to the price of equipment item
                            \ number A
    
     JSR MCASH              \ Add (Y X) cash to the cash pot in CASH, as the station
                            \ already took the money for this item in the JSR eq
                            \ instruction above, but we can't fit the item, so need
                            \ our money back
    
     LDA K                  \ Print the recursive token in K (the item's name)
     JSR spc                \ followed by a space
    
     LDA #31                \ Print recursive token 145 ("PRESENT")
     JSR TT27
    
    .err
    
     JSR dn2                \ Call dn2 to make a short, high beep and delay for 1
                            \ second
    
     JMP BAY                \ Jump to BAY to go to the docking bay (i.e. show the
                            \ Status Mode screen)
    
    .ed9
    
     DEC BST                \ We just bought a shiny new fuel scoop, so set BST to
                            \ &FF (as BST was 0 before the jump to ed9 above)
    
    .et6
    
     INY                    \ Increment Y to recursive token 112 ("E.C.M.SYSTEM")
    
     CMP #7                 \ If A is not 7 (i.e. the item we've just bought is not
     BNE et7                \ an escape pod), skip to et7
    
     LDX ESCP               \ If we already have an escape pod fitted (i.e. ESCP is
     BNE pres               \ non-zero), jump to pres to show the error "Escape Pod
                            \ Present", beep and exit to the docking bay (i.e. show
                            \ the Status Mode screen)
    
     DEC ESCP               \ Otherwise we just bought an escape pod, so set ESCP
                            \ to &FF (as ESCP was 0 before the DEC instruction)
    
    .et7
    
     INY                    \ Increment Y to recursive token 113 ("ENERGY BOMB")
    
     CMP #8                 \ If A is not 8 (i.e. the item we've just bought is not
     BNE et8                \ an energy bomb), skip to et8
    
     LDX BOMB               \ If we already have an energy bomb fitted (i.e. BOMB
     BNE pres               \ is non-zero), jump to pres to show the error "Energy
                            \ Bomb Present", beep and exit to the docking bay (i.e.
                            \ show the Status Mode screen)
    
     LDX #&7F               \ Otherwise we just bought an energy bomb, so set BOMB
     STX BOMB               \ to &7F
    
    .et8
    
     INY                    \ Increment Y to recursive token 114 ("ENERGY UNIT")
    
     CMP #9                 \ If A is not 9 (i.e. the item we've just bought is not
     BNE etA                \ an energy unit), skip to etA
    
     LDX ENGY               \ If we already have an energy unit fitted (i.e. ENGY is
     BNE pres               \ non-zero), jump to pres to show the error "Energy Unit
                            \ Present", beep and exit to the docking bay (i.e. show
                            \ the Status Mode screen)
    
     INC ENGY               \ Otherwise we just picked up an energy unit, so set
                            \ ENGY to 1 (as ENGY was 0 before the INC instruction)
    
    .etA
    
     INY                    \ Increment Y to recursive token 115 ("DOCKING
                            \ COMPUTERS")
    
     CMP #10                \ If A is not 10 (i.e. the item we've just bought is not
     BNE etB                \ a docking computer), skip to etB
    
     LDX DKCMP              \ If we already have a docking computer fitted (i.e.
     BNE pres               \ DKCMP is non-zero), jump to pres to show the error
                            \ "Docking Computer Present", beep and exit to the
                            \ docking bay (i.e. show the Status Mode screen)
    
     DEC DKCMP              \ Otherwise we just got hold of a docking computer, so
                            \ set DKCMP to &FF (as DKCMP was 0 before the DEC
                            \ instruction)
    
    .etB
    
     INY                    \ Increment Y to recursive token 116 ("GALACTIC
                            \ HYPERSPACE ")
    
     CMP #11                \ If A is not 11 (i.e. the item we've just bought is not
     BNE et9                \ a galactic hyperdrive), skip to et9
    
     LDX GHYP               \ If we already have a galactic hyperdrive fitted (i.e.
     BNE pres               \ GHYP is non-zero), jump to pres to show the error
                            \ "Galactic Hyperspace Present", beep and exit to the
                            \ docking bay (i.e. show the Status Mode screen)
    
     DEC GHYP               \ Otherwise we just splashed out on a galactic
                            \ hyperdrive, so set GHYP to &FF (as GHYP was 0 before
                            \ the DEC instruction)
    
    .et9
    
     INY                    \ Increment Y to recursive token 117 ("MILITARY  LASER")
    
     CMP #12                \ If A is not 12 (i.e. the item we've just bought is not
     BNE et10               \ a military laser), skip to et10
    
     JSR qv                 \ Print a menu listing the four views, with a "View ?"
                            \ prompt, and ask for a view number, which is returned
                            \ in X (which now contains 0-3)
    
     LDA #Armlas            \ Call refund with A set to the power of the new
     JSR refund             \ military laser to install the new laser and process a
                            \ refund if we already have a laser fitted to this view
    
    .et10
    
     INY                    \ Increment Y to recursive token 118 ("MINING  LASER")
    
     CMP #13                \ If A is not 13 (i.e. the item we've just bought is not
     BNE et11               \ a mining laser), skip to et11
    
     JSR qv                 \ Print a menu listing the four views, with a "View ?"
                            \ prompt, and ask for a view number, which is returned
                            \ in X (which now contains 0-3)
    
     LDA #Mlas              \ Call refund with A set to the power of the new mining
     JSR refund             \ laser to install the new laser and process a refund if
                            \ we already have a laser fitted to this view
    
    .et11
    
     JSR dn                 \ We are done buying equipment, so print the amount of
                            \ cash left in the cash pot, then make a short, high
                            \ beep to confirm the purchase, and delay for 1 second
    
     JMP EQSHP              \ Jump back up to EQSHP to show the Equip Ship screen
                            \ again and see if we can't track down another bargain
    
    
    
    
           Name: dn                                                      [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print the amount of money we have left in the cash pot, then make
                 a short, high beep and delay for 1 second
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dn.md)
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls dn
                 * [TT219](../main/subroutine/tt219.md) calls dn
    
    
    
    
    
    
    .dn
    
     JSR TT162              \ Print a space
    
     LDA #119               \ Print recursive token 119 ("CASH:{cash} CR{crlf}")
     JSR spc                \ followed by a space
    
                            \ Fall through into dn2 to make a beep and delay for
                            \ 1 second before returning from the subroutine
    
    
    
    
           Name: dn2                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Make a short, high beep and delay for 0.5 seconds
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dn2.md)
     Variations: See [code variations](../related/compare/main/subroutine/dn2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls dn2
                 * [NWDAV4](../main/subroutine/nwdav4.md) calls dn2
                 * [TT210](../main/subroutine/tt210.md) calls dn2
                 * [TT219](../main/subroutine/tt219.md) calls dn2
    
    
    
    
    
    
    .dn2
    
     JSR BEEP               \ Call the BEEP subroutine to make a short, high beep
    
     LDY #25                \ Wait for 25/50 of a second (0.5 second) and return
     JMP DELAY              \ from the subroutine using a tail call
    
    
    
    
           Name: eq                                                      [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Subtract the price of equipment from the cash pot
    
    
        Context: See this subroutine [on its own page](../main/subroutine/eq.md)
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls eq
    
    
    
    
    
    * * *
    
    
     If we have enough cash, subtract the price of a specified piece of equipment
     from our cash pot and return from the subroutine. If we don't have enough
     cash, exit to the docking bay (i.e. show the Status Mode screen).
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The item number of the piece of equipment (0-11) as
                            shown in the table at PRXS
    
    
    
    
    .eq
    
     JSR prx                \ Call prx to set (Y X) to the price of equipment item
                            \ number A
    
     JSR LCASH              \ Subtract (Y X) cash from the cash pot, but only if
                            \ we have enough cash
    
     BCS c                  \ If the C flag is set then we did have enough cash for
                            \ the transaction, so jump to c to return from the
                            \ subroutine (as c contains an RTS)
    
     LDA #197               \ Otherwise we don't have enough cash to buy this piece
     JSR prq                \ of equipment, so print recursive token 37 ("CASH")
                            \ followed by a question mark
    
     JMP err                \ Jump to err to beep, pause and go to the docking bay
                            \ (i.e. show the Status Mode screen)
    
    
    
    
           Name: prx                                                     [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Return the price of a piece of equipment
    
    
        Context: See this subroutine [on its own page](../main/subroutine/prx.md)
     Variations: See [code variations](../related/compare/main/subroutine/prx.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [eq](../main/subroutine/eq.md) calls prx
                 * [EQSHP](../main/subroutine/eqshp.md) calls prx
                 * [refund](../main/subroutine/refund.md) calls prx
                 * [EQSHP](../main/subroutine/eqshp.md) calls via prx-3
                 * [eq](../main/subroutine/eq.md) calls via c
    
    
    
    
    
    * * *
    
    
     This routine returns the price of equipment as listed in the table at PRXS.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The item number of the piece of equipment (0-13) as
                            shown in the table at PRXS
    
    
    
    * * *
    
    
     Returns:
    
       (Y X)                The item price in Cr * 10 (Y = high byte, X = low byte)
    
    
    
    * * *
    
    
     Other entry points:
    
       prx-3                Return the price of the item with number A - 1
    
       c                    Contains an RTS
    
    
    
    
     SEC                    \ Decrement A (for when this routine is called via
     SBC #1                 \ prx-3)
    
    .prx
    
     ASL A                  \ Set Y = A * 2, so it can act as an index into the
     TAY                    \ PRXS table, which has two bytes per entry
    
     LDX PRXS,Y             \ Fetch the low byte of the price into X
    
     LDA PRXS+1,Y           \ Fetch the high byte of the price into A and transfer
     TAY                    \ it to X, so the price is now in (Y X)
    
    .c
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: qv                                                      [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Print a menu of the four space views, for buying lasers
    
    
        Context: See this subroutine [on its own page](../main/subroutine/qv.md)
     Variations: See [code variations](../related/compare/main/subroutine/qv.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls qv
    
    
    
    
    
    * * *
    
    
     Print a menu in the bottom-middle of the screen, at row 16, column 12, that
     lists the four available space views, like this:
    
                     0 Front
                     1 Rear
                     2 Left
                     3 Right
    
     Also print a "View ?" prompt and ask for a view number. The menu is shown
     when we choose to buy a new laser in the Equip Ship screen.
    
    
    
    * * *
    
    
     Returns:
    
       X                    The chosen view number (0-3)
    
    
    
    
    .qv
    
     LDA tek                \ If the current system's tech level is less than 8,
     CMP #8                 \ skip the next two instructions, otherwise we clear the
     BCC P%+7               \ screen to prevent the view menu from clashing with the
                            \ longer equipment menu available in higher tech systems
    
     LDA #32                \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 32 (Equip
                            \ Ship screen)
    
     LDA #16                \ Move the text cursor to row 16, and at the same time
     TAY                    \ set Y to a counter going from 16 to 19 in the loop
     STA YC                 \ below
    
    .qv1
    
     LDA #12                \ Move the text cursor to column 12
     STA XC
    
     TYA                    \ Transfer the counter value from Y to A
    
     CLC                    \ Print ASCII character "0" - 16 + A, so as A goes from
     ADC #'0'-16            \ 16 to 19, this prints "0" through "3" followed by a
     JSR spc                \ space
    
     LDA YC                 \ Print recursive text token 80 + YC, so as YC goes from
     CLC                    \ 16 to 19, this prints "FRONT", "REAR", "LEFT" and
     ADC #80                \ "RIGHT"
     JSR TT27
    
     INC YC                 \ Move the text cursor down a row, and increment the
                            \ counter in YC at the same time
    
     LDY YC                 \ Update Y with the incremented counter in YC
    
     CPY #20                \ If Y < 20 then loop back up to qv1 to print the next
     BCC qv1                \ view in the menu
    
     JSR CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ and move the text cursor to the first cleared row
    
    .qv2
    
     LDA #175               \ Print recursive text token 15 ("VIEW ") followed by
     JSR prq                \ a question mark
    
     JSR TT217              \ Scan the keyboard until a key is pressed, and return
                            \ the key's ASCII code in A (and X)
    
     SEC                    \ Subtract ASCII "0" from the key pressed, to leave the
     SBC #'0'               \ numeric value of the key in A (if it was a number key)
    
     CMP #4                 \ If the number entered in A < 4, then it is a valid
     BCC qv3                \ view number, so jump down to qv3 as we are done
    
     JSR CLYNS              \ Otherwise we didn't get a valid view number, so clear
                            \ the bottom three text rows of the upper screen, and
                            \ move the text cursor to column 1 on row 21
    
     JMP qv2                \ Jump back to qv2 to try again
    
    .qv3
    
     TAX                    \ We have a valid view number, so transfer it to X
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: hm                                                      [Show more]
           Type: Subroutine
       Category: Charts
        Summary: Select the closest system and redraw the chart crosshairs
    
    
        Context: See this subroutine [on its own page](../main/subroutine/hm.md)
     Variations: See [code variations](../related/compare/main/subroutine/hm.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hyp](../main/subroutine/hyp.md) calls hm
                 * [TT102](../main/subroutine/tt102.md) calls hm
    
    
    
    
    
    * * *
    
    
     Set the system closest to galactic coordinates (QQ9, QQ10) as the selected
     system, redraw the crosshairs on the chart accordingly (if they are being
     shown), and if this is not the space view, clear the bottom three text rows of
     the screen.
    
    
    
    
    .hm
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will erase the crosshairs currently there
    
     JSR TT111              \ Select the system closest to galactic coordinates
                            \ (QQ9, QQ10)
    
     JSR TT103              \ Draw small crosshairs at coordinates (QQ9, QQ10),
                            \ which will draw the crosshairs at our current home
                            \ system
    
     JMP CLYNS              \ Clear the bottom three text rows of the upper screen,
                            \ move the text cursor to the first cleared row, and
                            \ return from the subroutine using a tail call
    
    
    
    
           Name: refund                                                  [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Install a new laser, processing a refund if applicable
    
    
        Context: See this subroutine [on its own page](../main/subroutine/refund.md)
     Variations: See [code variations](../related/compare/main/subroutine/refund.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) calls refund
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The power of the new laser to be fitted
    
       X                    The view number for fitting the new laser (0-3)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       X                    X is preserved
    
    
    
    
    \.ref2                  \ These instructions are commented out in the original
    \                       \ source, but they would jump to pres in the EQSHP
    \LDY #187               \ routine with Y = 187, which would show the error:
    \JMP pres               \ "LASER PRESENT" (this code was part of the refund
    \                       \ bug in the BBC Micro disc version of Elite, which
    \Belgium                \ is why it is commented out)
                            \
                            \ There is also a comment in the original source - the
                            \ solitary word "Belgium"
                            \
                            \ This is presumably a reference to the Hitchhiker's
                            \ Guide to the Galaxy, which says that Belgium is the
                            \ galaxy's most unspeakably rude word, so this no doubt
                            \ reflects the authors' strong feelings on the refund
                            \ bug
    
    .refund
    
     STA T1                 \ Store A in T1 so we can retrieve it later
    
     LDA LASER,X            \ If there is no laser in view X (i.e. the laser power
     BEQ ref3               \ is zero), jump to ref3 to skip the refund code
    
    \CMP T1                 \ These instructions are commented out in the original
    \BEQ ref2               \ source, but they would jump to ref2 above if we were
                            \ trying to replace a laser with one of the same type
                            \ (this code was part of the refund bug in the BBC Micro
                            \ disc version of Elite, which is why it is commented
                            \ out)
    
     LDY #4                 \ If the current laser has power #POW (pulse laser),
     CMP #POW               \ jump to ref1 with Y = 4 (the item number of a pulse
     BEQ ref1               \ laser in the table at PRXS)
    
     LDY #5                 \ If the current laser has power #POW+128 (beam laser),
     CMP #POW+128           \ jump to ref1 with Y = 5 (the item number of a beam
     BEQ ref1               \ laser in the table at PRXS)
    
     LDY #12                \ If the current laser has power #Armlas (military
     CMP #Armlas            \ laser), jump to ref1 with Y = 12 (the item number of a
     BEQ ref1               \ military laser in the table at PRXS)
    
     LDY #13                \ Otherwise this is a mining laser, so fall through into
                            \ ref1 with Y = 13 (the item number of a mining laser in
                            \ the table at PRXS)
    
    .ref1
    
                            \ We now want to refund the laser of type Y that we are
                            \ exchanging for the new laser
    
     STX ZZ                 \ Store the view number in ZZ so we can retrieve it
                            \ later
    
     TYA                    \ Copy the laser type to be refunded from Y to A
    
     JSR prx                \ Call prx to set (Y X) to the price of equipment item
                            \ number A
    
     JSR MCASH              \ Call MCASH to add (Y X) to the cash pot
    
     LDX ZZ                 \ Retrieve the view number from ZZ
    
    .ref3
    
                            \ Finally, we install the new laser
    
     LDA T1                 \ Retrieve the new laser's power from T1 into A
    
     STA LASER,X            \ Set the laser view to the new laser's power
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PRXS                                                    [Show more]
           Type: Variable
       Category: Equipment
        Summary: Equipment prices
    
    
        Context: See this variable [on its own page](../main/variable/prxs.md)
     Variations: See [code variations](../related/compare/main/variable/prxs.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [EQSHP](../main/subroutine/eqshp.md) uses PRXS
                 * [prx](../main/subroutine/prx.md) uses PRXS
    
    
    
    
    
    * * *
    
    
     Equipment prices are stored as 10 * the actual value, so we can support prices
     with fractions of credits (0.1 Cr). This is used for the price of fuel only.
    
    
    
    
    .PRXS
    
     EQUW 1                 \ 0  Fuel, calculated in EQSHP  140.0 Cr (full tank)
     EQUW 300               \ 1  Missile                     30.0 Cr
     EQUW 4000              \ 2  Large Cargo Bay            400.0 Cr
     EQUW 6000              \ 3  E.C.M. System              600.0 Cr
     EQUW 4000              \ 4  Extra Pulse Lasers         400.0 Cr
     EQUW 10000             \ 5  Extra Beam Lasers         1000.0 Cr
     EQUW 5250              \ 6  Fuel Scoops                525.0 Cr
     EQUW 10000             \ 7  Escape Pod                1000.0 Cr
     EQUW 9000              \ 8  Energy Bomb                900.0 Cr
     EQUW 15000             \ 9  Energy Unit               1500.0 Cr
     EQUW 10000             \ 10 Docking Computer          1000.0 Cr
     EQUW 50000             \ 11 Galactic Hyperspace       5000.0 Cr
     EQUW 60000             \ 12 Extra Military Lasers     6000.0 Cr
     EQUW 8000              \ 13 Extra Mining Lasers        800.0 Cr
    
    
    
     Save ELTD.bin
    
    
    
    
     PRINT "ELITE D"
     PRINT "Assembled at ", ~CODE_D%
     PRINT "Ends at ", ~P%
     PRINT "Code size is ", ~(P% - CODE_D%)
     PRINT "Execute at ", ~LOAD%
     PRINT "Reload at ", ~LOAD_D%
    
     PRINT "S.ELTD ", ~CODE_D%, " ", ~P%, " ", ~LOAD%, " ", ~LOAD_D%
    \SAVE "3-assembled-output/ELTD.bin", CODE_D%, P%, LOAD%
    
    
    

[X]

Variable [AVL](workspaces.md#avl) in workspace [WP](workspaces.md#header-wp)

Market availability in the current system

[X]

Configuration variable [Armlas](workspaces.md#armlas)

Military laser power

[X]

Subroutine [BAD](elite_f.md#header-bad) (category: Status)

Calculate how bad we have been

[X]

Subroutine [BAY](elite_f.md#header-bay) (category: Status)

Go to the docking bay (i.e. show the Status Mode screen)

[X]

Entry point [BAY2](elite_d.md#bay2) in subroutine [TT219](elite_d.md#header-tt219) (category: Market)

Jump into the main loop at FRCE, setting the key "pressed" to red key f9 (so we show the Inventory screen)

[X]

Subroutine [BEEP](elite_a.md#header-beep) (category: Sound)

Make a short, high beep

[X]

Variable [BOMB](workspaces.md#bomb) in workspace [WP](workspaces.md#header-wp)

Energy bomb

[X]

Variable [BST](workspaces.md#bst) in workspace [WP](workspaces.md#header-wp)

Fuel scoops (BST stands for "barrel status")

[X]

Variable [CASH](workspaces.md#cash) in workspace [WP](workspaces.md#header-wp)

Our current cash pot

[X]

Subroutine [CIRCLE2](elite_e.md#header-circle2) (category: Drawing circles)

Draw a circle (for the planet or chart)

[X]

Subroutine [CLYNS](elite_a.md#header-clyns) (category: Drawing the screen)

Clear the bottom three text rows of the space view

[X]

Variable [COK](workspaces.md#cok) in workspace [WP](workspaces.md#header-wp)

Flags used to generate the competition code

[X]

Variable [COL](workspaces.md#col) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Variable [CRGO](workspaces.md#crgo) in workspace [WP](workspaces.md#header-wp)

Our ship's cargo capacity

[X]

Subroutine [CTRL](elite_h.md#header-ctrl) (category: Keyboard)

Scan the keyboard to see if CTRL is currently pressed

[X]

Subroutine [CTRLmc](elite_a.md#header-ctrlmc) (category: Keyboard)

Scan the Master Compact keyboard to see if CTRL is currently pressed

[X]

Configuration variable [CYAN](workspaces.md#cyan)

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Entry point [DASC](elite_b.md#dasc) in subroutine [TT26](elite_b.md#header-tt26) (category: Text)

DASC does exactly the same as TT26 and prints a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [DELAY](elite_a.md#header-delay) (category: Utility routines)

Wait for a specified time, in 1/50s of a second

[X]

Variable [DELTA](workspaces.md#delta) in workspace [ZP](workspaces.md#header-zp)

Our current speed, in the range 1-40

[X]

Subroutine [DETOK](elite_a.md#header-detok) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Variable [DKCMP](workspaces.md#dkcmp) in workspace [WP](workspaces.md#header-wp)

Docking computer

[X]

Label [DOANS](elite_d.md#doans) in subroutine [TT210](elite_d.md#header-tt210)

[X]

Subroutine [DORND](elite_f.md#header-dornd) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [DOVDU19](elite_a.md#header-dovdu19) (category: Drawing the screen)

Change the mode 1 palette

[X]

Subroutine [DOXC](elite_d.md#header-doxc) (category: Text)

Move the text cursor to a specific column

[X]

Subroutine [DOYC](elite_d.md#header-doyc) (category: Text)

Move the text cursor to a specific row

[X]

Variable [ECM](workspaces.md#ecm) in workspace [WP](workspaces.md#header-wp)

E.C.M. system

[X]

Label [EE3](elite_d.md#ee3) in subroutine [TT23](elite_d.md#header-tt23)

[X]

Label [EE4](elite_d.md#ee4) in subroutine [TT23](elite_d.md#header-tt23)

[X]

Variable [ENGY](workspaces.md#engy) in workspace [WP](workspaces.md#header-wp)

Energy unit

[X]

Label [EQL1](elite_d.md#eql1) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Subroutine [EQSHP](elite_d.md#header-eqshp) (category: Equipment)

Show the Equip Ship screen (red key f3)

[X]

Variable [ESCP](workspaces.md#escp) in workspace [WP](workspaces.md#header-wp)

Escape pod

[X]

Variable [EV](workspaces.md#ev) in workspace [WP](workspaces.md#header-wp)

The "extra vessels" spawning counter

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

Label [G1](elite_d.md#g1) in subroutine [Ghy](elite_d.md#header-ghy)

[X]

Subroutine [GC2](elite_d.md#header-gc2) (category: Maths (Arithmetic))

Calculate (Y X) = (A P) * 4

[X]

Subroutine [GCASH](elite_d.md#header-gcash) (category: Maths (Arithmetic))

Calculate (Y X) = P * Q * 4

[X]

Variable [GCNT](workspaces.md#gcnt) in workspace [WP](workspaces.md#header-wp)

The number of the current galaxy (0-7)

[X]

Configuration variable [GCYB](workspaces.md#gcyb)

The y-coordinate of the bottom of the Long-range chart

[X]

Configuration variable [GCYT](workspaces.md#gcyt)

The y-coordinate of the top of the Long-range Chart

[X]

Variable [GHYP](workspaces.md#ghyp) in workspace [WP](workspaces.md#header-wp)

Galactic hyperdrive

[X]

Configuration variable [GREEN](workspaces.md#green)

Four mode 1 pixels of colour 3, 1, 3, 1 (cyan/yellow)

[X]

Subroutine [GTHG](elite_d.md#header-gthg) (category: Universe)

Spawn a Thargoid ship and a Thargon companion

[X]

Subroutine [Ghy](elite_d.md#header-ghy) (category: Flight)

Perform a galactic hyperspace jump

[X]

Entry point [HFS1](elite_c.md#hfs1) in subroutine [HFS2](elite_c.md#header-hfs2) (category: Drawing circles)

Don't clear the screen, and draw 8 concentric rings with the step size in STP

[X]

Entry point [HLOIN3](elite_a.md#hloin3) in subroutine [HLOIN](elite_a.md#header-hloin) (category: Drawing lines)

Draw a line from (X, Y1) to (X2, Y1) in the colour given in A

[X]

Variable [INWK](workspaces.md#inwk) in workspace [ZP](workspaces.md#header-zp)

The zero-page internal workspace for the current ship data block

[X]

Variable [K](workspaces.md#k) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K3](workspaces.md#k3) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K4](workspaces.md#k4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [LASER](workspaces.md#laser) in workspace [WP](workspaces.md#header-wp)

The specifications of the lasers fitted to each of the four space views

[X]

Subroutine [LAUN](elite_c.md#header-laun) (category: Drawing circles)

Make the launch sound and draw the launch tunnel

[X]

Subroutine [LCASH](elite_d.md#header-lcash) (category: Maths (Arithmetic))

Subtract an amount of cash from the cash pot

[X]

Subroutine [LL164](elite_c.md#header-ll164) (category: Drawing circles)

Make the hyperspace sound and draw the hyperspace tunnel

[X]

Subroutine [LL5](elite_g.md#header-ll5) (category: Maths (Arithmetic))

Calculate Q = SQRT(R Q)

[X]

Subroutine [LOIN](elite_a.md#header-loin) (category: Drawing lines)

Draw a one-segment line

[X]

Subroutine [LOOK1](elite_h.md#header-look1) (category: Flight)

Initialise the space view

[X]

Variable [LSP](workspaces.md#lsp) in workspace [ZP](workspaces.md#header-zp)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Configuration variable [MAGENTA](workspaces.md#magenta)

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Variable [MANY](workspaces.md#many) in workspace [WP](workspaces.md#header-wp)

The number of ships of each type in the local bubble of universe

[X]

Subroutine [MCASH](elite_d.md#header-mcash) (category: Maths (Arithmetic))

Add an amount of cash to the cash pot

[X]

Subroutine [MESS](elite_f.md#header-mess) (category: Flight)

Display an in-flight message

[X]

Variable [MJ](workspaces.md#mj) in workspace [WP](workspaces.md#header-wp)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Subroutine [MJP](elite_d.md#header-mjp) (category: Flight)

Process a mis-jump into witchspace

[X]

Label [MJP1](elite_d.md#mjp1) in subroutine [MJP](elite_d.md#header-mjp)

[X]

Subroutine [MULTU](elite_c.md#header-multu) (category: Maths (Arithmetic))

Calculate (A P) = P * Q

[X]

Configuration variable [Mlas](workspaces.md#mlas)

Mining laser power

[X]

Subroutine [NLIN](elite_b.md#header-nlin) (category: Drawing lines)

Draw a horizontal line at pixel row 23 to box in a title

[X]

Subroutine [NLIN3](elite_b.md#header-nlin3) (category: Drawing lines)

Print a title and draw a horizontal line at row 19 to box it in

[X]

Subroutine [NLIN4](elite_b.md#header-nlin4) (category: Drawing lines)

Draw a horizontal line at pixel row 19 to box in a title

[X]

Entry point [NLIN5](elite_b.md#nlin5) in subroutine [NLIN](elite_b.md#header-nlin) (category: Drawing lines)

Move the text cursor down one line before drawing the line

[X]

Label [NLUNCH](elite_d.md#nlunch) in subroutine [TT110](elite_d.md#header-tt110)

[X]

Variable [NOMSL](workspaces.md#nomsl) in workspace [WP](workspaces.md#header-wp)

The number of missiles we have fitted (0-4)

[X]

Variable [NOSTM](workspaces.md#nostm) in workspace [ZP](workspaces.md#header-zp)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Label [NWDAV1](elite_d.md#nwdav1) in subroutine [gnum](elite_d.md#header-gnum)

[X]

Label [NWDAV2](elite_d.md#nwdav2) in subroutine [gnum](elite_d.md#header-gnum)

[X]

Label [NWDAV3](elite_d.md#nwdav3) in subroutine [gnum](elite_d.md#header-gnum)

[X]

Subroutine [NWDAV4](elite_d.md#header-nwdav4) (category: Market)

Print an "ITEM?" error, make a beep and rejoin the TT210 routine

[X]

Entry point [NWDAVxx](elite_d.md#nwdavxx) in subroutine [TT210](elite_d.md#header-tt210) (category: Market)

Used to rejoin this routine from the call to NWDAV4

[X]

Subroutine [NWSHP](elite_e.md#header-nwshp) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Subroutine [NWSPS](elite_e.md#header-nwsps) (category: Universe)

Add a new space station to our local bubble of universe

[X]

Entry point [OUT](elite_d.md#out) in subroutine [gnum](elite_d.md#header-gnum) (category: Market)

The OUTK routine jumps back here after printing the key that was just pressed

[X]

Subroutine [OUTK](elite_d.md#header-outk) (category: Text)

Print the character in Q before returning to gnum

[X]

Variable [P](workspaces.md#p) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [PATG](elite_a.md#patg) in workspace [UP](elite_a.md#header-up)

Configuration setting to show the author names on the start-up screen and enable manual hyperspace mis-jumps

[X]

Subroutine [PDESC](elite_c.md#header-pdesc) (category: Universe)

Print the system's extended description or a mission 1 directive

[X]

Subroutine [PIXEL](elite_a.md#header-pixel) (category: Drawing pixels)

Draw a one-pixel dot, two-pixel dash or four-pixel square

[X]

Configuration variable [POW](workspaces.md#pow)

Pulse laser power

[X]

Variable [PRXS](elite_d.md#header-prxs) (category: Equipment)

Equipment prices

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

Variable [QQ10](workspaces.md#qq10) in workspace [ZP](workspaces.md#header-zp)

The galactic y-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic y-coordinate)

[X]

Variable [QQ11](workspaces.md#qq11) in workspace [ZP](workspaces.md#header-zp)

The type of the current view

[X]

Variable [QQ12](workspaces.md#qq12) in workspace [ZP](workspaces.md#header-zp)

Our "docked" status

[X]

Variable [QQ14](workspaces.md#qq14) in workspace [WP](workspaces.md#header-wp)

Our current fuel level (0-70)

[X]

Variable [QQ15](workspaces.md#qq15) in workspace [ZP](workspaces.md#header-zp)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ17](workspaces.md#qq17) in workspace [ZP](workspaces.md#header-zp)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Variable [QQ19](workspaces.md#qq19) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [QQ2](workspaces.md#qq2) in workspace [WP](workspaces.md#header-wp)

The three 16-bit seeds for the current system, i.e. the one we are currently in

[X]

Variable [QQ20](workspaces.md#qq20) in workspace [WP](workspaces.md#header-wp)

The contents of our cargo hold

[X]

Variable [QQ21](workspaces.md#qq21) in workspace [WP](workspaces.md#header-wp)

The three 16-bit seeds for the current galaxy

[X]

Variable [QQ22](workspaces.md#qq22) in workspace [ZP](workspaces.md#header-zp)

The two hyperspace countdown counters

[X]

Variable [QQ23](elite_f.md#header-qq23) (category: Market)

Market prices table

[X]

Variable [QQ24](workspaces.md#qq24) in workspace [WP](workspaces.md#header-wp)

Temporary storage, used to store the current market item's price in routine TT151

[X]

Variable [QQ25](workspaces.md#qq25) in workspace [WP](workspaces.md#header-wp)

Temporary storage, used to store the current market item's availability in routine TT151

[X]

Variable [QQ26](workspaces.md#qq26) in workspace [WP](workspaces.md#header-wp)

A random value used to randomise market data

[X]

Variable [QQ28](workspaces.md#qq28) in workspace [WP](workspaces.md#header-wp)

The current system's economy (0-7)

[X]

Variable [QQ29](workspaces.md#qq29) in workspace [WP](workspaces.md#header-wp)

Temporary storage, used in a number of places

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

Variable [QQ6](workspaces.md#qq6) in workspace [ZP](workspaces.md#header-zp)

The selected system's population in billions * 10 (1-71), so the maximum population is 7.1 billion

[X]

Variable [QQ7](workspaces.md#qq7) in workspace [ZP](workspaces.md#header-zp)

The selected system's productivity in M CR (96-62480)

[X]

Variable [QQ8](workspaces.md#qq8) in workspace [ZP](workspaces.md#header-zp)

The distance from the current system to the selected system in light years * 10, stored as a 16-bit number

[X]

Variable [QQ9](workspaces.md#qq9) in workspace [ZP](workspaces.md#header-zp)

The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)

[X]

Variable [R](workspaces.md#r) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [RED](workspaces.md#red)

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Subroutine [RES2](elite_f.md#header-res2) (category: Start and end)

Reset a number of flight variables and workspaces

[X]

Entry point [RTS111](elite_d.md#rts111) in subroutine [MJP](elite_d.md#header-mjp) (category: Flight)

Contains an RTS

[X]

Variable [S](workspaces.md#s) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [SCALEX](elite_d.md#header-scalex) (category: Maths (Geometry))

Scale the x-coordinate in A (leave it unchanged)

[X]

Subroutine [SCALEY](elite_d.md#header-scaley) (category: Maths (Geometry))

Scale the y-coordinate in A to 0.5 * A

[X]

Subroutine [SCALEY2](elite_d.md#header-scaley2) (category: Maths (Geometry))

Scale the y-coordinate in A (leave it unchanged)

[X]

Subroutine [SOLAR](elite_e.md#header-solar) (category: Universe)

Set up various aspects of arriving in a new system

[X]

Subroutine [SOS1](elite_e.md#header-sos1) (category: Universe)

Update the missile indicators, set up the planet data block

[X]

Subroutine [SQUA2](elite_c.md#header-squa2) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[X]

Variable [STP](workspaces.md#stp) in workspace [ZP](workspaces.md#header-zp)

The step size for drawing circles

[X]

Subroutine [SUN (Part 1 of 4)](elite_e.md#header-sun-part-1-of-4) (category: Drawing suns)

Draw the sun: Set up all the variables needed to draw the sun

[X]

Variable [T](workspaces.md#t) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [T1](workspaces.md#t1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Configuration variable [TGL](workspaces.md#tgl)

Ship type for a Thargon

[X]

Configuration variable [THG](workspaces.md#thg)

Ship type for a Thargoid

[X]

Label [TQ4](elite_d.md#tq4) in subroutine [TT219](elite_d.md#header-tt219)

[X]

Subroutine [TRADEMODE](elite_d.md#header-trademode) (category: Drawing the screen)

Clear the screen and set up a trading screen

[X]

Variable [TRIBBLE](workspaces.md#tribble) in workspace [WP](workspaces.md#header-wp)

The number of Trumbles in the cargo hold

[X]

Subroutine [TT103](elite_d.md#header-tt103) (category: Charts)

Draw a small set of crosshairs on a chart

[X]

Subroutine [TT105](elite_d.md#header-tt105) (category: Charts)

Draw crosshairs on the Short-range Chart, with clipping

[X]

Subroutine [TT11](elite_b.md#header-tt11) (category: Text)

Print a 16-bit number, left-padded to n digits, and optional point

[X]

Subroutine [TT110](elite_d.md#header-tt110) (category: Flight)

Launch from a station or show the front space view

[X]

Subroutine [TT111](elite_d.md#header-tt111) (category: Universe)

Set the current system to the nearest system to a point

[X]

Label [TT112](elite_d.md#tt112) in subroutine [hyp1](elite_d.md#header-hyp1)

[X]

Entry point [TT113](elite_d.md#tt113) in subroutine [MCASH](elite_d.md#header-mcash) (category: Maths (Arithmetic))

Contains an RTS

[X]

Subroutine [TT114](elite_d.md#header-tt114) (category: Charts)

Display either the Long-range or Short-range Chart

[X]

Label [TT115](elite_d.md#tt115) in subroutine [TT114](elite_d.md#header-tt114)

[X]

Subroutine [TT123](elite_d.md#header-tt123) (category: Charts)

Move galactic coordinates by a signed delta

[X]

Label [TT124](elite_d.md#tt124) in subroutine [TT123](elite_d.md#header-tt123)

[X]

Label [TT125](elite_d.md#tt125) in subroutine [TT123](elite_d.md#header-tt123)

[X]

Label [TT126](elite_d.md#tt126) in subroutine [TT14](elite_d.md#header-tt14)

[X]

Subroutine [TT128](elite_d.md#header-tt128) (category: Drawing circles)

Draw a circle on a chart

[X]

Label [TT130](elite_d.md#tt130) in subroutine [TT111](elite_d.md#header-tt111)

[X]

Label [TT132](elite_d.md#tt132) in subroutine [TT111](elite_d.md#header-tt111)

[X]

Label [TT134](elite_d.md#tt134) in subroutine [TT111](elite_d.md#header-tt111)

[X]

Label [TT135](elite_d.md#tt135) in subroutine [TT111](elite_d.md#header-tt111)

[X]

Label [TT136](elite_d.md#tt136) in subroutine [TT111](elite_d.md#header-tt111)

[X]

Label [TT137](elite_d.md#tt137) in subroutine [TT111](elite_d.md#header-tt111)

[X]

Label [TT139](elite_d.md#tt139) in subroutine [TT111](elite_d.md#header-tt111)

[X]

Subroutine [TT14](elite_d.md#header-tt14) (category: Drawing circles)

Draw a circle with crosshairs on a chart

[X]

Label [TT141](elite_d.md#tt141) in subroutine [TT111](elite_d.md#header-tt111)

[X]

Subroutine [TT146](elite_d.md#header-tt146) (category: Universe)

Print the distance to the selected system in light years

[X]

Subroutine [TT147](elite_d.md#header-tt147) (category: Flight)

Print an error when a system is out of hyperspace range

[X]

Subroutine [TT15](elite_d.md#header-tt15) (category: Drawing lines)

Draw a set of crosshairs

[X]

Subroutine [TT151](elite_d.md#header-tt151) (category: Market)

Print the name, price and availability of a market item

[X]

Label [TT151q](elite_d.md#tt151q) in subroutine [TT151](elite_d.md#header-tt151)

[X]

Subroutine [TT152](elite_d.md#header-tt152) (category: Market)

Print the unit ("t", "kg" or "g") for a market item

[X]

Label [TT153](elite_d.md#tt153) in subroutine [var](elite_d.md#header-var)

[X]

Label [TT154](elite_d.md#tt154) in subroutine [var](elite_d.md#header-var)

[X]

Label [TT155](elite_d.md#tt155) in subroutine [TT151](elite_d.md#header-tt151)

[X]

Label [TT156](elite_d.md#tt156) in subroutine [TT151](elite_d.md#header-tt151)

[X]

Label [TT157](elite_d.md#tt157) in subroutine [GVL](elite_d.md#header-gvl)

[X]

Label [TT158](elite_d.md#tt158) in subroutine [GVL](elite_d.md#header-gvl)

[X]

Label [TT159](elite_d.md#tt159) in subroutine [GVL](elite_d.md#header-gvl)

[X]

Subroutine [TT160](elite_d.md#header-tt160) (category: Market)

Print "t" (for tonne) and a space

[X]

Subroutine [TT161](elite_d.md#header-tt161) (category: Market)

Print "kg" (for kilograms)

[X]

Subroutine [TT162](elite_d.md#header-tt162) (category: Text)

Print a space

[X]

Entry point [TT162+2](elite_d.md#tt162) in subroutine [TT162](elite_d.md#header-tt162) (category: Text)

Jump to TT27 to print the text token in A

[X]

Subroutine [TT163](elite_d.md#header-tt163) (category: Market)

Print the headers for the table of market prices

[X]

Label [TT168](elite_d.md#tt168) in subroutine [TT167](elite_d.md#header-tt167)

[X]

Subroutine [TT16a](elite_d.md#header-tt16a) (category: Market)

Print "g" (for grams)

[X]

Label [TT172](elite_d.md#tt172) in subroutine [TT151](elite_d.md#header-tt151)

[X]

Label [TT178](elite_d.md#tt178) in subroutine [TT15](elite_d.md#header-tt15)

[X]

Label [TT179](elite_d.md#tt179) in subroutine [TT105](elite_d.md#header-tt105)

[X]

Entry point [TT180](elite_d.md#tt180) in subroutine [TT123](elite_d.md#header-tt123) (category: Charts)

Contains an RTS

[X]

Label [TT182](elite_d.md#tt182) in subroutine [TT23](elite_d.md#header-tt23)

[X]

Label [TT184](elite_d.md#tt184) in subroutine [TT23](elite_d.md#header-tt23)

[X]

Label [TT186](elite_d.md#tt186) in subroutine [TT23](elite_d.md#header-tt23)

[X]

Label [TT187](elite_d.md#tt187) in subroutine [TT23](elite_d.md#header-tt23)

[X]

Label [TT187s](elite_d.md#tt187s) in subroutine [TT23](elite_d.md#header-tt23)

[X]

Subroutine [TT20](elite_d.md#header-tt20) (category: Universe)

Twist the selected system's seeds four times

[X]

Label [TT205](elite_d.md#tt205) in subroutine [TT25](elite_d.md#header-tt25)

[X]

Label [TT206](elite_d.md#tt206) in subroutine [TT25](elite_d.md#header-tt25)

[X]

Label [TT207](elite_d.md#tt207) in subroutine [TT25](elite_d.md#header-tt25)

[X]

Subroutine [TT210](elite_d.md#header-tt210) (category: Market)

Show a list of current cargo in our hold, optionally to sell

[X]

Label [TT211](elite_d.md#tt211) in subroutine [TT210](elite_d.md#header-tt210)

[X]

Label [TT212](elite_d.md#tt212) in subroutine [TT210](elite_d.md#header-tt210)

[X]

Subroutine [TT217](elite_f.md#header-tt217) (category: Keyboard)

Scan the keyboard until a key is pressed

[X]

Label [TT218](elite_d.md#tt218) in subroutine [TT214](elite_d.md#header-tt214)

[X]

Subroutine [TT22](elite_d.md#header-tt22) (category: Charts)

Show the Long-range Chart (red key f4)

[X]

Label [TT220](elite_d.md#tt220) in subroutine [TT219](elite_d.md#header-tt219)

[X]

Label [TT222](elite_d.md#tt222) in subroutine [TT219](elite_d.md#header-tt219)

[X]

Label [TT223](elite_d.md#tt223) in subroutine [gnum](elite_d.md#header-gnum)

[X]

Label [TT224](elite_d.md#tt224) in subroutine [TT219](elite_d.md#header-tt219)

[X]

Label [TT226](elite_d.md#tt226) in subroutine [gnum](elite_d.md#header-gnum)

[X]

Subroutine [TT23](elite_d.md#header-tt23) (category: Charts)

Show the Short-range Chart (red key f5)

[X]

Subroutine [TT24](elite_d.md#header-tt24) (category: Universe)

Calculate system data from the system seeds

[X]

Subroutine [TT26](elite_b.md#header-tt26) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [TT27](elite_e.md#header-tt27) (category: Text)

Print a text token

[X]

Subroutine [TT60](elite_d.md#header-tt60) (category: Text)

Print a text token and a paragraph break

[X]

Label [TT63](elite_d.md#tt63) in subroutine [TT146](elite_d.md#header-tt146)

[X]

Subroutine [TT66](elite_h.md#header-tt66) (category: Drawing the screen)

Clear the screen and set the current view type

[X]

Subroutine [TT67](elite_d.md#header-tt67) (category: Text)

Print a newline

[X]

Subroutine [TT68](elite_e.md#header-tt68) (category: Text)

Print a text token followed by a colon

[X]

Subroutine [TT69](elite_d.md#header-tt69) (category: Text)

Set Sentence Case and print a newline

[X]

Subroutine [TT70](elite_d.md#header-tt70) (category: Universe)

Display "MAINLY " and jump to TT72

[X]

Label [TT71](elite_d.md#tt71) in subroutine [TT25](elite_d.md#header-tt25)

[X]

Entry point [TT72](elite_d.md#tt72) in subroutine [TT25](elite_d.md#header-tt25) (category: Universe)

Used by TT70 to re-enter the routine after displaying "MAINLY" for the economy type

[X]

Label [TT75](elite_d.md#tt75) in subroutine [TT25](elite_d.md#header-tt25)

[X]

Label [TT76](elite_d.md#tt76) in subroutine [TT25](elite_d.md#header-tt25)

[X]

Label [TT77](elite_d.md#tt77) in subroutine [TT24](elite_d.md#header-tt24)

[X]

Subroutine [TT81](elite_d.md#header-tt81) (category: Universe)

Set the selected system's seeds to those of system 0

[X]

[X]

Label [TT83](elite_d.md#tt83) in subroutine [TT22](elite_d.md#header-tt22)

[X]

Label [TT84](elite_d.md#tt84) in subroutine [TT15](elite_d.md#header-tt15)

[X]

Label [TT85](elite_d.md#tt85) in subroutine [TT15](elite_d.md#header-tt15)

[X]

Label [TT86](elite_d.md#tt86) in subroutine [TT15](elite_d.md#header-tt15)

[X]

Label [TT87](elite_d.md#tt87) in subroutine [TT15](elite_d.md#header-tt15)

[X]

Subroutine [TTX110](elite_d.md#header-ttx110) (category: Flight)

Set the current system to the nearest system and return to hyp

[X]

Entry point [TTX111](elite_d.md#ttx111) in subroutine [hyp](elite_d.md#header-hyp) (category: Flight)

Used to rejoin this routine from the call to TTX110

[X]

Subroutine [TTX66](elite_a.md#header-ttx66) (category: Drawing the screen)

Clear the top part of the screen and draw a border box

[X]

Subroutine [TTX69](elite_d.md#header-ttx69) (category: Text)

Print a paragraph break

[X]

Label [Tc](elite_d.md#tc) in subroutine [TT219](elite_d.md#header-tt219)

[X]

Label [Tml](elite_d.md#tml) in subroutine [tnpr](elite_d.md#header-tnpr)

[X]

Variable [U](workspaces.md#u) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [WSCAN](elite_a.md#header-wscan) (category: Drawing the screen)

Implement the #wscn command (wait for the vertical sync)

[X]

Variable [X1](workspaces.md#x1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](workspaces.md#x2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XC](workspaces.md#xc) in workspace [ZP](workspaces.md#header-zp)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [XSAV](workspaces.md#xsav) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX12](workspaces.md#xx12) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX13](workspaces.md#xx13) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used in the line-drawing routines

[X]

Variable [XX20](workspaces.md#xx20) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [XX4](workspaces.md#xx4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [Y1](workspaces.md#y1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](workspaces.md#y2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [YC](workspaces.md#yc) in workspace [ZP](workspaces.md#header-zp)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Configuration variable [YELLOW](workspaces.md#yellow)

Four mode 1 pixels of colour 1 (yellow)

[X]

Variable [ZZ](workspaces.md#zz) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for distance values

[X]

Subroutine [Ze](elite_f.md#header-ze) (category: Universe)

Initialise the INWK workspace to a fairly aggressive ship

[X]

Label [bay](elite_d.md#bay) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [botchfix12](elite_d.md#botchfix12) in subroutine [TT15](elite_d.md#header-tt15)

[X]

Label [botchfix13](elite_d.md#botchfix13) in subroutine [TT15](elite_d.md#header-tt15)

[X]

Entry point [c](elite_d.md#c) in subroutine [prx](elite_d.md#header-prx) (category: Equipment)

Contains an RTS

[X]

Subroutine [cpl](elite_e.md#header-cpl) (category: Universe)

Print the selected system name

[X]

Subroutine [dn](elite_d.md#header-dn) (category: Market)

Print the amount of money we have left in the cash pot, then make a short, high beep and delay for 1 second

[X]

Subroutine [dn2](elite_d.md#header-dn2) (category: Text)

Make a short, high beep and delay for 0.5 seconds

[X]

Subroutine [dockEd](elite_d.md#header-docked) (category: Flight)

Print a message to say there is no hyperspacing allowed inside the station

[X]

Label [dumdeedum](elite_d.md#dumdeedum) in subroutine [Ghy](elite_d.md#header-ghy)

[X]

Label [ed9](elite_d.md#ed9) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [ee1](elite_d.md#ee1) in subroutine [TT23](elite_d.md#header-tt23)

[X]

Subroutine [ee3](elite_d.md#header-ee3) (category: Flight)

Print the hyperspace countdown in the top-left of the screen

[X]

Label [ee5](elite_d.md#ee5) in subroutine [TT18](elite_d.md#header-tt18)

[X]

Subroutine [eq](elite_d.md#header-eq) (category: Equipment)

Subtract the price of equipment from the cash pot

[X]

Entry point [err](elite_d.md#err) in subroutine [EQSHP](elite_d.md#header-eqshp) (category: Equipment)

Beep, pause and go to the docking bay (i.e. show the Status Mode screen)

[X]

Label [et0](elite_d.md#et0) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et1](elite_d.md#et1) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et10](elite_d.md#et10) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et11](elite_d.md#et11) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et2](elite_d.md#et2) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et3](elite_d.md#et3) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et4](elite_d.md#et4) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et5](elite_d.md#et5) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et6](elite_d.md#et6) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et7](elite_d.md#et7) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et8](elite_d.md#et8) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [et9](elite_d.md#et9) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [etA](elite_d.md#eta) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Label [etB](elite_d.md#etb) in subroutine [EQSHP](elite_d.md#header-eqshp)

[X]

Configuration variable [f9](workspaces.md#f9)

Internal key number for red key f9 (Inventory)

[X]

Subroutine [fwl](elite_e.md#header-fwl) (category: Status)

Print fuel and cash levels

[X]

Subroutine [gnum](elite_d.md#header-gnum) (category: Market)

Get a number from the keyboard

[X]

Label [goTT147](elite_d.md#gott147) in subroutine [hyp](elite_d.md#header-hyp)

[X]

Variable [gov](workspaces.md#gov) in workspace [WP](workspaces.md#header-wp)

The current system's government type (0-7)

[X]

Subroutine [hm](elite_d.md#header-hm) (category: Charts)

Select the closest system and redraw the chart crosshairs

[X]

Label [hy9](elite_d.md#hy9) in subroutine [GVL](elite_d.md#header-gvl)

[X]

Entry point [hyp1+3](elite_d.md#hyp1) in subroutine [hyp1](elite_d.md#header-hyp1) (category: Universe)

Jump straight to the system at (QQ9, QQ10) without first calculating which system is closest. We do this if we already know that (QQ9, QQ10) points to a system

[X]

Subroutine [jmp](elite_d.md#header-jmp) (category: Universe)

Set the current system to the selected system

[X]

Label [kg](elite_d.md#kg) in subroutine [tnpr](elite_d.md#header-tnpr)

[X]

Subroutine [msblob](elite_f.md#header-msblob) (category: Dashboard)

Display the dashboard's missile indicators in green

[X]

Subroutine [pr2](elite_b.md#header-pr2) (category: Text)

Print an 8-bit number, left-padded to 3 digits, and optional point

[X]

Entry point [pr2+2](elite_b.md#pr2) in subroutine [pr2](elite_b.md#header-pr2) (category: Text)

Print the 8-bit number in X to the number of digits in A

[X]

Subroutine [pr5](elite_d.md#header-pr5) (category: Text)

Print a 16-bit number, left-padded to 5 digits, and optional point

[X]

Subroutine [pr6](elite_d.md#header-pr6) (category: Text)

Print 16-bit number, left-padded to 5 digits, no point

[X]

Entry point [pres](elite_d.md#pres) in subroutine [EQSHP](elite_d.md#header-eqshp) (category: Equipment)

Given an item number A with the item name in recursive token Y, show an error to say that the item is already present, refund the cost of the item, and then beep and exit to the docking bay (i.e. show the Status Mode screen)

[X]

Subroutine [prq](elite_d.md#header-prq) (category: Text)

Print a text token followed by a question mark

[X]

Subroutine [prx](elite_d.md#header-prx) (category: Equipment)

Return the price of a piece of equipment

[X]

Entry point [prx-3](elite_d.md#prx) in subroutine [prx](elite_d.md#header-prx) (category: Equipment)

Return the price of the item with number A - 1

[X]

Entry point [ptg](elite_d.md#ptg) in subroutine [MJP](elite_d.md#header-mjp) (category: Flight)

Called when the user manually forces a mis-jump

[X]

Subroutine [qv](elite_d.md#header-qv) (category: Equipment)

Print a menu of the four space views, for buying lasers

[X]

Label [qv1](elite_d.md#qv1) in subroutine [qv](elite_d.md#header-qv)

[X]

Label [qv2](elite_d.md#qv2) in subroutine [qv](elite_d.md#header-qv)

[X]

Label [qv3](elite_d.md#qv3) in subroutine [qv](elite_d.md#header-qv)

[X]

Entry point [readdistnce](elite_d.md#readdistnce) in subroutine [TT111](elite_d.md#header-tt111) (category: Universe)

Calculate the distance between the system with galactic coordinates (A, QQ15+1) and the system at (QQ0, QQ1), returning the result in QQ8(1 0)

[X]

Label [ref1](elite_d.md#ref1) in subroutine [refund](elite_d.md#header-refund)

[X]

Label [ref3](elite_d.md#ref3) in subroutine [refund](elite_d.md#header-refund)

[X]

Subroutine [refund](elite_d.md#header-refund) (category: Equipment)

Install a new laser, processing a refund if applicable

[X]

Variable [safehouse](workspaces.md#safehouse) in workspace [WP](workspaces.md#header-wp)

Backup storage for the seeds for the selected system

[X]

Label [sob](elite_d.md#sob) in subroutine [hyp](elite_d.md#header-hyp)

[X]

Subroutine [spc](elite_d.md#header-spc) (category: Text)

Print a text token followed by a space

[X]

Variable [tek](workspaces.md#tek) in workspace [WP](workspaces.md#header-wp)

The current system's tech level (0-14)

[X]

Subroutine [tnpr](elite_d.md#header-tnpr) (category: Market)

Work out if we have space for a specific amount of cargo

[X]

Subroutine [var](elite_d.md#header-var) (category: Market)

Calculate QQ19+3 = economy * |economic_factor|

[X]

Entry point [wW2](elite_d.md#ww2) in subroutine [wW](elite_d.md#header-ww) (category: Flight)

Start the hyperspace countdown, starting the countdown from the value in A

[X]

Entry point [zZ+1](elite_d.md#zz) in subroutine [Ghy](elite_d.md#header-ghy) (category: Flight)

Contains an RTS

[X]

Label [zebra](elite_d.md#zebra) in subroutine [TT210](elite_d.md#header-tt210)

[Elite C source](elite_c.md "Previous routine")[Elite E source](elite_e.md "Next routine")
