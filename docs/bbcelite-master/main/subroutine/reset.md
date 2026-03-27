---
title: "The RESET subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/reset.html"
---

[NUMBOR](numbor.md "Previous routine")[RES2](res2.md "Next routine")
    
    
           Name: RESET                                                   [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Reset most variables
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-reset)
     Variations: See [code variations](../../related/compare/main/subroutine/reset.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TITLE](title.md) calls RESET
                 * [TT170](tt170.md) calls RESET
    
    
    
    
    
    * * *
    
    
     Reset our ship and various controls, recharge shields and energy, and then
     fall through into RES2 to reset the stardust and the ship workspace at INWK.
    
     In this subroutine, this means zero-filling the following locations:
    
       * Pages &9, &A, &B, &C and &D
    
       * BETA to BETA+6, which covers the following:
    
         * BETA, BET1 - Set pitch to 0
    
         * XC, YC - Set text cursor to (0, 0)
    
         * QQ22 - Set hyperspace counters to 0
    
         * ECMA - Turn E.C.M. off
    
     It also sets QQ12 to &FF, to indicate we are docked, recharges the shields and
     energy banks, and then falls through into RES2.
    
    
    
    
    .RESET
    
     JSR ZERO               \ Reset the ship slots for the local bubble of universe,
                            \ and various flight and ship status variables
    
     LDX #6                 \ Set up a counter for zeroing BETA through BETA+6
    
    .SAL3
    
     STA BETA,X             \ Zero the X-th byte after BETA
    
     DEX                    \ Decrement the loop counter
    
     BPL SAL3               \ Loop back for the next byte to zero
    
     STX JSTGY              \ X is now negative - i.e. &FF - so this sets JSTGY to
                            \ &FF to set the joystick Y-channel to the default
                            \ direction
    
     TXA                    \ X is now negative - i.e. &FF - so this sets A and QQ12
     STA QQ12               \ to &FF to indicate we are docked
    
     LDX #2                 \ We're now going to recharge both shields and the
                            \ energy bank, which live in the three bytes at FSH,
                            \ ASH (FSH+1) and ENERGY (FSH+2), so set a loop counter
                            \ in X for 3 bytes
    
    .REL5
    
     STA FSH,X              \ Set the X-th byte of FSH to &FF to charge up that
                            \ shield/bank
    
     DEX                    \ Decrement the loop counter
    
     BPL REL5               \ Loop back to REL5 until we have recharged both shields
                            \ and the energy bank
    
                            \ Fall through into RES2 to reset the stardust and ship
                            \ workspace at INWK
    

[X]

Variable [BETA](../workspace/zp.md#beta) in workspace [ZP](../workspace/zp.md)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Variable [FSH](../workspace/zp.md#fsh) in workspace [ZP](../workspace/zp.md)

Forward shield status

[X]

Variable [JSTGY](../workspace/up.md#jstgy) in workspace [UP](../workspace/up.md)

Reverse joystick Y-channel configuration setting

[X]

Variable [QQ12](../workspace/zp.md#qq12) in workspace [ZP](../workspace/zp.md)

Our "docked" status

[X]

Label [REL5](reset.md#rel5) is local to this routine

[X]

Label [SAL3](reset.md#sal3) is local to this routine

[X]

Subroutine [ZERO](zero.md) (category: Utility routines)

Reset the local bubble of universe and ship status

[NUMBOR](numbor.md "Previous routine")[RES2](res2.md "Next routine")
