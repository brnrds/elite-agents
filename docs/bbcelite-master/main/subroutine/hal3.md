---
title: "The HAL3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/hal3.html"
---

[HAS2](has2.md "Previous routine")[HAS3](has3.md "Next routine")
    
    
           Name: HAL3                                                    [Show more]
           Type: Subroutine
       Category: Ship hangar
        Summary: Draw a hangar background line from left to right, stopping when it
                 bumps into existing on-screen content
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-hal3)
     References: This subroutine is called as follows:
                 * [HANGER](hanger.md) calls HAL3
    
    
    
    
    
    
    .HAL3
    
     TAX                    \ Store A in X so we can retrieve it after the following
                            \ check and again after updating screen memory
    
     AND (SC),Y             \ If the pixel we want to draw is non-zero (using A as a
     BNE HA3                \ mask), then this means it already contains something,
                            \ so we stop drawing because we have run into something
                            \ that's already on-screen, and return from the
                            \ subroutine (as HA3 contains an RTS)
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     AND #RED               \ Apply the pixel mask in A to a four-pixel block of
                            \ red pixels, so we now know which bits to set in screen
                            \ memory
    
     ORA (SC),Y             \ OR the byte with the current contents of screen
                            \ memory, so the pixel we want is set to red (because
                            \ we know the bits are already 0 from the above test)
    
     STA (SC),Y             \ Store the updated pixel in screen memory
    
     TXA                    \ Retrieve the value of A we stored above, so A now
                            \ contains the pixel mask again
    
     LSR A                  \ Shift A to the right to move on to the next pixel
    
     BCC HAL3               \ If bit 0 before the shift was clear (i.e. we didn't
                            \ just do the fourth pixel in this block), loop back to
                            \ HAL3 to check and draw the next pixel
    
     TYA                    \ Set Y = Y + 8 (as we know the C flag is set) to point
     ADC #7                 \ to the next character block along
     TAY
    
     LDA #%10001000         \ Reset the pixel mask in A to the first pixel in the
                            \ new four-pixel character block
    
     BCC HAL3               \ If the above addition didn't overflow, jump back to
                            \ HAL3 to keep drawing the line in the next character
                            \ block
    
     RTS                    \ The addition overflowed, so we have reached the last
                            \ character block in this page of memory, which is the
                            \ end of the line, so we return from the subroutine
    

[X]

Entry point [HA3](hanger.md#ha3) in subroutine [HANGER](hanger.md) (category: Ship hangar)

Contains an RTS

[X]

Subroutine [HAL3](hal3.md) (category: Ship hangar)

Draw a hangar background line from left to right, stopping when it bumps into existing on-screen content

[X]

Configuration variable [RED](../../all/workspaces.md#red) = %11110000

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[HAS2](has2.md "Previous routine")[HAS3](has3.md "Next routine")
