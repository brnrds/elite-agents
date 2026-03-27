---
title: "The HAS3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/has3.html"
---

[HAL3](hal3.md "Previous routine")[DVID4K](dvid4k.md "Next routine")
    
    
           Name: HAS3                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw a hangar background line from right to left
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-has3)
     Variations: See [code variations](../../related/compare/main/subroutine/has3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HANGER](hanger.md) calls HAS3
    
    
    
    
    
    * * *
    
    
     This routine draws a line to the left, starting with the pixel mask in A at
     screen address SC(1 0) and character block offset Y, and aborting if we bump
     into something that's already on-screen.
    
    
    
    
    .HAS3
    
     TAX                    \ Store A in X so we can retrieve it after the following
                            \ check and again after updating screen memory
    
     AND (SC),Y             \ If the pixel we want to draw is non-zero (using A as a
     BNE HA3                \ mask), then this means it already contains something,
                            \ so we stop drawing because we have run into something
                            \ that's already on-screen, and return from the
                            \ subroutine (as HA3 contains an RTS)
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     ORA (SC),Y             \ OR the byte with the current contents of screen
                            \ memory, so the pixel we want is set to red (because
                            \ we know the bits are already 0 from the above test)
    
     STA (SC),Y             \ Store the updated pixel in screen memory
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     ASL A                  \ Shift A to the left to move to the next pixel to the
                            \ left
    
     BCC HAS3               \ If bit 7 before the shift was clear (i.e. we didn't
                            \ just do the first pixel in this block), loop back to
                            \ HAS3 to check and draw the next pixel to the left
    
     TYA                    \ Set Y = Y - 8 (as we know the C flag is set) to point
     SBC #8                 \ to the next character block to the left
     TAY
    
     LDA #%00010000         \ Set a mask in A to the last pixel in the four-pixel
                            \ byte
    
     BCS HAS3               \ If the above subtraction didn't underflow, jump back
                            \ to HAS3 to keep drawing the line in the next character
                            \ block to the left
    
     RTS                    \ Return from the subroutine
    

[X]

Entry point [HA3](hanger.md#ha3) in subroutine [HANGER](hanger.md) (category: Ship hangar)

Contains an RTS

[X]

Subroutine [HAS3](has3.md) (category: Ship hangar)

Draw a hangar background line from right to left

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[HAL3](hal3.md "Previous routine")[DVID4K](dvid4k.md "Next routine")
