---
title: "The PL2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pl2.html"
---

[PROJ](proj.md "Previous routine")[PLANET](planet.md "Next routine")
    
    
           Name: PL2                                                     [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Remove the planet or sun from the screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-pl2)
     Variations: See [code variations](../../related/compare/main/subroutine/pl2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLANET](planet.md) calls PL2
                 * [PROJ](proj.md) calls via PL2-1
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       PL2-1                Contains an RTS
    
    
    
    
    .PL2
    
     LDA TYPE               \ Shift bit 0 of the planet/sun's type into the C flag
     LSR A
    
     BCS P%+5               \ If the planet/sun's type has bit 0 clear, then it's
                            \ either 128 or 130, which is a planet; meanwhile, the
                            \ sun has type 129, which has bit 0 set. So if this is
                            \ the sun, skip the following instruction
    
     JMP WPLS2              \ This is the planet, so jump to WPLS2 to remove it from
                            \ screen, returning from the subroutine using a tail
                            \ call
    
     JMP WPLS               \ This is the sun, so jump to WPLS to remove it from
                            \ screen, returning from the subroutine using a tail
                            \ call
    

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Subroutine [WPLS](wpls.md) (category: Drawing suns)

Remove the sun from the screen

[X]

Subroutine [WPLS2](wpls2.md) (category: Drawing planets)

Remove the planet from the screen

[PROJ](proj.md "Previous routine")[PLANET](planet.md "Next routine")
