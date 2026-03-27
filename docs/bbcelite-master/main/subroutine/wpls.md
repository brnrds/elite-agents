---
title: "The WPLS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/wpls.html"
---

[WP1](wp1.md "Previous routine")[EDGES](edges.md "Next routine")
    
    
           Name: WPLS                                                    [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Remove the sun from the screen
      Deep dive: [Drawing the sun](https://elite.bbcelite.com/deep_dives/drawing_the_sun.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-wpls)
     Variations: See [code variations](../../related/compare/main/subroutine/wpls.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md) calls WPLS
                 * [PL2](pl2.md) calls WPLS
                 * [SUN (Part 1 of 4)](sun_part_1_of_4.md) calls WPLS
    
    
    
    
    
    * * *
    
    
     We do this by redrawing it using the lines stored in the sun line heap when
     the sun was originally drawn by the SUN routine.
    
    
    
    * * *
    
    
     Arguments:
    
       SUNX(1 0)            The x-coordinate of the vertical centre axis of the sun
    
    
    
    * * *
    
    
     Other entry points:
    
       WPLS-1               Contains an RTS
    
    
    
    
    .WPLS
    
     LDA LSX                \ If LSX < 0, the sun line heap is empty, so return from
     BMI WPLS-1             \ the subroutine (as WPLS-1 contains an RTS)
    
     LDA SUNX               \ Set YY(1 0) = SUNX(1 0), the x-coordinate of the
     STA YY                 \ vertical centre axis of the sun that's currently on
     LDA SUNX+1             \ screen
     STA YY+1
    
     LDY #2*Y-1             \ #Y is the y-coordinate of the centre of the space
                            \ view, so this sets Y as a counter for the number of
                            \ lines in the space view (i.e. 191), which is also the
                            \ number of lines in the LSO block
    
    .WPL2
    
     LDA LSO,Y              \ Fetch the Y-th point from the sun line heap, which
                            \ gives us the half-width of the sun's line on this line
                            \ of the screen
    
     BEQ P%+5               \ If A = 0, skip the following call to HLOIN2 as there
                            \ is no sun line on this line of the screen
    
     JSR HLOIN2             \ Call HLOIN2 to draw a horizontal line on pixel line Y,
                            \ with centre point YY(1 0) and half-width A, and remove
                            \ the line from the sun line heap once done
    
     DEY                    \ Decrement the loop counter
    
     BNE WPL2               \ Loop back for the next line in the line heap until
                            \ we have gone through the entire heap
    
     DEY                    \ This sets Y to &FF, as we end the loop with Y = 0
    
     STY LSX                \ Set LSX to &FF to indicate the sun line heap is empty
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [HLOIN2](hloin2.md) (category: Drawing lines)

Remove a line from the sun line heap and draw it on-screen

[X]

Variable [LSO](../workspace/wp.md#lso) in workspace [WP](../workspace/wp.md)

The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)

[X]

Variable [LSX](../workspace/zp.md#lsx) in workspace [ZP](../workspace/zp.md)

LSX contains the status of the sun line heap at LSO

[X]

Variable [SUNX](../workspace/zp.md#sunx) in workspace [ZP](../workspace/zp.md)

The 16-bit x-coordinate of the vertical centre axis of the sun (which might be off-screen)

[X]

Label [WPL2](wpls.md#wpl2) is local to this routine

[X]

Entry point [WPLS-1](wpls.md#wpls) in subroutine [WPLS](wpls.md) (category: Drawing suns)

Contains an RTS

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [YY](../workspace/zp.md#yy) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing a 16-bit y-coordinate

[WP1](wp1.md "Previous routine")[EDGES](edges.md "Next routine")
