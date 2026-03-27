---
title: "List of all subroutines in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/indexes/subroutines.html"
---

[Source code cross-references](../articles/source_code_cross-references.md "Previous routine")[List of all variables](variables.md "Next routine")

This index contains every subroutine and entry point that appears in the source code for the BBC Master version of Elite, grouped by category. An entry points is a label within a subroutine that is called from outside the subroutine, which typically implements a subset or variation of the functionality of the parent subroutine.

  * Charts
  * Dashboard
  * Drawing circles
  * Drawing lines
  * Drawing pixels
  * Drawing planets
  * Drawing ships
  * Drawing suns
  * Drawing the screen
  * Equipment
  * Flight
  * Keyboard
  * Loader
  * Main loop
  * Market
  * Maths (Arithmetic)
  * Maths (Geometry)
  * Missions
  * Moving
  * Save and load
  * Ship hangar
  * Sound
  * Stardust
  * Start and end
  * Status
  * Tactics
  * Text
  * Universe
  * Utility routines



## Charts  
\------  
  
---  
[hm](../main/subroutine/hm.md)| Select the closest system and redraw the chart crosshairs  
[HME2](../main/subroutine/hme2.md)| Search the galaxy for a system  
[TT103](../main/subroutine/tt103.md)| Draw a small set of crosshairs on a chart  
[TT105](../main/subroutine/tt105.md)| Draw crosshairs on the Short-range Chart, with clipping  
[TT114](../main/subroutine/tt114.md)| Display either the Long-range or Short-range Chart  
[TT123](../main/subroutine/tt123.md)| Move galactic coordinates by a signed delta  
[TT16](../main/subroutine/tt16.md)| Move the crosshairs on a chart  
[TT180](../main/subroutine/tt123.md)| Contains an RTS  
[TT22](../main/subroutine/tt22.md)| Show the Long-range Chart (red key f4)  
[TT23](../main/subroutine/tt23.md)| Show the Short-range Chart (red key f5)  
  
## Dashboard  
\---------  
  
[ABORT](../main/subroutine/abort.md)| Disarm missiles and update the dashboard indicators  
[ABORT2](../main/subroutine/abort2.md)| Set/unset the lock target for a missile and update the dashboard  
[away](../main/subroutine/spblb.md)| Switch main memory back into &3000-&7FFF and return from the subroutine  
[BUMP2](../main/subroutine/bump2.md)| Bump up the value of the pitch or roll dashboard indicator  
[cntr](../main/subroutine/cntr.md)| Apply damping to the pitch or roll dashboard indicator  
[COMPAS](../main/subroutine/compas.md)| Update the compass  
[DIALS (Part 1 of 4)](../main/subroutine/dials_part_1_of_4.md)| Update the dashboard: speed indicator  
[DIALS (Part 2 of 4)](../main/subroutine/dials_part_2_of_4.md)| Update the dashboard: pitch and roll indicators  
[DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)| Update the dashboard: four energy banks  
[DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md)| Update the dashboard: shields, fuel, laser & cabin temp, altitude  
[DIL](../main/subroutine/dilx.md)| The range of the indicator is 0-16 (for the energy banks)  
[DIL-1](../main/subroutine/dilx.md)| The range of the indicator is 0-32 (for the speed indicator)  
[DIL2](../main/subroutine/dil2.md)| Update the roll or pitch indicator on the dashboard  
[DILX](../main/subroutine/dilx.md)| Update a bar-based indicator on the dashboard  
[DILX+2](../main/subroutine/dilx.md)| The range of the indicator is 0-64 (for the fuel indicator)  
[djd1](../main/subroutine/redu2.md)| Auto-recentre the value in X, if keyboard auto-recentre is configured  
[DOT](../main/subroutine/dot.md)| Draw a dash on the compass  
[ECBLB](../main/subroutine/ecblb.md)| Light up the E.C.M. indicator bulb ("E") on the dashboard  
[ECBLB2](../main/subroutine/ecblb2.md)| Start up the E.C.M. (light up the indicator, start the countdown and make the E.C.M. sound)  
[ECMOF](../main/subroutine/ecmof.md)| Switch off the E.C.M. and turn off the dashboard bulb  
[MSBAR](../main/subroutine/msbar.md)| Draw a specific indicator in the dashboard's missile bar  
[msblob](../main/subroutine/msblob.md)| Display the dashboard's missile indicators in green  
[PZW](../main/subroutine/pzw.md)| Fetch the current dashboard colours, to support flashing  
[PZW2](../main/subroutine/pzw2.md)| Fetch the current dashboard colours for non-striped indicators, to support flashing  
[RE2+2](../main/subroutine/bump2.md)| Restore A from T and return from the subroutine  
[REDU2](../main/subroutine/redu2.md)| Reduce the value of the pitch or roll dashboard indicator  
[SCAN](../main/subroutine/scan.md)| Display the current ship on the scanner  
[SP1](../main/subroutine/sp1.md)| Draw the space station on the compass  
[SP2](../main/subroutine/sp2.md)| Draw a dot on the compass, given the planet/station vector  
[SPBLB](../main/subroutine/spblb.md)| Light up the space station indicator ("S") on the dashboard  
[WPSHPS](../main/subroutine/wpshps.md)| Clear the scanner, reset the ball line and sun line heaps  
  
## Drawing circles  
\---------------  
  
[BLINE](../main/subroutine/bline.md)| Draw a circle segment and add it to the ball line heap  
[CHKON](../main/subroutine/chkon.md)| Check whether any part of a circle appears on the extended screen  
[CIRCLE](../main/subroutine/circle.md)| Draw a circle for the planet  
[CIRCLE2](../main/subroutine/circle2.md)| Draw a circle (for the planet or chart)  
[HFS1](../main/subroutine/hfs2.md)| Don't clear the screen, and draw 8 concentric rings with the step size in STP  
[HFS2](../main/subroutine/hfs2.md)| Draw the launch or hyperspace tunnel  
[LAUN](../main/subroutine/laun.md)| Make the launch sound and draw the launch tunnel  
[LL164](../main/subroutine/ll164.md)| Make the hyperspace sound and draw the hyperspace tunnel  
[TT128](../main/subroutine/tt128.md)| Draw a circle on a chart  
[TT14](../main/subroutine/tt14.md)| Draw a circle with crosshairs on a chart  
  
## Drawing lines  
\-------------  
  
