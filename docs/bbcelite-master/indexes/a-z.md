---
title: "A-Z index of the source code in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/indexes/a-z.html"
---

[Source code cross-references](../articles/source_code_cross-references.md "Next routine")

This index contains every subroutine, entry point, variable, workspace and macro that appears in the source code for the BBC Master version of Elite, sorted alphabetically.

  * A
  * B
  * C
  * D
  * E
  * F
  * G
  * H
  * I
  * J
  * K
  * L
  * M
  * N
  * O
  * P
  * Q
  * R
  * S
  * T
  * U
  * V
  * W
  * X
  * Y
  * Z

  
---  
Name| Category| Description  
[ABORT](../main/subroutine/abort.md)| Dashboard| Disarm missiles and update the dashboard indicators  
[ABORT2](../main/subroutine/abort2.md)| Dashboard| Set/unset the lock target for a missile and update the dashboard  
[ACT (Game data)](../game_data/variable/act.md)| Maths (Geometry)| Arctan table  
[ADD](../main/subroutine/add.md)| Maths (Arithmetic)| Calculate (A X) = (A P) + (S R)  
[ADDK](../main/subroutine/addk.md)| Maths (Arithmetic)| Calculate (A X) = (A P) + (S R)  
[alogh](../main/variable/alogh.md)| Maths (Arithmetic)| Binary antilogarithm table  
[ALP1](../main/workspace/zp.md#alp1)| Workspace variable|  Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31  
[ALP2](../main/workspace/zp.md#alp2)| Workspace variable|  Bit 7 of ALP2 = sign of the roll angle in ALPHA  
[ALPHA](../main/workspace/zp.md#alpha)| Workspace variable|  The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31  
[ALTIT](../main/workspace/wp.md#altit)| Workspace variable|  Our altitude above the surface of the planet or sun  
[ANGRY](../main/subroutine/angry.md)| Tactics| Make a ship or station hostile, and if this is a ship then enable the ship's AI and give it a kick of speed  
[ARCTAN](../main/subroutine/arctan.md)| Maths (Geometry)| Calculate A = arctan(P / Q)  
[ARSR1](../main/subroutine/arctan.md)| Maths (Geometry)| Contains an RTS  
[ASH](../main/workspace/zp.md#ash)| Workspace variable|  Aft shield status  
[auto](../main/workspace/wp.md#auto)| Workspace variable|  Docking computer activation status  
[AVL](../main/workspace/wp.md#avl)| Workspace variable|  Market availability in the current system  
[away](../main/subroutine/spblb.md)| Dashboard| Switch main memory back into &3000-&7FFF and return from the subroutine  
[B% (Loader)](../loader/variable/b_per_cent.md)| Drawing the screen| VDU commands for setting the square mode 1 screen  
[backtonormal](../main/subroutine/backtonormal.md)| Utility routines| Disable the keyboard, set the SVN flag to 0, and return with A = 0  
[BAD](../main/subroutine/bad.md)| Status| Calculate how bad we have been  
[BALI](../main/workspace/wp.md#bali)| Workspace variable|  This byte appears to be unused  
[BAY](../main/subroutine/bay.md)| Status| Go to the docking bay (i.e. show the Status Mode screen)  
[BAY2](../main/subroutine/tt219.md)| Market| Jump into the main loop at FRCE, setting the key "pressed" to red key f9 (so we show the Inventory screen)  
[BAYSTEP](../main/subroutine/brp.md)| Missions| Go to the docking bay (i.e. show the Status Mode screen)  
[beamcol](../main/variable/beamcol.md)| Drawing lines| An unused table of laser colours  
[BEEP](../main/subroutine/beep.md)| Sound| Make a short, high beep  
[BEGIN](../main/subroutine/begin.md)| Loader| Initialise the configuration variables and start the game  
[BELL](../main/subroutine/bell.md)| Sound| Make a standard system beep  
[BET1](../main/workspace/zp.md#bet1)| Workspace variable|  The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8  
[BET2](../main/workspace/zp.md#bet2)| Workspace variable|  Bit 7 of BET2 = sign of the pitch angle in BETA  
[BETA](../main/workspace/zp.md#beta)| Workspace variable|  The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8  
[BLINE](../main/subroutine/bline.md)| Drawing circles| Draw a circle segment and add it to the ball line heap  
[BOMB](../main/workspace/wp.md#bomb)| Workspace variable|  Energy bomb  
[BOMBEFF2](../main/subroutine/bombeff2.md)| Drawing lines| Erase the energy bomb zig-zag lightning bolt, make the sound of the energy bomb going off, draw a new bolt and repeat four times  
[BOMBOFF](../main/subroutine/bomboff.md)| Drawing lines| Draw the zig-zag lightning bolt for the energy bomb  
[BOMBON](../main/subroutine/bombon.md)| Drawing lines| Randomise and draw the energy bomb's zig-zag lightning bolt lines  
[BOMBPOS](../main/variable/bombpos.md)| Drawing lines| A set of x-coordinates that are used as the basis for the energy bomb's zig-zag lightning bolt  
[BOMBTBX](../main/variable/bombtbx.md)| Drawing lines| This is where we store the x-coordinates for the energy bomb's zig-zag lightning bolt  
[BOMBTBY](../main/variable/bombtby.md)| Drawing lines| This is where we store the y-coordinates for the energy bomb's zig-zag lightning bolt  
[BOOP](../main/subroutine/boop.md)| Sound| Make a long, low beep  
[BOX](../main/subroutine/ttx66.md)| Drawing the screen| Just draw the border box along the top and sides  
[boxsize](../main/workspace/wp.md#boxsize)| Workspace variable|  This byte appears to be unused  
[BPRNT](../main/subroutine/bprnt.md)| Text| Print a 32-bit number, left-padded to a specific number of digits, with an optional decimal point  
[BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md)| Start and end| Show the "Load New Commander (Y/N)?" screen and start the game  
[BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)| Start and end| Show the "Press Fire or Space, Commander" screen and start the game  
[BRIEF](../main/subroutine/brief.md)| Missions| Start mission 1 and show the mission briefing  
[BRIEF2](../main/subroutine/brief2.md)| Missions| Start mission 2  
[BRIEF3](../main/subroutine/brief3.md)| Missions| Receive the briefing and plans for mission 2  
[BRIS](../main/subroutine/bris.md)| Missions| Clear the screen, display "INCOMING MESSAGE" and wait for 2 seconds  
[BRP](../main/subroutine/brp.md)| Missions| Print an extended token and show the Status Mode screen  
[BRPS](../main/subroutine/debrief.md)| Missions| Print the extended token in A, show the Status Mode screen and return from the subroutine  
[BST](../main/workspace/wp.md#bst)| Workspace variable|  Fuel scoops (BST stands for "barrel status")  
[BSTK](../main/workspace/up.md#bstk)| Workspace variable|  Bitstik configuration setting  
[BUF](../main/workspace/wp.md#buf)| Workspace variable|  The line buffer used by DASC to print justified text  
[BUMP2](../main/subroutine/bump2.md)| Dashboard| Bump up the value of the pitch or roll dashboard indicator  
[c](../main/subroutine/prx.md)| Equipment| Contains an RTS  
[CABTMP](../main/workspace/wp.md#cabtmp)| Workspace variable|  Cabin temperature  
[CASH](../main/workspace/wp.md#cash)| Workspace variable|  Our current cash pot  
[CATF](../main/workspace/up.md#catf)| Workspace variable|  The disc catalogue flag  
[CATS](../main/subroutine/cats.md)| Save and load| Ask for a disc drive number and print a catalogue of that drive  
[CHAR (Game data)](../game_data/macro/char.md)| Text| Macro definition for characters in the recursive token table  
[CHECK](../main/subroutine/check.md)| Save and load| Calculate the checksum for the last saved commander data block  
[CHECK2](../main/subroutine/check2.md)| Save and load| Calculate the third checksum for the last saved commander data block (Commodore 64 and Apple II versions only)  
[CHK](../main/variable/chk.md)| Save and load| First checksum byte for the saved commander data file  
[CHK2](../main/variable/chk2.md)| Save and load| Second checksum byte for the saved commander data file  
[CHKON](../main/subroutine/chkon.md)| Drawing circles| Check whether any part of a circle appears on the extended screen  
[CHPR](../main/subroutine/chpr.md)| Text| Print a character at the text cursor by poking into screen memory  
[CIRCLE](../main/subroutine/circle.md)| Drawing circles| Draw a circle for the planet  
[CIRCLE2](../main/subroutine/circle2.md)| Drawing circles| Draw a circle (for the planet or chart)  
[CLDELAY](../main/subroutine/cldelay.md)| Utility routines| Delay by iterating through 5 * 256 (1280) empty loops  
[CLIP](../main/subroutine/ll145_part_1_of_4.md)| Drawing lines| Another name for LL145  
[CLIP2](../main/subroutine/ll145_part_1_of_4.md)| Drawing lines| Don't initialise the values in SWAP or A  
[cls](../main/subroutine/cls.md)| Drawing the screen| Clear the top part of the screen and draw a border box  
[CLYNS](../main/subroutine/clyns.md)| Drawing the screen| Clear the bottom three text rows of the space view  
[cmn](../main/subroutine/cmn.md)| Status| Print the commander's name  
[cmn-1](../main/subroutine/cmn.md)| Status| Contains an RTS  
[CNT](../main/workspace/zp.md#cnt)| Workspace variable|  Temporary storage, typically used for storing the number of iterations required when looping  
[CNT (Loader)](../loader/variable/cnt.md)| Drawing planets| A counter for use in drawing Saturn's planetary body  
[CNT2](../main/workspace/zp.md#cnt2)| Workspace variable|  Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start  
[CNT2 (Loader)](../loader/variable/cnt2.md)| Drawing planets| A counter for use in drawing Saturn's background stars  
[CNT3 (Loader)](../loader/variable/cnt3.md)| Drawing planets| A counter for use in drawing Saturn's rings  
[cntr](../main/subroutine/cntr.md)| Dashboard| Apply damping to the pitch or roll dashboard indicator  
[COK](../main/workspace/wp.md#cok)| Workspace variable|  Flags used to generate the competition code  
[COL](../main/workspace/zp.md#col)| Workspace variable|  Temporary storage, used to store colour information when drawing pixels in the dashboard  
[COLD](../main/subroutine/cold.md)| Save and load| Set the standard BRKV handler for the game  
[coltabl](../main/variable/coltabl.md)| Drawing ships| Colours for ship explosions  
[COMC](../main/workspace/up.md#comc)| Workspace variable|  The colour of the dot on the compass  
[COMPAS](../main/subroutine/compas.md)| Dashboard| Update the compass  
[COMX](../main/workspace/wp.md#comx)| Workspace variable|  The x-coordinate of the compass dot  
[COMY](../main/workspace/wp.md#comy)| Workspace variable|  The y-coordinate of the compass dot  
[CONT (Game data)](../game_data/macro/cont.md)| Text| Macro definition for control codes in the recursive token table  
[CPIXK](../main/subroutine/cpixk.md)| Drawing pixels| Draw a single-height dash on the dashboard  
[cpl](../main/subroutine/cpl.md)| Universe| Print the selected system name  
[CRGO](../main/workspace/wp.md#crgo)| Workspace variable|  Our ship's cargo capacity  
[crlf](../main/subroutine/crlf.md)| Text| Tab to column 21 and print a colon  
[csh](../main/subroutine/csh.md)| Status| Print the current amount of cash  
[CTLI](../main/variable/ctli.md)| Save and load| The OS command string for cataloguing a disc  
[CTRL](../main/subroutine/ctrl.md)| Keyboard| Scan the keyboard to see if CTRL is currently pressed  
[CTRLmc](../main/subroutine/ctrlmc.md)| Keyboard| Scan the Master Compact keyboard to see if CTRL is currently pressed  
[CTWOS](../main/variable/ctwos.md)| Drawing pixels| Ready-made single-pixel character row bytes for mode 2  
[DAMP](../main/workspace/up.md#damp)| Workspace variable|  Keyboard damping configuration setting  
[DASC](../main/subroutine/tt26.md)| Text| DASC does exactly the same as TT26 and prints a character at the text cursor, with support for verified text in extended tokens  
[Dashboard image (Game data)](../game_data/variable/dashboard_image.md)| Loader| The binary for the dashboard image  
[DCS1](../main/subroutine/dcs1.md)| Flight| Calculate the vector from the ideal docking position to the ship  
[de](../main/workspace/wp.md#de)| Workspace variable|  Equipment destruction flag  
[DEATH](../main/subroutine/death.md)| Start and end| Display the death screen  
[DEATH2](../main/subroutine/death2.md)| Start and end| Reset most of the game and restart from the title screen  
[DEBRIEF](../main/subroutine/debrief.md)| Missions| Finish mission 1  
[DEBRIEF2](../main/subroutine/debrief2.md)| Missions| Finish mission 2  
[DEEOR](../main/subroutine/deeor.md)| Utility routines| Unscramble the main code  
[DEEORS](../main/subroutine/deeors.md)| Utility routines| Decrypt a multi-page block of memory  
[DELAY](../main/subroutine/delay.md)| Utility routines| Wait for a specified time, in 1/50s of a second  
[DELI](../main/variable/deli.md)| Save and load| The OS command string for deleting a file  
[DELT](../main/subroutine/delt.md)| Save and load| Catalogue a disc, ask for a filename to delete, and delete the file  
[DELT-1](../main/subroutine/delt.md)| Save and load| Contains an RTS  
[DELT4](../main/workspace/zp.md#delt4)| Workspace variable|  Our current speed * 64 as a 16-bit value  
[DELTA](../main/workspace/zp.md#delta)| Workspace variable|  Our current speed, in the range 1-40  
[DENGY](../main/subroutine/dengy.md)| Flight| Drain some energy from the energy banks  
[DET1](../main/subroutine/det1.md)| Drawing the screen| Show or hide the dashboard (for when we die)  
[DETOK](../main/subroutine/detok.md)| Text| Print an extended recursive token from the TKN1 token table  
[DETOK2](../main/subroutine/detok2.md)| Text| Print an extended text token (1-255)  
[DETOK3](../main/subroutine/detok3.md)| Text| Print an extended recursive token from the RUTOK token table  
[DFAULT](../main/subroutine/dfault.md)| Start and end| Reset the current commander data block to the last saved commander  
[DFLAG](../main/workspace/up.md#dflag)| Workspace variable|  This byte appears to be unused  
[dialc](../main/workspace/wp.md#dialc)| Workspace variable|  These bytes are unused in this version of Elite  
[dials](../main/workspace/up.md#dials)| Workspace variable|  These bytes appear to be unused  
[DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md)| Dashboard| Update the dashboard: speed indicator  
[DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)| Dashboard| Update the dashboard: pitch and roll indicators  
[DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)| Dashboard| Update the dashboard: four energy banks  
[DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)| Dashboard| Update the dashboard: shields, fuel, laser & cabin temp, altitude  
[DIL](../main/subroutine/dilx.md)| Dashboard| The range of the indicator is 0-16 (for the energy banks)  
[DIL-1](../main/subroutine/dilx.md)| Dashboard| The range of the indicator is 0-32 (for the speed indicator)  
[DIL2](../main/subroutine/dil2.md)| Dashboard| Update the roll or pitch indicator on the dashboard  
[DILX](../main/subroutine/dilx.md)| Dashboard| Update a bar-based indicator on the dashboard  
[DILX+2](../main/subroutine/dilx.md)| Dashboard| The range of the indicator is 0-64 (for the fuel indicator)  
[DIRI](../main/variable/diri.md)| Save and load| The OS command string for changing directory on the Master Compact  
[DISK](../main/workspace/up.md#disk)| Workspace variable|  The configuration setting for toggle key "T", which isn't actually used but is still updated by pressing "T" while the game is paused. This is a configuration option from the Commodore 64 version of Elite that lets you switch between tape and disc  
[distaway](../main/workspace/wp.md#distaway)| Workspace variable|  Used to store the nearest distance of the rotating ship on the title screen  
[DJD](../main/workspace/up.md#djd)| Workspace variable|  Keyboard auto-recentre configuration setting  
[djd1](../main/subroutine/redu2.md)| Dashboard| Auto-recentre the value in X, if keyboard auto-recentre is configured  
[DJOY](../main/subroutine/djoy.md)| Keyboard| Scan the keyboard for cursor key or digital joystick movement  
[DK4](../main/subroutine/dk4.md)| Keyboard| Scan for pause, configuration and secondary flight keys  
[DKCMP](../main/workspace/wp.md#dkcmp)| Workspace variable|  Docking computer  
[DKS3](../main/subroutine/dks3.md)| Keyboard| Toggle a configuration setting and emit a beep  
[DKS4mc](../main/subroutine/dks4mc.md)| Keyboard| Scan the Master Compact keyboard to see if a specific key is being pressed  
[DKS5](../main/subroutine/dks5.md)| Keyboard| Scan the keyboard to see if a specific key is being pressed  
[DL](../main/workspace/zp.md#dl)| Workspace variable|  Vertical sync flag  
[DLY](../main/workspace/wp.md#dly)| Workspace variable|  In-flight message delay  
[dn](../main/subroutine/dn.md)| Market| Print the amount of money we have left in the cash pot, then make a short, high beep and delay for 1 second  
[dn2](../main/subroutine/dn2.md)| Text| Make a short, high beep and delay for 0.5 seconds  
[DNOIZ](../main/workspace/up.md#dnoiz)| Workspace variable|  Sound on/off configuration setting  
[dockEd](../main/subroutine/docked.md)| Flight| Print a message to say there is no hyperspacing allowed inside the station  
[DOCKIT](../main/subroutine/dockit.md)| Flight| Apply docking manoeuvres to the ship in INWK  
[DOENTRY](../main/subroutine/doentry.md)| Flight| Dock at the space station, show the ship hangar and work out any mission progression  
[DOEXP](../main/subroutine/doexp.md)| Drawing ships| Draw an exploding ship  
[DOKEY](../main/subroutine/dokey.md)| Keyboard| Scan for the seven primary flight controls and apply the docking computer manoeuvring code  
[dontclip](../main/workspace/zp.md#dontclip)| Workspace variable|  This is set to 0 in the RES2 routine, but the value is never actually read (this is left over from the Commodore 64 version of Elite)  
[DORND](../main/subroutine/dornd.md)| Maths (Arithmetic)| Generate random numbers  
[DORND (Loader)](../loader/subroutine/dornd.md)| Maths (Arithmetic)| Generate random numbers  
[DORND2](../main/subroutine/dornd.md)| Maths (Arithmetic)| Make sure the C flag doesn't affect the outcome  
[DOT](../main/subroutine/dot.md)| Dashboard| Draw a dash on the compass  
[DOVDU19](../main/subroutine/dovdu19.md)| Drawing the screen| Change the mode 1 palette  
[DOXC](../main/subroutine/doxc.md)| Text| Move the text cursor to a specific column  
[DOYC](../main/subroutine/doyc.md)| Text| Move the text cursor to a specific row  
[DTEN](../main/subroutine/detok.md)| Text| Print recursive token number X from the token table pointed to by (A V), used to print tokens from the RUTOK table via calls to DETOK3  
[DTS](../main/subroutine/detok2.md)| Text| Print a single letter in the correct case  
[DTW1](../main/variable/dtw1.md)| Text| A mask for applying the lower case part of Sentence Case to extended text tokens  
[DTW2](../main/variable/dtw2.md)| Text| A flag that indicates whether we are currently printing a word  
[DTW3](../main/variable/dtw3.md)| Text| A flag for switching between standard and extended text tokens  
[DTW4](../main/variable/dtw4.md)| Text| Flags that govern how justified extended text tokens are printed  
[DTW5](../main/variable/dtw5.md)| Text| The size of the justified text buffer at BUF  
[DTW6](../main/variable/dtw6.md)| Text| A flag to denote whether printing in lower case is enabled for extended text tokens  
[DTW8](../main/variable/dtw8.md)| Text| A mask for capitalising the next letter in an extended text token  
[DV41](../main/subroutine/dv41.md)| Maths (Arithmetic)| Calculate (P R) = 256 * DELTA / A  
[DV42](../main/subroutine/dv42.md)| Maths (Arithmetic)| Calculate (P R) = 256 * DELTA / z_hi  
[DVID3B2](../main/subroutine/dvid3b2.md)| Maths (Arithmetic)| Calculate K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)  
[DVID4](../main/subroutine/dvid4.md)| Maths (Arithmetic)| Calculate (P R) = 256 * A / Q  
[DVID4K](../main/subroutine/dvid4k.md)| Maths (Arithmetic)| Calculate (P R) = 256 * A / Q  
[DVIDT](../main/subroutine/dvidt.md)| Maths (Arithmetic)| Calculate (P+1 A) = (A P) / Q  
[DVLOIN](../main/subroutine/dvloin.md)| Drawing lines| Draw a vertical line from (A, GCYT) to (A, GCYB)  
[E% (Game data)](../game_data/variable/e_per_cent.md)| Drawing ships| Ship blueprints default NEWB flags  
[ECBLB](../main/subroutine/ecblb.md)| Dashboard| Light up the E.C.M. indicator bulb ("E") on the dashboard  
[ECBLB2](../main/subroutine/ecblb2.md)| Dashboard| Start up the E.C.M. (light up the indicator, start the countdown and make the E.C.M. sound)  
[ECBT](../main/variable/ecbt.md)| Dashboard| The character bitmap for the E.C.M. indicator bulb  
[ECHR (Game data)](../game_data/macro/echr.md)| Text| Macro definition for characters in the extended token table  
[ECM](../main/workspace/wp.md#ecm)| Workspace variable|  E.C.M. system  
[ECMA](../main/workspace/zp.md#ecma)| Workspace variable|  The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating:  
[ECMOF](../main/subroutine/ecmof.md)| Dashboard| Switch off the E.C.M. and turn off the dashboard bulb  
[ECMP](../main/workspace/wp.md#ecmp)| Workspace variable|  Our E.C.M. status  
[EDGE (Game data)](../game_data/macro/edge.md)| Drawing ships| Macro definition for adding edges to ship blueprints  
[EDGES](../main/subroutine/edges.md)| Drawing lines| Draw a horizontal line given a centre and a half-width  
[ee3](../main/subroutine/ee3.md)| Flight| Print the hyperspace countdown in the top-left of the screen  
[EE51](../main/subroutine/ll9_part_1_of_12.md)| Drawing ships| Remove the current ship from the screen, called from SHPPT before drawing the ship as a point  
[EJMP (Game data)](../game_data/macro/ejmp.md)| Text| Macro definition for jump tokens in the extended token table  
[ELASNO](../main/subroutine/elasno.md)| Sound| Make the sound of us being hit  
[Elite loader (Loader)](../loader/subroutine/elite_loader.md)| Loader| Perform a number of OS calls, check for sideways RAM, load and move the main game data, and load and run the main game code  
[ENERGY](../main/workspace/zp.md#energy)| Workspace variable|  Energy bank status  
[ENGY](../main/workspace/wp.md#engy)| Workspace variable|  Energy unit  
[eq](../main/subroutine/eq.md)| Equipment| Subtract the price of equipment from the cash pot  
[EQSHP](../main/subroutine/eqshp.md)| Equipment| Show the Equip Ship screen (red key f3)  
[ERND (Game data)](../game_data/macro/ernd.md)| Text| Macro definition for random tokens in the extended token table  
[err](../main/subroutine/eqshp.md)| Equipment| Beep, pause and go to the docking bay (i.e. show the Status Mode screen)  
[ESCAPE](../main/subroutine/escape.md)| Flight| Launch our escape pod  
[ESCP](../main/workspace/wp.md#escp)| Workspace variable|  Escape pod  
[ETOK (Game data)](../game_data/macro/etok.md)| Text| Macro definition for recursive tokens in the extended token table  
[ETWO (Game data)](../game_data/macro/etwo.md)| Text| Macro definition for two-letter tokens in the extended token table  
[EV](../main/workspace/wp.md#ev)| Workspace variable|  The "extra vessels" spawning counter  
[ex](../main/subroutine/ex.md)| Text| Print a recursive token  
[exlook](../main/variable/exlook.md)| Drawing ships| A table to shift X left by one place when X is 0 or 1  
[EXNO](../main/subroutine/exno.md)| Sound| Make the sound of a laser strike on another ship or a ship explosion  
[EXNO2](../main/subroutine/exno2.md)| Status| Process us making a kill  
[EXNO3](../main/subroutine/exno3.md)| Sound| Make an explosion sound  
[F%](../main/variable/f_per_cent.md)| Utility routines| Denotes the end of the main game code, from ELITE A to ELITE H  
[FACE (Game data)](../game_data/macro/face.md)| Drawing ships| Macro definition for adding faces to ship blueprints  
[FAROF](../main/subroutine/farof.md)| Maths (Geometry)| Compare x_hi, y_hi and z_hi with 224  
[FAROF2](../main/subroutine/farof2.md)| Maths (Geometry)| Compare x_hi, y_hi and z_hi with A  
[FEED](../main/subroutine/feed.md)| Text| Print a newline  
[FILEPR](../main/subroutine/filepr.md)| Save and load| Display the currently selected media (disk or tape)  
[FILLKL](../main/subroutine/fillkl.md)| Keyboard| Scan the keyboard for a flight key and update the key logger  
[FIST](../main/workspace/wp.md#fist)| Workspace variable|  Our legal status (FIST stands for "fugitive/innocent status"):  
[FLAG](../main/workspace/zp.md#flag)| Workspace variable|  A flag that's used to define whether this is the first call to the ball line routine in BLINE, so it knows whether to wait for the second call before storing segment data in the ball line heap  
[FLFLLS](../main/subroutine/flflls.md)| Drawing suns| Reset the sun line heap  
[FLH](../main/workspace/up.md#flh)| Workspace variable|  Flashing console bars configuration setting  
[FLIP](../main/subroutine/flip.md)| Stardust| Reflect the stardust particles in the screen diagonal and redraw the stardust field  
[FLKB](../main/subroutine/flkb.md)| Keyboard| Flush the keyboard buffer  
[FMLTU](../main/subroutine/fmltu.md)| Maths (Arithmetic)| Calculate A = A * Q / 256  
[FMLTU2](../main/subroutine/fmltu2.md)| Maths (Arithmetic)| Calculate A = K * sin(A)  
[FONT%](../main/variable/font_per_cent.md)| Text| A copy of the character definition bitmap table from the MOS ROM  
[fq1](../main/subroutine/frs1.md)| Tactics| Used to add a cargo canister to the universe  
[FR1](../main/subroutine/fr1.md)| Tactics| Display the "missile jammed" message  
[FR1-2](../main/subroutine/fr1.md)| Tactics| Clear the C flag and return from the subroutine  
[FRCE](../main/subroutine/main_game_loop_part_6_of_6.md)| Main loop| The entry point for the main game loop if we want to jump straight to a specific screen, by pretending to "press" a key, in which case A contains the internal key number of the key we want to "press"  
[FRIN](../main/workspace/wp.md#frin)| Workspace variable|  Slots for the ships in the local bubble of universe  
[FRMIS](../main/subroutine/frmis.md)| Tactics| Fire a missile from our ship  
[FRS1](../main/subroutine/frs1.md)| Tactics| Launch a ship straight ahead of us, below the laser sights  
[frump](../main/workspace/wp.md#frump)| Workspace variable|  Used to store the number of particles in the explosion cloud, though the number is never actually used  
[FSH](../main/workspace/zp.md#fsh)| Workspace variable|  Forward shield status  
[fwl](../main/subroutine/fwl.md)| Status| Print fuel and cash levels  
[G%](../main/variable/g_per_cent.md)| Utility routines| Denotes the start of the main game code, from ELITE A to ELITE H  
[GC2](../main/subroutine/gc2.md)| Maths (Arithmetic)| Calculate (Y X) = (A P) * 4  
[GCASH](../main/subroutine/gcash.md)| Maths (Arithmetic)| Calculate (Y X) = P * Q * 4  
[GCNT](../main/workspace/wp.md#gcnt)| Workspace variable|  The number of the current galaxy (0-7)  
[getzp](../main/subroutine/getzp.md)| Utility routines| Swap zero page (&0090 to &00EF) with the buffer at &3000  
[getzp+3](../main/subroutine/getzp.md)| Utility routines| Restore the top part of zero page, but without first claiming the NMI workspace (Master Compact variant only)  
[Ghy](../main/subroutine/ghy.md)| Flight| Perform a galactic hyperspace jump  
[GHYP](../main/workspace/wp.md#ghyp)| Workspace variable|  Galactic hyperdrive  
[GINF](../main/subroutine/ginf.md)| Universe| Fetch the address of a ship's data block into INF  
[GNTMP](../main/workspace/wp.md#gntmp)| Workspace variable|  Laser temperature (or "gun temperature")  
[gnum](../main/subroutine/gnum.md)| Market| Get a number from the keyboard  
[GOIN](../main/subroutine/main_flight_loop_part_9_of_16.md)| Main loop| We jump here from part 3 of the main flight loop if the docking computer is activated by pressing "C"  
[GOPL](../main/subroutine/tactics_part_3_of_7.md)| Tactics| Make the ship head towards the planet  
[gov](../main/workspace/wp.md#gov)| Workspace variable|  The current system's government type (0-7)  
[GTDIR](../main/subroutine/gtdir.md)| Save and load| Fetch the name of an ADFS directory on the Master Compact and change to that directory  
[GTDRV](../main/subroutine/gtdrv.md)| Save and load| Get an ASCII disc drive number from the keyboard  
[GTHG](../main/subroutine/gthg.md)| Universe| Spawn a Thargoid ship and a Thargon companion  
[GTNMEW](../main/subroutine/gtnmew.md)| Save and load| Fetch the name of a commander file to save or load  
[GVL](../main/subroutine/gvl.md)| Universe| Calculate the availability of market items  
[HA3](../main/subroutine/hanger.md)| Ship hangar| Contains an RTS  
[HAL3](../main/subroutine/hal3.md)| Ship hangar| Draw a hangar background line from left to right, stopping when it bumps into existing on-screen content  
[HALL](../main/subroutine/hall.md)| Ship hangar| Draw the ships in the ship hangar, then draw the hangar  
[HANGER](../main/subroutine/hanger.md)| Ship hangar| Display the ship hangar  
[HANGFLAG](../main/variable/hangflag.md)| Ship hangar| The number of ships being displayed in the ship hangar  
[HAS1](../main/subroutine/has1.md)| Ship hangar| Draw a ship in the ship hangar  
[HAS2](../main/subroutine/has2.md)| Ship hangar| Draw a hangar background line from left to right  
[HAS3](../main/subroutine/has3.md)| Ship hangar| Draw a hangar background line from right to left  
[HATB](../main/variable/hatb.md)| Ship hangar| Ship hangar group table  
[HFS1](../main/subroutine/hfs2.md)| Drawing circles| Don't clear the screen, and draw 8 concentric rings with the step size in STP  
[HFS2](../main/subroutine/hfs2.md)| Drawing circles| Draw the launch or hyperspace tunnel  
[HFX](../main/workspace/wp.md#hfx)| Workspace variable|  A flag that toggles the hyperspace colour effect  
[HI1](../main/subroutine/hitch.md)| Tactics| Contains an RTS  
[HITCH](../main/subroutine/hitch.md)| Tactics| Work out if the ship in INWK is in our crosshairs  
[HLOIN](../main/subroutine/hloin.md)| Drawing lines| Draw a horizontal line from (X1, Y1) to (X2, Y1)  
[HLOIN2](../main/subroutine/hloin2.md)| Drawing lines| Remove a line from the sun line heap and draw it on-screen  
[HLOIN3](../main/subroutine/hloin.md)| Drawing lines| Draw a line from (X, Y1) to (X2, Y1) in the colour given in A  
[hm](../main/subroutine/hm.md)| Charts| Select the closest system and redraw the chart crosshairs  
[HME2](../main/subroutine/hme2.md)| Charts| Search the galaxy for a system  
[hy5](../main/subroutine/jmp.md)| Universe| Contains an RTS  
[hyp](../main/subroutine/hyp.md)| Flight| Start the hyperspace process  
[hyp1](../main/subroutine/hyp1.md)| Universe| Process a jump to the system closest to (QQ9, QQ10)  
[hyp1+3](../main/subroutine/hyp1.md)| Universe| Jump straight to the system at (QQ9, QQ10) without first calculating which system is closest. We do this if we already know that (QQ9, QQ10) points to a system  
[hyR](../main/subroutine/gvl.md)| Universe| Contains an RTS  
[IKNS](../main/variable/ikns.md)| Keyboard| Lookup table for in-flight keyboard controls  
[INCYC](../main/subroutine/incyc.md)| Text| Move the text cursor to the next row  
[INF](../main/workspace/zp.md#inf)| Workspace variable|  Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK  
[INWK](../main/workspace/zp.md#inwk)| Workspace variable|  The zero-page internal workspace for the current ship data block  
[IRQ1](../main/subroutine/irq1.md)| Drawing the screen| The main screen-mode interrupt handler (IRQ1V points here)  
[ITEM](../main/macro/item.md)| Market| Macro definition for the market prices table  
[JAMESON](../main/subroutine/jameson.md)| Save and load| Restore the default JAMESON commander  
[jmp](../main/subroutine/jmp.md)| Universe| Set the current system to the selected system  
[JMTB](../main/variable/jmtb.md)| Text| The extended token table for jump tokens 1-32 (DETOK)  
[JOPOS](../main/workspace/wp.md#jopos)| Workspace variable|  Contains the high byte of the latest value from ADC channel 1 (the joystick X value), which gets updated regularly by the IRQ1 interrupt handler  
[JSTE](../main/workspace/up.md#jste)| Workspace variable|  Reverse both joystick channels configuration setting  
[JSTGY](../main/workspace/up.md#jstgy)| Workspace variable|  Reverse joystick Y-channel configuration setting  
[JSTK](../main/workspace/up.md#jstk)| Workspace variable|  Keyboard or joystick configuration setting  
[JSTX](../main/workspace/zp.md#jstx)| Workspace variable|  Our current roll rate  
[JSTY](../main/workspace/zp.md#jsty)| Workspace variable|  Our current pitch rate  
[JUNK](../main/workspace/wp.md#junk)| Workspace variable|  The amount of junk in the local bubble  
[K](../main/workspace/zp.md#k)| Workspace variable|  Temporary storage, used in a number of places  
[K%](../main/workspace/k_per_cent.md)| Workspaces| Ship data blocks and ship line heaps  
[K2](../main/workspace/zp.md#k2)| Workspace variable|  Temporary storage, used in a number of places  
[K3](../main/workspace/zp.md#k3)| Workspace variable|  Temporary storage, used in a number of places  
[K4](../main/workspace/zp.md#k4)| Workspace variable|  Temporary storage, used in a number of places  
[K5](../main/workspace/zp.md#k5)| Workspace variable|  Temporary storage used to store segment coordinates across successive calls to BLINE, the ball line routine  
[K6](../main/workspace/zp.md#k6)| Workspace variable|  Temporary storage, typically used for storing coordinates during vector calculations  
[KILLSHP](../main/subroutine/killshp.md)| Universe| Remove a ship from our local bubble of universe  
[KL](../main/workspace/zp.md#kl)| Workspace variable|  The following bytes implement a key logger that enables Elite to scan for concurrent key presses of the primary flight keys, plus a secondary flight key  
[KS1](../main/subroutine/ks1.md)| Universe| Remove the current ship from our local bubble of universe  
[KS2](../main/subroutine/ks2.md)| Universe| Check the local bubble for missiles with target lock  
[KS3](../main/subroutine/ks3.md)| Universe| Set the SLSP ship line heap pointer after shuffling ship slots  
[KS4](../main/subroutine/ks4.md)| Universe| Remove the space station and replace it with the sun  
[KTRAN](../main/variable/ktran.md)| Keyboard| An unused key logger buffer that's left over from the 6502 Second Processor version of Elite  
[KWH% (Game data)](../game_data/variable/kwh_per_cent.md)| Status| Integer number of kills awarded for destroying each type of ship  
[KWL% (Game data)](../game_data/variable/kwl_per_cent.md)| Status| Fractional number of kills awarded for destroying each type of ship  
[KY1](../main/workspace/zp.md#ky1)| Workspace variable|  "?" is being pressed (slow down)  
[KY12](../main/workspace/zp.md#ky12)| Workspace variable|  TAB is being pressed (energy bomb)  
[KY13](../main/workspace/zp.md#ky13)| Workspace variable|  ESCAPE is being pressed (launch escape pod)  
[KY14](../main/workspace/zp.md#ky14)| Workspace variable|  "T" is being pressed (target missile)  
[KY15](../main/workspace/zp.md#ky15)| Workspace variable|  "U" is being pressed (unarm missile)  
[KY16](../main/workspace/zp.md#ky16)| Workspace variable|  "M" is being pressed (fire missile)  
[KY17](../main/workspace/zp.md#ky17)| Workspace variable|  "E" is being pressed (activate E.C.M.)  
[KY18](../main/workspace/zp.md#ky18)| Workspace variable|  "J" is being pressed (in-system jump)  
[KY19](../main/workspace/zp.md#ky19)| Workspace variable|  "C" is being pressed (activate docking computer)  
[KY2](../main/workspace/zp.md#ky2)| Workspace variable|  Space is being pressed (speed up)  
[KY20](../main/workspace/zp.md#ky20)| Workspace variable|  "P" is being pressed (deactivate docking computer)  
[KY3](../main/workspace/zp.md#ky3)| Workspace variable|  "<" is being pressed (roll left)  
[KY4](../main/workspace/zp.md#ky4)| Workspace variable|  ">" is being pressed (roll right)  
[KY5](../main/workspace/zp.md#ky5)| Workspace variable|  "X" is being pressed (pull up)  
[KY6](../main/workspace/zp.md#ky6)| Workspace variable|  "S" is being pressed (pitch down)  
[KY7](../main/workspace/zp.md#ky7)| Workspace variable|  "A" is being pressed (fire lasers)  
[LAS](../main/workspace/zp.md#las)| Workspace variable|  Contains the laser power of the laser fitted to the current space view (or 0 if there is no laser fitted to the current view)  
[LAS2](../main/workspace/wp.md#las2)| Workspace variable|  Laser power for the current laser  
[LASCT](../main/workspace/wp.md#lasct)| Workspace variable|  The laser pulse count for the current laser  
[LASER](../main/workspace/wp.md#laser)| Workspace variable|  The specifications of the lasers fitted to each of the four space views:  
[LASLI](../main/subroutine/lasli.md)| Drawing lines| Draw the laser lines for when we fire our lasers  
[LASLI-1](../main/subroutine/lasli.md)| Drawing lines| Contains an RTS  
[LASLI2](../main/subroutine/lasli.md)| Drawing lines| Just draw the current laser lines without moving the centre point, draining energy or heating up. This has the effect of removing the lines from the screen  
[LASNO](../main/subroutine/lasno.md)| Sound| Make the sound of our laser firing  
[LASX](../main/workspace/wp.md#lasx)| Workspace variable|  The x-coordinate of the tip of the laser line  
[LASY](../main/workspace/wp.md#lasy)| Workspace variable|  The y-coordinate of the tip of the laser line  
[LATCH (Loader)](../loader/workspace/zp.md#latch)| Workspace variable|  The RAM copy of the currently selected paged ROM/RAM in SHEILA &30  
[LAUN](../main/subroutine/laun.md)| Drawing circles| Make the launch sound and draw the launch tunnel  
[LCASH](../main/subroutine/lcash.md)| Maths (Arithmetic)| Subtract an amount of cash from the cash pot  
[LL10-1](../main/subroutine/ll9_part_2_of_12.md)| Drawing ships| Contains an RTS  
[LL118](../main/subroutine/ll118.md)| Drawing lines| Move a point along a line until it is on-screen  
[LL118-1](../main/subroutine/ll118.md)| Drawing lines| Contains an RTS  
[LL120](../main/subroutine/ll120.md)| Maths (Arithmetic)| Calculate (Y X) = (S x1_lo) * XX12+2 or (S x1_lo) / XX12+2  
[LL121](../main/subroutine/ll123.md)| Maths (Arithmetic)| Calculate (Y X) = (S R) / Q and set the sign to the opposite of the top byte on the stack  
[LL122](../main/subroutine/ll120.md)| Maths (Arithmetic)| Calculate (Y X) = (S R) * Q and set the sign to the opposite of the top byte on the stack  
[LL123](../main/subroutine/ll123.md)| Maths (Arithmetic)| Calculate (Y X) = (S R) / XX12+2 or (S R) * XX12+2  
[LL128](../main/subroutine/ll123.md)| Maths (Arithmetic)| Contains an RTS  
[LL129](../main/subroutine/ll129.md)| Maths (Arithmetic)| Calculate Q = XX12+2, A = S EOR XX12+3 and (S R) = |S R|  
[LL133](../main/subroutine/ll123.md)| Maths (Arithmetic)| Negate (Y X) and return from the subroutine  
[LL145 (Part 1 of 4)](../main/subroutine/ll145_part_1_of_4.md)| Drawing lines| Clip line: Work out which end-points are on-screen, if any  
[LL145 (Part 2 of 4)](../main/subroutine/ll145_part_2_of_4.md)| Drawing lines| Clip line: Work out if any part of the line is on-screen  
[LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md)| Drawing lines| Clip line: Calculate the line's gradient  
[LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)| Drawing lines| Clip line: Call the routine in LL188 to do the actual clipping  
[LL164](../main/subroutine/ll164.md)| Drawing circles| Make the hyperspace sound and draw the hyperspace tunnel  
[LL28](../main/subroutine/ll28.md)| Maths (Arithmetic)| Calculate R = 256 * A / Q  
[LL28+4](../main/subroutine/ll28.md)| Maths (Arithmetic)| Skips the A >= Q check and always returns with C flag cleared, so this can be called if we know the division will work  
[LL31](../main/subroutine/ll28.md)| Maths (Arithmetic)| Skips the A >= Q check and does not set the R counter, so this can be used for jumping straight into the division loop if R is already set to 254 and we know the division will work  
[LL38](../main/subroutine/ll38.md)| Maths (Arithmetic)| Calculate (S A) = (S R) + (A Q)  
[LL5](../main/subroutine/ll5.md)| Maths (Arithmetic)| Calculate Q = SQRT(R Q)  
[LL51](../main/subroutine/ll51.md)| Maths (Geometry)| Calculate the dot product of XX15 and XX16  
[LL61](../main/subroutine/ll61.md)| Maths (Arithmetic)| Calculate (U R) = 256 * A / Q  
[LL62](../main/subroutine/ll62.md)| Maths (Arithmetic)| Calculate 128 - (U R)  
[LL66](../main/subroutine/ll9_part_8_of_12.md)| Drawing ships| A re-entry point into the ship-drawing routine, used by the LL62 routine to store 128 - (U R) on the XX3 heap  
[LL70+1](../main/subroutine/ll9_part_8_of_12.md)| Drawing ships| Contains an RTS (as the first byte of an LDA instruction)  
[LL81+2](../main/subroutine/ll9_part_11_of_12.md)| Drawing ships| Draw the contents of the ship line heap, used to draw the ship as a dot from SHPPT  
[LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)| Drawing ships| Draw ship: Check if ship is exploding, check if ship is in front  
[LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md)| Drawing ships| Draw ship: Check if ship is in field of view, close enough to draw  
[LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)| Drawing ships| Draw ship: Set up orientation vector, ship coordinate variables  
[LL9 (Part 4 of 12)](../main/subroutine/ll9_part_4_of_12.md)| Drawing ships| Draw ship: Set visibility for exploding ship (all faces visible)  
[LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)| Drawing ships| Draw ship: Calculate the visibility of each of the ship's faces  
[LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)| Drawing ships| Draw ship: Calculate the visibility of each of the ship's vertices  
[LL9 (Part 7 of 12)](../main/subroutine/ll9_part_7_of_12.md)| Drawing ships| Draw ship: Calculate the visibility of each of the ship's vertices  
[LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)| Drawing ships| Draw ship: Calculate the screen coordinates of visible vertices  
[LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)| Drawing ships| Draw ship: Draw laser beams if the ship is firing its laser at us  
[LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)| Drawing ships| Draw ship: Calculate the visibility of each of the ship's edges and draw the visible ones using flicker-free animation  
[LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)| Drawing ships| Draw ship: Loop back for the next edge  
[LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md)| Drawing ships| Draw ship: Draw all the visible edges from the ship line heap  
[LO2](../main/subroutine/look1.md)| Flight| Contains an RTS  
[LOD](../main/subroutine/lod.md)| Save and load| Load a commander file  
[lodosc](../main/variable/lodosc.md)| Save and load| The OS command string for loading a commander file  
[log](../main/variable/log.md)| Maths (Arithmetic)| Binary logarithm table (high byte)  
[logL](../main/variable/logl.md)| Maths (Arithmetic)| Binary logarithm table (low byte)  
[LOIN](../main/subroutine/loin.md)| Drawing lines| Draw a one-segment line  
[LOINQ](../main/subroutine/loinq_part_1_of_7.md)| Drawing lines| Draw a one-segment line from (X1, Y1) to (X2, Y2)  
[LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)| Drawing lines| Draw a line: Calculate the line gradient in the form of deltas  
[LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)| Drawing lines| Draw a line: Line has a shallow gradient, step right along x-axis  
[LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)| Drawing lines| Draw a shallow line going right and up or left and down  
[LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)| Drawing lines| Draw a shallow line going right and down or left and up  
[LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)| Drawing lines| Draw a line: Line has a steep gradient, step up along y-axis  
[LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)| Drawing lines| Draw a steep line going up and left or down and right  
[LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)| Drawing lines| Draw a steep line going up and right or down and left  
[LOOK1](../main/subroutine/look1.md)| Flight| Initialise the space view  
[LOR](../main/subroutine/lod.md)| Save and load| Set the C flag and return from the subroutine  
[LSC3](../main/subroutine/ll9_part_12_of_12.md)| Drawing ships| Contains an RTS  
[LSCLR](../main/subroutine/ll9_part_12_of_12.md)| Drawing ships| Draw any remaining lines from the old ship that are still in the ship line heap  
[LSNUM](../main/workspace/zp.md#lsnum)| Workspace variable|  The pointer to the current position in the ship line heap as we work our way through the new ship's edges (and the corresponding old ship's edges) when drawing the ship in the main ship-drawing routine at LL9  
[LSNUM2](../main/workspace/zp.md#lsnum2)| Workspace variable|  The size of the existing ship line heap for the ship we are drawing in LL9, i.e. the number of lines in the old ship that is currently shown on-screen and which we need to erase  
[LSO](../main/workspace/wp.md#lso)| Workspace variable|  The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)  
[LSP](../main/workspace/zp.md#lsp)| Workspace variable|  The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps  
[LSPUT](../main/subroutine/lsput.md)| Drawing lines| Draw a ship line using flicker-free animation  
[LSX](../main/workspace/zp.md#lsx)| Workspace variable|  LSX contains the status of the sun line heap at LSO  
[LSX2](../main/workspace/wp.md#lsx2)| Workspace variable|  The ball line heap for storing x-coordinates  
[LSY2](../main/workspace/wp.md#lsy2)| Workspace variable|  The ball line heap for storing y-coordinates  
[m](../main/subroutine/mas2.md)| Maths (Geometry)| Do not include A in the calculation  
[M%](../main/subroutine/main_flight_loop_part_1_of_16.md)| Main loop| The entry point for the main flight loop  
[MA9](../main/subroutine/mas1.md)| Maths (Geometry)| Contains an RTS  
[MAD](../main/subroutine/mad.md)| Maths (Arithmetic)| Calculate (A X) = Q * A + (S R)  
[Main flight loop (Part 1 of 16)](../main/subroutine/main_flight_loop_part_1_of_16.md)| Main loop| Seed the random number generator  
[Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)| Main loop| Calculate the alpha and beta angles from the current pitch and roll of our ship  
[Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)| Main loop| Scan for flight keys and process the results  
[Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)| Main loop| For each nearby ship: Copy the ship's data block from K% to the zero-page workspace at INWK  
[Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)| Main loop| For each nearby ship: If an energy bomb has been set off, potentially kill this ship  
[Main flight loop (Part 6 of 16)](../main/subroutine/main_flight_loop_part_6_of_16.md)| Main loop| For each nearby ship: Move the ship in space and copy the updated INWK data block back to K%  
[Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md)| Main loop| For each nearby ship: Check whether we are docking, scooping or colliding with it  
[Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md)| Main loop| For each nearby ship: Process us potentially scooping this item  
[Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md)| Main loop| For each nearby ship: If it is a space station, check whether we are successfully docking with it  
[Main flight loop (Part 10 of 16)](../main/subroutine/main_flight_loop_part_10_of_16.md)| Main loop| For each nearby ship: Remove if scooped, or process collisions  
[Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)| Main loop| For each nearby ship: Process missile lock and firing our laser  
[Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)| Main loop| For each nearby ship: Draw the ship, remove if killed, loop back  
[Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)| Main loop| Show energy bomb effect, charge shields and energy banks  
[Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md)| Main loop| Spawn a space station if we are close enough to the planet  
[Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)| Main loop| Perform altitude checks with the planet and sun and process fuel scooping if appropriate  
[Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)| Main loop| Process laser pulsing, E.C.M. energy drain, call stardust routine  
[Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md)| Main loop| Spawn a trader (a Cobra Mk III, Python, Boa or Anaconda)  
[Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)| Main loop| Call the main flight loop, and potentially spawn a trader, an asteroid, or a cargo canister  
[Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md)| Main loop| Potentially spawn a cop, particularly if we've been bad  
[Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)| Main loop| Potentially spawn a lone bounty hunter, a Thargoid, or up to four pirates  
[Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md)| Main loop| Cool down lasers, make calls to update the dashboard  
[Main game loop (Part 6 of 6)](../main/subroutine/main_game_loop_part_6_of_6.md)| Main loop| Process non-flight key presses (red function keys, docked keys)  
[MAL1](../main/subroutine/main_flight_loop_part_4_of_16.md)| Main loop| Marks the beginning of the ship analysis loop, so we can jump back here from part 12 of the main flight loop to work our way through each ship in the local bubble. We also jump back here when a ship is removed from the bubble, so we can continue processing from the next ship  
[MANY](../main/workspace/wp.md#many)| Workspace variable|  The number of ships of each type in the local bubble of universe  
[MAS1](../main/subroutine/mas1.md)| Maths (Geometry)| Add an orientation vector coordinate to an INWK coordinate  
[MAS2](../main/subroutine/mas2.md)| Maths (Geometry)| Calculate a cap on the maximum distance to the planet or sun  
[MAS3](../main/subroutine/mas3.md)| Maths (Arithmetic)| Calculate A = x_hi^2 + y_hi^2 + z_hi^2 in the K% block  
[MAS4](../main/subroutine/mas4.md)| Maths (Geometry)| Calculate a cap on the maximum distance to a ship  
[MCASH](../main/subroutine/mcash.md)| Maths (Arithmetic)| Add an amount of cash to the cash pot  
[MCH](../main/workspace/wp.md#mch)| Workspace variable|  The text token number of the in-flight message that is currently being shown, and which will be removed by the me2 routine when the counter in DLY reaches zero  
[MCNT](../main/workspace/zp.md#mcnt)| Workspace variable|  The main loop counter  
[me1](../main/subroutine/me1.md)| Flight| Erase an old in-flight message and display a new one  
[me2](../main/subroutine/me2.md)| Flight| Remove an in-flight message from the space view  
[me3](../main/subroutine/main_game_loop_part_2_of_6.md)| Main loop| Used by me2 to jump back into the main game loop after printing an in-flight message  
[mes9](../main/subroutine/mes9.md)| Flight| Print a text token, possibly followed by " DESTROYED"  
[MESS](../main/subroutine/mess.md)| Flight| Display an in-flight message  
[MESS1 (Loader)](../loader/variable/mess1.md)| Loader| The OS command string for loading the BDATA binary  
[MESS2 (Loader)](../loader/variable/mess2.md)| Loader| The OS command string for loading the main game code binary  
[MESS3 (Loader)](../loader/variable/mess3.md)| Loader| The OS command string for changing the disc directory to E  
[messXC](../main/workspace/zp.md#messxc)| Workspace variable|  Temporary storage, used to store the text column of the in-flight message in MESS, so it can be erased from the screen at the correct time  
[MJ](../main/workspace/wp.md#mj)| Workspace variable|  Are we in witchspace (i.e. have we mis-jumped)?  
[MJP](../main/subroutine/mjp.md)| Flight| Process a mis-jump into witchspace  
[MLOOP](../main/subroutine/main_game_loop_part_5_of_6.md)| Main loop| The entry point for the main game loop. This entry point comes after the call to the main flight loop and spawning routines, so it marks the start of the main game loop for when we are docked (as we don't need to call the main flight loop or spawning routines if we aren't in space)  
[MLS1](../main/subroutine/mls1.md)| Maths (Arithmetic)| Calculate (A P) = ALP1 * A  
[MLS2](../main/subroutine/mls2.md)| Maths (Arithmetic)| Calculate (S R) = XX(1 0) and (A P) = A * ALP1  
[MLTU2](../main/subroutine/mltu2.md)| Maths (Arithmetic)| Calculate (A P+1 P) = (A ~P) * Q  
[MLTU2-2](../main/subroutine/mltu2.md)| Maths (Arithmetic)| Set Q to X, so this calculates (A P+1 P) = (A ~P) * X  
[MLU1](../main/subroutine/mlu1.md)| Maths (Arithmetic)| Calculate Y1 = y_hi and (A P) = |y_hi| * Q for Y-th stardust  
[MLU2](../main/subroutine/mlu2.md)| Maths (Arithmetic)| Calculate (A P) = |A| * Q  
[MOS](../main/workspace/zp.md#mos)| Workspace variable|  Determines whether we are running on a Master Compact  
[MOS (Loader)](../loader/workspace/zp.md#mos)| Workspace variable|  Determines whether we are running on a Master Compact  
[MSAR](../main/workspace/wp.md#msar)| Workspace variable|  The targeting state of our leftmost missile  
[MSBAR](../main/subroutine/msbar.md)| Dashboard| Draw a specific indicator in the dashboard's missile bar  
[msblob](../main/subroutine/msblob.md)| Dashboard| Display the dashboard's missile indicators in green  
[mscol](../main/workspace/up.md#mscol)| Workspace variable|  This byte appears to be unused  
[MSTG](../main/workspace/zp.md#mstg)| Workspace variable|  The current missile lock target  
[MT1](../main/subroutine/mt1.md)| Text| Switch to ALL CAPS when printing extended tokens  
[MT13](../main/subroutine/mt13.md)| Text| Switch to lower case when printing extended tokens  
[MT14](../main/subroutine/mt14.md)| Text| Switch to justified text when printing extended tokens  
[MT15](../main/subroutine/mt15.md)| Text| Switch to left-aligned text when printing extended tokens  
[MT16](../main/subroutine/mt16.md)| Text| Print the character in variable DTW7  
[MT17](../main/subroutine/mt17.md)| Text| Print the selected system's adjective, e.g. Lavian for Lave  
[MT18](../main/subroutine/mt18.md)| Text| Print a random 1-8 letter word in Sentence Case  
[MT19](../main/subroutine/mt19.md)| Text| Capitalise the next letter  
[MT2](../main/subroutine/mt2.md)| Text| Switch to Sentence Case when printing extended tokens  
[MT23](../main/subroutine/mt23.md)| Text| Move to row 10, switch to white text, and switch to lower case when printing extended tokens  
[MT26](../main/subroutine/mt26.md)| Text| Fetch a line of text from the keyboard  
[MT27](../main/subroutine/mt27.md)| Text| Print the captain's name during mission briefings  
[MT28](../main/subroutine/mt28.md)| Text| Print the location hint during the mission 1 briefing  
[MT29](../main/subroutine/mt29.md)| Text| Move to row 6, switch to white text, and switch to lower case when printing extended tokens  
[MT5](../main/subroutine/mt5.md)| Text| Switch to extended tokens  
[MT6](../main/subroutine/mt6.md)| Text| Switch to standard tokens in Sentence Case  
[MT8](../main/subroutine/mt8.md)| Text| Tab to column 6 and start a new word when printing extended tokens  
[MT9](../main/subroutine/mt9.md)| Text| Clear the screen and set the current view type to 1  
[MTIN](../main/variable/mtin.md)| Text| Lookup table for random tokens in the extended token table (0-37)  
[MU1](../main/subroutine/mu1.md)| Maths (Arithmetic)| Copy X into P and A, and clear the C flag  
[MU11](../main/subroutine/mu11.md)| Maths (Arithmetic)| Calculate (A P) = P * X  
[MU5](../main/subroutine/mu5.md)| Maths (Arithmetic)| Set K(3 2 1 0) = (A A A A) and clear the C flag  
[MU6](../main/subroutine/mu6.md)| Maths (Arithmetic)| Set P(1 0) = (A A)  
[MULT1](../main/subroutine/mult1.md)| Maths (Arithmetic)| Calculate (A P) = Q * A  
[MULT12](../main/subroutine/mult12.md)| Maths (Arithmetic)| Calculate (S R) = Q * A  
[MULT3](../main/subroutine/mult3.md)| Maths (Arithmetic)| Calculate K(3 2 1 0) = (A P+1 P) * Q  
[MULTS-2](../main/subroutine/mls1.md)| Maths (Arithmetic)| Calculate (A P) = X * A  
[MULTU](../main/subroutine/multu.md)| Maths (Arithmetic)| Calculate (A P) = P * Q  
[MUT1](../main/subroutine/mut1.md)| Maths (Arithmetic)| Calculate R = XX and (A P) = Q * A  
[MUT2](../main/subroutine/mut2.md)| Maths (Arithmetic)| Calculate (S R) = XX(1 0) and (A P) = Q * A  
[MUT3](../main/subroutine/mut3.md)| Maths (Arithmetic)| An unused routine that does the same as MUT2  
[MV40](../main/subroutine/mv40.md)| Moving| Rotate the planet or sun's location in space by the amount of pitch and roll of our ship  
[MV45](../main/subroutine/mveit_part_6_of_9.md)| Moving| Rejoin the MVEIT routine after the rotation, tactics and scanner code  
[MVEIT (Part 1 of 9)](../main/subroutine/mveit_part_1_of_9.md)| Moving| Move current ship: Tidy the orientation vectors  
[MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md)| Moving| Move current ship: Call tactics routine, remove ship from scanner  
[MVEIT (Part 3 of 9)](../main/subroutine/mveit_part_3_of_9.md)| Moving| Move current ship: Move ship forward according to its speed  
[MVEIT (Part 4 of 9)](../main/subroutine/mveit_part_4_of_9.md)| Moving| Move current ship: Apply acceleration to ship's speed as a one-off  
[MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)| Moving| Move current ship: Rotate ship's location by our pitch and roll  
[MVEIT (Part 6 of 9)](../main/subroutine/mveit_part_6_of_9.md)| Moving| Move current ship: Move the ship in space according to our speed  
[MVEIT (Part 7 of 9)](../main/subroutine/mveit_part_7_of_9.md)| Moving| Move current ship: Rotate ship's orientation vectors by pitch/roll  
[MVEIT (Part 8 of 9)](../main/subroutine/mveit_part_8_of_9.md)| Moving| Move current ship: Rotate ship about itself by its own pitch/roll  
[MVEIT (Part 9 of 9)](../main/subroutine/mveit_part_9_of_9.md)| Moving| Move current ship: Redraw on scanner, if it hasn't been destroyed  
[MVS4](../main/subroutine/mvs4.md)| Moving| Apply pitch and roll to an orientation vector  
[MVS5](../main/subroutine/mvs5.md)| Moving| Apply a 3.6 degree pitch or roll to an orientation vector  
[MVT1](../main/subroutine/mvt1.md)| Moving| Calculate (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (A R)  
[MVT1-2](../main/subroutine/mvt1.md)| Moving| Clear bits 0-6 of A before entering MVT1  
[MVT3](../main/subroutine/mvt3.md)| Moving| Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)  
[MVT6](../main/subroutine/mvt6.md)| Moving| Calculate (A P+2 P+1) = (x_sign x_hi x_lo) + (A P+2 P+1)  
[NA%](../main/variable/na_per_cent.md)| Save and load| The data block for the last saved commander  
[NA2%](../main/variable/na2_per_cent.md)| Save and load| The data block for the default commander  
[NAME](../main/workspace/wp.md#name)| Workspace variable|  The current commander name  
[NEWB](../main/workspace/zp.md#newb)| Workspace variable|  The ship's "new byte flags" (or NEWB flags)  
[NEWBRK](../main/subroutine/newbrk.md)| Utility routines| The standard BRKV handler for the game  
[newzp](../main/workspace/zp.md#newzp)| Workspace variable|  This is used by the STARS2 routine for storing the stardust particle's delta_x value  
[NLIN](../main/subroutine/nlin.md)| Drawing lines| Draw a horizontal line at pixel row 23 to box in a title  
[NLIN2](../main/subroutine/nlin2.md)| Drawing lines| Draw a screen-wide horizontal line at the pixel row in A  
[NLIN3](../main/subroutine/nlin3.md)| Drawing lines| Print a title and draw a horizontal line at row 19 to box it in  
[NLIN4](../main/subroutine/nlin4.md)| Drawing lines| Draw a horizontal line at pixel row 19 to box in a title  
[NLIN5](../main/subroutine/nlin.md)| Drawing lines| Move the text cursor down one line before drawing the line  
[NMI](../main/workspace/wp.md#nmi)| Workspace variable|  Used to store the ID of the current owner of the NMI workspace in the NMICLAIM routine, so we can hand it back to them in the NMIRELEASE routine once we are done using it  
[NMICLAIM](../main/subroutine/nmiclaim.md)| Utility routines| Claim the NMI workspace (&00A0 to &00A7) back from the MOS so the game can use it once again  
[NMIpissoff](../main/subroutine/nmipissoff.md)| Loader| Acknowledge NMI interrupts and ignore them  
[NMIRELEASE](../main/subroutine/nmirelease.md)| Utility routines| Release the NMI workspace (&00A0 to &00A7) so the MOS can use it and store the top part of zero page in the buffer at &3000  
[NO1](../main/subroutine/norm.md)| Maths (Geometry)| Contains an RTS  
[NOISE](../main/subroutine/noise.md)| Sound| Make the sound whose number is in Y by populating the sound buffer  
[NOMSL](../main/workspace/wp.md#nomsl)| Workspace variable|  The number of missiles we have fitted (0-4)  
[NORM](../main/subroutine/norm.md)| Maths (Geometry)| Normalise the three-coordinate vector in XX15  
[NOSTM](../main/workspace/zp.md#nostm)| Workspace variable|  The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace  
[NUMBOR](../main/subroutine/numbor.md)| Text| An unused routine that prints a number in hexadecimal  
[NWDAV4](../main/subroutine/nwdav4.md)| Market| Print an "ITEM?" error, make a beep and rejoin the TT210 routine  
[NWDAVxx](../main/subroutine/tt210.md)| Market| Used to rejoin this routine from the call to NWDAV4  
[nWq](../main/subroutine/nwq.md)| Stardust| Create a random cloud of stardust  
[NwS1](../main/subroutine/nws1.md)| Universe| Flip the sign and double an INWK byte  
[NWSHP](../main/subroutine/nwshp.md)| Universe| Add a new ship to our local bubble of universe  
[NWSPS](../main/subroutine/nwsps.md)| Universe| Add a new space station to our local bubble of universe  
[NWSTARS](../main/subroutine/nwstars.md)| Stardust| Initialise the stardust field  
[oh](../main/subroutine/spin.md)| Universe| Contains an RTS  
[oldlong](../main/variable/oldlong.md)| Save and load| Contains the length of the last saved commander name  
[OOPS](../main/subroutine/oops.md)| Flight| Take some damage  
[orange](../main/variable/orange.md)| Drawing pixels| Lookup table for two-pixel mode 1 orange pixels for the sun  
[OSB (Loader)](../loader/subroutine/osb.md)| Utility routines| A convenience routine for calling OSBYTE with Y = 0  
[OTHERFILEPR](../main/subroutine/otherfilepr.md)| Save and load| Display the non-selected media (disk or tape)  
[ou2](../main/subroutine/ou2.md)| Flight| Display "E.C.M.SYSTEM DESTROYED" as an in-flight message  
[ou3](../main/subroutine/ou3.md)| Flight| Display "FUEL SCOOPS DESTROYED" as an in-flight message  
[OUCH](../main/subroutine/ouch.md)| Flight| Potentially lose cargo or equipment following damage  
[out](../main/subroutine/tt217.md)| Keyboard| Contains an RTS  
[OUT](../main/subroutine/gnum.md)| Market| The OUTK routine jumps back here after printing the key that was just pressed  
[OUTK](../main/subroutine/outk.md)| Text| Print the character in Q before returning to gnum  
[P](../main/workspace/zp.md#p)| Workspace variable|  Temporary storage, used in a number of places  
[P (Loader)](../loader/workspace/zp.md#p)| Workspace variable|  Temporary storage, used in a number of places  
[PAS1](../main/subroutine/pas1.md)| Missions| Display a rotating ship at space coordinates (0, 120, 256) and scan the keyboard  
[PATG](../main/workspace/up.md#patg)| Workspace variable|  Configuration setting to show the author names on the start-up screen and enable manual hyperspace mis-jumps  
[PAUSE](../main/subroutine/pause.md)| Missions| Display a rotating ship, waiting until a key is pressed, then remove the ship from the screen  
[PAUSE2](../main/subroutine/pause2.md)| Keyboard| Wait until a key is pressed, ignoring any existing key press  
[PDESC](../main/subroutine/pdesc.md)| Universe| Print the system's extended description or a mission 1 directive  
[ping](../main/subroutine/ping.md)| Universe| Set the selected system to the current system  
[PIX (Loader)](../loader/subroutine/pix.md)| Drawing pixels| Draw a single pixel at a specific coordinate  
[PIX1](../main/subroutine/pix1.md)| Maths (Arithmetic)| Calculate (YY+1 SYL+Y) = (A P) + (S R) and draw stardust particle  
[PIXEL](../main/subroutine/pixel.md)| Drawing pixels| Draw a one-pixel dot, two-pixel dash or four-pixel square  
[PIXEL2](../main/subroutine/pixel2.md)| Drawing pixels| Draw a stardust particle relative to the screen centre  
[PL2](../main/subroutine/pl2.md)| Drawing planets| Remove the planet or sun from the screen  
[PL2-1](../main/subroutine/pl2.md)| Drawing planets| Contains an RTS  
[PL21](../main/subroutine/pl21.md)| Drawing planets| Return from a planet/sun-drawing routine with a failure flag  
[PL44](../main/subroutine/pls6.md)| Drawing planets| Clear the C flag and return from the subroutine  
[PL6](../main/subroutine/pls6.md)| Drawing planets| Contains an RTS  
[PL9 (Part 1 of 3)](../main/subroutine/pl9_part_1_of_3.md)| Drawing planets| Draw the planet, with either an equator and meridian, or a crater  
[PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md)| Drawing planets| Draw the planet's equator and meridian  
[PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)| Drawing planets| Draw the planet's crater  
[PLANET](../main/subroutine/planet.md)| Drawing planets| Draw the planet or sun  
[plf](../main/subroutine/plf.md)| Text| Print a text token followed by a newline  
[plf2](../main/subroutine/plf2.md)| Text| Print text followed by a newline and indent of 6 characters  
[PLL1 (Part 1 of 3) (Loader)](../loader/subroutine/pll1_part_1_of_3.md)| Drawing planets| Draw Saturn on the loading screen (draw the planet)  
[PLL1 (Part 2 of 3) (Loader)](../loader/subroutine/pll1_part_2_of_3.md)| Drawing planets| Draw Saturn on the loading screen (draw the stars)  
[PLL1 (Part 3 of 3) (Loader)](../loader/subroutine/pll1_part_3_of_3.md)| Drawing planets| Draw Saturn on the loading screen (draw the rings)  
[PLS1](../main/subroutine/pls1.md)| Drawing planets| Calculate (Y A) = nosev_x / z  
[PLS2](../main/subroutine/pls2.md)| Drawing planets| Draw a half-ellipse  
[PLS22](../main/subroutine/pls22.md)| Drawing planets| Draw an ellipse or half-ellipse  
[PLS3](../main/subroutine/pls3.md)| Drawing planets| Calculate (Y A P) = 222 * roofv_x / z  
[PLS4](../main/subroutine/pls4.md)| Drawing planets| Calculate CNT2 = arctan(P / A) / 4  
[PLS5](../main/subroutine/pls5.md)| Drawing planets| Calculate roofv_x / z and roofv_y / z  
[PLS6](../main/subroutine/pls6.md)| Drawing planets| Calculate (X K) = (A P+1 P) / (z_sign z_hi z_lo)  
[PLUT](../main/subroutine/plut.md)| Flight| Flip the coordinate axes for the four different views  
[pr2](../main/subroutine/pr2.md)| Text| Print an 8-bit number, left-padded to 3 digits, and optional point  
[pr2+2](../main/subroutine/pr2.md)| Text| Print the 8-bit number in X to the number of digits in A  
[pr5](../main/subroutine/pr5.md)| Text| Print a 16-bit number, left-padded to 5 digits, and optional point  
[pr6](../main/subroutine/pr6.md)| Text| Print 16-bit number, left-padded to 5 digits, no point  
[pres](../main/subroutine/eqshp.md)| Equipment| Given an item number A with the item name in recursive token Y, show an error to say that the item is already present, refund the cost of the item, and then beep and exit to the docking bay (i.e. show the Status Mode screen)  
[PROJ](../main/subroutine/proj.md)| Maths (Geometry)| Project the current ship or planet onto the screen  
[prq](../main/subroutine/prq.md)| Text| Print a text token followed by a question mark  
[prq+3](../main/subroutine/prq.md)| Text| Print a question mark  
[prx](../main/subroutine/prx.md)| Equipment| Return the price of a piece of equipment  
[prx-3](../main/subroutine/prx.md)| Equipment| Return the price of the item with number A - 1  
[PRXS](../main/variable/prxs.md)| Equipment| Equipment prices  
[ptg](../main/subroutine/mjp.md)| Flight| Called when the user manually forces a mis-jump  
[PXCL](../main/variable/pxcl.md)| Drawing pixels| A four-colour mode 1 pixel byte that represents a dot's distance  
[PXR1](../main/subroutine/pixel.md)| Drawing pixels| Contains an RTS  
[PZW](../main/subroutine/pzw.md)| Dashboard| Fetch the current dashboard colours, to support flashing  
[PZW2](../main/subroutine/pzw2.md)| Dashboard| Fetch the current dashboard colours for non-striped indicators, to support flashing  
[Q](../main/workspace/zp.md#q)| Workspace variable|  Temporary storage, used in a number of places  
[Q (Loader)](../loader/workspace/zp.md#q)| Workspace variable|  Temporary storage, used in a number of places  
[QQ0](../main/workspace/wp.md#qq0)| Workspace variable|  The current system's galactic x-coordinate (0-256)  
[QQ1](../main/workspace/wp.md#qq1)| Workspace variable|  The current system's galactic y-coordinate (0-256)  
[QQ10](../main/workspace/zp.md#qq10)| Workspace variable|  The galactic y-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic y-coordinate)  
[QQ11](../main/workspace/zp.md#qq11)| Workspace variable|  The type of the current view:  
[QQ12](../main/workspace/zp.md#qq12)| Workspace variable|  Our "docked" status  
[QQ14](../main/workspace/wp.md#qq14)| Workspace variable|  Our current fuel level (0-70)  
[QQ15](../main/workspace/zp.md#qq15)| Workspace variable|  The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart  
[QQ16](../main/variable/qq16.md)| Text| The two-letter token lookup table  
[QQ17](../main/workspace/zp.md#qq17)| Workspace variable|  Contains a number of flags that affect how text tokens are printed, particularly capitalisation:  
[QQ18 (Game data)](../game_data/variable/qq18.md)| Text| The recursive token table for tokens 0-148  
[QQ19](../main/workspace/zp.md#qq19)| Workspace variable|  Temporary storage, used in a number of places  
[QQ2](../main/workspace/wp.md#qq2)| Workspace variable|  The three 16-bit seeds for the current system, i.e. the one we are currently in  
[QQ20](../main/workspace/wp.md#qq20)| Workspace variable|  The contents of our cargo hold  
[QQ21](../main/workspace/wp.md#qq21)| Workspace variable|  The three 16-bit seeds for the current galaxy  
[QQ22](../main/workspace/zp.md#qq22)| Workspace variable|  The two hyperspace countdown counters  
[QQ23](../main/variable/qq23.md)| Market| Market prices table  
[QQ24](../main/workspace/wp.md#qq24)| Workspace variable|  Temporary storage, used to store the current market item's price in routine TT151  
[QQ25](../main/workspace/wp.md#qq25)| Workspace variable|  Temporary storage, used to store the current market item's availability in routine TT151  
[QQ26](../main/workspace/wp.md#qq26)| Workspace variable|  A random value used to randomise market data  
[QQ28](../main/workspace/wp.md#qq28)| Workspace variable|  The current system's economy (0-7)  
[QQ29](../main/workspace/wp.md#qq29)| Workspace variable|  Temporary storage, used in a number of places  
[QQ3](../main/workspace/zp.md#qq3)| Workspace variable|  The selected system's economy (0-7)  
[QQ4](../main/workspace/zp.md#qq4)| Workspace variable|  The selected system's government (0-7)  
[QQ5](../main/workspace/zp.md#qq5)| Workspace variable|  The selected system's tech level (0-14)  
[QQ6](../main/workspace/zp.md#qq6)| Workspace variable|  The selected system's population in billions * 10 (1-71), so the maximum population is 7.1 billion  
[QQ7](../main/workspace/zp.md#qq7)| Workspace variable|  The selected system's productivity in M CR (96-62480)  
[QQ8](../main/workspace/zp.md#qq8)| Workspace variable|  The distance from the current system to the selected system in light years * 10, stored as a 16-bit number  
[QQ9](../main/workspace/zp.md#qq9)| Workspace variable|  The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)  
[QU5](../main/subroutine/br1_part_1_of_2.md)| Start and end| Restart the game using the last saved commander without asking whether to load a new commander file  
[qv](../main/subroutine/qv.md)| Equipment| Print a menu of the four space views, for buying lasers  
[qw](../main/subroutine/qw.md)| Text| Print a recursive token in the range 128-145  
[R](../main/workspace/zp.md#r)| Workspace variable|  Temporary storage, used in a number of places  
[RAND](../main/workspace/zp.md#rand)| Workspace variable|  Four 8-bit seeds for the random number generation system implemented in the DORND routine  
[RAND (Loader)](../loader/variable/rand.md)| Drawing planets| The random number seed used for drawing Saturn  
[RAT](../main/workspace/zp.md#rat)| Workspace variable|  Used to store different signs depending on the current space view, for use in calculating stardust movement  
[RAT2](../main/workspace/zp.md#rat2)| Workspace variable|  Temporary storage, used to store the pitch and roll signs when moving objects and stardust  
[RDFIRE](../main/subroutine/rdfire.md)| Keyboard| Read the fire button on either the analogue or digital joystick  
[RDJOY](../main/subroutine/rdjoy.md)| Keyboard| Read from either the analogue or digital joystick  
[RDKEY](../main/subroutine/rdkey.md)| Keyboard| Scan the keyboard for key presses and update the key logger  
[RDKEY-1](../main/subroutine/rdkey.md)| Keyboard| Only scan the keyboard for valid BCD key numbers  
[RE2+2](../main/subroutine/bump2.md)| Dashboard| Restore A from T and return from the subroutine  
[readdistnce](../main/subroutine/tt111.md)| Universe| Calculate the distance between the system with galactic coordinates (A, QQ15+1) and the system at (QQ0, QQ1), returning the result in QQ8(1 0)  
[REDU2](../main/subroutine/redu2.md)| Dashboard| Reduce the value of the pitch or roll dashboard indicator  
[refund](../main/subroutine/refund.md)| Equipment| Install a new laser, processing a refund if applicable  
[RES2](../main/subroutine/res2.md)| Start and end| Reset a number of flight variables and workspaces  
[RESET](../main/subroutine/reset.md)| Start and end| Reset most variables  
[RETURN](../main/subroutine/return.md)| Keyboard| Scan the keyboard to see if RETURN is currently pressed  
[rfile](../main/subroutine/rfile.md)| Save and load| Load the commander file  
[RLINE](../main/variable/rline.md)| Text| The OSWORD configuration block used to fetch a line of text from the keyboard  
[ROOT (Loader)](../loader/subroutine/root.md)| Maths (Arithmetic)| Calculate ZP = SQRT(ZP(1 0))  
[RR4](../main/subroutine/chpr.md)| Text| Restore the registers and return from the subroutine  
[RTOK (Game data)](../game_data/macro/rtok.md)| Text| Macro definition for recursive tokens in the recursive token table  
[RTS111](../main/subroutine/mjp.md)| Flight| Contains an RTS  
[RTS2](../main/subroutine/sun_part_4_of_4.md)| Drawing suns| Contains an RTS  
[RUGAL (Game data)](../game_data/variable/rugal.md)| Text| The criteria for systems with extended description overrides  
[RUPLA (Game data)](../game_data/variable/rupla.md)| Text| System numbers that have extended description overrides  
[RUTOK (Game data)](../game_data/variable/rutok.md)| Text| The second extended token table for recursive tokens 0-26 (DETOK3)  
[S](../main/workspace/zp.md#s)| Workspace variable|  Temporary storage, used in a number of places  
[S%](../main/subroutine/s_per_cent.md)| Loader| Move code, set up break handler and start the game  
[S1%](../main/variable/s1_per_cent.md)| Save and load| The drive and directory number used when saving or loading a commander file  
[safehouse](../main/workspace/wp.md#safehouse)| Workspace variable|  Backup storage for the seeds for the selected system  
[savosc](../main/variable/savosc.md)| Save and load| The OS command string for saving a commander file  
[SC](../main/workspace/zp.md#sc)| Workspace variable|  Screen address (low byte)  
[scacol](../main/variable/scacol.md)| Drawing ships| Ship colours on the scanner  
[SCALEX](../main/subroutine/scalex.md)| Maths (Geometry)| Scale the x-coordinate in A (leave it unchanged)  
[SCALEY](../main/subroutine/scaley.md)| Maths (Geometry)| Scale the y-coordinate in A to 0.5 * A  
[SCALEY2](../main/subroutine/scaley2.md)| Maths (Geometry)| Scale the y-coordinate in A (leave it unchanged)  
[SCAN](../main/subroutine/scan.md)| Dashboard| Display the current ship on the scanner  
[SCH](../main/workspace/zp.md#sch)| Workspace variable|  Screen address (high byte)  
[SCTBX1](../main/variable/sctbx1.md)| Drawing the screen| Lookup table for converting a pixel x-coordinate to the bit number within the pixel row byte that corresponds to this pixel  
[SCTBX2](../main/variable/sctbx2.md)| Drawing the screen| Lookup table for converting a pixel x-coordinate to the byte number in the pixel row that corresponds to this pixel  
[SESCP](../main/subroutine/sescp.md)| Flight| Spawn an escape pod from the current (parent) ship  
[SETINTS](../main/subroutine/setints.md)| Loader| Set the various vectors, interrupts and timers  
[setzp](../main/subroutine/setzp.md)| Utility routines| Copy the top part of zero page (&0090 to &00FF) into the buffer at &3000  
[SFRMIS](../main/subroutine/sfrmis.md)| Tactics| Add an enemy missile to our local bubble of universe  
[SFS1](../main/subroutine/sfs1.md)| Universe| Spawn a child ship from the current (parent) ship  
[SFS1-2](../main/subroutine/sfs1.md)| Universe| Used to add a missile to the local bubble that that has AI (bit 7 set), is hostile (bit 6 set) and has been launched (bit 0 clear); the target slot number is set to 31, but this is ignored as the hostile flags means we are the target  
[SFS2](../main/subroutine/sfs2.md)| Moving| Move a ship in space along one of the coordinate axes  
[SFXBT](../main/variable/sfxbt.md)| Sound| Sound data block 2  
[SFXFQ](../main/variable/sfxfq.md)| Sound| Sound data block 3  
[SFXPR](../main/variable/sfxpr.md)| Sound| Sound data block 1  
[SFXVC](../main/variable/sfxvc.md)| Sound| Sound data block 4  
[SHD](../main/subroutine/shd.md)| Flight| Charge a shield and drain some energy from the energy banks  
[SHIFT](../main/subroutine/shift.md)| Keyboard| Scan the keyboard to see if SHIFT is currently pressed  
[SHIP_ADDER (Game data)](../game_data/variable/ship_adder.md)| Drawing ships| Ship blueprint for an Adder  
[SHIP_ANACONDA (Game data)](../game_data/variable/ship_anaconda.md)| Drawing ships| Ship blueprint for an Anaconda  
[SHIP_ASP_MK_2 (Game data)](../game_data/variable/ship_asp_mk_2.md)| Drawing ships| Ship blueprint for an Asp Mk II  
[SHIP_ASTEROID (Game data)](../game_data/variable/ship_asteroid.md)| Drawing ships| Ship blueprint for an asteroid  
[SHIP_BOA (Game data)](../game_data/variable/ship_boa.md)| Drawing ships| Ship blueprint for a Boa  
[SHIP_BOULDER (Game data)](../game_data/variable/ship_boulder.md)| Drawing ships| Ship blueprint for a boulder  
[SHIP_CANISTER (Game data)](../game_data/variable/ship_canister.md)| Drawing ships| Ship blueprint for a cargo canister  
[SHIP_COBRA_MK_1 (Game data)](../game_data/variable/ship_cobra_mk_1.md)| Drawing ships| Ship blueprint for a Cobra Mk I  
[SHIP_COBRA_MK_3 (Game data)](../game_data/variable/ship_cobra_mk_3.md)| Drawing ships| Ship blueprint for a Cobra Mk III  
[SHIP_COBRA_MK_3_P (Game data)](../game_data/variable/ship_cobra_mk_3_p.md)| Drawing ships| Ship blueprint for a Cobra Mk III (pirate)  
[SHIP_CONSTRICTOR (Game data)](../game_data/variable/ship_constrictor.md)| Drawing ships| Ship blueprint for a Constrictor  
[SHIP_CORIOLIS (Game data)](../game_data/variable/ship_coriolis.md)| Drawing ships| Ship blueprint for a Coriolis space station  
[SHIP_COUGAR (Game data)](../game_data/variable/ship_cougar.md)| Drawing ships| Ship blueprint for a Cougar  
[SHIP_DODO (Game data)](../game_data/variable/ship_dodo.md)| Drawing ships| Ship blueprint for a Dodecahedron ("Dodo") space station  
[SHIP_ESCAPE_POD (Game data)](../game_data/variable/ship_escape_pod.md)| Drawing ships| Ship blueprint for an escape pod  
[SHIP_FER_DE_LANCE (Game data)](../game_data/variable/ship_fer_de_lance.md)| Drawing ships| Ship blueprint for a Fer-de-Lance  
[SHIP_GECKO (Game data)](../game_data/variable/ship_gecko.md)| Drawing ships| Ship blueprint for a Gecko  
[SHIP_KRAIT (Game data)](../game_data/variable/ship_krait.md)| Drawing ships| Ship blueprint for a Krait  
[SHIP_MAMBA (Game data)](../game_data/variable/ship_mamba.md)| Drawing ships| Ship blueprint for a Mamba  
[SHIP_MISSILE (Game data)](../game_data/variable/ship_missile.md)| Drawing ships| Ship blueprint for a missile  
[SHIP_MORAY (Game data)](../game_data/variable/ship_moray.md)| Drawing ships| Ship blueprint for a Moray  
[SHIP_PLATE (Game data)](../game_data/variable/ship_plate.md)| Drawing ships| Ship blueprint for an alloy plate  
[SHIP_PYTHON (Game data)](../game_data/variable/ship_python.md)| Drawing ships| Ship blueprint for a Python  
[SHIP_PYTHON_P (Game data)](../game_data/variable/ship_python_p.md)| Drawing ships| Ship blueprint for a Python (pirate)  
[SHIP_ROCK_HERMIT (Game data)](../game_data/variable/ship_rock_hermit.md)| Drawing ships| Ship blueprint for a rock hermit (asteroid)  
[SHIP_SHUTTLE (Game data)](../game_data/variable/ship_shuttle.md)| Drawing ships| Ship blueprint for a Shuttle  
[SHIP_SIDEWINDER (Game data)](../game_data/variable/ship_sidewinder.md)| Drawing ships| Ship blueprint for a Sidewinder  
[SHIP_SPLINTER (Game data)](../game_data/variable/ship_splinter.md)| Drawing ships| Ship blueprint for a splinter  
[SHIP_THARGOID (Game data)](../game_data/variable/ship_thargoid.md)| Drawing ships| Ship blueprint for a Thargoid mothership  
[SHIP_THARGON (Game data)](../game_data/variable/ship_thargon.md)| Drawing ships| Ship blueprint for a Thargon  
[SHIP_TRANSPORTER (Game data)](../game_data/variable/ship_transporter.md)| Drawing ships| Ship blueprint for a Transporter  
[SHIP_VIPER (Game data)](../game_data/variable/ship_viper.md)| Drawing ships| Ship blueprint for a Viper  
[SHIP_WORM (Game data)](../game_data/variable/ship_worm.md)| Drawing ships| Ship blueprint for a Worm  
[shpcol](../main/variable/shpcol.md)| Drawing ships| Ship colours  
[SHPPT](../main/subroutine/shppt.md)| Drawing ships| Draw a distant ship as a point rather than a full wireframe  
[SIGHT](../main/subroutine/sight.md)| Flight| Draw the laser crosshairs  
[sightcol](../main/variable/sightcol.md)| Drawing lines| Colours for the crosshair sights on the different laser types  
[SLSP](../main/workspace/wp.md#slsp)| Workspace variable|  The address of the bottom of the ship line heap  
[SNE (Game data)](../game_data/variable/sne.md)| Maths (Geometry)| Sine/cosine table  
[SOCNT](../main/workspace/sound_variables.md#socnt)| Workspace variable|  Sound buffer for sound effect counters  
[SOFH](../main/variable/sofh.md)| Sound| Sound chip data mask for choosing a tone channel in the range 0-2  
[SOFLG](../main/workspace/sound_variables.md#soflg)| Workspace variable|  Sound buffer for sound effect flags  
[SOFLUSH](../main/subroutine/soflush.md)| Sound| Reset the sound buffers and turn off all sound channels  
[SOFRCH](../main/workspace/sound_variables.md#sofrch)| Workspace variable|  Sound buffer for frequency change values  
[SOFRQ](../main/workspace/sound_variables.md#sofrq)| Workspace variable|  Sound buffer for sound effect frequencies  
[SOINT](../main/subroutine/soint.md)| Sound| Process the contents of the sound buffer and send it to the sound chip  
[SOLAR](../main/subroutine/solar.md)| Universe| Set up various aspects of arriving in a new system  
[SOOFF](../main/variable/sooff.md)| Sound| Sound chip data to turn the volume down on all channels and to act as a mask for choosing a tone channel in the range 0-2  
[SOPR](../main/workspace/sound_variables.md#sopr)| Workspace variable|  Sound buffer for sound effect priorities  
[SOS1](../main/subroutine/sos1.md)| Universe| Update the missile indicators, set up the planet data block  
[Sound variables](../main/workspace/sound_variables.md)| Sound| The sound buffer where the data to be sent to the sound chip is processed  
[SOUR1](../main/subroutine/sous1.md)| Sound| Contains an RTS  
[SOUS1](../main/subroutine/sous1.md)| Sound| Write sound data directly to the 76489 sound chip  
[SOVCH](../main/workspace/sound_variables.md#sovch)| Workspace variable|  Sound buffer for the volume change rate  
[SOVOL](../main/workspace/sound_variables.md#sovol)| Workspace variable|  Sound buffer for volume levels  
[SP1](../main/subroutine/sp1.md)| Dashboard| Draw the space station on the compass  
[SP2](../main/subroutine/sp2.md)| Dashboard| Draw a dot on the compass, given the planet/station vector  
[spasto](../main/variable/spasto.md)| Universe| Contains the address of the Coriolis space station's ship blueprint  
[SPBLB](../main/subroutine/spblb.md)| Dashboard| Light up the space station indicator ("S") on the dashboard  
[SPBT](../main/variable/spbt.md)| Dashboard| The bitmap definition for the space station indicator bulb  
[spc](../main/subroutine/spc.md)| Text| Print a text token followed by a space  
[SPIN](../main/subroutine/spin.md)| Universe| Randomly spawn cargo from a destroyed ship  
[SPIN2](../main/subroutine/spin.md)| Universe| Remove any randomness: spawn cargo of a specific type (given in X), and always spawn the number given in A  
[SPMASK](../main/variable/spmask.md)| Missions| Masks for updating sprite bits in VIC+&10 for the top bit of the 9-bit x-coordinates of the Trumble sprites  
[SPS1](../main/subroutine/sps1.md)| Maths (Geometry)| Calculate the vector to the planet and store it in XX15  
[SPS1+1](../main/subroutine/sps1.md)| Maths (Geometry)| A BRK instruction  
[SPS2](../main/subroutine/sps2.md)| Maths (Arithmetic)| Calculate (Y X) = A / 10  
[SPS3](../main/subroutine/sps3.md)| Maths (Geometry)| Copy a space coordinate from the K% block into K3  
[SPS4](../main/subroutine/sps4.md)| Maths (Geometry)| Calculate the vector to the space station  
[SQUA](../main/subroutine/squa.md)| Maths (Arithmetic)| Clear bit 7 of A and calculate (A P) = A * A  
[SQUA2](../main/subroutine/squa2.md)| Maths (Arithmetic)| Calculate (A P) = A * A  
[SQUA2 (Loader)](../loader/subroutine/squa2.md)| Maths (Arithmetic)| Calculate (A P) = A * A  
[SSPR](../main/workspace/wp.md#sspr)| Workspace variable|  "Space station present" flag  
[stackpt](../main/variable/stackpt.md)| Save and load| Temporary storage for the stack pointer when jumping to the break handler at NEWBRK  
[STARS](../main/subroutine/stars.md)| Stardust| The main routine for processing the stardust  
[STARS1](../main/subroutine/stars1.md)| Stardust| Process the stardust for the front view  
[STARS2](../main/subroutine/stars2.md)| Stardust| Process the stardust for the left or right view  
[STARS6](../main/subroutine/stars6.md)| Stardust| Process the stardust for the rear view  
[STATUS](../main/subroutine/status.md)| Status| Show the Status Mode screen (red key f8)  
[STP](../main/workspace/zp.md#stp)| Workspace variable|  The step size for drawing circles  
[SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)| Drawing suns| Draw the sun: Set up all the variables needed to draw the sun  
[SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)| Drawing suns| Draw the sun: Start from the bottom of the screen and erase the old sun line by line  
[SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)| Drawing suns| Draw the sun: Continue to move up the screen, drawing the new sun line by line  
[SUN (Part 4 of 4)](../main/subroutine/sun_part_4_of_4.md)| Drawing suns| Draw the sun: Continue to the top of the screen, erasing the old sun line by line  
[SUNX](../main/workspace/zp.md#sunx)| Workspace variable|  The 16-bit x-coordinate of the vertical centre axis of the sun (which might be off-screen)  
[SVC](../main/workspace/wp.md#svc)| Workspace variable|  The save count  
[SVE](../main/subroutine/sve.md)| Save and load| Display the disk access menu and process saving of commander files  
[SWAP](../main/workspace/wp.md#swap)| Workspace variable|  Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)  
[SWAPPZERO](../main/subroutine/swappzero.md)| Utility routines| An unused routine that swaps bytes in and out of zero page  
[SX](../main/workspace/wp.md#sx)| Workspace variable|  This is where we store the x_hi coordinates for all the stardust particles  
[SXL](../main/workspace/wp.md#sxl)| Workspace variable|  This is where we store the x_lo coordinates for all the stardust particles  
[SY](../main/workspace/wp.md#sy)| Workspace variable|  This is where we store the y_hi coordinates for all the stardust particles  
[SYL](../main/workspace/wp.md#syl)| Workspace variable|  This is where we store the y_lo coordinates for all the stardust particles  
[SZ](../main/workspace/wp.md#sz)| Workspace variable|  This is where we store the z_hi coordinates for all the stardust particles  
[SZL](../main/workspace/wp.md#szl)| Workspace variable|  This is where we store the z_lo coordinates for all the stardust particles  
[t](../main/subroutine/tt217.md)| Keyboard| As TT217 but don't preserve Y, set it to YSAV instead  
[T](../main/workspace/zp.md#t)| Workspace variable|  Temporary storage, used in a number of places  
[T (Loader)](../loader/workspace/zp.md#t)| Workspace variable|  Temporary storage, used in a number of places  
[T1](../main/workspace/zp.md#t1)| Workspace variable|  Temporary storage, used in a number of places  
[T2](../main/workspace/zp.md#t2)| Workspace variable|  This byte appears to be unused  
[T3](../main/workspace/zp.md#t3)| Workspace variable|  This byte appears to be unused  
[T4](../main/workspace/zp.md#t4)| Workspace variable|  This byte appears to be unused  
[T95](../main/subroutine/tt102.md)| Keyboard| Print the distance to the selected system  
[TA151](../main/subroutine/tactics_part_7_of_7.md)| Tactics| Make the ship head towards the planet  
[TA2](../main/subroutine/tas2.md)| Maths (Geometry)| Calculate the length of the vector in XX15 (ignoring the low coordinates), returning it in Q  
[TA9-1](../main/subroutine/tactics_part_7_of_7.md)| Tactics| Contains an RTS  
[TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)| Tactics| Apply tactics: Process missiles, both enemy missiles and our own  
[TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)| Tactics| Apply tactics: Escape pod, station, lone Thargon, safe-zone pirate  
[TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)| Tactics| Apply tactics: Calculate dot product to determine ship's aim  
[TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)| Tactics| Apply tactics: Check energy levels, maybe launch escape pod if low  
[TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)| Tactics| Apply tactics: Consider whether to launch a missile at us  
[TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md)| Tactics| Apply tactics: Consider firing a laser at us, if aim is true  
[TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)| Tactics| Apply tactics: Set pitch, roll, and acceleration  
[tal](../main/subroutine/tal.md)| Universe| Print the current galaxy number  
[TALLY](../main/workspace/wp.md#tally)| Workspace variable|  Our combat rank  
[TALLYL](../main/workspace/wp.md#tallyl)| Workspace variable|  Combat rank fraction  
[TAS1](../main/subroutine/tas1.md)| Maths (Arithmetic)| Calculate K3 = (x_sign x_hi x_lo) - V(1 0)  
[TAS2](../main/subroutine/tas2.md)| Maths (Geometry)| Normalise the three-coordinate vector in K3  
[TAS3](../main/subroutine/tas3.md)| Maths (Geometry)| Calculate the dot product of XX15 and an orientation vector  
[TAS4](../main/subroutine/tas4.md)| Maths (Geometry)| Calculate the dot product of XX15 and one of the space station's orientation vectors  
[TAS6](../main/subroutine/tas6.md)| Maths (Geometry)| Negate the vector in XX15 so it points in the opposite direction  
[TBRIEF](../main/subroutine/tbrief.md)| Missions| Start mission 3  
[tek](../main/workspace/wp.md#tek)| Workspace variable|  The current system's tech level (0-14)  
[TENS](../main/variable/tens.md)| Text| A constant used when printing large numbers in BPRNT  
[TGINT](../main/variable/tgint.md)| Keyboard| The keys used to toggle configuration settings when the game is paused  
[TGT](../main/workspace/zp.md#tgt)| Workspace variable|  Temporary storage, typically used as a target value for counters when drawing explosion clouds and partial circles  
[THERE](../main/subroutine/there.md)| Missions| Check whether we are in the Constrictor's system in mission 1  
[thislong](../main/variable/thislong.md)| Save and load| Contains the length of the most recently entered commander name  
[TIDY](../main/subroutine/tidy.md)| Maths (Geometry)| Orthonormalise the orientation vectors for a ship  
[TIS1](../main/subroutine/tis1.md)| Maths (Arithmetic)| Calculate (A ?) = (-X * A + (S R)) / 96  
[TIS2](../main/subroutine/tis2.md)| Maths (Arithmetic)| Calculate A = A / Q  
[TIS3](../main/subroutine/tis3.md)| Maths (Arithmetic)| Calculate -(nosev_1 * roofv_1 + nosev_2 * roofv_2) / nosev_3  
[TITLE](../main/subroutine/title.md)| Start and end| Display a title screen with a rotating ship and prompt  
[TJ1](../main/subroutine/tt17.md)| Keyboard| Check for cursor key presses and return the combined deltas for the digital joystick and cursor keys (Master Compact only)  
[TKN1 (Game data)](../game_data/variable/tkn1.md)| Text| The first extended token table for recursive tokens 0-255 (DETOK)  
[TKN2](../main/variable/tkn2.md)| Text| The extended two-letter token lookup table  
[tnpr](../main/subroutine/tnpr.md)| Market| Work out if we have space for a specific amount of cargo  
[tnpr1](../main/subroutine/tnpr1.md)| Market| Work out if we have space for one tonne of cargo  
[TOKN (Game data)](../game_data/macro/tokn.md)| Text| Macro definition for standard tokens in the extended token table  
[TP](../main/workspace/wp.md#tp)| Workspace variable|  The current mission status  
[TR1](../main/subroutine/tr1.md)| Save and load| Copy the last saved commander's name from NA% to INWK  
[TRADEMODE](../main/subroutine/trademode.md)| Drawing the screen| Clear the screen and set up a trading screen  
[TRADEMODE2](../main/subroutine/trademode.md)| Drawing the screen| Set the palette for trading screens and switch the current colour to white  
[TRIBBLE](../main/workspace/wp.md#tribble)| Workspace variable|  The number of Trumbles in the cargo hold  
[TRIBDIR](../main/variable/tribdir.md)| Missions| The low byte of the four 16-bit directions in which Trumble sprites can move  
[TRIBDIRH](../main/variable/tribdirh.md)| Missions| The high byte of the four 16-bit directions in which Trumble sprites can move  
[TRIBMA](../main/variable/tribma.md)| Missions| A table for converting the number of Trumbles in the hold into a sprite-enable flag to use with VIC register &15  
[TRIBTA](../main/variable/tribta.md)| Missions| A table for converting the number of Trumbles in the hold into a number of sprites in the range 0 to 6  
[TRNME](../main/subroutine/trnme.md)| Save and load| Copy the last saved commander's name from INWK to NA%  
[TRTB%](../main/variable/trtb_per_cent.md)| Keyboard| Translation table from internal key number to ASCII  
[TT100](../main/subroutine/main_game_loop_part_2_of_6.md)| Main loop| The entry point for the start of the main game loop, which calls the main flight loop and the moves into the spawning routine  
[TT102](../main/subroutine/tt102.md)| Keyboard| Process function key, save key, hyperspace and chart key presses and update the hyperspace counter  
[TT103](../main/subroutine/tt103.md)| Charts| Draw a small set of crosshairs on a chart  
[TT105](../main/subroutine/tt105.md)| Charts| Draw crosshairs on the Short-range Chart, with clipping  
[TT11](../main/subroutine/tt11.md)| Text| Print a 16-bit number, left-padded to n digits, and optional point  
[TT110](../main/subroutine/tt110.md)| Flight| Launch from a station or show the front space view  
[TT111](../main/subroutine/tt111.md)| Universe| Set the current system to the nearest system to a point  
[TT111-1](../main/subroutine/tt111.md)| Universe| Contains an RTS  
[TT113](../main/subroutine/mcash.md)| Maths (Arithmetic)| Contains an RTS  
[TT114](../main/subroutine/tt114.md)| Charts| Display either the Long-range or Short-range Chart  
[TT123](../main/subroutine/tt123.md)| Charts| Move galactic coordinates by a signed delta  
[TT128](../main/subroutine/tt128.md)| Drawing circles| Draw a circle on a chart  
[TT14](../main/subroutine/tt14.md)| Drawing circles| Draw a circle with crosshairs on a chart  
[TT146](../main/subroutine/tt146.md)| Universe| Print the distance to the selected system in light years  
[TT147](../main/subroutine/tt147.md)| Flight| Print an error when a system is out of hyperspace range  
[TT15](../main/subroutine/tt15.md)| Drawing lines| Draw a set of crosshairs  
[TT151](../main/subroutine/tt151.md)| Market| Print the name, price and availability of a market item  
[TT152](../main/subroutine/tt152.md)| Market| Print the unit ("t", "kg" or "g") for a market item  
[TT16](../main/subroutine/tt16.md)| Charts| Move the crosshairs on a chart  
[TT160](../main/subroutine/tt160.md)| Market| Print "t" (for tonne) and a space  
[TT161](../main/subroutine/tt161.md)| Market| Print "kg" (for kilograms)  
[TT162](../main/subroutine/tt162.md)| Text| Print a space  
[TT162+2](../main/subroutine/tt162.md)| Text| Jump to TT27 to print the text token in A  
[TT163](../main/subroutine/tt163.md)| Market| Print the headers for the table of market prices  
[TT167](../main/subroutine/tt167.md)| Market| Show the Market Price screen (red key f7)  
[TT16a](../main/subroutine/tt16a.md)| Market| Print "g" (for grams)  
[TT17](../main/subroutine/tt17.md)| Keyboard| Scan the keyboard for cursor key or joystick movement  
[TT170](../main/subroutine/tt170.md)| Start and end| Main entry point for the Elite game code  
[TT17X](../main/subroutine/tt17x.md)| Keyboard| Scan the digital joystick for movement  
[TT17X-1](../main/subroutine/tt17x.md)| Keyboard| Contains an RTS  
[TT18](../main/subroutine/tt18.md)| Flight| Try to initiate a jump into hyperspace  
[TT180](../main/subroutine/tt123.md)| Charts| Contains an RTS  
[TT20](../main/subroutine/tt20.md)| Universe| Twist the selected system's seeds four times  
[TT208](../main/subroutine/tt208.md)| Market| Show the Sell Cargo screen (red key f2)  
[TT210](../main/subroutine/tt210.md)| Market| Show a list of current cargo in our hold, optionally to sell  
[TT213](../main/subroutine/tt213.md)| Market| Show the Inventory screen (red key f9)  
[TT214](../main/subroutine/tt214.md)| Keyboard| Ask a question with a "Y/N?" prompt and return the response  
[TT217](../main/subroutine/tt217.md)| Keyboard| Scan the keyboard until a key is pressed  
[TT219](../main/subroutine/tt219.md)| Market| Show the Buy Cargo screen (red key f1)  
[TT22](../main/subroutine/tt22.md)| Charts| Show the Long-range Chart (red key f4)  
[TT23](../main/subroutine/tt23.md)| Charts| Show the Short-range Chart (red key f5)  
[TT24](../main/subroutine/tt24.md)| Universe| Calculate system data from the system seeds  
[TT25](../main/subroutine/tt25.md)| Universe| Show the Data on System screen (red key f6)  
[TT26](../main/subroutine/tt26.md)| Text| Print a character at the text cursor, with support for verified text in extended tokens  
[TT27](../main/subroutine/tt27.md)| Text| Print a text token  
[TT41](../main/subroutine/tt41.md)| Text| Print a letter according to Sentence Case  
[TT42](../main/subroutine/tt42.md)| Text| Print a letter in lower case  
[TT43](../main/subroutine/tt43.md)| Text| Print a two-letter token or recursive token 0-95  
[TT44](../main/subroutine/tt42.md)| Text| Jumps to TT26 to print the character in A (used to enable us to use a branch instruction to jump to TT26)  
[TT45](../main/subroutine/tt45.md)| Text| Print a letter in lower case  
[TT46](../main/subroutine/tt46.md)| Text| Print a character and switch to capitals  
[TT48](../main/subroutine/ex.md)| Text| Contains an RTS  
[TT54](../main/subroutine/tt54.md)| Universe| Twist the selected system's seeds  
[TT60](../main/subroutine/tt60.md)| Text| Print a text token and a paragraph break  
[TT66](../main/subroutine/tt66.md)| Drawing the screen| Clear the screen and set the current view type  
[TT67](../main/subroutine/tt67.md)| Text| Print a newline  
[TT67K](../main/subroutine/tt67k.md)| Text| Print a newline  
[TT68](../main/subroutine/tt68.md)| Text| Print a text token followed by a colon  
[TT69](../main/subroutine/tt69.md)| Text| Set Sentence Case and print a newline  
[TT70](../main/subroutine/tt70.md)| Universe| Display "MAINLY " and jump to TT72  
[TT72](../main/subroutine/tt25.md)| Universe| Used by TT70 to re-enter the routine after displaying "MAINLY" for the economy type  
[TT73](../main/subroutine/tt73.md)| Text| Print a colon  
[TT74](../main/subroutine/tt74.md)| Text| Print a character  
[TT81](../main/subroutine/tt81.md)| Universe| Set the selected system's seeds to those of system 0  
[TTX110](../main/subroutine/ttx110.md)| Flight| Set the current system to the nearest system and return to hyp  
[TTX111](../main/subroutine/hyp.md)| Flight| Used to rejoin this routine from the call to TTX110  
[TTX66](../main/subroutine/ttx66.md)| Drawing the screen| Clear the top part of the screen and draw a border box  
[TTX66K](../main/subroutine/ttx66k.md)| Drawing the screen| Clear the top part of the screen, draw a border box and configure the specified view  
[TTX69](../main/subroutine/ttx69.md)| Text| Print a paragraph break  
[TVT1](../main/variable/tvt1.md)| Drawing the screen| Palette data for the mode 2 part of the screen (the dashboard)  
[TVT3](../main/variable/tvt3.md)| Drawing the screen| Palette data for the mode 1 part of the screen (the top part)  
[TWFL](../main/variable/twfl.md)| Drawing pixels| Ready-made character rows for the left end of a horizontal line  
[TWFR](../main/variable/twfr.md)| Drawing pixels| Ready-made character rows for the right end of a horizontal line  
[TWOK (Game data)](../game_data/macro/twok.md)| Text| Macro definition for two-letter tokens in the token table  
[TWOS](../main/variable/twos.md)| Drawing pixels| Ready-made single-pixel character row bytes for mode 1  
[TWOS (Loader)](../loader/variable/twos.md)| Drawing pixels| Ready-made single-pixel character row bytes for mode 1  
[TWOS2](../main/variable/twos2.md)| Drawing pixels| Ready-made double-pixel character row bytes for mode 1  
[TYPE](../main/workspace/zp.md#type)| Workspace variable|  The current ship type  
[U](../main/workspace/zp.md#u)| Workspace variable|  Temporary storage, used in a number of places  
[UNIV](../main/variable/univ.md)| Universe| Table of pointers to the local universe's ship data blocks  
[UP](../main/workspace/up.md)| Workspaces| Configuration variables  
[UPO](../main/workspace/wp.md#upo)| Workspace variable|  This byte appears to be unused  
[UPTOG](../main/workspace/up.md#uptog)| Workspace variable|  The configuration setting for toggle key "U", which isn't actually used but is still updated by pressing "U" while the game is paused. This is a configuration option from the Apple II version of Elite that lets you switch between lower-case and upper-case text  
[V](../main/workspace/zp.md#v)| Workspace variable|  Temporary storage, typically used for storing an address pointer  
[var](../main/subroutine/var.md)| Market| Calculate QQ19+3 = economy * |economic_factor|  
[VCSU1](../main/subroutine/vcsu1.md)| Maths (Arithmetic)| Calculate vector K3(8 0) = [x y z] - coordinates of the sun or space station  
[VCSUB](../main/subroutine/vcsub.md)| Maths (Arithmetic)| Calculate vector K3(8 0) = [x y z] - coordinates in (A V)  
[VEC](../main/variable/vec.md)| Drawing the screen| The original value of the IRQ1 vector  
[VERTEX (Game data)](../game_data/macro/vertex.md)| Drawing ships| Macro definition for adding vertices to ship blueprints  
[VIEW](../main/workspace/wp.md#view)| Workspace variable|  The number of the current space view  
[VOL](../main/workspace/up.md#vol)| Workspace variable|  The volume level for the game's sound effects (0-7)  
[VOWEL](../main/subroutine/vowel.md)| Text| Test whether a character is a vowel  
[VSCAN](../main/variable/vscan.md)| Drawing the screen| Defines the split position in the split-screen mode  
[W](../main/workspace/zp.md#w)| Workspace variable|  Temporary storage, used in a number of places  
[WARP](../main/subroutine/warp.md)| Flight| Perform an in-system jump  
[wfile](../main/subroutine/wfile.md)| Save and load| Save the commander file  
[widget](../main/workspace/zp.md#widget)| Workspace variable|  Temporary storage, used to store the original argument in A in the logarithmic FMLTU and LL28 routines  
[WP](../main/workspace/wp.md)| Workspaces| Ship slots, variables  
[WP1](../main/subroutine/wp1.md)| Drawing planets| Reset the ball line heap  
[WPLS](../main/subroutine/wpls.md)| Drawing suns| Remove the sun from the screen  
[WPLS-1](../main/subroutine/wpls.md)| Drawing suns| Contains an RTS  
[WPLS2](../main/subroutine/wpls2.md)| Drawing planets| Remove the planet from the screen  
[WPSHPS](../main/subroutine/wpshps.md)| Dashboard| Clear the scanner, reset the ball line and sun line heaps  
[WSCAN](../main/subroutine/wscan.md)| Drawing the screen| Implement the #wscn command (wait for the vertical sync)  
[wtable](../main/variable/wtable.md)| Save and load| 6-bit to 7-bit nibble conversion table  
[wW](../main/subroutine/ww.md)| Flight| Start a hyperspace countdown  
[wW2](../main/subroutine/ww.md)| Flight| Start the hyperspace countdown, starting the countdown from the value in A  
[X1](../main/workspace/zp.md#x1)| Workspace variable|  Temporary storage, typically used for x-coordinates in the line-drawing routines  
[X2](../main/workspace/zp.md#x2)| Workspace variable|  Temporary storage, typically used for x-coordinates in the line-drawing routines  
[XC](../main/workspace/zp.md#xc)| Workspace variable|  The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32  
[XP](../main/workspace/wp.md#xp)| Workspace variable|  This byte appears to be unused  
[XSAV](../main/workspace/zp.md#xsav)| Workspace variable|  Temporary storage for saving the value of the X register, used in a number of places  
[XSAV2](../main/workspace/wp.md#xsav2)| Workspace variable|  This byte appears to be unused  
[XX](../main/workspace/zp.md#xx)| Workspace variable|  Temporary storage, typically used for storing a 16-bit x-coordinate  
[XX0](../main/workspace/zp.md#xx0)| Workspace variable|  Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop  
[XX1](../main/workspace/zp.md#xx1)| Workspace variable|  This is an alias for INWK that is used in the main ship-drawing routine at LL9  
[XX12](../main/workspace/zp.md#xx12)| Workspace variable|  Temporary storage for a block of values, used in a number of places  
[XX13](../main/workspace/zp.md#xx13)| Workspace variable|  Temporary storage, typically used in the line-drawing routines  
[XX15](../main/workspace/zp.md#xx15)| Workspace variable|  Temporary storage, typically used for storing screen coordinates in line-drawing routines  
[XX16](../main/workspace/zp.md#xx16)| Workspace variable|  Temporary storage for a block of values, used in a number of places  
[XX17](../main/workspace/zp.md#xx17)| Workspace variable|  Temporary storage, used in BPRNT to store the number of characters to print, and as the edge counter in the main ship-drawing routine  
[XX18](../main/workspace/zp.md#xx18)| Workspace variable|  Temporary storage used to store coordinates in the LL9 ship-drawing routine  
[XX19](../main/workspace/zp.md#xx19)| Workspace variable|  XX19(1 0) shares its location with INWK(34 33), which contains the address of the ship line heap  
[XX2](../main/workspace/zp.md#xx2)| Workspace variable|  Temporary storage, used to store the visibility of the ship's faces during the ship-drawing routine at LL9  
[XX20](../main/workspace/zp.md#xx20)| Workspace variable|  Temporary storage, used in a number of places  
[XX21 (Game data)](../game_data/variable/xx21.md)| Drawing ships| Ship blueprints lookup table  
[XX24](../main/workspace/wp.md#xx24)| Workspace variable|  This byte appears to be unused  
[XX3](../main/workspace/xx3.md)| Workspaces| Temporary storage space for complex calculations  
[XX4](../main/workspace/zp.md#xx4)| Workspace variable|  Temporary storage, used in a number of places  
[Y1](../main/workspace/zp.md#y1)| Workspace variable|  Temporary storage, typically used for y-coordinates in line-drawing routines  
[Y2](../main/workspace/zp.md#y2)| Workspace variable|  Temporary storage, typically used for y-coordinates in line-drawing routines  
[YC](../main/workspace/zp.md#yc)| Workspace variable|  The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23  
[YESNO](../main/subroutine/yesno.md)| Keyboard| Wait until either "Y" or "N" is pressed  
[yetanotherrts](../main/subroutine/yetanotherrts.md)| Tactics| Contains an RTS  
[ylookup](../main/variable/ylookup.md)| Drawing pixels| Lookup table for converting pixel y-coordinate to page number of screen address  
[YP](../main/workspace/wp.md#yp)| Workspace variable|  This byte appears to be unused  
[ypl](../main/subroutine/ypl.md)| Universe| Print the current system name  
[ypl-1](../main/subroutine/ypl.md)| Universe| Contains an RTS  
[YS](../main/workspace/wp.md#ys)| Workspace variable|  This byte appears to be unused  
[YSAV](../main/workspace/zp.md#ysav)| Workspace variable|  Temporary storage for saving the value of the Y register, used in a number of places  
[YSAV2](../main/workspace/wp.md#ysav2)| Workspace variable|  This byte appears to be unused  
[Yx2M1](../main/workspace/zp.md#yx2m1)| Workspace variable|  This is used to store the number of pixel rows in the space view minus 1, which is also the y-coordinate of the bottom pixel row of the space view (it is set to 191 in the RES2 routine)  
[YY](../main/workspace/zp.md#yy)| Workspace variable|  Temporary storage, typically used for storing a 16-bit y-coordinate  
[YY (Loader)](../loader/workspace/zp.md#yy)| Workspace variable|  Temporary storage, used in a number of places  
[Ze](../main/subroutine/ze.md)| Universe| Initialise the INWK workspace to a fairly aggressive ship  
[ZEKTRAN](../main/subroutine/zektran.md)| Keyboard| Clear the key logger  
[ZERO](../main/subroutine/zero.md)| Utility routines| Reset the local bubble of universe and ship status  
[ZES1](../main/subroutine/zes1.md)| Utility routines| Zero-fill the page whose number is in X  
[ZES2](../main/subroutine/zes2.md)| Utility routines| Zero-fill a specific page  
[ZINF](../main/subroutine/zinf.md)| Universe| Reset the INWK workspace and orientation vectors  
[ZP](../main/workspace/zp.md)| Workspaces| Lots of important variables are stored in the zero page workspace as it is quicker and more space-efficient to access memory here  
[ZP (Loader)](../loader/workspace/zp.md)| Workspaces| Important variables used by the loader  
[ZZ](../main/workspace/zp.md#zz)| Workspace variable|  Temporary storage, typically used for distance values  
[zZ+1](../main/subroutine/ghy.md)| Flight| Contains an RTS  
  
[Source code cross-references](../articles/source_code_cross-references.md "Next routine")
