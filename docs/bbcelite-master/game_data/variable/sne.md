---
title: "The SNE (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/sne.html"
---

[QQ18 (Game data)](qq18.md "Previous routine")[ACT (Game data)](act.md "Next routine")
    
    
           Name: SNE                                                     [Show more]
           Type: Variable
       Category: Maths (Geometry)
        Summary: Sine/cosine table
      Deep dive: [The sine, cosine and arctan tables](https://elite.bbcelite.com/deep_dives/the_sine_cosine_and_arctan_tables.html)
                 [Drawing circles](https://elite.bbcelite.com/deep_dives/drawing_circles.html)
                 [Drawing ellipses](https://elite.bbcelite.com/deep_dives/drawing_ellipses.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-sne)
     References: This variable is used as follows:
                 * [FMLTU2](../../main/subroutine/fmltu2.md) uses SNE
                 * [PLS22](../../main/subroutine/pls22.md) uses SNE
    
    
    
    
    
    * * *
    
    
     This lookup table contains sine values for the first half of a circle, from 0
     to 180 degrees (0 to PI radians). In terms of circle or ellipse line segments,
     there are 64 segments in a circle, so this contains sine values for segments
     0 to 31.
    
     In terms of segments, to calculate the sine of the angle at segment x, we look
     up the value in SNE + x, and to calculate the cosine of the angle we look up
     the value in SNE + ((x + 16) mod 32).
    
     In terms of radians, to calculate the following:
    
       sin(theta) * 256
    
     where theta is in radians, we look up the value in:
    
       SNE + (theta * 10)
    
     To calculate the following:
    
       cos(theta) * 256
    
     where theta is in radians, look up the value in:
    
       SNE + ((theta * 10) + 16) mod 32
    
     Theta must be between 0 and 3.1 radians, so theta * 10 is between 0 and 31.
    
    
    
    
    .SNE
    
     FOR I%, 0, 31
    
      N = ABS(SIN((I% / 64) * 2 * PI))
    
      IF N >= 1
       EQUB 255
      ELSE
       EQUB INT(256 * N + 0.5)
      ENDIF
    
     NEXT
    

[QQ18 (Game data)](qq18.md "Previous routine")[ACT (Game data)](act.md "Next routine")