[BOMBEFF2](../main/subroutine/bombeff2.md)| Erase the energy bomb zig-zag lightning bolt, make the sound of the energy bomb going off, draw a new bolt and repeat four times  
[BOMBOFF](../main/subroutine/bomboff.md)| Draw the zig-zag lightning bolt for the energy bomb  
[BOMBON](../main/subroutine/bombon.md)| Randomise and draw the energy bomb's zig-zag lightning bolt lines  
[CLIP](../main/subroutine/ll145_part_1_of_4.md)| Another name for LL145  
[CLIP2](../main/subroutine/ll145_part_1_of_4.md)| Don't initialise the values in SWAP or A  
[DVLOIN](../main/subroutine/dvloin.md)| Draw a vertical line from (A, GCYT) to (A, GCYB)  
[EDGES](../main/subroutine/edges.md)| Draw a horizontal line given a centre and a half-width  
[HLOIN](../main/subroutine/hloin.md)| Draw a horizontal line from (X1, Y1) to (X2, Y1)  
[HLOIN2](../main/subroutine/hloin2.md)| Remove a line from the sun line heap and draw it on-screen  
[HLOIN3](../main/subroutine/hloin.md)| Draw a line from (X, Y1) to (X2, Y1) in the colour given in A  
[LASLI](../main/subroutine/lasli.md)| Draw the laser lines for when we fire our lasers  
[LASLI-1](../main/subroutine/lasli.md)| Contains an RTS  
[LASLI2](../main/subroutine/lasli.md)| Just draw the current laser lines without moving the centre point, draining energy or heating up. This has the effect of removing the lines from the screen  
[LL118](../main/subroutine/ll118.md)| Move a point along a line until it is on-screen  
[LL118-1](../main/subroutine/ll118.md)| Contains an RTS  
[LL145 (Part 1 of 4)](../main/subroutine/ll145_part_1_of_4.md)| Clip line: Work out which end-points are on-screen, if any  
[LL145 (Part 2 of 4)](../main/subroutine/ll145_part_2_of_4.md)| Clip line: Work out if any part of the line is on-screen  
[LL145 (Part 3 of 4)](../main/subroutine/ll145_part_3_of_4.md)| Clip line: Calculate the line's gradient  
[LL145 (Part 4 of 4)](../main/subroutine/ll145_part_4_of_4.md)| Clip line: Call the routine in LL188 to do the actual clipping  
[LOIN](../main/subroutine/loin.md)| Draw a one-segment line  
[LOINQ](../main/subroutine/loinq_part_1_of_7.md)| Draw a one-segment line from (X1, Y1) to (X2, Y2)  
[LOINQ (Part 1 of 7)](../main/subroutine/loinq_part_1_of_7.md)| Draw a line: Calculate the line gradient in the form of deltas  
[LOINQ (Part 2 of 7)](../main/subroutine/loinq_part_2_of_7.md)| Draw a line: Line has a shallow gradient, step right along x-axis  
[LOINQ (Part 3 of 7)](../main/subroutine/loinq_part_3_of_7.md)| Draw a shallow line going right and up or left and down  
[LOINQ (Part 4 of 7)](../main/subroutine/loinq_part_4_of_7.md)| Draw a shallow line going right and down or left and up  
[LOINQ (Part 5 of 7)](../main/subroutine/loinq_part_5_of_7.md)| Draw a line: Line has a steep gradient, step up along y-axis  
[LOINQ (Part 6 of 7)](../main/subroutine/loinq_part_6_of_7.md)| Draw a steep line going up and left or down and right  
[LOINQ (Part 7 of 7)](../main/subroutine/loinq_part_7_of_7.md)| Draw a steep line going up and right or down and left  
[LSPUT](../main/subroutine/lsput.md)| Draw a ship line using flicker-free animation  
[NLIN](../main/subroutine/nlin.md)| Draw a horizontal line at pixel row 23 to box in a title  
[NLIN2](../main/subroutine/nlin2.md)| Draw a screen-wide horizontal line at the pixel row in A  
[NLIN3](../main/subroutine/nlin3.md)| Print a title and draw a horizontal line at row 19 to box it in  
[NLIN4](../main/subroutine/nlin4.md)| Draw a horizontal line at pixel row 19 to box in a title  
[NLIN5](../main/subroutine/nlin.md)| Move the text cursor down one line before drawing the line  
[TT15](../main/subroutine/tt15.md)| Draw a set of crosshairs  
  
## Drawing pixels  
\--------------  
  
[CPIXK](../main/subroutine/cpixk.md)| Draw a single-height dash on the dashboard  
[PIX (Loader)](../loader/subroutine/pix.md)| Draw a single pixel at a specific coordinate  
[PIXEL](../main/subroutine/pixel.md)| Draw a one-pixel dot, two-pixel dash or four-pixel square  
[PIXEL2](../main/subroutine/pixel2.md)| Draw a stardust particle relative to the screen centre  
[PXR1](../main/subroutine/pixel.md)| Contains an RTS  
  
## Drawing planets  
\---------------  
  
[PL2](../main/subroutine/pl2.md)| Remove the planet or sun from the screen  
[PL2-1](../main/subroutine/pl2.md)| Contains an RTS  
[PL21](../main/subroutine/pl21.md)| Return from a planet/sun-drawing routine with a failure flag  
[PL44](../main/subroutine/pls6.md)| Clear the C flag and return from the subroutine  
[PL6](../main/subroutine/pls6.md)| Contains an RTS  
[PL9 (Part 1 of 3)](../main/subroutine/pl9_part_1_of_3.md)| Draw the planet, with either an equator and meridian, or a crater  
[PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md)| Draw the planet's equator and meridian  
[PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md)| Draw the planet's crater  
[PLANET](../main/subroutine/planet.md)| Draw the planet or sun  
[PLL1 (Part 1 of 3) (Loader)](../loader/subroutine/pll1_part_1_of_3.md)| Draw Saturn on the loading screen (draw the planet)  
[PLL1 (Part 2 of 3) (Loader)](../loader/subroutine/pll1_part_2_of_3.md)| Draw Saturn on the loading screen (draw the stars)  
[PLL1 (Part 3 of 3) (Loader)](../loader/subroutine/pll1_part_3_of_3.md)| Draw Saturn on the loading screen (draw the rings)  
[PLS1](../main/subroutine/pls1.md)| Calculate (Y A) = nosev_x / z  
[PLS2](../main/subroutine/pls2.md)| Draw a half-ellipse  
[PLS22](../main/subroutine/pls22.md)| Draw an ellipse or half-ellipse  
[PLS3](../main/subroutine/pls3.md)| Calculate (Y A P) = 222 * roofv_x / z  
[PLS4](../main/subroutine/pls4.md)| Calculate CNT2 = arctan(P / A) / 4  
[PLS5](../main/subroutine/pls5.md)| Calculate roofv_x / z and roofv_y / z  
[PLS6](../main/subroutine/pls6.md)| Calculate (X K) = (A P+1 P) / (z_sign z_hi z_lo)  
[WP1](../main/subroutine/wp1.md)| Reset the ball line heap  
[WPLS2](../main/subroutine/wpls2.md)| Remove the planet from the screen  
  
## Drawing ships  
\-------------  
  
[DOEXP](../main/subroutine/doexp.md)| Draw an exploding ship  
[EE51](../main/subroutine/ll9_part_1_of_12.md)| Remove the current ship from the screen, called from SHPPT before drawing the ship as a point  
[LL10-1](../main/subroutine/ll9_part_2_of_12.md)| Contains an RTS  
[LL66](../main/subroutine/ll9_part_8_of_12.md)| A re-entry point into the ship-drawing routine, used by the LL62 routine to store 128 - (U R) on the XX3 heap  
[LL70+1](../main/subroutine/ll9_part_8_of_12.md)| Contains an RTS (as the first byte of an LDA instruction)  
[LL81+2](../main/subroutine/ll9_part_11_of_12.md)| Draw the contents of the ship line heap, used to draw the ship as a dot from SHPPT  
[LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)| Draw ship: Check if ship is exploding, check if ship is in front  
[LL9 (Part 2 of 12)](../main/subroutine/ll9_part_2_of_12.md)| Draw ship: Check if ship is in field of view, close enough to draw  
[LL9 (Part 3 of 12)](../main/subroutine/ll9_part_3_of_12.md)| Draw ship: Set up orientation vector, ship coordinate variables  
[LL9 (Part 4 of 12)](../main/subroutine/ll9_part_4_of_12.md)| Draw ship: Set visibility for exploding ship (all faces visible)  
[LL9 (Part 5 of 12)](../main/subroutine/ll9_part_5_of_12.md)| Draw ship: Calculate the visibility of each of the ship's faces  
[LL9 (Part 6 of 12)](../main/subroutine/ll9_part_6_of_12.md)| Draw ship: Calculate the visibility of each of the ship's vertices  
[LL9 (Part 7 of 12)](../main/subroutine/ll9_part_7_of_12.md)| Draw ship: Calculate the visibility of each of the ship's vertices  
[LL9 (Part 8 of 12)](../main/subroutine/ll9_part_8_of_12.md)| Draw ship: Calculate the screen coordinates of visible vertices  
[LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)| Draw ship: Draw laser beams if the ship is firing its laser at us  
[LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)| Draw ship: Calculate the visibility of each of the ship's edges and draw the visible ones using flicker-free animation  
[LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)| Draw ship: Loop back for the next edge  
[LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md)| Draw ship: Draw all the visible edges from the ship line heap  
[LSC3](../main/subroutine/ll9_part_12_of_12.md)| Contains an RTS  
[LSCLR](../main/subroutine/ll9_part_12_of_12.md)| Draw any remaining lines from the old ship that are still in the ship line heap  
[SHPPT](../main/subroutine/shppt.md)| Draw a distant ship as a point rather than a full wireframe  
  
## Drawing suns  
\------------  
  
[FLFLLS](../main/subroutine/flflls.md)| Reset the sun line heap  
[RTS2](../main/subroutine/sun_part_4_of_4.md)| Contains an RTS  
[SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)| Draw the sun: Set up all the variables needed to draw the sun  
[SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)| Draw the sun: Start from the bottom of the screen and erase the old sun line by line  
[SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md)| Draw the sun: Continue to move up the screen, drawing the new sun line by line  
[SUN (Part 4 of 4)](../main/subroutine/sun_part_4_of_4.md)| Draw the sun: Continue to the top of the screen, erasing the old sun line by line  
[WPLS](../main/subroutine/wpls.md)| Remove the sun from the screen  
[WPLS-1](../main/subroutine/wpls.md)| Contains an RTS  
  
