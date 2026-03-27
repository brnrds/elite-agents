---
title: "The UP workspace - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/workspace/up.html"
---

[wtable](../variable/wtable.md "Previous routine")[TGINT](../variable/tgint.md "Next routine")
    
    
           Name: UP                                                      [Show more]
           Type: Workspace
        Address: &2C40 to &2C61
       Category: Workspaces
        Summary: Configuration variables
    
    
        Context: See this workspace [in context in the source code](../../all/elite_a.md#header-up)
     Variations: See [code variations](../../related/compare/main/workspace/up.md) for this workspace in the different versions
     References: No direct references to this workspace in this source file
    
    
    
    
    
    
    .UP
    
     SKIP 0                 \ The start of the UP workspace
    
    .COMC
    
     SKIP 1                 \ The colour of the dot on the compass
                            \
                            \   * #WHITE2 = the object in the compass is in front of
                            \     us, so the dot is white
                            \
                            \   * #GREEN2 = the object in the compass is behind us,
                            \     so the dot is green
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BEGIN](../subroutine/begin.md)
                            \   * [DOT](../subroutine/dot.md)
                            \   * [SP2](../subroutine/sp2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .dials
    
     SKIP 14                \ These bytes appear to be unused
    
    .mscol
    
     SKIP 4                 \ This byte appears to be unused
    
    .CATF
    
     SKIP 1                 \ The disc catalogue flag
                            \
                            \ Determines whether a disc catalogue is currently in
                            \ progress, so the TT26 print routine can format the
                            \ output correctly:
                            \
                            \   * 0 = disc is not currently being catalogued
                            \
                            \   * 1 = disc is currently being catalogued
                            \
                            \ Specifically, when CATF is non-zero, TT26 will omit
                            \ column 17 from the catalogue so that it will fit
                            \ on-screen (column 17 is blank column in the middle
                            \ of the catalogue, between the two lists of filenames,
                            \ so it can be dropped without affecting the layout)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CATS](../subroutine/cats.md)
                            \   * [CHPR](../subroutine/chpr.md)
                            \   * [NEWBRK](../subroutine/newbrk.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DFLAG
    
     SKIP 1                 \ This byte appears to be unused
    
    .DNOIZ
    
     SKIP 1                 \ Sound on/off configuration setting
                            \
                            \   * 0 = sound is on (default)
                            \
                            \   * Non-zero = sound is off
                            \
                            \ Toggled by pressing "S" when paused, see the DK4
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../subroutine/dk4.md)
                            \   * [NOISE](../subroutine/noise.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DAMP
    
     SKIP 1                 \ Keyboard damping configuration setting
                            \
                            \   * 0 = damping is enabled (default)
                            \
                            \   * &FF = damping is disabled
                            \
                            \ Toggled by pressing CAPS LOCK when paused, see the
                            \ DKS3 routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [cntr](../subroutine/cntr.md)
                            \   * [DKS3](../subroutine/dks3.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DJD
    
     SKIP 1                 \ Keyboard auto-recentre configuration setting
                            \
                            \   * 0 = auto-recentre is enabled (default)
                            \
                            \   * &FF = auto-recentre is disabled
                            \
                            \ Toggled by pressing "A" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [REDU2](../subroutine/redu2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .PATG
    
     SKIP 1                 \ Configuration setting to show the author names on the
                            \ start-up screen and enable manual hyperspace mis-jumps
                            \
                            \   * 0 = no author names or manual mis-jumps (default)
                            \
                            \   * &FF = show author names and allow manual mis-jumps
                            \
                            \ Toggled by pressing "X" when paused, see the DKS3
                            \ routine for details
                            \
                            \ This needs to be turned on for manual mis-jumps to be
                            \ possible. To do a manual mis-jump, first toggle the
                            \ author display by pausing the game and pressing "X",
                            \ and during the next hyperspace, hold down CTRL to
                            \ force a mis-jump. See routine ee5 for the "AND PATG"
                            \ instruction that implements this logic
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main game loop (Part 5 of 6)](../subroutine/main_game_loop_part_5_of_6.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TT18](../subroutine/tt18.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .FLH
    
     SKIP 1                 \ Flashing console bars configuration setting
                            \
                            \   * 0 = static bars (default)
                            \
                            \   * &FF = flashing bars
                            \
                            \ Toggled by pressing "F" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [PZW](../subroutine/pzw.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTGY
    
     SKIP 1                 \ Reverse joystick Y-channel configuration setting
                            \
                            \   * 0 = reversed Y-channel
                            \
                            \   * &FF = standard Y-channel (default)
                            \
                            \ Toggled by pressing "Y" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \   * [RESET](../subroutine/reset.md)
                            \   * [TT17X](../subroutine/tt17x.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTE
    
     SKIP 1                 \ Reverse both joystick channels configuration setting
                            \
                            \   * 0 = standard channels (default)
                            \
                            \   * &FF = reversed channels
                            \
                            \ Toggled by pressing "J" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../subroutine/dk4.md)
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \   * [TT17X](../subroutine/tt17x.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTK
    
     SKIP 1                 \ Keyboard or joystick configuration setting
                            \
                            \   * 0 = keyboard (default)
                            \
                            \   * &FF = joystick
                            \
                            \ Toggled by pressing "K" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../subroutine/dk4.md)
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TT17](../subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .UPTOG
    
     SKIP 1                 \ The configuration setting for toggle key "U", which
                            \ isn't actually used but is still updated by pressing
                            \ "U" while the game is paused. This is a configuration
                            \ option from the Apple II version of Elite that lets
                            \ you switch between lower-case and upper-case text
    
    .DISK
    
     SKIP 1                 \ The configuration setting for toggle key "T", which
                            \ isn't actually used but is still updated by pressing
                            \ "T" while the game is paused. This is a configuration
                            \ option from the Commodore 64 version of Elite that
                            \ lets you switch between tape and disc
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BEGIN](../subroutine/begin.md)
                            \   * [FILEPR](../subroutine/filepr.md)
                            \   * [OTHERFILEPR](../subroutine/otherfilepr.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BSTK
    
     SKIP 1                 \ Bitstik configuration setting
                            \
                            \   * 0 = keyboard or joystick (default)
                            \
                            \   * &FF = Bitstik
                            \
                            \ Toggled by pressing "B" when paused, see the DKS3
                            \ routine for details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../subroutine/dk4.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SKIP 1                 \ This byte appears to be unused
    
    .VOL
    
     EQUB 7                 \ The volume level for the game's sound effects (0-7)
                            \
                            \ This is controlled by the "<" and ">" keys while the
                            \ game is paused, and the default level is 7
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../subroutine/dk4.md)
                            \   * [SOINT](../subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     PRINT "UP workspace from ", ~UP, "to ", ~P%-1, "inclusive"
    

[wtable](../variable/wtable.md "Previous routine")[TGINT](../variable/tgint.md "Next routine")
