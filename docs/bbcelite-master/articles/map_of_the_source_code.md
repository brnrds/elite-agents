---
title: "Map of the source code in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/articles/map_of_the_source_code.html"
---

[Different variants of the BBC Master version](../releases.md "Previous routine")[Loader source](../all/loader.md "Next routine")

This page contains a map of all the routines, variables and macros in the BBC Master version of Elite. The source files are structured like this:

  * The Loader, which displays the loading screen, implements the copy protection and sets things up for the main game
  * The main game source, which consists of Workspaces, Elite A, Elite B, Elite C, Elite D, Elite E, Elite F, Elite G and Elite H
  * The Game data source, which contains game images, ship blueprints and text tokens



You can click on the links above to jump to the relevant part of the map.

## Loader  
\------

Category| Details  
---|---  
Workspaces| [Workspace: ZP (Loader)](../loader/workspace/zp.md)Important variables used by the loader  
Drawing the screen| [Variable: B% (Loader)](../loader/variable/b_per_cent.md)VDU commands for setting the square mode 1 screen  
Loader| [Subroutine: Elite loader (Loader)](../loader/subroutine/elite_loader.md)Perform a number of OS calls, check for sideways RAM, load and move the main game data, and load and run the main game code  
Drawing planets| [Subroutine: PLL1 (Part 1 of 3) (Loader)](../loader/subroutine/pll1_part_1_of_3.md)Draw Saturn on the loading screen (draw the planet)  
Drawing planets| [Subroutine: PLL1 (Part 2 of 3) (Loader)](../loader/subroutine/pll1_part_2_of_3.md)Draw Saturn on the loading screen (draw the stars)  
Drawing planets| [Subroutine: PLL1 (Part 3 of 3) (Loader)](../loader/subroutine/pll1_part_3_of_3.md)Draw Saturn on the loading screen (draw the rings)  
Maths (Arithmetic)| [Subroutine: DORND (Loader)](../loader/subroutine/dornd.md)Generate random numbers  
Drawing planets| [Variable: RAND (Loader)](../loader/variable/rand.md)The random number seed used for drawing Saturn  
Maths (Arithmetic)| [Subroutine: SQUA2 (Loader)](../loader/subroutine/squa2.md)Calculate (A P) = A * A  
Drawing pixels| [Subroutine: PIX (Loader)](../loader/subroutine/pix.md)Draw a single pixel at a specific coordinate  
Drawing pixels| [Variable: TWOS (Loader)](../loader/variable/twos.md)Ready-made single-pixel character row bytes for mode 1  
Drawing planets| [Variable: CNT (Loader)](../loader/variable/cnt.md)A counter for use in drawing Saturn's planetary body  
Drawing planets| [Variable: CNT2 (Loader)](../loader/variable/cnt2.md)A counter for use in drawing Saturn's background stars  
Drawing planets| [Variable: CNT3 (Loader)](../loader/variable/cnt3.md)A counter for use in drawing Saturn's rings  
Maths (Arithmetic)| [Subroutine: ROOT (Loader)](../loader/subroutine/root.md)Calculate ZP = SQRT(ZP(1 0))  
Utility routines| [Subroutine: OSB (Loader)](../loader/subroutine/osb.md)A convenience routine for calling OSBYTE with Y = 0  
Loader| [Variable: MESS1 (Loader)](../loader/variable/mess1.md)The OS command string for loading the BDATA binary  
Loader| [Variable: MESS2 (Loader)](../loader/variable/mess2.md)The OS command string for loading the main game code binary  
Loader| [Variable: MESS3 (Loader)](../loader/variable/mess3.md)The OS command string for changing the disc directory to E  
  
## Workspaces  
\----------

Category| Details  
---|---  
Workspaces| [Workspace: ZP](../main/workspace/zp.md)Lots of important variables are stored in the zero page workspace as it is quicker and more space-efficient to access memory here  
Workspaces| [Workspace: XX3](../main/workspace/xx3.md)Temporary storage space for complex calculations  
Workspaces| [Workspace: K%](../main/workspace/k_per_cent.md)Ship data blocks and ship line heaps  
Workspaces| [Workspace: WP](../main/workspace/wp.md)Ship slots, variables  
  
## Elite A  
\-------