## Drawing the screen  
\------------------  
  
[BOX](../main/subroutine/ttx66.md)| Just draw the border box along the top and sides  
[cls](../main/subroutine/cls.md)| Clear the top part of the screen and draw a border box  
[CLYNS](../main/subroutine/clyns.md)| Clear the bottom three text rows of the space view  
[DET1](../main/subroutine/det1.md)| Show or hide the dashboard (for when we die)  
[DOVDU19](../main/subroutine/dovdu19.md)| Change the mode 1 palette  
[IRQ1](../main/subroutine/irq1.md)| The main screen-mode interrupt handler (IRQ1V points here)  
[TRADEMODE](../main/subroutine/trademode.md)| Clear the screen and set up a trading screen  
[TRADEMODE2](../main/subroutine/trademode.md)| Set the palette for trading screens and switch the current colour to white  
[TT66](../main/subroutine/tt66.md)| Clear the screen and set the current view type  
[TTX66](../main/subroutine/ttx66.md)| Clear the top part of the screen and draw a border box  
[TTX66K](../main/subroutine/ttx66k.md)| Clear the top part of the screen, draw a border box and configure the specified view  
[WSCAN](../main/subroutine/wscan.md)| Implement the #wscn command (wait for the vertical sync)  
  
## Equipment  
\---------  
  
[c](../main/subroutine/prx.md)| Contains an RTS  
[eq](../main/subroutine/eq.md)| Subtract the price of equipment from the cash pot  
[EQSHP](../main/subroutine/eqshp.md)| Show the Equip Ship screen (red key f3)  
[err](../main/subroutine/eqshp.md)| Beep, pause and go to the docking bay (i.e. show the Status Mode screen)  
[pres](../main/subroutine/eqshp.md)| Given an item number A with the item name in recursive token Y, show an error to say that the item is already present, refund the cost of the item, and then beep and exit to the docking bay (i.e. show the Status Mode screen)  
[prx](../main/subroutine/prx.md)| Return the price of a piece of equipment  
[prx-3](../main/subroutine/prx.md)| Return the price of the item with number A - 1  
[qv](../main/subroutine/qv.md)| Print a menu of the four space views, for buying lasers  
[refund](../main/subroutine/refund.md)| Install a new laser, processing a refund if applicable  
  
## Flight  
\------  
  
[DCS1](../main/subroutine/dcs1.md)| Calculate the vector from the ideal docking position to the ship  
[DENGY](../main/subroutine/dengy.md)| Drain some energy from the energy banks  
[dockEd](../main/subroutine/docked.md)| Print a message to say there is no hyperspacing allowed inside the station  
[DOCKIT](../main/subroutine/dockit.md)| Apply docking manoeuvres to the ship in INWK  
[DOENTRY](../main/subroutine/doentry.md)| Dock at the space station, show the ship hangar and work out any mission progression  
[ee3](../main/subroutine/ee3.md)| Print the hyperspace countdown in the top-left of the screen  
[ESCAPE](../main/subroutine/escape.md)| Launch our escape pod  
[Ghy](../main/subroutine/ghy.md)| Perform a galactic hyperspace jump  
[hyp](../main/subroutine/hyp.md)| Start the hyperspace process  
[LO2](../main/subroutine/look1.md)| Contains an RTS  
[LOOK1](../main/subroutine/look1.md)| Initialise the space view  
[me1](../main/subroutine/me1.md)| Erase an old in-flight message and display a new one  
[me2](../main/subroutine/me2.md)| Remove an in-flight message from the space view  
[mes9](../main/subroutine/mes9.md)| Print a text token, possibly followed by " DESTROYED"  
[MESS](../main/subroutine/mess.md)| Display an in-flight message  
[MJP](../main/subroutine/mjp.md)| Process a mis-jump into witchspace  
[OOPS](../main/subroutine/oops.md)| Take some damage  
[ou2](../main/subroutine/ou2.md)| Display "E.C.M.SYSTEM DESTROYED" as an in-flight message  
[ou3](../main/subroutine/ou3.md)| Display "FUEL SCOOPS DESTROYED" as an in-flight message  
[OUCH](../main/subroutine/ouch.md)| Potentially lose cargo or equipment following damage  
[PLUT](../main/subroutine/plut.md)| Flip the coordinate axes for the four different views  
[ptg](../main/subroutine/mjp.md)| Called when the user manually forces a mis-jump  
[RTS111](../main/subroutine/mjp.md)| Contains an RTS  
[SESCP](../main/subroutine/sescp.md)| Spawn an escape pod from the current (parent) ship  
[SHD](../main/subroutine/shd.md)| Charge a shield and drain some energy from the energy banks  
[SIGHT](../main/subroutine/sight.md)| Draw the laser crosshairs  
[TT110](../main/subroutine/tt110.md)| Launch from a station or show the front space view  
[TT147](../main/subroutine/tt147.md)| Print an error when a system is out of hyperspace range  
[TT18](../main/subroutine/tt18.md)| Try to initiate a jump into hyperspace  
[TTX110](../main/subroutine/ttx110.md)| Set the current system to the nearest system and return to hyp  
[TTX111](../main/subroutine/hyp.md)| Used to rejoin this routine from the call to TTX110  
[WARP](../main/subroutine/warp.md)| Perform an in-system jump  
[wW](../main/subroutine/ww.md)| Start a hyperspace countdown  
[wW2](../main/subroutine/ww.md)| Start the hyperspace countdown, starting the countdown from the value in A  
[zZ+1](../main/subroutine/ghy.md)| Contains an RTS  
  
## Keyboard  
\--------  
  
[CTRL](../main/subroutine/ctrl.md)| Scan the keyboard to see if CTRL is currently pressed  
[CTRLmc](../main/subroutine/ctrlmc.md)| Scan the Master Compact keyboard to see if CTRL is currently pressed  
[DJOY](../main/subroutine/djoy.md)| Scan the keyboard for cursor key or digital joystick movement  
[DK4](../main/subroutine/dk4.md)| Scan for pause, configuration and secondary flight keys  
[DKS3](../main/subroutine/dks3.md)| Toggle a configuration setting and emit a beep  
[DKS4mc](../main/subroutine/dks4mc.md)| Scan the Master Compact keyboard to see if a specific key is being pressed  
[DKS5](../main/subroutine/dks5.md)| Scan the keyboard to see if a specific key is being pressed  
[DOKEY](../main/subroutine/dokey.md)| Scan for the seven primary flight controls and apply the docking computer manoeuvring code  
[FILLKL](../main/subroutine/fillkl.md)| Scan the keyboard for a flight key and update the key logger  
[FLKB](../main/subroutine/flkb.md)| Flush the keyboard buffer  
[out](../main/subroutine/tt217.md)| Contains an RTS  
[PAUSE2](../main/subroutine/pause2.md)| Wait until a key is pressed, ignoring any existing key press  
[RDFIRE](../main/subroutine/rdfire.md)| Read the fire button on either the analogue or digital joystick  
[RDJOY](../main/subroutine/rdjoy.md)| Read from either the analogue or digital joystick  
[RDKEY](../main/subroutine/rdkey.md)| Scan the keyboard for key presses and update the key logger  
[RDKEY-1](../main/subroutine/rdkey.md)| Only scan the keyboard for valid BCD key numbers  
[RETURN](../main/subroutine/return.md)| Scan the keyboard to see if RETURN is currently pressed  
[SHIFT](../main/subroutine/shift.md)| Scan the keyboard to see if SHIFT is currently pressed  
[t](../main/subroutine/tt217.md)| As TT217 but don't preserve Y, set it to YSAV instead  
[T95](../main/subroutine/tt102.md)| Print the distance to the selected system  
[TJ1](../main/subroutine/tt17.md)| Check for cursor key presses and return the combined deltas for the digital joystick and cursor keys (Master Compact only)  
[TT102](../main/subroutine/tt102.md)| Process function key, save key, hyperspace and chart key presses and update the hyperspace counter  
[TT17](../main/subroutine/tt17.md)| Scan the keyboard for cursor key or joystick movement  
[TT17X](../main/subroutine/tt17x.md)| Scan the digital joystick for movement  
[TT17X-1](../main/subroutine/tt17x.md)| Contains an RTS  
[TT214](../main/subroutine/tt214.md)| Ask a question with a "Y/N?" prompt and return the response  
[TT217](../main/subroutine/tt217.md)| Scan the keyboard until a key is pressed  
[YESNO](../main/subroutine/yesno.md)| Wait until either "Y" or "N" is pressed  
[ZEKTRAN](../main/subroutine/zektran.md)| Clear the key logger  
  
