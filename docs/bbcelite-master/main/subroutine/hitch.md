---
title: "The HITCH subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hitch.html"
---

[DCS1](dcs1.md "Previous routine")[FRS1](frs1.md "Next routine")
    
    
           Name: HITCH                                                   [Show more]
           Type: Subroutine
       Category: Tactics
        Summary: Work out if the ship in INWK is in our crosshairs
      Deep dive: [In the crosshairs](https://elite.bbcelite.com/deep_dives/in_the_crosshairs.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-hitch)
     Variations: See [code variations](../../related/compare/main/subroutine/hitch.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](main_flight_loop_part_11_of_16.md) calls HITCH
                 * [ANGRY](angry.md) calls via HI1
    
    
    
    
    
    * * *
    
    
     This is called by the main flight loop to see if we have laser or missile lock
     on an enemy ship.
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the ship is in our crosshairs, clear if it isn't
    
    
    
    * * *
    
    
     Other entry points:
    
       HI1                  Contains an RTS
    
    
    
    
    .HITCH
    
     CLC                    \ Clear the C flag so we can return with it cleared if
                            \ our checks fail
    
     LDA INWK+8             \ Set A = z_sign
    
     BNE HI1                \ If A is non-zero then the ship is behind us and can't
                            \ be in our crosshairs, so return from the subroutine
                            \ with the C flag clear (as HI1 contains an RTS)
    
     LDA TYPE               \ If the ship type has bit 7 set then it is the planet
     BMI HI1                \ or sun, which we can't target or hit with lasers, so
                            \ return from the subroutine with the C flag clear (as
                            \ HI1 contains an RTS)
    
     LDA INWK+31            \ Fetch bit 5 of byte #31 (the exploding flag) and OR
     AND #%00100000         \ with x_hi and y_hi
     ORA INWK+1
     ORA INWK+4
    
     BNE HI1                \ If this value is non-zero then either the ship is
                            \ exploding (so we can't target it), or the ship is too
                            \ far away from our line of fire to be targeted, so
                            \ return from the subroutine with the C flag clear (as
                            \ HI1 contains an RTS)
    
     LDA INWK               \ Set A = x_lo
    
     JSR SQUA2              \ Set (A P) = A * A = x_lo^2
    
     STA S                  \ Set (S R) = (A P) = x_lo^2
     LDA P
     STA R
    
     LDA INWK+3             \ Set A = y_lo
    
     JSR SQUA2              \ Set (A P) = A * A = y_lo^2
    
     TAX                    \ Store the high byte in X
    
     LDA P                  \ Add the two low bytes, so:
     ADC R                  \
     STA R                  \   R = P + R
    
     TXA                    \ Restore the high byte into A and add S to give the
     ADC S                  \ following:
                            \
                            \   (A R) = (S R) + (A P) = x_lo^2 + y_lo^2
    
     BCS TN10               \ If the addition just overflowed then there is no way
                            \ our crosshairs are within the ship's targetable area,
                            \ so return from the subroutine with the C flag clear
                            \ (as TN10 contains a CLC then an RTS)
    
     STA S                  \ Set (S R) = (A P) = x_lo^2 + y_lo^2
    
     LDY #2                 \ Fetch the ship's blueprint and set A to the high byte
     LDA (XX0),Y            \ of the targetable area of the ship
    
     CMP S                  \ We now compare the high bytes of the targetable area
                            \ and the calculation in (S R):
                            \
                            \   * If A >= S then then the C flag will be set
                            \
                            \   * If A < S then the C flag will be C clear
    
     BNE HI1                \ If A <> S we have just set the C flag correctly, so
                            \ return from the subroutine (as HI1 contains an RTS)
    
     DEY                    \ The high bytes were identical, so now we fetch the
     LDA (XX0),Y            \ low byte of the targetable area into A
    
     CMP R                  \ We now compare the low bytes of the targetable area
                            \ and the calculation in (S R):
                            \
                            \   * If A >= R then the C flag will be set
                            \
                            \   * If A < R then the C flag will be C clear
    
    .HI1
    
     RTS                    \ Return from the subroutine
    
    .TN10
    
     CLC                    \ Clear the C flag to indicate the ship is not in our
                            \ crosshairs
    
     RTS                    \ Return from the subroutine
    

[X]

Entry point [HI1](hitch.md#hi1) in subroutine [HITCH](hitch.md) (category: Tactics)

Contains an RTS

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [SQUA2](squa2.md) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[X]

Label [TN10](hitch.md#tn10) is local to this routine

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[DCS1](dcs1.md "Previous routine")[FRS1](frs1.md "Next routine")
