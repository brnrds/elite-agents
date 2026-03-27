---
title: "The DIALS (Part 2 of 4) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dials_part_2_of_4.html"
---

[DIALS (Part 1 of 4)](dials_part_1_of_4.md "Previous routine")[DIALS (Part 3 of 4)](dials_part_3_of_4.md "Next routine")
    
    
           Name: DIALS (Part 2 of 4)                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the dashboard: pitch and roll indicators
      Deep dive: [The dashboard indicators](https://elite.bbcelite.com/deep_dives/the_dashboard_indicators.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-dials-part-2-of-4)
     Variations: See [code variations](../../related/compare/main/subroutine/dials_part_2_of_4.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
     STZ R                  \ Set R = P = 0 for the low bytes in the call to the ADD
     STZ P                  \ routine below
    
     LDA #8                 \ Set S = 8, which is the value of the centre of the
     STA S                  \ roll indicator
    
     LDA ALP1               \ Fetch the roll angle alpha as a value between 0 and
     LSR A                  \ 31, and divide by 4 to get a value of 0 to 7
     LSR A
    
     ORA ALP2               \ Apply the roll sign to the value, and flip the sign,
     EOR #%10000000         \ so it's now in the range -7 to +7, with a positive
                            \ roll angle alpha giving a negative value in A
    
     JSR ADDK               \ We now add A to S to give us a value in the range 1 to
                            \ 15, which we can pass to DIL2 to draw the vertical
                            \ bar on the indicator at this position. We use the ADD
                            \ routine like this:
                            \
                            \ (A X) = (A 0) + (S 0)
                            \
                            \ and just take the high byte of the result. We use ADD
                            \ rather than a normal ADC because ADD separates out the
                            \ sign bit and does the arithmetic using absolute values
                            \ and separate sign bits, which we want here rather than
                            \ the two's complement that ADC uses
    
     JSR DIL2               \ Draw a vertical bar on the roll indicator at offset A
                            \ and increment SC to point to the next indicator (the
                            \ pitch indicator)
    
     LDA BETA               \ Fetch the pitch angle beta as a value between -8 and
                            \ +8
    
     LDX BET1               \ Fetch the magnitude of the pitch angle beta, and if it
     BEQ P%+4               \ is 0 (i.e. we are not pitching), skip the next
                            \ instruction
    
     SBC #1                 \ The pitch angle beta is non-zero, so set A = A - 1
                            \ (the C flag is set by the call to DIL2 above, so we
                            \ don't need to do a SEC). This gives us a value of A
                            \ from -7 to +7 because these are magnitude-based
                            \ numbers with sign bits, rather than two's complement
                            \ numbers
    
     JSR ADDK               \ We now add A to S to give us a value in the range 1 to
                            \ 15, which we can pass to DIL2 to draw the vertical
                            \ bar on the indicator at this position (see the JSR ADD
                            \ above for more on this)
    
     JSR DIL2               \ Draw a vertical bar on the pitch indicator at offset A
                            \ and increment SC to point to the next indicator (the
                            \ four energy banks)
    

[X]

Subroutine [ADDK](addk.md) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Variable [ALP1](../workspace/zp.md#alp1) in workspace [ZP](../workspace/zp.md)

Magnitude of the roll angle alpha, i.e. |alpha|, which is a positive value between 0 and 31

[X]

Variable [ALP2](../workspace/zp.md#alp2) in workspace [ZP](../workspace/zp.md)

Bit 7 of ALP2 = sign of the roll angle in ALPHA

[X]

Variable [BET1](../workspace/zp.md#bet1) in workspace [ZP](../workspace/zp.md)

The magnitude of the pitch angle beta, i.e. |beta|, which is a positive value between 0 and 8

[X]

Variable [BETA](../workspace/zp.md#beta) in workspace [ZP](../workspace/zp.md)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Subroutine [DIL2](dil2.md) (category: Dashboard)

Update the roll or pitch indicator on the dashboard

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[DIALS (Part 1 of 4)](dials_part_1_of_4.md "Previous routine")[DIALS (Part 3 of 4)](dials_part_3_of_4.md "Next routine")