## Loader  
\------  
  
[BEGIN](../main/subroutine/begin.md)| Initialise the configuration variables and start the game  
[Elite loader (Loader)](../loader/subroutine/elite_loader.md)| Perform a number of OS calls, check for sideways RAM, load and move the main game data, and load and run the main game code  
[NMIpissoff](../main/subroutine/nmipissoff.md)| Acknowledge NMI interrupts and ignore them  
[S%](../main/subroutine/s_per_cent.md)| Move code, set up break handler and start the game  
[SETINTS](../main/subroutine/setints.md)| Set the various vectors, interrupts and timers  
  
## Main loop  
\---------  
  
[FRCE](../main/subroutine/main_game_loop_part_6_of_6.md)| The entry point for the main game loop if we want to jump straight to a specific screen, by pretending to "press" a key, in which case A contains the internal key number of the key we want to "press"  
[GOIN](../main/subroutine/main_flight_loop_part_9_of_16.md)| We jump here from part 3 of the main flight loop if the docking computer is activated by pressing "C"  
[M%](../main/subroutine/main_flight_loop_part_1_of_16.md)| The entry point for the main flight loop  
[Main flight loop (Part 1 of 16)](../main/subroutine/main_flight_loop_part_1_of_16.md)| Seed the random number generator  
[Main flight loop (Part 2 of 16)](../main/subroutine/main_flight_loop_part_2_of_16.md)| Calculate the alpha and beta angles from the current pitch and roll of our ship  
[Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)| Scan for flight keys and process the results  
[Main flight loop (Part 4 of 16)](../main/subroutine/main_flight_loop_part_4_of_16.md)| For each nearby ship: Copy the ship's data block from K% to the zero-page workspace at INWK  
[Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)| For each nearby ship: If an energy bomb has been set off, potentially kill this ship  
[Main flight loop (Part 6 of 16)](../main/subroutine/main_flight_loop_part_6_of_16.md)| For each nearby ship: Move the ship in space and copy the updated INWK data block back to K%  
[Main flight loop (Part 7 of 16)](../main/subroutine/main_flight_loop_part_7_of_16.md)| For each nearby ship: Check whether we are docking, scooping or colliding with it  
[Main flight loop (Part 8 of 16)](../main/subroutine/main_flight_loop_part_8_of_16.md)| For each nearby ship: Process us potentially scooping this item  
[Main flight loop (Part 9 of 16)](../main/subroutine/main_flight_loop_part_9_of_16.md)| For each nearby ship: If it is a space station, check whether we are successfully docking with it  
[Main flight loop (Part 10 of 16)](../main/subroutine/main_flight_loop_part_10_of_16.md)| For each nearby ship: Remove if scooped, or process collisions  
[Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)| For each nearby ship: Process missile lock and firing our laser  
[Main flight loop (Part 12 of 16)](../main/subroutine/main_flight_loop_part_12_of_16.md)| For each nearby ship: Draw the ship, remove if killed, loop back  
[Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)| Show energy bomb effect, charge shields and energy banks  
[Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md)| Spawn a space station if we are close enough to the planet  
[Main flight loop (Part 15 of 16)](../main/subroutine/main_flight_loop_part_15_of_16.md)| Perform altitude checks with the planet and sun and process fuel scooping if appropriate  
[Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)| Process laser pulsing, E.C.M. energy drain, call stardust routine  
[Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md)| Spawn a trader (a Cobra Mk III, Python, Boa or Anaconda)  
[Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md)| Call the main flight loop, and potentially spawn a trader, an asteroid, or a cargo canister  
[Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md)| Potentially spawn a cop, particularly if we've been bad  
[Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)| Potentially spawn a lone bounty hunter, a Thargoid, or up to four pirates  
[Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md)| Cool down lasers, make calls to update the dashboard  
[Main game loop (Part 6 of 6)](../main/subroutine/main_game_loop_part_6_of_6.md)| Process non-flight key presses (red function keys, docked keys)  
[MAL1](../main/subroutine/main_flight_loop_part_4_of_16.md)| Marks the beginning of the ship analysis loop, so we can jump back here from part 12 of the main flight loop to work our way through each ship in the local bubble. We also jump back here when a ship is removed from the bubble, so we can continue processing from the next ship  
[me3](../main/subroutine/main_game_loop_part_2_of_6.md)| Used by me2 to jump back into the main game loop after printing an in-flight message  
[MLOOP](../main/subroutine/main_game_loop_part_5_of_6.md)| The entry point for the main game loop. This entry point comes after the call to the main flight loop and spawning routines, so it marks the start of the main game loop for when we are docked (as we don't need to call the main flight loop or spawning routines if we aren't in space)  
[TT100](../main/subroutine/main_game_loop_part_2_of_6.md)| The entry point for the start of the main game loop, which calls the main flight loop and the moves into the spawning routine  
  
## Market  
\------  
  
[BAY2](../main/subroutine/tt219.md)| Jump into the main loop at FRCE, setting the key "pressed" to red key f9 (so we show the Inventory screen)  
[dn](../main/subroutine/dn.md)| Print the amount of money we have left in the cash pot, then make a short, high beep and delay for 1 second  
[gnum](../main/subroutine/gnum.md)| Get a number from the keyboard  
[NWDAV4](../main/subroutine/nwdav4.md)| Print an "ITEM?" error, make a beep and rejoin the TT210 routine  
[NWDAVxx](../main/subroutine/tt210.md)| Used to rejoin this routine from the call to NWDAV4  
[OUT](../main/subroutine/gnum.md)| The OUTK routine jumps back here after printing the key that was just pressed  
[tnpr](../main/subroutine/tnpr.md)| Work out if we have space for a specific amount of cargo  
[tnpr1](../main/subroutine/tnpr1.md)| Work out if we have space for one tonne of cargo  
[TT151](../main/subroutine/tt151.md)| Print the name, price and availability of a market item  
[TT152](../main/subroutine/tt152.md)| Print the unit ("t", "kg" or "g") for a market item  
[TT160](../main/subroutine/tt160.md)| Print "t" (for tonne) and a space  
[TT161](../main/subroutine/tt161.md)| Print "kg" (for kilograms)  
[TT163](../main/subroutine/tt163.md)| Print the headers for the table of market prices  
[TT167](../main/subroutine/tt167.md)| Show the Market Price screen (red key f7)  
[TT16a](../main/subroutine/tt16a.md)| Print "g" (for grams)  
[TT208](../main/subroutine/tt208.md)| Show the Sell Cargo screen (red key f2)  
[TT210](../main/subroutine/tt210.md)| Show a list of current cargo in our hold, optionally to sell  
[TT213](../main/subroutine/tt213.md)| Show the Inventory screen (red key f9)  
[TT219](../main/subroutine/tt219.md)| Show the Buy Cargo screen (red key f1)  
[var](../main/subroutine/var.md)| Calculate QQ19+3 = economy * |economic_factor|  
  
## Maths (Arithmetic)  
\------------------  
  