Category| Details  
---|---  
Drawing the screen| [Variable: TVT3](../main/variable/tvt3.md)Palette data for the mode 1 part of the screen (the top part)  
Drawing the screen| [Variable: VEC](../main/variable/vec.md)The original value of the IRQ1 vector  
Drawing the screen| [Subroutine: WSCAN](../main/subroutine/wscan.md)Implement the #wscn command (wait for the vertical sync)  
Utility routines| [Subroutine: DELAY](../main/subroutine/delay.md)Wait for a specified time, in 1/50s of a second  
Sound| [Subroutine: BOOP](../main/subroutine/boop.md)Make a long, low beep  
Sound| [Subroutine: BEEP](../main/subroutine/beep.md)Make a short, high beep  
Sound| [Variable: SOFH](../main/variable/sofh.md)Sound chip data mask for choosing a tone channel in the range 0-2  
Sound| [Variable: SOOFF](../main/variable/sooff.md)Sound chip data to turn the volume down on all channels and to act as a mask for choosing a tone channel in the range 0-2  
Sound| [Subroutine: SOUS1](../main/subroutine/sous1.md)Write sound data directly to the 76489 sound chip  
Sound| [Subroutine: SOFLUSH](../main/subroutine/soflush.md)Reset the sound buffers and turn off all sound channels  
Sound| [Subroutine: ELASNO](../main/subroutine/elasno.md)Make the sound of us being hit  
Sound| [Subroutine: LASNO](../main/subroutine/lasno.md)Make the sound of our laser firing  
Sound| [Subroutine: NOISE](../main/subroutine/noise.md)Make the sound whose number is in Y by populating the sound buffer  
Sound| [Subroutine: SOINT](../main/subroutine/soint.md)Process the contents of the sound buffer and send it to the sound chip  
Sound| [Workspace: Sound variables](../main/workspace/sound_variables.md)The sound buffer where the data to be sent to the sound chip is processed  
Sound| [Variable: SFXPR](../main/variable/sfxpr.md)Sound data block 1  
Sound| [Variable: SFXBT](../main/variable/sfxbt.md)Sound data block 2  
Sound| [Variable: SFXFQ](../main/variable/sfxfq.md)Sound data block 3  
Sound| [Variable: SFXVC](../main/variable/sfxvc.md)Sound data block 4  
Loader| [Subroutine: SETINTS](../main/subroutine/setints.md)Set the various vectors, interrupts and timers  
Drawing the screen| [Variable: TVT1](../main/variable/tvt1.md)Palette data for the mode 2 part of the screen (the dashboard)  
Drawing the screen| [Subroutine: IRQ1](../main/subroutine/irq1.md)The main screen-mode interrupt handler (IRQ1V points here)  
Drawing the screen| [Variable: VSCAN](../main/variable/vscan.md)Defines the split position in the split-screen mode  
Drawing the screen| [Subroutine: DOVDU19](../main/subroutine/dovdu19.md)Change the mode 1 palette  
Utility routines| [Subroutine: setzp](../main/subroutine/setzp.md)Copy the top part of zero page (&0090 to &00FF) into the buffer at &3000  
Utility routines| [Subroutine: NMIRELEASE](../main/subroutine/nmirelease.md)Release the NMI workspace (&00A0 to &00A7) so the MOS can use it and store the top part of zero page in the buffer at &3000  
Utility routines| [Subroutine: getzp](../main/subroutine/getzp.md)Swap zero page (&0090 to &00EF) with the buffer at &3000  
Utility routines| [Subroutine: NMICLAIM](../main/subroutine/nmiclaim.md)Claim the NMI workspace (&00A0 to &00A7) back from the MOS so the game can use it once again  
Drawing pixels| [Variable: ylookup](../main/variable/ylookup.md)Lookup table for converting pixel y-coordinate to page number of screen address  
Keyboard| [Subroutine: SHIFT](../main/subroutine/shift.md)Scan the keyboard to see if SHIFT is currently pressed  
Keyboard| [Subroutine: RETURN](../main/subroutine/return.md)Scan the keyboard to see if RETURN is currently pressed  
Keyboard| [Subroutine: CTRLmc](../main/subroutine/ctrlmc.md)Scan the Master Compact keyboard to see if CTRL is currently pressed  
Keyboard| [Subroutine: DKS4mc](../main/subroutine/dks4mc.md)Scan the Master Compact keyboard to see if a specific key is being pressed  
Dashboard| [Subroutine: SCAN](../main/subroutine/scan.md)Display the current ship on the scanner  
Drawing lines| [Subroutine: LOIN](../main/subroutine/loin.md)Draw a one-segment line  
Drawing pixels| [Variable: TWOS](../main/variable/twos.md)Ready-made single-pixel character row bytes for mode 1  
Drawing pixels| [Variable: TWOS2](../main/variable/twos2.md)Ready-made double-pixel character row bytes for mode 1  
Drawing pixels| [Variable: CTWOS](../main/variable/ctwos.md)Ready-made single-pixel character row bytes for mode 2  
Drawing lines| [Subroutine: LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)Draw a line: Calculate the line gradient in the form of deltas  
Drawing lines| [Subroutine: LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)Draw a line: Line has a shallow gradient, step right along x-axis  
Drawing lines| [Subroutine: LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)Draw a shallow line going right and up or left and down  
Drawing lines| [Subroutine: LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)Draw a shallow line going right and down or left and up  
Drawing lines| [Subroutine: LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)Draw a line: Line has a steep gradient, step up along y-axis  
Drawing lines| [Subroutine: LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)Draw a steep line going up and left or down and right  
Drawing lines| [Subroutine: LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)Draw a steep line going up and right or down and left  
Drawing lines| [Subroutine: HLOIN](../main/subroutine/hloin.md)Draw a horizontal line from (X1, Y1) to (X2, Y1)  
Drawing pixels| [Variable: TWFL](../main/variable/twfl.md)Ready-made character rows for the left end of a horizontal line  
Drawing pixels| [Variable: TWFR](../main/variable/twfr.md)Ready-made character rows for the right end of a horizontal line  
Drawing pixels| [Variable: orange](../main/variable/orange.md)Lookup table for two-pixel mode 1 orange pixels for the sun  
Maths (Arithmetic)| [Subroutine: PIX1](../main/subroutine/pix1.md)Calculate (YY+1 SYL+Y) = (A P) + (S R) and draw stardust particle  
Drawing pixels| [Subroutine: PIXEL2](../main/subroutine/pixel2.md)Draw a stardust particle relative to the screen centre  
Drawing pixels| [Subroutine: PIXEL](../main/subroutine/pixel.md)Draw a one-pixel dot, two-pixel dash or four-pixel square  
Drawing pixels| [Variable: PXCL](../main/variable/pxcl.md)A four-colour mode 1 pixel byte that represents a dot's distance  
Dashboard| [Subroutine: DOT](../main/subroutine/dot.md)Draw a dash on the compass  
Drawing pixels| [Subroutine: CPIXK](../main/subroutine/cpixk.md)Draw a single-height dash on the dashboard  
Dashboard| [Subroutine: ECBLB2](../main/subroutine/ecblb2.md)Start up the E.C.M. (light up the indicator, start the countdown and make the E.C.M. sound)  
Dashboard| [Subroutine: ECBLB](../main/subroutine/ecblb.md)Light up the E.C.M. indicator bulb ("E") on the dashboard  
Dashboard| [Subroutine: SPBLB](../main/subroutine/spblb.md)Light up the space station indicator ("S") on the dashboard  
Dashboard| [Variable: SPBT](../main/variable/spbt.md)The bitmap definition for the space station indicator bulb  
Dashboard| [Variable: ECBT](../main/variable/ecbt.md)The character bitmap for the E.C.M. indicator bulb  
Dashboard| [Subroutine: MSBAR](../main/subroutine/msbar.md)Draw a specific indicator in the dashboard's missile bar  
Ship hangar| [Variable: HANGFLAG](../main/variable/hangflag.md)The number of ships being displayed in the ship hangar  
Ship hangar| [Subroutine: HANGER](../main/subroutine/hanger.md)Display the ship hangar  
Ship hangar| [Subroutine: HAS2](../main/subroutine/has2.md)Draw a hangar background line from left to right  
Ship hangar| [Subroutine: HAL3](../main/subroutine/hal3.md)Draw a hangar background line from left to right, stopping when it bumps into existing on-screen content  
Ship hangar| [Subroutine: HAS3](../main/subroutine/has3.md)Draw a hangar background line from right to left  
Maths (Arithmetic)| [Subroutine: DVID4K](../main/subroutine/dvid4k.md)Calculate (P R) = 256 * A / Q  
Drawing the screen| [Subroutine: cls](../main/subroutine/cls.md)Clear the top part of the screen and draw a border box  
Text| [Subroutine: TT67K](../main/subroutine/tt67k.md)Print a newline  
Text| [Subroutine: CHPR](../main/subroutine/chpr.md)Print a character at the text cursor by poking into screen memory  
Drawing the screen| [Subroutine: TTX66](../main/subroutine/ttx66.md)Clear the top part of the screen and draw a border box  
Utility routines| [Subroutine: ZES1](../main/subroutine/zes1.md)Zero-fill the page whose number is in X  
Utility routines| [Subroutine: ZES2](../main/subroutine/zes2.md)Zero-fill a specific page  
Drawing the screen| [Subroutine: CLYNS](../main/subroutine/clyns.md)Clear the bottom three text rows of the space view  
Dashboard| [Subroutine: DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md)Update the dashboard: speed indicator  
Dashboard| [Subroutine: DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)Update the dashboard: pitch and roll indicators  
Dashboard| [Subroutine: DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)Update the dashboard: four energy banks  
Dashboard| [Subroutine: DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)Update the dashboard: shields, fuel, laser & cabin temp, altitude  
Dashboard| [Subroutine: PZW2](../main/subroutine/pzw2.md)Fetch the current dashboard colours for non-striped indicators, to support flashing  
Dashboard| [Subroutine: PZW](../main/subroutine/pzw.md)Fetch the current dashboard colours, to support flashing  
Dashboard| [Subroutine: DILX](../main/subroutine/dilx.md)Update a bar-based indicator on the dashboard  
Dashboard| [Subroutine: DIL2](../main/subroutine/dil2.md)Update the roll or pitch indicator on the dashboard  
Maths (Arithmetic)| [Subroutine: ADDK](../main/subroutine/addk.md)Calculate (A X) = (A P) + (S R)  
Text| [Variable: FONT%](../main/variable/font_per_cent.md)A copy of the character definition bitmap table from the MOS ROM  
Maths (Arithmetic)| [Variable: log](../main/variable/log.md)Binary logarithm table (high byte)  
Maths (Arithmetic)| [Variable: logL](../main/variable/logl.md)Binary logarithm table (low byte)  
Maths (Arithmetic)| [Variable: alogh](../main/variable/alogh.md)Binary antilogarithm table  
Drawing the screen| [Variable: SCTBX1](../main/variable/sctbx1.md)Lookup table for converting a pixel x-coordinate to the bit number within the pixel row byte that corresponds to this pixel  
Drawing the screen| [Variable: SCTBX2](../main/variable/sctbx2.md)Lookup table for converting a pixel x-coordinate to the byte number in the pixel row that corresponds to this pixel  
Save and load| [Variable: wtable](../main/variable/wtable.md)6-bit to 7-bit nibble conversion table  
Workspaces| [Workspace: UP](../main/workspace/up.md)Configuration variables  
Keyboard| [Variable: TGINT](../main/variable/tgint.md)The keys used to toggle configuration settings when the game is paused  
Loader| [Subroutine: S%](../main/subroutine/s_per_cent.md)Move code, set up break handler and start the game  
Utility routines| [Subroutine: DEEOR](../main/subroutine/deeor.md)Unscramble the main code  
Utility routines| [Subroutine: DEEORS](../main/subroutine/deeors.md)Decrypt a multi-page block of memory  
Utility routines| [Variable: G%](../main/variable/g_per_cent.md)Denotes the start of the main game code, from ELITE A to ELITE H  
Flight| [Subroutine: DOENTRY](../main/subroutine/doentry.md)Dock at the space station, show the ship hangar and work out any mission progression  
Missions| [Variable: TRIBDIR](../main/variable/tribdir.md)The low byte of the four 16-bit directions in which Trumble sprites can move  
Missions| [Variable: TRIBDIRH](../main/variable/tribdirh.md)The high byte of the four 16-bit directions in which Trumble sprites can move  
Missions| [Variable: SPMASK](../main/variable/spmask.md)Masks for updating sprite bits in VIC+&10 for the top bit of the 9-bit x-coordinates of the Trumble sprites  
Main loop| [Subroutine: Main flight loop (Part 1 of 16)](../main/subroutine/main_flight_loop_part_1_of_16.md)Seed the random number generator  
Main loop| [Subroutine: Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)Calculate the alpha and beta angles from the current pitch and roll of our ship  
Main loop| [Subroutine: Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)Scan for flight keys and process the results  
Main loop| [Subroutine: Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)For each nearby ship: Copy the ship's data block from K% to the zero-page workspace at INWK  
Main loop| [Subroutine: Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)For each nearby ship: If an energy bomb has been set off, potentially kill this ship  
Main loop| [Subroutine: Main flight loop (Part 6 of 16)](../main/subroutine/main_flight_loop_part_6_of_16.md)For each nearby ship: Move the ship in space and copy the updated INWK data block back to K%  
Main loop| [Subroutine: Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md)For each nearby ship: Check whether we are docking, scooping or colliding with it  
Main loop| [Subroutine: Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md)For each nearby ship: Process us potentially scooping this item  
Main loop| [Subroutine: Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md)For each nearby ship: If it is a space station, check whether we are successfully docking with it  
Main loop| [Subroutine: Main flight loop (Part 10 of 16)](../main/subroutine/main_flight_loop_part_10_of_16.md)For each nearby ship: Remove if scooped, or process collisions  
Main loop| [Subroutine: Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)For each nearby ship: Process missile lock and firing our laser  
Main loop| [Subroutine: Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)For each nearby ship: Draw the ship, remove if killed, loop back  
Main loop| [Subroutine: Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)Show energy bomb effect, charge shields and energy banks  
Main loop| [Subroutine: Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md)Spawn a space station if we are close enough to the planet  
Main loop| [Subroutine: Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)Perform altitude checks with the planet and sun and process fuel scooping if appropriate  
Main loop| [Subroutine: Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)Process laser pulsing, E.C.M. energy drain, call stardust routine  
Universe| [Subroutine: SPIN](../main/subroutine/spin.md)Randomly spawn cargo from a destroyed ship  
Drawing lines| [Subroutine: BOMBOFF](../main/subroutine/bomboff.md)Draw the zig-zag lightning bolt for the energy bomb  
Drawing lines| [Subroutine: BOMBEFF2](../main/subroutine/bombeff2.md)Erase the energy bomb zig-zag lightning bolt, make the sound of the energy bomb going off, draw a new bolt and repeat four times  
Drawing lines| [Subroutine: BOMBON](../main/subroutine/bombon.md)Randomise and draw the energy bomb's zig-zag lightning bolt lines  
Drawing lines| [Variable: BOMBPOS](../main/variable/bombpos.md)A set of x-coordinates that are used as the basis for the energy bomb's zig-zag lightning bolt  
Drawing lines| [Variable: BOMBTBX](../main/variable/bombtbx.md)This is where we store the x-coordinates for the energy bomb's zig-zag lightning bolt  
Drawing lines| [Variable: BOMBTBY](../main/variable/bombtby.md)This is where we store the y-coordinates for the energy bomb's zig-zag lightning bolt  
Text| [Subroutine: MT27](../main/subroutine/mt27.md)Print the captain's name during mission briefings  
Text| [Subroutine: MT28](../main/subroutine/mt28.md)Print the location hint during the mission 1 briefing  
Text| [Subroutine: DETOK3](../main/subroutine/detok3.md)Print an extended recursive token from the RUTOK token table  
Text| [Subroutine: DETOK](../main/subroutine/detok.md)Print an extended recursive token from the TKN1 token table  
Text| [Subroutine: DETOK2](../main/subroutine/detok2.md)Print an extended text token (1-255)  
Text| [Subroutine: MT1](../main/subroutine/mt1.md)Switch to ALL CAPS when printing extended tokens  
Text| [Subroutine: MT2](../main/subroutine/mt2.md)Switch to Sentence Case when printing extended tokens  
Text| [Subroutine: MT8](../main/subroutine/mt8.md)Tab to column 6 and start a new word when printing extended tokens  
Text| [Subroutine: MT9](../main/subroutine/mt9.md)Clear the screen and set the current view type to 1  
Text| [Subroutine: MT13](../main/subroutine/mt13.md)Switch to lower case when printing extended tokens  
Text| [Subroutine: MT6](../main/subroutine/mt6.md)Switch to standard tokens in Sentence Case  
Text| [Subroutine: MT5](../main/subroutine/mt5.md)Switch to extended tokens  
Text| [Subroutine: MT14](../main/subroutine/mt14.md)Switch to justified text when printing extended tokens  
Text| [Subroutine: MT15](../main/subroutine/mt15.md)Switch to left-aligned text when printing extended tokens  
Text| [Subroutine: MT17](../main/subroutine/mt17.md)Print the selected system's adjective, e.g. Lavian for Lave  
Text| [Subroutine: MT18](../main/subroutine/mt18.md)Print a random 1-8 letter word in Sentence Case  
Text| [Subroutine: MT19](../main/subroutine/mt19.md)Capitalise the next letter  
Text| [Subroutine: VOWEL](../main/subroutine/vowel.md)Test whether a character is a vowel  
Text| [Variable: JMTB](../main/variable/jmtb.md)The extended token table for jump tokens 1-32 (DETOK)  
Text| [Variable: TKN2](../main/variable/tkn2.md)The extended two-letter token lookup table  
Text| [Variable: QQ16](../main/variable/qq16.md)The two-letter token lookup table  
Save and load| [Variable: NA%](../main/variable/na_per_cent.md)The data block for the last saved commander  
Save and load| [Variable: CHK2](../main/variable/chk2.md)Second checksum byte for the saved commander data file  
Save and load| [Variable: CHK](../main/variable/chk.md)First checksum byte for the saved commander data file  
Save and load| [Variable: S1%](../main/variable/s1_per_cent.md)The drive and directory number used when saving or loading a commander file  
Save and load| [Variable: NA2%](../main/variable/na2_per_cent.md)The data block for the default commander  
Drawing ships| [Variable: shpcol](../main/variable/shpcol.md)Ship colours  
Drawing ships| [Variable: scacol](../main/variable/scacol.md)Ship colours on the scanner  
  
