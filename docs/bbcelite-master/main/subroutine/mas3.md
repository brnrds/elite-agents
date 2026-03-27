---
title: "The MAS3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mas3.html"
---

[MAS2](mas2.md "Previous routine")[STATUS](status.md "Next routine")
    
    
           Name: MAS3                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate A = x_hi^2 + y_hi^2 + z_hi^2 in the K% block
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-mas3)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 15 of 16)](main_flight_loop_part_15_of_16.md) calls MAS3
    
    
    
    
    
    * * *
    
    
     Given a value in Y that points to the start of a ship data block as an offset
     from K%, calculate the following:
    
       (A ?) = x_hi^2 + y_hi^2 + z_hi^2
    
     returning A = &FF if the calculation overflows a one-byte result. The K%
     workspace contains the ship data blocks, so the offset in Y must be 0 or a
     multiple of NI% (as each block in K% contains NI% bytes).
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The offset from K% for the start of the ship data block
                            to use
    
     Returns
    
       A                    The high byte of x_hi^2 + y_hi^2 + z_hi^2
    
       C flag               The overflow status (i.e. did the result fit into one
                            byte):
    
                              * Clear if the calculation didn't overflow
    
                              * Set if the calculation overflowed (in which case A
                                is set to &FF)
    
    
    
    
    .MAS3
    
     LDA K%+1,Y             \ Set (A P) = x_hi * x_hi
     JSR SQUA2
    
     STA R                  \ Store A (high byte of result) in R
    
     LDA K%+4,Y             \ Set (A P) = y_hi * y_hi
     JSR SQUA2
    
     ADC R                  \ Add A (high byte of second result) to R
    
     BCS MA30               \ If the addition of the two high bytes caused a carry
                            \ (i.e. they overflowed), jump to MA30 to return A = &FF
    
     STA R                  \ Store A (sum of the two high bytes) in R
    
     LDA K%+7,Y             \ Set (A P) = z_hi * z_hi
     JSR SQUA2
    
     ADC R                  \ Add A (high byte of third result) to R, so R now
                            \ contains the high byte of the entire sum, i.e. of
                            \ x_hi^2 + y_hi^2 + z_hi^2
    
     BCC P%+4               \ If there is no carry, skip the following instruction
                            \ to return straight from the subroutine
    
    .MA30
    
     LDA #&FF               \ The calculation has overflowed, so set A = &FF
    
     RTS                    \ Return from the subroutine
    

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Label [MA30](mas3.md#ma30) is local to this routine

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [SQUA2](squa2.md) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[MAS2](mas2.md "Previous routine")[STATUS](status.md "Next routine")