[ADD](../main/subroutine/add.md)| Calculate (A X) = (A P) + (S R)  
[ADDK](../main/subroutine/addk.md)| Calculate (A X) = (A P) + (S R)  
[DORND](../main/subroutine/dornd.md)| Generate random numbers  
[DORND (Loader)](../loader/subroutine/dornd.md)| Generate random numbers  
[DORND2](../main/subroutine/dornd.md)| Make sure the C flag doesn't affect the outcome  
[DV41](../main/subroutine/dv41.md)| Calculate (P R) = 256 * DELTA / A  
[DV42](../main/subroutine/dv42.md)| Calculate (P R) = 256 * DELTA / z_hi  
[DVID3B2](../main/subroutine/dvid3b2.md)| Calculate K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)  
[DVID4](../main/subroutine/dvid4.md)| Calculate (P R) = 256 * A / Q  
[DVID4K](../main/subroutine/dvid4k.md)| Calculate (P R) = 256 * A / Q  
[DVIDT](../main/subroutine/dvidt.md)| Calculate (P+1 A) = (A P) / Q  
[FMLTU](../main/subroutine/fmltu.md)| Calculate A = A * Q / 256  
[FMLTU2](../main/subroutine/fmltu2.md)| Calculate A = K * sin(A)  
[GC2](../main/subroutine/gc2.md)| Calculate (Y X) = (A P) * 4  
[GCASH](../main/subroutine/gcash.md)| Calculate (Y X) = P * Q * 4  
[LCASH](../main/subroutine/lcash.md)| Subtract an amount of cash from the cash pot  
[LL120](../main/subroutine/ll120.md)| Calculate (Y X) = (S x1_lo) * XX12+2 or (S x1_lo) / XX12+2  
[LL121](../main/subroutine/ll123.md)| Calculate (Y X) = (S R) / Q and set the sign to the opposite of the top byte on the stack  
[LL122](../main/subroutine/ll120.md)| Calculate (Y X) = (S R) * Q and set the sign to the opposite of the top byte on the stack  
[LL123](../main/subroutine/ll123.md)| Calculate (Y X) = (S R) / XX12+2 or (S R) * XX12+2  
[LL128](../main/subroutine/ll123.md)| Contains an RTS  
[LL129](../main/subroutine/ll129.md)| Calculate Q = XX12+2, A = S EOR XX12+3 and (S R) = |S R|  
[LL133](../main/subroutine/ll123.md)| Negate (Y X) and return from the subroutine  
[LL28](../main/subroutine/ll28.md)| Calculate R = 256 * A / Q  
[LL28+4](../main/subroutine/ll28.md)| Skips the A >= Q check and always returns with C flag cleared, so this can be called if we know the division will work  
[LL31](../main/subroutine/ll28.md)| Skips the A >= Q check and does not set the R counter, so this can be used for jumping straight into the division loop if R is already set to 254 and we know the division will work  
[LL38](../main/subroutine/ll38.md)| Calculate (S A) = (S R) + (A Q)  
[LL5](../main/subroutine/ll5.md)| Calculate Q = SQRT(R Q)  
[LL61](../main/subroutine/ll61.md)| Calculate (U R) = 256 * A / Q  
[LL62](../main/subroutine/ll62.md)| Calculate 128 - (U R)  
[MAD](../main/subroutine/mad.md)| Calculate (A X) = Q * A + (S R)  
[MAS3](../main/subroutine/mas3.md)| Calculate A = x_hi^2 + y_hi^2 + z_hi^2 in the K% block  
[MCASH](../main/subroutine/mcash.md)| Add an amount of cash to the cash pot  
[MLS1](../main/subroutine/mls1.md)| Calculate (A P) = ALP1 * A  
[MLS2](../main/subroutine/mls2.md)| Calculate (S R) = XX(1 0) and (A P) = A * ALP1  
[MLTU2](../main/subroutine/mltu2.md)| Calculate (A P+1 P) = (A ~P) * Q  
[MLTU2-2](../main/subroutine/mltu2.md)| Set Q to X, so this calculates (A P+1 P) = (A ~P) * X  
[MLU1](../main/subroutine/mlu1.md)| Calculate Y1 = y_hi and (A P) = |y_hi| * Q for Y-th stardust  
[MLU2](../main/subroutine/mlu2.md)| Calculate (A P) = |A| * Q  
[MU1](../main/subroutine/mu1.md)| Copy X into P and A, and clear the C flag  
[MU11](../main/subroutine/mu11.md)| Calculate (A P) = P * X  
[MU5](../main/subroutine/mu5.md)| Set K(3 2 1 0) = (A A A A) and clear the C flag  
[MU6](../main/subroutine/mu6.md)| Set P(1 0) = (A A)  
[MULT1](../main/subroutine/mult1.md)| Calculate (A P) = Q * A  
[MULT12](../main/subroutine/mult12.md)| Calculate (S R) = Q * A  
[MULT3](../main/subroutine/mult3.md)| Calculate K(3 2 1 0) = (A P+1 P) * Q  
[MULTS-2](../main/subroutine/mls1.md)| Calculate (A P) = X * A  
[MULTU](../main/subroutine/multu.md)| Calculate (A P) = P * Q  
[MUT1](../main/subroutine/mut1.md)| Calculate R = XX and (A P) = Q * A  
[MUT2](../main/subroutine/mut2.md)| Calculate (S R) = XX(1 0) and (A P) = Q * A  
[MUT3](../main/subroutine/mut3.md)| An unused routine that does the same as MUT2  
[PIX1](../main/subroutine/pix1.md)| Calculate (YY+1 SYL+Y) = (A P) + (S R) and draw stardust particle  
[ROOT (Loader)](../loader/subroutine/root.md)| Calculate ZP = SQRT(ZP(1 0))  
[SPS2](../main/subroutine/sps2.md)| Calculate (Y X) = A / 10  
[SQUA](../main/subroutine/squa.md)| Clear bit 7 of A and calculate (A P) = A * A  
[SQUA2](../main/subroutine/squa2.md)| Calculate (A P) = A * A  
[SQUA2 (Loader)](../loader/subroutine/squa2.md)| Calculate (A P) = A * A  
[TAS1](../main/subroutine/tas1.md)| Calculate K3 = (x_sign x_hi x_lo) - V(1 0)  
[TIS1](../main/subroutine/tis1.md)| Calculate (A ?) = (-X * A + (S R)) / 96  
[TIS2](../main/subroutine/tis2.md)| Calculate A = A / Q  
[TIS3](../main/subroutine/tis3.md)| Calculate -(nosev_1 * roofv_1 + nosev_2 * roofv_2) / nosev_3  
[TT113](../main/subroutine/mcash.md)| Contains an RTS  
[VCSU1](../main/subroutine/vcsu1.md)| Calculate vector K3(8 0) = [x y z] - coordinates of the sun or space station  
[VCSUB](../main/subroutine/vcsub.md)| Calculate vector K3(8 0) = [x y z] - coordinates in (A V)  
  
## Maths (Geometry)  
\----------------  
  
[ARCTAN](../main/subroutine/arctan.md)| Calculate A = arctan(P / Q)  
[ARSR1](../main/subroutine/arctan.md)| Contains an RTS  
[FAROF](../main/subroutine/farof.md)| Compare x_hi, y_hi and z_hi with 224  
[FAROF2](../main/subroutine/farof2.md)| Compare x_hi, y_hi and z_hi with A  
[LL51](../main/subroutine/ll51.md)| Calculate the dot product of XX15 and XX16  
[m](../main/subroutine/mas2.md)| Do not include A in the calculation  
[MA9](../main/subroutine/mas1.md)| Contains an RTS  
[MAS1](../main/subroutine/mas1.md)| Add an orientation vector coordinate to an INWK coordinate  
[MAS2](../main/subroutine/mas2.md)| Calculate a cap on the maximum distance to the planet or sun  
[MAS4](../main/subroutine/mas4.md)| Calculate a cap on the maximum distance to a ship  
[NO1](../main/subroutine/norm.md)| Contains an RTS  
[NORM](../main/subroutine/norm.md)| Normalise the three-coordinate vector in XX15  
[PROJ](../main/subroutine/proj.md)| Project the current ship or planet onto the screen  
[SCALEX](../main/subroutine/scalex.md)| Scale the x-coordinate in A (leave it unchanged)  
[SCALEY](../main/subroutine/scaley.md)| Scale the y-coordinate in A to 0.5 * A  
[SCALEY2](../main/subroutine/scaley2.md)| Scale the y-coordinate in A (leave it unchanged)  
[SPS1](../main/subroutine/sps1.md)| Calculate the vector to the planet and store it in XX15  
[SPS1+1](../main/subroutine/sps1.md)| A BRK instruction  
[SPS3](../main/subroutine/sps3.md)| Copy a space coordinate from the K% block into K3  
[SPS4](../main/subroutine/sps4.md)| Calculate the vector to the space station  
[TA2](../main/subroutine/tas2.md)| Calculate the length of the vector in XX15 (ignoring the low coordinates), returning it in Q  
[TAS2](../main/subroutine/tas2.md)| Normalise the three-coordinate vector in K3  
[TAS3](../main/subroutine/tas3.md)| Calculate the dot product of XX15 and an orientation vector  
[TAS4](../main/subroutine/tas4.md)| Calculate the dot product of XX15 and one of the space station's orientation vectors  
[TAS6](../main/subroutine/tas6.md)| Negate the vector in XX15 so it points in the opposite direction  
[TIDY](../main/subroutine/tidy.md)| Orthonormalise the orientation vectors for a ship  
  