## Elite B  
\-------

Category| Details  
---|---  
Universe| [Variable: UNIV](../main/variable/univ.md)Table of pointers to the local universe's ship data blocks  
Keyboard| [Subroutine: FLKB](../main/subroutine/flkb.md)Flush the keyboard buffer  
Drawing lines| [Subroutine: NLIN3](../main/subroutine/nlin3.md)Print a title and draw a horizontal line at row 19 to box it in  
Drawing lines| [Subroutine: NLIN4](../main/subroutine/nlin4.md)Draw a horizontal line at pixel row 19 to box in a title  
Drawing lines| [Subroutine: NLIN](../main/subroutine/nlin.md)Draw a horizontal line at pixel row 23 to box in a title  
Drawing lines| [Subroutine: NLIN2](../main/subroutine/nlin2.md)Draw a screen-wide horizontal line at the pixel row in A  
Drawing lines| [Subroutine: HLOIN2](../main/subroutine/hloin2.md)Remove a line from the sun line heap and draw it on-screen  
Drawing circles| [Subroutine: BLINE](../main/subroutine/bline.md)Draw a circle segment and add it to the ball line heap  
Stardust| [Subroutine: FLIP](../main/subroutine/flip.md)Reflect the stardust particles in the screen diagonal and redraw the stardust field  
Stardust| [Subroutine: STARS](../main/subroutine/stars.md)The main routine for processing the stardust  
Stardust| [Subroutine: STARS1](../main/subroutine/stars1.md)Process the stardust for the front view  
Stardust| [Subroutine: STARS6](../main/subroutine/stars6.md)Process the stardust for the rear view  
Maths (Geometry)| [Subroutine: MAS1](../main/subroutine/mas1.md)Add an orientation vector coordinate to an INWK coordinate  
Maths (Geometry)| [Subroutine: MAS2](../main/subroutine/mas2.md)Calculate a cap on the maximum distance to the planet or sun  
Maths (Arithmetic)| [Subroutine: MAS3](../main/subroutine/mas3.md)Calculate A = x_hi^2 + y_hi^2 + z_hi^2 in the K% block  
Status| [Subroutine: STATUS](../main/subroutine/status.md)Show the Status Mode screen (red key f8)  
Text| [Subroutine: plf2](../main/subroutine/plf2.md)Print text followed by a newline and indent of 6 characters  
Moving| [Subroutine: MVT3](../main/subroutine/mvt3.md)Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)  
Moving| [Subroutine: MVS5](../main/subroutine/mvs5.md)Apply a 3.6 degree pitch or roll to an orientation vector  
Text| [Variable: TENS](../main/variable/tens.md)A constant used when printing large numbers in BPRNT  
Text| [Subroutine: pr2](../main/subroutine/pr2.md)Print an 8-bit number, left-padded to 3 digits, and optional point  
Text| [Subroutine: TT11](../main/subroutine/tt11.md)Print a 16-bit number, left-padded to n digits, and optional point  
Text| [Subroutine: BPRNT](../main/subroutine/bprnt.md)Print a 32-bit number, left-padded to a specific number of digits, with an optional decimal point  
Text| [Variable: DTW1](../main/variable/dtw1.md)A mask for applying the lower case part of Sentence Case to extended text tokens  
Text| [Variable: DTW2](../main/variable/dtw2.md)A flag that indicates whether we are currently printing a word  
Text| [Variable: DTW3](../main/variable/dtw3.md)A flag for switching between standard and extended text tokens  
Text| [Variable: DTW4](../main/variable/dtw4.md)Flags that govern how justified extended text tokens are printed  
Text| [Variable: DTW5](../main/variable/dtw5.md)The size of the justified text buffer at BUF  
Text| [Variable: DTW6](../main/variable/dtw6.md)A flag to denote whether printing in lower case is enabled for extended text tokens  
Text| [Variable: DTW8](../main/variable/dtw8.md)A mask for capitalising the next letter in an extended text token  
Text| [Subroutine: FEED](../main/subroutine/feed.md)Print a newline  
Text| [Subroutine: MT16](../main/subroutine/mt16.md)Print the character in variable DTW7  
Text| [Subroutine: TT26](../main/subroutine/tt26.md)Print a character at the text cursor, with support for verified text in extended tokens  
Sound| [Subroutine: BELL](../main/subroutine/bell.md)Make a standard system beep  
Flight| [Subroutine: ESCAPE](../main/subroutine/escape.md)Launch our escape pod  
Charts| [Subroutine: HME2](../main/subroutine/hme2.md)Search the galaxy for a system  
  
## Elite C  
\-------

