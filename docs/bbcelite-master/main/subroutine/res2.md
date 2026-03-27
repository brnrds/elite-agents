---
title: "The RES2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/res2.html"
---

[RESET](reset.md "Previous routine")[ZINF](zinf.md "Next routine")
    
    
           Name: RES2                                                    [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Reset a number of flight variables and workspaces
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-res2)
     Variations: See [code variations](../../related/compare/main/subroutine/res2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](death.md) calls RES2
                 * [DEATH2](death2.md) calls RES2
                 * [DOENTRY](doentry.md) calls RES2
                 * [ESCAPE](escape.md) calls RES2
                 * [MJP](mjp.md) calls RES2
                 * [TT110](tt110.md) calls RES2
                 * [TT18](tt18.md) calls RES2
    
    
    
    
    
    * * *
    
    
     This is called after we launch from a space station, arrive in a new system
     after hyperspace, launch an escape pod, or die a cold, lonely death in the
     depths of space.
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is set to &FF
    
    
    
    
    .RES2
    
    \JSR stopbd             \ This instruction is commented out in the original
                            \ source
    
     LDA #NOST              \ Reset NOSTM, the number of stardust particles, to the
     STA NOSTM              \ maximum allowed (20)
    
     LDX #&FF               \ Reset LSX2 and LSY2, the ball line heaps used by the
     STX LSX2               \ BLINE routine for drawing circles, to &FF, to set the
     STX LSY2               \ heap to empty
    
     STX MSTG               \ Reset MSTG, the missile target, to &FF (no target)
    
     LDA #128               \ Set the current pitch rate to the mid-point, 128
     STA JSTY
    
     STA ALP2               \ Reset ALP2 (roll sign) and BET2 (pitch sign)
     STA BET2               \ to negative, i.e. pitch and roll negative
    
     ASL A                  \ This sets A to 0
    
     STA BETA               \ Reset BETA (pitch angle alpha) to 0
    
     STA BET1               \ Reset BET1 (magnitude of the pitch angle) to 0
    
     STA ALP2+1             \ Reset ALP2+1 (flipped roll sign) and BET2+1 (flipped
     STA BET2+1             \ pitch sign) to positive, i.e. pitch and roll negative
    
     STA MCNT               \ Reset MCNT (the main loop counter) to 0
    
    \STA TRIBCT             \ This instruction is commented out in the original
                            \ source; it is left over from the Commodore 64 version
                            \ of Elite and would reset the number of Trumbles
    
     LDA #3                 \ Reset DELTA (speed) to 3
     STA DELTA
    
     STA ALPHA              \ Reset ALPHA (roll angle alpha) to 3
    
     STA ALP1               \ Reset ALP1 (magnitude of roll angle alpha) to 3
    
    \LDA #&10               \ These instructions are commented out in the original
    \STA COL2               \ source
    
     LDA #0                 \ Set dontclip to 0 (though this variable is never used,
     STA dontclip           \ so this has no effect; it is left over from the
                            \ Commodore 64 version)
    
     LDA #2*Y-1             \ Set Yx2M1 to the number of pixel lines in the space
     STA Yx2M1              \ view
    
     LDA SSPR               \ Fetch the "space station present" flag, and if we are
     BEQ P%+5               \ not inside the safe zone, skip the next instruction
    
     JSR SPBLB              \ Light up the space station bulb on the dashboard
    
     LDA ECMA               \ Fetch the E.C.M. status flag, and if E.C.M. is off,
     BEQ yu                 \ skip the next instruction
    
     JSR ECMOF              \ Turn off the E.C.M. sound
    
    .yu
    
     JSR WPSHPS             \ Wipe all ships from the scanner
    
     JSR ZERO               \ Reset the ship slots for the local bubble of universe,
                            \ and various flight and ship status variables
    
     LDA #LO(LS%)           \ We have reset the ship line heap, so we now point
     STA SLSP               \ SLSP to LS% (the byte below the ship blueprints at D%)
     LDA #HI(LS%)           \ to indicate that the heap is empty
     STA SLSP+1
    
                            \ Finally, fall through into ZINF to reset the INWK
                            \ ship workspace
    

