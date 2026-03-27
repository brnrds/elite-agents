---
title: "The DENGY subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dengy.html"
---

[SHD](shd.md "Previous routine")[COMPAS](compas.md "Next routine")
    
    
           Name: DENGY                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Drain some energy from the energy banks
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-dengy)
     Variations: See [code variations](../../related/compare/main/subroutine/dengy.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LASLI](lasli.md) calls DENGY
                 * [Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md) calls DENGY
    
    
    
    
    
    * * *
    
    
     Returns:
    
       Z flag               Set if we have no energy left, clear otherwise
    
    
    
    
    .DENGY
    
     DEC ENERGY             \ Decrement the energy banks in ENERGY
    
     PHP                    \ Save the flags on the stack
    
     BNE paen2              \ If the energy levels are not yet zero, skip the
                            \ following instruction
    
     INC ENERGY             \ The minimum allowed energy level is 1, and we just
                            \ reached 0, so increment ENERGY back to 1
    
    .paen2
    
     PLP                    \ Restore the flags from the stack, so we return with
                            \ the Z flag from the DEC instruction above
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [ENERGY](../workspace/zp.md#energy) in workspace [ZP](../workspace/zp.md)

Energy bank status

[X]

Label [paen2](dengy.md#paen2) is local to this routine

[SHD](shd.md "Previous routine")[COMPAS](compas.md "Next routine")
