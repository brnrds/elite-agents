---
title: "The LL9 (Part 4 of 12) subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ll9_part_4_of_12.html"
---

[LL9 (Part 3 of 12)](ll9_part_3_of_12.md "Previous routine")[LL9 (Part 5 of 12)](ll9_part_5_of_12.md "Next routine")
    
    
           Name: LL9 (Part 4 of 12)                                      [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw ship: Set visibility for exploding ship (all faces visible)
      Deep dive: [Drawing ships](https://elite.bbcelite.com/deep_dives/drawing_ships.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_g.md#header-ll9-part-4-of-12)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part sets up the visibility block in XX2 for a ship that is exploding.
    
     The XX2 block consists of one byte for each face in the ship's blueprint,
     which holds the visibility of that face. Because the ship is exploding, we
     want to set all the faces to be visible. A value of 255 in the visibility
     table means the face is visible, so the following code sets each face to 255
     and then skips over the face visibility calculations that we would apply to a
     non-exploding ship.
    
    
    
    
     LDA (XX0),Y            \ Fetch byte #12 of the ship's blueprint, which contains
                            \ the number of faces * 4
    
     LSR A                  \ Set X = A / 4
     LSR A                  \       = the number of faces
     TAX
    
     LDA #255               \ Set A = 255
    
    .EE30
    
     STA XX2,X              \ Set the X-th byte of XX2 to 255
    
     DEX                    \ Decrement the loop counter
    
     BPL EE30               \ Loop back for the next byte until there is one byte
                            \ set to 255 for each face
    
     INX                    \ Set XX4 = 0 for the distance value we use to test
     STX XX4                \ for visibility, so we always shows everything
    
    .LL41
    
     JMP LL42               \ Jump to LL42 to skip the face visibility calculations
                            \ as we don't need to do them now we've set up the XX2
                            \ block for the explosion
    

[X]

Label [EE30](ll9_part_4_of_12.md#ee30) is local to this routine

[X]

Label [LL42](ll9_part_6_of_12.md#ll42) in subroutine [LL9 (Part 6 of 12)](ll9_part_6_of_12.md)

[X]

Variable [XX0](../workspace/zp.md#xx0) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX2](../workspace/zp.md#xx2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store the visibility of the ship's faces during the ship-drawing routine at LL9

[X]

Variable [XX4](../workspace/zp.md#xx4) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[LL9 (Part 3 of 12)](ll9_part_3_of_12.md "Previous routine")[LL9 (Part 5 of 12)](ll9_part_5_of_12.md "Next routine")
