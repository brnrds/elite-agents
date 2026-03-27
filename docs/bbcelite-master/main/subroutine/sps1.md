---
title: "The SPS1 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/sps1.html"
---

[rfile](rfile.md "Previous routine")[TAS2](tas2.md "Next routine")
    
    
           Name: SPS1                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the vector to the planet and store it in XX15
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-sps1)
     References: This subroutine is called as follows:
                 * [COMPAS](compas.md) calls SPS1
                 * [Main flight loop (Part 9 of 16)](main_flight_loop_part_9_of_16.md) calls SPS1
                 * [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md) calls SPS1
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       SPS1+1               A BRK instruction
    
    
    
    
    .SPS1
    
     LDX #0                 \ Copy the two high bytes of the planet's x-coordinate
     JSR SPS3               \ into K3(2 1 0), separating out the sign bit into K3+2
    
     LDX #3                 \ Copy the two high bytes of the planet's y-coordinate
     JSR SPS3               \ into K3(5 4 3), separating out the sign bit into K3+5
    
     LDX #6                 \ Copy the two high bytes of the planet's z-coordinate
     JSR SPS3               \ into K3(8 7 6), separating out the sign bit into K3+8
    
                            \ Fall through into TAS2 to build XX15 from K3
    

[X]

Subroutine [SPS3](sps3.md) (category: Maths (Geometry))

Copy a space coordinate from the K% block into K3

[rfile](rfile.md "Previous routine")[TAS2](tas2.md "Next routine")
