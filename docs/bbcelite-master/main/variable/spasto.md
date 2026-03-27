---
title: "The spasto variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/spasto.html"
---

[DEATH](../subroutine/death.md "Previous routine")[BEGIN](../subroutine/begin.md "Next routine")
    
    
           Name: spasto                                                  [Show more]
           Type: Variable
       Category: Universe
        Summary: Contains the address of the Coriolis space station's ship
                 blueprint
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-spasto)
     References: This variable is used as follows:
                 * [BEGIN](../subroutine/begin.md) uses spasto
                 * [NWSPS](../subroutine/nwsps.md) uses spasto
    
    
    
    
    
    
    .spasto
    
     EQUW &8888             \ This variable is set by routine BEGIN to the address
                            \ of the Coriolis space station's ship blueprint
    

[DEATH](../subroutine/death.md "Previous routine")[BEGIN](../subroutine/begin.md "Next routine")