## Missions  
\--------  
  
[BAYSTEP](../main/subroutine/brp.md)| Go to the docking bay (i.e. show the Status Mode screen)  
[BRIEF](../main/subroutine/brief.md)| Start mission 1 and show the mission briefing  
[BRIEF2](../main/subroutine/brief2.md)| Start mission 2  
[BRIEF3](../main/subroutine/brief3.md)| Receive the briefing and plans for mission 2  
[BRIS](../main/subroutine/bris.md)| Clear the screen, display "INCOMING MESSAGE" and wait for 2 seconds  
[BRP](../main/subroutine/brp.md)| Print an extended token and show the Status Mode screen  
[BRPS](../main/subroutine/debrief.md)| Print the extended token in A, show the Status Mode screen and return from the subroutine  
[DEBRIEF](../main/subroutine/debrief.md)| Finish mission 1  
[DEBRIEF2](../main/subroutine/debrief2.md)| Finish mission 2  
[PAS1](../main/subroutine/pas1.md)| Display a rotating ship at space coordinates (0, 120, 256) and scan the keyboard  
[PAUSE](../main/subroutine/pause.md)| Display a rotating ship, waiting until a key is pressed, then remove the ship from the screen  
[TBRIEF](../main/subroutine/tbrief.md)| Start mission 3  
[THERE](../main/subroutine/there.md)| Check whether we are in the Constrictor's system in mission 1  
  
## Moving  
\------  
  
[MV40](../main/subroutine/mv40.md)| Rotate the planet or sun's location in space by the amount of pitch and roll of our ship  
[MV45](../main/subroutine/mveit_part_6_of_9.md)| Rejoin the MVEIT routine after the rotation, tactics and scanner code  
[MVEIT (Part 1 of 9)](../main/subroutine/mveit_part_1_of_9.md)| Move current ship: Tidy the orientation vectors  
[MVEIT (Part 2 of 9)](../main/subroutine/mveit_part_2_of_9.md)| Move current ship: Call tactics routine, remove ship from scanner  
[MVEIT (Part 3 of 9)](../main/subroutine/mveit_part_3_of_9.md)| Move current ship: Move ship forward according to its speed  
[MVEIT (Part 4 of 9)](../main/subroutine/mveit_part_4_of_9.md)| Move current ship: Apply acceleration to ship's speed as a one-off  
[MVEIT (Part 5 of 9)](../main/subroutine/mveit_part_5_of_9.md)| Move current ship: Rotate ship's location by our pitch and roll  
[MVEIT (Part 6 of 9)](../main/subroutine/mveit_part_6_of_9.md)| Move current ship: Move the ship in space according to our speed  
[MVEIT (Part 7 of 9)](../main/subroutine/mveit_part_7_of_9.md)| Move current ship: Rotate ship's orientation vectors by pitch/roll  
[MVEIT (Part 8 of 9)](../main/subroutine/mveit_part_8_of_9.md)| Move current ship: Rotate ship about itself by its own pitch/roll  
[MVEIT (Part 9 of 9)](../main/subroutine/mveit_part_9_of_9.md)| Move current ship: Redraw on scanner, if it hasn't been destroyed  
[MVS4](../main/subroutine/mvs4.md)| Apply pitch and roll to an orientation vector  
[MVS5](../main/subroutine/mvs5.md)| Apply a 3.6 degree pitch or roll to an orientation vector  
[MVT1](../main/subroutine/mvt1.md)| Calculate (x_sign x_hi x_lo) = (x_sign x_hi x_lo) + (A R)  
[MVT1-2](../main/subroutine/mvt1.md)| Clear bits 0-6 of A before entering MVT1  
[MVT3](../main/subroutine/mvt3.md)| Calculate K(3 2 1) = (x_sign x_hi x_lo) + K(3 2 1)  
[MVT6](../main/subroutine/mvt6.md)| Calculate (A P+2 P+1) = (x_sign x_hi x_lo) + (A P+2 P+1)  
[SFS2](../main/subroutine/sfs2.md)| Move a ship in space along one of the coordinate axes  
  
## Save and load  
\-------------  
  
[CATS](../main/subroutine/cats.md)| Ask for a disc drive number and print a catalogue of that drive  
[CHECK](../main/subroutine/check.md)| Calculate the checksum for the last saved commander data block  
[CHECK2](../main/subroutine/check2.md)| Calculate the third checksum for the last saved commander data block (Commodore 64 and Apple II versions only)  
[COLD](../main/subroutine/cold.md)| Set the standard BRKV handler for the game  
[DELT](../main/subroutine/delt.md)| Catalogue a disc, ask for a filename to delete, and delete the file  
[DELT-1](../main/subroutine/delt.md)| Contains an RTS  
[FILEPR](../main/subroutine/filepr.md)| Display the currently selected media (disk or tape)  
[GTDIR](../main/subroutine/gtdir.md)| Fetch the name of an ADFS directory on the Master Compact and change to that directory  
[GTDRV](../main/subroutine/gtdrv.md)| Get an ASCII disc drive number from the keyboard  
[GTNMEW](../main/subroutine/gtnmew.md)| Fetch the name of a commander file to save or load  
[JAMESON](../main/subroutine/jameson.md)| Restore the default JAMESON commander  
[LOD](../main/subroutine/lod.md)| Load a commander file  
[LOR](../main/subroutine/lod.md)| Set the C flag and return from the subroutine  
[OTHERFILEPR](../main/subroutine/otherfilepr.md)| Display the non-selected media (disk or tape)  
[rfile](../main/subroutine/rfile.md)| Load the commander file  
[SVE](../main/subroutine/sve.md)| Display the disk access menu and process saving of commander files  
[TR1](../main/subroutine/tr1.md)| Copy the last saved commander's name from NA% to INWK  
[TRNME](../main/subroutine/trnme.md)| Copy the last saved commander's name from INWK to NA%  
[wfile](../main/subroutine/wfile.md)| Save the commander file  
  
## Ship hangar  
\-----------  
  
[HA3](../main/subroutine/hanger.md)| Contains an RTS  
[HAL3](../main/subroutine/hal3.md)| Draw a hangar background line from left to right, stopping when it bumps into existing on-screen content  
[HALL](../main/subroutine/hall.md)| Draw the ships in the ship hangar, then draw the hangar  
[HANGER](../main/subroutine/hanger.md)| Display the ship hangar  
[HAS1](../main/subroutine/has1.md)| Draw a ship in the ship hangar  
[HAS2](../main/subroutine/has2.md)| Draw a hangar background line from left to right  
[HAS3](../main/subroutine/has3.md)| Draw a hangar background line from right to left  
  
## Sound  
\-----  
  
[BEEP](../main/subroutine/beep.md)| Make a short, high beep  
[BELL](../main/subroutine/bell.md)| Make a standard system beep  
[BOOP](../main/subroutine/boop.md)| Make a long, low beep  
[ELASNO](../main/subroutine/elasno.md)| Make the sound of us being hit  
[EXNO](../main/subroutine/exno.md)| Make the sound of a laser strike on another ship or a ship explosion  
[EXNO3](../main/subroutine/exno3.md)| Make an explosion sound  
[LASNO](../main/subroutine/lasno.md)| Make the sound of our laser firing  
[NOISE](../main/subroutine/noise.md)| Make the sound whose number is in Y by populating the sound buffer  
[SOFLUSH](../main/subroutine/soflush.md)| Reset the sound buffers and turn off all sound channels  
[SOINT](../main/subroutine/soint.md)| Process the contents of the sound buffer and send it to the sound chip  
[SOUR1](../main/subroutine/sous1.md)| Contains an RTS  
[SOUS1](../main/subroutine/sous1.md)| Write sound data directly to the 76489 sound chip  
  
## Stardust  
\--------  
  
[FLIP](../main/subroutine/flip.md)| Reflect the stardust particles in the screen diagonal and redraw the stardust field  
[nWq](../main/subroutine/nwq.md)| Create a random cloud of stardust  
[NWSTARS](../main/subroutine/nwstars.md)| Initialise the stardust field  
[STARS](../main/subroutine/stars.md)| The main routine for processing the stardust  
[STARS1](../main/subroutine/stars1.md)| Process the stardust for the front view  
[STARS2](../main/subroutine/stars2.md)| Process the stardust for the left or right view  
[STARS6](../main/subroutine/stars6.md)| Process the stardust for the rear view  
  
