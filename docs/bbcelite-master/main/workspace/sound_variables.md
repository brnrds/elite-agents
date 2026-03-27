---
title: "The Sound variables workspace - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/workspace/sound_variables.html"
---

[SOINT](../subroutine/soint.md "Previous routine")[SFXPR](../variable/sfxpr.md "Next routine")
    
    
           Name: Sound variables                                         [Show more]
           Type: Workspace
        Address: &144D to &1491
       Category: Sound
        Summary: The sound buffer where the data to be sent to the sound chip is
                 processed
    
    
        Context: See this workspace [in context in the source code](../../all/elite_a.md#header-sound-variables)
     References: No direct references to this workspace in this source file
    
    
    
    
    
    
    .SOFLG
    
     EQUB 0                 \ Sound buffer for sound effect flags
     EQUB 0                 \
     EQUB 0                 \ SOFLG,Y contains the following:
                            \
                            \   * Bits 0-5: sound effect number + 1 of the sound
                            \               currently being made on voice Y
                            \
                            \   * Bit 7 is set if this is a new sound being made,
                            \     rather than one that is in progress
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../subroutine/noise.md)
                            \   * [SOFLUSH](../subroutine/soflush.md)
                            \   * [SOINT](../subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOCNT
    
     EQUB 0                 \ Sound buffer for sound effect counters
     EQUB 0                 \
     EQUB 0                 \ SOCNT,Y contains the counter of the sound currently
                            \ being made on voice Y
                            \
                            \ The counter decrements each frame, and when it reaches
                            \ zero, the sound effect has finished
                            \
                            \ These values come from the SFXCNT table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../subroutine/noise.md)
                            \   * [SOINT](../subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOVOL
    
     EQUB 0                 \ Sound buffer for volume levels
     EQUB 0                 \
     EQUB 0                 \ SOVOL,Y contains the volume of the sound currently
                            \ being made on voice Y
                            \
                            \ These values come from bits 1-3 of the SFXPR table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../subroutine/noise.md)
                            \   * [SOINT](../subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOVCH
    
     EQUB 0                 \ Sound buffer for the volume change rate
     EQUB 0                 \
     EQUB 0                 \ SOVCH,Y contains the volume change rate of the sound
                            \ currently being made on voice Y
                            \
                            \ The sound's volume gets reduced by one every SOVCH,Y
                            \ frames
                            \
                            \ These values come from the SFXVCH table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../subroutine/noise.md)
                            \   * [SOINT](../subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOPR
    
     EQUB 0                 \ Sound buffer for sound effect priorities
     EQUB 0                 \
     EQUB 0                 \ SOPR,Y contains the priority of the sound currently
                            \ being made on voice Y
                            \
                            \ These values come from the SFXPR table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../subroutine/noise.md)
                            \   * [SOINT](../subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOFRCH
    
     EQUB 0                 \ Sound buffer for frequency change values
     EQUB 0                 \
     EQUB 0                 \ SOFRCH,Y contains the frequency change to be applied
                            \ to the sound currently being made on voice Y in each
                            \ frame
                            \
                            \ These values come from the SFXFRCH table
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../subroutine/noise.md)
                            \   * [SOINT](../subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SOFRQ
    
     EQUB 0                 \ Sound buffer for sound effect frequencies
     EQUB 0                 \
     EQUB 0                 \ SOFRQ,Y contains the frequency of the sound currently
                            \ being made on voice Y
                            \
                            \ These values come from the SFXFQ table, and have the
                            \ frequency change from the SFXFRCH table applied in
                            \ each frame
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [NOISE](../subroutine/noise.md)
                            \   * [SOINT](../subroutine/soint.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    

[SOINT](../subroutine/soint.md "Previous routine")[SFXPR](../variable/sfxpr.md "Next routine")
