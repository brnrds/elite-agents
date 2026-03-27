---
title: "The KS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ks1.html"
---

[KS3](ks3.md "Previous routine")[KS4](ks4.md "Next routine")
    
    
           Name: KS1                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Remove the current ship from our local bubble of universe
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-ks1)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 12 of 16)](main_flight_loop_part_12_of_16.md) calls KS1
    
    
    
    
    
    * * *
    
    
     Part 12 of the main flight loop calls this routine to remove the ship that is
     currently being analysed by the flight loop. Once the ship is removed, it
     jumps back to MAL1 to rejoin the main flight loop, with X pointing to the
     same slot that we just cleared (and which now contains the next ship in the
     local bubble of universe).
    
    
    
    * * *
    
    
     Arguments:
    
       XX0                  The address of the blueprint for this ship
    
       INF                  The address of the data block for this ship
    
    
    
    
    .KS1
    
     LDX XSAV               \ Fetch the current ship's slot number from XSAV
    
     JSR KILLSHP            \ Call KILLSHP to remove the ship in slot X from our
                            \ local bubble of universe
    
     LDX XSAV               \ Restore the current ship's slot number from XSAV,
                            \ which now points to the next ship in the bubble
    
     JMP MAL1               \ Jump to MAL1 to rejoin the main flight loop at the
                            \ start of the ship analysis loop
    

[X]

Subroutine [KILLSHP](killshp.md) (category: Universe)

Remove a ship from our local bubble of universe

[X]

Entry point [MAL1](main_flight_loop_part_4_of_16.md#mal1) in subroutine [Main flight loop (Part 4 of 16)](main_flight_loop_part_4_of_16.md) (category: Main loop)

Marks the beginning of the ship analysis loop, so we can jump back here from part 12 of the main flight loop to work our way through each ship in the local bubble. We also jump back here when a ship is removed from the bubble, so we can continue processing from the next ship

[X]

Variable [XSAV](../workspace/zp.md#xsav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the X register, used in a number of places

[KS3](ks3.md "Previous routine")[KS4](ks4.md "Next routine")
