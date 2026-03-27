---
title: "The Ze subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/ze.html"
---

[me2](me2.md "Previous routine")[DORND](dornd.md "Next routine")
    
    
           Name: Ze                                                      [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Initialise the INWK workspace to a fairly aggressive ship
      Deep dive: [Fixing ship positions](https://elite.bbcelite.com/deep_dives/elite-a_fixing_ship_positions.html)
                 [Aggression and hostility in ship tactics](https://elite.bbcelite.com/deep_dives/aggression_and_hostility_in_ship_tactics.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-ze)
     Variations: See [code variations](../../related/compare/main/subroutine/ze.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](death.md) calls Ze
                 * [GTHG](gthg.md) calls Ze
                 * [Main game loop (Part 3 of 6)](main_game_loop_part_3_of_6.md) calls Ze
                 * [Main game loop (Part 4 of 6)](main_game_loop_part_4_of_6.md) calls Ze
    
    
    
    
    
    * * *
    
    
     Specifically, this routine does the following:
    
       * Reset the INWK ship workspace
    
       * Set the ship to a fair distance away in all axes, in front of us but
         randomly up or down, left or right
    
       * Give the ship a 4% chance of having E.C.M.
    
       * Set the ship's aggression level to at least 32 out of 63, with AI enabled
    
     This routine also sets A, X, T1 and the C flag to random values.
    
     Note that because this routine uses the value of X returned by DORND, and X
     contains the value of A returned by the previous call to DORND, this routine
     does not necessarily set the new ship to a totally random location.
    
    
    
    
    .Ze
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace
    
     JSR DORND              \ Set A and X to random numbers
    
     STA T1                 \ Store A in T1
    
     AND #%10000000         \ Extract the sign of A and store in x_sign
     STA INWK+2
    
     TXA                    \ Extract the sign of X and store in y_sign
     AND #%10000000
     STA INWK+5
    
     LDA #25                \ Set x_hi = y_hi = z_hi = 25, a fair distance away
     STA INWK+1
     STA INWK+4
     STA INWK+7
    
     TXA                    \ Set the C flag if X >= 245 (4% chance)
     CMP #245
    
     ROL A                  \ Set bit 0 of A to the C flag (i.e. there's a 4%
                            \ chance of this ship having E.C.M.)
    
     ORA #%11000000         \ Set bits 6 and 7 of A, so the ship has AI (bit 7) and
                            \ an aggression level of at least 32 out of 63
    
     STA INWK+32            \ Store A in the AI flag of this ship
    
                            \ Fall through into DORND2 to set A, X and the C flag
                            \ randomly
    

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Variable [T1](../workspace/zp.md#t1) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Subroutine [ZINF](zinf.md) (category: Universe)

Reset the INWK workspace and orientation vectors

[me2](me2.md "Previous routine")[DORND](dornd.md "Next routine")
