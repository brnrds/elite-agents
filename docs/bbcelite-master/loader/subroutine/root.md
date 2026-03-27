---
title: "The ROOT (Loader) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/subroutine/root.html"
---

[CNT3 (Loader)](../variable/cnt3.md "Previous routine")[OSB (Loader)](osb.md "Next routine")
    
    
           Name: ROOT                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate ZP = SQRT(ZP(1 0))
    
    
        Context: See this subroutine [in context in the source code](../../all/loader.md#header-root)
     References: This subroutine is called as follows:
                 * [PLL1 (Part 1 of 3)](pll1_part_1_of_3.md) calls ROOT
    
    
    
    
    
    * * *
    
    
     Calculate the following square root:
    
       ZP = SQRT(ZP(1 0))
    
     This routine is identical to LL5 in the main game code - it even has the same
     label names. The only difference is that LL5 calculates Q = SQRT(R Q), but
     apart from the variables used, the instructions are identical, so see the LL5
     routine in the main game code for more details on the algorithm used here.
    
    
    
    
    .ROOT
    
     LDY ZP+1               \ Set (Y Q) = ZP(1 0)
     LDA ZP
     STA Q
    
                            \ So now to calculate ZP = SQRT(Y Q)
    
     LDX #0                 \ Set X = 0, to hold the remainder
    
     STX ZP                 \ Set ZP = 0, to hold the result
    
     LDA #8                 \ Set P = 8, to use as a loop counter
     STA P
    
    .LL6
    
     CPX ZP                 \ If X < ZP, jump to LL7
     BCC LL7
    
     BNE LL8                \ If X > ZP, jump to LL8
    
     CPY #64                \ If Y < 64, jump to LL7 with the C flag clear,
     BCC LL7                \ otherwise fall through into LL8 with the C flag set
    
    .LL8
    
     TYA                    \ Set Y = Y - 64
     SBC #64                \
     TAY                    \ This subtraction will work as we know C is set from
                            \ the BCC above, and the result will not underflow as we
                            \ already checked that Y >= 64, so the C flag is also
                            \ set for the next subtraction
    
     TXA                    \ Set X = X - ZP
     SBC ZP
     TAX
    
    .LL7
    
     ROL ZP                 \ Shift the result in Q to the left, shifting the C flag
                            \ into bit 0 and bit 7 into the C flag
    
     ASL Q                  \ Shift the dividend in (Y S) to the left, inserting
     TYA                    \ bit 7 from above into bit 0
     ROL A
     TAY
    
     TXA                    \ Shift the remainder in X to the left
     ROL A
     TAX
    
     ASL Q                  \ Shift the dividend in (Y S) to the left
     TYA
     ROL A
     TAY
    
     TXA                    \ Shift the remainder in X to the left
     ROL A
     TAX
    
     DEC P                  \ Decrement the loop counter
    
     BNE LL6                \ Loop back to LL6 until we have done 8 loops
    
     RTS                    \ Return from the subroutine
    

[X]

Label [LL6](root.md#ll6) is local to this routine

[X]

Label [LL7](root.md#ll7) is local to this routine

[X]

Label [LL8](root.md#ll8) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Workspace [ZP](../workspace/zp.md) (category: Workspaces)

Important variables used by the loader

[CNT3 (Loader)](../variable/cnt3.md "Previous routine")[OSB (Loader)](osb.md "Next routine")
