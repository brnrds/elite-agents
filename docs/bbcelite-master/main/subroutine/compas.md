---
title: "The COMPAS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/compas.html"
---

[DENGY](dengy.md "Previous routine")[SPS2](sps2.md "Next routine")
    
    
           Name: COMPAS                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the compass
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-compas)
     Variations: See [code variations](../../related/compare/main/subroutine/compas.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DIALS (Part 4 of 4)](dials_part_4_of_4.md) calls COMPAS
    
    
    
    
    
    
    .COMPAS
    
     JSR DOT                \ Call DOT to redraw (i.e. remove) the current compass
                            \ dot
    
     LDA SSPR               \ If we are inside the space station safe zone, jump to
     BNE SP1                \ SP1 to draw the space station on the compass
    
     JSR SPS1               \ Otherwise we need to draw the planet on the compass,
                            \ so first call SPS1 to calculate the vector to the
                            \ planet and store it in XX15
    
     JMP SP2                \ Jump to SP2 to draw XX15 on the compass, returning
                            \ from the subroutine using a tail call
    

[X]

Subroutine [DOT](dot.md) (category: Dashboard)

Draw a dash on the compass

[X]

Subroutine [SP1](sp1.md) (category: Dashboard)

Draw the space station on the compass

[X]

Subroutine [SP2](sp2.md) (category: Dashboard)

Draw a dot on the compass, given the planet/station vector

[X]

Subroutine [SPS1](sps1.md) (category: Maths (Geometry))

Calculate the vector to the planet and store it in XX15

[X]

Variable [SSPR](../workspace/wp.md#sspr) in workspace [WP](../workspace/wp.md)

"Space station present" flag

[DENGY](dengy.md "Previous routine")[SPS2](sps2.md "Next routine")
