---
title: "The exlook variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/exlook.html"
---

[DOEXP](../subroutine/doexp.md "Previous routine")[coltabl](coltabl.md "Next routine")
    
    
           Name: exlook                                                  [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: A table to shift X left by one place when X is 0 or 1
    
    
        Context: See this variable [in context in the source code](../../all/elite_e.md#header-exlook)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .exlook
    
     EQUB %00               \ Looking up exlook,X will return X shifted left by one
     EQUB %10               \ place, where X is 0 or 1
                            \
                            \ This is not used in this version of Elite; it is left
                            \ over from the Commodore 64 version of Elite
    

[DOEXP](../subroutine/doexp.md "Previous routine")[coltabl](coltabl.md "Next routine")
