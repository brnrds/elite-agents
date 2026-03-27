---
title: "The PLANET subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/planet.html"
---

[PL2](pl2.md "Previous routine")[PL9 (Part 1 of 3)](pl9_part_1_of_3.md "Next routine")
    
    
           Name: PLANET                                                  [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw the planet or sun
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_e.md#header-planet)
     Variations: See [code variations](../../related/compare/main/subroutine/planet.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL9 (Part 1 of 12)](ll9_part_1_of_12.md) calls PLANET
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The planet or sun's ship data block
    
    
    
    
    .PLANET
    
     LDA #GREEN             \ Switch to stripe 3-1-3-1, which is cyan/yellow in the
     STA COL                \ space view
    
     LDA INWK+8             \ Set A = z_sign (the highest byte in the planet/sun's
                            \ coordinates)
    
    \BMI PL2                \ This instruction is commented out in the original
                            \ source. It would remove the planet from the screen
                            \ when it's behind us
    
     CMP #48                \ If A >= 48 then the planet/sun is too far away to be
     BCS PL2                \ seen, so jump to PL2 to remove it from the screen,
                            \ returning from the subroutine using a tail call
    
     ORA INWK+7             \ Set A to 0 if both z_sign and z_hi are 0
    
     BEQ PL2                \ If both z_sign and z_hi are 0, then the planet/sun is
                            \ too close to be shown, so jump to PL2 to remove it
                            \ from the screen, returning from the subroutine using a
                            \ tail call
    
     JSR PROJ               \ Project the planet/sun onto the screen, returning the
                            \ centre's coordinates in K3(1 0) and K4(1 0)
    
     BCS PL2                \ If the C flag is set by PROJ then the planet/sun is
                            \ not visible on-screen, so jump to PL2 to remove it
                            \ from the screen, returning from the subroutine using
                            \ a tail call
    
     LDA #96                \ Set (A P+1 P) = (0 96 0) = 24576
     STA P+1                \
     LDA #0                 \ This represents the planet/sun's radius at a distance
     STA P                  \ of z = 1
    
     JSR DVID3B2            \ Call DVID3B2 to calculate:
                            \
                            \   K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)
                            \              = (0 96 0) / z
                            \              = 24576 / z
                            \
                            \ so K now contains the planet/sun's radius, reduced by
                            \ the actual distance to the planet/sun. We know that
                            \ K+3 and K+2 will be 0, as the number we are dividing,
                            \ (0 96 0), fits into the two bottom bytes, so the
                            \ result is actually in K(1 0)
    
     LDA K+1                \ If the high byte of the reduced radius is zero, jump
     BEQ PL82               \ to PL82, as K contains the radius on its own
    
     LDA #248               \ Otherwise set K = 248, to round up the radius in
     STA K                  \ K(1 0) to the nearest integer (if we consider the low
                            \ byte to be the fractional part)
    
    .PL82
    
     LDA TYPE               \ If the planet/sun's type has bit 0 clear, then it's
     LSR A                  \ either 128 or 130, which is a planet (the sun has type
     BCC PL9                \ 129, which has bit 0 set). So jump to PL9 to draw the
                            \ planet with radius K, returning from the subroutine
                            \ using a tail call
    
     JMP SUN                \ Otherwise jump to SUN to draw the sun with radius K,
                            \ returning from the subroutine using a tail call
    

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Subroutine [DVID3B2](dvid3b2.md) (category: Maths (Arithmetic))

Calculate K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)

[X]

Configuration variable [GREEN](../../all/workspaces.md#green) = %10101111

Four mode 1 pixels of colour 3, 1, 3, 1 (cyan/yellow)

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [PL2](pl2.md) (category: Drawing planets)

Remove the planet or sun from the screen

[X]

Label [PL82](planet.md#pl82) is local to this routine

[X]

Subroutine [PL9 (Part 1 of 3)](pl9_part_1_of_3.md) (category: Drawing planets)

Draw the planet, with either an equator and meridian, or a crater

[X]

Subroutine [PROJ](proj.md) (category: Maths (Geometry))

Project the current ship or planet onto the screen

[X]

Subroutine [SUN (Part 1 of 4)](sun_part_1_of_4.md) (category: Drawing suns)

Draw the sun: Set up all the variables needed to draw the sun

[X]

Variable [TYPE](../workspace/zp.md#type) in workspace [ZP](../workspace/zp.md)

The current ship type

[PL2](pl2.md "Previous routine")[PL9 (Part 1 of 3)](pl9_part_1_of_3.md "Next routine")