## Start and end  
\-------------  
  
[BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md)| Show the "Load New Commander (Y/N)?" screen and start the game  
[BR1 (Part 2 of 2)](../main/subroutine/br1_part_2_of_2.md)| Show the "Press Fire or Space, Commander" screen and start the game  
[DEATH](../main/subroutine/death.md)| Display the death screen  
[DEATH2](../main/subroutine/death2.md)| Reset most of the game and restart from the title screen  
[DFAULT](../main/subroutine/dfault.md)| Reset the current commander data block to the last saved commander  
[QU5](../main/subroutine/br1_part_1_of_2.md)| Restart the game using the last saved commander without asking whether to load a new commander file  
[RES2](../main/subroutine/res2.md)| Reset a number of flight variables and workspaces  
[RESET](../main/subroutine/reset.md)| Reset most variables  
[TITLE](../main/subroutine/title.md)| Display a title screen with a rotating ship and prompt  
[TT170](../main/subroutine/tt170.md)| Main entry point for the Elite game code  
  
## Status  
\------  
  
[BAD](../main/subroutine/bad.md)| Calculate how bad we have been  
[BAY](../main/subroutine/bay.md)| Go to the docking bay (i.e. show the Status Mode screen)  
[cmn](../main/subroutine/cmn.md)| Print the commander's name  
[cmn-1](../main/subroutine/cmn.md)| Contains an RTS  
[csh](../main/subroutine/csh.md)| Print the current amount of cash  
[EXNO2](../main/subroutine/exno2.md)| Process us making a kill  
[fwl](../main/subroutine/fwl.md)| Print fuel and cash levels  
[STATUS](../main/subroutine/status.md)| Show the Status Mode screen (red key f8)  
  
## Tactics  
\-------  
  
[ANGRY](../main/subroutine/angry.md)| Make a ship or station hostile, and if this is a ship then enable the ship's AI and give it a kick of speed  
[fq1](../main/subroutine/frs1.md)| Used to add a cargo canister to the universe  
[FR1](../main/subroutine/fr1.md)| Display the "missile jammed" message  
[FR1-2](../main/subroutine/fr1.md)| Clear the C flag and return from the subroutine  
[FRMIS](../main/subroutine/frmis.md)| Fire a missile from our ship  
[FRS1](../main/subroutine/frs1.md)| Launch a ship straight ahead of us, below the laser sights  
[GOPL](../main/subroutine/tactics_part_3_of_7.md)| Make the ship head towards the planet  
[HI1](../main/subroutine/hitch.md)| Contains an RTS  
[HITCH](../main/subroutine/hitch.md)| Work out if the ship in INWK is in our crosshairs  
[SFRMIS](../main/subroutine/sfrmis.md)| Add an enemy missile to our local bubble of universe  
[TA151](../main/subroutine/tactics_part_7_of_7.md)| Make the ship head towards the planet  
[TA9-1](../main/subroutine/tactics_part_7_of_7.md)| Contains an RTS  
[TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)| Apply tactics: Process missiles, both enemy missiles and our own  
[TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)| Apply tactics: Escape pod, station, lone Thargon, safe-zone pirate  
[TACTICS (Part 3 of 7)](../main/subroutine/tactics_part_3_of_7.md)| Apply tactics: Calculate dot product to determine ship's aim  
[TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)| Apply tactics: Check energy levels, maybe launch escape pod if low  
[TACTICS (Part 5 of 7)](../main/subroutine/tactics_part_5_of_7.md)| Apply tactics: Consider whether to launch a missile at us  
[TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md)| Apply tactics: Consider firing a laser at us, if aim is true  
[TACTICS (Part 7 of 7)](../main/subroutine/tactics_part_7_of_7.md)| Apply tactics: Set pitch, roll, and acceleration  
[yetanotherrts](../main/subroutine/yetanotherrts.md)| Contains an RTS  
  
## Text  
\----  
  
[BPRNT](../main/subroutine/bprnt.md)| Print a 32-bit number, left-padded to a specific number of digits, with an optional decimal point  
[CHPR](../main/subroutine/chpr.md)| Print a character at the text cursor by poking into screen memory  
[crlf](../main/subroutine/crlf.md)| Tab to column 21 and print a colon  
[DASC](../main/subroutine/tt26.md)| DASC does exactly the same as TT26 and prints a character at the text cursor, with support for verified text in extended tokens  
[DETOK](../main/subroutine/detok.md)| Print an extended recursive token from the TKN1 token table  
[DETOK2](../main/subroutine/detok2.md)| Print an extended text token (1-255)  
[DETOK3](../main/subroutine/detok3.md)| Print an extended recursive token from the RUTOK token table  
[dn2](../main/subroutine/dn2.md)| Make a short, high beep and delay for 0.5 seconds  
[DOXC](../main/subroutine/doxc.md)| Move the text cursor to a specific column  
[DOYC](../main/subroutine/doyc.md)| Move the text cursor to a specific row  
[DTEN](../main/subroutine/detok.md)| Print recursive token number X from the token table pointed to by (A V), used to print tokens from the RUTOK table via calls to DETOK3  
[DTS](../main/subroutine/detok2.md)| Print a single letter in the correct case  
[ex](../main/subroutine/ex.md)| Print a recursive token  
[FEED](../main/subroutine/feed.md)| Print a newline  
[INCYC](../main/subroutine/incyc.md)| Move the text cursor to the next row  
[MT1](../main/subroutine/mt1.md)| Switch to ALL CAPS when printing extended tokens  
[MT13](../main/subroutine/mt13.md)| Switch to lower case when printing extended tokens  
[MT14](../main/subroutine/mt14.md)| Switch to justified text when printing extended tokens  
[MT15](../main/subroutine/mt15.md)| Switch to left-aligned text when printing extended tokens  
[MT16](../main/subroutine/mt16.md)| Print the character in variable DTW7  
[MT17](../main/subroutine/mt17.md)| Print the selected system's adjective, e.g. Lavian for Lave  
[MT18](../main/subroutine/mt18.md)| Print a random 1-8 letter word in Sentence Case  
[MT19](../main/subroutine/mt19.md)| Capitalise the next letter  
[MT2](../main/subroutine/mt2.md)| Switch to Sentence Case when printing extended tokens  
[MT23](../main/subroutine/mt23.md)| Move to row 10, switch to white text, and switch to lower case when printing extended tokens  
[MT26](../main/subroutine/mt26.md)| Fetch a line of text from the keyboard  
[MT27](../main/subroutine/mt27.md)| Print the captain's name during mission briefings  
[MT28](../main/subroutine/mt28.md)| Print the location hint during the mission 1 briefing  
[MT29](../main/subroutine/mt29.md)| Move to row 6, switch to white text, and switch to lower case when printing extended tokens  
[MT5](../main/subroutine/mt5.md)| Switch to extended tokens  
[MT6](../main/subroutine/mt6.md)| Switch to standard tokens in Sentence Case  
[MT8](../main/subroutine/mt8.md)| Tab to column 6 and start a new word when printing extended tokens  
[MT9](../main/subroutine/mt9.md)| Clear the screen and set the current view type to 1  
[NUMBOR](../main/subroutine/numbor.md)| An unused routine that prints a number in hexadecimal  
[OUTK](../main/subroutine/outk.md)| Print the character in Q before returning to gnum  
[plf](../main/subroutine/plf.md)| Print a text token followed by a newline  
[plf2](../main/subroutine/plf2.md)| Print text followed by a newline and indent of 6 characters  
[pr2](../main/subroutine/pr2.md)| Print an 8-bit number, left-padded to 3 digits, and optional point  
[pr2+2](../main/subroutine/pr2.md)| Print the 8-bit number in X to the number of digits in A  
[pr5](../main/subroutine/pr5.md)| Print a 16-bit number, left-padded to 5 digits, and optional point  
[pr6](../main/subroutine/pr6.md)| Print 16-bit number, left-padded to 5 digits, no point  
[prq](../main/subroutine/prq.md)| Print a text token followed by a question mark  
[prq+3](../main/subroutine/prq.md)| Print a question mark  
[qw](../main/subroutine/qw.md)| Print a recursive token in the range 128-145  
[RR4](../main/subroutine/chpr.md)| Restore the registers and return from the subroutine  
[spc](../main/subroutine/spc.md)| Print a text token followed by a space  
[TT11](../main/subroutine/tt11.md)| Print a 16-bit number, left-padded to n digits, and optional point  
[TT162](../main/subroutine/tt162.md)| Print a space  
[TT162+2](../main/subroutine/tt162.md)| Jump to TT27 to print the text token in A  
[TT26](../main/subroutine/tt26.md)| Print a character at the text cursor, with support for verified text in extended tokens  
[TT27](../main/subroutine/tt27.md)| Print a text token  
[TT41](../main/subroutine/tt41.md)| Print a letter according to Sentence Case  
[TT42](../main/subroutine/tt42.md)| Print a letter in lower case  
[TT43](../main/subroutine/tt43.md)| Print a two-letter token or recursive token 0-95  
[TT44](../main/subroutine/tt42.md)| Jumps to TT26 to print the character in A (used to enable us to use a branch instruction to jump to TT26)  
[TT45](../main/subroutine/tt45.md)| Print a letter in lower case  
[TT46](../main/subroutine/tt46.md)| Print a character and switch to capitals  
[TT48](../main/subroutine/ex.md)| Contains an RTS  
[TT60](../main/subroutine/tt60.md)| Print a text token and a paragraph break  
[TT67](../main/subroutine/tt67.md)| Print a newline  
[TT67K](../main/subroutine/tt67k.md)| Print a newline  
[TT68](../main/subroutine/tt68.md)| Print a text token followed by a colon  
[TT69](../main/subroutine/tt69.md)| Set Sentence Case and print a newline  
[TT73](../main/subroutine/tt73.md)| Print a colon  
[TT74](../main/subroutine/tt74.md)| Print a character  
[TTX69](../main/subroutine/ttx69.md)| Print a paragraph break  
[VOWEL](../main/subroutine/vowel.md)| Test whether a character is a vowel  
  
