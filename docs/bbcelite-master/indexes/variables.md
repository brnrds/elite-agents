---
title: "List of all variables in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/indexes/variables.html"
---

[List of all subroutines](subroutines.md "Previous routine")[List of all workspaces](workspaces.md "Next routine")

This index contains every variable that appears in the source code for the BBC Master version of Elite, grouped by category. A variable is defined as a labelled memory location that is used for storing data, and this list includes both variables that are defined in workspaces, and variables that are declared within the body of the source code.

  * Dashboard
  * Drawing lines
  * Drawing pixels
  * Drawing planets
  * Drawing ships
  * Drawing the screen
  * Equipment
  * Keyboard
  * Loader
  * Market
  * Maths (Arithmetic)
  * Maths (Geometry)
  * Missions
  * Save and load
  * Ship hangar
  * Sound
  * Status
  * Text
  * Universe
  * Utility routines
  * Workspace variables



## Dashboard  
\---------  
  
---  
[ECBT](../main/variable/ecbt.md)| The character bitmap for the E.C.M. indicator bulb  
[SPBT](../main/variable/spbt.md)| The bitmap definition for the space station indicator bulb  
  
## Drawing lines  
\-------------  
  
[beamcol](../main/variable/beamcol.md)| An unused table of laser colours  
[BOMBPOS](../main/variable/bombpos.md)| A set of x-coordinates that are used as the basis for the energy bomb's zig-zag lightning bolt  
[BOMBTBX](../main/variable/bombtbx.md)| This is where we store the x-coordinates for the energy bomb's zig-zag lightning bolt  
[BOMBTBY](../main/variable/bombtby.md)| This is where we store the y-coordinates for the energy bomb's zig-zag lightning bolt  
[sightcol](../main/variable/sightcol.md)| Colours for the crosshair sights on the different laser types  
  
## Drawing pixels  
\--------------  
  
[CTWOS](../main/variable/ctwos.md)| Ready-made single-pixel character row bytes for mode 2  
[orange](../main/variable/orange.md)| Lookup table for two-pixel mode 1 orange pixels for the sun  
[PXCL](../main/variable/pxcl.md)| A four-colour mode 1 pixel byte that represents a dot's distance  
[TWFL](../main/variable/twfl.md)| Ready-made character rows for the left end of a horizontal line  
[TWFR](../main/variable/twfr.md)| Ready-made character rows for the right end of a horizontal line  
[TWOS](../main/variable/twos.md)| Ready-made single-pixel character row bytes for mode 1  
[TWOS (Loader)](../loader/variable/twos.md)| Ready-made single-pixel character row bytes for mode 1  
[TWOS2](../main/variable/twos2.md)| Ready-made double-pixel character row bytes for mode 1  
[ylookup](../main/variable/ylookup.md)| Lookup table for converting pixel y-coordinate to page number of screen address  
  
## Drawing planets  
\---------------  
  
[CNT (Loader)](../loader/variable/cnt.md)| A counter for use in drawing Saturn's planetary body  
[CNT2 (Loader)](../loader/variable/cnt2.md)| A counter for use in drawing Saturn's background stars  
[CNT3 (Loader)](../loader/variable/cnt3.md)| A counter for use in drawing Saturn's rings  
[RAND (Loader)](../loader/variable/rand.md)| The random number seed used for drawing Saturn  
  
## Drawing ships  
\-------------  
  
