---
title: "The OUCH subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ouch.html"
---

[mes9](mes9.md "Previous routine")[ou2](ou2.md "Next routine")
    
    
           Name: OUCH                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Potentially lose cargo or equipment following damage
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-ouch)
     Variations: See [code variations](../../related/compare/main/subroutine/ouch.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [OOPS](oops.md) calls OUCH
    
    
    
    
    
    * * *
    
    
     Our shields are dead and we are taking damage, so there is a small chance of
     losing cargo or equipment.
    
    
    
    
    .OUCH
    
     JSR DORND              \ Set A and X to random numbers
    
     BMI out                \ If A < 0 (50% chance), return from the subroutine
                            \ (as out contains an RTS)
    
     CPX #22                \ If X >= 22 (91% chance), return from the subroutine
     BCS out                \ (as out contains an RTS)
    
     LDA QQ20,X             \ If we do not have any of item QQ20+X, return from the
     BEQ out                \ subroutine (as out contains an RTS). X is in the range
                            \ 0-21, so this not only checks for cargo, but also for
                            \ E.C.M., fuel scoops, energy bomb, energy unit and
                            \ docking computer, all of which can be destroyed
    
     LDA DLY                \ If there is already an in-flight message on-screen,
     BNE out                \ return from the subroutine (as out contains an RTS)
    
     LDY #3                 \ Set bit 1 of de, the equipment destruction flag, so
     STY de                 \ that when we call MESS below, " DESTROYED" is appended
                            \ to the in-flight message
    
     STA QQ20,X             \ A is 0 (as we didn't branch with the BNE above), so
                            \ this sets QQ20+X to 0, which destroys any cargo or
                            \ equipment we have of that type
    
     CPX #17                \ If X >= 17 then we just lost a piece of equipment, so
     BCS ou1                \ jump to ou1 to print the relevant message
    
     TXA                    \ Print recursive token 48 + A as an in-flight token,
     ADC #208               \ which will be in the range 48 ("FOOD") to 64 ("ALIEN
     JMP MESS               \ ITEMS") as the C flag is clear, so this prints the
                            \ destroyed item's name, followed by " DESTROYED" (as we
                            \ set bit 1 of the de flag above), and returns from the
                            \ subroutine using a tail call
    
    .ou1
    
     BEQ ou2                \ If X = 17, jump to ou2 to print "E.C.M.SYSTEM
                            \ DESTROYED" and return from the subroutine using a tail
                            \ call
    
     CPX #18                \ If X = 18, jump to ou3 to print "FUEL SCOOPS
     BEQ ou3                \ DESTROYED" and return from the subroutine using a tail
                            \ call
    
     TXA                    \ Otherwise X is in the range 19 to 21 and the C flag is
     ADC #113-20            \ set (as we got here via a BCS to ou1), so we set A as
                            \ follows:
                            \
                            \   A = 113 - 20 + X + C
                            \     = 113 - 19 + X
                            \     = 113 to 115
    
     JMP MESS               \ Print recursive token A ("ENERGY BOMB", "ENERGY UNIT"
                            \ or "DOCKING COMPUTERS") as an in-flight message,
                            \ followed by " DESTROYED", and return from the
                            \ subroutine using a tail call
    

[X]

Variable [DLY](../workspace/wp.md#dly) in workspace [WP](../workspace/wp.md)

In-flight message delay

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [MESS](mess.md) (category: Flight)

Display an in-flight message

[X]

Variable [QQ20](../workspace/wp.md#qq20) in workspace [WP](../workspace/wp.md)

The contents of our cargo hold

[X]

Variable [de](../workspace/wp.md#de) in workspace [WP](../workspace/wp.md)

Equipment destruction flag

[X]

Label [ou1](ouch.md#ou1) is local to this routine

[X]

Subroutine [ou2](ou2.md) (category: Flight)

Display "E.C.M.SYSTEM DESTROYED" as an in-flight message

[X]

Subroutine [ou3](ou3.md) (category: Flight)

Display "FUEL SCOOPS DESTROYED" as an in-flight message

[X]

Entry point [out](tt217.md#out) in subroutine [TT217](tt217.md) (category: Keyboard)

Contains an RTS

[mes9](mes9.md "Previous routine")[ou2](ou2.md "Next routine")
