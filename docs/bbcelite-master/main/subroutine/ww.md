---
title: "The wW subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ww.html"
---

[hyp](hyp.md "Previous routine")[TTX110](ttx110.md "Next routine")
    
    
           Name: wW                                                      [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Start a hyperspace countdown
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-ww)
     Variations: See [code variations](../../related/compare/main/subroutine/ww.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Ghy](ghy.md) calls via wW2
    
    
    
    
    
    * * *
    
    
     Start the hyperspace countdown (for both inter-system hyperspace and the
     galactic hyperdrive).
    
    
    
    * * *
    
    
     Other entry points:
    
       wW2                  Start the hyperspace countdown, starting the countdown
                            from the value in A
    
    
    
    
    .wW
    
     LDA #15                \ The hyperspace countdown starts from 15, so set A to
                            \ 15 so we can set the two hyperspace counters
    
    .wW2
    
     STA QQ22+1             \ Set the number in QQ22+1 to A, which is the number
                            \ that's shown on-screen during the hyperspace countdown
    
     STA QQ22               \ Set the number in QQ22 to 15, which is the internal
                            \ counter that counts down by 1 each iteration of the
                            \ main game loop, and each time it reaches zero, the
                            \ on-screen counter gets decremented, and QQ22 gets set
                            \ to 5, so setting QQ22 to 15 here makes the first tick
                            \ of the hyperspace counter longer than subsequent ticks
    
     TAX                    \ Print the 8-bit number in X (i.e. 15) at text location
     JMP ee3                \ (0, 1), padded to 5 digits, so it appears in the top
                            \ left corner of the screen, and return from the
                            \ subroutine using a tail call
    

[X]

Variable [QQ22](../workspace/zp.md#qq22) in workspace [ZP](../workspace/zp.md)

The two hyperspace countdown counters

[X]

Subroutine [ee3](ee3.md) (category: Flight)

Print the hyperspace countdown in the top-left of the screen

[hyp](hyp.md "Previous routine")[TTX110](ttx110.md "Next routine")
