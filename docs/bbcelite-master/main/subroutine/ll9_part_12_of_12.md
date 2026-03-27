---
title: "The LL9 (Part 12 of 12) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll9_part_12_of_12.html"
---

[LL145 (Part 4 of 4)](ll145_part_4_of_4.md "Previous routine")[LSPUT](lsput.md "Next routine")
    
    
           Name: LL9 (Part 12 of 12)                                     [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Draw all the visible edges from the ship line heap
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll9-part-12-of-12)
     Variations: See [code variations](../../related/compare/main/subroutine/ll9_part_12_of_12.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) calls via LSCLR
                 * [LL9 (Part 11 of 12)](ll9_part_11_of_12.md) calls via LSCLR
                 * [SHPPT](shppt.md) calls via LSCLR
                 * [LSPUT](lsput.md) calls via LSC3
    
    
    
    
    
    * * *
    
    
     This part draws any remaining lines from the old ship that are still in the
     ship line heap.
    
    
    
    * * *
    
    
     Other entry points:
    
       LSCLR                Draw any remaining lines from the old ship that are
                            still in the ship line heap
    
       LSC3                 Contains an RTS
    
    
    
    
    .LSCLR
    
     LDY LSNUM              \ Set Y to the offset in the line heap LSNUM
    
    .LSC1
    
     CPY LSNUM2             \ If Y >= LSNUM2, jump to LSC2 to return from the ship
     BCS LSC2               \ drawing routine, because the index in Y is greater
                            \ than the size of the existing ship line heap, which
                            \ means we have already erased all the old ship's lines
                            \ when drawing the new ship
    
                            \ If we get here then Y < LSNUM2, which means Y is
                            \ pointing to an on-screen line from the old ship that
                            \ we need to erase
    
     LDA (XX19),Y           \ Fetch the X1 line coordinate from the heap and store
     INY                    \ it in XX15, incrementing the heap pointer
     STA XX15
    
     LDA (XX19),Y           \ Fetch the Y1 line coordinate from the heap and store
     INY                    \ it in XX15+1, incrementing the heap pointer
     STA XX15+1
    
     LDA (XX19),Y           \ Fetch the X2 line coordinate from the heap and store
     INY                    \ it in XX15+2, incrementing the heap pointer
     STA XX15+2
    
     LDA (XX19),Y           \ Fetch the Y2 line coordinate from the heap and store
     INY                    \ it in XX15+3, incrementing the heap pointer
     STA XX15+3
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2) to erase it from
                            \ the screen
    
     JMP LSC1               \ Loop back to LSC1 to draw (i.e. erase) the next line
                            \ from the heap
    
    .LSC2
    
     LDA LSNUM              \ Store LSNUM in the first byte of the ship line heap
     LDY #0
     STA (XX19),Y
    
    .LSC3
    
     RTS                    \ Return from the subroutine
    

[X]

Subroutine [LOIN](loin.md) (category: Drawing lines)

Draw a one-segment line

[X]

Label [LSC1](ll9_part_12_of_12.md#lsc1) is local to this routine

[X]

Label [LSC2](ll9_part_12_of_12.md#lsc2) is local to this routine

[X]

Variable [LSNUM](../workspace/zp.md#lsnum) in workspace [ZP](../workspace/zp.md)

The pointer to the current position in the ship line heap as we work our way through the new ship's edges (and the corresponding old ship's edges) when drawing the ship in the main ship-drawing routine at LL9

[X]

Variable [LSNUM2](../workspace/zp.md#lsnum2) in workspace [ZP](../workspace/zp.md)

The size of the existing ship line heap for the ship we are drawing in LL9, i.e. the number of lines in the old ship that is currently shown on-screen and which we need to erase

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Variable [XX19](../workspace/zp.md#xx19) in workspace [ZP](../workspace/zp.md)

XX19(1 0) shares its location with INWK(34 33), which contains the address of the ship line heap

[LL145 (Part 4 of 4)](ll145_part_4_of_4.md "Previous routine")[LSPUT](lsput.md "Next routine")
