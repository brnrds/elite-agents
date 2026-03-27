---
title: "The NMIpissoff subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nmipissoff.html"
---

[COLD](cold.md "Previous routine")[F%](../variable/f_per_cent.md "Next routine")
    
    
           Name: NMIpissoff                                              [Show more]
           Type: Subroutine
       Category: Loader
        Summary: Acknowledge NMI interrupts and ignore them
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-nmipissoff)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    IF _SNG47
    
    .NMIpissoff
    
     CLI                    \ These instructions are never reached and have no
     RTI                    \ effect
    
    ENDIF
    

[COLD](cold.md "Previous routine")[F%](../variable/f_per_cent.md "Next routine")
