---
title: "The prx subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/prx.html"
---

[eq](eq.md "Previous routine")[qv](qv.md "Next routine")
    
    
           Name: prx                                                     [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Return the price of a piece of equipment
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-prx)
     Variations: See [code variations](../../related/compare/main/subroutine/prx.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [eq](eq.md) calls prx
                 * [EQSHP](eqshp.md) calls prx
                 * [refund](refund.md) calls prx
                 * [EQSHP](eqshp.md) calls via prx-3
                 * [eq](eq.md) calls via c
    
    
    
    
    
    * * *
    
    
     This routine returns the price of equipment as listed in the table at PRXS.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The item number of the piece of equipment (0-13) as
                            shown in the table at PRXS
    
    
    
    * * *
    
    
     Returns:
    
       (Y X)                The item price in Cr * 10 (Y = high byte, X = low byte)
    
    
    
    * * *
    
    
     Other entry points:
    
       prx-3                Return the price of the item with number A - 1
    
       c                    Contains an RTS
    
    
    
    
     SEC                    \ Decrement A (for when this routine is called via
     SBC #1                 \ prx-3)
    
    .prx
    
     ASL A                  \ Set Y = A * 2, so it can act as an index into the
     TAY                    \ PRXS table, which has two bytes per entry
    
     LDX PRXS,Y             \ Fetch the low byte of the price into X
    
     LDA PRXS+1,Y           \ Fetch the high byte of the price into A and transfer
     TAY                    \ it to X, so the price is now in (Y X)
    
    .c
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [PRXS](../variable/prxs.md) (category: Equipment)

Equipment prices

[eq](eq.md "Previous routine")[qv](qv.md "Next routine")
