---
title: "The SFRMIS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sfrmis.html"
---

[ECMOF](ecmof.md "Previous routine")[EXNO2](exno2.md "Next routine")
    
    
           Name: SFRMIS                                                  [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Add an enemy missile to our local bubble of universe
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-sfrmis)
     Variations: See [code variations](../../related/compare/main/subroutine/sfrmis.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TACTICS (Part 5 of 7)](tactics_part_5_of_7.md) calls SFRMIS
    
    
    
    
    
    * * *
    
    
     An enemy has fired a missile, so add the missile to our universe if there is
     room, and if there is, make the appropriate warnings and noises.
    
    
    
    
    .SFRMIS
    
     LDX #MSL               \ Set X to the ship type of a missile, and call SFS1-2
     JSR SFS1-2             \ to add a missile to our universe that has AI (bit 7
                            \ set), is hostile (bit 6 set) and has been launched
                            \ (bit 0 clear); the target slot number is set to 31,
                            \ but this is ignored as the hostile flags means we
                            \ are the target
    
    IF _SNG47
    
     BCC yetanotherrts      \ The C flag will be set if the call to SFS1-2 was a
                            \ success, so if it's clear, jump to yetanotherrts to
                            \ return from the subroutine (as yetanotherrts contains
                            \ an RTS)
    
    ELIF _COMPACT
    
     BCC TT17X-1            \ The C flag will be set if the call to SFS1-2 was a
                            \ success, so if it's clear, jump to TT17X-1 to return
                            \ from the subroutine (as TT17X-1 contains an RTS)
    
    ENDIF
    
     LDA #120               \ Print recursive token 120 ("INCOMING MISSILE") as an
     JSR MESS               \ in-flight message
    
     LDY #solaun            \ Call the NOISE routine with Y = 8 to make the sound
     JMP NOISE              \ of the missile being launched and return from the
                            \ subroutine using a tail call
    

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[X]

Configuration variable [MSL](../../all/workspaces.md#msl) = 1

Ship type for a missile

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Entry point [SFS1-2](sfs1.md#sfs1) in subroutine [SFS1](sfs1.md) (category: Universe)

Used to add a missile to the local bubble that that has AI (bit 7 set), is hostile (bit 6 set) and has been launched (bit 0 clear); the target slot number is set to 31, but this is ignored as the hostile flags means we are the target

[X]

Entry point [TT17X-1](tt17x.md#tt17x) in subroutine [TT17X](tt17x.md) (category: Keyboard)

Contains an RTS

[X]

Configuration variable [solaun](../../all/workspaces.md#solaun) = 8

Sound 8 = Missile launched / Ship launch

[X]

Subroutine [yetanotherrts](yetanotherrts.md) (category: Tactics)

Contains an RTS

[ECMOF](ecmof.md "Previous routine")[EXNO2](exno2.md "Next routine")
