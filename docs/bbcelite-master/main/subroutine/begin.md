---
title: "The BEGIN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/begin.html"
---

[spasto](../variable/spasto.md "Previous routine")[TT170](tt170.md "Next routine")
    
    
           Name: BEGIN                                                   [Show more]
           Type: Subroutine
       Category: Loader
        Summary: Initialise the configuration variables and start the game
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-begin)
     Variations: See [code variations](../../related/compare/main/subroutine/begin.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [S%](s_per_cent.md) calls BEGIN
    
    
    
    
    
    
    .BEGIN
    
     LDX #(DISK-COMC)       \ We start by zeroing all the configuration variables
                            \ between COMC and DISK, to set them to their default
                            \ values, so set a counter in X for DISK - COMC bytes
    
     LDA #0                 \ Set A = 0 so we can zero the variables
    
    .BEL1
    
     STA COMC,X             \ Zero the X-th configuration variable
    
     DEX                    \ Decrement the loop counter
    
     BPL BEL1               \ Loop back to BEL1 to zero the next byte, until we have
                            \ zeroed them all
    
     LDA XX21+SST*2-2       \ Set spasto(1 0) to the Coriolis space station entry
     STA spasto             \ from the ship blueprint lookup table at XX21 (so
     LDA XX21+SST*2-1       \ spasto(1 0) points to the Coriolis blueprint)
     STA spasto+1
    
     JSR JAMESON            \ Call JAMESON to set the last saved commander to the
                            \ default "JAMESON" commander
    
                            \ Fall through into TT170 to start the game
    

[X]

Label [BEL1](begin.md#bel1) is local to this routine

[X]

Variable [COMC](../workspace/up.md#comc) in workspace [UP](../workspace/up.md)

The colour of the dot on the compass

[X]

Variable [DISK](../workspace/up.md#disk) in workspace [UP](../workspace/up.md)

The configuration setting for toggle key "T", which isn't actually used but is still updated by pressing "T" while the game is paused. This is a configuration option from the Commodore 64 version of Elite that lets you switch between tape and disc

[X]

Subroutine [JAMESON](jameson.md) (category: Save and load)

Restore the default JAMESON commander

[X]

Configuration variable [SST](../../all/workspaces.md#sst) = 2

Ship type for a Coriolis space station

[X]

Configuration variable [XX21](../../all/workspaces.md#xx21) = &8000

The address of the ship blueprints lookup table, as set in elite-data.asm

[X]

Variable [spasto](../variable/spasto.md) (category: Universe)

Contains the address of the Coriolis space station's ship blueprint

[spasto](../variable/spasto.md "Previous routine")[TT170](tt170.md "Next routine")
