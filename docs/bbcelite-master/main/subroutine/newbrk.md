---
title: "The NEWBRK subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/newbrk.html"
---

[stackpt](../variable/stackpt.md "Previous routine")[DEATH](death.md "Next routine")
    
    
           Name: NEWBRK                                                  [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: The standard BRKV handler for the game
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-newbrk)
     Variations: See [code variations](../../related/compare/main/subroutine/brbr-newbrk.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [COLD](cold.md) calls NEWBRK
    
    
    
    
    
    * * *
    
    
     This routine is used to display error messages, before restarting the game.
     When called, it makes a beep and prints the system error message in the block
     pointed to by (&FD &FE), which is where the MOS will put any system errors. It
     then waits for a key press and restarts the game.
    
    
    
    
    .NEWBRK
    
     LDX stackpt            \ Set the stack pointer to the value that we stored in
     TXS                    \ location stack, so that's back to the value it had
                            \ before we change it in the SVE routine
    
     JSR getzp              \ Call getzp to restore the top part of zero page from
                            \ the buffer at &3000, as this will have been stored in
                            \ the buffer before performing the disc access that gave
                            \ the error we're processsing
    
     STZ CATF               \ Set the CATF flag to 0, so the TT26 routine reverts to
                            \ standard formatting
    
     LDY #0                 \ Set Y to 0, which we use as a loop counter below
    
     LDA #7                 \ Set A = 7 to generate a beep before we print the error
                            \ message
    
    .BRBRLOOP
    
     JSR CHPR               \ Print the character in A, which contains a line feed
                            \ on the first loop iteration, and then any non-zero
                            \ characters we fetch from the error message
    
     INY                    \ Increment the loop counter
    
     LDA (&FD),Y            \ Fetch the Y-th byte of the block pointed to by
                            \ (&FD &FE), so that's the Y-th character of the message
                            \ pointed to by the error message pointer
    
     BNE BRBRLOOP           \ If the fetched character is non-zero, loop back to the
                            \ JSR OSWRCH above to print the it, and keep looping
                            \ until we fetch a zero (which marks the end of the
                            \ message)
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     JMP SVE                \ Jump to SVE to display the disc access menu and return
                            \ from the subroutine using a tail call
    

[X]

Label [BRBRLOOP](newbrk.md#brbrloop) is local to this routine

[X]

Variable [CATF](../workspace/up.md#catf) in workspace [UP](../workspace/up.md)

The disc catalogue flag

[X]

Subroutine [CHPR](chpr.md) (category: Text)

Print a character at the text cursor by poking into screen memory

[X]

Subroutine [SVE](sve.md) (category: Save and load)

Display the disk access menu and process saving of commander files

[X]

Subroutine [getzp](getzp.md) (category: Utility routines)

Swap zero page (&0090 to &00EF) with the buffer at &3000

[X]

Variable [stackpt](../variable/stackpt.md) (category: Save and load)

Temporary storage for the stack pointer when jumping to the break handler at NEWBRK

[X]

Entry point [t](tt217.md#t) in subroutine [TT217](tt217.md) (category: Keyboard)

As TT217 but don't preserve Y, set it to YSAV instead

[stackpt](../variable/stackpt.md "Previous routine")[DEATH](death.md "Next routine")
