---
title: "The STARS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/stars.html"
---

[FLIP](flip.md "Previous routine")[STARS1](stars1.md "Next routine")
    
    
           Name: STARS                                                   [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: The main routine for processing the stardust
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-stars)
     Variations: See [code variations](../../related/compare/main/subroutine/stars.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 16 of 16)](main_flight_loop_part_16_of_16.md) calls STARS
    
    
    
    
    
    * * *
    
    
     Called at the very end of the main flight loop.
    
    
    
    
    .STARS
    
     LDA #DUST              \ Switch to stripe 3-2-3-2, which is cyan/red in the
     STA COL                \ space view
    
     LDX VIEW               \ Load the current view into X:
                            \
                            \   0 = front
                            \   1 = rear
                            \   2 = left
                            \   3 = right
    
     BEQ STARS1             \ If this is view 0, jump to STARS1 to process the
                            \ stardust for the front view
    
     DEX                    \ If this is view 2 or 3, jump to STARS2 (via ST11) to
     BNE ST11               \ process the stardust for the left or right views
    
     JMP STARS6             \ Otherwise this is the rear view, so jump to STARS6 to
                            \ process the stardust for the rear view
    
    .ST11
    
     JMP STARS2             \ Jump to STARS2 for the left or right views, as it's
                            \ too far for the branch instruction above
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [DUST](../../all/workspaces.md#dust) = WHITE

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[X]

Label [ST11](stars.md#st11) is local to this routine

[X]

Subroutine [STARS1](stars1.md) (category: Stardust)

Process the stardust for the front view

[X]

Subroutine [STARS2](stars2.md) (category: Stardust)

Process the stardust for the left or right view

[X]

Subroutine [STARS6](stars6.md) (category: Stardust)

Process the stardust for the rear view

[X]

Variable [VIEW](../workspace/wp.md#view) in workspace [WP](../workspace/wp.md)

The number of the current space view

[FLIP](flip.md "Previous routine")[STARS1](stars1.md "Next routine")
