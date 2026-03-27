---
title: "The LL145 (Part 4 of 4) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll145_part_4_of_4.html"
---

[LL145 (Part 3 of 4)](ll145_part_3_of_4.md "Previous routine")[LL9 (Part 12 of 12)](ll9_part_12_of_12.md "Next routine")
    
    
           Name: LL145 (Part 4 of 4)                                     [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Clip line: Call the routine in LL188 to do the actual clipping
      Deep dive: [Line-clipping](https://elite.bbcelite.com/deep_dives/line-clipping.html)
                 [Extended screen coordinates](https://elite.bbcelite.com/deep_dives/extended_screen_coordinates.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll145-part-4-of-4)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part sets things up to call the routine in LL188, which does the actual
     clipping.
    
     If we get here, then R has been set to the gradient of the line (x1, y1) to
     (x2, y2), with T indicating the gradient of slope:
    
       * 0   = shallow slope (more horizontal than vertical)
    
       * &FF = steep slope (more vertical than horizontal)
    
     and XX13 has been set as follows:
    
       * 0   = (x1, y1) off-screen, (x2, y2) on-screen
    
       * 95  = (x1, y1) on-screen,  (x2, y2) off-screen
    
       * 191 = (x1, y1) off-screen, (x2, y2) off-screen
    
    
    
    
    .LL116
    
     LDA R                  \ Store the gradient in XX12+2
     STA XX12+2
    
     LDA S                  \ Store the type of slope in XX12+3, bit 7 clear means
     STA XX12+3             \ top left to bottom right, bit 7 set means top right to
                            \ bottom left
    
     LDA XX13               \ If XX13 = 0, skip the following instruction
     BEQ LL138
    
     BPL LLX117             \ If XX13 is positive, it must be 95. This means
                            \ (x1, y1) is on-screen but (x2, y2) isn't, so we jump
                            \ to LLX117 to swap the (x1, y1) and (x2, y2)
                            \ coordinates around before doing the actual clipping,
                            \ because we need to clip (x2, y2) but the clipping
                            \ routine at LL118 only clips (x1, y1)
    
    .LL138
    
                            \ If we get here, XX13 = 0 or 191, so (x1, y1) is
                            \ off-screen and needs clipping
    
     JSR LL118              \ Call LL118 to move (x1, y1) along the line onto the
                            \ screen, i.e. clip the line at the (x1, y1) end
    
     LDA XX13               \ If XX13 = 0, i.e. (x2, y2) is on-screen, jump down to
     BPL LL124              \ LL124 to return with a successfully clipped line
    
    .LL117
    
                            \ If we get here, XX13 = 191 (both coordinates are
                            \ off-screen)
    
     LDA XX15+1             \ If either of x1_hi or y1_hi are non-zero, jump to
     ORA XX15+3             \ LL137 to return from the subroutine with the C flag
     BNE LL137              \ set, as the line doesn't fit on-screen
    
     LDA XX15+2             \ If y1_lo > y-coordinate of the bottom of the screen
     CMP #Y*2               \ jump to LL137 to return from the subroutine with the
     BCS LL137              \ C flag set, as the line doesn't fit on-screen
    
    .LLX117
    
                            \ If we get here, XX13 = 95 or 191, and in both cases
                            \ (x2, y2) is off-screen, so we now need to swap the
                            \ (x1, y1) and (x2, y2) coordinates around before doing
                            \ the actual clipping, because we need to clip (x2, y2)
                            \ but the clipping routine at LL118 only clips (x1, y1)
    
     LDX XX15               \ Swap x1_lo = x2_lo
     LDA XX15+4
     STA XX15
     STX XX15+4
    
     LDA XX15+5             \ Swap x2_lo = x1_lo
     LDX XX15+1
     STX XX15+5
     STA XX15+1
    
     LDX XX15+2             \ Swap y1_lo = y2_lo
     LDA XX12
     STA XX15+2
     STX XX12
    
     LDA XX12+1             \ Swap y2_lo = y1_lo
     LDX XX15+3
     STX XX12+1
     STA XX15+3
    
     JSR LL118              \ Call LL118 to move (x1, y1) along the line onto the
                            \ screen, i.e. clip the line at the (x1, y1) end
    
     DEC SWAP               \ Set SWAP = &FF to indicate that we just clipped the
                            \ line at the (x2, y2) end by swapping the coordinates
                            \ (the DEC does this as we set SWAP to 0 at the start of
                            \ this subroutine)
    
    .LL124
    
     PLA                    \ Restore Y from the stack so it gets preserved through
     TAY                    \ the call to this subroutine
    
     JMP LL146              \ Jump up to LL146 to move the low bytes of (x1, y1) and
                            \ (x2, y2) into (X1, Y1) and (X2, Y2), and return from
                            \ the subroutine with a successfully clipped line
    
    .LL137
    
     PLA                    \ Restore Y from the stack so it gets preserved through
     TAY                    \ the call to this subroutine
    
     SEC                    \ Set the C flag to indicate the clipped line does not
                            \ fit on-screen
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [LL118](ll118.md) (category: Drawing lines)

Move a point along a line until it is on-screen

[X]

Label [LL124](ll145_part_4_of_4.md#ll124) is local to this routine

[X]

Label [LL137](ll145_part_4_of_4.md#ll137) is local to this routine

[X]

Label [LL138](ll145_part_4_of_4.md#ll138) is local to this routine

[X]

Label [LL146](ll145_part_1_of_4.md#ll146) in subroutine [LL145 (Part 1 of 4)](ll145_part_1_of_4.md)

[X]

Label [LLX117](ll145_part_4_of_4.md#llx117) is local to this routine

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [SWAP](../workspace/wp.md#swap) in workspace [WP](../workspace/wp.md)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[X]

Variable [XX12](../workspace/zp.md#xx12) in workspace [ZP](../workspace/zp.md)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX13](../workspace/zp.md#xx13) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used in the line-drawing routines

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[LL145 (Part 3 of 4)](ll145_part_3_of_4.md "Previous routine")[LL9 (Part 12 of 12)](ll9_part_12_of_12.md "Next routine")
