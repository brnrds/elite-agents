---
title: "The LL145 (Part 3 of 4) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll145_part_3_of_4.html"
---

[LL145 (Part 2 of 4)](ll145_part_2_of_4.md "Previous routine")[LL145 (Part 4 of 4)](ll145_part_4_of_4.md "Next routine")
    
    
           Name: LL145 (Part 3 of 4)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Clip line: Calculate the line's gradient
      Deep dive: [Line-clipping](https://elite.bbcelite.com/deep_dives/line-clipping.html)
                 [Extended screen coordinates](https://elite.bbcelite.com/deep_dives/extended_screen_coordinates.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll145-part-3-of-4)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .LL115
    
     TYA                    \ Store Y on the stack so we can preserve it through the
     PHA                    \ call to this subroutine
    
     LDA XX15+4             \ Set XX12+2 = x2_lo - x1_lo
     SEC
     SBC XX15
     STA XX12+2
    
     LDA XX15+5             \ Set XX12+3 = x2_hi - x1_hi
     SBC XX15+1
     STA XX12+3
    
     LDA XX12               \ Set XX12+4 = y2_lo - y1_lo
     SEC
     SBC XX15+2
     STA XX12+4
    
     LDA XX12+1             \ Set XX12+5 = y2_hi - y1_hi
     SBC XX15+3
     STA XX12+5
    
                            \ So we now have:
                            \
                            \   delta_x in XX12(3 2)
                            \   delta_y in XX12(5 4)
                            \
                            \ where the delta is (x1, y1) - (x2, y2))
    
     EOR XX12+3             \ Set S = the sign of delta_x * the sign of delta_y, so
     STA S                  \ if bit 7 of S is set, the deltas have different signs
    
     LDA XX12+5             \ If delta_y_hi is positive, jump down to LL110 to skip
     BPL LL110              \ the following
    
     LDA #0                 \ Otherwise flip the sign of delta_y to make it
     SEC                    \ positive, starting with the low bytes
     SBC XX12+4
     STA XX12+4
    
     LDA #0                 \ And then doing the high bytes, so now:
     SBC XX12+5             \
     STA XX12+5             \   XX12(5 4) = |delta_y|
    
    .LL110
    
     LDA XX12+3             \ If delta_x_hi is positive, jump down to LL111 to skip
     BPL LL111              \ the following
    
     SEC                    \ Otherwise flip the sign of delta_x to make it
     LDA #0                 \ positive, starting with the low bytes
     SBC XX12+2
     STA XX12+2
    
     LDA #0                 \ And then doing the high bytes, so now:
     SBC XX12+3             \
                            \   (A XX12+2) = |delta_x|
    
    .LL111
    
                            \ We now keep halving |delta_x| and |delta_y| until
                            \ both of them have zero in their high bytes
    
     TAX                    \ If |delta_x_hi| is non-zero, skip the following
     BNE LL112
    
     LDX XX12+5             \ If |delta_y_hi| = 0, jump down to LL113 (as both
     BEQ LL113              \ |delta_x_hi| and |delta_y_hi| are 0)
    
    .LL112
    
     LSR A                  \ Halve the value of delta_x in (A XX12+2)
     ROR XX12+2
    
     LSR XX12+5             \ Halve the value of delta_y XX12(5 4)
     ROR XX12+4
    
     JMP LL111              \ Loop back to LL111
    
    .LL113
    
                            \ By now, the high bytes of both |delta_x| and |delta_y|
                            \ are zero
    
     STX T                  \ We know that X = 0 as that's what we tested with a BEQ
                            \ above, so this sets T = 0
    
     LDA XX12+2             \ If delta_x_lo < delta_y_lo, so our line is more
     CMP XX12+4             \ vertical than horizontal, jump to LL114
     BCC LL114
    
                            \ If we get here then our line is more horizontal than
                            \ vertical, so it is a shallow slope
    
     STA Q                  \ Set Q = delta_x_lo
    
     LDA XX12+4             \ Set A = delta_y_lo
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
                            \     = 256 * delta_y_lo / delta_x_lo
    
     JMP LL116              \ Jump to LL116, as we now have the line's gradient in R
    
    .LL114
    
                            \ If we get here then our line is more vertical than
                            \ horizontal, so it is a steep slope
    
     LDA XX12+4             \ Set Q = delta_y_lo
     STA Q
     LDA XX12+2             \ Set A = delta_x_lo
    
     JSR LL28               \ Call LL28 to calculate:
                            \
                            \   R = 256 * A / Q
                            \     = 256 * delta_x_lo / delta_y_lo
    
     DEC T                  \ T was set to 0 above, so this sets T = &FF when our
                            \ line is steep
    

[X]

Label [LL110](ll145_part_3_of_4.md#ll110) is local to this routine

[X]

Label [LL111](ll145_part_3_of_4.md#ll111) is local to this routine

[X]

Label [LL112](ll145_part_3_of_4.md#ll112) is local to this routine

[X]

Label [LL113](ll145_part_3_of_4.md#ll113) is local to this routine

[X]

Label [LL114](ll145_part_3_of_4.md#ll114) is local to this routine

[X]

Label [LL116](ll145_part_4_of_4.md#ll116) in subroutine [LL145 (Part 4 of 4)](ll145_part_4_of_4.md)

[X]

Subroutine [LL28](ll28.md) (category: Maths (Arithmetic))

Calculate R = 256 * A / Q

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX12](../workspace/zp.md#xx12) in workspace [ZP](../workspace/zp.md)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[LL145 (Part 2 of 4)](ll145_part_2_of_4.md "Previous routine")[LL145 (Part 4 of 4)](ll145_part_4_of_4.md "Next routine")
