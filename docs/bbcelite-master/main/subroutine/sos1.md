---
title: "The SOS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sos1.html"
---

[coltabl](../variable/coltabl.md "Previous routine")[SOLAR](solar.md "Next routine")
    
    
           Name: SOS1                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Update the missile indicators, set up the planet data block
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-sos1)
     References: This subroutine is called as follows:
                 * [SOLAR](solar.md) calls SOS1
                 * [TT110](tt110.md) calls SOS1
    
    
    
    
    
    * * *
    
    
     Update the missile indicators, and set up a data block for the planet, but
     only setting the pitch and roll counters to 127 (no damping).
    
    
    
    
    .SOS1
    
     JSR msblob             \ Reset the dashboard's missile indicators so none of
                            \ them are targeted
    
     LDA #127               \ Set the pitch and roll counters to 127, so that's a
     STA INWK+29            \ clockwise roll and a diving pitch with no damping, so
     STA INWK+30            \ the planet's rotation doesn't slow down
    
     LDA tek                \ Set A = 128 or 130 depending on bit 1 of the system's
     AND #%00000010         \ tech level in tek
     ORA #%10000000
    
     JMP NWSHP              \ Add a new planet to our local bubble of universe,
                            \ with the planet type defined by A (128 is a planet
                            \ with an equator and meridian, 130 is a planet with
                            \ a crater)
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [NWSHP](nwshp.md) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Subroutine [msblob](msblob.md) (category: Dashboard)

Display the dashboard's missile indicators in green

[X]

Variable [tek](../workspace/wp.md#tek) in workspace [WP](../workspace/wp.md)

The current system's tech level (0-14)

[coltabl](../variable/coltabl.md "Previous routine")[SOLAR](solar.md "Next routine")
