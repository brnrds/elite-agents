---
title: "The GVL subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/gvl.html"
---

[hyp1](hyp1.md "Previous routine")[GTHG](gthg.md "Next routine")
    
    
           Name: GVL                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Calculate the availability of market items
      Deep dive: [Market item prices and availability](https://elite.bbcelite.com/deep_dives/market_item_prices_and_availability.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-gvl)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Calculate the availability for each market item and store it in AVL. This is
     called on arrival in a new system.
    
    
    
    * * *
    
    
     Other entry points:
    
       hyR                  Contains an RTS
    
    
    
    
    .GVL
    
     JSR DORND              \ Set A and X to random numbers
    
     STA QQ26               \ Set QQ26 to the random byte that's used in the market
                            \ calculations
    
     LDX #0                 \ We are now going to loop through the market item
     STX XX4                \ availability table in AVL, so set a counter in XX4
                            \ (and X) for the market item number, starting with 0
    
    .hy9
    
     LDA QQ23+1,X           \ Fetch byte #1 from the market prices table (units and
     STA QQ19+1             \ economic_factor) for item number X and store it in
                            \ QQ19+1
    
     JSR var                \ Call var to set QQ19+3 = economy * |economic_factor|
                            \ (and set the availability of alien items to 0)
    
     LDA QQ23+3,X           \ Fetch byte #3 from the market prices table (mask) and
     AND QQ26               \ AND with the random number for this system visit
                            \ to give:
                            \
                            \   A = random AND mask
    
     CLC                    \ Add byte #2 from the market prices table
     ADC QQ23+2,X           \ (base_quantity) so we now have:
                            \
                            \   A = base_quantity + (random AND mask)
    
     LDY QQ19+1             \ Fetch the byte #1 that we stored above and jump to
     BMI TT157              \ TT157 if it is negative (i.e. if the economic_factor
                            \ is negative)
    
     SEC                    \ Set A = A - QQ19+3
     SBC QQ19+3             \
                            \       = base_quantity + (random AND mask)
                            \         - (economy * |economic_factor|)
                            \
                            \ which is the result we want, as the economic_factor
                            \ is positive
    
     JMP TT158              \ Jump to TT158 to skip TT157
    
    .TT157
    
     CLC                    \ Set A = A + QQ19+3
     ADC QQ19+3             \
                            \       = base_quantity + (random AND mask)
                            \         + (economy * |economic_factor|)
                            \
                            \ which is the result we want, as the economic_factor
                            \ is negative
    
    .TT158
    
     BPL TT159              \ If A < 0, then set A = 0, so we don't have negative
     LDA #0                 \ availability
    
    .TT159
    
     LDY XX4                \ Fetch the counter (the market item number) into Y
    
     AND #%00111111         \ Take bits 0-5 of A, i.e. A mod 64, and store this as
     STA AVL,Y              \ this item's availability in the Y=th byte of AVL, so
                            \ each item has a maximum availability of 63t
    
     INY                    \ Increment the counter into XX44, Y and A
     TYA
     STA XX4
    
     ASL A                  \ Set X = counter * 4, so that X points to the next
     ASL A                  \ item's entry in the four-byte market prices table,
     TAX                    \ ready for the next loop
    
     CMP #63                \ If A < 63, jump back up to hy9 to set the availability
     BCC hy9                \ for the next market item
    
    .hyR
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [AVL](../workspace/wp.md#avl) in workspace [WP](../workspace/wp.md)

Market availability in the current system

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [QQ19](../workspace/zp.md#qq19) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [QQ23](../variable/qq23.md) (category: Market)

Market prices table

[X]

Variable [QQ26](../workspace/wp.md#qq26) in workspace [WP](../workspace/wp.md)

A random value used to randomise market data

[X]

Label [TT157](gvl.md#tt157) is local to this routine

[X]

Label [TT158](gvl.md#tt158) is local to this routine

[X]

Label [TT159](gvl.md#tt159) is local to this routine

[X]

Variable [XX4](../workspace/zp.md#xx4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [hy9](gvl.md#hy9) is local to this routine

[X]

Subroutine [var](var.md) (category: Market)

Calculate QQ19+3 = economy * |economic_factor|

[hyp1](hyp1.md "Previous routine")[GTHG](gthg.md "Next routine")
