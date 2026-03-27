---
title: "The TAS6 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tas6.html"
---

[TAS4](tas4.md "Previous routine")[DCS1](dcs1.md "Next routine")
    
    
           Name: TAS6                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Negate the vector in XX15 so it points in the opposite direction
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tas6)
     References: This subroutine is called as follows:
                 * [DOCKIT](dockit.md) calls TAS6
                 * [TACTICS (Part 7 of 7)](tactics_part_7_of_7.md) calls TAS6
    
    
    
    
    
    
    .TAS6
    
     LDA XX15               \ Reverse the sign of the x-coordinate of the vector in
     EOR #%10000000         \ XX15
     STA XX15
    
     LDA XX15+1             \ Then reverse the sign of the y-coordinate
     EOR #%10000000
     STA XX15+1
    
     LDA XX15+2             \ And then the z-coordinate, so now the XX15 vector is
     EOR #%10000000         \ pointing in the opposite direction
     STA XX15+2
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[TAS4](tas4.md "Previous routine")[DCS1](dcs1.md "Next routine")
