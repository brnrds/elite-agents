---
title: "The RUGAL (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/rugal.html"
---

[RUPLA (Game data)](rupla.md "Previous routine")[RUTOK (Game data)](rutok.md "Next routine")
    
    
           Name: RUGAL                                                   [Show more]
           Type: Variable
       Category: Text
        Summary: The criteria for systems with extended description overrides
      Deep dive: [Extended system descriptions](https://elite.bbcelite.com/deep_dives/extended_system_descriptions.html)
                 [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
                 [The Constrictor mission](https://elite.bbcelite.com/deep_dives/the_constrictor_mission.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-rugal)
     Variations: See [code variations](../../related/compare/main/variable/rugal.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [PDESC](../../main/subroutine/pdesc.md) uses RUGAL
    
    
    
    
    
    * * *
    
    
     This table contains the criteria for printing an extended description override
     for a system. The galaxy number is in bits 0-6, while bit 7 determines whether
     to show this token during mission 1 only (bit 7 is clear, i.e. a value of &0x
     in the table below), or all of the time (bit 7 is set, i.e. a value of &8x in
     the table below).
    
     In other words, Teorge, Arredi, Anreer and Lave have extended description
     overrides that are always shown, while the rest only appear when mission 1 is
     in progress.
    
     The three variables work as follows:
    
       * The RUPLA table contains the system numbers
    
       * The RUGAL table contains the galaxy numbers and mission criteria
    
       * The RUTOK table contains the extended token to display instead of the
         normal extended description if the criteria in RUPLA and RUGAL are met
    
     See the PDESC routine for details of how extended system descriptions work.
    
     This version of Elite contains an extended system description override for
     Lave that welcomes us to the seventeenth galaxy. This is never shown in-game,
     as the galaxy number in RUGAL has to be 16, and this cannot happen.
    
    
    
    
    .RUGAL
    
     EQUB &80               \ System 211, Galaxy 0                 Teorge = Token  1
     EQUB &00               \ System 150, Galaxy 0, Mission 1        Xeer = Token  2
     EQUB &00               \ System  36, Galaxy 0, Mission 1    Reesdice = Token  3
     EQUB &00               \ System  28, Galaxy 0, Mission 1       Arexe = Token  4
     EQUB &01               \ System 253, Galaxy 1, Mission 1      Errius = Token  5
     EQUB &01               \ System  79, Galaxy 1, Mission 1      Inbibe = Token  6
     EQUB &01               \ System  53, Galaxy 1, Mission 1       Ausar = Token  7
     EQUB &01               \ System 118, Galaxy 1, Mission 1      Usleri = Token  8
     EQUB &82               \ System 100, Galaxy 2                 Arredi = Token  9
     EQUB &01               \ System  32, Galaxy 1, Mission 1      Bebege = Token 10
     EQUB &01               \ System  68, Galaxy 1, Mission 1      Cearso = Token 11
     EQUB &01               \ System 164, Galaxy 1, Mission 1      Dicela = Token 12
     EQUB &01               \ System 220, Galaxy 1, Mission 1      Eringe = Token 13
     EQUB &01               \ System 106, Galaxy 1, Mission 1      Gexein = Token 14
     EQUB &01               \ System  16, Galaxy 1, Mission 1      Isarin = Token 15
     EQUB &01               \ System 162, Galaxy 1, Mission 1    Letibema = Token 16
     EQUB &01               \ System   3, Galaxy 1, Mission 1      Maisso = Token 17
     EQUB &01               \ System 107, Galaxy 1, Mission 1        Onen = Token 18
     EQUB &01               \ System  26, Galaxy 1, Mission 1      Ramaza = Token 19
     EQUB &01               \ System 192, Galaxy 1, Mission 1      Sosole = Token 20
     EQUB &01               \ System 184, Galaxy 1, Mission 1      Tivere = Token 21
     EQUB &01               \ System   5, Galaxy 1, Mission 1      Veriar = Token 22
     EQUB &02               \ System 101, Galaxy 2, Mission 1      Xeveon = Token 23
     EQUB &01               \ System 193, Galaxy 1, Mission 1      Orarra = Token 24
     EQUB &82               \ System  41, Galaxy 2                 Anreer = Token 25
     EQUB &90               \ System   7, Galaxy 16                  Lave = Token 26
    

[RUPLA (Game data)](rupla.md "Previous routine")[RUTOK (Game data)](rutok.md "Next routine")
