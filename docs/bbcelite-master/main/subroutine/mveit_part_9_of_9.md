---
title: "The MVEIT (Part 9 of 9) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mveit_part_9_of_9.html"
---

[MVEIT (Part 8 of 9)](mveit_part_8_of_9.md "Previous routine")[MVT1](mvt1.md "Next routine")
    
    
           Name: MVEIT (Part 9 of 9)                                     [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Move current ship: Redraw on scanner, if it hasn't been destroyed
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-mveit-part-9-of-9)
     Variations: See [code variations](../../related/compare/main/subroutine/mveit_part_9_of_9.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine has multiple stages. This stage does the following:
    
       * If the ship is exploding or being removed, hide it on the scanner
    
       * Otherwise redraw the ship on the scanner, now that it's been moved
    
    
    
    
    .MV5
    
     LDA INWK+31            \ Fetch the ship's exploding/killed state from byte #31
    
     AND #%10100000         \ If we are exploding or removing this ship then jump to
     BNE MVD1               \ MVD1 to remove it from the scanner permanently
    
     LDA INWK+31            \ Set bit 4 to keep the ship visible on the scanner
     ORA #%00010000
     STA INWK+31
    
     JMP SCAN               \ Display the ship on the scanner, returning from the
                            \ subroutine using a tail call
    
    .MVD1
    
     LDA INWK+31            \ Clear bit 4 to hide the ship on the scanner
     AND #%11101111
     STA INWK+31
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Label [MVD1](mveit_part_9_of_9.md#mvd1) is local to this routine

[X]

Subroutine [SCAN](scan.md) (category: Dashboard)

Display the current ship on the scanner

[MVEIT (Part 8 of 9)](mveit_part_8_of_9.md "Previous routine")[MVT1](mvt1.md "Next routine")
