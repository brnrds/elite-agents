---
title: "The IKNS variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/ikns.html"
---

[TRTB%](trtb_per_cent.md "Previous routine")[FILLKL](../subroutine/fillkl.md "Next routine")
    
    
           Name: IKNS                                                    [Show more]
           Type: Variable
       Category: Keyboard
        Summary: Lookup table for in-flight keyboard controls
      Deep dive: [The key logger](https://elite.bbcelite.com/deep_dives/the_key_logger.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_h.md#header-ikns)
     Variations: See [code variations](../../related/compare/main/variable/kytb-ikns.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [FILLKL](../subroutine/fillkl.md) uses IKNS
    
    
    
    
    
    * * *
    
    
     Keyboard table for in-flight controls. This table contains the internal key
     codes for the flight keys, EOR'd with &FF to invert each bit.
    
    
    
    
    .IKNS
    
     EQUB ⅅ EOR &FF       \ E         IKNS+0    KY13     E.C.M.
     EQUB &DC EOR &FF       \ T         IKNS+1    KY10     Arm missile
     EQUB &CA EOR &FF       \ U         IKNS+2    KY11     Unarm missile
     EQUB &C8 EOR &FF       \ P         IKNS+3    KY16     Cancel docking computer
     EQUB &BE EOR &FF       \ A         IKNS+4    KY7      Fire lasers
     EQUB &BD EOR &FF       \ X         IKNS+5    KY5      Pull up
     EQUB &BA EOR &FF       \ J         IKNS+6    KY14     In-system jump
     EQUB &AE EOR &FF       \ S         IKNS+7    KY6      Pitch down
     EQUB &AD EOR &FF       \ C         IKNS+8    KY15     Docking computer
     EQUB &9F EOR &FF       \ TAB       IKNS+9    KY8      Energy bomb
     EQUB &9D EOR &FF       \ Space     IKNS+10   KY2      Speed up
     EQUB &9A EOR &FF       \ M         IKNS+11   KY12     Fire missile
     EQUB &99 EOR &FF       \ <         IKNS+12   KY3      Roll left
     EQUB &98 EOR &FF       \ >         IKNS+13   KY4      Roll right
     EQUB &97 EOR &FF       \ ?         IKNS+14   KY1      Slow down
     EQUB &8F EOR &FF       \ ESCAPE    IKNS+15   KY9      Launch escape pod
    
     EQUB &F0               \ This value just has to be higher than &80 to act as a
                            \ terminator for the IKNS matching process in FILLKL
    

[TRTB%](trtb_per_cent.md "Previous routine")[FILLKL](../subroutine/fillkl.md "Next routine")