Category| Details  
---|---  
Ship hangar| [Variable: HATB](../main/variable/hatb.md)Ship hangar group table  
Ship hangar| [Subroutine: HALL](../main/subroutine/hall.md)Draw the ships in the ship hangar, then draw the hangar  
Ship hangar| [Subroutine: HAS1](../main/subroutine/has1.md)Draw a ship in the ship hangar  
Tactics| [Subroutine: TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)Apply tactics: Process missiles, both enemy missiles and our own  
Tactics| [Subroutine: TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)Apply tactics: Escape pod, station, lone Thargon, safe-zone pirate  
Tactics| [Subroutine: TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)Apply tactics: Calculate dot product to determine ship's aim  
Tactics| [Subroutine: TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)Apply tactics: Check energy levels, maybe launch escape pod if low  
Tactics| [Subroutine: TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)Apply tactics: Consider whether to launch a missile at us  
Tactics| [Subroutine: TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md)Apply tactics: Consider firing a laser at us, if aim is true  
Tactics| [Subroutine: TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)Apply tactics: Set pitch, roll, and acceleration  
Flight| [Subroutine: DOCKIT](../main/subroutine/dockit.md)Apply docking manoeuvres to the ship in INWK  
Maths (Arithmetic)| [Subroutine: VCSU1](../main/subroutine/vcsu1.md)Calculate vector K3(8 0) = [x y z] - coordinates of the sun or space station  
Maths (Arithmetic)| [Subroutine: VCSUB](../main/subroutine/vcsub.md)Calculate vector K3(8 0) = [x y z] - coordinates in (A V)  
Maths (Arithmetic)| [Subroutine: TAS1](../main/subroutine/tas1.md)Calculate K3 = (x_sign x_hi x_lo) - V(1 0)  
Maths (Geometry)| [Subroutine: TAS4](../main/subroutine/tas4.md)Calculate the dot product of XX15 and one of the space station's orientation vectors  
Maths (Geometry)| [Subroutine: TAS6](../main/subroutine/tas6.md)Negate the vector in XX15 so it points in the opposite direction  
Flight| [Subroutine: DCS1](../main/subroutine/dcs1.md)Calculate the vector from the ideal docking position to the ship  
Tactics| [Subroutine: HITCH](../main/subroutine/hitch.md)Work out if the ship in INWK is in our crosshairs  
Tactics| [Subroutine: FRS1](../main/subroutine/frs1.md)Launch a ship straight ahead of us, below the laser sights  
Tactics| [Subroutine: FRMIS](../main/subroutine/frmis.md)Fire a missile from our ship  
Tactics| [Subroutine: ANGRY](../main/subroutine/angry.md)Make a ship or station hostile, and if this is a ship then enable the ship's AI and give it a kick of speed  
Tactics| [Subroutine: FR1](../main/subroutine/fr1.md)Display the "missile jammed" message  
Flight| [Subroutine: SESCP](../main/subroutine/sescp.md)Spawn an escape pod from the current (parent) ship  
Universe| [Subroutine: SFS1](../main/subroutine/sfs1.md)Spawn a child ship from the current (parent) ship  
Moving| [Subroutine: SFS2](../main/subroutine/sfs2.md)Move a ship in space along one of the coordinate axes  
Drawing circles| [Subroutine: LL164](../main/subroutine/ll164.md)Make the hyperspace sound and draw the hyperspace tunnel  
Drawing circles| [Subroutine: LAUN](../main/subroutine/laun.md)Make the launch sound and draw the launch tunnel  
Drawing circles| [Subroutine: HFS2](../main/subroutine/hfs2.md)Draw the launch or hyperspace tunnel  
Stardust| [Subroutine: STARS2](../main/subroutine/stars2.md)Process the stardust for the left or right view  
Maths (Arithmetic)| [Subroutine: MU5](../main/subroutine/mu5.md)Set K(3 2 1 0) = (A A A A) and clear the C flag  
Maths (Arithmetic)| [Subroutine: MULT3](../main/subroutine/mult3.md)Calculate K(3 2 1 0) = (A P+1 P) * Q  
Maths (Arithmetic)| [Subroutine: MLS2](../main/subroutine/mls2.md)Calculate (S R) = XX(1 0) and (A P) = A * ALP1  
Maths (Arithmetic)| [Subroutine: MLS1](../main/subroutine/mls1.md)Calculate (A P) = ALP1 * A  
Maths (Arithmetic)| [Subroutine: MU6](../main/subroutine/mu6.md)Set P(1 0) = (A A)  
Maths (Arithmetic)| [Subroutine: SQUA](../main/subroutine/squa.md)Clear bit 7 of A and calculate (A P) = A * A  
Maths (Arithmetic)| [Subroutine: SQUA2](../main/subroutine/squa2.md)Calculate (A P) = A * A  
Maths (Arithmetic)| [Subroutine: MU1](../main/subroutine/mu1.md)Copy X into P and A, and clear the C flag  
Maths (Arithmetic)| [Subroutine: MLU1](../main/subroutine/mlu1.md)Calculate Y1 = y_hi and (A P) = |y_hi| * Q for Y-th stardust  
Maths (Arithmetic)| [Subroutine: MLU2](../main/subroutine/mlu2.md)Calculate (A P) = |A| * Q  
Maths (Arithmetic)| [Subroutine: MULTU](../main/subroutine/multu.md)Calculate (A P) = P * Q  
Maths (Arithmetic)| [Subroutine: MU11](../main/subroutine/mu11.md)Calculate (A P) = P * X  
Maths (Arithmetic)| [Subroutine: FMLTU2](../main/subroutine/fmltu2.md)Calculate A = K * sin(A)  
Maths (Arithmetic)| [Subroutine: FMLTU](../main/subroutine/fmltu.md)Calculate A = A * Q / 256  
Maths (Arithmetic)| [Subroutine: MLTU2](../main/subroutine/mltu2.md)Calculate (A P+1 P) = (A ~P) * Q  
Maths (Arithmetic)| [Subroutine: MUT3](../main/subroutine/mut3.md)An unused routine that does the same as MUT2  
Maths (Arithmetic)| [Subroutine: MUT2](../main/subroutine/mut2.md)Calculate (S R) = XX(1 0) and (A P) = Q * A  
Maths (Arithmetic)| [Subroutine: MUT1](../main/subroutine/mut1.md)Calculate R = XX and (A P) = Q * A  
Maths (Arithmetic)| [Subroutine: MULT1](../main/subroutine/mult1.md)Calculate (A P) = Q * A  
Maths (Arithmetic)| [Subroutine: MULT12](../main/subroutine/mult12.md)Calculate (S R) = Q * A  
Maths (Geometry)| [Subroutine: TAS3](../main/subroutine/tas3.md)Calculate the dot product of XX15 and an orientation vector  
Maths (Arithmetic)| [Subroutine: MAD](../main/subroutine/mad.md)Calculate (A X) = Q * A + (S R)  
Maths (Arithmetic)| [Subroutine: ADD](../main/subroutine/add.md)Calculate (A X) = (A P) + (S R)  
Maths (Arithmetic)| [Subroutine: TIS1](../main/subroutine/tis1.md)Calculate (A ?) = (-X * A + (S R)) / 96  
Maths (Arithmetic)| [Subroutine: DV42](../main/subroutine/dv42.md)Calculate (P R) = 256 * DELTA / z_hi  
Maths (Arithmetic)| [Subroutine: DV41](../main/subroutine/dv41.md)Calculate (P R) = 256 * DELTA / A  
Maths (Arithmetic)| [Subroutine: DVID4](../main/subroutine/dvid4.md)Calculate (P R) = 256 * A / Q  
Maths (Arithmetic)| [Subroutine: DVID3B2](../main/subroutine/dvid3b2.md)Calculate K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)  
Dashboard| [Subroutine: cntr](../main/subroutine/cntr.md)Apply damping to the pitch or roll dashboard indicator  
Dashboard| [Subroutine: BUMP2](../main/subroutine/bump2.md)Bump up the value of the pitch or roll dashboard indicator  
Dashboard| [Subroutine: REDU2](../main/subroutine/redu2.md)Reduce the value of the pitch or roll dashboard indicator  
Maths (Geometry)| [Subroutine: ARCTAN](../main/subroutine/arctan.md)Calculate A = arctan(P / Q)  
Drawing lines| [Subroutine: LASLI](../main/subroutine/lasli.md)Draw the laser lines for when we fire our lasers  
Universe| [Subroutine: PDESC](../main/subroutine/pdesc.md)Print the system's extended description or a mission 1 directive  
Missions| [Subroutine: BRIEF2](../main/subroutine/brief2.md)Start mission 2  
Missions| [Subroutine: BRP](../main/subroutine/brp.md)Print an extended token and show the Status Mode screen  
Missions| [Subroutine: BRIEF3](../main/subroutine/brief3.md)Receive the briefing and plans for mission 2  
Missions| [Subroutine: DEBRIEF2](../main/subroutine/debrief2.md)Finish mission 2  
Missions| [Subroutine: DEBRIEF](../main/subroutine/debrief.md)Finish mission 1  
Missions| [Subroutine: TBRIEF](../main/subroutine/tbrief.md)Start mission 3  
Missions| [Subroutine: BRIEF](../main/subroutine/brief.md)Start mission 1 and show the mission briefing  
Missions| [Subroutine: BRIS](../main/subroutine/bris.md)Clear the screen, display "INCOMING MESSAGE" and wait for 2 seconds  
Missions| [Subroutine: PAUSE](../main/subroutine/pause.md)Display a rotating ship, waiting until a key is pressed, then remove the ship from the screen  
Text| [Subroutine: MT23](../main/subroutine/mt23.md)Move to row 10, switch to white text, and switch to lower case when printing extended tokens  
Text| [Subroutine: MT29](../main/subroutine/mt29.md)Move to row 6, switch to white text, and switch to lower case when printing extended tokens  
Missions| [Subroutine: PAS1](../main/subroutine/pas1.md)Display a rotating ship at space coordinates (0, 120, 256) and scan the keyboard  
Keyboard| [Subroutine: PAUSE2](../main/subroutine/pause2.md)Wait until a key is pressed, ignoring any existing key press  
Universe| [Subroutine: GINF](../main/subroutine/ginf.md)Fetch the address of a ship's data block into INF  
Universe| [Subroutine: ping](../main/subroutine/ping.md)Set the selected system to the current system  
Text| [Variable: MTIN](../main/variable/mtin.md)Lookup table for random tokens in the extended token table (0-37)  
  
## Elite D  
\-------

