---
title: "The SPMASK variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/spmask.html"
---

[TRIBDIRH](tribdirh.md "Previous routine")[Main flight loop (Part 1 of 16)](../subroutine/main_flight_loop_part_1_of_16.md "Next routine")
    
    
           Name: SPMASK                                                  [Show more]
           Type: Variable
       Category: Missions
        Summary: Masks for updating sprite bits in VIC+&10 for the top bit of the
                 9-bit x-coordinates of the Trumble sprites
    
    
        Context: See this variable [in context in the source code](../../all/elite_a.md#header-spmask)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .SPMASK
    
     EQUW &04FB             \ These bytes are unused and are left over from the
     EQUW &08F7             \ Commodore 64 version
     EQUW &10EF
     EQUW &20DF
     EQUW &40BF
     EQUW &807F
    

[TRIBDIRH](tribdirh.md "Previous routine")[Main flight loop (Part 1 of 16)](../subroutine/main_flight_loop_part_1_of_16.md "Next routine")
