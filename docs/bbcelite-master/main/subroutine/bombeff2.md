---
title: "The BOMBEFF2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bombeff2.html"
---

[BOMBOFF](bomboff.md "Previous routine")[BOMBON](bombon.md "Next routine")
    
    
           Name: BOMBEFF2                                                [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Erase the energy bomb zig-zag lightning bolt, make the sound of
                 the energy bomb going off, draw a new bolt and repeat four times
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-bombeff2)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 13 of 16)](main_flight_loop_part_13_of_16.md) calls BOMBEFF2
    
    
    
    
    
    
    .BOMBEFF2
    
     JSR P%+3               \ This pair of JSRs runs the following code four times
     JSR BOMBEFF
    
    .BOMBEFF
    
     LDY #sobomb            \ Call the NOISE routine with Y = 6 to make the sound of
     JSR NOISE              \ an energy bomb going off
    
     JSR BOMBOFF            \ Our energy bomb is going off, so call BOMBOFF to draw
                            \ the current zig-zag lightning bolt, which will erase
                            \ it from the screen
    
                            \ Fall through into BOMBON to set up and display a new
                            \ zig-zag lightning bolt
    

[X]

Label [BOMBEFF](bombeff2.md#bombeff) is local to this routine

[X]

Subroutine [BOMBOFF](bomboff.md) (category: Drawing lines)

Draw the zig-zag lightning bolt for the energy bomb

[X]

Subroutine [NOISE](noise.md) (category: Sound)

Make the sound whose number is in Y by populating the sound buffer

[X]

Configuration variable [sobomb](../../all/workspaces.md#sobomb) = 6

Sound 6 = Energy bomb

[BOMBOFF](bomboff.md "Previous routine")[BOMBON](bombon.md "Next routine")
