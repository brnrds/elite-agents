---
title: "The HLOIN2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hloin2.html"
---

[NLIN2](nlin2.md "Previous routine")[BLINE](bline.md "Next routine")
    
    
           Name: HLOIN2                                                  [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Remove a line from the sun line heap and draw it on-screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-hloin2)
     Variations: See [code variations](../../related/compare/main/subroutine/hloin2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SUN (Part 2 of 4)](sun_part_2_of_4.md) calls HLOIN2
                 * [SUN (Part 4 of 4)](sun_part_4_of_4.md) calls HLOIN2
                 * [WPLS](wpls.md) calls HLOIN2
    
    
    
    
    
    * * *
    
    
     Specifically, this does the following:
    
       * Set X1 and X2 to the x-coordinates of the ends of the horizontal line with
         centre YY(1 0) and length A to the left and right
    
       * Set the Y-th byte of the LSO block to 0 (i.e. remove this line from the
         sun line heap)
    
       * Draw a horizontal line from (X1, Y) to (X2, Y)
    
    
    
    * * *
    
    
     Arguments:
    
       YY(1 0)              The x-coordinate of the centre point of the line
    
       A                    The half-width of the line, i.e. the contents of the
                            Y-th byte of the sun line heap
    
       Y                    The number of the entry in the sun line heap (which is
                            also the y-coordinate of the line)
    
    
    
    * * *
    
    
     Returns:
    
       Y                    Y is preserved
    
    
    
    
    .HLOIN2
    
     JSR EDGES              \ Call EDGES to calculate X1 and X2 for the horizontal
                            \ line centred on YY(1 0) and with half-width A
    
     STY Y1                 \ Set Y1 = Y
    
     LDA #0                 \ Set the Y-th byte of the LSO block to 0
     STA LSO,Y
    
     JMP HLOIN              \ Call HLOIN to draw a horizontal line from (X1, Y) to
                            \ (X2, Y), returning from the subroutine using a tail
                            \ call
    

[X]

Subroutine [EDGES](edges.md) (category: Drawing lines)

Draw a horizontal line given a centre and a half-width

[X]

Subroutine [HLOIN](hloin.md) (category: Drawing lines)

Draw a horizontal line from (X1, Y1) to (X2, Y1)

[X]

Variable [LSO](../workspace/wp.md#lso) in workspace [WP](../workspace/wp.md)

The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[NLIN2](nlin2.md "Previous routine")[BLINE](bline.md "Next routine")
