---
title: "The COLD subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/cold.html"
---

[EXNO](exno.md "Previous routine")[NMIpissoff](nmipissoff.md "Next routine")
    
    
           Name: COLD                                                    [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Set the standard BRKV handler for the game
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-cold)
     Variations: See [code variations](../../related/compare/main/subroutine/brkbk-cold.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [S%](s_per_cent.md) calls COLD
    
    
    
    
    
    
    .COLD
    
     LDA #LO(NEWBRK)        \ Set BRKV to point to the NEWBRK routine
     STA BRKV
     LDA #HI(NEWBRK)
     STA BRKV+1
    
     LDA #LO(CHPR)          \ Set WRCHV to point to the CHPR routine
     STA WRCHV
     LDA #HI(CHPR)
     STA WRCHV+1
    
     JSR setzp              \ Call setzp to copy the top part of zero page into
                            \ the buffer at &3000
    
     JSR SETINTS            \ Call SETINTS to set various vectors, interrupts and
                            \ timers
    
     JMP SOFLUSH            \ Call SOFLUSH to reset the sound buffers and return
                            \ from the subroutine using a tail call
    

[X]

Configuration variable [BRKV](../../all/workspaces.md#brkv) = &0202

The break vector that we intercept to enable us to handle and display system errors

[X]

Subroutine [CHPR](chpr.md) (category: Text)

Print a character at the text cursor by poking into screen memory

[X]

Subroutine [NEWBRK](newbrk.md) (category: Utility routines)

The standard BRKV handler for the game

[X]

Subroutine [SETINTS](setints.md) (category: Loader)

Set the various vectors, interrupts and timers

[X]

Subroutine [SOFLUSH](soflush.md) (category: Sound)

Reset the sound buffers and turn off all sound channels

[X]

Configuration variable [WRCHV](../../all/workspaces.md#wrchv) = &020E

The WRCHV vector that we intercept with our custom text printing routine

[X]

Subroutine [setzp](setzp.md) (category: Utility routines)

Copy the top part of zero page (&0090 to &00FF) into the buffer at &3000

[EXNO](exno.md "Previous routine")[NMIpissoff](nmipissoff.md "Next routine")
