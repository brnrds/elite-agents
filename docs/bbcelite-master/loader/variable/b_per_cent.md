---
title: "The B% (Loader) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/variable/b_per_cent.html"
---

[ZP (Loader)](../workspace/zp.md "Previous routine")[Elite loader (Loader)](../subroutine/elite_loader.md "Next routine")
    
    
           Name: B%                                                      [Show more]
           Type: Variable
       Category: Drawing the screen
        Summary: VDU commands for setting the square mode 1 screen
      Deep dive: [The split-screen mode in BBC Micro Elite](https://elite.bbcelite.com/deep_dives/the_split-screen_mode.html)
                 [Drawing monochrome pixels on the BBC Micro](https://elite.bbcelite.com/deep_dives/drawing_monochrome_pixels_in_mode_4.html)
    
    
        Context: See this variable [in context in the source code](../../all/loader.md#header-b-per-cent)
     Variations: See [code variations](../../related/compare/loader/variable/b_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [Elite loader](../subroutine/elite_loader.md) uses B%
    
    
    
    
    
    * * *
    
    
     This block contains the bytes that get written by OSWRCH to set up the screen
     mode (this is equivalent to using the VDU statement in BASIC).
    
     It defines the whole screen using a square, monochrome mode 1 configuration;
     the mode 2 part for the dashboard is implemented in the IRQ1 routine.
    
     The top part of Elite's screen mode is based on mode 1 but with the following
     differences:
    
       * 64 columns, 31 rows (256 x 248 pixels) rather than 80, 32
    
       * The horizontal sync position is at character 90 rather than 98, which
         pushes the screen to the right (which centres it as it's not as wide as
         the normal screen modes)
    
       * Screen memory goes from &4000 to &7EFF
    
       * In the Master version of Elite, the screen mode is actually based on mode
         129 rather than mode 1, so shadow RAM (known as LYNNE) is used to store
         the screen memory, though in all other respects the screen mode is the
         same as if it were based on mode 1
    
       * The text window is 1 row high and 13 columns wide, and is at (2, 16)
    
       * The cursor is disabled
    
     This almost-square mode 1 variant makes life a lot easier when drawing to the
     screen, as there are 256 pixels on each row (or, to put it in screen memory
     terms, there are two pages of memory per row of pixels).
    
     There is also an interrupt-driven routine that switches the bytes-per-pixel
     setting from that of mode 1 to that of mode 2, when the raster reaches the
     split between the space view and the dashboard.
    
    
    
    
    .B%
    
     EQUB 22, 129           \ Switch to screen mode 129
    
     EQUB 28                \ Define a text window as follows:
     EQUB 2, 17, 15, 16     \
                            \   * Left = 2
                            \   * Right = 15
                            \   * Top = 16
                            \   * Bottom = 17
                            \
                            \ i.e. 1 row high, 13 columns wide at (2, 16)
    
     EQUB 23, 0, 6, 31      \ Set 6845 register R6 = 31
     EQUB 0, 0, 0           \
     EQUB 0, 0, 0           \ This is the "vertical displayed" register, and sets
                            \ the number of displayed character rows to 31. For
                            \ comparison, this value is 32 for standard modes 1 and
                            \ 2, but we claw back the last row for storing code just
                            \ above the end of screen memory
    
     EQUB 23, 0, 12, &08    \ Set 6845 register R12 = &08 and R13 = &00
     EQUB 0, 0, 0           \
     EQUB 0, 0, 0           \ This sets 6845 registers (R12 R13) = &0800 to point
     EQUB 23, 0, 13, &00    \ to the start of screen memory in terms of character
     EQUB 0, 0, 0           \ rows. There are 8 pixel lines in each character row,
     EQUB 0, 0, 0           \ so to get the actual address of the start of screen
                            \ memory, we multiply by 8:
                            \
                            \   &0800 * 8 = &4000
                            \
                            \ So this sets the start of screen memory to &4000
    
     EQUB 23, 0, 1, 64      \ Set 6845 register R1 = 64
     EQUB 0, 0, 0           \
     EQUB 0, 0, 0           \ This is the "horizontal displayed" register, which
                            \ defines the number of character blocks per horizontal
                            \ character row. For comparison, this value is 80 for
                            \ modes 1 and 2, but our custom screen is not as wide at
                            \ only 64 character blocks across
    
     EQUB 23, 0, 2, 90      \ Set 6845 register R2 = 90
     EQUB 0, 0, 0           \
     EQUB 0, 0, 0           \ This is the "horizontal sync position" register, which
                            \ defines the position of the horizontal sync pulse on
                            \ the horizontal line in terms of character widths from
                            \ the left-hand side of the screen. For comparison this
                            \ is 98 for modes 1 and 2, but needs to be adjusted for
                            \ our custom screen's width
    
     EQUB 23, 0, 10, 32     \ Set 6845 register R10 = %00100000 = 32
     EQUB 0, 0, 0           \
     EQUB 0, 0, 0           \ This is the "cursor start" register, and bits 5 and 6
                            \ define the "cursor display mode", as follows:
                            \
                            \   * %00 = steady, non-blinking cursor
                            \
                            \   * %01 = do not display a cursor
                            \
                            \   * %10 = fast blinking cursor (blink at 1/16 of the
                            \           field rate)
                            \
                            \   * %11 = slow blinking cursor (blink at 1/32 of the
                            \           field rate)
                            \
                            \ We can therefore turn off the cursor completely by
                            \ setting cursor display mode %01, with bit 6 of R10
                            \ clear and bit 5 of R10 set
    

[ZP (Loader)](../workspace/zp.md "Previous routine")[Elite loader (Loader)](../subroutine/elite_loader.md "Next routine")
