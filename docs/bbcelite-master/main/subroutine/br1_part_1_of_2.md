---
title: "The BR1 (Part 1 of 2) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/br1_part_1_of_2.html"
---

[DEATH2](death2.md "Previous routine")[BR1 (Part 2 of 2)](br1_part_2_of_2.md "Next routine")
    
    
           Name: BR1 (Part 1 of 2)                                       [Show more]
           Type: Subroutine
       Category: Start and end
        Summary: Show the "Load New Commander (Y/N)?" screen and start the game
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-br1-part-1-of-2)
     Variations: See [code variations](../../related/compare/main/subroutine/br1_part_1_of_2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT102](tt102.md) calls via QU5
    
    
    
    
    
    * * *
    
    
     BRKV is set to point to BR1 by the loading process.
    
    
    
    * * *
    
    
     Other entry points:
    
       QU5                  Restart the game using the last saved commander without
                            asking whether to load a new commander file
    
    
    
    
    .BR1
    
     JSR ZEKTRAN            \ Call ZEKTRAN to clear the key logger
    
     LDA #3                 \ Set XC = 3 (set text cursor to column 3)
     STA XC
    
    \JSR startat            \ This instruction is commented out in the original
                            \ source
    
     LDX #CYL               \ Call TITLE to show a rotating Cobra Mk III (#CYL) and
     LDA #6                 \ token 6 ("LOAD NEW {single cap}COMMANDER {all caps}
     LDY #200               \ (Y/N)?{sentence case}{cr}{cr}"), with the ship at a
     JSR TITLE              \ distance of 200, returning with the internal number
                            \ of the key pressed in A
    
     CPX #'Y'               \ Did we press "Y"? If not, jump to QU5, otherwise
     BNE QU5                \ continue on to load a new commander
    
    \JSR stopat             \ This instruction is commented out in the original
                            \ source
    
     JSR DFAULT             \ Call DFAULT to reset the current commander data block
                            \ to the last saved commander
    
     JSR SVE                \ Call SVE to load a new commander into the last saved
                            \ commander data block
    
    \JSR startat            \ This instruction is commented out in the original
                            \ source
    
    .QU5
    
     JSR DFAULT             \ Call DFAULT to reset the current commander data block
                            \ to the last saved commander
    

[X]

Configuration variable [CYL](../../all/workspaces.md#cyl) = 11

Ship type for a Cobra Mk III

[X]

Subroutine [DFAULT](dfault.md) (category: Start and end)

Reset the current commander data block to the last saved commander

[X]

Entry point [QU5](br1_part_1_of_2.md#qu5) in subroutine [BR1 (Part 1 of 2)](br1_part_1_of_2.md) (category: Start and end)

Restart the game using the last saved commander without asking whether to load a new commander file

[X]

Subroutine [SVE](sve.md) (category: Save and load)

Display the disk access menu and process saving of commander files

[X]

Subroutine [TITLE](title.md) (category: Start and end)

Display a title screen with a rotating ship and prompt

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Subroutine [ZEKTRAN](zektran.md) (category: Keyboard)

Clear the key logger

[DEATH2](death2.md "Previous routine")[BR1 (Part 2 of 2)](br1_part_2_of_2.md "Next routine")
