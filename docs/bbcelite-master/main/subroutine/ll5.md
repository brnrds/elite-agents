---
title: "The LL5 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll5.html"
---

[SHPPT](shppt.md "Previous routine")[LL28](ll28.md "Next routine")
    
    
           Name: LL5                                                     [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate Q = SQRT(R Q)
      Deep dive: [Calculating square roots](https://elite.bbcelite.com/deep_dives/calculating_square_roots.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll5)
     Variations: See [code variations](../../related/compare/main/subroutine/ll5.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HAS1](has1.md) calls LL5
                 * [Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md) calls LL5
                 * [NORM](norm.md) calls LL5
                 * [SUN (Part 3 of 4)](sun_part_3_of_4.md) calls LL5
                 * [TT111](tt111.md) calls LL5
    
    
    
    
    
    * * *
    
    
     Calculate the following square root:
    
       Q = SQRT(R Q)
    
    
    
    
    .LL5
    
     LDY R                  \ Set (Y S) = (R Q)
     LDA Q
     STA S
    
                            \ So now to calculate Q = SQRT(Y S)
    
     LDX #0                 \ Set X = 0, to hold the remainder
    
     STX Q                  \ Set Q = 0, to hold the result
    
     LDA #8                 \ Set T = 8, to use as a loop counter
     STA T
    
    .LL6
    
     CPX Q                  \ If X < Q, jump to LL7
     BCC LL7
    
     BNE P%+6               \ If X > Q, skip the next two instructions
    
     CPY #64                \ If Y < 64, jump to LL7 with the C flag clear,
     BCC LL7                \ otherwise fall through into LL8 with the C flag set
    
     TYA                    \ Set Y = Y - 64
     SBC #64                \
     TAY                    \ This subtraction will work as we know C is set from
                            \ the BCC above, and the result will not underflow as we
                            \ already checked that Y >= 64, so the C flag is also
                            \ set for the next subtraction
    
     TXA                    \ Set X = X - Q
     SBC Q
     TAX
    
    .LL7
    
     ROL Q                  \ Shift the result in Q to the left, shifting the C flag
                            \ into bit 0 and bit 7 into the C flag
    
     ASL S                  \ Shift the dividend in (Y S) to the left, inserting
     TYA                    \ bit 7 from above into bit 0
     ROL A
     TAY
    
     TXA                    \ Shift the remainder in X to the left
     ROL A
     TAX
    
     ASL S                  \ Shift the dividend in (Y S) to the left
     TYA
     ROL A
     TAY
    
     TXA                    \ Shift the remainder in X to the left
     ROL A
     TAX
    
     DEC T                  \ Decrement the loop counter
    
     BNE LL6                \ Loop back to LL6 until we have done 8 loops
    
     RTS                    \ Return from the subroutine
    

[X]

Label [LL6](ll5.md#ll6) is local to this routine

[X]

Label [LL7](ll5.md#ll7) is local to this routine

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[SHPPT](shppt.md "Previous routine")[LL28](ll28.md "Next routine")