Category| Details  
---|---  
Maths (Geometry)| [Subroutine: SCALEY](../main/subroutine/scaley.md)Scale the y-coordinate in A to 0.5 * A  
Maths (Geometry)| [Subroutine: SCALEY2](../main/subroutine/scaley2.md)Scale the y-coordinate in A (leave it unchanged)  
Maths (Geometry)| [Subroutine: SCALEX](../main/subroutine/scalex.md)Scale the x-coordinate in A (leave it unchanged)  
Drawing lines| [Subroutine: DVLOIN](../main/subroutine/dvloin.md)Draw a vertical line from (A, GCYT) to (A, GCYB)  
Market| [Subroutine: tnpr1](../main/subroutine/tnpr1.md)Work out if we have space for one tonne of cargo  
Market| [Subroutine: tnpr](../main/subroutine/tnpr.md)Work out if we have space for a specific amount of cargo  
Text| [Subroutine: DOXC](../main/subroutine/doxc.md)Move the text cursor to a specific column  
Text| [Subroutine: DOYC](../main/subroutine/doyc.md)Move the text cursor to a specific row  
Text| [Subroutine: INCYC](../main/subroutine/incyc.md)Move the text cursor to the next row  
Drawing the screen| [Subroutine: TRADEMODE](../main/subroutine/trademode.md)Clear the screen and set up a trading screen  
Universe| [Subroutine: TT20](../main/subroutine/tt20.md)Twist the selected system's seeds four times  
Universe| [Subroutine: TT54](../main/subroutine/tt54.md)Twist the selected system's seeds  
Universe| [Subroutine: TT146](../main/subroutine/tt146.md)Print the distance to the selected system in light years  
Text| [Subroutine: TT60](../main/subroutine/tt60.md)Print a text token and a paragraph break  
Text| [Subroutine: TTX69](../main/subroutine/ttx69.md)Print a paragraph break  
Text| [Subroutine: TT69](../main/subroutine/tt69.md)Set Sentence Case and print a newline  
Text| [Subroutine: TT67](../main/subroutine/tt67.md)Print a newline  
Universe| [Subroutine: TT70](../main/subroutine/tt70.md)Display "MAINLY " and jump to TT72  
Text| [Subroutine: spc](../main/subroutine/spc.md)Print a text token followed by a space  
Universe| [Subroutine: TT25](../main/subroutine/tt25.md)Show the Data on System screen (red key f6)  
Universe| [Subroutine: TT24](../main/subroutine/tt24.md)Calculate system data from the system seeds  
Charts| [Subroutine: TT22](../main/subroutine/tt22.md)Show the Long-range Chart (red key f4)  
Drawing lines| [Subroutine: TT15](../main/subroutine/tt15.md)Draw a set of crosshairs  
Drawing circles| [Subroutine: TT14](../main/subroutine/tt14.md)Draw a circle with crosshairs on a chart  
Drawing circles| [Subroutine: TT128](../main/subroutine/tt128.md)Draw a circle on a chart  
Market| [Subroutine: TT219](../main/subroutine/tt219.md)Show the Buy Cargo screen (red key f1)  
Market| [Subroutine: gnum](../main/subroutine/gnum.md)Get a number from the keyboard  
Market| [Subroutine: NWDAV4](../main/subroutine/nwdav4.md)Print an "ITEM?" error, make a beep and rejoin the TT210 routine  
Text| [Subroutine: OUTK](../main/subroutine/outk.md)Print the character in Q before returning to gnum  
Market| [Subroutine: TT208](../main/subroutine/tt208.md)Show the Sell Cargo screen (red key f2)  
Market| [Subroutine: TT210](../main/subroutine/tt210.md)Show a list of current cargo in our hold, optionally to sell  
Market| [Subroutine: TT213](../main/subroutine/tt213.md)Show the Inventory screen (red key f9)  
Keyboard| [Subroutine: TT214](../main/subroutine/tt214.md)Ask a question with a "Y/N?" prompt and return the response  
Charts| [Subroutine: TT16](../main/subroutine/tt16.md)Move the crosshairs on a chart  
Charts| [Subroutine: TT103](../main/subroutine/tt103.md)Draw a small set of crosshairs on a chart  
Charts| [Subroutine: TT123](../main/subroutine/tt123.md)Move galactic coordinates by a signed delta  
Charts| [Subroutine: TT105](../main/subroutine/tt105.md)Draw crosshairs on the Short-range Chart, with clipping  
Charts| [Subroutine: TT23](../main/subroutine/tt23.md)Show the Short-range Chart (red key f5)  
Universe| [Subroutine: TT81](../main/subroutine/tt81.md)Set the selected system's seeds to those of system 0  
Universe| [Subroutine: TT111](../main/subroutine/tt111.md)Set the current system to the nearest system to a point  
Flight| [Subroutine: dockEd](../main/subroutine/docked.md)Print a message to say there is no hyperspacing allowed inside the station  
Flight| [Subroutine: hyp](../main/subroutine/hyp.md)Start the hyperspace process  
Flight| [Subroutine: wW](../main/subroutine/ww.md)Start a hyperspace countdown  
Flight| [Subroutine: TTX110](../main/subroutine/ttx110.md)Set the current system to the nearest system and return to hyp  
Flight| [Subroutine: Ghy](../main/subroutine/ghy.md)Perform a galactic hyperspace jump  
Universe| [Subroutine: jmp](../main/subroutine/jmp.md)Set the current system to the selected system  
Flight| [Subroutine: ee3](../main/subroutine/ee3.md)Print the hyperspace countdown in the top-left of the screen  
Text| [Subroutine: pr6](../main/subroutine/pr6.md)Print 16-bit number, left-padded to 5 digits, no point  
Text| [Subroutine: pr5](../main/subroutine/pr5.md)Print a 16-bit number, left-padded to 5 digits, and optional point  
Flight| [Subroutine: TT147](../main/subroutine/tt147.md)Print an error when a system is out of hyperspace range  
Text| [Subroutine: prq](../main/subroutine/prq.md)Print a text token followed by a question mark  
Market| [Subroutine: TT151](../main/subroutine/tt151.md)Print the name, price and availability of a market item  
Market| [Subroutine: TT152](../main/subroutine/tt152.md)Print the unit ("t", "kg" or "g") for a market item  
Text| [Subroutine: TT162](../main/subroutine/tt162.md)Print a space  
Market| [Subroutine: TT160](../main/subroutine/tt160.md)Print "t" (for tonne) and a space  
Market| [Subroutine: TT161](../main/subroutine/tt161.md)Print "kg" (for kilograms)  
Market| [Subroutine: TT16a](../main/subroutine/tt16a.md)Print "g" (for grams)  
Market| [Subroutine: TT163](../main/subroutine/tt163.md)Print the headers for the table of market prices  
Market| [Subroutine: TT167](../main/subroutine/tt167.md)Show the Market Price screen (red key f7)  
Market| [Subroutine: var](../main/subroutine/var.md)Calculate QQ19+3 = economy * |economic_factor|  
Universe| [Subroutine: hyp1](../main/subroutine/hyp1.md)Process a jump to the system closest to (QQ9, QQ10)  
Universe| [Subroutine: GVL](../main/subroutine/gvl.md)Calculate the availability of market items  
Universe| [Subroutine: GTHG](../main/subroutine/gthg.md)Spawn a Thargoid ship and a Thargon companion  
Flight| [Subroutine: MJP](../main/subroutine/mjp.md)Process a mis-jump into witchspace  
Flight| [Subroutine: TT18](../main/subroutine/tt18.md)Try to initiate a jump into hyperspace  
Flight| [Subroutine: TT110](../main/subroutine/tt110.md)Launch from a station or show the front space view  
Charts| [Subroutine: TT114](../main/subroutine/tt114.md)Display either the Long-range or Short-range Chart  
Maths (Arithmetic)| [Subroutine: LCASH](../main/subroutine/lcash.md)Subtract an amount of cash from the cash pot  
Maths (Arithmetic)| [Subroutine: MCASH](../main/subroutine/mcash.md)Add an amount of cash to the cash pot  
Maths (Arithmetic)| [Subroutine: GCASH](../main/subroutine/gcash.md)Calculate (Y X) = P * Q * 4  
Maths (Arithmetic)| [Subroutine: GC2](../main/subroutine/gc2.md)Calculate (Y X) = (A P) * 4  
Equipment| [Subroutine: EQSHP](../main/subroutine/eqshp.md)Show the Equip Ship screen (red key f3)  
Market| [Subroutine: dn](../main/subroutine/dn.md)Print the amount of money we have left in the cash pot, then make a short, high beep and delay for 1 second  
Text| [Subroutine: dn2](../main/subroutine/dn2.md)Make a short, high beep and delay for 0.5 seconds  
Equipment| [Subroutine: eq](../main/subroutine/eq.md)Subtract the price of equipment from the cash pot  
Equipment| [Subroutine: prx](../main/subroutine/prx.md)Return the price of a piece of equipment  
Equipment| [Subroutine: qv](../main/subroutine/qv.md)Print a menu of the four space views, for buying lasers  
Charts| [Subroutine: hm](../main/subroutine/hm.md)Select the closest system and redraw the chart crosshairs  
Equipment| [Subroutine: refund](../main/subroutine/refund.md)Install a new laser, processing a refund if applicable  
Equipment| [Variable: PRXS](../main/variable/prxs.md)Equipment prices  
  
## Elite E  
\-------

Category| Details  
---|---  
Universe| [Subroutine: cpl](../main/subroutine/cpl.md)Print the selected system name  
Status| [Subroutine: cmn](../main/subroutine/cmn.md)Print the commander's name  
Universe| [Subroutine: ypl](../main/subroutine/ypl.md)Print the current system name  
Universe| [Subroutine: tal](../main/subroutine/tal.md)Print the current galaxy number  
Status| [Subroutine: fwl](../main/subroutine/fwl.md)Print fuel and cash levels  
Status| [Subroutine: csh](../main/subroutine/csh.md)Print the current amount of cash  
Text| [Subroutine: plf](../main/subroutine/plf.md)Print a text token followed by a newline  
Text| [Subroutine: TT68](../main/subroutine/tt68.md)Print a text token followed by a colon  
Text| [Subroutine: TT73](../main/subroutine/tt73.md)Print a colon  
Text| [Subroutine: TT27](../main/subroutine/tt27.md)Print a text token  
Text| [Subroutine: TT42](../main/subroutine/tt42.md)Print a letter in lower case  
Text| [Subroutine: TT41](../main/subroutine/tt41.md)Print a letter according to Sentence Case  
Text| [Subroutine: qw](../main/subroutine/qw.md)Print a recursive token in the range 128-145  
Text| [Subroutine: crlf](../main/subroutine/crlf.md)Tab to column 21 and print a colon  
Text| [Subroutine: TT45](../main/subroutine/tt45.md)Print a letter in lower case  
Text| [Subroutine: TT46](../main/subroutine/tt46.md)Print a character and switch to capitals  
Text| [Subroutine: TT74](../main/subroutine/tt74.md)Print a character  
Text| [Subroutine: TT43](../main/subroutine/tt43.md)Print a two-letter token or recursive token 0-95  
Text| [Subroutine: ex](../main/subroutine/ex.md)Print a recursive token  
Utility routines| [Subroutine: SWAPPZERO](../main/subroutine/swappzero.md)An unused routine that swaps bytes in and out of zero page  
Drawing ships| [Subroutine: DOEXP](../main/subroutine/doexp.md)Draw an exploding ship  
Drawing ships| [Variable: exlook](../main/variable/exlook.md)A table to shift X left by one place when X is 0 or 1  
Drawing ships| [Variable: coltabl](../main/variable/coltabl.md)Colours for ship explosions  
Universe| [Subroutine: SOS1](../main/subroutine/sos1.md)Update the missile indicators, set up the planet data block  
Universe| [Subroutine: SOLAR](../main/subroutine/solar.md)Set up various aspects of arriving in a new system  
Stardust| [Subroutine: NWSTARS](../main/subroutine/nwstars.md)Initialise the stardust field  
Stardust| [Subroutine: nWq](../main/subroutine/nwq.md)Create a random cloud of stardust  
Dashboard| [Subroutine: WPSHPS](../main/subroutine/wpshps.md)Clear the scanner, reset the ball line and sun line heaps  
Drawing suns| [Subroutine: FLFLLS](../main/subroutine/flflls.md)Reset the sun line heap  
Drawing the screen| [Subroutine: DET1](../main/subroutine/det1.md)Show or hide the dashboard (for when we die)  
Flight| [Subroutine: SHD](../main/subroutine/shd.md)Charge a shield and drain some energy from the energy banks  
Flight| [Subroutine: DENGY](../main/subroutine/dengy.md)Drain some energy from the energy banks  
Dashboard| [Subroutine: COMPAS](../main/subroutine/compas.md)Update the compass  
Maths (Arithmetic)| [Subroutine: SPS2](../main/subroutine/sps2.md)Calculate (Y X) = A / 10  
Maths (Geometry)| [Subroutine: SPS4](../main/subroutine/sps4.md)Calculate the vector to the space station  
Dashboard| [Subroutine: SP1](../main/subroutine/sp1.md)Draw the space station on the compass  
Dashboard| [Subroutine: SP2](../main/subroutine/sp2.md)Draw a dot on the compass, given the planet/station vector  
Flight| [Subroutine: OOPS](../main/subroutine/oops.md)Take some damage  
Maths (Geometry)| [Subroutine: SPS3](../main/subroutine/sps3.md)Copy a space coordinate from the K% block into K3  
Universe| [Subroutine: NWSPS](../main/subroutine/nwsps.md)Add a new space station to our local bubble of universe  
Universe| [Subroutine: NWSHP](../main/subroutine/nwshp.md)Add a new ship to our local bubble of universe  
Universe| [Subroutine: NwS1](../main/subroutine/nws1.md)Flip the sign and double an INWK byte  
Dashboard| [Subroutine: ABORT](../main/subroutine/abort.md)Disarm missiles and update the dashboard indicators  
Dashboard| [Subroutine: ABORT2](../main/subroutine/abort2.md)Set/unset the lock target for a missile and update the dashboard  
Maths (Geometry)| [Subroutine: PROJ](../main/subroutine/proj.md)Project the current ship or planet onto the screen  
Drawing planets| [Subroutine: PL2](../main/subroutine/pl2.md)Remove the planet or sun from the screen  
Drawing planets| [Subroutine: PLANET](../main/subroutine/planet.md)Draw the planet or sun  
Drawing planets| [Subroutine: PL9 (Part 1 of 3)](../main/subroutine/pl9_part_1_of_3.md)Draw the planet, with either an equator and meridian, or a crater  
Drawing planets| [Subroutine: PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md)Draw the planet's equator and meridian  
Drawing planets| [Subroutine: PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)Draw the planet's crater  
Drawing planets| [Subroutine: PLS1](../main/subroutine/pls1.md)Calculate (Y A) = nosev_x / z  
Drawing planets| [Subroutine: PLS2](../main/subroutine/pls2.md)Draw a half-ellipse  
Drawing planets| [Subroutine: PLS22](../main/subroutine/pls22.md)Draw an ellipse or half-ellipse  
Drawing suns| [Subroutine: SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)Draw the sun: Set up all the variables needed to draw the sun  
Drawing suns| [Subroutine: SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)Draw the sun: Start from the bottom of the screen and erase the old sun line by line  
Drawing suns| [Subroutine: SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)Draw the sun: Continue to move up the screen, drawing the new sun line by line  
Drawing suns| [Subroutine: SUN (Part 4 of 4)](../main/subroutine/sun_part_4_of_4.md)Draw the sun: Continue to the top of the screen, erasing the old sun line by line  
Drawing circles| [Subroutine: CIRCLE](../main/subroutine/circle.md)Draw a circle for the planet  
Drawing circles| [Subroutine: CIRCLE2](../main/subroutine/circle2.md)Draw a circle (for the planet or chart)  
Drawing planets| [Subroutine: WPLS2](../main/subroutine/wpls2.md)Remove the planet from the screen  
Drawing planets| [Subroutine: WP1](../main/subroutine/wp1.md)Reset the ball line heap  
Drawing suns| [Subroutine: WPLS](../main/subroutine/wpls.md)Remove the sun from the screen  
Drawing lines| [Subroutine: EDGES](../main/subroutine/edges.md)Draw a horizontal line given a centre and a half-width  
Drawing circles| [Subroutine: CHKON](../main/subroutine/chkon.md)Check whether any part of a circle appears on the extended screen  
Drawing planets| [Subroutine: PL21](../main/subroutine/pl21.md)Return from a planet/sun-drawing routine with a failure flag  
Drawing planets| [Subroutine: PLS3](../main/subroutine/pls3.md)Calculate (Y A P) = 222 * roofv_x / z  
Drawing planets| [Subroutine: PLS4](../main/subroutine/pls4.md)Calculate CNT2 = arctan(P / A) / 4  
Drawing planets| [Subroutine: PLS5](../main/subroutine/pls5.md)Calculate roofv_x / z and roofv_y / z  
Drawing planets| [Subroutine: PLS6](../main/subroutine/pls6.md)Calculate (X K) = (A P+1 P) / (z_sign z_hi z_lo)  
Keyboard| [Subroutine: YESNO](../main/subroutine/yesno.md)Wait until either "Y" or "N" is pressed  
Keyboard| [Subroutine: TT17](../main/subroutine/tt17.md)Scan the keyboard for cursor key or joystick movement  
  
