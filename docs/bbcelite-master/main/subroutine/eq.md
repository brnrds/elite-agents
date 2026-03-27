---
title: "The eq subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/eq.html"
---

[dn2](dn2.md "Previous routine")[prx](prx.md "Next routine")
    
    
           Name: eq                                                      [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Subtract the price of equipment from the cash pot
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-eq)
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls eq
    
    
    
    
    
    * * *
    
    
     If we have enough cash, subtract the price of a specified piece of equipment
     from our cash pot and return from the subroutine. If we don't have enough
     cash, exit to the docking bay (i.e. show the Status Mode screen).
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The item number of the piece of equipment (0-11) as
                            shown in the table at PRXS
    
    
    
    
    .eq
    
     JSR prx                \ Call prx to set (Y X) to the price of equipment item
                            \ number A
    
     JSR LCASH              \ Subtract (Y X) cash from the cash pot, but only if
                            \ we have enough cash
    
     BCS c                  \ If the C flag is set then we did have enough cash for
                            \ the transaction, so jump to c to return from the
                            \ subroutine (as c contains an RTS)
    
     LDA #197               \ Otherwise we don't have enough cash to buy this piece
     JSR prq                \ of equipment, so print recursive token 37 ("CASH")
                            \ followed by a question mark
    
     JMP err                \ Jump to err to beep, pause and go to the docking bay
                            \ (i.e. show the Status Mode screen)
    

[X]

Subroutine [LCASH](lcash.md) (category: Maths (Arithmetic))

Subtract an amount of cash from the cash pot

[X]

Entry point [c](prx.md#c) in subroutine [prx](prx.md) (category: Equipment)

Contains an RTS

[X]

Entry point [err](eqshp.md#err) in subroutine [EQSHP](eqshp.md) (category: Equipment)

Beep, pause and go to the docking bay (i.e. show the Status Mode screen)

[X]

Subroutine [prq](prq.md) (category: Text)

Print a text token followed by a question mark

[X]

Subroutine [prx](prx.md) (category: Equipment)

Return the price of a piece of equipment

[dn2](dn2.md "Previous routine")[prx](prx.md "Next routine")
