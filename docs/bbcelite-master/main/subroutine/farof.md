---
title: "The FAROF subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/farof.html"
---

[BAD](bad.md "Previous routine")[FAROF2](farof2.md "Next routine")
    
    
           Name: FAROF                                                   [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Compare x_hi, y_hi and z_hi with 224
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-farof)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md) calls FAROF
    
    
    
    
    
    * * *
    
    
     Compare x_hi, y_hi and z_hi with 224, and set the C flag if all three <= 224,
     otherwise clear the C flag.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if x_hi <= 224 and y_hi <= 224 and z_hi <= 224
    
                            Clear otherwise (i.e. if any one of them are bigger than
                            224)
    
    
    
    
    .FAROF
    
     LDA #224               \ Set A = 224 and fall through into FAROF2 to do the
                            \ comparison
    

[BAD](bad.md "Previous routine")[FAROF2](farof2.md "Next routine")