## Elite F  
\-------

Category| Details  
---|---  
Keyboard| [Subroutine: DJOY](../main/subroutine/djoy.md)Scan the keyboard for cursor key or digital joystick movement  
Universe| [Subroutine: KS3](../main/subroutine/ks3.md)Set the SLSP ship line heap pointer after shuffling ship slots  
Universe| [Subroutine: KS1](../main/subroutine/ks1.md)Remove the current ship from our local bubble of universe  
Universe| [Subroutine: KS4](../main/subroutine/ks4.md)Remove the space station and replace it with the sun  
Universe| [Subroutine: KS2](../main/subroutine/ks2.md)Check the local bubble for missiles with target lock  
Universe| [Subroutine: KILLSHP](../main/subroutine/killshp.md)Remove a ship from our local bubble of universe  
Missions| [Subroutine: THERE](../main/subroutine/there.md)Check whether we are in the Constrictor's system in mission 1  
Text| [Subroutine: NUMBOR](../main/subroutine/numbor.md)An unused routine that prints a number in hexadecimal  
Start and end| [Subroutine: RESET](../main/subroutine/reset.md)Reset most variables  
Start and end| [Subroutine: RES2](../main/subroutine/res2.md)Reset a number of flight variables and workspaces  
Universe| [Subroutine: ZINF](../main/subroutine/zinf.md)Reset the INWK workspace and orientation vectors  
Dashboard| [Subroutine: msblob](../main/subroutine/msblob.md)Display the dashboard's missile indicators in green  
Flight| [Subroutine: me2](../main/subroutine/me2.md)Remove an in-flight message from the space view  
Universe| [Subroutine: Ze](../main/subroutine/ze.md)Initialise the INWK workspace to a fairly aggressive ship  
Maths (Arithmetic)| [Subroutine: DORND](../main/subroutine/dornd.md)Generate random numbers  
Main loop| [Subroutine: Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md)Spawn a trader (a Cobra Mk III, Python, Boa or Anaconda)  
Main loop| [Subroutine: Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)Call the main flight loop, and potentially spawn a trader, an asteroid, or a cargo canister  
Main loop| [Subroutine: Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md)Potentially spawn a cop, particularly if we've been bad  
Main loop| [Subroutine: Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)Potentially spawn a lone bounty hunter, a Thargoid, or up to four pirates  
Main loop| [Subroutine: Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md)Cool down lasers, make calls to update the dashboard  
Main loop| [Subroutine: Main game loop (Part 6 of 6)](../main/subroutine/main_game_loop_part_6_of_6.md)Process non-flight key presses (red function keys, docked keys)  
Keyboard| [Subroutine: TT102](../main/subroutine/tt102.md)Process function key, save key, hyperspace and chart key presses and update the hyperspace counter  
Status| [Subroutine: BAD](../main/subroutine/bad.md)Calculate how bad we have been  
Maths (Geometry)| [Subroutine: FAROF](../main/subroutine/farof.md)Compare x_hi, y_hi and z_hi with 224  
Maths (Geometry)| [Subroutine: FAROF2](../main/subroutine/farof2.md)Compare x_hi, y_hi and z_hi with A  
Maths (Geometry)| [Subroutine: MAS4](../main/subroutine/mas4.md)Calculate a cap on the maximum distance to a ship  
Save and load| [Variable: stackpt](../main/variable/stackpt.md)Temporary storage for the stack pointer when jumping to the break handler at NEWBRK  
Utility routines| [Subroutine: NEWBRK](../main/subroutine/newbrk.md)The standard BRKV handler for the game  
Start and end| [Subroutine: DEATH](../main/subroutine/death.md)Display the death screen  
Universe| [Variable: spasto](../main/variable/spasto.md)Contains the address of the Coriolis space station's ship blueprint  
Loader| [Subroutine: BEGIN](../main/subroutine/begin.md)Initialise the configuration variables and start the game  
Start and end| [Subroutine: TT170](../main/subroutine/tt170.md)Main entry point for the Elite game code  
Start and end| [Subroutine: DEATH2](../main/subroutine/death2.md)Reset most of the game and restart from the title screen  
Start and end| [Subroutine: BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md)Show the "Load New Commander (Y/N)?" screen and start the game  
Start and end| [Subroutine: BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)Show the "Press Fire or Space, Commander" screen and start the game  
Status| [Subroutine: BAY](../main/subroutine/bay.md)Go to the docking bay (i.e. show the Status Mode screen)  
Start and end| [Subroutine: DFAULT](../main/subroutine/dfault.md)Reset the current commander data block to the last saved commander  
Start and end| [Subroutine: TITLE](../main/subroutine/title.md)Display a title screen with a rotating ship and prompt  
Save and load| [Subroutine: CHECK](../main/subroutine/check.md)Calculate the checksum for the last saved commander data block  
Save and load| [Subroutine: CHECK2](../main/subroutine/check2.md)Calculate the third checksum for the last saved commander data block (Commodore 64 and Apple II versions only)  
Save and load| [Subroutine: JAMESON](../main/subroutine/jameson.md)Restore the default JAMESON commander  
Save and load| [Subroutine: TRNME](../main/subroutine/trnme.md)Copy the last saved commander's name from INWK to NA%  
Save and load| [Subroutine: TR1](../main/subroutine/tr1.md)Copy the last saved commander's name from NA% to INWK  
Save and load| [Subroutine: GTNMEW](../main/subroutine/gtnmew.md)Fetch the name of a commander file to save or load  
Text| [Subroutine: MT26](../main/subroutine/mt26.md)Fetch a line of text from the keyboard  
Text| [Variable: RLINE](../main/variable/rline.md)The OSWORD configuration block used to fetch a line of text from the keyboard  
Save and load| [Subroutine: FILEPR](../main/subroutine/filepr.md)Display the currently selected media (disk or tape)  
Save and load| [Subroutine: OTHERFILEPR](../main/subroutine/otherfilepr.md)Display the non-selected media (disk or tape)  
Utility routines| [Subroutine: ZERO](../main/subroutine/zero.md)Reset the local bubble of universe and ship status  
Save and load| [Subroutine: GTDIR](../main/subroutine/gtdir.md)Fetch the name of an ADFS directory on the Master Compact and change to that directory  
Save and load| [Subroutine: CATS](../main/subroutine/cats.md)Ask for a disc drive number and print a catalogue of that drive  
Save and load| [Subroutine: DELT](../main/subroutine/delt.md)Catalogue a disc, ask for a filename to delete, and delete the file  
Save and load| [Subroutine: SVE](../main/subroutine/sve.md)Display the disk access menu and process saving of commander files  
Save and load| [Variable: thislong](../main/variable/thislong.md)Contains the length of the most recently entered commander name  
Save and load| [Variable: oldlong](../main/variable/oldlong.md)Contains the length of the last saved commander name  
Save and load| [Subroutine: GTDRV](../main/subroutine/gtdrv.md)Get an ASCII disc drive number from the keyboard  
Save and load| [Subroutine: LOD](../main/subroutine/lod.md)Load a commander file  
Utility routines| [Subroutine: backtonormal](../main/subroutine/backtonormal.md)Disable the keyboard, set the SVN flag to 0, and return with A = 0  
Utility routines| [Subroutine: CLDELAY](../main/subroutine/cldelay.md)Delay by iterating through 5 * 256 (1280) empty loops  
Save and load| [Variable: CTLI](../main/variable/ctli.md)The OS command string for cataloguing a disc  
Save and load| [Variable: DELI](../main/variable/deli.md)The OS command string for deleting a file  
Save and load| [Variable: DIRI](../main/variable/diri.md)The OS command string for changing directory on the Master Compact  
Save and load| [Variable: savosc](../main/variable/savosc.md)The OS command string for saving a commander file  
Save and load| [Variable: lodosc](../main/variable/lodosc.md)The OS command string for loading a commander file  
Save and load| [Subroutine: wfile](../main/subroutine/wfile.md)Save the commander file  
Save and load| [Subroutine: rfile](../main/subroutine/rfile.md)Load the commander file  
Maths (Geometry)| [Subroutine: SPS1](../main/subroutine/sps1.md)Calculate the vector to the planet and store it in XX15  
Maths (Geometry)| [Subroutine: TAS2](../main/subroutine/tas2.md)Normalise the three-coordinate vector in K3  
Maths (Geometry)| [Subroutine: NORM](../main/subroutine/norm.md)Normalise the three-coordinate vector in XX15  
Flight| [Subroutine: WARP](../main/subroutine/warp.md)Perform an in-system jump  
Keyboard| [Subroutine: DKS3](../main/subroutine/dks3.md)Toggle a configuration setting and emit a beep  
Keyboard| [Subroutine: DOKEY](../main/subroutine/dokey.md)Scan for the seven primary flight controls and apply the docking computer manoeuvring code  
Keyboard| [Subroutine: DK4](../main/subroutine/dk4.md)Scan for pause, configuration and secondary flight keys  
Keyboard| [Subroutine: TT217](../main/subroutine/tt217.md)Scan the keyboard until a key is pressed  
Flight| [Subroutine: me1](../main/subroutine/me1.md)Erase an old in-flight message and display a new one  
Flight| [Subroutine: MESS](../main/subroutine/mess.md)Display an in-flight message  
Flight| [Subroutine: mes9](../main/subroutine/mes9.md)Print a text token, possibly followed by " DESTROYED"  
Flight| [Subroutine: OUCH](../main/subroutine/ouch.md)Potentially lose cargo or equipment following damage  
Flight| [Subroutine: ou2](../main/subroutine/ou2.md)Display "E.C.M.SYSTEM DESTROYED" as an in-flight message  
Flight| [Subroutine: ou3](../main/subroutine/ou3.md)Display "FUEL SCOOPS DESTROYED" as an in-flight message  
Market| [Macro: ITEM](../main/macro/item.md)Macro definition for the market prices table  
Market| [Variable: QQ23](../main/variable/qq23.md)Market prices table  
Maths (Geometry)| [Subroutine: TIDY](../main/subroutine/tidy.md)Orthonormalise the orientation vectors for a ship  
Maths (Arithmetic)| [Subroutine: TIS2](../main/subroutine/tis2.md)Calculate A = A / Q  
Maths (Arithmetic)| [Subroutine: TIS3](../main/subroutine/tis3.md)Calculate -(nosev_1 * roofv_1 + nosev_2 * roofv_2) / nosev_3  
Maths (Arithmetic)| [Subroutine: DVIDT](../main/subroutine/dvidt.md)Calculate (P+1 A) = (A P) / Q  
Keyboard| [Variable: KTRAN](../main/variable/ktran.md)An unused key logger buffer that's left over from the 6502 Second Processor version of Elite  
  
