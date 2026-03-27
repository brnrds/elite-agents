---
title: "The MJP subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mjp.html"
---

[GTHG](gthg.md "Previous routine")[TT18](tt18.md "Next routine")
    
    
           Name: MJP                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Process a mis-jump into witchspace
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-mjp)
     Variations: See [code variations](../../related/compare/main/subroutine/mjp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT18](tt18.md) calls MJP
                 * [TT18](tt18.md) calls via ptg
                 * [TT18](tt18.md) calls via RTS111
    
    
    
    
    
    * * *
    
    
     Process a mis-jump into witchspace (which happens very rarely). Witchspace has
     a strange, almost dust-free aspect to it, and it is populated by aggressive
     Thargoids. Using our escape pod will be fatal, and our position on the
     galactic chart is in-between systems. It is a scary place...
    
     There is a 0.78% chance that this routine is called from TT18 instead of doing
     a normal hyperspace, or we can manually trigger a mis-jump by holding down
     CTRL after first enabling the "author display" configuration option ("X") when
     paused.
    
    
    
    * * *
    
    
     Other entry points:
    
       ptg                  Called when the user manually forces a mis-jump
    
       RTS111               Contains an RTS
    
    
    
    
    .ptg
    
     LSR COK                \ Set bit 0 of the competition flags in COK, so that the
     SEC                    \ competition code will include the fact that we have
     ROL COK                \ manually forced a mis-jump into witchspace
    
    .MJP
    
    \JSR CATLOD             \ This instruction is commented out in the original
                            \ source
    
     LDA #3                 \ Clear the top part of the screen, draw a border box,
     JSR TT66               \ and set the current view type in QQ11 to 3
    
     JSR LL164              \ Call LL164 to show the hyperspace tunnel and make the
                            \ hyperspace sound for a second time (as we already
                            \ called LL164 in TT18)
    
     JSR RES2               \ Reset a number of flight variables and workspaces, as
                            \ well as setting Y to &FF
    
     STY MJ                 \ Set the mis-jump flag in MJ to &FF, to indicate that
                            \ we are now in witchspace
    
    .MJP1
    
     JSR GTHG               \ Call GTHG to spawn a Thargoid ship and a Thargon
                            \ companion
    
     LDA #2                 \ Fetch the number of Thargoid ships from MANY+THG, and
     CMP MANY+THG           \ if it is less than or equal to 2, loop back to MJP1 to
     BCS MJP1               \ spawn another one, until we have three Thargoids
    
     STA NOSTM              \ Set NOSTM (the maximum number of stardust particles)
                            \ to 3, so there are fewer bits of stardust in
                            \ witchspace (normal space has a maximum of 18)
    
     LDX #0                 \ Initialise the front space view
     JSR LOOK1
    
     LDA QQ1                \ Fetch the current system's galactic y-coordinate in
     EOR #%00011111         \ QQ1 and flip bits 0-5, so we end up somewhere in the
     STA QQ1                \ vicinity of our original destination, but above or
                            \ below it in the galactic chart
    
     RTS                    \ Return from the subroutine
    
    .RTS111
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COK](../workspace/wp.md#cok) in workspace [WP](../workspace/wp.md)

Flags used to generate the competition code

[X]

Subroutine [GTHG](gthg.md) (category: Universe)

Spawn a Thargoid ship and a Thargon companion

[X]

Subroutine [LL164](ll164.md) (category: Drawing circles)

Make the hyperspace sound and draw the hyperspace tunnel

[X]

Subroutine [LOOK1](look1.md) (category: Flight)

Initialise the space view

[X]

Variable [MANY](../workspace/wp.md#many) in workspace [WP](../workspace/wp.md)

The number of ships of each type in the local bubble of universe

[X]

Variable [MJ](../workspace/wp.md#mj) in workspace [WP](../workspace/wp.md)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Label [MJP1](mjp.md#mjp1) is local to this routine

[X]

Variable [NOSTM](../workspace/zp.md#nostm) in workspace [ZP](../workspace/zp.md)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Variable [QQ1](../workspace/wp.md#qq1) in workspace [WP](../workspace/wp.md)

The current system's galactic y-coordinate (0-256)

[X]

Subroutine [RES2](res2.md) (category: Start and end)

Reset a number of flight variables and workspaces

[X]

Configuration variable [THG](../../all/workspaces.md#thg) = 29

Ship type for a Thargoid

[X]

Subroutine [TT66](tt66.md) (category: Drawing the screen)

Clear the screen and set the current view type

[GTHG](gthg.md "Previous routine")[TT18](tt18.md "Next routine")
