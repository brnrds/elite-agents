---
title: "The PRXS variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/prxs.html"
---

[refund](../subroutine/refund.md "Previous routine")[cpl](../subroutine/cpl.md "Next routine")
    
    
           Name: PRXS                                                    [Show more]
           Type: Variable
       Category: Equipment
        Summary: Equipment prices
    
    
        Context: See this variable [in context in the source code](../../all/elite_d.md#header-prxs)
     Variations: See [code variations](../../related/compare/main/variable/prxs.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [EQSHP](../subroutine/eqshp.md) uses PRXS
                 * [prx](../subroutine/prx.md) uses PRXS
    
    
    
    
    
    * * *
    
    
     Equipment prices are stored as 10 * the actual value, so we can support prices
     with fractions of credits (0.1 Cr). This is used for the price of fuel only.
    
    
    
    
    .PRXS
    
     EQUW 1                 \ 0  Fuel, calculated in EQSHP  140.0 Cr (full tank)
     EQUW 300               \ 1  Missile                     30.0 Cr
     EQUW 4000              \ 2  Large Cargo Bay            400.0 Cr
     EQUW 6000              \ 3  E.C.M. System              600.0 Cr
     EQUW 4000              \ 4  Extra Pulse Lasers         400.0 Cr
     EQUW 10000             \ 5  Extra Beam Lasers         1000.0 Cr
     EQUW 5250              \ 6  Fuel Scoops                525.0 Cr
     EQUW 10000             \ 7  Escape Pod                1000.0 Cr
     EQUW 9000              \ 8  Energy Bomb                900.0 Cr
     EQUW 15000             \ 9  Energy Unit               1500.0 Cr
     EQUW 10000             \ 10 Docking Computer          1000.0 Cr
     EQUW 50000             \ 11 Galactic Hyperspace       5000.0 Cr
     EQUW 60000             \ 12 Extra Military Lasers     6000.0 Cr
     EQUW 8000              \ 13 Extra Mining Lasers        800.0 Cr
    

[refund](../subroutine/refund.md "Previous routine")[cpl](../subroutine/cpl.md "Next routine")
