---
title: "The refund subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/refund.html"
---

[hm](hm.md "Previous routine")[PRXS](../variable/prxs.md "Next routine")
    
    
           Name: refund                                                  [Show more]
           Type: Subroutine
       Category: Equipment
        Summary: Install a new laser, processing a refund if applicable
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-refund)
     Variations: See [code variations](../../related/compare/main/subroutine/refund.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [EQSHP](eqshp.md) calls refund
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The power of the new laser to be fitted
    
       X                    The view number for fitting the new laser (0-3)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       X                    X is preserved
    
    
    
    
    \.ref2                  \ These instructions are commented out in the original
    \                       \ source, but they would jump to pres in the EQSHP
    \LDY #187               \ routine with Y = 187, which would show the error:
    \JMP pres               \ "LASER PRESENT" (this code was part of the refund
    \                       \ bug in the BBC Micro disc version of Elite, which
    \Belgium                \ is why it is commented out)
                            \
                            \ There is also a comment in the original source - the
                            \ solitary word "Belgium"
                            \
                            \ This is presumably a reference to the Hitchhiker's
                            \ Guide to the Galaxy, which says that Belgium is the
                            \ galaxy's most unspeakably rude word, so this no doubt
                            \ reflects the authors' strong feelings on the refund
                            \ bug
    
    .refund
    
     STA T1                 \ Store A in T1 so we can retrieve it later
    
     LDA LASER,X            \ If there is no laser in view X (i.e. the laser power
     BEQ ref3               \ is zero), jump to ref3 to skip the refund code
    
    \CMP T1                 \ These instructions are commented out in the original
    \BEQ ref2               \ source, but they would jump to ref2 above if we were
                            \ trying to replace a laser with one of the same type
                            \ (this code was part of the refund bug in the BBC Micro
                            \ disc version of Elite, which is why it is commented
                            \ out)
    
     LDY #4                 \ If the current laser has power #POW (pulse laser),
     CMP #POW               \ jump to ref1 with Y = 4 (the item number of a pulse
     BEQ ref1               \ laser in the table at PRXS)
    
     LDY #5                 \ If the current laser has power #POW+128 (beam laser),
     CMP #POW+128           \ jump to ref1 with Y = 5 (the item number of a beam
     BEQ ref1               \ laser in the table at PRXS)
    
     LDY #12                \ If the current laser has power #Armlas (military
     CMP #Armlas            \ laser), jump to ref1 with Y = 12 (the item number of a
     BEQ ref1               \ military laser in the table at PRXS)
    
     LDY #13                \ Otherwise this is a mining laser, so fall through into
                            \ ref1 with Y = 13 (the item number of a mining laser in
                            \ the table at PRXS)
    
    .ref1
    
                            \ We now want to refund the laser of type Y that we are
                            \ exchanging for the new laser
    
     STX ZZ                 \ Store the view number in ZZ so we can retrieve it
                            \ later
    
     TYA                    \ Copy the laser type to be refunded from Y to A
    
     JSR prx                \ Call prx to set (Y X) to the price of equipment item
                            \ number A
    
     JSR MCASH              \ Call MCASH to add (Y X) to the cash pot
    
     LDX ZZ                 \ Retrieve the view number from ZZ
    
    .ref3
    
                            \ Finally, we install the new laser
    
     LDA T1                 \ Retrieve the new laser's power from T1 into A
    
     STA LASER,X            \ Set the laser view to the new laser's power
    
     RTS                    \ Return from the subroutine
    

[X]

Configuration variable [Armlas](../../all/workspaces.md#armlas) = INT(128.5 + 1.5*POW)

Military laser power

[X]

Variable [LASER](../workspace/wp.md#laser) in workspace [WP](../workspace/wp.md)

The specifications of the lasers fitted to each of the four space views

[X]

Subroutine [MCASH](mcash.md) (category: Maths (Arithmetic))

Add an amount of cash to the cash pot

[X]

Configuration variable [POW](../../all/workspaces.md#pow) = 15

Pulse laser power

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [ZZ](../workspace/zp.md#zz) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for distance values

[X]

Subroutine [prx](prx.md) (category: Equipment)

Return the price of a piece of equipment

[X]

Label [ref1](refund.md#ref1) is local to this routine

[X]

Label [ref3](refund.md#ref3) is local to this routine

[hm](hm.md "Previous routine")[PRXS](../variable/prxs.md "Next routine")
