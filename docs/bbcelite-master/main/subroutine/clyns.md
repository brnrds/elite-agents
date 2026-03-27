---
title: "The CLYNS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/clyns.html"
---

[ZES2](zes2.md "Previous routine")[DIALS (Part 1 of 4)](dials_part_1_of_4.md "Next routine")
    
    
           Name: CLYNS                                                   [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the bottom three text rows of the space view
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-clyns)
     Variations: See [code variations](../../related/compare/main/subroutine/clyns.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [dockEd](docked.md) calls CLYNS
                 * [EQSHP](eqshp.md) calls CLYNS
                 * [hm](hm.md) calls CLYNS
                 * [JMTB](../variable/jmtb.md) calls CLYNS
                 * [me2](me2.md) calls CLYNS
                 * [MESS](mess.md) calls CLYNS
                 * [qv](qv.md) calls CLYNS
                 * [TT219](tt219.md) calls CLYNS
    
    
    
    
    
    
    .CLYNS
    
     STZ DLY                \ Set the delay in DLY to 0, to indicate that we are
                            \ no longer showing an in-flight message, so any new
                            \ in-flight messages will be shown instantly
    
     STZ de                 \ Clear de, the flag that appends " DESTROYED" to the
                            \ end of the next text token, so that it doesn't
    
     LDA #%11111111         \ Set DTW2 = %11111111 to denote that we are not
     STA DTW2               \ currently printing a word
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch standard tokens to
     STA QQ17               \ Sentence Case
    
     LDA #20                \ Move the text cursor to row 20, near the bottom of
     STA YC                 \ the screen
    
     JSR TT67K              \ Print a newline
    
     LDA #%00001111         \ Set bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch screen memory into &3000-&7FFF
    
     LDA #&6A               \ Set SC+1 = &6A, for the high byte of SC(1 0)
     STA SC+1
    
     LDA #0                 \ Set SC = 0, so now SC(1 0) = &6A00
     STA SC
    
     LDX #3                 \ We want to clear three text rows, so set a counter in
                            \ X for 3 rows
    
    .CLYL
    
     LDY #8                 \ We want to clear each text row, starting from the
                            \ left, but we don't want to overwrite the border, so we
                            \ start from the second character block, which is byte
                            \ #8 from the edge, so set Y to 8 to act as the byte
                            \ counter within the row
    
    .EE2
    
     STA (SC),Y             \ Zero the Y-th byte from SC(1 0), which clears it by
                            \ setting it to colour 0, black
    
     INY                    \ Increment the byte counter in Y
    
     BNE EE2                \ Loop back to EE2 to blank the next byte along, until
                            \ we have done one page's worth (from byte #8 to #255)
    
     INC SC+1               \ We have just finished the first page - which covers
                            \ the left half of the text row - so we increment SC+1
                            \ so SC(1 0) points to the start of the next page, or
                            \ the start of the right half of the row
    
     STA (SC),Y             \ Clear the byte at SC(1 0), as that won't be caught by
                            \ the next loop
    
     LDY #247               \ The second page covers the right half of the text row,
                            \ and as before we don't want to overwrite the border,
                            \ which we can do by starting from the last-but-one
                            \ character block and working our way left towards the
                            \ centre of the row. The last-but-one character block
                            \ ends at byte 247 (that's 255 - 8, as each character
                            \ is 8 bytes), so we put this in Y to act as a byte
                            \ counter, as before
    
    .EE3K                   \ This label is a duplicate of a label in the TT23
                            \ routine
                            \
                            \ In the original source this label is EE3, but
                            \ because BeebAsm doesn't allow us to redefine labels,
                            \ I have renamed it to EE3K
    
     STA (SC),Y             \ Zero the Y-th byte from SC(1 0), which clears it by
                            \ setting it to colour 0, black
    
     DEY                    \ Decrement the byte counter in Y
    
     BNE EE3K               \ Loop back to EE3K to blank the next byte to the left,
                            \ until we have done one page's worth (from byte #247 to
                            \ #1)
    
     INC SC+1               \ We have now blanked a whole text row, so increment
                            \ SC+1 so that SC(1 0) points to the next row
    
     DEX                    \ Decrement the row counter in X
    
     BNE CLYL               \ Loop back to blank another row, until we have done the
                            \ number of rows in X
    
     LDA #%00001001         \ Clear bits 1 and 2 of the Access Control Register at
     STA VIA+&34            \ SHEILA &34 to switch main memory back into &3000-&7FFF
    
     LDA #0                 \ Set A = 0 as this is a return value for this routine
    
     RTS                    \ Return from the subroutine
    

[X]

Label [CLYL](clyns.md#clyl) is local to this routine

[X]

Variable [DLY](../workspace/wp.md#dly) in workspace [WP](../workspace/wp.md)

In-flight message delay

[X]

Variable [DTW2](../variable/dtw2.md) (category: Text)

A flag that indicates whether we are currently printing a word

[X]

Label [EE2](clyns.md#ee2) is local to this routine

[X]

Label [EE3K](clyns.md#ee3k) is local to this routine

[X]

Variable [QQ17](../workspace/zp.md#qq17) in workspace [ZP](../workspace/zp.md)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Subroutine [TT67K](tt67k.md) (category: Text)

Print a newline

[X]

Configuration variable [VIA](../../all/workspaces.md#via) = &FE00

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Variable [YC](../workspace/zp.md#yc) in workspace [ZP](../workspace/zp.md)

The y-coordinate of the text cursor (i.e. the text row), which can be from 0 to 23

[X]

Variable [de](../workspace/wp.md#de) in workspace [WP](../workspace/wp.md)

Equipment destruction flag

[ZES2](zes2.md "Previous routine")[DIALS (Part 1 of 4)](dials_part_1_of_4.md "Next routine")
