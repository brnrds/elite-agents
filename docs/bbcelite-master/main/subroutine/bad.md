---
title: "The BAD subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bad.html"
---

[TT102](tt102.md "Previous routine")[FAROF](farof.md "Next routine")
    
    
           Name: BAD                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Calculate how bad we have been
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-bad)
     References: This subroutine is called as follows:
                 * [Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md) calls BAD
                 * [TT110](tt110.md) calls BAD
    
    
    
    
    
    * * *
    
    
     Work out how bad we are from the amount of contraband in our hold. The
     formula is:
    
       (slaves + narcotics) * 2 + firearms
    
     so slaves and narcotics are twice as illegal as firearms. The value in FIST
     (our legal status) is set to at least this value whenever we launch from a
     space station, and a FIST of 50 or more gives us fugitive status, so leaving a
     station carrying 25 tonnes of slaves/narcotics, or 50 tonnes of firearms
     across multiple trips, is enough to make us a fugitive.
    
    
    
    * * *
    
    
     Returns:
    
       A                    A value that determines how bad we are from the amount
                            of contraband in our hold
    
    
    
    
    .BAD
    
     LDA QQ20+3             \ Set A to the number of tonnes of slaves in the hold
    
     CLC                    \ Clear the C flag so we can do addition without the
                            \ C flag affecting the result
    
     ADC QQ20+6             \ Add the number of tonnes of narcotics in the hold
    
     ASL A                  \ Double the result and add the number of tonnes of
     ADC QQ20+10            \ firearms in the hold
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [QQ20](../workspace/wp.md#qq20) in workspace [WP](../workspace/wp.md)

The contents of our cargo hold

[TT102](tt102.md "Previous routine")[FAROF](farof.md "Next routine")