## Universe  
\--------  
  
[cpl](../main/subroutine/cpl.md)| Print the selected system name  
[GINF](../main/subroutine/ginf.md)| Fetch the address of a ship's data block into INF  
[GTHG](../main/subroutine/gthg.md)| Spawn a Thargoid ship and a Thargon companion  
[GVL](../main/subroutine/gvl.md)| Calculate the availability of market items  
[hy5](../main/subroutine/jmp.md)| Contains an RTS  
[hyp1](../main/subroutine/hyp1.md)| Process a jump to the system closest to (QQ9, QQ10)  
[hyp1+3](../main/subroutine/hyp1.md)| Jump straight to the system at (QQ9, QQ10) without first calculating which system is closest. We do this if we already know that (QQ9, QQ10) points to a system  
[hyR](../main/subroutine/gvl.md)| Contains an RTS  
[jmp](../main/subroutine/jmp.md)| Set the current system to the selected system  
[KILLSHP](../main/subroutine/killshp.md)| Remove a ship from our local bubble of universe  
[KS1](../main/subroutine/ks1.md)| Remove the current ship from our local bubble of universe  
[KS2](../main/subroutine/ks2.md)| Check the local bubble for missiles with target lock  
[KS3](../main/subroutine/ks3.md)| Set the SLSP ship line heap pointer after shuffling ship slots  
[KS4](../main/subroutine/ks4.md)| Remove the space station and replace it with the sun  
[NwS1](../main/subroutine/nws1.md)| Flip the sign and double an INWK byte  
[NWSHP](../main/subroutine/nwshp.md)| Add a new ship to our local bubble of universe  
[NWSPS](../main/subroutine/nwsps.md)| Add a new space station to our local bubble of universe  
[oh](../main/subroutine/spin.md)| Contains an RTS  
[PDESC](../main/subroutine/pdesc.md)| Print the system's extended description or a mission 1 directive  
[ping](../main/subroutine/ping.md)| Set the selected system to the current system  
[readdistnce](../main/subroutine/tt111.md)| Calculate the distance between the system with galactic coordinates (A, QQ15+1) and the system at (QQ0, QQ1), returning the result in QQ8(1 0)  
[SFS1](../main/subroutine/sfs1.md)| Spawn a child ship from the current (parent) ship  
[SFS1-2](../main/subroutine/sfs1.md)| Used to add a missile to the local bubble that that has AI (bit 7 set), is hostile (bit 6 set) and has been launched (bit 0 clear); the target slot number is set to 31, but this is ignored as the hostile flags means we are the target  
[SOLAR](../main/subroutine/solar.md)| Set up various aspects of arriving in a new system  
[SOS1](../main/subroutine/sos1.md)| Update the missile indicators, set up the planet data block  
[SPIN](../main/subroutine/spin.md)| Randomly spawn cargo from a destroyed ship  
[SPIN2](../main/subroutine/spin.md)| Remove any randomness: spawn cargo of a specific type (given in X), and always spawn the number given in A  
[tal](../main/subroutine/tal.md)| Print the current galaxy number  
[TT111](../main/subroutine/tt111.md)| Set the current system to the nearest system to a point  
[TT111-1](../main/subroutine/tt111.md)| Contains an RTS  
[TT146](../main/subroutine/tt146.md)| Print the distance to the selected system in light years  
[TT20](../main/subroutine/tt20.md)| Twist the selected system's seeds four times  
[TT24](../main/subroutine/tt24.md)| Calculate system data from the system seeds  
[TT25](../main/subroutine/tt25.md)| Show the Data on System screen (red key f6)  
[TT54](../main/subroutine/tt54.md)| Twist the selected system's seeds  
[TT70](../main/subroutine/tt70.md)| Display "MAINLY " and jump to TT72  
[TT72](../main/subroutine/tt25.md)| Used by TT70 to re-enter the routine after displaying "MAINLY" for the economy type  
[TT81](../main/subroutine/tt81.md)| Set the selected system's seeds to those of system 0  
[ypl](../main/subroutine/ypl.md)| Print the current system name  
[ypl-1](../main/subroutine/ypl.md)| Contains an RTS  
[Ze](../main/subroutine/ze.md)| Initialise the INWK workspace to a fairly aggressive ship  
[ZINF](../main/subroutine/zinf.md)| Reset the INWK workspace and orientation vectors  
  
## Utility routines  
\----------------  
  
[backtonormal](../main/subroutine/backtonormal.md)| Disable the keyboard, set the SVN flag to 0, and return with A = 0  
[CLDELAY](../main/subroutine/cldelay.md)| Delay by iterating through 5 * 256 (1280) empty loops  
[DEEOR](../main/subroutine/deeor.md)| Unscramble the main code  
[DEEORS](../main/subroutine/deeors.md)| Decrypt a multi-page block of memory  
[DELAY](../main/subroutine/delay.md)| Wait for a specified time, in 1/50s of a second  
[getzp](../main/subroutine/getzp.md)| Swap zero page (&0090 to &00EF) with the buffer at &3000  
[getzp+3](../main/subroutine/getzp.md)| Restore the top part of zero page, but without first claiming the NMI workspace (Master Compact variant only)  
[NEWBRK](../main/subroutine/newbrk.md)| The standard BRKV handler for the game  
[NMICLAIM](../main/subroutine/nmiclaim.md)| Claim the NMI workspace (&00A0 to &00A7) back from the MOS so the game can use it once again  
[NMIRELEASE](../main/subroutine/nmirelease.md)| Release the NMI workspace (&00A0 to &00A7) so the MOS can use it and store the top part of zero page in the buffer at &3000  
[OSB (Loader)](../loader/subroutine/osb.md)| A convenience routine for calling OSBYTE with Y = 0  
[setzp](../main/subroutine/setzp.md)| Copy the top part of zero page (&0090 to &00FF) into the buffer at &3000  
[SWAPPZERO](../main/subroutine/swappzero.md)| An unused routine that swaps bytes in and out of zero page  
[ZERO](../main/subroutine/zero.md)| Reset the local bubble of universe and ship status  
[ZES1](../main/subroutine/zes1.md)| Zero-fill the page whose number is in X  
[ZES2](../main/subroutine/zes2.md)| Zero-fill a specific page  
  
[Source code cross-references](../articles/source_code_cross-references.md "Previous routine")[List of all variables](variables.md "Next routine")
