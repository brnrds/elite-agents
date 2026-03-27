---
title: "The DOVDU19 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dovdu19.html"
---

[VSCAN](../variable/vscan.md "Previous routine")[setzp](setzp.md "Next routine")
    
    
           Name: DOVDU19                                                 [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Change the mode 1 palette
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-dovdu19)
     Variations: See [code variations](../../related/compare/main/subroutine/setvdu19-dovdu19.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HALL](hall.md) calls DOVDU19
                 * [LOOK1](look1.md) calls DOVDU19
                 * [TITLE](title.md) calls DOVDU19
                 * [TRADEMODE](trademode.md) calls DOVDU19
                 * [TT22](tt22.md) calls DOVDU19
                 * [TT23](tt23.md) calls DOVDU19
    
    
    
    
    
    * * *
    
    
     This routine updates the VNT3+1 location in the IRQ1 handler to change the
     palette that's applied to the top part of the screen (the four-colour mode 1
     part). The parameter is the offset within the TVT3 palette block of the
     desired palette.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The offset within the TVT3 table of palettes:
    
                              * 0 = Yellow, red, cyan palette (space view)
    
                              * 16 = Yellow, red, white palette (charts)
    
                              * 32 = Yellow, white, cyan palette (title screen)
    
                              * 48 = Yellow, magenta, white palette (trading)
    
    
    
    
    .DOVDU19
    
     STA VNT3+1             \ Store the new colour in VNT3+1, in the IRQ1 routine,
                            \ which modifies which TVT3 palette block gets applied
                            \ to the mode 1 part of the screen
    
     RTS                    \ Return from the subroutine
    

[X]

Label [VNT3](irq1.md#vnt3) in subroutine [IRQ1](irq1.md)

[VSCAN](../variable/vscan.md "Previous routine")[setzp](setzp.md "Next routine")
