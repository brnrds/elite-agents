---
title: "The EXNO2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/exno2.html"
---

[SFRMIS](sfrmis.md "Previous routine")[EXNO3](exno3.md "Next routine")
    
    
           Name: EXNO2                                                   [Show more]
           Type: Subroutine
       Category: Status
        Summary: Process us making a kill
      Deep dive: [Combat rank](https://elite.bbcelite.com/deep_dives/combat_rank.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-exno2)
     Variations: See [code variations](../../related/compare/main/subroutine/exno2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 5 of 16)](main_flight_loop_part_5_of_16.md) calls EXNO2
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls EXNO2
                 * [TACTICS (Part 1 of 7)](tactics_part_1_of_7.md) calls EXNO2
    
    
    
    
    
    * * *
    
    
     We have killed a ship, so increase the kill tally, displaying an iconic
     message of encouragement if the kill total is a multiple of 256, and then
     make a nearby explosion sound.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The type of the ship that was killed
    
    
    
    
    .EXNO2
    
     LDA TALLYL             \ We now add the fractional kill count to our tally,
     CLC                    \ starting with the fractional bytes:
     ADC KWL%-1,X           \
     STA TALLYL             \   TALLYL = TALLYL + fractional kill count
                            \
                            \ where the fractional kill count is taken from the
                            \ KWL% table, according to the ship's type (we look up
                            \ the X-1-th value from KWL% because ship types start
                            \ at 1 rather than 0)
    
     LDA TALLY              \ And then we add the low byte of TALLY(1 0):
     ADC KWH%-1,X           \
     STA TALLY              \   TALLY = TALLY + carry + integer kill count
                            \
                            \ where the integer kill count is taken from the KWH%
                            \ table in the same way
    
     BCC davidscockup       \ If there is no carry, jump to davidscockup to skip the
                            \ following three instructions, as we have not earned
                            \ a "RIGHT ON COMMANDER!" message
    
     INC TALLY+1            \ Increment the high byte of the kill count in TALLY
    
     LDA #101               \ The kill total is a multiple of 256, so it's time
     JSR MESS               \ for a pat on the back, so print recursive token 101
                            \ ("RIGHT ON COMMANDER!") as an in-flight message
    
    .davidscockup
    
                            \ Fall through into EXNO3 to make the sound of a
                            \ ship exploding
    

[X]

Configuration variable [KWH%](../../all/workspaces.md#kwh-per-cent) = &8084

The address of the kill tally integer table, as set in elite-data.asm

[X]

Configuration variable [KWL%](../../all/workspaces.md#kwl-per-cent) = &8063

The address of the kill tally fraction table, as set in elite-data.asm

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[X]

Variable [TALLY](../workspace/wp.md#tally) in workspace [WP](../workspace/wp.md)

Our combat rank

[X]

Variable [TALLYL](../workspace/wp.md#tallyl) in workspace [WP](../workspace/wp.md)

Combat rank fraction

[X]

Label [davidscockup](exno2.md#davidscockup) is local to this routine

[SFRMIS](sfrmis.md "Previous routine")[EXNO3](exno3.md "Next routine")