## Elite G  
\-------

Category| Details  
---|---  
Drawing ships| [Subroutine: SHPPT](../main/subroutine/shppt.md)Draw a distant ship as a point rather than a full wireframe  
Maths (Arithmetic)| [Subroutine: LL5](../main/subroutine/ll5.md)Calculate Q = SQRT(R Q)  
Maths (Arithmetic)| [Subroutine: LL28](../main/subroutine/ll28.md)Calculate R = 256 * A / Q  
Maths (Arithmetic)| [Subroutine: LL38](../main/subroutine/ll38.md)Calculate (S A) = (S R) + (A Q)  
Maths (Geometry)| [Subroutine: LL51](../main/subroutine/ll51.md)Calculate the dot product of XX15 and XX16  
Drawing ships| [Subroutine: LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)Draw ship: Check if ship is exploding, check if ship is in front  
Drawing ships| [Subroutine: LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md)Draw ship: Check if ship is in field of view, close enough to draw  
Drawing ships| [Subroutine: LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)Draw ship: Set up orientation vector, ship coordinate variables  
Drawing ships| [Subroutine: LL9 (Part 4 of 12)](../main/subroutine/ll9_part_4_of_12.md)Draw ship: Set visibility for exploding ship (all faces visible)  
Drawing ships| [Subroutine: LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)Draw ship: Calculate the visibility of each of the ship's faces  
Drawing ships| [Subroutine: LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)Draw ship: Calculate the visibility of each of the ship's vertices  
Maths (Arithmetic)| [Subroutine: LL61](../main/subroutine/ll61.md)Calculate (U R) = 256 * A / Q  
Maths (Arithmetic)| [Subroutine: LL62](../main/subroutine/ll62.md)Calculate 128 - (U R)  
Drawing ships| [Subroutine: LL9 (Part 7 of 12)](../main/subroutine/ll9_part_7_of_12.md)Draw ship: Calculate the visibility of each of the ship's vertices  
Drawing ships| [Subroutine: LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)Draw ship: Calculate the screen coordinates of visible vertices  
Drawing ships| [Subroutine: LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)Draw ship: Draw laser beams if the ship is firing its laser at us  
Drawing ships| [Subroutine: LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)Draw ship: Calculate the visibility of each of the ship's edges and draw the visible ones using flicker-free animation  
Drawing ships| [Subroutine: LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)Draw ship: Loop back for the next edge  
Drawing lines| [Subroutine: LL118](../main/subroutine/ll118.md)Move a point along a line until it is on-screen  
Maths (Arithmetic)| [Subroutine: LL120](../main/subroutine/ll120.md)Calculate (Y X) = (S x1_lo) * XX12+2 or (S x1_lo) / XX12+2  
Maths (Arithmetic)| [Subroutine: LL123](../main/subroutine/ll123.md)Calculate (Y X) = (S R) / XX12+2 or (S R) * XX12+2  
Maths (Arithmetic)| [Subroutine: LL129](../main/subroutine/ll129.md)Calculate Q = XX12+2, A = S EOR XX12+3 and (S R) = |S R|  
Drawing lines| [Subroutine: LL145 (Part 1 of 4)](../main/subroutine/ll145_part_1_of_4.md)Clip line: Work out which end-points are on-screen, if any  
Drawing lines| [Subroutine: LL145 (Part 2 of 4)](../main/subroutine/ll145_part_2_of_4.md)Clip line: Work out if any part of the line is on-screen  
Drawing lines| [Subroutine: LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md)Clip line: Calculate the line's gradient  
Drawing lines| [Subroutine: LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)Clip line: Call the routine in LL188 to do the actual clipping  
Drawing ships| [Subroutine: LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md)Draw ship: Draw all the visible edges from the ship line heap  
Drawing lines| [Subroutine: LSPUT](../main/subroutine/lsput.md)Draw a ship line using flicker-free animation  
  
## Elite H  
\-------

