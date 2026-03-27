---
title: "The CHKON subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/chkon.html"
---

[EDGES](edges.md "Previous routine")[PL21](pl21.md "Next routine")
    
    
           Name: CHKON                                                   [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Check whether any part of a circle appears on the extended screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-chkon)
     Variations: See [code variations](../../related/compare/main/subroutine/chkon.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CIRCLE](circle.md) calls CHKON
                 * [SUN (Part 1 of 4)](sun_part_1_of_4.md) calls CHKON
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       K                    The circle's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the circle
    
       K4(1 0)              Pixel y-coordinate of the centre of the circle
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Clear if any part of the circle appears on-screen, set
                            if none of the circle appears on-screen
    
       (A X)                Minimum y-coordinate of the circle on-screen (i.e. the
                            y-coordinate of the top edge of the circle)
    
       P(2 1)               Maximum y-coordinate of the circle on-screen (i.e. the
                            y-coordinate of the bottom edge of the circle)
    
    
    
    
    .CHKON
    
     LDA K3                 \ Set A = K3 + K
     CLC
     ADC K
    
     LDA K3+1               \ Set A = K3+1 + 0 + any carry from above, so this
     ADC #0                 \ effectively sets A to the high byte of K3(1 0) + K:
                            \
                            \   (A ?) = K3(1 0) + K
                            \
                            \ so A is the high byte of the x-coordinate of the right
                            \ edge of the circle
    
     BMI PL21               \ If A is negative then the right edge of the circle is
                            \ to the left of the screen, so jump to PL21 to set the
                            \ C flag and return from the subroutine, as the whole
                            \ circle is off-screen to the left
    
     LDA K3                 \ Set A = K3 - K
     SEC
     SBC K
    
     LDA K3+1               \ Set A = K3+1 - 0 - any carry from above, so this
     SBC #0                 \ effectively sets A to the high byte of K3(1 0) - K:
                            \
                            \   (A ?) = K3(1 0) - K
                            \
                            \ so A is the high byte of the x-coordinate of the left
                            \ edge of the circle
    
     BMI PL31               \ If A is negative then the left edge of the circle is
                            \ to the left of the screen, and we already know the
                            \ right edge is either on-screen or off-screen to the
                            \ right, so skip to PL31 to move on to the y-coordinate
                            \ checks, as at least part of the circle is on-screen in
                            \ terms of the x-axis
    
     BNE PL21               \ If A is non-zero, then the left edge of the circle is
                            \ to the right of the screen, so jump to PL21 to set the
                            \ C flag and return from the subroutine, as the whole
                            \ circle is off-screen to the right
    
    .PL31
    
     LDA K4                 \ Set P+1 = K4 + K
     CLC
     ADC K
     STA P+1
    
     LDA K4+1               \ Set A = K4+1 + 0 + any carry from above, so this
     ADC #0                 \ does the following:
                            \
                            \   (A P+1) = K4(1 0) + K
                            \
                            \ so A is the high byte of the y-coordinate of the
                            \ bottom edge of the circle
    
     BMI PL21               \ If A is negative then the bottom edge of the circle is
                            \ above the top of the screen, so jump to PL21 to set
                            \ the C flag and return from the subroutine, as the
                            \ whole circle is off-screen to the top
    
     STA P+2                \ Store the high byte in P+2, so now we have:
                            \
                            \   P(2 1) = K4(1 0) + K
                            \
                            \ i.e. the maximum y-coordinate of the circle on-screen
                            \ (which we return)
    
     LDA K4                 \ Set X = K4 - K
     SEC
     SBC K
     TAX
    
     LDA K4+1               \ Set A = K4+1 - 0 - any carry from above, so this
     SBC #0                 \ does the following:
                            \
                            \   (A X) = K4(1 0) - K
                            \
                            \ so A is the high byte of the y-coordinate of the top
                            \ edge of the circle
    
     BMI PL44               \ If A is negative then the top edge of the circle is
                            \ above the top of the screen, and we already know the
                            \ bottom edge is either on-screen or below the bottom
                            \ of the screen, so skip to PL44 to clear the C flag and
                            \ return from the subroutine using a tail call, as part
                            \ of the circle definitely appears on-screen
    
     BNE PL21               \ If A is non-zero, then the top edge of the circle is
                            \ below the bottom of the screen, so jump to PL21 to set
                            \ the C flag and return from the subroutine, as the
                            \ whole circle is off-screen to the bottom
    
     CPX Yx2M1              \ If we get here then A is zero, which means the top
                            \ edge of the circle is within the screen boundary, so
                            \ now we need to check whether it is in the space view
                            \ (in which case it is on-screen) or the dashboard (in
                            \ which case the top of the circle is hidden by the
                            \ dashboard, so the circle isn't on-screen). We do this
                            \ by checking the low byte of the result in X against
                            \ Yx2M1, and returning the C flag from this comparison.
                            \ The value in Yx2M1 is the y-coordinate of the bottom
                            \ pixel row of the space view, so this does the
                            \ following:
                            \
                            \   * The C flag is set if coordinate (A X) is below the
                            \     bottom row of the space view, i.e. the top edge of
                            \     the circle is hidden by the dashboard
                            \
                            \   * The C flag is clear if coordinate (A X) is above
                            \     the bottom row of the space view, i.e. the top
                            \     edge of the circle is on-screen
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [K4](../workspace/zp.md#k4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PL21](pl21.md) (category: Drawing planets)

Return from a planet/sun-drawing routine with a failure flag

[X]

Label [PL31](chkon.md#pl31) is local to this routine

[X]

Entry point [PL44](pls6.md#pl44) in subroutine [PLS6](pls6.md) (category: Drawing planets)

Clear the C flag and return from the subroutine

[X]

Variable [Yx2M1](../workspace/zp.md#yx2m1) in workspace [ZP](../workspace/zp.md)

This is used to store the number of pixel rows in the space view minus 1, which is also the y-coordinate of the bottom pixel row of the space view (it is set to 191 in the RES2 routine)

[EDGES](edges.md "Previous routine")[PL21](pl21.md "Next routine")
