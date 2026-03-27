---
title: "The Ghy subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ghy.html"
---

[TTX110](ttx110.md "Previous routine")[jmp](jmp.md "Next routine")
    
    
           Name: Ghy                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Perform a galactic hyperspace jump
      Deep dive: [Twisting the system seeds](https://elite.bbcelite.com/deep_dives/twisting_the_system_seeds.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-ghy)
     Variations: See [code variations](../../related/compare/main/subroutine/ghy.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [hyp](hyp.md) calls Ghy
    
    
    
    
    
    * * *
    
    
     Engage the galactic hyperdrive. Called from the hyp routine above if CTRL-H is
     being pressed.
    
     This routine also updates the galaxy seeds to point to the next galaxy. Using
     a galactic hyperdrive rotates each seed byte to the left, rolling each byte
     left within itself like this:
    
       01234567 -> 12345670
    
     to get the seeds for the next galaxy. So after 8 galactic jumps, the seeds
     roll round to those of the first galaxy again.
    
     We always arrive in a new galaxy at galactic coordinates (96, 96), and then
     find the nearest system and set that as our location.
    
    
    
    * * *
    
    
     Other entry points:
    
       zZ+1                 Contains an RTS
    
    
    
    
    .Ghy
    
     LDX GHYP               \ Fetch GHYP, which tells us whether we own a galactic
     BEQ zZ+1               \ hyperdrive, and if it is zero, which means we don't,
                            \ return from the subroutine (as zZ+1 contains an RTS)
    
     INX                    \ We own a galactic hyperdrive, so X is &FF, so this
                            \ instruction sets X = 0
    
    \STX QQ8                \ These instructions are commented out in the original
    \STX QQ8+1              \ source
    
     STX GHYP               \ The galactic hyperdrive is a one-use item, so set GHYP
                            \ to 0 so we no longer have one fitted
    
     STX FIST               \ Changing galaxy also clears our criminal record, so
                            \ set our legal status in FIST to 0 ("clean")
    
     LDA #2                 \ Call wW2 with A = 2 to start the hyperspace countdown,
     JSR wW2                \ but starting the countdown from 2
    
     LDX #5                 \ To move galaxy, we rotate the galaxy's seeds left, so
                            \ set a counter in X for the 6 seed bytes
    
     INC GCNT               \ Increment the current galaxy number in GCNT
    
     LDA GCNT               \ Clear bit 3 of GCNT, so we jump from galaxy 7 back
     AND #%11110111         \ to galaxy 0 (shown in-game as going from galaxy 8 back
     STA GCNT               \ to the starting point in galaxy 1). We also retain any
                            \ set bits in the high nibble, so if the galaxy number
                            \ is manually set to 16 or higher, it will stay high
                            \ (though the high nibble doesn't seem to get set by
                            \ the game at any point, so it isn't clear what this is
                            \ for, though Lave in galaxy 16 does show a unique
                            \ system description override, so something is going on
                            \ here...)
    
    .G1
    
     LDA QQ21,X             \ Load the X-th seed byte into A
    
     ASL A                  \ Set the C flag to bit 7 of the seed
    
     ROL QQ21,X             \ Rotate the seed in memory, which will add bit 7 back
                            \ in as bit 0, so this rolls the seed around on itself
    
     DEX                    \ Decrement the counter
    
     BPL G1                 \ Loop back for the next seed byte, until we have
                            \ rotated them all
    
    \JSR DORND              \ This instruction is commented out in the original
                            \ source, and would set A and X to random numbers, so
                            \ perhaps the original plan was to arrive in each new
                            \ galaxy in a random place?
    
    .zZ
    
     LDA #96                \ Set (QQ9, QQ10) to (96, 96), which is where we always
     STA QQ9                \ arrive in a new galaxy (the selected system will be
     STA QQ10               \ set to the nearest actual system later on)
    
     JSR TT110              \ Call TT110 to show the front space view
    
     JSR TT111              \ Call TT111 to set the current system to the nearest
                            \ system to (QQ9, QQ10), and put the seeds of the
                            \ nearest system into QQ15 to QQ15+5
                            \
                            \ This call fixes a bug in the cassette version, where
                            \ the galactic hyperdrive will take us to coordinates
                            \ (96, 96) in the new galaxy, even if there isn't
                            \ actually a system there, so if we jump when we are
                            \ low on fuel, it is possible to get stuck in the
                            \ middle of nowhere when changing galaxy
                            \
                            \ This call sets the current system correctly, so we
                            \ always arrive at the nearest system to (96, 96)
    
     LDX #5                 \ We now want to copy those seeds into safehouse, so we
                            \ so set a counter in X to copy 6 bytes
    
    .dumdeedum
    
     LDA QQ15,X             \ Copy the X-th byte of QQ15 into the X-th byte of
     STA safehouse,X        \ safehouse
    
     DEX                    \ Decrement the loop counter
    
     BPL dumdeedum          \ Loop back to copy the next byte until we have copied
                            \ all six seed bytes
    
     LDX #0                 \ Set the distance to the selected system in QQ8(1 0)
     STX QQ8                \ to 0
     STX QQ8+1
    
     LDA #116               \ Print recursive token 116 (GALACTIC HYPERSPACE ")
     JSR MESS               \ as an in-flight message
    
                            \ Fall through into jmp to set the system to the
                            \ current system and return from the subroutine there
    

[X]

Variable [FIST](../workspace/wp.md#fist) in workspace [WP](../workspace/wp.md)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Label [G1](ghy.md#g1) is local to this routine

[X]

Variable [GCNT](../workspace/wp.md#gcnt) in workspace [WP](../workspace/wp.md)

The number of the current galaxy (0-7)

[X]

Variable [GHYP](../workspace/wp.md#ghyp) in workspace [WP](../workspace/wp.md)

Galactic hyperdrive

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[X]

Variable [QQ10](../workspace/zp.md#qq10) in workspace [ZP](../workspace/zp.md)

The galactic y-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic y-coordinate)

[X]

Variable [QQ15](../workspace/zp.md#qq15) in workspace [ZP](../workspace/zp.md)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ21](../workspace/wp.md#qq21) in workspace [WP](../workspace/wp.md)

The three 16-bit seeds for the current galaxy

[X]

Variable [QQ8](../workspace/zp.md#qq8) in workspace [ZP](../workspace/zp.md)

The distance from the current system to the selected system in light years * 10, stored as a 16-bit number

[X]

Variable [QQ9](../workspace/zp.md#qq9) in workspace [ZP](../workspace/zp.md)

The galactic x-coordinate of the crosshairs in the galaxy chart (and, most of the time, the selected system's galactic x-coordinate)

[X]

Subroutine [TT110](tt110.md) (category: Flight)

Launch from a station or show the front space view

[X]

Subroutine [TT111](tt111.md) (category: Universe)

Set the current system to the nearest system to a point

[X]

Label [dumdeedum](ghy.md#dumdeedum) is local to this routine

[X]

Variable [safehouse](../workspace/wp.md#safehouse) in workspace [WP](../workspace/wp.md)

Backup storage for the seeds for the selected system

[X]

Entry point [wW2](ww.md#ww2) in subroutine [wW](ww.md) (category: Flight)

Start the hyperspace countdown, starting the countdown from the value in A

[X]

Entry point [zZ+1](ghy.md#zz) in subroutine [Ghy](ghy.md) (category: Flight)

Contains an RTS

[TTX110](ttx110.md "Previous routine")[jmp](jmp.md "Next routine")
