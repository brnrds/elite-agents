---
title: "The DOEXP subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/doexp.html"
---

[SWAPPZERO](swappzero.md "Previous routine")[exlook](../variable/exlook.md "Next routine")
    
    
           Name: DOEXP                                                   [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw an exploding ship
      Deep dive: [Drawing explosion clouds](https://elite.bbcelite.com/deep_dives/drawing_explosion_clouds.html)
                 [Generating random numbers](https://elite.bbcelite.com/deep_dives/generating_random_numbers.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-doexp)
     Variations: See [code variations](../../related/compare/main/subroutine/doexp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) calls DOEXP
                 * [LL9 (Part 9 of 12)](ll9_part_9_of_12.md) calls DOEXP
    
    
    
    
    
    
    .EX2
    
     LDA INWK+31            \ Set bits 5 and 7 of the ship's byte #31 to denote that
     ORA #%10100000         \ the ship is exploding and has been killed
     STA INWK+31
    
     RTS                    \ Return from the subroutine
    
    .DOEXP
    
     LDA INWK+31            \ If bit 6 of the ship's byte #31 is clear, then the
     AND #%01000000         \ ship is not already exploding so there is no existing
     BEQ P%+5               \ explosion cloud to remove, so skip the following
                            \ instruction
    
     JSR PTCLS              \ Call PTCLS to remove the existing cloud by drawing it
                            \ again
    
     LDA INWK+6             \ Set T = z_lo
     STA T
    
     LDA INWK+7             \ Set A = z_hi, so (A T) = z
    
     CMP #32                \ If z_hi < 32, skip the next two instructions
     BCC P%+6
    
     LDA #&FE               \ Set A = 254 and jump to yy (this BNE is effectively a
     BNE yy                 \ JMP, as A is never zero)
    
     ASL T                  \ Shift (A T) left twice
     ROL A
     ASL T
     ROL A
    
     SEC                    \ And then shift A left once more, inserting a 1 into
     ROL A                  \ bit 0
    
                            \ Overall, the above multiplies A by 8 and makes sure it
                            \ is at least 1, to leave a one-byte distance in A. We
                            \ can use this as the distance for our cloud, to ensure
                            \ that the explosion cloud is visible even for ships
                            \ that blow up a long way away
    
    .yy
    
     STA Q                  \ Store the distance to the explosion in Q
    
     LDY #1                 \ Fetch byte #1 of the ship line heap, which contains
     LDA (XX19),Y           \ the cloud counter
    
     STA frump              \ Store the cloud counter in frump (though this value is
                            \ never read, so this has no effect)
    
     ADC #4                 \ Add 4 to the cloud counter, so it ticks onwards every
                            \ we redraw it
    
     BCS EX2                \ If the addition overflowed, jump up to EX2 to update
                            \ the explosion flags and return from the subroutine
    
     STA (XX19),Y           \ Store the updated cloud counter in byte #1 of the ship
                            \ line heap
    
     JSR DVID4              \ Calculate the following:
                            \
                            \   (P R) = 256 * A / Q
                            \         = 256 * cloud counter / distance
                            \
                            \ We are going to use this as our cloud size, so the
                            \ further away the cloud, the smaller it is, and as the
                            \ cloud counter ticks onward, the cloud expands
    
     LDA P                  \ Set A = P, so we now have:
                            \
                            \   (A R) = 256 * cloud counter / distance
    
     CMP #&1C               \ If A < 28, skip the next two instructions
     BCC P%+6
    
     LDA #&FE               \ Set A = 254 and skip the following (this BNE is
     BNE LABEL_1            \ effectively a JMP as A is never zero)
    
     ASL R                  \ Shift (A R) left three times to multiply by 8
     ROL A
     ASL R
     ROL A
     ASL R
     ROL A
    
                            \ Overall, the above multiplies (A R) by 8 to leave a
                            \ one-byte cloud size in A, given by the following:
                            \
                            \   A = 8 * cloud counter / distance
    
    .LABEL_1
    
     DEY                    \ Decrement Y to 0
    
     STA (XX19),Y           \ Store the cloud size in byte #0 of the ship line heap
    
     LDA INWK+31            \ Clear bit 6 of the ship's byte #31 to denote that the
     AND #%10111111         \ explosion has not yet been drawn
     STA INWK+31
    
     AND #%00001000         \ If bit 3 of the ship's byte #31 is clear, then nothing
     BEQ TT48               \ is being drawn on-screen for this ship anyway, so
                            \ return from the subroutine (as TT48 contains an RTS)
    
     LDY #2                 \ Otherwise it's time to draw an explosion cloud, so
     LDA (XX19),Y           \ fetch byte #2 of the ship line heap into Y, which we
     TAY                    \ set to the explosion count for this ship (i.e. the
                            \ number of vertices used as origins for explosion
                            \ clouds)
                            \
                            \ The explosion count is stored as 4 * n + 6, where n is
                            \ the number of vertices, so the following loop copies
                            \ the coordinates of the first n vertices from the heap
                            \ at XX3, which is where we stored all the visible
                            \ vertex coordinates in part 8 of the LL9 routine, and
                            \ sticks them in the ship line heap pointed to by XX19,
                            \ starting at byte #7 (so it leaves the first 6 bytes of
                            \ the ship line heap alone)
    
    .EXL1
    
     LDA XX3-7,Y            \ Copy byte Y-7 from the XX3 heap, into the Y-th byte of
     STA (XX19),Y           \ the ship line heap
    
     DEY                    \ Decrement the loop counter
    
     CPY #6                 \ Keep copying vertex coordinates into the ship line
     BNE EXL1               \ heap until Y = 6 (which will copy n vertices, where n
                            \ is the number of vertices we should be exploding)
    
     LDA INWK+31            \ Set bit 6 of the ship's byte #31 to denote that the
     ORA #%01000000         \ explosion has been drawn (as it's about to be)
     STA INWK+31
    
    .PTCLS
    
                            \ This part of the routine actually draws the explosion
                            \ cloud
    
     LDY #0                 \ Fetch byte #0 of the ship line heap, which contains
     LDA (XX19),Y           \ the cloud size we stored above, and store it in Q
     STA Q
    
     INY                    \ Increment the index in Y to point to byte #1
    
     LDA (XX19),Y           \ Fetch byte #1 of the ship line heap, which contains
                            \ the cloud counter. We are now going to process this
                            \ into the number of particles in each vertex's cloud
    
     BPL P%+4               \ If the cloud counter < 128, then we are in the first
                            \ half of the cloud's existence, so skip the next
                            \ instruction
    
     EOR #&FF               \ Flip the value of A so that in the second half of the
                            \ cloud's existence, A counts down instead of up
    
     LSR A                  \ Divide A by 16 so that is has a maximum value of 7
     LSR A
     LSR A
     LSR A
    
     ORA #1                 \ Make sure A is at least 1 and store it in U, to
     STA U                  \ give us the number of particles in the explosion for
                            \ each vertex
    
     INY                    \ Increment the index in Y to point to byte #2
    
     LDA (XX19),Y           \ Fetch byte #2 of the ship line heap, which contains
     STA TGT                \ the explosion count for this ship (i.e. the number of
                            \ vertices used as origins for explosion clouds) and
                            \ store it in TGT
    
     LDA RAND+1             \ Fetch the current random number seed in RAND+1 and
     PHA                    \ store it on the stack, so we can re-randomise the
                            \ seeds when we are done
    
     LDY #6                 \ Set Y = 6 to point to the byte before the first vertex
                            \ coordinate we stored on the ship line heap above (we
                            \ increment it below so it points to the first vertex)
    
    .EXL5
    
     LDX #3                 \ We are about to fetch a pair of coordinates from the
                            \ ship line heap, so set a counter in X for 4 bytes
    
    .EXL3
    
     INY                    \ Increment the index in Y so it points to the next byte
                            \ from the coordinate we are copying
    
     LDA (XX19),Y           \ Copy the Y-th byte from the ship line heap to the X-th
     STA K3,X               \ byte of K3
    
     DEX                    \ Decrement the X index
    
     BPL EXL3               \ Loop back to EXL3 until we have copied all four bytes
    
                            \ The above loop copies the vertex coordinates from the
                            \ ship line heap to K3, reversing them as we go, so it
                            \ sets the following:
                            \
                            \   K3+3 = x_lo
                            \   K3+2 = x_hi
                            \   K3+1 = y_lo
                            \   K3+0 = y_hi
    
     STY CNT                \ Set CNT to the index that points to the next vertex on
                            \ the ship line heap
    
     LDY #2                 \ Set Y = 2, which we will use to point to bytes #3 to
                            \ #6, after incrementing it
    
                            \ This next loop copies bytes #3 to #6 from the ship
                            \ line heap into the four random number seeds in RAND to
                            \ RAND+3, EOR'ing them with the vertex index so they are
                            \ different for every vertex. This enables us to
                            \ generate random numbers for drawing each vertex that
                            \ are random but repeatable, which we need when we
                            \ redraw the cloud to remove it
                            \
                            \ Note that we haven't actually set the values of bytes
                            \ #3 to #6 in the ship line heap, so we have no idea
                            \ what they are, we just use what's already there. But
                            \ the fact that those bytes are stored for this ship
                            \ means we can repeat the random generation of the
                            \ cloud, which is the important bit
    
    .EXL2
    
     INY                    \ Increment the index in Y so it points to the next
                            \ random number seed to copy
    
     LDA (XX19),Y           \ Fetch the Y-th byte from the ship line heap
    
     EOR CNT                \ EOR with the vertex index, so the seeds are different
                            \ for each vertex
    
    IF _SNG47
    
     STA &FFFF,Y            \ Y is going from 3 to 6, so this stores the four bytes
                            \ in memory locations &02, &03, &04 and &05, which are
                            \ the memory locations of RAND through RAND+3
    
    ELIF _COMPACT
    
     STA &0000,Y            \ Y is going from 3 to 6, so this stores the four bytes
                            \ in memory locations &03, &04, &05 and &06, which are
                            \ the memory locations of RAND through RAND+3
    
    ENDIF
    
     CPY #6                 \ Loop back to EXL2 until Y = 6, which means we have
     BNE EXL2               \ copied four bytes
    
     LDY U                  \ Set Y to the number of particles in the explosion for
                            \ each vertex, which we stored in U above. We will now
                            \ use this as a loop counter to iterate through all the
                            \ particles in the explosion
    
     STY CNT2               \ Store the number of explosion particles in CNT2, to
                            \ use this as a loop counter to iterate through all the
                            \ particles in the explosion
    
    .EXL4
    
     CLC                    \ This contains the code from the DORND2 routine, so
     LDA RAND               \ this section is exactly equivalent to a JSR DORND2
     ROL A                  \ call, but is slightly faster as it's been inlined
     TAX                    \ (so it sets A and X to random values, making sure
     ADC RAND+2             \ the C flag doesn't affect the outcome)
     STA RAND
     STX RAND+2
     LDA RAND+1
     TAX
     ADC RAND+3
     STA RAND+1
     STX RAND+3
    
     STA ZZ                 \ Set ZZ to a random number
    
     AND #3                 \ Set X to this random number, reduced to be in the
     TAX                    \ range 0-3
    
     LDA coltabl,X          \ Set the colour randomly to one of the four colours
     STA COL                \ in the coltabl table, so explosions are made up of
                            \ yellow, red and cyan particles
    
     LDA K3+1               \ Set (A R) = (y_hi y_lo)
     STA R                  \           = y
     LDA K3
    
     JSR EXS1               \ Set (A X) = (A R) +/- random * cloud size
                            \           = y +/- random * cloud size
    
     BNE EX11               \ If A is non-zero, the particle is off-screen as the
                            \ coordinate is bigger than 255), so jump to EX11 to do
                            \ the next particle
    
     CPX #2*Y-1             \ If X > the y-coordinate of the bottom of the screen,
     BCS EX11               \ the particle is off the bottom of the screen, so jump
                            \ to EX11 to do the next particle
    
                            \ Otherwise X contains a random y-coordinate within the
                            \ cloud
    
     STX Y1                 \ Set Y1 = our random y-coordinate within the cloud
    
     LDA K3+3               \ Set (A R) = (x_hi x_lo)
     STA R
     LDA K3+2
    
     JSR EXS1               \ Set (A X) = (A R) +/- random * cloud size
                            \           = x +/- random * cloud size
    
     BNE EX4                \ If A is non-zero, the particle is off-screen as the
                            \ coordinate is bigger than 255), so jump to EX4 to do
                            \ the next particle
    
                            \ Otherwise X contains a random x-coordinate within the
                            \ cloud
    
     LDA Y1                 \ Set A = our random y-coordinate within the cloud
    
     JSR PIXEL              \ Draw a point at screen coordinate (X, A) with the
                            \ point size determined by the distance in ZZ
    
    .EX4
    
     DEC CNT2               \ Decrement the loop counter in CNT2 for the next
                            \ particle
    
     BPL EXL4               \ Loop back to EXL4 until we have done all the particles
                            \ in the cloud
    
     LDY CNT                \ Set Y to the index that points to the next vertex on
                            \ the ship line heap
    
     CPY TGT                \ If Y < TGT, which we set to the explosion count for
     BCC EXL5               \ this ship (i.e. the number of vertices used as origins
                            \ for explosion clouds), loop back to EXL5 to do a cloud
                            \ for the next vertex
    
     PLA                    \ Restore the current random number seed to RAND+1 that
     STA RAND+1             \ we stored at the start of the routine
    
     LDA K%+6               \ Store the z_lo coordinate for the planet (which will
     STA RAND+3             \ be pretty random) in the RAND+3 seed
    
     RTS                    \ Return from the subroutine
    
    .EX11
    
     CLC                    \ This contains the code from the DORND2 routine, so
     LDA RAND               \ this section is exactly equivalent to a JSR DORND2
     ROL A                  \ call, but is slightly faster as it's been inlined
     TAX                    \ (so it sets A and X to random values, making sure
     ADC RAND+2             \ the C flag doesn't affect the outcome)
     STA RAND
     STX RAND+2
     LDA RAND+1
     TAX
     ADC RAND+3
     STA RAND+1
     STX RAND+3
    
     JMP EX4                \ We just skipped a particle, so jump up to EX4 to do
                            \ the next one
    
    .EXS1
    
                            \ This routine calculates the following:
                            \
                            \   (A X) = (A R) +/- random * cloud size
                            \
                            \ returning with the flags set for the high byte in A
    
     STA S                  \ Store A in S so we can use it later
    
     CLC                    \ This contains the code from the DORND2 routine, so
     LDA RAND               \ this section is exactly equivalent to a JSR DORND2
     ROL A                  \ call, but is slightly faster as it's been inlined
     TAX                    \ (so it sets A and X to random values, making sure
     ADC RAND+2             \ the C flag doesn't affect the outcome)
     STA RAND
     STX RAND+2
     LDA RAND+1
     TAX
     ADC RAND+3
     STA RAND+1
     STX RAND+3
    
     ROL A                  \ Set A = A * 2
    
     BCS EX5                \ If bit 7 of A was set (50% chance), jump to EX5
    
     JSR FMLTU              \ Set A = A * Q / 256
                            \       = random << 1 * projected cloud size / 256
    
     ADC R                  \ Set (A X) = (S R) + A
     TAX                    \           = (S R) + random * projected cloud size
                            \
                            \ where S contains the argument A, starting with the low
                            \ bytes
    
     LDA S                  \ And then the high bytes
     ADC #0
    
     RTS                    \ Return from the subroutine
    
    .EX5
    
     JSR FMLTU              \ Set T = A * Q / 256
     STA T                  \       = random << 1 * projected cloud size / 256
    
     LDA R                  \ Set (A X) = (S R) - T
     SBC T                  \
     TAX                    \ where S contains the argument A, starting with the low
                            \ bytes
    
     LDA S                  \ And then the high bytes
     SBC #0
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [CNT](../workspace/zp.md#cnt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [CNT2](../workspace/zp.md#cnt2) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [DVID4](dvid4.md) (category: Maths (Arithmetic))

Calculate (P R) = 256 * A / Q

[X]

Label [EX11](doexp.md#ex11) is local to this routine

[X]

Label [EX2](doexp.md#ex2) is local to this routine

[X]

Label [EX4](doexp.md#ex4) is local to this routine

[X]

Label [EX5](doexp.md#ex5) is local to this routine

[X]

Label [EXL1](doexp.md#exl1) is local to this routine

[X]

Label [EXL2](doexp.md#exl2) is local to this routine

[X]

Label [EXL3](doexp.md#exl3) is local to this routine

[X]

Label [EXL4](doexp.md#exl4) is local to this routine

[X]

Label [EXL5](doexp.md#exl5) is local to this routine

[X]

Label [EXS1](doexp.md#exs1) is local to this routine

[X]

Subroutine [FMLTU](fmltu.md) (category: Maths (Arithmetic))

Calculate A = A * Q / 256

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Variable [K3](../workspace/zp.md#k3) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Label [LABEL_1](doexp.md#label_1) is local to this routine

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PIXEL](pixel.md) (category: Drawing pixels)

Draw a one-pixel dot, two-pixel dash or four-pixel square

[X]

Label [PTCLS](doexp.md#ptcls) is local to this routine

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [RAND](../workspace/zp.md#rand) in workspace [ZP](../workspace/zp.md)

Four 8-bit seeds for the random number generation system implemented in the DORND routine

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [TGT](../workspace/zp.md#tgt) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used as a target value for counters when drawing explosion clouds and partial circles

[X]

Entry point [TT48](ex.md#tt48) in subroutine [ex](ex.md) (category: Text)

Contains an RTS

[X]

Variable [U](../workspace/zp.md#u) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX19](../workspace/zp.md#xx19) in workspace [ZP](../workspace/zp.md)

XX19(1 0) shares its location with INWK(34 33), which contains the address of the ship line heap

[X]

Workspace [XX3](../workspace/xx3.md) (category: Workspaces)

Temporary storage space for complex calculations

[X]

Configuration variable [Y](../../all/workspaces.md#y) = 96

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [ZZ](../workspace/zp.md#zz) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for distance values

[X]

Variable [coltabl](../variable/coltabl.md) (category: Drawing ships)

Colours for ship explosions

[X]

Variable [frump](../workspace/wp.md#frump) in workspace [WP](../workspace/wp.md)

Used to store the number of particles in the explosion cloud, though the number is never actually used

[X]

Label [yy](doexp.md#yy) is local to this routine

[SWAPPZERO](swappzero.md "Previous routine")[exlook](../variable/exlook.md "Next routine")
