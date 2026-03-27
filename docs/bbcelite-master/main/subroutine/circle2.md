---
title: "The CIRCLE2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/circle2.html"
---

[CIRCLE](circle.md "Previous routine")[WPLS2](wpls2.md "Next routine")
    
    
           Name: CIRCLE2                                                 [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw a circle (for the planet or chart)
      Deep dive: [Drawing circles](https://elite.bbcelite.com/deep_dives/drawing_circles.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-circle2)
     Variations: See [code variations](../../related/compare/main/subroutine/circle2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HFS2](hfs2.md) calls CIRCLE2
                 * [TT128](tt128.md) calls CIRCLE2
    
    
    
    
    
    * * *
    
    
     Draw a circle with the centre at (K3, K4) and radius K. Used to draw the
     planet and the chart circles.
    
    
    
    * * *
    
    
     Arguments:
    
       STP                  The step size for the circle
    
       K                    The circle's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the circle
    
       K4(1 0)              Pixel y-coordinate of the centre of the circle
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .CIRCLE2
    
     LDX #&FF               \ Set FLAG = &FF to reset the ball line heap in the call
     STX FLAG               \ to the BLINE routine below
    
     INX                    \ Set CNT = 0, our counter that goes up to 64, counting
     STX CNT                \ segments in our circle
    
    .PLL3
    
     LDA CNT                \ Set A = CNT
    
     JSR FMLTU2             \ Call FMLTU2 to calculate:
                            \
                            \   A = K * sin(A)
                            \     = K * sin(CNT)
    
     LDX #0                 \ Set T = 0, so we have the following:
     STX T                  \
                            \   (T A) = K * sin(CNT)
                            \
                            \ which is the x-coordinate of the circle for this count
    
     LDX CNT                \ If CNT < 33 then jump to PL37, as this is the right
     CPX #33                \ half of the circle and the sign of the x-coordinate is
     BCC PL37               \ correct
    
     EOR #%11111111         \ This is the left half of the circle, so we want to
     ADC #0                 \ flip the sign of the x-coordinate in (T A) using two's
     TAX                    \ complement, so we start with the low byte and store it
                            \ in X (the ADC adds 1 as we know the C flag is set)
    
     LDA #&FF               \ And then we flip the high byte in T
     ADC #0
     STA T
    
     TXA                    \ Finally, we restore the low byte from X, so we have
                            \ now negated the x-coordinate in (T A)
    
     CLC                    \ Clear the C flag so we can do some more addition below
    
    .PL37
    
     ADC K3                 \ We now calculate the following:
     STA K6                 \
                            \   K6(1 0) = (T A) + K3(1 0)
                            \
                            \ to add the coordinates of the centre to our circle
                            \ point, starting with the low bytes
    
     LDA K3+1               \ And then doing the high bytes, so we now have:
     ADC T                  \
     STA K6+1               \   K6(1 0) = K * sin(CNT) + K3(1 0)
                            \
                            \ which is the result we want for the x-coordinate
    
     LDA CNT                \ Set A = CNT + 16
     CLC
     ADC #16
    
     JSR FMLTU2             \ Call FMLTU2 to calculate:
                            \
                            \   A = K * sin(A)
                            \     = K * sin(CNT + 16)
                            \     = K * cos(CNT)
    
     TAX                    \ Set X = A
                            \       = K * cos(CNT)
    
     LDA #0                 \ Set T = 0, so we have the following:
     STA T                  \
                            \   (T X) = K * cos(CNT)
                            \
                            \ which is the y-coordinate of the circle for this count
    
     LDA CNT                \ Set A = (CNT + 15) mod 64
     ADC #15
     AND #63
    
     CMP #33                \ If A < 33 (i.e. CNT is 0-16 or 48-64) then jump to
     BCC PL38               \ PL38, as this is the bottom half of the circle and the
                            \ sign of the y-coordinate is correct
    
     TXA                    \ This is the top half of the circle, so we want to
     EOR #%11111111         \ flip the sign of the y-coordinate in (T X) using two's
     ADC #0                 \ complement, so we start with the low byte in X (the
     TAX                    \ ADC adds 1 as we know the C flag is set)
    
     LDA #&FF               \ And then we flip the high byte in T, so we have
     ADC #0                 \ now negated the y-coordinate in (T X)
     STA T
    
     CLC                    \ Clear the C flag so the addition at the start of BLINE
                            \ will work
    
    .PL38
    
     JSR BLINE              \ Call BLINE to draw this segment, which also increases
                            \ CNT by STP, the step size
    
     CMP #65                \ If CNT >= 65 then skip the next instruction
     BCS P%+5
    
     JMP PLL3               \ Jump back for the next segment
    
     CLC                    \ Clear the C flag to indicate success
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [BLINE](bline.md) (category: Drawing circles)

Draw a circle segment and add it to the ball line heap

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [FLAG](../workspace/zp.md#flag) in workspace [ZP](../workspace/zp.md)

A flag that's used to define whether this is the first call to the ball line routine in BLINE, so it knows whether to wait for the second call before storing segment data in the ball line heap

[X]

Subroutine [FMLTU2](fmltu2.md) (category: Maths (Arithmetic))

Calculate A = K * sin(A)

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [K6](../workspace/zp.md#k6) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing coordinates during vector calculations

[X]

Label [PL37](circle2.md#pl37) is local to this routine

[X]

Label [PL38](circle2.md#pl38) is local to this routine

[X]

Label [PLL3](circle2.md#pll3) is local to this routine

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[CIRCLE](circle.md "Previous routine")[WPLS2](wpls2.md "Next routine")
