---
title: "The TTX66K subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ttx66k.html"
---

[TT66](tt66.md "Previous routine")[TRTB%](../variable/trtb_per_cent.md "Next routine")
    
    
           Name: TTX66K                                                  [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Clear the top part of the screen, draw a border box and configure
                 the specified view
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-ttx66k)
     Variations: See [code variations](../../related/compare/main/subroutine/ttx66-ttx66k.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Clear the top part of the screen (the space view) and draw a border box
     along the top and sides.
    
    
    
    
    .TTX66K
    
     JSR TTX66              \ Call TTX66 to clear the top part of the screen and
                            \ draw a border box
    
     JSR MT2                \ Switch to Sentence Case when printing extended tokens
    
     LDA #0                 \ Reset the ball line heap pointer at LSP
     STA LSP
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch to Sentence Case
     STA QQ17
    
     STA DTW2               \ Set bit 7 of DTW2 to indicate we are not currently
                            \ printing a word
    
     JSR FLFLLS             \ Call FLFLLS to reset the LSO block
    
     LDA #0                 \ Set LAS2 = 0 to stop any laser pulsing
     STA LAS2
    
     STA DLY                \ Set the delay in DLY to 0, to indicate that we are
                            \ no longer showing an in-flight message, so any new
                            \ in-flight messages will be shown instantly
    
     STA de                 \ Clear de, the flag that appends " DESTROYED" to the
                            \ end of the next text token, so that it doesn't
    
     LDX QQ22+1             \ Fetch into X the number that's shown on-screen during
                            \ the hyperspace countdown
    
     BEQ P%+5               \ If the counter is zero then we are not counting down
                            \ to hyperspace, so skip the next instruction
    
     JSR ee3                \ Print the 8-bit number in X at text location (0, 1),
                            \ i.e. print the hyperspace countdown in the top-left
                            \ corner
    
     LDA QQ11               \ If this is not a space view, jump to tt66 to skip
     BNE tt66               \ displaying the view name
    
     LDA #11                \ Move the text cursor to row 11
     STA XC
    
     LDA #CYAN              \ Switch to colour 3, which is cyan in the space view
     STA COL
    
     LDA VIEW               \ Load the current view into A:
                            \
                            \   0 = front
                            \   1 = rear
                            \   2 = left
                            \   3 = right
    
     ORA #&60               \ OR with &60 so we get a value of &60 to &63 (96 to 99)
    
     JSR TT27               \ Print recursive token 96 to 99, which will be in the
                            \ range "FRONT" to "RIGHT"
    
     JSR TT162              \ Print a space
    
     LDA #175               \ Print recursive token 15 ("VIEW ")
     JSR TT27
    
    .tt66
    
     LDX #0                 \ Set QQ17 = 0 to switch to ALL CAPS
     STX QQ17
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Variable [DLY](../workspace/wp.md#dly) in workspace [WP](../workspace/wp.md)

In-flight message delay

[X]

Variable [DTW2](../variable/dtw2.md) (category: Text)

A flag that indicates whether we are currently printing a word

[X]

Subroutine [FLFLLS](flflls.md) (category: Drawing suns)

Reset the sun line heap

[X]

Variable [LAS2](../workspace/wp.md#las2) in workspace [WP](../workspace/wp.md)

Laser power for the current laser

[X]

Variable [LSP](../workspace/zp.md#lsp) in workspace [ZP](../workspace/zp.md)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Subroutine [MT2](mt2.md) (category: Text)

Switch to Sentence Case when printing extended tokens

[X]

Variable [QQ11](../workspace/zp.md#qq11) in workspace [ZP](../workspace/zp.md)

The type of the current view

[X]

Variable [QQ17](../workspace/zp.md#qq17) in workspace [ZP](../workspace/zp.md)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Variable [QQ22](../workspace/zp.md#qq22) in workspace [ZP](../workspace/zp.md)

The two hyperspace countdown counters

[X]

Subroutine [TT162](tt162.md) (category: Text)

Print a space

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[X]

Subroutine [TTX66](ttx66.md) (category: Drawing the screen)

Clear the top part of the screen and draw a border box

[X]

Variable [VIEW](../workspace/wp.md#view) in workspace [WP](../workspace/wp.md)

The number of the current space view

[X]

Variable [XC](../workspace/zp.md#xc) in workspace [ZP](../workspace/zp.md)

The x-coordinate of the text cursor (i.e. the text column), which can be from 0 to 32

[X]

Variable [de](../workspace/wp.md#de) in workspace [WP](../workspace/wp.md)

Equipment destruction flag

[X]

Subroutine [ee3](ee3.md) (category: Flight)

Print the hyperspace countdown in the top-left of the screen

[X]

Label [tt66](ttx66k.md#tt66) is local to this routine

[TT66](tt66.md "Previous routine")[TRTB%](../variable/trtb_per_cent.md "Next routine")