[coltabl](../main/variable/coltabl.md)| Colours for ship explosions  
[E% (Game data)](../game_data/variable/e_per_cent.md)| Ship blueprints default NEWB flags  
[exlook](../main/variable/exlook.md)| A table to shift X left by one place when X is 0 or 1  
[scacol](../main/variable/scacol.md)| Ship colours on the scanner  
[SHIP_ADDER (Game data)](../game_data/variable/ship_adder.md)| Ship blueprint for an Adder  
[SHIP_ANACONDA (Game data)](../game_data/variable/ship_anaconda.md)| Ship blueprint for an Anaconda  
[SHIP_ASP_MK_2 (Game data)](../game_data/variable/ship_asp_mk_2.md)| Ship blueprint for an Asp Mk II  
[SHIP_ASTEROID (Game data)](../game_data/variable/ship_asteroid.md)| Ship blueprint for an asteroid  
[SHIP_BOA (Game data)](../game_data/variable/ship_boa.md)| Ship blueprint for a Boa  
[SHIP_BOULDER (Game data)](../game_data/variable/ship_boulder.md)| Ship blueprint for a boulder  
[SHIP_CANISTER (Game data)](../game_data/variable/ship_canister.md)| Ship blueprint for a cargo canister  
[SHIP_COBRA_MK_1 (Game data)](../game_data/variable/ship_cobra_mk_1.md)| Ship blueprint for a Cobra Mk I  
[SHIP_COBRA_MK_3 (Game data)](../game_data/variable/ship_cobra_mk_3.md)| Ship blueprint for a Cobra Mk III  
[SHIP_COBRA_MK_3_P (Game data)](../game_data/variable/ship_cobra_mk_3_p.md)| Ship blueprint for a Cobra Mk III (pirate)  
[SHIP_CONSTRICTOR (Game data)](../game_data/variable/ship_constrictor.md)| Ship blueprint for a Constrictor  
[SHIP_CORIOLIS (Game data)](../game_data/variable/ship_coriolis.md)| Ship blueprint for a Coriolis space station  
[SHIP_COUGAR (Game data)](../game_data/variable/ship_cougar.md)| Ship blueprint for a Cougar  
[SHIP_DODO (Game data)](../game_data/variable/ship_dodo.md)| Ship blueprint for a Dodecahedron ("Dodo") space station  
[SHIP_ESCAPE_POD (Game data)](../game_data/variable/ship_escape_pod.md)| Ship blueprint for an escape pod  
[SHIP_FER_DE_LANCE (Game data)](../game_data/variable/ship_fer_de_lance.md)| Ship blueprint for a Fer-de-Lance  
[SHIP_GECKO (Game data)](../game_data/variable/ship_gecko.md)| Ship blueprint for a Gecko  
[SHIP_KRAIT (Game data)](../game_data/variable/ship_krait.md)| Ship blueprint for a Krait  
[SHIP_MAMBA (Game data)](../game_data/variable/ship_mamba.md)| Ship blueprint for a Mamba  
[SHIP_MISSILE (Game data)](../game_data/variable/ship_missile.md)| Ship blueprint for a missile  
[SHIP_MORAY (Game data)](../game_data/variable/ship_moray.md)| Ship blueprint for a Moray  
[SHIP_PLATE (Game data)](../game_data/variable/ship_plate.md)| Ship blueprint for an alloy plate  
[SHIP_PYTHON (Game data)](../game_data/variable/ship_python.md)| Ship blueprint for a Python  
[SHIP_PYTHON_P (Game data)](../game_data/variable/ship_python_p.md)| Ship blueprint for a Python (pirate)  
[SHIP_ROCK_HERMIT (Game data)](../game_data/variable/ship_rock_hermit.md)| Ship blueprint for a rock hermit (asteroid)  
[SHIP_SHUTTLE (Game data)](../game_data/variable/ship_shuttle.md)| Ship blueprint for a Shuttle  
[SHIP_SIDEWINDER (Game data)](../game_data/variable/ship_sidewinder.md)| Ship blueprint for a Sidewinder  
[SHIP_SPLINTER (Game data)](../game_data/variable/ship_splinter.md)| Ship blueprint for a splinter  
[SHIP_THARGOID (Game data)](../game_data/variable/ship_thargoid.md)| Ship blueprint for a Thargoid mothership  
[SHIP_THARGON (Game data)](../game_data/variable/ship_thargon.md)| Ship blueprint for a Thargon  
[SHIP_TRANSPORTER (Game data)](../game_data/variable/ship_transporter.md)| Ship blueprint for a Transporter  
[SHIP_VIPER (Game data)](../game_data/variable/ship_viper.md)| Ship blueprint for a Viper  
[SHIP_WORM (Game data)](../game_data/variable/ship_worm.md)| Ship blueprint for a Worm  
[shpcol](../main/variable/shpcol.md)| Ship colours  
[XX21 (Game data)](../game_data/variable/xx21.md)| Ship blueprints lookup table  
  
## Drawing the screen  
\------------------  
  
[B% (Loader)](../loader/variable/b_per_cent.md)| VDU commands for setting the square mode 1 screen  
[SCTBX1](../main/variable/sctbx1.md)| Lookup table for converting a pixel x-coordinate to the bit number within the pixel row byte that corresponds to this pixel  
[SCTBX2](../main/variable/sctbx2.md)| Lookup table for converting a pixel x-coordinate to the byte number in the pixel row that corresponds to this pixel  
[TVT1](../main/variable/tvt1.md)| Palette data for the mode 2 part of the screen (the dashboard)  
[TVT3](../main/variable/tvt3.md)| Palette data for the mode 1 part of the screen (the top part)  
[VEC](../main/variable/vec.md)| The original value of the IRQ1 vector  
[VSCAN](../main/variable/vscan.md)| Defines the split position in the split-screen mode  
  
## Equipment  
\---------  
  
[PRXS](../main/variable/prxs.md)| Equipment prices  
  
## Keyboard  
\--------  
  
[IKNS](../main/variable/ikns.md)| Lookup table for in-flight keyboard controls  
[KTRAN](../main/variable/ktran.md)| An unused key logger buffer that's left over from the 6502 Second Processor version of Elite  
[TGINT](../main/variable/tgint.md)| The keys used to toggle configuration settings when the game is paused  
[TRTB%](../main/variable/trtb_per_cent.md)| Translation table from internal key number to ASCII  
  
## Loader  
\------  
  
[Dashboard image (Game data)](../game_data/variable/dashboard_image.md)| The binary for the dashboard image  
[MESS1 (Loader)](../loader/variable/mess1.md)| The OS command string for loading the BDATA binary  
[MESS2 (Loader)](../loader/variable/mess2.md)| The OS command string for loading the main game code binary  
[MESS3 (Loader)](../loader/variable/mess3.md)| The OS command string for changing the disc directory to E  
  
