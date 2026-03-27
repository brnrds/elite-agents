---
title: "The ypl subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ypl.html"
---

[cmn](cmn.md "Previous routine")[tal](tal.md "Next routine")
    
    
           Name: ypl                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Print the current system name
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-ypl)
     Variations: See [code variations](../../related/compare/main/subroutine/ypl.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT27](tt27.md) calls ypl
                 * [cmn](cmn.md) calls via ypl-1
    
    
    
    
    
    * * *
    
    
     Print control code 2 (the current system name).
    
    
    
    * * *
    
    
     Other entry points:
    
       ypl-1                Contains an RTS
    
    
    
    
    .ypl
    
     BIT MJ                 \ Check the mis-jump flag at MJ, and if bit 7 is set
     BMI ypl16              \ then we are in witchspace, and witchspace doesn't have
                            \ a system name, so jump to ypl16 to return from the
                            \ subroutine
    
     JSR TT62               \ Call TT62 below to swap the three 16-bit seeds in
                            \ QQ2 and QQ15 (before the swap, QQ2 contains the seeds
                            \ for the current system, while QQ15 contains the seeds
                            \ for the selected system)
    
     JSR cpl                \ Call cpl to print out the system name for the seeds
                            \ in QQ15 (which now contains the seeds for the current
                            \ system)
    
                            \ Now we fall through into the TT62 subroutine, which
                            \ will swap QQ2 and QQ15 once again, so everything goes
                            \ back into the right place, and the RTS at the end of
                            \ TT62 will return from the subroutine
    
    .TT62
    
     LDX #5                 \ Set up a counter in X for the three 16-bit seeds we
                            \ want to swap (i.e. 6 bytes)
    
    .TT78
    
     LDA QQ15,X             \ Swap byte X between QQ2 and QQ15
     LDY QQ2,X
     STA QQ2,X
     STY QQ15,X
    
     DEX                    \ Decrement the loop counter
    
     BPL TT78               \ Loop back for the next byte to swap
    
    .ypl16
    
     RTS                    \ Once all bytes are swapped, return from the
                            \ subroutine
    

[X]

Variable [MJ](../workspace/wp.md#mj) in workspace [WP](../workspace/wp.md)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Variable [QQ15](../workspace/zp.md#qq15) in workspace [ZP](../workspace/zp.md)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ2](../workspace/wp.md#qq2) in workspace [WP](../workspace/wp.md)

The three 16-bit seeds for the current system, i.e. the one we are currently in

[X]

Label [TT62](ypl.md#tt62) is local to this routine

[X]

Label [TT78](ypl.md#tt78) is local to this routine

[X]

Subroutine [cpl](cpl.md) (category: Universe)

Print the selected system name

[X]

Label [ypl16](ypl.md#ypl16) is local to this routine

[cmn](cmn.md "Previous routine")[tal](tal.md "Next routine")
