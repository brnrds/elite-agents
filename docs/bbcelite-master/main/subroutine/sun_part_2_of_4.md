---
title: "The SUN (Part 2 of 4) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sun_part_2_of_4.html"
---

[SUN (Part 1 of 4)](sun_part_1_of_4.md "Previous routine")[SUN (Part 3 of 4)](sun_part_3_of_4.md "Next routine")
    
    
           Name: SUN (Part 2 of 4)                                       [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Draw the sun: Start from the bottom of the screen and erase the
                 old sun line by line
      Deep dive: [Drawing the sun](https://elite.bbcelite.com/deep_dives/drawing_the_sun.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-sun-part-2-of-4)
     Variations: See [code variations](../../related/compare/main/subroutine/sun_part_2_of_4.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part erases the old sun, starting at the bottom of the screen and working
     upwards until we reach the bottom of the new sun.
    
    
    
    
     LDY Yx2M1              \ Set Y = y-coordinate of the bottom of the screen,
                            \ which we use as a counter in the following routine to
                            \ redraw the old sun
    
     LDA SUNX               \ Set YY(1 0) = SUNX(1 0), the x-coordinate of the
     STA YY                 \ vertical centre axis of the old sun that's currently
     LDA SUNX+1             \ on-screen
     STA YY+1
    
    .PLFL2
    
     CPY TGT                \ If Y = TGT, we have reached the line where we will
     BEQ PLFL               \ start drawing the new sun, so there is no need to
                            \ keep erasing the old one, so jump down to PLFL
    
     LDA LSO,Y              \ Fetch the Y-th point from the sun line heap, which
                            \ gives us the half-width of the old sun's line on this
                            \ line of the screen
    
     BEQ PLF13              \ If A = 0, skip the following call to HLOIN2 as there
                            \ is no sun line on this line of the screen
    
     JSR HLOIN2             \ Call HLOIN2 to draw a horizontal line on pixel line Y,
                            \ with centre point YY(1 0) and half-width A, and remove
                            \ the line from the sun line heap once done
    
    .PLF13
    
     DEY                    \ Decrement the loop counter
    
     BNE PLFL2              \ Loop back for the next line in the line heap until
                            \ we have either gone through the entire heap, or
                            \ reached the bottom row of the new sun
    

[X]

Subroutine [HLOIN2](hloin2.md) (category: Drawing lines)

Remove a line from the sun line heap and draw it on-screen

[X]

Variable [LSO](../workspace/wp.md#lso) in workspace [WP](../workspace/wp.md)

The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)

[X]

Label [PLF13](sun_part_2_of_4.md#plf13) is local to this routine

[X]

Label [PLFL](sun_part_3_of_4.md#plfl) in subroutine [SUN (Part 3 of 4)](sun_part_3_of_4.md)

[X]

Label [PLFL2](sun_part_2_of_4.md#plfl2) is local to this routine

[X]

Variable [SUNX](../workspace/zp.md#sunx) in workspace [ZP](../workspace/zp.md)

The 16-bit x-coordinate of the vertical centre axis of the sun (which might be off-screen)

[X]

Variable [TGT](../workspace/zp.md#tgt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used as a target value for counters when drawing explosion clouds and partial circles

[X]

Variable [YY](../workspace/zp.md#yy) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit y-coordinate

[X]

Variable [Yx2M1](../workspace/zp.md#yx2m1) in workspace [ZP](../workspace/zp.md)

This is used to store the number of pixel rows in the space view minus 1, which is also the y-coordinate of the bottom pixel row of the space view (it is set to 191 in the RES2 routine)

[SUN (Part 1 of 4)](sun_part_1_of_4.md "Previous routine")[SUN (Part 3 of 4)](sun_part_3_of_4.md "Next routine")
