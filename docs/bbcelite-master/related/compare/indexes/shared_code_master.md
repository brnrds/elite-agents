---
title: "Compare code for features of BBC Master Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/compare/indexes/shared_code_master.html"
---

[Variations in 6502SP Elite](shared_code_6502sp.md "Previous routine")[Other interesting variations](shared_code_other.md "Next routine")

The following table shows code variations that are unique to the BBC Master version.

Click on a name to see all the code differences for that part of the codebase, or click on an individual variation to focus on that particular feature.

Name| Comments | [BEGIN](../main/subroutine/begin.md)  
Loader | 

  * The Master version contains an embedded copy of the default JAMESON commander file that gets loaded on start-up.[See the code variation for this feature](../main/subroutine/begin.md#compare-master-1)

  
---|---  
[BR1 (Part 1 of 2)](../main/subroutine/br1_part_1_of_2.md)  
Start and end | 

  * The rotating Cobra Mk III on the title screen is further away on the Master version compared to the other versions, so it doesn't overlap the title text as much.[See the code variation for this feature](../main/subroutine/br1_part_1_of_2.md#compare-master-1)

  
[BRIEF](../main/subroutine/brief.md)  
Missions | 

  * The Master version shows the Constrictor slightly higher up the screen during the mission 1 briefing, so it doesn't get quite so close to the briefing text.[See the code variation for this feature](../main/subroutine/brief.md#compare-master-1)

  
[CATS](../main/subroutine/cats.md)  
Save and load | 

  * The Master Compact only has one disc drive, and it uses ADFS rather than DFS, so instead of asking for a drive number, it asks for a directory name.[See the code variation for this feature](../main/subroutine/cats.md#compare-master-1)
  * As the Master Compact uses ADFS, this release has to claim and release the NMI workspace when accessing the disc, in order to prevent ADFS from corrupting that part of zero page. It does this by calling the NMICLAIM and NMIRELEASE routines at the appropriate time.[See the code variation for this feature](../main/subroutine/cats.md#compare-master-2)

  
[DEATH](../main/subroutine/death.md)  
Start and end | 

  * In the cassette, disc and 6502SP versions, our ship is given a gentle pitch up or down when we die; the same is true in the Master version, but the pitch is always downwards so the detritus of our death always rises to the top of the screen.[See the code variation for this feature](../main/subroutine/death.md#compare-master-1)

  
[DELT](../main/subroutine/delt.md)  
Save and load | 

  * When deleting a file via the disc menu, the disc and 6502SP versions ask for the "FILE TO DELETE?", while the Master version asks for the "COMMANDER'S NAME?".[See the code variation for this feature](../main/subroutine/delt.md#compare-master-1)

  
[DIALS (Part 3 of 4)](../main/subroutine/dials_part_3_of_4.md)  
Dashboard | 

  * The Master version updates all the dials on every iteration of the main loop, while the other versions only update the speed, pitch and roll indicators on every loop (the other indicators only update every four iterations of the main loop).[See the code variation for this feature](../main/subroutine/dials_part_3_of_4.md#compare-master-1)

  
[DK4](../main/subroutine/dk4.md)  
Keyboard | 

  * The Master version allows you to change the volume of the sound effects using the "<" and ">" keys when the game is paused.[See the code variation for this feature](../main/subroutine/dk4.md#compare-master-1)
  * The Master version makes two beeps when the Bitstik is configured and one when it is disabled, while the disc and 6502SP versions remain totally silent and give no clue as to whether you just turned the Bitstik on or off.[See the code variation for this feature](../main/subroutine/dk4.md#compare-master-2)

  
[DKS3](../main/subroutine/dks3.md)  
Keyboard | 

  * When toggling a configuration option in the Master version, we get two beeps when we turn the option on (which sounds like one long beep) and one beep when we turn it off (which sounds like a short beep), whereas the other versions beep when we turn it on and remain silent when we turn it off.[See the code variation for this feature](../main/subroutine/dks3.md#compare-master-1)

  
[DOEXP](../main/subroutine/doexp.md)  
Drawing ships | 

  * In the Master version, explosions are made up of yellow, red and cyan particles.[See the code variation for this feature](../main/subroutine/doexp.md#compare-master-1)

  
[DOKEY](../main/subroutine/dokey.md)  
Keyboard | 

  * The Master Compact release supports the Compact's digital joystick via the RDJOY routine, which also supports the standard analogue joystick interface.[See the code variation for this feature](../main/subroutine/dokey.md#compare-master-1)

  
[ee3](../main/subroutine/ee3.md)  
Flight | 

  * The Master version's hyperspace countdown is padded to 3 characters rather than 5, so it appears two characters to the left compared to the disc and 6502SP versions.[See the code variation for this feature](../main/subroutine/ee3.md#compare-master-1)

  
[ESCAPE](../main/subroutine/escape.md)  
Flight | 

  * In the Master version, if you launch your escape pod while looking out of the side or rear views, you won't see your Cobra as you leave it behind, while in the other versions you do.[See the code variation for this feature](../main/subroutine/escape.md#compare-master-1)

  
[EXNO2](../main/subroutine/exno2.md)  
Status | 

  * The Master version incorporates a fractional kill tally when calculating the combat rank, so each ship type has a different number of kill points, while all the other versions count one point for each kill.[See the code variation for this feature](../main/subroutine/exno2.md#compare-master-1)

  
[EXNO3](../main/subroutine/exno3.md)  
Sound | 

  * The Master version has a unique explosion sound.[See the code variation for this feature](../main/subroutine/exno3.md#compare-master-1)

  
[Ghy](../main/subroutine/ghy.md)  
Flight | 

  * In the Master version, the internal galaxy number can be set to be greater than 16, and it will stay high even if you jump to the next galaxy (though it isn't clear what this is for, as the game doesn't set the galaxy to more than 7 at any point, so perhaps this was for an expansion that never happened).[See the code variation for this feature](../main/subroutine/ghy.md#compare-master-1)

  
[gnum](../main/subroutine/gnum.md)  
Market | 

  * If you try to enter a number that is too big when buying or selling in the Master version, all your key presses are displayed, whereas in the other versions the key press that pushes you over the edge is not shown.[See the code variation for this feature](../main/subroutine/gnum.md#compare-master-1)

  
[HATB](../main/variable/hatb.md)  
Ship hangar | 

  * In the disc and 6502SP versions, the first ship hangar group (of four) consists of a Transporter on the right and a Shuttle on the left, while in the Master version the first group has a lone Cobra Mk III on the left.[See the code variation for this feature](../main/variable/hatb.md#compare-master-1)
  * In the disc and 6502SP versions, the fourth ship hangar group (of four) consists of a Viper on the right and a Krait on the left, while in the Master version the fourth group has an Adder on the right and a Viper on the left.[See the code variation for this feature](../main/variable/hatb.md#compare-master-2)

  
[JMTB](../main/variable/jmtb.md)  
Text | 

  * The Master version has an extended jump token for displaying the non-selected file system, though this token isn't actually used as the file system can't be changed from disc.[See the code variation for this feature](../main/variable/jmtb.md#compare-master-1)

  
[KILLSHP](../main/subroutine/killshp.md)  
Universe | 

  * In the Master version, killing the Constrictor at the end of mission 1 instantly gives you 256 kill points.[See the code variation for this feature](../main/subroutine/killshp.md#compare-master-1)

  
[KYTB / IKNS](../main/variable/kytb-ikns.md)  
Keyboard | 

  * The Master has a very different set of internal key numbers to the BBC Micro and Electron, so the keyboard lookup table for the Master is also very different; the Electron and BBC Micro are much more similar, but the Electron has no TAB key, so "-" launches an energy bomb instead.[See the code variation for this feature](../main/variable/kytb-ikns.md#compare-master-1)

  
[LL164](../main/subroutine/ll164.md)  
Drawing circles | 

  * The Master version has a unique hyperdrive sound.[See the code variation for this feature](../main/subroutine/ll164.md#compare-master-1)

  
[LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md)  
Drawing ships | 

  * The Master has a flicker-free ship plotting algorithm that plots and erases ship lines one line at a time. As part the new algorithm, it stores its progress while working its way through the ship line heap in the new variables at LSNUM and LSNUM2.[See the code variation for this feature](../main/subroutine/ll9_part_1_of_12.md#compare-master-1)

  
[LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md)  
Drawing ships | 

  * When drawing a ship, the cassette, disc and 6502SP versions erase the entire on-screen ship before redrawing it, while the Master version erases and redraws each ship one line at a time.[See the code variation for this feature](../main/subroutine/ll9_part_9_of_12.md#compare-master-1)
  * The cassette, disc and 6502SP versions draw the laser lines along with the ships, whereas the Master version erases and draws lines individually, including the laser lines.[See the code variation for this feature](../main/subroutine/ll9_part_9_of_12.md#compare-master-2)

  
[LL9 (Part 10 of 12)](../main/subroutine/ll9_part_10_of_12.md)  
Drawing ships | 

  * The Master implements flicker-free ship drawing using the LSPUT routine, which manages the erasing and drawing of individual lines.[See the code variation for this feature](../main/subroutine/ll9_part_10_of_12.md#compare-master-1)

  
[LL9 (Part 11 of 12)](../main/subroutine/ll9_part_11_of_12.md)  
Drawing ships | 

  * The cassette, disc and 6502SP versions add all the lines in a ship to the heap and then draw them all in one go, whereas the Master version erases and draws lines as they are added to the ship line heap.[See the code variation for this feature](../main/subroutine/ll9_part_11_of_12.md#compare-master-1)

  
[LL9 (Part 12 of 12)](../main/subroutine/ll9_part_12_of_12.md)  
Drawing ships | 

  * The cassette, disc and 6502SP versions do all their line drawing at the very end of the LL9 ship-drawing routine, but the Master has already redrawn most of the ship by this point and only needs to erase any remaining lines.[See the code variation for this feature](../main/subroutine/ll9_part_12_of_12.md#compare-master-1)

  
[LOOK1](../main/subroutine/look1.md)  
Flight | 

  * The Master has a unique lightning bolt effect for the energy bomb.[See the code variation for this feature](../main/subroutine/look1.md#compare-master-1)

  
[Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md)  
Main loop | 

  * The Master version has a unique "low beep" sound that has more reverb than in the other versions.[See the code variation for this feature](../main/subroutine/main_flight_loop_part_3_of_16.md#compare-master-1)
  * The Master's energy bomb lightning bolt effect contains nine random zig-zag lines that are set up in the BOMBON routine.[See the code variation for this feature](../main/subroutine/main_flight_loop_part_3_of_16.md#compare-master-2)
  * The Master version has a unique sound for when our laser is firing.[See the code variation for this feature](../main/subroutine/main_flight_loop_part_3_of_16.md#compare-master-3)

  
[Main flight loop (Part 5 of 16)](../main/subroutine/main_flight_loop_part_5_of_16.md)  
Main loop | 

  * In the Master version, energy bombs have no effect against Thargoids.[See the code variation for this feature](../main/subroutine/main_flight_loop_part_5_of_16.md#compare-master-1)
  * The Master version awards different numbers of kill points to all the different types of ship that the energy bomb kills.[See the code variation for this feature](../main/subroutine/main_flight_loop_part_5_of_16.md#compare-master-2)

  
[Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md)  
Main loop | 

  * The Master version awards different kill points depending on the type of the ship that we kill, ranging from 0.03125 points for a splinter to 5.33203125 points for a Constrictor or Cougar.[See the code variation for this feature](../main/subroutine/main_flight_loop_part_11_of_16.md#compare-master-1)

  
[Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md)  
Main loop | 

  * The Master version's energy bomb lightning bolt flashes on the screen, just like real lightning.[See the code variation for this feature](../main/subroutine/main_flight_loop_part_13_of_16.md#compare-master-1)

  
[Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md)  
Main loop | 

  * The Master version has a unique E.C.M. sound.[See the code variation for this feature](../main/subroutine/main_flight_loop_part_16_of_16.md#compare-master-1)

  
[Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md)  
Main loop | 

  * In the disc and 6502SP versions there's a 22% chance of spawning a Thargoid during mission 2, while it's a 14% chance in the Master version.[See the code variation for this feature](../main/subroutine/main_game_loop_part_4_of_6.md#compare-master-1)

  
[me2](../main/subroutine/me2.md)  
Flight | 

  * The Master version clears the bottom part of the screen when displaying in-flight messages in screens other than the space view, whereas the other versions messily superimpose the message over the current screen.[See the code variation for this feature](../main/subroutine/me2.md#compare-master-1)

  
[MESS](../main/subroutine/mess.md)  
Flight | 

  * The Master version flashes in-flight messages 10% more quickly than the other versions.[See the code variation for this feature](../main/subroutine/mess.md#compare-master-1)

  
[MJP](../main/subroutine/mjp.md)  
Flight | 

  * The Master version spawns three Thargoid motherships in witchspace, while the other versions spawn four.[See the code variation for this feature](../main/subroutine/mjp.md#compare-master-1)

  
[MT26](../main/subroutine/mt26.md)  
Text | 

  * When entering text in the Master version, the text that is typed is shown in magenta, while it is shown in white in the other versions.[See the code variation for this feature](../main/subroutine/mt26.md#compare-master-1)
  * The Master version contains its own custom routine for the extended token that asks for a line of text, while the other enhanced versions use the standard OSWORD command. The Master Compact version of that custom routine supports more characters, as it supports ADFS rather than DFS.[See the code variation for this feature](../main/subroutine/mt26.md#compare-master-2)

  
[NA% / NA2%](../main/variable/na_per_cent-na2_per_cent.md)  
Save and load | 

  * The Master version contains an embedded copy of the default JAMESON commander file that can be restored over the current commander via the disc access menu.[See the code variation for this feature](../main/variable/na_per_cent-na2_per_cent.md#compare-master-1)

  
[NOISE](../main/subroutine/noise.md)  
Sound | 

  * The Master supports a much more sophisticated interrupt-driven sound system rather than the standard sound envelope system that the other versions use.[See the code variation for this feature](../main/subroutine/noise.md#compare-master-1)

  
[PAS1](../main/subroutine/pas1.md)  
Missions | 

  * In the Master version, the rotating Constrictor in the mission 1 briefing is slightly higher up the screen than in the other versions.[See the code variation for this feature](../main/subroutine/pas1.md#compare-master-1)

  
[PLL1 (Part 1 of 3)](../loader/subroutine/pll1_part_1_of_3.md)  
Drawing planets | 

  * For most versions, the loading screen's Saturn is drawn randomly, so the dots are different every time the game loads. However, the Master version always draws exactly the same pixels for the Saturn, as the random number generator gets seeded to the same value every time.[See the code variation for this feature](../loader/subroutine/pll1_part_1_of_3.md#compare-master-1)

  
[scacol](../main/variable/scacol.md)  
Drawing ships | 

  * In the Master version, the Cougar has a cloaking device that hides it from the scanner, unlike in the 6502 Second Processor version, where the Cougar appears on the scanner in cyan.[See the code variation for this feature](../main/variable/scacol.md#compare-master-1)

  
[SCAN](../main/subroutine/scan.md)  
Dashboard | 

  * In most versions, ships that are exactly ahead of us or behind us are shown on the 3D scanner so the stick goes from the dot onto the centre line of the ellipse, but in the Master version the dot is moved over to the right so the stick goes from the dot just to the right of the centre line.[See the code variation for this feature](../main/subroutine/scan.md#compare-master-1)
  * The Master version's 3D scanner draws a dash at the end of each stick (i.e. one pixel high, two pixels wide), while the other versions draw a full dot (i.e. two pixels high, two pixels wide).[See the code variation for this feature](../main/subroutine/scan.md#compare-master-2)

  
[SFRMIS](../main/subroutine/sfrmis.md)  
Tactics | 

  * The Master version has a unique missile launch sound.[See the code variation for this feature](../main/subroutine/sfrmis.md#compare-master-1)

  
[SHPPT](../main/subroutine/shppt.md)  
Drawing ships | 

  * When drawing a distant ship as a dot, the cassette, disc and 6502SP versions erase the entire on-screen ship before redrawing it, while the Master version erases and redraws each ship one line at a time.[See the code variation for this feature](../main/subroutine/shppt.md#compare-master-1)
  * The Master implements flicker-free ship drawing using the LSPUT routine, which is used both for drawing wireframes and for drawing distant ships as dots.[See the code variation for this feature](../main/subroutine/shppt.md#compare-master-2)

  
[SIGHT](../main/subroutine/sight.md)  
Flight | 

  * In the Master version, the laser crosshairs are different colours for the different laser types.[See the code variation for this feature](../main/subroutine/sight.md#compare-master-1)

  
[SOLAR](../main/subroutine/solar.md)  
Universe | 

  * The Master version runs an extra bit of code when arriving in a new system that is responsible for breeding any Trumbles in the hold, though as there are no Trumbles in the BBC versions of the game, this never actually breeds anything.[See the code variation for this feature](../main/subroutine/solar.md#compare-master-1)

  
[STARS2](../main/subroutine/stars2.md)  
Stardust | 

  * The side-view stardust routine in the Master version was recoded to cope with arbitrary screen widths, code that was presumably inherited from the non-BBC versions of the game.[See the code variation for this feature](../main/subroutine/stars2.md#compare-master-1)

  
[STATUS](../main/subroutine/status.md)  
Status | 

  * The Master version shows the escape pod by name in the Status Mode screen but doesn't show the large cargo bay; the Electron version is similar (though it shows it as "Escape Capsule"), while the other versions do show the large cargo bay but don't show the escape pod.[See the code variation for this feature](../main/subroutine/status.md#compare-master-1)

  
[SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md)  
Drawing suns | 

  * In the Master version, the screen size is not hard-coded, but is stored in a dedicated location, an approach that was presumably inherited from the non-BBC versions of the game.[See the code variation for this feature](../main/subroutine/sun_part_1_of_4.md#compare-master-1)

  
[SUN (Part 2 of 4)](../main/subroutine/sun_part_2_of_4.md)  
Drawing suns | 

  * In the Master version, the screen size is not hard-coded, but is stored in a dedicated location, an approach that was presumably inherited from the non-BBC versions of the game.[See the code variation for this feature](../main/subroutine/sun_part_2_of_4.md#compare-master-1)

  
[SVE](../main/subroutine/sve.md)  
Save and load | 

  * The Master version doesn't show the competition number when saving, as the competition closed some time before the Master came on the scene.[See the code variation for this feature](../main/subroutine/sve.md#compare-master-1)

  
[TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md)  
Tactics | 

  * In the Master version, destroying a missile using E.C.M. gives us the same number of fractional kill points as killing an alloy plate, while the other versions always award one point, whatever is killed.[See the code variation for this feature](../main/subroutine/tactics_part_1_of_7.md#compare-master-1)
  * In the Master version, the tactics routine is slightly more efficient as it skips the NEWB logic checks for missiles, which aren't relevant (the disc and 6502SP versions still perform the checks, though this has no effect).[See the code variation for this feature](../main/subroutine/tactics_part_1_of_7.md#compare-master-2)
  * In the Master version, destroying a missile (not using E.C.M.) gives us a number of kill points that depends on the missile's target slot number, and therefore is fairly random (this also affects the Commodore 64 and Apple II versions, but was fixed in the NES version).[See the code variation for this feature](../main/subroutine/tactics_part_1_of_7.md#compare-master-3)

  
[TACTICS (Part 2 of 7)](../main/subroutine/tactics_part_2_of_7.md)  
Tactics | 

  * In the Master version, rock hermits spawn ships that are hostile and innocent bystanders.[See the code variation for this feature](../main/subroutine/tactics_part_2_of_7.md#compare-master-1)

  
[TACTICS (Part 4 of 7)](../main/subroutine/tactics_part_4_of_7.md)  
Tactics | 

  * In the Master version, escape pods that other ships drop do not inherit the parent ship's characteristics (i.e. trader, bounty hunter, hostile, pirate).[See the code variation for this feature](../main/subroutine/tactics_part_4_of_7.md#compare-master-1)

  
[TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md)  
Tactics | 

  * In the Master version, if we are being hit by lasers, the ship firing at us can still manoeuvre, whereas in the other versions enemies mysteriously forget to move if they manage to hit us.[See the code variation for this feature](../main/subroutine/tactics_part_6_of_7.md#compare-master-1)

  
[TITLE](../main/subroutine/title.md)  
Start and end | 

  * In the Master version, the Cobra Mk III shown on the first title page is further away than in the other versions, which is implemented by a new variable that contains the distance that the ship should be shown at.[See the code variation for this feature](../main/subroutine/title.md#compare-master-1)
  * The Master version shows the "Load New Commander (Y/N)?" prompt on row 20, while the other versions show it one line lower, on row 21.[See the code variation for this feature](../main/subroutine/title.md#compare-master-2)
  * The Master version shows the copyright year on the title screen as 1986 rather than 1984.[See the code variation for this feature](../main/subroutine/title.md#compare-master-3)

  
[TKN1](../main/variable/tkn1.md)  
Text | 

  * In the Master version, option 3 in the disc access menu is "Catalogue Disk" rather than just "Catalogue".[See the code variation for this feature](../main/variable/tkn1.md#compare-master-1)
  * In the Master version, the disc access menu has an extra option, "Default JAMESON", which resets the commander to the default starting point.[See the code variation for this feature](../main/variable/tkn1.md#compare-master-2)
  * Text token 12 in the Master version has a copyright notice of "(C) Acornsoft 1986", rather than the "1984" of the other versions.[See the code variation for this feature](../main/variable/tkn1.md#compare-master-3)
  * The disc and 6502SP versions contain a spelling mistake in the extended token system - they incorrectly spell weird as "wierd". The correct spelling is used in the Master version.[See the code variation for this feature](../main/variable/tkn1.md#compare-master-4)
  * The Master Compact release replaces the "DRIVE" text tokwn with "DIRECTORY", as the Compact only has one disc drive.[See the code variation for this feature](../main/variable/tkn1.md#compare-master-5)
  * The Master version indents the "INCOMING MESSAGE" shown during mission briefings to the right by one space.[See the code variation for this feature](../main/variable/tkn1.md#compare-master-6)
  * The disc and 6502SP versions contain a spelling mistake in the mission 2 briefing that's shown when picking up the plans from Ceerdi - they incorrectly spell intelligence as "intellegence". The correct spelling is used in the Master version.[See the code variation for this feature](../main/variable/tkn1.md#compare-master-7)
  * The disc and 6502SP versions call the Thargoids "those mothers" in the mission 2 briefing that's shown when picking up the plans from Ceerdi. In the Master version, this has changed to "those murderers".[See the code variation for this feature](../main/variable/tkn1.md#compare-master-8)
  * The Master version contains a new prompt in the extended token table, "ARE YOU SURE?", which isn't present in the other versions. It is used to make sure you really do want to revert to the default commander if you choose that option from the disc access menu.[See the code variation for this feature](../main/variable/tkn1.md#compare-master-9)
  * The Master version contains a new prompt in the extended token table, "{currently selected media} ERROR", which isn't present in the other versions, though it isn't actually used (it was left over from the Commodore 64 version of the game).[See the code variation for this feature](../main/variable/tkn1.md#compare-master-10)

  
[tnpr](../main/subroutine/tnpr.md)  
Market | 

  * The Master version contains the code for Trumbles to take up cargo space, though as we never actually get given any Trumbles, the value is always zero.[See the code variation for this feature](../main/subroutine/tnpr.md#compare-master-1)

  
[TT103](../main/subroutine/tt103.md)  
Charts | 

  * The moveable chart crosshairs in the Master version are drawn with white/yellow vertical stripes (with the exception of the static crosshairs on the Long-range Chart, which are white). All crosshairs are white in the other versions.[See the code variation for this feature](../main/subroutine/tt103.md#compare-master-1)
  * Master version contains code to scale the crosshairs on the chart views, though it has no effect in this version. The code is left over from the Apple II version, which uses a different scale.[See the code variation for this feature](../main/subroutine/tt103.md#compare-master-2)

  
[TT105](../main/subroutine/tt105.md)  
Charts | 

  * In most versions the Short-range Chart crosshairs can be moved to the right edge of the screen, but in the Master version they disappear before they get to the edge.[See the code variation for this feature](../main/subroutine/tt105.md#compare-master-1)
  * The Master version contains code to scale the chart views, though it has no effect in this version. The code is left over from the Apple II version, which uses a different scale.[See the code variation for this feature](../main/subroutine/tt105.md#compare-master-2)
  * In most versions the Short-range Chart crosshairs can be moved close to the bottom edge of the screen, but in the Master version they disappear before they get quite as far.[See the code variation for this feature](../main/subroutine/tt105.md#compare-master-3)
  * The moveable chart crosshairs in the Master version are drawn with white/yellow vertical stripes (with the exception of the static crosshairs on the Long-range Chart, which are white). All crosshairs are white in the other versions.[See the code variation for this feature](../main/subroutine/tt105.md#compare-master-4)

  
[TT14](../main/subroutine/tt14.md)  
Drawing circles | 

  * The static chart crosshairs in the Master version are drawn with white/yellow vertical stripes (with the exception of the static crosshairs on the Long-range Chart, which are white). All crosshairs are white in the other versions.[See the code variation for this feature](../main/subroutine/tt14.md#compare-master-1)
  * The Master version contains code to scale the chart views, though it has no effect in this version. The code is left over from the Apple II version, which uses a different scale.[See the code variation for this feature](../main/subroutine/tt14.md#compare-master-2)
  * The Master version uses variables to define the size of the Long-range Chart.[See the code variation for this feature](../main/subroutine/tt14.md#compare-master-3)

  
[TT15](../main/subroutine/tt15.md)  
Drawing lines | 

  * The Master version uses variables to define the size of the Long-range Chart.[See the code variation for this feature](../main/subroutine/tt15.md#compare-master-1)
  * In the Master version, the horizontal crosshair doesn't overlap the left border of the Short-range Chart, while it does in the other versions.[See the code variation for this feature](../main/subroutine/tt15.md#compare-master-2)
  * The Master version uses variables to define the size of the Long-range Chart.[See the code variation for this feature](../main/subroutine/tt15.md#compare-master-3)
  * The bottom border of the Long-range Chart is one pixel lower down the screen in the Master version than in the other versions, and it uses variables to define the chart size, so the crosshair-clipping code is slightly different too.[See the code variation for this feature](../main/subroutine/tt15.md#compare-master-4)

  
[TT17](../main/subroutine/tt17.md)  
Keyboard | 

  * The Master version doesn't check for SHIFT being held down in the space view, as it has no effect there (it's only used in the chart views for speeding up the crosshairs).[See the code variation for this feature](../main/subroutine/tt17.md#compare-master-1)
  * The Master Compact release allows you to move the chart crosshairs using the digital joystick, using the TT17X and DJOY routines.[See the code variation for this feature](../main/subroutine/tt17.md#compare-master-2)
  * The Master has different logic around moving the crosshairs on the chart views, though the results appear to be the same.[See the code variation for this feature](../main/subroutine/tt17.md#compare-master-3)

  
[TT210](../main/subroutine/tt210.md)  
Market | 

  * The Master version contains a fair amount of Trumble-related code, though it doesn't have any effect as we never get to pick up any Trumbles (though if we did, they would take over our cargo bay and hoof up all the food and narcotics, just as in the Commodore 64 version, so their essence is still encoded in the Master version).[See the code variation for this feature](../main/subroutine/tt210.md#compare-master-1)

  
[TT22](../main/subroutine/tt22.md)  
Charts | 

  * The bottom border of the Long-range Chart is one pixel lower down the screen in the Master version than in the other versions, and its position is defined using a variable.[See the code variation for this feature](../main/subroutine/tt22.md#compare-master-1)
  * The Master version contains code to scale the chart views, though it has no effect in this version. It also uses variables to define the size of the Lond-range Chart. The code is left over from the Apple II version, which uses a different scale.[See the code variation for this feature](../main/subroutine/tt22.md#compare-master-2)

  
[TT23](../main/subroutine/tt23.md)  
Charts | 

  * The Master version includes systems on the Short-range Chart if they are in the horizontal range 0-29, while the other versions include systems in the range 0-20, so the Master version can show systems that are further to the right. You can see an example of this difference in the Short-range Chart at Lave, which contains a lone system (Qutiri) out to the far right. This system isn't shown in the other versions because of their stricter horizontal distance check.[See the code variation for this feature](../main/subroutine/tt23.md#compare-master-1)
  * The Master version includes systems on the Short-range Chart if they are in the vertical range 0-40, while the other versions include systems in the range 0-38, so the Master version version can show systems that are further down the chart.[See the code variation for this feature](../main/subroutine/tt23.md#compare-master-2)
  * The Master version contains code to scale the chart views, though it has no effect in this version. The code is left over from the Apple II version, which uses a different scale.[See the code variation for this feature](../main/subroutine/tt23.md#compare-master-3)
  * The Master version only shows systems on the Short-range Chart that are within 7 light years of the current system, whereas the other versions show all systems in the vicinity, irrespective of the distance. You can see this on the Short-range Chart at Lave, where Orrere, Uszaa and Tionisla are all missing from the Master version's chart, but are present in the other versions. Interestingly, the unlabelled system on the far right (Qutiri) that slipped through the Master's more generous horizontal distance check is still shown, even though it's more than 7 light years away. This is a bug, and it's caused by the label-printing logic; because there is no room for a label for this system, the code skips label-printing by jumping to ee1 rather than TT187, inadvertently jumping to the system-drawing routine rather than moving on to the next system.[See the code variation for this feature](../main/subroutine/tt23.md#compare-master-4)

  
[TT26 / CHPR](../main/subroutine/tt26-chpr.md)  
Text | 

  * The Master Compact release prints the disc catalogue using different logic, as the Compact uses ADFS rather than DFS.[See the code variation for this feature](../main/subroutine/tt26-chpr.md#compare-master-1)

  
  
[Variations in 6502SP Elite](shared_code_6502sp.md "Previous routine")[Other interesting variations](shared_code_other.md "Next routine")
