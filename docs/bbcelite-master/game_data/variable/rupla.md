---
title: "The RUPLA (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/rupla.html"
---

[TKN1 (Game data)](tkn1.md "Previous routine")[RUGAL (Game data)](rugal.md "Next routine")
    
    
           Name: RUPLA                                                   [Show more]
           Type: Variable
       Category: Text
        Summary: System numbers that have extended description overrides
      Deep dive: [Extended system descriptions](https://elite.bbcelite.com/deep_dives/extended_system_descriptions.html)
                 [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
                 [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-rupla)
     Variations: See [code variations](../../related/compare/main/variable/rupla.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [PDESC](../../main/subroutine/pdesc.md) uses RUPLA
    
    
    
    
    
    * * *
    
    
     This table contains the extended token numbers to show as the specified
     system's extended description, if the criteria in the RUGAL table are met.
    
     The three variables work as follows:
    
       * The RUPLA table contains the system numbers
    
       * The RUGAL table contains the galaxy numbers and mission criteria
    
       * The RUTOK table contains the extended token to display instead of the
         normal extended description if the criteria in RUPLA and RUGAL are met
    
     See the PDESC routine for details of how extended system descriptions work.
    
     This version of Elite contains an extended system description override for
     Lave that welcomes us to the seventeenth galaxy. This is never shown in-game,
     as the galaxy number in RUGAL has to be 16, and this cannot happen.
    
    
    
    
    .RUPLA
    
     EQUB 211               \ System 211, Galaxy 0                 Teorge = Token  1
     EQUB 150               \ System 150, Galaxy 0, Mission 1        Xeer = Token  2
     EQUB 36                \ System  36, Galaxy 0, Mission 1    Reesdice = Token  3
     EQUB 28                \ System  28, Galaxy 0, Mission 1       Arexe = Token  4
     EQUB 253               \ System 253, Galaxy 1, Mission 1      Errius = Token  5
     EQUB 79                \ System  79, Galaxy 1, Mission 1      Inbibe = Token  6
     EQUB 53                \ System  53, Galaxy 1, Mission 1       Ausar = Token  7
     EQUB 118               \ System 118, Galaxy 1, Mission 1      Usleri = Token  8
     EQUB 100               \ System 100, Galaxy 2                 Arredi = Token  9
     EQUB 32                \ System  32, Galaxy 1, Mission 1      Bebege = Token 10
     EQUB 68                \ System  68, Galaxy 1, Mission 1      Cearso = Token 11
     EQUB 164               \ System 164, Galaxy 1, Mission 1      Dicela = Token 12
     EQUB 220               \ System 220, Galaxy 1, Mission 1      Eringe = Token 13
     EQUB 106               \ System 106, Galaxy 1, Mission 1      Gexein = Token 14
     EQUB 16                \ System  16, Galaxy 1, Mission 1      Isarin = Token 15
     EQUB 162               \ System 162, Galaxy 1, Mission 1    Letibema = Token 16
     EQUB 3                 \ System   3, Galaxy 1, Mission 1      Maisso = Token 17
     EQUB 107               \ System 107, Galaxy 1, Mission 1        Onen = Token 18
     EQUB 26                \ System  26, Galaxy 1, Mission 1      Ramaza = Token 19
     EQUB 192               \ System 192, Galaxy 1, Mission 1      Sosole = Token 20
     EQUB 184               \ System 184, Galaxy 1, Mission 1      Tivere = Token 21
     EQUB 5                 \ System   5, Galaxy 1, Mission 1      Veriar = Token 22
     EQUB 101               \ System 101, Galaxy 2, Mission 1      Xeveon = Token 23
     EQUB 193               \ System 193, Galaxy 1, Mission 1      Orarra = Token 24
     EQUB 41                \ System  41, Galaxy 2                 Anreer = Token 25
     EQUB 1                 \ System   7, Galaxy 16                  Lave = Token 26
    

[TKN1 (Game data)](tkn1.md "Previous routine")[RUGAL (Game data)](rugal.md "Next routine")