## Market  
\------  
  
[QQ23](../main/variable/qq23.md)| Market prices table  
  
## Maths (Arithmetic)  
\------------------  
  
[alogh](../main/variable/alogh.md)| Binary antilogarithm table  
[log](../main/variable/log.md)| Binary logarithm table (high byte)  
[logL](../main/variable/logl.md)| Binary logarithm table (low byte)  
  
## Maths (Geometry)  
\----------------  
  
[ACT (Game data)](../game_data/variable/act.md)| Arctan table  
[SNE (Game data)](../game_data/variable/sne.md)| Sine/cosine table  
  
## Missions  
\--------  
  
[SPMASK](../main/variable/spmask.md)| Masks for updating sprite bits in VIC+&10 for the top bit of the 9-bit x-coordinates of the Trumble sprites  
[TRIBDIR](../main/variable/tribdir.md)| The low byte of the four 16-bit directions in which Trumble sprites can move  
[TRIBDIRH](../main/variable/tribdirh.md)| The high byte of the four 16-bit directions in which Trumble sprites can move  
[TRIBMA](../main/variable/tribma.md)| A table for converting the number of Trumbles in the hold into a sprite-enable flag to use with VIC register &15  
[TRIBTA](../main/variable/tribta.md)| A table for converting the number of Trumbles in the hold into a number of sprites in the range 0 to 6  
  
## Save and load  
\-------------  
  
[CHK](../main/variable/chk.md)| First checksum byte for the saved commander data file  
[CHK2](../main/variable/chk2.md)| Second checksum byte for the saved commander data file  
[CTLI](../main/variable/ctli.md)| The OS command string for cataloguing a disc  
[DELI](../main/variable/deli.md)| The OS command string for deleting a file  
[DIRI](../main/variable/diri.md)| The OS command string for changing directory on the Master Compact  
[lodosc](../main/variable/lodosc.md)| The OS command string for loading a commander file  
[NA%](../main/variable/na_per_cent.md)| The data block for the last saved commander  
[NA2%](../main/variable/na2_per_cent.md)| The data block for the default commander  
[oldlong](../main/variable/oldlong.md)| Contains the length of the last saved commander name  
[S1%](../main/variable/s1_per_cent.md)| The drive and directory number used when saving or loading a commander file  
[savosc](../main/variable/savosc.md)| The OS command string for saving a commander file  
[stackpt](../main/variable/stackpt.md)| Temporary storage for the stack pointer when jumping to the break handler at NEWBRK  
[thislong](../main/variable/thislong.md)| Contains the length of the most recently entered commander name  
[wtable](../main/variable/wtable.md)| 6-bit to 7-bit nibble conversion table  
  
## Ship hangar  
\-----------  
  
[HANGFLAG](../main/variable/hangflag.md)| The number of ships being displayed in the ship hangar  
[HATB](../main/variable/hatb.md)| Ship hangar group table  
  
## Sound  
\-----  
  
[SFXBT](../main/variable/sfxbt.md)| Sound data block 2  
[SFXFQ](../main/variable/sfxfq.md)| Sound data block 3  
[SFXPR](../main/variable/sfxpr.md)| Sound data block 1  
[SFXVC](../main/variable/sfxvc.md)| Sound data block 4  
[SOFH](../main/variable/sofh.md)| Sound chip data mask for choosing a tone channel in the range 0-2  
[SOOFF](../main/variable/sooff.md)| Sound chip data to turn the volume down on all channels and to act as a mask for choosing a tone channel in the range 0-2  
  
## Status  
\------  
  
[KWH% (Game data)](../game_data/variable/kwh_per_cent.md)| Integer number of kills awarded for destroying each type of ship  
[KWL% (Game data)](../game_data/variable/kwl_per_cent.md)| Fractional number of kills awarded for destroying each type of ship  
  
## Text  
\----  
  
[DTW1](../main/variable/dtw1.md)| A mask for applying the lower case part of Sentence Case to extended text tokens  
[DTW2](../main/variable/dtw2.md)| A flag that indicates whether we are currently printing a word  
[DTW3](../main/variable/dtw3.md)| A flag for switching between standard and extended text tokens  
[DTW4](../main/variable/dtw4.md)| Flags that govern how justified extended text tokens are printed  
[DTW5](../main/variable/dtw5.md)| The size of the justified text buffer at BUF  
[DTW6](../main/variable/dtw6.md)| A flag to denote whether printing in lower case is enabled for extended text tokens  
[DTW8](../main/variable/dtw8.md)| A mask for capitalising the next letter in an extended text token  
[FONT%](../main/variable/font_per_cent.md)| A copy of the character definition bitmap table from the MOS ROM  
[JMTB](../main/variable/jmtb.md)| The extended token table for jump tokens 1-32 (DETOK)  
[MTIN](../main/variable/mtin.md)| Lookup table for random tokens in the extended token table (0-37)  
[QQ16](../main/variable/qq16.md)| The two-letter token lookup table  
[QQ18 (Game data)](../game_data/variable/qq18.md)| The recursive token table for tokens 0-148  
[RLINE](../main/variable/rline.md)| The OSWORD configuration block used to fetch a line of text from the keyboard  
[RUGAL (Game data)](../game_data/variable/rugal.md)| The criteria for systems with extended description overrides  
[RUPLA (Game data)](../game_data/variable/rupla.md)| System numbers that have extended description overrides  
[RUTOK (Game data)](../game_data/variable/rutok.md)| The second extended token table for recursive tokens 0-26 (DETOK3)  
[TENS](../main/variable/tens.md)| A constant used when printing large numbers in BPRNT  
[TKN1 (Game data)](../game_data/variable/tkn1.md)| The first extended token table for recursive tokens 0-255 (DETOK)  
[TKN2](../main/variable/tkn2.md)| The extended two-letter token lookup table  
  
