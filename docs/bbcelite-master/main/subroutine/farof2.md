---
title: "The FAROF2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/farof2.html"
---

[FAROF](farof.md "Previous routine")[MAS4](mas4.md "Next routine")
    
    
           Name: FAROF2                                                  [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Compare x_hi, y_hi and z_hi with A
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-farof2)
     Variations: See [code variations](../../related/compare/main/subroutine/farof2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](main_flight_loop_part_14_of_16.md) calls FAROF2
    
    
    
    
    
    * * *
    
    
     Compare x_hi, y_hi and z_hi with A, and set the C flag if all three <= A,
     otherwise clear the C flag.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if x_hi <= A and y_hi <= A and z_hi <= A
    
                            Clear otherwise (i.e. if any one of them are bigger than
                            A)
    
    
    
    
    .FAROF2
    
     CMP INWK+1             \ If A < x_hi, C will be clear so jump to FA1 to
     BCC FA1                \ return from the subroutine with C clear, otherwise
                            \ C will be set so move on to the next one
    
     CMP INWK+4             \ If A < y_hi, C will be clear so jump to FA1 to
     BCC FA1                \ return from the subroutine with C clear, otherwise
                            \ C will be set so move on to the next one
    
     CMP INWK+7             \ If A < z_hi, C will be clear, otherwise C will be set
    
    .FA1
    
     RTS                    \ Return from the subroutine
    

[X]

Label [FA1](farof2.md#fa1) is local to this routine

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[FAROF](farof.md "Previous routine")[MAS4](mas4.md "Next routine")