Category| Details  
---|---  
Moving| [Subroutine: MVEIT (Part 1 of 9)](../main/subroutine/mveit_part_1_of_9.md)Move current ship: Tidy the orientation vectors  
Moving| [Subroutine: MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md)Move current ship: Call tactics routine, remove ship from scanner  
Moving| [Subroutine: MVEIT (Part 3 of 9)](../main/subroutine/mveit_part_3_of_9.md)Move current ship: Move ship forward according to its speed  
Moving| [Subroutine: MVEIT (Part 4 of 9)](../main/subroutine/mveit_part_4_of_9.md)Move current ship: Apply acceleration to ship's speed as a one-off  
Moving| [Subroutine: MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)Move current ship: Rotate ship's location by our pitch and roll  
Moving| [Subroutine: MVEIT (Part 6 of 9)](../main/subroutine/mveit_part_6_of_9.md)Move current ship: Move the ship in space according to our speed  
Moving| [Subroutine: MVEIT (Part 7 of 9)](../main/subroutine/mveit_part_7_of_9.md)Move current ship: Rotate ship's orientation vectors by pitch/roll  
Moving| [Subroutine: MVEIT (Part 8 of 9)](../main/subroutine/mveit_part_8_of_9.md)Move current ship: Rotate ship about itself by its own pitch/roll  
Moving| [Subroutine: MVEIT (Part 9 of 9)](../main/subroutine/mveit_part_9_of_9.md)Move current ship: Redraw on scanner, if it hasn't been destroyed  
Moving| [Subroutine: MVT1](../main/subroutine/mvt1.md)Calculate (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (A R)  
Moving| [Subroutine: MVS4](../main/subroutine/mvs4.md)Apply pitch and roll to an orientation vector  
Moving| [Subroutine: MVT6](../main/subroutine/mvt6.md)Calculate (A P+2 P+1) = (x_sign x_hi x_lo) + (A P+2 P+1)  
Moving| [Subroutine: MV40](../main/subroutine/mv40.md)Rotate the planet or sun's location in space by the amount of pitch and roll of our ship  
Flight| [Subroutine: PLUT](../main/subroutine/plut.md)Flip the coordinate axes for the four different views  
Flight| [Subroutine: LOOK1](../main/subroutine/look1.md)Initialise the space view  
Flight| [Subroutine: SIGHT](../main/subroutine/sight.md)Draw the laser crosshairs  
Drawing lines| [Variable: sightcol](../main/variable/sightcol.md)Colours for the crosshair sights on the different laser types  
Drawing lines| [Variable: beamcol](../main/variable/beamcol.md)An unused table of laser colours  
Missions| [Variable: TRIBTA](../main/variable/tribta.md)A table for converting the number of Trumbles in the hold into a number of sprites in the range 0 to 6  
Missions| [Variable: TRIBMA](../main/variable/tribma.md)A table for converting the number of Trumbles in the hold into a sprite-enable flag to use with VIC register &15  
Drawing the screen| [Subroutine: TT66](../main/subroutine/tt66.md)Clear the screen and set the current view type  
Drawing the screen| [Subroutine: TTX66K](../main/subroutine/ttx66k.md)Clear the top part of the screen, draw a border box and configure the specified view  
Keyboard| [Variable: TRTB%](../main/variable/trtb_per_cent.md)Translation table from internal key number to ASCII  
Keyboard| [Variable: IKNS](../main/variable/ikns.md)Lookup table for in-flight keyboard controls  
Keyboard| [Subroutine: FILLKL](../main/subroutine/fillkl.md)Scan the keyboard for a flight key and update the key logger  
Keyboard| [Subroutine: CTRL](../main/subroutine/ctrl.md)Scan the keyboard to see if CTRL is currently pressed  
Keyboard| [Subroutine: DKS5](../main/subroutine/dks5.md)Scan the keyboard to see if a specific key is being pressed  
Keyboard| [Subroutine: ZEKTRAN](../main/subroutine/zektran.md)Clear the key logger  
Keyboard| [Subroutine: RDKEY](../main/subroutine/rdkey.md)Scan the keyboard for key presses and update the key logger  
Keyboard| [Subroutine: RDFIRE](../main/subroutine/rdfire.md)Read the fire button on either the analogue or digital joystick  
Keyboard| [Subroutine: RDJOY](../main/subroutine/rdjoy.md)Read from either the analogue or digital joystick  
Keyboard| [Subroutine: TT17X](../main/subroutine/tt17x.md)Scan the digital joystick for movement  
Tactics| [Subroutine: yetanotherrts](../main/subroutine/yetanotherrts.md)Contains an RTS  
Dashboard| [Subroutine: ECMOF](../main/subroutine/ecmof.md)Switch off the E.C.M. and turn off the dashboard bulb  
Tactics| [Subroutine: SFRMIS](../main/subroutine/sfrmis.md)Add an enemy missile to our local bubble of universe  
Status| [Subroutine: EXNO2](../main/subroutine/exno2.md)Process us making a kill  
Sound| [Subroutine: EXNO3](../main/subroutine/exno3.md)Make an explosion sound  
Sound| [Subroutine: EXNO](../main/subroutine/exno.md)Make the sound of a laser strike on another ship or a ship explosion  
Save and load| [Subroutine: COLD](../main/subroutine/cold.md)Set the standard BRKV handler for the game  
Loader| [Subroutine: NMIpissoff](../main/subroutine/nmipissoff.md)Acknowledge NMI interrupts and ignore them  
Utility routines| [Variable: F%](../main/variable/f_per_cent.md)Denotes the end of the main game code, from ELITE A to ELITE H  
  
## Game data  
\---------

Category| Details  
---|---  
Loader| [Variable: Dashboard image (Game data)](../game_data/variable/dashboard_image.md)The binary for the dashboard image  
Drawing ships| [Variable: XX21 (Game data)](../game_data/variable/xx21.md)Ship blueprints lookup table  
Drawing ships| [Variable: E% (Game data)](../game_data/variable/e_per_cent.md)Ship blueprints default NEWB flags  
Status| [Variable: KWL% (Game data)](../game_data/variable/kwl_per_cent.md)Fractional number of kills awarded for destroying each type of ship  
Status| [Variable: KWH% (Game data)](../game_data/variable/kwh_per_cent.md)Integer number of kills awarded for destroying each type of ship  
Drawing ships| [Macro: VERTEX (Game data)](../game_data/macro/vertex.md)Macro definition for adding vertices to ship blueprints  
Drawing ships| [Macro: EDGE (Game data)](../game_data/macro/edge.md)Macro definition for adding edges to ship blueprints  
Drawing ships| [Macro: FACE (Game data)](../game_data/macro/face.md)Macro definition for adding faces to ship blueprints  
Drawing ships| [Variable: SHIP_MISSILE (Game data)](../game_data/variable/ship_missile.md)Ship blueprint for a missile  
Drawing ships| [Variable: SHIP_CORIOLIS (Game data)](../game_data/variable/ship_coriolis.md)Ship blueprint for a Coriolis space station  
Drawing ships| [Variable: SHIP_ESCAPE_POD (Game data)](../game_data/variable/ship_escape_pod.md)Ship blueprint for an escape pod  
Drawing ships| [Variable: SHIP_PLATE (Game data)](../game_data/variable/ship_plate.md)Ship blueprint for an alloy plate  
Drawing ships| [Variable: SHIP_CANISTER (Game data)](../game_data/variable/ship_canister.md)Ship blueprint for a cargo canister  
Drawing ships| [Variable: SHIP_BOULDER (Game data)](../game_data/variable/ship_boulder.md)Ship blueprint for a boulder  
Drawing ships| [Variable: SHIP_ASTEROID (Game data)](../game_data/variable/ship_asteroid.md)Ship blueprint for an asteroid  
Drawing ships| [Variable: SHIP_SPLINTER (Game data)](../game_data/variable/ship_splinter.md)Ship blueprint for a splinter  
Drawing ships| [Variable: SHIP_SHUTTLE (Game data)](../game_data/variable/ship_shuttle.md)Ship blueprint for a Shuttle  
Drawing ships| [Variable: SHIP_TRANSPORTER (Game data)](../game_data/variable/ship_transporter.md)Ship blueprint for a Transporter  
Drawing ships| [Variable: SHIP_COBRA_MK_3 (Game data)](../game_data/variable/ship_cobra_mk_3.md)Ship blueprint for a Cobra Mk III  
Drawing ships| [Variable: SHIP_PYTHON (Game data)](../game_data/variable/ship_python.md)Ship blueprint for a Python  
Drawing ships| [Variable: SHIP_BOA (Game data)](../game_data/variable/ship_boa.md)Ship blueprint for a Boa  
Drawing ships| [Variable: SHIP_ANACONDA (Game data)](../game_data/variable/ship_anaconda.md)Ship blueprint for an Anaconda  
Drawing ships| [Variable: SHIP_ROCK_HERMIT (Game data)](../game_data/variable/ship_rock_hermit.md)Ship blueprint for a rock hermit (asteroid)  
Drawing ships| [Variable: SHIP_VIPER (Game data)](../game_data/variable/ship_viper.md)Ship blueprint for a Viper  
Drawing ships| [Variable: SHIP_SIDEWINDER (Game data)](../game_data/variable/ship_sidewinder.md)Ship blueprint for a Sidewinder  
Drawing ships| [Variable: SHIP_MAMBA (Game data)](../game_data/variable/ship_mamba.md)Ship blueprint for a Mamba  
Drawing ships| [Variable: SHIP_KRAIT (Game data)](../game_data/variable/ship_krait.md)Ship blueprint for a Krait  
Drawing ships| [Variable: SHIP_ADDER (Game data)](../game_data/variable/ship_adder.md)Ship blueprint for an Adder  
Drawing ships| [Variable: SHIP_GECKO (Game data)](../game_data/variable/ship_gecko.md)Ship blueprint for a Gecko  
Drawing ships| [Variable: SHIP_COBRA_MK_1 (Game data)](../game_data/variable/ship_cobra_mk_1.md)Ship blueprint for a Cobra Mk I  
Drawing ships| [Variable: SHIP_WORM (Game data)](../game_data/variable/ship_worm.md)Ship blueprint for a Worm  
Drawing ships| [Variable: SHIP_COBRA_MK_3_P (Game data)](../game_data/variable/ship_cobra_mk_3_p.md)Ship blueprint for a Cobra Mk III (pirate)  
Drawing ships| [Variable: SHIP_ASP_MK_2 (Game data)](../game_data/variable/ship_asp_mk_2.md)Ship blueprint for an Asp Mk II  
Drawing ships| [Variable: SHIP_PYTHON_P (Game data)](../game_data/variable/ship_python_p.md)Ship blueprint for a Python (pirate)  
Drawing ships| [Variable: SHIP_FER_DE_LANCE (Game data)](../game_data/variable/ship_fer_de_lance.md)Ship blueprint for a Fer-de-Lance  
Drawing ships| [Variable: SHIP_MORAY (Game data)](../game_data/variable/ship_moray.md)Ship blueprint for a Moray  
Drawing ships| [Variable: SHIP_THARGOID (Game data)](../game_data/variable/ship_thargoid.md)Ship blueprint for a Thargoid mothership  
Drawing ships| [Variable: SHIP_THARGON (Game data)](../game_data/variable/ship_thargon.md)Ship blueprint for a Thargon  
Drawing ships| [Variable: SHIP_CONSTRICTOR (Game data)](../game_data/variable/ship_constrictor.md)Ship blueprint for a Constrictor  
Drawing ships| [Variable: SHIP_COUGAR (Game data)](../game_data/variable/ship_cougar.md)Ship blueprint for a Cougar  
Drawing ships| [Variable: SHIP_DODO (Game data)](../game_data/variable/ship_dodo.md)Ship blueprint for a Dodecahedron ("Dodo") space station  
Text| [Macro: CHAR (Game data)](../game_data/macro/char.md)Macro definition for characters in the recursive token table  
Text| [Macro: TWOK (Game data)](../game_data/macro/twok.md)Macro definition for two-letter tokens in the token table  
Text| [Macro: CONT (Game data)](../game_data/macro/cont.md)Macro definition for control codes in the recursive token table  
Text| [Macro: RTOK (Game data)](../game_data/macro/rtok.md)Macro definition for recursive tokens in the recursive token table  
Text| [Variable: QQ18 (Game data)](../game_data/variable/qq18.md)The recursive token table for tokens 0-148  
Maths (Geometry)| [Variable: SNE (Game data)](../game_data/variable/sne.md)Sine/cosine table  
Maths (Geometry)| [Variable: ACT (Game data)](../game_data/variable/act.md)Arctan table  
Text| [Macro: EJMP (Game data)](../game_data/macro/ejmp.md)Macro definition for jump tokens in the extended token table  
Text| [Macro: ECHR (Game data)](../game_data/macro/echr.md)Macro definition for characters in the extended token table  
Text| [Macro: ETOK (Game data)](../game_data/macro/etok.md)Macro definition for recursive tokens in the extended token table  
Text| [Macro: ETWO (Game data)](../game_data/macro/etwo.md)Macro definition for two-letter tokens in the extended token table  
Text| [Macro: ERND (Game data)](../game_data/macro/ernd.md)Macro definition for random tokens in the extended token table  
Text| [Macro: TOKN (Game data)](../game_data/macro/tokn.md)Macro definition for standard tokens in the extended token table  
Text| [Variable: TKN1 (Game data)](../game_data/variable/tkn1.md)The first extended token table for recursive tokens 0-255 (DETOK)  
Text| [Variable: RUPLA (Game data)](../game_data/variable/rupla.md)System numbers that have extended description overrides  
Text| [Variable: RUGAL (Game data)](../game_data/variable/rugal.md)The criteria for systems with extended description overrides  
Text| [Variable: RUTOK (Game data)](../game_data/variable/rutok.md)The second extended token table for recursive tokens 0-26 (DETOK3)  
  
[Different variants of the BBC Master version](../releases.md "Previous routine")[Loader source](../all/loader.md "Next routine")
