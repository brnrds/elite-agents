---
title: "The SUN (Part 4 of 4) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sun_part_4_of_4.html"
---

[SUN (Part 3 of 4)](sun_part_3_of_4.md "Previous routine")[CIRCLE](circle.md "Next routine")
    
    
           Name: SUN (Part 4 of 4)                                       [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Draw the sun: Continue to the top of the screen, erasing the old
                 sun line by line
      Deep dive: [Drawing the sun](https://elite.bbcelite.com/deep_dives/drawing_the_sun.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-sun-part-4-of-4)
     Variations: See [code variations](../../related/compare/main/subroutine/sun_part_4_of_4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CIRCLE](circle.md) calls via RTS2
    
    
    
    
    
    * * *
    
    
     This part erases any remaining traces of the old sun, now that we have drawn
     all the way to the top of the new sun.
    
    
    
    * * *
    
    
     Other entry points:
    
       RTS2                 Contains an RTS
    
    
    
    
     LDA SUNX               \ Set YY(1 0) = SUNX(1 0), the x-coordinate of the
     STA YY                 \ vertical centre axis of the old sun that's currently
     LDA SUNX+1             \ on-screen
     STA YY+1
    
    .PLFL3
    
     LDA LSO,Y              \ Fetch the Y-th point from the sun line heap, which
                            \ gives us the half-width of the old sun's line on this
                            \ line of the screen
    
     BEQ PLF9               \ If A = 0, skip the following call to HLOIN2 as there
                            \ is no sun line on this line of the screen
    
     JSR HLOIN2             \ Call HLOIN2 to draw a horizontal line on pixel line Y,
                            \ with centre point YY(1 0) and half-width A, and remove
                            \ the line from the sun line heap once done
    
    .PLF9
    
     DEY                    \ Decrement the line number in Y to move to the line
                            \ above
    
     BNE PLFL3              \ Jump up to PLFL3 to redraw the next line up, until we
                            \ have reached the top of the screen
    
    .PLF8
    
                            \ If we get here, we have successfully made it from the
                            \ bottom line of the screen to the top, and the old sun
                            \ has been replaced by the new one
    
     CLC                    \ Clear the C flag to indicate success in drawing the
                            \ sun
    
     LDA K3                 \ Set SUNX(1 0) = K3(1 0)
     STA SUNX
     LDA K3+1
     STA SUNX+1
    
    .RTS2
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [HLOIN2](hloin2.md) (category: Drawing lines)

Remove a line from the sun line heap and draw it on-screen

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [LSO](../workspace/wp.md#lso) in workspace [WP](../workspace/wp.md)

The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)

[X]

Label [PLF9](sun_part_4_of_4.md#plf9) is local to this routine

[X]

Label [PLFL3](sun_part_4_of_4.md#plfl3) is local to this routine

[X]

Variable [SUNX](../workspace/zp.md#sunx) in workspace [ZP](../workspace/zp.md)

The 16-bit x-coordinate of the vertical centre axis of the sun (which might be off-screen)

[X]

Variable [YY](../workspace/zp.md#yy) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit y-coordinate

[SUN (Part 3 of 4)](sun_part_3_of_4.md "Previous routine")[CIRCLE](circle.md "Next routine")
