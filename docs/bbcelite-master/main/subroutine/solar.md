---
title: "The SOLAR subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/solar.html"
---

[SOS1](sos1.md "Previous routine")[NWSTARS](nwstars.md "Next routine")
    
    
           Name: SOLAR                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set up various aspects of arriving in a new system
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-solar)
     Variations: See [code variations](../../related/compare/main/subroutine/solar.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT18](tt18.md) calls SOLAR
    
    
    
    
    
    * * *
    
    
     Halve our legal status, update the missile indicators, and set up data blocks
     and slots for the planet and sun.
    
    
    
    
    .SOLAR
    
     LDA TRIBBLE            \ If we have no Trumbles in the hold, skip to nobirths
     BEQ nobirths
    
                            \ If we get here then we have Trumbles in the hold, so
                            \ this is where they breed (though we never get here in
                            \ the Master version as the number of Trumbles is always
                            \ zero)
    
     LDA #0                 \ Trumbles eat food and narcotics during the hyperspace
     STA QQ20               \ journey, so zero the amount of food and narcotics in
     STA QQ20+6             \ the hold
    
     JSR DORND              \ Take the number of Trumbles from TRIBBLE(1 0), add a
     AND #15                \ random number between 4 and 15, and double the result,
     ADC TRIBBLE            \ storing the resulting number in TRIBBLE(1 0)
     ORA #4                 \
     ROL A                  \ We start with the low byte
     STA TRIBBLE
    
     ROL TRIBBLE+1          \ And then do the high byte
    
     BPL P%+5               \ If bit 7 of the high byte is set, then rotate the high
     ROR TRIBBLE+1          \ byte back to the right, so the number of Trumbles is
                            \ always positive
    
    .nobirths
    
     LSR FIST               \ Halve our legal status in FIST, making us less bad,
                            \ and moving bit 0 into the C flag (so every time we
                            \ arrive in a new system, our legal status improves a
                            \ bit)
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace, which
                            \ doesn't affect the C flag
    
     LDA QQ15+1             \ Fetch s0_hi
    
     AND #%00000011         \ Extract bits 0-1 (which also help to determine the
                            \ economy), which will be between 0 and 3
    
     ADC #3                 \ Add 3 + C, to get a result between 3 and 7, clearing
                            \ the C flag in the process
    
     STA INWK+8             \ Store the result in z_sign in byte #6
    
     ROR A                  \ Halve A, rotating in the C flag (which is clear) and
     STA INWK+2             \ store in both x_sign and y_sign, moving the planet to
     STA INWK+5             \ the upper right
    
     JSR SOS1               \ Call SOS1 to set up the planet's data block and add it
                            \ to FRIN, where it will get put in the first slot as
                            \ it's the first one to be added to our local bubble of
                            \ this new system's universe
    
     LDA QQ15+3             \ Fetch s1_hi, extract bits 0-2, set bits 0 and 7 and
     AND #%00000111         \ store in z_sign, so the sun is behind us at a distance
     ORA #%10000001         \ of 1 to 7
     STA INWK+8
    
     LDA QQ15+5             \ Fetch s2_hi, extract bits 0-1 and store in x_sign and
     AND #%00000011         \ y_sign, so the sun is either dead centre in our rear
     STA INWK+2             \ laser crosshairs, or off to the top left by a distance
     STA INWK+1             \ of 1 or 2 when we look out the back
    
     LDA #0                 \ Set the pitch and roll counters to 0 (no rotation)
     STA INWK+29
     STA INWK+30
    
     LDA #129               \ Set A = 129, the ship type for the sun
    
     JSR NWSHP              \ Call NWSHP to set up the sun's data block and add it
                            \ to FRIN, where it will get put in the second slot as
                            \ it's the second one to be added to our local bubble
                            \ of this new system's universe
    

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [FIST](../workspace/wp.md#fist) in workspace [WP](../workspace/wp.md)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Variable [QQ15](../workspace/zp.md#qq15) in workspace [ZP](../workspace/zp.md)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ20](../workspace/wp.md#qq20) in workspace [WP](../workspace/wp.md)

The contents of our cargo hold

[X]

Subroutine [SOS1](sos1.md) (category: Universe)

Update the missile indicators, set up the planet data block

[X]

Variable [TRIBBLE](../workspace/wp.md#tribble) in workspace [WP](../workspace/wp.md)

The number of Trumbles in the cargo hold

[X]

Subroutine [ZINF](zinf.md) (category: Universe)

Reset the INWK workspace and orientation vectors

[X]

Label [nobirths](solar.md#nobirths) is local to this routine

[SOS1](sos1.md "Previous routine")[NWSTARS](nwstars.md "Next routine")