## Universe  
\--------  
  
[spasto](../main/variable/spasto.md)| Contains the address of the Coriolis space station's ship blueprint  
[UNIV](../main/variable/univ.md)| Table of pointers to the local universe's ship data blocks  
  
## Utility routines  
\----------------  
  
[F%](../main/variable/f_per_cent.md)| Denotes the end of the main game code, from ELITE A to ELITE H  
[G%](../main/variable/g_per_cent.md)| Denotes the start of the main game code, from ELITE A to ELITE H  
  
## Workspace variables  
\-------------------  
  
[ALP1](../main/workspace/zp.md#alp1)|  Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31  
[ALP2](../main/workspace/zp.md#alp2)|  Bit 7 of ALP2 = sign of the roll angle in ALPHA  
[ALPHA](../main/workspace/zp.md#alpha)|  The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31  
[ALTIT](../main/workspace/wp.md#altit)|  Our altitude above the surface of the planet or sun  
[ASH](../main/workspace/zp.md#ash)|  Aft shield status  
[auto](../main/workspace/wp.md#auto)|  Docking computer activation status  
[AVL](../main/workspace/wp.md#avl)|  Market availability in the current system  
[BALI](../main/workspace/wp.md#bali)|  This byte appears to be unused  
[BET1](../main/workspace/zp.md#bet1)|  The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8  
[BET2](../main/workspace/zp.md#bet2)|  Bit 7 of BET2 = sign of the pitch angle in BETA  
[BETA](../main/workspace/zp.md#beta)|  The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8  
[BOMB](../main/workspace/wp.md#bomb)|  Energy bomb  
[boxsize](../main/workspace/wp.md#boxsize)|  This byte appears to be unused  
[BST](../main/workspace/wp.md#bst)|  Fuel scoops (BST stands for "barrel status")  
[BSTK](../main/workspace/up.md#bstk)|  Bitstik configuration setting  
[BUF](../main/workspace/wp.md#buf)|  The line buffer used by DASC to print justified text  
[CABTMP](../main/workspace/wp.md#cabtmp)|  Cabin temperature  
[CASH](../main/workspace/wp.md#cash)|  Our current cash pot  
[CATF](../main/workspace/up.md#catf)|  The disc catalogue flag  
[CNT](../main/workspace/zp.md#cnt)|  Temporary storage, typically used for storing the number of iterations required when looping  
[CNT2](../main/workspace/zp.md#cnt2)|  Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start  
[COK](../main/workspace/wp.md#cok)|  Flags used to generate the competition code  
[COL](../main/workspace/zp.md#col)|  Temporary storage, used to store colour information when drawing pixels in the dashboard  
[COMC](../main/workspace/up.md#comc)|  The colour of the dot on the compass  
[COMX](../main/workspace/wp.md#comx)|  The x-coordinate of the compass dot  
[COMY](../main/workspace/wp.md#comy)|  The y-coordinate of the compass dot  
[CRGO](../main/workspace/wp.md#crgo)|  Our ship's cargo capacity  
[DAMP](../main/workspace/up.md#damp)|  Keyboard damping configuration setting  
[de](../main/workspace/wp.md#de)|  Equipment destruction flag  
[DELT4](../main/workspace/zp.md#delt4)|  Our current speed * 64 as a 16-bit value  
[DELTA](../main/workspace/zp.md#delta)|  Our current speed, in the range 1-40  
[DFLAG](../main/workspace/up.md#dflag)|  This byte appears to be unused  
[dialc](../main/workspace/wp.md#dialc)|  These bytes are unused in this version of Elite  
[dials](../main/workspace/up.md#dials)|  These bytes appear to be unused  
[DISK](../main/workspace/up.md#disk)|  The configuration setting for toggle key "T", which isn't actually used but is still updated by pressing "T" while the game is paused. This is a configuration option from the Commodore 64 version of Elite that lets you switch between tape and disc  
[distaway](../main/workspace/wp.md#distaway)|  Used to store the nearest distance of the rotating ship on the title screen  
[DJD](../main/workspace/up.md#djd)|  Keyboard auto-recentre configuration setting  
[DKCMP](../main/workspace/wp.md#dkcmp)|  Docking computer  
[DL](../main/workspace/zp.md#dl)|  Vertical sync flag  
[DLY](../main/workspace/wp.md#dly)|  In-flight message delay  
[DNOIZ](../main/workspace/up.md#dnoiz)|  Sound on/off configuration setting  
[dontclip](../main/workspace/zp.md#dontclip)|  This is set to 0 in the RES2 routine, but the value is never actually read (this is left over from the Commodore 64 version of Elite)  
[ECM](../main/workspace/wp.md#ecm)|  E.C.M. system  
[ECMA](../main/workspace/zp.md#ecma)|  The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating:  
[ECMP](../main/workspace/wp.md#ecmp)|  Our E.C.M. status  
[ENERGY](../main/workspace/zp.md#energy)|  Energy bank status  
[ENGY](../main/workspace/wp.md#engy)|  Energy unit  
[ESCP](../main/workspace/wp.md#escp)|  Escape pod  
[EV](../main/workspace/wp.md#ev)|  The "extra vessels" spawning counter  
[FIST](../main/workspace/wp.md#fist)|  Our legal status (FIST stands for "fugitive/innocent status"):  
[FLAG](../main/workspace/zp.md#flag)|  A flag that's used to define whether this is the first call to the ball line routine in BLINE, so it knows whether to wait for the second call before storing segment data in the ball line heap  
[FLH](../main/workspace/up.md#flh)|  Flashing console bars configuration setting  
[FRIN](../main/workspace/wp.md#frin)|  Slots for the ships in the local bubble of universe  
[frump](../main/workspace/wp.md#frump)|  Used to store the number of particles in the explosion cloud, though the number is never actually used  
[FSH](../main/workspace/zp.md#fsh)|  Forward shield status  
[GCNT](../main/workspace/wp.md#gcnt)|  The number of the current galaxy (0-7)  
[GHYP](../main/workspace/wp.md#ghyp)|  Galactic hyperdrive  
[GNTMP](../main/workspace/wp.md#gntmp)|  Laser temperature (or "gun temperature")  
[gov](../main/workspace/wp.md#gov)|  The current system's government type (0-7)  
[HFX](../main/workspace/wp.md#hfx)|  A flag that toggles the hyperspace colour effect  
[INF](../main/workspace/zp.md#inf)|  Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK  
[INWK](../main/workspace/zp.md#inwk)|  The zero-page internal workspace for the current ship data block  
[JOPOS](../main/workspace/wp.md#jopos)|  Contains the high byte of the latest value from ADC channel 1 (the joystick X value), which gets updated regularly by the IRQ1 interrupt handler  
[JSTE](../main/workspace/up.md#jste)|  Reverse both joystick channels configuration setting  
[JSTGY](../main/workspace/up.md#jstgy)|  Reverse joystick Y-channel configuration setting  
[JSTK](../main/workspace/up.md#jstk)|  Keyboard or joystick configuration setting  
[JSTX](../main/workspace/zp.md#jstx)|  Our current roll rate  
[JSTY](../main/workspace/zp.md#jsty)|  Our current pitch rate  
[JUNK](../main/workspace/wp.md#junk)|  The amount of junk in the local bubble  
[K](../main/workspace/zp.md#k)|  Temporary storage, used in a number of places  
[K2](../main/workspace/zp.md#k2)|  Temporary storage, used in a number of places  
[K3](../main/workspace/zp.md#k3)|  Temporary storage, used in a number of places  
[K4](../main/workspace/zp.md#k4)|  Temporary storage, used in a number of places  
[K5](../main/workspace/zp.md#k5)|  Temporary storage used to store segment coordinates across successive calls to BLINE, the ball line routine  
[K6](../main/workspace/zp.md#k6)|  Temporary storage, typically used for storing coordinates during vector calculations  
[KL](../main/workspace/zp.md#kl)|  The following bytes implement a key logger that enables Elite to scan for concurrent key presses of the primary flight keys, plus a secondary flight key  
[KY1](../main/workspace/zp.md#ky1)|  "?" is being pressed (slow down)  
[KY12](../main/workspace/zp.md#ky12)|  TAB is being pressed (energy bomb)  
[KY13](../main/workspace/zp.md#ky13)|  ESCAPE is being pressed (launch escape pod)  
[KY14](../main/workspace/zp.md#ky14)|  "T" is being pressed (target missile)  
[KY15](../main/workspace/zp.md#ky15)|  "U" is being pressed (unarm missile)  
[KY16](../main/workspace/zp.md#ky16)|  "M" is being pressed (fire missile)  
[KY17](../main/workspace/zp.md#ky17)|  "E" is being pressed (activate E.C.M.)  
[KY18](../main/workspace/zp.md#ky18)|  "J" is being pressed (in-system jump)  
[KY19](../main/workspace/zp.md#ky19)|  "C" is being pressed (activate docking computer)  
[KY2](../main/workspace/zp.md#ky2)|  Space is being pressed (speed up)  
[KY20](../main/workspace/zp.md#ky20)|  "P" is being pressed (deactivate docking computer)  
[KY3](../main/workspace/zp.md#ky3)|  "<" is being pressed (roll left)  
[KY4](../main/workspace/zp.md#ky4)|  ">" is being pressed (roll right)  
[KY5](../main/workspace/zp.md#ky5)|  "X" is being pressed (pull up)  
[KY6](../main/workspace/zp.md#ky6)|  "S" is being pressed (pitch down)  
[KY7](../main/workspace/zp.md#ky7)|  "A" is being pressed (fire lasers)  
[LAS](../main/workspace/zp.md#las)|  Contains the laser power of the laser fitted to the current space view (or 0 if there is no laser fitted to the current view)  
[LAS2](../main/workspace/wp.md#las2)|  Laser power for the current laser  
[LASCT](../main/workspace/wp.md#lasct)|  The laser pulse count for the current laser  
[LASER](../main/workspace/wp.md#laser)|  The specifications of the lasers fitted to each of the four space views:  
[LASX](../main/workspace/wp.md#lasx)|  The x-coordinate of the tip of the laser line  
[LASY](../main/workspace/wp.md#lasy)|  The y-coordinate of the tip of the laser line  
[LATCH (Loader)](../loader/workspace/zp.md#latch)|  The RAM copy of the currently selected paged ROM/RAM in SHEILA &30  
[LSNUM](../main/workspace/zp.md#lsnum)|  The pointer to the current position in the ship line heap as we work our way through the new ship's edges (and the corresponding old ship's edges) when drawing the ship in the main ship-drawing routine at LL9  
[LSNUM2](../main/workspace/zp.md#lsnum2)|  The size of the existing ship line heap for the ship we are drawing in LL9, i.e. the number of lines in the old ship that is currently shown on-screen and which we need to erase  
[LSO](../main/workspace/wp.md#lso)|  The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)  
[LSP](../main/workspace/zp.md#lsp)|  The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps  
[LSX](../main/workspace/zp.md#lsx)|  LSX contains the status of the sun line heap at LSO  
[LSX2](../main/workspace/wp.md#lsx2)|  The ball line heap for storing x-coordinates  
[LSY2](../main/workspace/wp.md#lsy2)|  The ball line heap for storing y-coordinates  
[MANY](../main/workspace/wp.md#many)|  The number of ships of each type in the local bubble of universe  
[MCH](../main/workspace/wp.md#mch)|  The text token number of the in-flight message that is currently being shown, and which will be removed by the me2 routine when the counter in DLY reaches zero  
[MCNT](../main/workspace/zp.md#mcnt)|  The main loop counter  
[messXC](../main/workspace/zp.md#messxc)|  Temporary storage, used to store the text column of the in-flight message in MESS, so it can be erased from the screen at the correct time  
[MJ](../main/workspace/wp.md#mj)|  Are we in witchspace (i.e. have we mis-jumped)?  
[MOS](../main/workspace/zp.md#mos)|  Determines whether we are running on a Master Compact  
[MOS (Loader)](../loader/workspace/zp.md#mos)|  Determines whether we are running on a Master Compact  
[MSAR](../main/workspace/wp.md#msar)|  The targeting state of our leftmost missile  
[mscol](../main/workspace/up.md#mscol)|  This byte appears to be unused  
[MSTG](../main/workspace/zp.md#mstg)|  The current missile lock target  
[NAME](../main/workspace/wp.md#name)|  The current commander name  
[NEWB](../main/workspace/zp.md#newb)|  The ship's "new byte flags" (or NEWB flags)  
[newzp](../main/workspace/zp.md#newzp)|  This is used by the STARS2 routine for storing the stardust particle's delta_x value  
[NMI](../main/workspace/wp.md#nmi)|  Used to store the ID of the current owner of the NMI workspace in the NMICLAIM routine, so we can hand it back to them in the NMIRELEASE routine once we are done using it  
[NOMSL](../main/workspace/wp.md#nomsl)|  The number of missiles we have fitted (0-4)  
[NOSTM](../main/workspace/zp.md#nostm)|  The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace  
[P](../main/workspace/zp.md#p)|  Temporary storage, used in a number of places  
[P (Loader)](../loader/workspace/zp.md#p)|  Temporary storage, used in a number of places  
[PATG](../main/workspace/up.md#patg)|  Configuration setting to show the author names on the start-up screen and enable manual hyperspace mis-jumps  
[Q](../main/workspace/zp.md#q)|  Temporary storage, used in a number of places  
[Q (Loader)](../loader/workspace/zp.md#q)|  Temporary storage, used in a number of places  
[QQ0](../main/workspace/wp.md#qq0)|  The current system's galactic x-coordinate (0-256)  
[QQ1](../main/workspace/wp.md#qq1)|  The current system's galactic y-coordinate (0-256)  
[QQ10](../main/workspace/zp.md#qq10)|  The galactic y-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic y-coordinate)  
[QQ11](../main/workspace/zp.md#qq11)|  The type of the current view:  
[QQ12](../main/workspace/zp.md#qq12)|  Our "docked" status  
[QQ14](../main/workspace/wp.md#qq14)|  Our current fuel level (0-70)  
[QQ15](../main/workspace/zp.md#qq15)|  The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart  
[QQ17](../main/workspace/zp.md#qq17)|  Contains a number of flags that affect how text tokens are printed, particularly capitalisation:  
[QQ19](../main/workspace/zp.md#qq19)|  Temporary storage, used in a number of places  
[QQ2](../main/workspace/wp.md#qq2)|  The three 16-bit seeds for the current system, i.e. the one we are currently in  
[QQ20](../main/workspace/wp.md#qq20)|  The contents of our cargo hold  
[QQ21](../main/workspace/wp.md#qq21)|  The three 16-bit seeds for the current galaxy  
[QQ22](../main/workspace/zp.md#qq22)|  The two hyperspace countdown counters  
[QQ24](../main/workspace/wp.md#qq24)|  Temporary storage, used to store the current market item's price in routine TT151  
[QQ25](../main/workspace/wp.md#qq25)|  Temporary storage, used to store the current market item's availability in routine TT151  
[QQ26](../main/workspace/wp.md#qq26)|  A random value used to randomise market data  
[QQ28](../main/workspace/wp.md#qq28)|  The current system's economy (0-7)  
[QQ29](../main/workspace/wp.md#qq29)|  Temporary storage, used in a number of places  
[QQ3](../main/workspace/zp.md#qq3)|  The selected system's economy (0-7)  
[QQ4](../main/workspace/zp.md#qq4)|  The selected system's government (0-7)  
[QQ5](../main/workspace/zp.md#qq5)|  The selected system's tech level (0-14)  
[QQ6](../main/workspace/zp.md#qq6)|  The selected system's population in billions * 10 (1-71), so the maximum population is 7.1 billion  
[QQ7](../main/workspace/zp.md#qq7)|  The selected system's productivity in M CR (96-62480)  
[QQ8](../main/workspace/zp.md#qq8)|  The distance from the current system to the selected system in light years * 10, stored as a 16-bit number  
[QQ9](../main/workspace/zp.md#qq9)|  The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)  
[R](../main/workspace/zp.md#r)|  Temporary storage, used in a number of places  
[RAND](../main/workspace/zp.md#rand)|  Four 8-bit seeds for the random number generation system implemented in the DORND routine  
[RAT](../main/workspace/zp.md#rat)|  Used to store different signs depending on the current space view, for use in calculating stardust movement  
[RAT2](../main/workspace/zp.md#rat2)|  Temporary storage, used to store the pitch and roll signs when moving objects and stardust  
[S](../main/workspace/zp.md#s)|  Temporary storage, used in a number of places  
[safehouse](../main/workspace/wp.md#safehouse)|  Backup storage for the seeds for the selected system  
[SC](../main/workspace/zp.md#sc)|  Screen address (low byte)  
[SCH](../main/workspace/zp.md#sch)|  Screen address (high byte)  
[SLSP](../main/workspace/wp.md#slsp)|  The address of the bottom of the ship line heap  
[SOCNT](../main/workspace/sound_variables.md#socnt)|  Sound buffer for sound effect counters  
[SOFLG](../main/workspace/sound_variables.md#soflg)|  Sound buffer for sound effect flags  
[SOFRCH](../main/workspace/sound_variables.md#sofrch)|  Sound buffer for frequency change values  
[SOFRQ](../main/workspace/sound_variables.md#sofrq)|  Sound buffer for sound effect frequencies  
[SOPR](../main/workspace/sound_variables.md#sopr)|  Sound buffer for sound effect priorities  
[SOVCH](../main/workspace/sound_variables.md#sovch)|  Sound buffer for the volume change rate  
[SOVOL](../main/workspace/sound_variables.md#sovol)|  Sound buffer for volume levels  
[SSPR](../main/workspace/wp.md#sspr)|  "Space station present" flag  
[STP](../main/workspace/zp.md#stp)|  The step size for drawing circles  
[SUNX](../main/workspace/zp.md#sunx)|  The 16-bit x-coordinate of the vertical centre axis of the sun (which might be off-screen)  
[SVC](../main/workspace/wp.md#svc)|  The save count  
[SWAP](../main/workspace/wp.md#swap)|  Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)  
[SX](../main/workspace/wp.md#sx)|  This is where we store the x_hi coordinates for all the stardust particles  
[SXL](../main/workspace/wp.md#sxl)|  This is where we store the x_lo coordinates for all the stardust particles  
[SY](../main/workspace/wp.md#sy)|  This is where we store the y_hi coordinates for all the stardust particles  
[SYL](../main/workspace/wp.md#syl)|  This is where we store the y_lo coordinates for all the stardust particles  
[SZ](../main/workspace/wp.md#sz)|  This is where we store the z_hi coordinates for all the stardust particles  
[SZL](../main/workspace/wp.md#szl)|  This is where we store the z_lo coordinates for all the stardust particles  
[T](../main/workspace/zp.md#t)|  Temporary storage, used in a number of places  
[T (Loader)](../loader/workspace/zp.md#t)|  Temporary storage, used in a number of places  
[T1](../main/workspace/zp.md#t1)|  Temporary storage, used in a number of places  
[T2](../main/workspace/zp.md#t2)|  This byte appears to be unused  
[T3](../main/workspace/zp.md#t3)|  This byte appears to be unused  
[T4](../main/workspace/zp.md#t4)|  This byte appears to be unused  
[TALLY](../main/workspace/wp.md#tally)|  Our combat rank  
[TALLYL](../main/workspace/wp.md#tallyl)|  Combat rank fraction  
[tek](../main/workspace/wp.md#tek)|  The current system's tech level (0-14)  
[TGT](../main/workspace/zp.md#tgt)|  Temporary storage, typically used as a target value for counters when drawing explosion clouds and partial circles  
[TP](../main/workspace/wp.md#tp)|  The current mission status  
[TRIBBLE](../main/workspace/wp.md#tribble)|  The number of Trumbles in the cargo hold  
[TYPE](../main/workspace/zp.md#type)|  The current ship type  
[U](../main/workspace/zp.md#u)|  Temporary storage, used in a number of places  
[UPO](../main/workspace/wp.md#upo)|  This byte appears to be unused  
[UPTOG](../main/workspace/up.md#uptog)|  The configuration setting for toggle key "U", which isn't actually used but is still updated by pressing "U" while the game is paused. This is a configuration option from the Apple II version of Elite that lets you switch between lower-case and upper-case text  
[V](../main/workspace/zp.md#v)|  Temporary storage, typically used for storing an address pointer  
[VIEW](../main/workspace/wp.md#view)|  The number of the current space view  
[VOL](../main/workspace/up.md#vol)|  The volume level for the game's sound effects (0-7)  
[W](../main/workspace/zp.md#w)|  Temporary storage, used in a number of places  
[widget](../main/workspace/zp.md#widget)|  Temporary storage, used to store the original argument in A in the logarithmic FMLTU and LL28 routines  
[X1](../main/workspace/zp.md#x1)|  Temporary storage, typically used for x-coordinates in the line-drawing routines  
[X2](../main/workspace/zp.md#x2)|  Temporary storage, typically used for x-coordinates in the line-drawing routines  
[XC](../main/workspace/zp.md#xc)|  The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32  
[XP](../main/workspace/wp.md#xp)|  This byte appears to be unused  
[XSAV](../main/workspace/zp.md#xsav)|  Temporary storage for saving the value of the X register, used in a number of places  
[XSAV2](../main/workspace/wp.md#xsav2)|  This byte appears to be unused  
[XX](../main/workspace/zp.md#xx)|  Temporary storage, typically used for storing a 16-bit x-coordinate  
[XX0](../main/workspace/zp.md#xx0)|  Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop  
[XX1](../main/workspace/zp.md#xx1)|  This is an alias for INWK that is used in the main ship-drawing routine at LL9  
[XX12](../main/workspace/zp.md#xx12)|  Temporary storage for a block of values, used in a number of places  
[XX13](../main/workspace/zp.md#xx13)|  Temporary storage, typically used in the line-drawing routines  
[XX15](../main/workspace/zp.md#xx15)|  Temporary storage, typically used for storing screen coordinates in line-drawing routines  
[XX16](../main/workspace/zp.md#xx16)|  Temporary storage for a block of values, used in a number of places  
[XX17](../main/workspace/zp.md#xx17)|  Temporary storage, used in BPRNT to store the number of characters to print, and as the edge counter in the main ship-drawing routine  
[XX18](../main/workspace/zp.md#xx18)|  Temporary storage used to store coordinates in the LL9 ship-drawing routine  
[XX19](../main/workspace/zp.md#xx19)|  XX19(1 0) shares its location with INWK(34 33), which contains the address of the ship line heap  
[XX2](../main/workspace/zp.md#xx2)|  Temporary storage, used to store the visibility of the ship's faces during the ship-drawing routine at LL9  
[XX20](../main/workspace/zp.md#xx20)|  Temporary storage, used in a number of places  
[XX24](../main/workspace/wp.md#xx24)|  This byte appears to be unused  
[XX4](../main/workspace/zp.md#xx4)|  Temporary storage, used in a number of places  
[Y1](../main/workspace/zp.md#y1)|  Temporary storage, typically used for y-coordinates in line-drawing routines  
[Y2](../main/workspace/zp.md#y2)|  Temporary storage, typically used for y-coordinates in line-drawing routines  
[YC](../main/workspace/zp.md#yc)|  The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23  
[YP](../main/workspace/wp.md#yp)|  This byte appears to be unused  
[YS](../main/workspace/wp.md#ys)|  This byte appears to be unused  
[YSAV](../main/workspace/zp.md#ysav)|  Temporary storage for saving the value of the Y register, used in a number of places  
[YSAV2](../main/workspace/wp.md#ysav2)|  This byte appears to be unused  
[Yx2M1](../main/workspace/zp.md#yx2m1)|  This is used to store the number of pixel rows in the space view minus 1, which is also the y-coordinate of the bottom pixel row of the space view (it is set to 191 in the RES2 routine)  
[YY](../main/workspace/zp.md#yy)|  Temporary storage, typically used for storing a 16-bit y-coordinate  
[YY (Loader)](../loader/workspace/zp.md#yy)|  Temporary storage, used in a number of places  
[ZZ](../main/workspace/zp.md#zz)|  Temporary storage, typically used for distance values  
  
[List of all subroutines](subroutines.md "Previous routine")[List of all workspaces](workspaces.md "Next routine")
