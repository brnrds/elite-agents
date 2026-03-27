---
title: "The MUT3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mut3.html"
---

[MLTU2](mltu2.md "Previous routine")[MUT2](mut2.md "Next routine")
    
    
           Name: MUT3                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: An unused routine that does the same as MUT2
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-mut3)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine is never actually called, but it is identical to MUT2, as the
     extra instructions have no effect.
    
    
    
    
    .MUT3
    
     LDX ALP1               \ Set P = ALP1, though this gets overwritten by the
     STX P                  \ following, so this has no effect
    
                            \ Fall through into MUT2 to do the following:
                            \
                            \   (S R) = XX(1 0)
                            \   (A P) = Q * A
    

[X]

Variable [ALP1](../workspace/zp.md#alp1) in workspace [ZP](../workspace/zp.md)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MLTU2](mltu2.md "Previous routine")[MUT2](mut2.md "Next routine")
