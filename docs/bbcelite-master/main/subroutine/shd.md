---
title: "The SHD subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/shd.html"
---

[DET1](det1.md "Previous routine")[DENGY](dengy.md "Next routine")
    
    
           Name: SHD                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Charge a shield and drain some energy from the energy banks
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-shd)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 13 of 16)](main_flight_loop_part_13_of_16.md) calls SHD
    
    
    
    
    
    * * *
    
    
     Charge up a shield, and if it needs charging, drain some energy from the
     energy banks.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The value of the shield to recharge
    
    
    
    
     DEX                    \ If we get here then we just incremented the shield
                            \ value back around to zero, so decrement it back down
                            \ to 255 so it stays at the maximum value of 255
    
     RTS                    \ Return from the subroutine
    
    .SHD
    
     INX                    \ Increment the shield value
    
     BEQ SHD-2              \ If the shield value is 0 then this means it was 255
                            \ before, which is the maximum value, so jump to SHD-2
                            \ to bring it back down to 255 and return without
                            \ draining our energy banks
    
                            \ Otherwise fall through into DENGY to drain our energy
                            \ to pay for all this shield charging
    

[X]

Subroutine [SHD](shd.md) (category: Flight)

Charge a shield and drain some energy from the energy banks

[X]

[DET1](det1.md "Previous routine")[DENGY](dengy.md "Next routine")
