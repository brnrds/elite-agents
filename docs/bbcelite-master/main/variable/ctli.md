---
title: "The CTLI variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/ctli.html"
---

[CLDELAY](../subroutine/cldelay.md "Previous routine")[DELI](deli.md "Next routine")
    
    
           Name: CTLI                                                    [Show more]
           Type: Variable
       Category: Save and load
        Summary: The OS command string for cataloguing a disc
    
    
        Context: See this variable [in context in the source code](../../all/elite_f.md#header-ctli)
     Variations: See [code variations](../../related/compare/main/variable/ctli.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [CATS](../subroutine/cats.md) uses CTLI
                 * [DELT](../subroutine/delt.md) uses CTLI
    
    
    
    
    
    
    .CTLI
    
    IF _SNG47
    
     EQUS "CAT 1"           \ The "1" part of the string is overwritten with the
     EQUB 13                \ actual drive number by the CATS routine
    
    ELIF _COMPACT
    
     EQUS "CAT"             \ The Master Compact only has one drive, so the CAT
     EQUB 13                \ command always catalogues that drive
    
    ENDIF
    

[CLDELAY](../subroutine/cldelay.md "Previous routine")[DELI](deli.md "Next routine")