[X]

Variable [ALP1](../workspace/zp.md#alp1) in workspace [ZP](../workspace/zp.md)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Variable [ALP2](../workspace/zp.md#alp2) in workspace [ZP](../workspace/zp.md)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Variable [ALPHA](../workspace/zp.md#alpha) in workspace [ZP](../workspace/zp.md)

The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31

[X]

Variable [BET1](../workspace/zp.md#bet1) in workspace [ZP](../workspace/zp.md)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BET2](../workspace/zp.md#bet2) in workspace [ZP](../workspace/zp.md)

Bit 7 of BET2 = sign of the pitch angle in BETA

[X]

Variable [BETA](../workspace/zp.md#beta) in workspace [ZP](../workspace/zp.md)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Variable [DELTA](../workspace/zp.md#delta) in workspace [ZP](../workspace/zp.md)

Our current speed, in the range 1-40

[X]

Variable [ECMA](../workspace/zp.md#ecma) in workspace [ZP](../workspace/zp.md)

The E.C.M. countdown timer, which determines whether an E.C.M. system is currently operating

[X]

Subroutine [ECMOF](ecmof.md) (category: Dashboard)

Switch off the E.C.M. and turn off the dashboard bulb

[X]

Variable [JSTY](../workspace/zp.md#jsty) in workspace [ZP](../workspace/zp.md)

Our current pitch rate

[X]

Configuration variable [LS%](../../all/workspaces.md#ls-per-cent) = &0800

The start of the descending ship line heap

[X]

Variable [LSX2](../workspace/wp.md#lsx2) in workspace [WP](../workspace/wp.md)

The ball line heap for storing x-coordinates

[X]

Variable [LSY2](../workspace/wp.md#lsy2) in workspace [WP](../workspace/wp.md)

The ball line heap for storing y-coordinates

[X]

Variable [MCNT](../workspace/zp.md#mcnt) in workspace [ZP](../workspace/zp.md)

The main loop counter

[X]

Variable [MSTG](../workspace/zp.md#mstg) in workspace [ZP](../workspace/zp.md)

The current missile lock target

[X]

Configuration variable [NOST](../../all/workspaces.md#nost) = 20

The number of stardust particles in normal space (this goes down to 3 in witchspace)

[X]

Variable [NOSTM](../workspace/zp.md#nostm) in workspace [ZP](../workspace/zp.md)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Variable [SLSP](../workspace/wp.md#slsp) in workspace [WP](../workspace/wp.md)

The address of the bottom of the ship line heap

[X]

Subroutine [SPBLB](spblb.md) (category: Dashboard)

Light up the space station indicator ("S") on the dashboard

[X]

Variable [SSPR](../workspace/wp.md#sspr) in workspace [WP](../workspace/wp.md)

"Space station present" flag

[X]

Subroutine [WPSHPS](wpshps.md) (category: Dashboard)

Clear the scanner, reset the ball line and sun line heaps

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Yx2M1](../workspace/zp.md#yx2m1) in workspace [ZP](../workspace/zp.md)

This is used to store the number of pixel rows in the space view minus 1, which is also the y-coordinate of the bottom pixel row of the space view (it is set to 191 in the RES2 routine)

[X]

Subroutine [ZERO](zero.md) (category: Utility routines)

Reset the local bubble of universe and ship status

[X]

Variable [dontclip](../workspace/zp.md#dontclip) in workspace [ZP](../workspace/zp.md)

This is set to 0 in the RES2 routine, but the value is never actually read (this is left over from the Commodore 64 version of Elite)

[X]

Label [yu](res2.md#yu) is local to this routine

[RESET](reset.md "Previous routine")[ZINF](zinf.md "Next routine")
