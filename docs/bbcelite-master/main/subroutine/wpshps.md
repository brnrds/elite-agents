---
title: "The WPSHPS subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/wpshps.html"
---

[nWq](nwq.md "Previous routine")[FLFLLS](flflls.md "Next routine")
    
    
           Name: WPSHPS                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Clear the scanner, reset the ball line and sun line heaps
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-wpshps)
     Variations: See [code variations](../../related/compare/main/subroutine/wpshps.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOOK1](look1.md) calls WPSHPS
                 * [NWSTARS](nwstars.md) calls WPSHPS
                 * [RES2](res2.md) calls WPSHPS
    
    
    
    
    
    * * *
    
    
     Remove all ships from the scanner, reset the sun line heap at LSO, and reset
     the ball line heap at LSX2 and LSY2.
    
    
    
    
    .WPSHPS
    
     LDX #0                 \ Set up a counter in X to work our way through all the
                            \ ship slots in FRIN
    
    .WSL1
    
     LDA FRIN,X             \ Fetch the ship type in slot X
    
     BEQ WS2                \ If the slot contains 0 then it is empty and we have
                            \ checked all the slots (as they are always shuffled
                            \ down in the main loop to close up and gaps), so jump
                            \ to WS2 as we are done
    
     BMI WS1                \ If the slot contains a ship type with bit 7 set, then
                            \ it contains the planet or the sun, so jump down to WS1
                            \ to skip this slot, as the planet and sun don't appear
                            \ on the scanner
    
     STA TYPE               \ Store the ship type in TYPE
    
     JSR GINF               \ Call GINF to get the address of the data block for
                            \ ship slot X and store it in INF
    
     LDY #31                \ We now want to copy the first 32 bytes from the ship's
                            \ data block into INWK, so set a counter in Y
    
    .WSL2
    
     LDA (INF),Y            \ Copy the Y-th byte from the data block pointed to by
     STA INWK,Y             \ INF into the Y-th byte of INWK workspace
    
     DEY                    \ Decrement the counter to point at the next byte
    
     BPL WSL2               \ Loop back to WSL2 until we have copied all 32 bytes
    
     STX XSAV               \ Store the ship slot number in XSAV while we call SCAN
    
     JSR SCAN               \ Call SCAN to plot this ship on the scanner, which will
                            \ remove it as it's plotted with EOR logic
    
     LDX XSAV               \ Restore the ship slot number from XSAV into X
    
     LDY #31                \ Clear bits 3, 4 and 6 in the ship's byte #31, which
     LDA (INF),Y            \ stops drawing the ship on-screen (bit 3), hides it
     AND #%10100111         \ from the scanner (bit 4) and stops any lasers firing
     STA (INF),Y            \ (bit 6)
    
    .WS1
    
     INX                    \ Increment X to point to the next ship slot
    
     BNE WSL1               \ Loop back up to process the next slot (this BNE is
                            \ effectively a JMP as X will never be zero)
    
    .WS2
    
     LDX #0                 \ Reset the ball line heap by setting the ball line heap
     STX LSP                \ pointer to 0
    
     DEX                    \ Set X = &FF
    
     STX LSX2               \ Set LSX2 = LSY2 = &FF to clear the ball line heap
     STX LSY2
    
                            \ Fall through into FLFLLS to reset the LSO block
    

[X]

Variable [FRIN](../workspace/wp.md#frin) in workspace [WP](../workspace/wp.md)

Slots for the ships in the local bubble of universe

[X]

Subroutine [GINF](ginf.md) (category: Universe)

Fetch the address of a ship's data block into INF

[X]

Variable [INF](../workspace/zp.md#inf) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [LSP](../workspace/zp.md#lsp) in workspace [ZP](../workspace/zp.md)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Variable [LSX2](../workspace/wp.md#lsx2) in workspace [WP](../workspace/wp.md)

The ball line heap for storing x-coordinates

[X]

Variable [LSY2](../workspace/wp.md#lsy2) in workspace [WP](../workspace/wp.md)

The ball line heap for storing y-coordinates

[X]

Subroutine [SCAN](scan.md) (category: Dashboard)

Display the current ship on the scanner

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[X]

Label [WS1](wpshps.md#ws1) is local to this routine

[X]

Label [WS2](wpshps.md#ws2) is local to this routine

[X]

Label [WSL1](wpshps.md#wsl1) is local to this routine

[X]

Label [WSL2](wpshps.md#wsl2) is local to this routine

[X]

Variable [XSAV](../workspace/zp.md#xsav) in workspace [ZP](../workspace/zp.md)

Temporary storage for saving the value of the X register, used in a number of places

[nWq](nwq.md "Previous routine")[FLFLLS](flflls.md "Next routine")
