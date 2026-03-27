---
title: "The BOMBON subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bombon.html"
---

[BOMBEFF2](bombeff2.md "Previous routine")[BOMBPOS](../variable/bombpos.md "Next routine")
    
    
           Name: BOMBON                                                  [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Randomise and draw the energy bomb's zig-zag lightning bolt lines
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-bombon)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 3 of 16)](main_flight_loop_part_3_of_16.md) calls BOMBON
    
    
    
    
    
    
    .BOMBON
    
     LDY #0                 \ We first need to generate 10 random coordinates for a
                            \ zig-zag lightning bolt, with the x-coordinates in the
                            \ table at BOMBTBX and the y-coordinates in the table at
                            \ BOMBTBY, so set a counter in Y as an index to point at
                            \ each coordinate as we create them
                            \
                            \ Note that we generate the points from right to left,
                            \ so that's high x-coordinate to low x-coordinate
    
    .BOMBL2
    
     JSR DORND              \ Set A and X to random numbers and reduce A to a
     AND #127               \ random number in the range 0-127
    
     ADC #3                 \ Add 3 so A is now in the range 3-130, so the smallest
                            \ possible value gives a y-coordinate just below the top
                            \ border, and the highest possible value gives a
                            \ y-coordinate that's around two-thirds of the way down
                            \ the space view
    
     STA BOMBTBY,Y          \ Store A in the Y-th byte of BOMBTBY, as the
                            \ y-coordinate of the Y-th point in our lightning bolt
    
     TXA                    \ Fetch the random number from X into A and reduce it to
     AND #31                \ the range 0-31
    
     CLC                    \ Add the Y-th value from BOMBPOS table, which contains
     ADC BOMBPOS,Y          \ the smallest possible x-coordinate for the Y-th point
                            \ (so the coordinates in the bolt will step along the
                            \ screen from right to left, but with varying step
                            \ sizes)
    
     STA BOMBTBX,Y          \ Store A in the Y-th byte of BOMBTBX, as the
                            \ x-coordinate of the Y-th point in our lightning bolt
    
     INY                    \ Increment the loop index
    
     CPY #10                \ Loop back to generate the next coordinate until we
     BCC BOMBL2             \ have generated all ten
    
     LDX #0                 \ Set BOMBTBX+9 = 0, so the lightning bolt starts at the
     STX BOMBTBX+9          \ left edge of the screen
    
     DEX                    \ Set BOMBTBX = 255, so the lightning bolt ends at the
     STX BOMBTBX            \ right edge of the screen
    
     BCS BOMBOFF            \ Call BOMBOFF to draw the newly generated zig-zag
                            \ lightning bolt and return from the subroutine using a
                            \ tail call (this BCS is effectively a JMP as we passed
                            \ through the BCC above)
    

[X]

Label [BOMBL2](bombon.md#bombl2) is local to this routine

[X]

Subroutine [BOMBOFF](bomboff.md) (category: Drawing lines)

Draw the zig-zag lightning bolt for the energy bomb

[X]

Variable [BOMBPOS](../variable/bombpos.md) (category: Drawing lines)

A set of x-coordinates that are used as the basis for the energy bomb's zig-zag lightning bolt

[X]

Variable [BOMBTBX](../variable/bombtbx.md) (category: Drawing lines)

This is where we store the x-coordinates for the energy bomb's zig-zag lightning bolt

[X]

Variable [BOMBTBY](../variable/bombtby.md) (category: Drawing lines)

This is where we store the y-coordinates for the energy bomb's zig-zag lightning bolt

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[BOMBEFF2](bombeff2.md "Previous routine")[BOMBPOS](../variable/bombpos.md "Next routine")
