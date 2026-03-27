---
title: "The SP1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sp1.html"
---

[SPS4](sps4.md "Previous routine")[SP2](sp2.md "Next routine")
    
    
           Name: SP1                                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Draw the space station on the compass
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-sp1)
     References: This subroutine is called as follows:
                 * [COMPAS](compas.md) calls SP1
    
    
    
    
    
    
    .SP1
    
     JSR SPS4               \ Call SPS4 to calculate the vector to the space station
                            \ and store it in XX15
    
                            \ Fall through into SP2 to draw XX15 on the compass
    

[X]

Subroutine [SPS4](sps4.md) (category: Maths (Geometry))

Calculate the vector to the space station

[SPS4](sps4.md "Previous routine")[SP2](sp2.md "Next routine")
