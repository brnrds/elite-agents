---
title: "The ZP workspace - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/workspace/zp.html"
---

[MESS3 (Loader)](../../loader/variable/mess3.md "Previous routine")[XX3](xx3.md "Next routine")
    
    
           Name: ZP                                                      [Show more]
           Type: Workspace
        Address: &0000 to &00E3
       Category: Workspaces
        Summary: Lots of important variables are stored in the zero page workspace
                 as it is quicker and more space-efficient to access memory here
    
    
        Context: See this workspace [in context in the source code](../../all/workspaces.md#header-zp)
     Variations: See [code variations](../../related/compare/main/workspace/zp.md) for this workspace in the different versions
     References: This workspace is used as follows:
                 * [getzp](../subroutine/getzp.md) uses ZP
                 * [setzp](../subroutine/setzp.md) uses ZP
                 * [SWAPPZERO](../subroutine/swappzero.md) uses ZP
    
    
    
    
    
    
     ORG &0000              \ Set the assembly address to &0000
    
    .ZP
    
     SKIP 2                 \ These bytes appear to be unused
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [getzp](../subroutine/getzp.md)
                            \   * [setzp](../subroutine/setzp.md)
                            \   * [SWAPPZERO](../subroutine/swappzero.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    IF _COMPACT
    
    .MOS
    
     SKIP 1                 \ Determines whether we are running on a Master Compact
                            \
                            \   * 0 = This is a Master Compact
                            \
                            \   * &FF = This is not a Master Compact
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [IRQ1](../subroutine/irq1.md)
                            \   * [RDFIRE](../subroutine/rdfire.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \   * [TT17](../subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    ENDIF
    
    .RAND
    
     SKIP 4                 \ Four 8-bit seeds for the random number generation
                            \ system implemented in the DORND routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DORND](../subroutine/dornd.md)
                            \   * [Main flight loop (Part 1 of 16)](../subroutine/main_flight_loop_part_1_of_16.md)
                            \   * [PDESC](../subroutine/pdesc.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .T1
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../subroutine/add.md)
                            \   * [ADDK](../subroutine/addk.md)
                            \   * [ARCTAN](../subroutine/arctan.md)
                            \   * [DIALS (Part 1 of 4)](../subroutine/dials_part_1_of_4.md)
                            \   * [DIALS (Part 3 of 4)](../subroutine/dials_part_3_of_4.md)
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [DILX](../subroutine/dilx.md)
                            \   * [gnum](../subroutine/gnum.md)
                            \   * [LCASH](../subroutine/lcash.md)
                            \   * [MLS1](../subroutine/mls1.md)
                            \   * [MULT1](../subroutine/mult1.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [PIXEL](../subroutine/pixel.md)
                            \   * [refund](../subroutine/refund.md)
                            \   * [SFS1](../subroutine/sfs1.md)
                            \   * [TIS1](../subroutine/tis1.md)
                            \   * [TT102](../subroutine/tt102.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \   * [Ze](../subroutine/ze.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .T2
    
     SKIP 1                 \ This byte appears to be unused
    
    .T3
    
     SKIP 1                 \ This byte appears to be unused
    
    .T4
    
     SKIP 1                 \ This byte appears to be unused
    
    .SC
    
     SKIP 1                 \ Screen address (low byte)
                            \
                            \ Elite draws on-screen by poking bytes directly into
                            \ screen memory, and SC(1 0) is typically set to the
                            \ address of the character block containing the pixel
                            \ we want to draw
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../subroutine/chpr.md)
                            \   * [CLYNS](../subroutine/clyns.md)
                            \   * [CPIXK](../subroutine/cpixk.md)
                            \   * [DEEORS](../subroutine/deeors.md)
                            \   * [DETOK2](../subroutine/detok2.md)
                            \   * [DIALS (Part 1 of 4)](../subroutine/dials_part_1_of_4.md)
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [DIL2](../subroutine/dil2.md)
                            \   * [DILX](../subroutine/dilx.md)
                            \   * [ECBLB](../subroutine/ecblb.md)
                            \   * [HAL3](../subroutine/hal3.md)
                            \   * [HANGER](../subroutine/hanger.md)
                            \   * [HAS2](../subroutine/has2.md)
                            \   * [HAS3](../subroutine/has3.md)
                            \   * [HLOIN](../subroutine/hloin.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [KS2](../subroutine/ks2.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../subroutine/loinq_part_7_of_7.md)
                            \   * [MSBAR](../subroutine/msbar.md)
                            \   * [PIXEL](../subroutine/pixel.md)
                            \   * [SCAN](../subroutine/scan.md)
                            \   * [SPBLB](../subroutine/spblb.md)
                            \   * [TT26](../subroutine/tt26.md)
                            \   * [ZES1](../subroutine/zes1.md)
                            \   * [ZES2](../subroutine/zes2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SCH
    
     SKIP 1                 \ Screen address (high byte)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [MSBAR](../subroutine/msbar.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .P
    
     SKIP 4                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../subroutine/add.md)
                            \   * [ADDK](../subroutine/addk.md)
                            \   * [ARCTAN](../subroutine/arctan.md)
                            \   * [CHKON](../subroutine/chkon.md)
                            \   * [CHPR](../subroutine/chpr.md)
                            \   * [DIALS (Part 2 of 4)](../subroutine/dials_part_2_of_4.md)
                            \   * [DIALS (Part 3 of 4)](../subroutine/dials_part_3_of_4.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DVID3B2](../subroutine/dvid3b2.md)
                            \   * [DVID4](../subroutine/dvid4.md)
                            \   * [DVID4K](../subroutine/dvid4k.md)
                            \   * [DVIDT](../subroutine/dvidt.md)
                            \   * [FMLTU](../subroutine/fmltu.md)
                            \   * [GC2](../subroutine/gc2.md)
                            \   * [HANGER](../subroutine/hanger.md)
                            \   * [HITCH](../subroutine/hitch.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [KS3](../subroutine/ks3.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \   * [LOINQ (Part 1 of 7)](../subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../subroutine/loinq_part_7_of_7.md)
                            \   * [MLS1](../subroutine/mls1.md)
                            \   * [MLTU2](../subroutine/mltu2.md)
                            \   * [MLU2](../subroutine/mlu2.md)
                            \   * [MU1](../subroutine/mu1.md)
                            \   * [MU11](../subroutine/mu11.md)
                            \   * [MU6](../subroutine/mu6.md)
                            \   * [MULT1](../subroutine/mult1.md)
                            \   * [MULT12](../subroutine/mult12.md)
                            \   * [MULT3](../subroutine/mult3.md)
                            \   * [MUT3](../subroutine/mut3.md)
                            \   * [MV40](../subroutine/mv40.md)
                            \   * [MVEIT (Part 5 of 9)](../subroutine/mveit_part_5_of_9.md)
                            \   * [MVS4](../subroutine/mvs4.md)
                            \   * [MVS5](../subroutine/mvs5.md)
                            \   * [MVT6](../subroutine/mvt6.md)
                            \   * [NORM](../subroutine/norm.md)
                            \   * [PL9 (Part 2 of 3)](../subroutine/pl9_part_2_of_3.md)
                            \   * [PL9 (Part 3 of 3)](../subroutine/pl9_part_3_of_3.md)
                            \   * [PLANET](../subroutine/planet.md)
                            \   * [PLS1](../subroutine/pls1.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [PLS3](../subroutine/pls3.md)
                            \   * [PROJ](../subroutine/proj.md)
                            \   * [SPS2](../subroutine/sps2.md)
                            \   * [SQUA2](../subroutine/squa2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TIS3](../subroutine/tis3.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT151](../subroutine/tt151.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \   * [TT24](../subroutine/tt24.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XC
    
     SKIP 1                 \ The x-coordinate of the text cursor (i.e. the text
                            \ column), which can be from 0 to 32
                            \
                            \ A value of 0 denotes the leftmost column and 32 the
                            \ rightmost column, but because the top part of the
                            \ screen (the space view) has a border box that
                            \ clashes with columns 0 and 32, text is only shown
                            \ in columns 1-31
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 1 of 2)](../subroutine/br1_part_1_of_2.md)
                            \   * [CATS](../subroutine/cats.md)
                            \   * [CHPR](../subroutine/chpr.md)
                            \   * [DEATH](../subroutine/death.md)
                            \   * [dockEd](../subroutine/docked.md)
                            \   * [DOXC](../subroutine/doxc.md)
                            \   * [ee3](../subroutine/ee3.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [MESS](../subroutine/mess.md)
                            \   * [MT9](../subroutine/mt9.md)
                            \   * [plf2](../subroutine/plf2.md)
                            \   * [qv](../subroutine/qv.md)
                            \   * [STATUS](../subroutine/status.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TT151](../subroutine/tt151.md)
                            \   * [TT167](../subroutine/tt167.md)
                            \   * [TT208](../subroutine/tt208.md)
                            \   * [TT213](../subroutine/tt213.md)
                            \   * [TT22](../subroutine/tt22.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \   * [TTX66](../subroutine/ttx66.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .COL
    
     SKIP 1                 \ Temporary storage, used to store colour information
                            \ when drawing pixels in the dashboard
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BOMBOFF](../subroutine/bomboff.md)
                            \   * [BRP](../subroutine/brp.md)
                            \   * [CHPR](../subroutine/chpr.md)
                            \   * [CPIXK](../subroutine/cpixk.md)
                            \   * [DEATH](../subroutine/death.md)
                            \   * [DILX](../subroutine/dilx.md)
                            \   * [dockEd](../subroutine/docked.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DOT](../subroutine/dot.md)
                            \   * [ee3](../subroutine/ee3.md)
                            \   * [FLIP](../subroutine/flip.md)
                            \   * [gnum](../subroutine/gnum.md)
                            \   * [HLOIN](../subroutine/hloin.md)
                            \   * [HME2](../subroutine/hme2.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [LASLI](../subroutine/lasli.md)
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [LOINQ (Part 3 of 7)](../subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../subroutine/loinq_part_7_of_7.md)
                            \   * [me1](../subroutine/me1.md)
                            \   * [MESS](../subroutine/mess.md)
                            \   * [MT26](../subroutine/mt26.md)
                            \   * [MT29](../subroutine/mt29.md)
                            \   * [NLIN2](../subroutine/nlin2.md)
                            \   * [nWq](../subroutine/nwq.md)
                            \   * [PIXEL](../subroutine/pixel.md)
                            \   * [PLANET](../subroutine/planet.md)
                            \   * [SCAN](../subroutine/scan.md)
                            \   * [SIGHT](../subroutine/sight.md)
                            \   * [STARS](../subroutine/stars.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TRADEMODE](../subroutine/trademode.md)
                            \   * [TT103](../subroutine/tt103.md)
                            \   * [TT105](../subroutine/tt105.md)
                            \   * [TT128](../subroutine/tt128.md)
                            \   * [TT14](../subroutine/tt14.md)
                            \   * [TT22](../subroutine/tt22.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \   * [TTX66](../subroutine/ttx66.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .YC
    
     SKIP 1                 \ The y-coordinate of the text cursor (i.e. the text
                            \ row), which can be from 0 to 23
                            \
                            \ The screen actually has 31 character rows if you
                            \ include the dashboard, but the text printing routines
                            \ only work on the top part (the space view), so the
                            \ text cursor only goes up to a maximum of 23, the row
                            \ just before the screen splits
                            \
                            \ A value of 0 denotes the top row, but because the
                            \ top part of the screen has a border box that clashes
                            \ with row 0, text is always shown at row 1 or greater
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../subroutine/chpr.md)
                            \   * [CLYNS](../subroutine/clyns.md)
                            \   * [DEATH](../subroutine/death.md)
                            \   * [DOYC](../subroutine/doyc.md)
                            \   * [ee3](../subroutine/ee3.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [INCYC](../subroutine/incyc.md)
                            \   * [MESS](../subroutine/mess.md)
                            \   * [MT29](../subroutine/mt29.md)
                            \   * [NLIN](../subroutine/nlin.md)
                            \   * [qv](../subroutine/qv.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TT146](../subroutine/tt146.md)
                            \   * [TT167](../subroutine/tt167.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \   * [TTX66](../subroutine/ttx66.md)
                            \   * [TTX69](../subroutine/ttx69.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ17
    
     SKIP 1                 \ Contains a number of flags that affect how text tokens
                            \ are printed, particularly capitalisation:
                            \
                            \   * If all bits are set (255) then text printing is
                            \     disabled
                            \
                            \   * Bit 7: 0 = ALL CAPS
                            \            1 = Sentence Case, bit 6 determines the
                            \                case of the next letter to print
                            \
                            \   * Bit 6: 0 = print the next letter in upper case
                            \            1 = print the next letter in lower case
                            \
                            \   * Bits 0-5: If any of bits 0-5 are set, print in
                            \               lower case
                            \
                            \ So:
                            \
                            \   * QQ17 = 0 means case is set to ALL CAPS
                            \
                            \   * QQ17 = %10000000 means Sentence Case, currently
                            \            printing upper case
                            \
                            \   * QQ17 = %11000000 means Sentence Case, currently
                            \            printing lower case
                            \
                            \   * QQ17 = %11111111 means printing is disabled
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../subroutine/chpr.md)
                            \   * [CLYNS](../subroutine/clyns.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [MESS](../subroutine/mess.md)
                            \   * [MT17](../subroutine/mt17.md)
                            \   * [MT6](../subroutine/mt6.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TT102](../subroutine/tt102.md)
                            \   * [TT167](../subroutine/tt167.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \   * [TT27](../subroutine/tt27.md)
                            \   * [TT41](../subroutine/tt41.md)
                            \   * [TT46](../subroutine/tt46.md)
                            \   * [TT69](../subroutine/tt69.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K3
    
     SKIP 0                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHKON](../subroutine/chkon.md)
                            \   * [CHPR](../subroutine/chpr.md)
                            \   * [CIRCLE2](../subroutine/circle2.md)
                            \   * [DCS1](../subroutine/dcs1.md)
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [HFS2](../subroutine/hfs2.md)
                            \   * [PL9 (Part 3 of 3)](../subroutine/pl9_part_3_of_3.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [PROJ](../subroutine/proj.md)
                            \   * [SHPPT](../subroutine/shppt.md)
                            \   * [SPS3](../subroutine/sps3.md)
                            \   * [SPS4](../subroutine/sps4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [SUN (Part 4 of 4)](../subroutine/sun_part_4_of_4.md)
                            \   * [SWAPPZERO](../subroutine/swappzero.md)
                            \   * [TACTICS (Part 1 of 7)](../subroutine/tactics_part_1_of_7.md)
                            \   * [TACTICS (Part 3 of 7)](../subroutine/tactics_part_3_of_7.md)
                            \   * [TAS1](../subroutine/tas1.md)
                            \   * [TAS2](../subroutine/tas2.md)
                            \   * [TT128](../subroutine/tt128.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX2
    
     SKIP 14                \ Temporary storage, used to store the visibility of the
                            \ ship's faces during the ship-drawing routine at LL9
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 3 of 12)](../subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 4 of 12)](../subroutine/ll9_part_4_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K4
    
     SKIP 2                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [CHKON](../subroutine/chkon.md)
                            \   * [HFS2](../subroutine/hfs2.md)
                            \   * [PL9 (Part 3 of 3)](../subroutine/pl9_part_3_of_3.md)
                            \   * [PROJ](../subroutine/proj.md)
                            \   * [SHPPT](../subroutine/shppt.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [TT128](../subroutine/tt128.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX16
    
     SKIP 18                \ Temporary storage for a block of values, used in a
                            \ number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL51](../subroutine/ll51.md)
                            \   * [LL9 (Part 3 of 12)](../subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [PL9 (Part 2 of 3)](../subroutine/pl9_part_2_of_3.md)
                            \   * [PL9 (Part 3 of 3)](../subroutine/pl9_part_3_of_3.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [PLS5](../subroutine/pls5.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX0
    
     SKIP 2                 \ Temporary storage, used to store the address of a ship
                            \ blueprint. For example, it is used when we add a new
                            \ ship to the local bubble in routine NWSHP, and it
                            \ contains the address of the current ship's blueprint
                            \ as we loop through all the nearby ships in the main
                            \ flight loop
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HAS1](../subroutine/has1.md)
                            \   * [HITCH](../subroutine/hitch.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 2 of 12)](../subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 4 of 12)](../subroutine/ll9_part_4_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \   * [Main flight loop (Part 4 of 16)](../subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 8 of 16)](../subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [MVEIT (Part 4 of 9)](../subroutine/mveit_part_4_of_9.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [SFS1](../subroutine/sfs1.md)
                            \   * [SPIN](../subroutine/spin.md)
                            \   * [TACTICS (Part 2 of 7)](../subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 4 of 7)](../subroutine/tactics_part_4_of_7.md)
                            \   * [TACTICS (Part 6 of 7)](../subroutine/tactics_part_6_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .INF
    
     SKIP 2                 \ Temporary storage, typically used for storing the
                            \ address of a ship's data block, so it can be copied
                            \ to and from the internal workspace at INWK
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../subroutine/angry.md)
                            \   * [DEATH](../subroutine/death.md)
                            \   * [GINF](../subroutine/ginf.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [Main flight loop (Part 4 of 16)](../subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 6 of 16)](../subroutine/main_flight_loop_part_6_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [OOPS](../subroutine/oops.md)
                            \   * [SFS1](../subroutine/sfs1.md)
                            \   * [TACTICS (Part 4 of 7)](../subroutine/tactics_part_4_of_7.md)
                            \   * [WPSHPS](../subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .V
    
     SKIP 2                 \ Temporary storage, typically used for storing an
                            \ address pointer
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DETOK](../subroutine/detok.md)
                            \   * [DETOK2](../subroutine/detok2.md)
                            \   * [DETOK3](../subroutine/detok3.md)
                            \   * [ex](../subroutine/ex.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../subroutine/ll9_part_11_of_12.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TACTICS (Part 1 of 7)](../subroutine/tactics_part_1_of_7.md)
                            \   * [TAS1](../subroutine/tas1.md)
                            \   * [VCSU1](../subroutine/vcsu1.md)
                            \   * [VCSUB](../subroutine/vcsub.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX
    
     SKIP 2                 \ Temporary storage, typically used for storing a 16-bit
                            \ x-coordinate
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [MLS2](../subroutine/mls2.md)
                            \   * [MUT1](../subroutine/mut1.md)
                            \   * [MUT2](../subroutine/mut2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .YY
    
     SKIP 2                 \ Temporary storage, typically used for storing a 16-bit
                            \ y-coordinate
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [EDGES](../subroutine/edges.md)
                            \   * [PIX1](../subroutine/pix1.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [SUN (Part 2 of 4)](../subroutine/sun_part_2_of_4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [SUN (Part 4 of 4)](../subroutine/sun_part_4_of_4.md)
                            \   * [WPLS](../subroutine/wpls.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .SUNX
    
     SKIP 2                 \ The 16-bit x-coordinate of the vertical centre axis
                            \ of the sun (which might be off-screen)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [SUN (Part 2 of 4)](../subroutine/sun_part_2_of_4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [SUN (Part 4 of 4)](../subroutine/sun_part_4_of_4.md)
                            \   * [WPLS](../subroutine/wpls.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BETA
    
     SKIP 1                 \ The current pitch angle beta, which is reduced from
                            \ JSTY to a sign-magnitude value between -8 and +8
                            \
                            \ This describes how fast we are pitching our ship, and
                            \ determines how fast the universe pitches around us
                            \
                            \ The sign bit is also stored in BET2, while the
                            \ opposite sign is stored in BET2+1
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 2 of 4)](../subroutine/dials_part_2_of_4.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MV40](../subroutine/mv40.md)
                            \   * [MVS4](../subroutine/mvs4.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [RESET](../subroutine/reset.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BET1
    
     SKIP 1                 \ The magnitude of the pitch angle beta, i.e. |beta|,
                            \ which is a positive value between 0 and 8
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 2 of 4)](../subroutine/dials_part_2_of_4.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MVEIT (Part 5 of 9)](../subroutine/mveit_part_5_of_9.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ22
    
     SKIP 2                 \ The two hyperspace countdown counters
                            \
                            \ Before a hyperspace jump, both QQ22 and QQ22+1 are
                            \ set to 15
                            \
                            \ QQ22 is an internal counter that counts down by 1
                            \ each time TT102 is called, which happens every
                            \ iteration of the main game loop. When it reaches
                            \ zero, the on-screen counter in QQ22+1 gets
                            \ decremented, and QQ22 gets set to 5 and the countdown
                            \ continues (so the first tick of the hyperspace counter
                            \ takes 15 iterations to happen, but subsequent ticks
                            \ take 5 iterations each)
                            \
                            \ QQ22+1 contains the number that's shown on-screen
                            \ during the countdown. It counts down from 15 to 1, and
                            \ when it hits 0, the hyperspace engines kick in
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [TT102](../subroutine/tt102.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \   * [wW](../subroutine/ww.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ECMA
    
     SKIP 1                 \ The E.C.M. countdown timer, which determines whether
                            \ an E.C.M. system is currently operating:
                            \
                            \   * 0 = E.C.M. is off
                            \
                            \   * Non-zero = E.C.M. is on and is counting down
                            \
                            \ The counter starts at 32 when an E.C.M. is activated,
                            \ either by us or by an opponent, and it decreases by 1
                            \ in each iteration of the main flight loop until it
                            \ reaches zero, at which point the E.C.M. switches off.
                            \ Only one E.C.M. can be active at any one time, so
                            \ there is only one counter
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ECBLB2](../subroutine/ecblb2.md)
                            \   * [ECMOF](../subroutine/ecmof.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../subroutine/main_flight_loop_part_16_of_16.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [TACTICS (Part 1 of 7)](../subroutine/tactics_part_1_of_7.md)
                            \   * [TACTICS (Part 5 of 7)](../subroutine/tactics_part_5_of_7.md)
                            \   * [TACTICS (Part 6 of 7)](../subroutine/tactics_part_6_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ALP1
    
     SKIP 1                 \ Magnitude of the roll angle alpha, i.e. |alpha|,
                            \ which is a positive value between 0 and 31
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 2 of 4)](../subroutine/dials_part_2_of_4.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MLS1](../subroutine/mls1.md)
                            \   * [MUT3](../subroutine/mut3.md)
                            \   * [MVEIT (Part 5 of 9)](../subroutine/mveit_part_5_of_9.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ALP2
    
     SKIP 2                 \ Bit 7 of ALP2 = sign of the roll angle in ALPHA
                            \
                            \ Bit 7 of ALP2+1 = opposite sign to ALP2 and ALPHA
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 2 of 4)](../subroutine/dials_part_2_of_4.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MVEIT (Part 5 of 9)](../subroutine/mveit_part_5_of_9.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX15
    
     SKIP 0                 \ Temporary storage, typically used for storing screen
                            \ coordinates in line-drawing routines
                            \
                            \ There are six bytes of storage, from XX15 TO XX15+5.
                            \ The first four bytes have the following aliases:
                            \
                            \   X1 = XX15
                            \   Y1 = XX15+1
                            \   X2 = XX15+2
                            \   Y2 = XX15+3
                            \
                            \ These are typically used for describing lines in terms
                            \ of screen coordinates, i.e. (X1, Y1) to (X2, Y2)
                            \
                            \ The last two bytes of XX15 do not have aliases
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [BPRNT](../subroutine/bprnt.md)
                            \   * [DIALS (Part 3 of 4)](../subroutine/dials_part_3_of_4.md)
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [HALL](../subroutine/hall.md)
                            \   * [HAS1](../subroutine/has1.md)
                            \   * [LL118](../subroutine/ll118.md)
                            \   * [LL120](../subroutine/ll120.md)
                            \   * [LL145 (Part 1 of 4)](../subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 2 of 4)](../subroutine/ll145_part_2_of_4.md)
                            \   * [LL145 (Part 3 of 4)](../subroutine/ll145_part_3_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../subroutine/ll145_part_4_of_4.md)
                            \   * [LL51](../subroutine/ll51.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \   * [LL9 (Part 12 of 12)](../subroutine/ll9_part_12_of_12.md)
                            \   * [Main flight loop (Part 9 of 16)](../subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [NORM](../subroutine/norm.md)
                            \   * [SP2](../subroutine/sp2.md)
                            \   * [TAS2](../subroutine/tas2.md)
                            \   * [TAS3](../subroutine/tas3.md)
                            \   * [TAS4](../subroutine/tas4.md)
                            \   * [TAS6](../subroutine/tas6.md)
                            \   * [TIDY](../subroutine/tidy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .X1
    
     SKIP 1                 \ Temporary storage, typically used for x-coordinates in
                            \ the line-drawing routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [BOMBOFF](../subroutine/bomboff.md)
                            \   * [CPIXK](../subroutine/cpixk.md)
                            \   * [DOT](../subroutine/dot.md)
                            \   * [DVLOIN](../subroutine/dvloin.md)
                            \   * [EDGES](../subroutine/edges.md)
                            \   * [FLIP](../subroutine/flip.md)
                            \   * [HLOIN](../subroutine/hloin.md)
                            \   * [LASLI](../subroutine/lasli.md)
                            \   * [LOINQ (Part 1 of 7)](../subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [LSPUT](../subroutine/lsput.md)
                            \   * [NLIN2](../subroutine/nlin2.md)
                            \   * [nWq](../subroutine/nwq.md)
                            \   * [PIXEL2](../subroutine/pixel2.md)
                            \   * [SCAN](../subroutine/scan.md)
                            \   * [SHPPT](../subroutine/shppt.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TT15](../subroutine/tt15.md)
                            \   * [TTX66](../subroutine/ttx66.md)
                            \   * [WPLS2](../subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Y1
    
     SKIP 1                 \ Temporary storage, typically used for y-coordinates in
                            \ line-drawing routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [BOMBOFF](../subroutine/bomboff.md)
                            \   * [CPIXK](../subroutine/cpixk.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DOT](../subroutine/dot.md)
                            \   * [DVLOIN](../subroutine/dvloin.md)
                            \   * [FLIP](../subroutine/flip.md)
                            \   * [HLOIN](../subroutine/hloin.md)
                            \   * [HLOIN2](../subroutine/hloin2.md)
                            \   * [LASLI](../subroutine/lasli.md)
                            \   * [LOINQ (Part 1 of 7)](../subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [LSPUT](../subroutine/lsput.md)
                            \   * [MLU1](../subroutine/mlu1.md)
                            \   * [NLIN2](../subroutine/nlin2.md)
                            \   * [nWq](../subroutine/nwq.md)
                            \   * [PIXEL2](../subroutine/pixel2.md)
                            \   * [SCAN](../subroutine/scan.md)
                            \   * [SHPPT](../subroutine/shppt.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TT15](../subroutine/tt15.md)
                            \   * [TTX66](../subroutine/ttx66.md)
                            \   * [WPLS2](../subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .X2
    
     SKIP 1                 \ Temporary storage, typically used for x-coordinates in
                            \ the line-drawing routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [BOMBOFF](../subroutine/bomboff.md)
                            \   * [DVLOIN](../subroutine/dvloin.md)
                            \   * [EDGES](../subroutine/edges.md)
                            \   * [HLOIN](../subroutine/hloin.md)
                            \   * [LASLI](../subroutine/lasli.md)
                            \   * [LOINQ (Part 1 of 7)](../subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [LSPUT](../subroutine/lsput.md)
                            \   * [NLIN2](../subroutine/nlin2.md)
                            \   * [SHPPT](../subroutine/shppt.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TT15](../subroutine/tt15.md)
                            \   * [TTX66](../subroutine/ttx66.md)
                            \   * [WPLS2](../subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Y2
    
     SKIP 1                 \ Temporary storage, typically used for y-coordinates in
                            \ line-drawing routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [BOMBOFF](../subroutine/bomboff.md)
                            \   * [DVLOIN](../subroutine/dvloin.md)
                            \   * [LASLI](../subroutine/lasli.md)
                            \   * [LOINQ (Part 1 of 7)](../subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [LSPUT](../subroutine/lsput.md)
                            \   * [SCAN](../subroutine/scan.md)
                            \   * [SHPPT](../subroutine/shppt.md)
                            \   * [TT15](../subroutine/tt15.md)
                            \   * [TTX66](../subroutine/ttx66.md)
                            \   * [WPLS2](../subroutine/wpls2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     SKIP 2                 \ The last two bytes of the XX15 block
    
    .XX12
    
     SKIP 6                 \ Temporary storage for a block of values, used in a
                            \ number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [BOMBOFF](../subroutine/bomboff.md)
                            \   * [LL129](../subroutine/ll129.md)
                            \   * [LL145 (Part 1 of 4)](../subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 2 of 4)](../subroutine/ll145_part_2_of_4.md)
                            \   * [LL145 (Part 3 of 4)](../subroutine/ll145_part_3_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../subroutine/ll145_part_4_of_4.md)
                            \   * [LL51](../subroutine/ll51.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \   * [LSPUT](../subroutine/lsput.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K
    
     SKIP 4                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BPRNT](../subroutine/bprnt.md)
                            \   * [CHKON](../subroutine/chkon.md)
                            \   * [CIRCLE](../subroutine/circle.md)
                            \   * [csh](../subroutine/csh.md)
                            \   * [DIALS (Part 1 of 4)](../subroutine/dials_part_1_of_4.md)
                            \   * [DIALS (Part 3 of 4)](../subroutine/dials_part_3_of_4.md)
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [DILX](../subroutine/dilx.md)
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [DVID3B2](../subroutine/dvid3b2.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [FMLTU2](../subroutine/fmltu2.md)
                            \   * [HFS2](../subroutine/hfs2.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [MAS1](../subroutine/mas1.md)
                            \   * [MU5](../subroutine/mu5.md)
                            \   * [MULT3](../subroutine/mult3.md)
                            \   * [MV40](../subroutine/mv40.md)
                            \   * [MVS5](../subroutine/mvs5.md)
                            \   * [MVT3](../subroutine/mvt3.md)
                            \   * [PL9 (Part 1 of 3)](../subroutine/pl9_part_1_of_3.md)
                            \   * [PL9 (Part 2 of 3)](../subroutine/pl9_part_2_of_3.md)
                            \   * [PLANET](../subroutine/planet.md)
                            \   * [PLS1](../subroutine/pls1.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [PLS3](../subroutine/pls3.md)
                            \   * [PLS6](../subroutine/pls6.md)
                            \   * [PROJ](../subroutine/proj.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [SVE](../subroutine/sve.md)
                            \   * [TAS1](../subroutine/tas1.md)
                            \   * [TT11](../subroutine/tt11.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT14](../subroutine/tt14.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LAS
    
     SKIP 1                 \ Contains the laser power of the laser fitted to the
                            \ current space view (or 0 if there is no laser fitted
                            \ to the current view)
                            \
                            \ This gets set to bits 0-6 of the laser power byte from
                            \ the commander data block, which contains the laser's
                            \ power (bit 7 doesn't denote laser power, just whether
                            \ or not the laser pulses, so that is not stored here)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../subroutine/main_flight_loop_part_11_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .MSTG
    
     SKIP 1                 \ The current missile lock target
                            \
                            \   * &FF = no target
                            \
                            \   * 1-12 = the slot number of the ship that our
                            \            missile is locked onto
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ABORT2](../subroutine/abort2.md)
                            \   * [FRMIS](../subroutine/frmis.md)
                            \   * [FRS1](../subroutine/frs1.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [RES2](../subroutine/res2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DL
    
     SKIP 1                 \ Vertical sync flag
                            \
                            \ DL gets set to 30 every time we reach vertical sync on
                            \ the video system, which happens 50 times a second
                            \ (50Hz). The WSCAN routine uses this to pause until the
                            \ vertical sync, by setting DL to 0 and then monitoring
                            \ its value until it changes to 30
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [IRQ1](../subroutine/irq1.md)
                            \   * [WSCAN](../subroutine/wscan.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSP
    
     SKIP 1                 \ The ball line heap pointer, which contains the number
                            \ of the first free byte after the end of the LSX2 and
                            \ LSY2 heaps
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [HFS2](../subroutine/hfs2.md)
                            \   * [TT128](../subroutine/tt128.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \   * [WP1](../subroutine/wp1.md)
                            \   * [WPLS2](../subroutine/wpls2.md)
                            \   * [WPSHPS](../subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ15
    
     SKIP 6                 \ The three 16-bit seeds for the selected system, i.e.
                            \ the one in the crosshairs in the Short-range Chart
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [cpl](../subroutine/cpl.md)
                            \   * [Ghy](../subroutine/ghy.md)
                            \   * [HME2](../subroutine/hme2.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [PDESC](../subroutine/pdesc.md)
                            \   * [SOLAR](../subroutine/solar.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT22](../subroutine/tt22.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \   * [TT24](../subroutine/tt24.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \   * [TT54](../subroutine/tt54.md)
                            \   * [TT81](../subroutine/tt81.md)
                            \   * [ypl](../subroutine/ypl.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K5
    
     SKIP 0                 \ Temporary storage used to store segment coordinates
                            \ across successive calls to BLINE, the ball line
                            \ routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX18
    
     SKIP 4                 \ Temporary storage used to store coordinates in the
                            \ LL9 ship-drawing routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 3 of 12)](../subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K6
    
     SKIP 5                 \ Temporary storage, typically used for storing
                            \ coordinates during vector calculations
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [CIRCLE2](../subroutine/circle2.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ19
    
     SKIP 6                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [cpl](../subroutine/cpl.md)
                            \   * [GVL](../subroutine/gvl.md)
                            \   * [SIGHT](../subroutine/sight.md)
                            \   * [TT103](../subroutine/tt103.md)
                            \   * [TT105](../subroutine/tt105.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT123](../subroutine/tt123.md)
                            \   * [TT128](../subroutine/tt128.md)
                            \   * [TT14](../subroutine/tt14.md)
                            \   * [TT15](../subroutine/tt15.md)
                            \   * [TT151](../subroutine/tt151.md)
                            \   * [TT152](../subroutine/tt152.md)
                            \   * [TT16](../subroutine/tt16.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT22](../subroutine/tt22.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \   * [var](../subroutine/var.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .BET2
    
     SKIP 2                 \ Bit 7 of BET2 = sign of the pitch angle in BETA
                            \
                            \ Bit 7 of BET2+1 = opposite sign to BET2 and BETA
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MVEIT (Part 5 of 9)](../subroutine/mveit_part_5_of_9.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DELTA
    
     SKIP 1                 \ Our current speed, in the range 1-40
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DEATH](../subroutine/death.md)
                            \   * [DIALS (Part 1 of 4)](../subroutine/dials_part_1_of_4.md)
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [DV41](../subroutine/dv41.md)
                            \   * [FRS1](../subroutine/frs1.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 9 of 16)](../subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [Main flight loop (Part 10 of 16)](../subroutine/main_flight_loop_part_10_of_16.md)
                            \   * [MVEIT (Part 6 of 9)](../subroutine/mveit_part_6_of_9.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TT110](../subroutine/tt110.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .DELT4
    
     SKIP 2                 \ Our current speed * 64 as a 16-bit value
                            \
                            \ This is stored as DELT4(1 0), so the high byte in
                            \ DELT4+1 therefore contains our current speed / 4
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .U
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../subroutine/add.md)
                            \   * [ADDK](../subroutine/addk.md)
                            \   * [BPRNT](../subroutine/bprnt.md)
                            \   * [csh](../subroutine/csh.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [LL61](../subroutine/ll61.md)
                            \   * [LL62](../subroutine/ll62.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [PLS3](../subroutine/pls3.md)
                            \   * [TAS1](../subroutine/tas1.md)
                            \   * [TT11](../subroutine/tt11.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Q
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ARCTAN](../subroutine/arctan.md)
                            \   * [DIALS (Part 3 of 4)](../subroutine/dials_part_3_of_4.md)
                            \   * [DIL2](../subroutine/dil2.md)
                            \   * [DILX](../subroutine/dilx.md)
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DV41](../subroutine/dv41.md)
                            \   * [DVID3B2](../subroutine/dvid3b2.md)
                            \   * [DVID4](../subroutine/dvid4.md)
                            \   * [DVID4K](../subroutine/dvid4k.md)
                            \   * [DVIDT](../subroutine/dvidt.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [FMLTU](../subroutine/fmltu.md)
                            \   * [FMLTU2](../subroutine/fmltu2.md)
                            \   * [gnum](../subroutine/gnum.md)
                            \   * [HANGER](../subroutine/hanger.md)
                            \   * [HAS1](../subroutine/has1.md)
                            \   * [LL120](../subroutine/ll120.md)
                            \   * [LL123](../subroutine/ll123.md)
                            \   * [LL129](../subroutine/ll129.md)
                            \   * [LL145 (Part 3 of 4)](../subroutine/ll145_part_3_of_4.md)
                            \   * [LL28](../subroutine/ll28.md)
                            \   * [LL38](../subroutine/ll38.md)
                            \   * [LL5](../subroutine/ll5.md)
                            \   * [LL51](../subroutine/ll51.md)
                            \   * [LL61](../subroutine/ll61.md)
                            \   * [LL9 (Part 3 of 12)](../subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [LOINQ (Part 1 of 7)](../subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [MLTU2](../subroutine/mltu2.md)
                            \   * [MULT1](../subroutine/mult1.md)
                            \   * [MULT3](../subroutine/mult3.md)
                            \   * [MULTU](../subroutine/multu.md)
                            \   * [MV40](../subroutine/mv40.md)
                            \   * [MVEIT (Part 3 of 9)](../subroutine/mveit_part_3_of_9.md)
                            \   * [MVS4](../subroutine/mvs4.md)
                            \   * [MVS5](../subroutine/mvs5.md)
                            \   * [NORM](../subroutine/norm.md)
                            \   * [OUTK](../subroutine/outk.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [PLS3](../subroutine/pls3.md)
                            \   * [PLS4](../subroutine/pls4.md)
                            \   * [SPS2](../subroutine/sps2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TAS3](../subroutine/tas3.md)
                            \   * [TAS4](../subroutine/tas4.md)
                            \   * [TIDY](../subroutine/tidy.md)
                            \   * [TIS1](../subroutine/tis1.md)
                            \   * [TIS2](../subroutine/tis2.md)
                            \   * [TIS3](../subroutine/tis3.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \   * [TT24](../subroutine/tt24.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .R
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../subroutine/add.md)
                            \   * [ADDK](../subroutine/addk.md)
                            \   * [ARCTAN](../subroutine/arctan.md)
                            \   * [CPIXK](../subroutine/cpixk.md)
                            \   * [DCS1](../subroutine/dcs1.md)
                            \   * [DIALS (Part 2 of 4)](../subroutine/dials_part_2_of_4.md)
                            \   * [DILX](../subroutine/dilx.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DVID3B2](../subroutine/dvid3b2.md)
                            \   * [DVID4](../subroutine/dvid4.md)
                            \   * [gnum](../subroutine/gnum.md)
                            \   * [HANGER](../subroutine/hanger.md)
                            \   * [HAS1](../subroutine/has1.md)
                            \   * [HITCH](../subroutine/hitch.md)
                            \   * [HLOIN](../subroutine/hloin.md)
                            \   * [LL118](../subroutine/ll118.md)
                            \   * [LL120](../subroutine/ll120.md)
                            \   * [LL123](../subroutine/ll123.md)
                            \   * [LL129](../subroutine/ll129.md)
                            \   * [LL145 (Part 4 of 4)](../subroutine/ll145_part_4_of_4.md)
                            \   * [LL28](../subroutine/ll28.md)
                            \   * [LL38](../subroutine/ll38.md)
                            \   * [LL5](../subroutine/ll5.md)
                            \   * [LL51](../subroutine/ll51.md)
                            \   * [LL61](../subroutine/ll61.md)
                            \   * [LL62](../subroutine/ll62.md)
                            \   * [LL9 (Part 3 of 12)](../subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [LOINQ (Part 2 of 7)](../subroutine/loinq_part_2_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 5 of 7)](../subroutine/loinq_part_5_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../subroutine/loinq_part_7_of_7.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [MAS3](../subroutine/mas3.md)
                            \   * [MLS2](../subroutine/mls2.md)
                            \   * [MULT12](../subroutine/mult12.md)
                            \   * [MULT3](../subroutine/mult3.md)
                            \   * [MUT1](../subroutine/mut1.md)
                            \   * [MVEIT (Part 3 of 9)](../subroutine/mveit_part_3_of_9.md)
                            \   * [MVEIT (Part 6 of 9)](../subroutine/mveit_part_6_of_9.md)
                            \   * [MVS4](../subroutine/mvs4.md)
                            \   * [MVS5](../subroutine/mvs5.md)
                            \   * [MVT1](../subroutine/mvt1.md)
                            \   * [NORM](../subroutine/norm.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [SCAN](../subroutine/scan.md)
                            \   * [SFS2](../subroutine/sfs2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TAS3](../subroutine/tas3.md)
                            \   * [TAS4](../subroutine/tas4.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT219](../subroutine/tt219.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .S
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../subroutine/add.md)
                            \   * [ADDK](../subroutine/addk.md)
                            \   * [BPRNT](../subroutine/bprnt.md)
                            \   * [DIALS (Part 2 of 4)](../subroutine/dials_part_2_of_4.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DVID3B2](../subroutine/dvid3b2.md)
                            \   * [gnum](../subroutine/gnum.md)
                            \   * [HANGER](../subroutine/hanger.md)
                            \   * [HITCH](../subroutine/hitch.md)
                            \   * [LL118](../subroutine/ll118.md)
                            \   * [LL120](../subroutine/ll120.md)
                            \   * [LL123](../subroutine/ll123.md)
                            \   * [LL129](../subroutine/ll129.md)
                            \   * [LL145 (Part 3 of 4)](../subroutine/ll145_part_3_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../subroutine/ll145_part_4_of_4.md)
                            \   * [LL38](../subroutine/ll38.md)
                            \   * [LL5](../subroutine/ll5.md)
                            \   * [LL51](../subroutine/ll51.md)
                            \   * [LL61](../subroutine/ll61.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LOINQ (Part 1 of 7)](../subroutine/loinq_part_1_of_7.md)
                            \   * [LOINQ (Part 3 of 7)](../subroutine/loinq_part_3_of_7.md)
                            \   * [LOINQ (Part 4 of 7)](../subroutine/loinq_part_4_of_7.md)
                            \   * [LOINQ (Part 6 of 7)](../subroutine/loinq_part_6_of_7.md)
                            \   * [LOINQ (Part 7 of 7)](../subroutine/loinq_part_7_of_7.md)
                            \   * [MLS2](../subroutine/mls2.md)
                            \   * [MULT12](../subroutine/mult12.md)
                            \   * [MUT2](../subroutine/mut2.md)
                            \   * [MVS4](../subroutine/mvs4.md)
                            \   * [MVS5](../subroutine/mvs5.md)
                            \   * [MVT1](../subroutine/mvt1.md)
                            \   * [MVT3](../subroutine/mvt3.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [TAS3](../subroutine/tas3.md)
                            \   * [TAS4](../subroutine/tas4.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .T
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ADD](../subroutine/add.md)
                            \   * [ADDK](../subroutine/addk.md)
                            \   * [ARCTAN](../subroutine/arctan.md)
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [BPRNT](../subroutine/bprnt.md)
                            \   * [BUMP2](../subroutine/bump2.md)
                            \   * [CIRCLE2](../subroutine/circle2.md)
                            \   * [cpl](../subroutine/cpl.md)
                            \   * [DEEORS](../subroutine/deeors.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DVID3B2](../subroutine/dvid3b2.md)
                            \   * [DVIDT](../subroutine/dvidt.md)
                            \   * [EDGES](../subroutine/edges.md)
                            \   * [gnum](../subroutine/gnum.md)
                            \   * [HALL](../subroutine/hall.md)
                            \   * [HANGER](../subroutine/hanger.md)
                            \   * [HLOIN](../subroutine/hloin.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [LL120](../subroutine/ll120.md)
                            \   * [LL123](../subroutine/ll123.md)
                            \   * [LL145 (Part 3 of 4)](../subroutine/ll145_part_3_of_4.md)
                            \   * [LL5](../subroutine/ll5.md)
                            \   * [LL51](../subroutine/ll51.md)
                            \   * [LL9 (Part 2 of 12)](../subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [Main game loop (Part 3 of 6)](../subroutine/main_game_loop_part_3_of_6.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \   * [MLS1](../subroutine/mls1.md)
                            \   * [MSBAR](../subroutine/msbar.md)
                            \   * [MU11](../subroutine/mu11.md)
                            \   * [MULT1](../subroutine/mult1.md)
                            \   * [MULT3](../subroutine/mult3.md)
                            \   * [MV40](../subroutine/mv40.md)
                            \   * [MVS5](../subroutine/mvs5.md)
                            \   * [MVT1](../subroutine/mvt1.md)
                            \   * [MVT3](../subroutine/mvt3.md)
                            \   * [NORM](../subroutine/norm.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [OOPS](../subroutine/oops.md)
                            \   * [PIXEL2](../subroutine/pixel2.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [REDU2](../subroutine/redu2.md)
                            \   * [SP2](../subroutine/sp2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TACTICS (Part 5 of 7)](../subroutine/tactics_part_5_of_7.md)
                            \   * [TIS1](../subroutine/tis1.md)
                            \   * [TIS2](../subroutine/tis2.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XSAV
    
     SKIP 1                 \ Temporary storage for saving the value of the X
                            \ register, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HAS1](../subroutine/has1.md)
                            \   * [KS1](../subroutine/ks1.md)
                            \   * [Main flight loop (Part 4 of 16)](../subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [MVEIT (Part 1 of 9)](../subroutine/mveit_part_1_of_9.md)
                            \   * [MVEIT (Part 2 of 9)](../subroutine/mveit_part_2_of_9.md)
                            \   * [TT22](../subroutine/tt22.md)
                            \   * [WPSHPS](../subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .YSAV
    
     SKIP 1                 \ Temporary storage for saving the value of the Y
                            \ register, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HLOIN](../subroutine/hloin.md)
                            \   * [LOIN](../subroutine/loin.md)
                            \   * [TT217](../subroutine/tt217.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX17
    
     SKIP 1                 \ Temporary storage, used in BPRNT to store the number
                            \ of characters to print, and as the edge counter in the
                            \ main ship-drawing routine
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BPRNT](../subroutine/bprnt.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../subroutine/ll9_part_11_of_12.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .W
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHPR](../subroutine/chpr.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ11
    
     SKIP 1                 \ The type of the current view:
                            \
                            \   0   = Space view
                            \         Death screen
                            \   1   = Data on System screen (red key f6)
                            \         Get commander name ("@", save/load commander)
                            \         In-system jump just arrived ("J")
                            \   2   = Buy Cargo screen (red key f1)
                            \   3   = Mis-jump just arrived (witchspace)
                            \   4   = Sell Cargo screen (red key f2)
                            \   8   = Status Mode screen (red key f8)
                            \         Inventory screen (red key f9)
                            \   13  = Rotating ship view (title or mission screen)
                            \   16  = Market Price screen (red key f7)
                            \   32  = Equip Ship screen (red key f3)
                            \   64  = Long-range Chart (red key f4)
                            \   128 = Short-range Chart (red key f5)
                            \   255 = Launch view
                            \
                            \ This value is typically set by calling routine TT66
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BOMBOFF](../subroutine/bomboff.md)
                            \   * [DEATH](../subroutine/death.md)
                            \   * [DFAULT](../subroutine/dfault.md)
                            \   * [ESCAPE](../subroutine/escape.md)
                            \   * [HFS2](../subroutine/hfs2.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [LASLI](../subroutine/lasli.md)
                            \   * [LOOK1](../subroutine/look1.md)
                            \   * [Main flight loop (Part 11 of 16)](../subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 16 of 16)](../subroutine/main_flight_loop_part_16_of_16.md)
                            \   * [Main game loop (Part 5 of 6)](../subroutine/main_game_loop_part_5_of_6.md)
                            \   * [me2](../subroutine/me2.md)
                            \   * [MESS](../subroutine/mess.md)
                            \   * [NWSTARS](../subroutine/nwstars.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TT102](../subroutine/tt102.md)
                            \   * [TT103](../subroutine/tt103.md)
                            \   * [TT110](../subroutine/tt110.md)
                            \   * [TT14](../subroutine/tt14.md)
                            \   * [TT15](../subroutine/tt15.md)
                            \   * [TT17](../subroutine/tt17.md)
                            \   * [TT18](../subroutine/tt18.md)
                            \   * [TT210](../subroutine/tt210.md)
                            \   * [TT66](../subroutine/tt66.md)
                            \   * [TTX66K](../subroutine/ttx66k.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ZZ
    
     SKIP 1                 \ Temporary storage, typically used for distance values
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [FLIP](../subroutine/flip.md)
                            \   * [nWq](../subroutine/nwq.md)
                            \   * [PDESC](../subroutine/pdesc.md)
                            \   * [PIXEL](../subroutine/pixel.md)
                            \   * [refund](../subroutine/refund.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT22](../subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX13
    
     SKIP 1                 \ Temporary storage, typically used in the line-drawing
                            \ routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [EQSHP](../subroutine/eqshp.md)
                            \   * [LL145 (Part 1 of 4)](../subroutine/ll145_part_1_of_4.md)
                            \   * [LL145 (Part 2 of 4)](../subroutine/ll145_part_2_of_4.md)
                            \   * [LL145 (Part 4 of 4)](../subroutine/ll145_part_4_of_4.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .MCNT
    
     SKIP 1                 \ The main loop counter
                            \
                            \ This counter determines how often certain actions are
                            \ performed within the main loop
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BRIEF](../subroutine/brief.md)
                            \   * [DEATH](../subroutine/death.md)
                            \   * [Main flight loop (Part 13 of 16)](../subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [Main flight loop (Part 14 of 16)](../subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [Main game loop (Part 2 of 6)](../subroutine/main_game_loop_part_2_of_6.md)
                            \   * [MVEIT (Part 1 of 9)](../subroutine/mveit_part_1_of_9.md)
                            \   * [MVEIT (Part 2 of 9)](../subroutine/mveit_part_2_of_9.md)
                            \   * [PZW](../subroutine/pzw.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [WARP](../subroutine/warp.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .TYPE
    
     SKIP 1                 \ The current ship type
                            \
                            \ This is where we store the current ship type for when
                            \ we are iterating through the ships in the local bubble
                            \ as part of the main flight loop. See the table at XX21
                            \ for information about ship types
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ANGRY](../subroutine/angry.md)
                            \   * [BRIEF](../subroutine/brief.md)
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [ESCAPE](../subroutine/escape.md)
                            \   * [HITCH](../subroutine/hitch.md)
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [Main flight loop (Part 4 of 16)](../subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 5 of 16)](../subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [Main flight loop (Part 7 of 16)](../subroutine/main_flight_loop_part_7_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [MVEIT (Part 2 of 9)](../subroutine/mveit_part_2_of_9.md)
                            \   * [MVEIT (Part 6 of 9)](../subroutine/mveit_part_6_of_9.md)
                            \   * [PL2](../subroutine/pl2.md)
                            \   * [PL9 (Part 1 of 3)](../subroutine/pl9_part_1_of_3.md)
                            \   * [PLANET](../subroutine/planet.md)
                            \   * [SCAN](../subroutine/scan.md)
                            \   * [SFS1](../subroutine/sfs1.md)
                            \   * [TACTICS (Part 4 of 7)](../subroutine/tactics_part_4_of_7.md)
                            \   * [TACTICS (Part 5 of 7)](../subroutine/tactics_part_5_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../subroutine/tactics_part_7_of_7.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [WPSHPS](../subroutine/wpshps.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ALPHA
    
     SKIP 1                 \ The current roll angle alpha, which is reduced from
                            \ JSTX to a sign-magnitude value between -31 and +31
                            \
                            \ This describes how fast we are rolling our ship, and
                            \ determines how fast the universe rolls around us
                            \
                            \ The sign bit is also stored in ALP2, while the
                            \ opposite sign is stored in ALP2+1
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [MV40](../subroutine/mv40.md)
                            \   * [MVS4](../subroutine/mvs4.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ12
    
     SKIP 1                 \ Our "docked" status
                            \
                            \   * 0 = we are not docked
                            \
                            \   * &FF = we are docked
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BAY](../subroutine/bay.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [Main game loop (Part 6 of 6)](../subroutine/main_game_loop_part_6_of_6.md)
                            \   * [PDESC](../subroutine/pdesc.md)
                            \   * [RESET](../subroutine/reset.md)
                            \   * [STATUS](../subroutine/status.md)
                            \   * [TT102](../subroutine/tt102.md)
                            \   * [TT110](../subroutine/tt110.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .TGT
    
     SKIP 1                 \ Temporary storage, typically used as a target value
                            \ for counters when drawing explosion clouds and partial
                            \ circles
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [PL9 (Part 3 of 3)](../subroutine/pl9_part_3_of_3.md)
                            \   * [PLS2](../subroutine/pls2.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 2 of 4)](../subroutine/sun_part_2_of_4.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .FLAG
    
     SKIP 1                 \ A flag that's used to define whether this is the first
                            \ call to the ball line routine in BLINE, so it knows
                            \ whether to wait for the second call before storing
                            \ segment data in the ball line heap
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [CIRCLE2](../subroutine/circle2.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .CNT
    
     SKIP 1                 \ Temporary storage, typically used for storing the
                            \ number of iterations required when looping
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [CIRCLE2](../subroutine/circle2.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../subroutine/ll9_part_11_of_12.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [SPIN](../subroutine/spin.md)
                            \   * [STATUS](../subroutine/status.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \   * [TACTICS (Part 3 of 7)](../subroutine/tactics_part_3_of_7.md)
                            \   * [TACTICS (Part 6 of 7)](../subroutine/tactics_part_6_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../subroutine/tactics_part_7_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .CNT2
    
     SKIP 1                 \ Temporary storage, used in the planet-drawing routine
                            \ to store the segment number where the arc of a partial
                            \ circle should start
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [HALL](../subroutine/hall.md)
                            \   * [PL9 (Part 3 of 3)](../subroutine/pl9_part_3_of_3.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [PLS4](../subroutine/pls4.md)
                            \   * [TACTICS (Part 2 of 7)](../subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../subroutine/tactics_part_7_of_7.md)
                            \   * [TITLE](../subroutine/title.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .STP
    
     SKIP 1                 \ The step size for drawing circles
                            \
                            \ Circles in Elite are split up into 64 points, and the
                            \ step size determines how many points to skip with each
                            \ straight-line segment, so the smaller the step size,
                            \ the smoother the circle. The values used are:
                            \
                            \   * 2 for big planets and the circles on the charts
                            \   * 4 for medium planets and the launch tunnel
                            \   * 8 for small planets and the hyperspace tunnel
                            \
                            \ As the step size increases we move from smoother
                            \ circles at the top to more polygonal at the bottom.
                            \ See the CIRCLE2 routine for more details
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BLINE](../subroutine/bline.md)
                            \   * [CIRCLE](../subroutine/circle.md)
                            \   * [HFS2](../subroutine/hfs2.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [TT128](../subroutine/tt128.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX4
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [GVL](../subroutine/gvl.md)
                            \   * [HFS2](../subroutine/hfs2.md)
                            \   * [KILLSHP](../subroutine/killshp.md)
                            \   * [KS2](../subroutine/ks2.md)
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 2 of 12)](../subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 4 of 12)](../subroutine/ll9_part_4_of_12.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \   * [STATUS](../subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX20
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [HME2](../subroutine/hme2.md)
                            \   * [LL9 (Part 5 of 12)](../subroutine/ll9_part_5_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../subroutine/ll9_part_11_of_12.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSNUM
    
     SKIP 1                 \ The pointer to the current position in the ship line
                            \ heap as we work our way through the new ship's edges
                            \ (and the corresponding old ship's edges) when drawing
                            \ the ship in the main ship-drawing routine at LL9
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 11 of 12)](../subroutine/ll9_part_11_of_12.md)
                            \   * [LL9 (Part 12 of 12)](../subroutine/ll9_part_12_of_12.md)
                            \   * [LSPUT](../subroutine/lsput.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSNUM2
    
     SKIP 1                 \ The size of the existing ship line heap for the ship
                            \ we are drawing in LL9, i.e. the number of lines in the
                            \ old ship that is currently shown on-screen and which
                            \ we need to erase
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 12 of 12)](../subroutine/ll9_part_12_of_12.md)
                            \   * [LSPUT](../subroutine/lsput.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .RAT
    
     SKIP 1                 \ Used to store different signs depending on the current
                            \ space view, for use in calculating stardust movement
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [PLUT](../subroutine/plut.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [TACTICS (Part 2 of 7)](../subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../subroutine/tactics_part_7_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .RAT2
    
     SKIP 1                 \ Temporary storage, used to store the pitch and roll
                            \ signs when moving objects and stardust
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [HAS1](../subroutine/has1.md)
                            \   * [MVEIT (Part 8 of 9)](../subroutine/mveit_part_8_of_9.md)
                            \   * [MVS5](../subroutine/mvs5.md)
                            \   * [PLUT](../subroutine/plut.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [TACTICS (Part 2 of 7)](../subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../subroutine/tactics_part_7_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .K2
    
     SKIP 4                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [MV40](../subroutine/mv40.md)
                            \   * [MVEIT (Part 5 of 9)](../subroutine/mveit_part_5_of_9.md)
                            \   * [PL9 (Part 2 of 3)](../subroutine/pl9_part_2_of_3.md)
                            \   * [PL9 (Part 3 of 3)](../subroutine/pl9_part_3_of_3.md)
                            \   * [PLS22](../subroutine/pls22.md)
                            \   * [PLS5](../subroutine/pls5.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 3 of 4)](../subroutine/sun_part_3_of_4.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .widget
    
     SKIP 1                 \ Temporary storage, used to store the original argument
                            \ in A in the logarithmic FMLTU and LL28 routines
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DVID4](../subroutine/dvid4.md)
                            \   * [FMLTU](../subroutine/fmltu.md)
                            \   * [LL28](../subroutine/ll28.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .dontclip
    
     SKIP 1                 \ This is set to 0 in the RES2 routine, but the value is
                            \ never actually read (this is left over from the
                            \ Commodore 64 version of Elite)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [RES2](../subroutine/res2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Yx2M1
    
     SKIP 1                 \ This is used to store the number of pixel rows in the
                            \ space view minus 1, which is also the y-coordinate of
                            \ the bottom pixel row of the space view (it is set to
                            \ 191 in the RES2 routine)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [CHKON](../subroutine/chkon.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [SUN (Part 2 of 4)](../subroutine/sun_part_2_of_4.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .messXC
    
     SKIP 1                 \ Temporary storage, used to store the text column
                            \ of the in-flight message in MESS, so it can be erased
                            \ from the screen at the correct time
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [MESS](../subroutine/mess.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .newzp
    
     SKIP 1                 \ This is used by the STARS2 routine for storing the
                            \ stardust particle's delta_x value
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [STARS2](../subroutine/stars2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX1
    
     SKIP 0                 \ This is an alias for INWK that is used in the main
                            \ ship-drawing routine at LL9
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 2 of 12)](../subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 3 of 12)](../subroutine/ll9_part_3_of_12.md)
                            \   * [LL9 (Part 6 of 12)](../subroutine/ll9_part_6_of_12.md)
                            \   * [LL9 (Part 7 of 12)](../subroutine/ll9_part_7_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../subroutine/ll9_part_9_of_12.md)
                            \   * [SHPPT](../subroutine/shppt.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .INWK
    
     SKIP 33                \ The zero-page internal workspace for the current ship
                            \ data block
                            \
                            \ As operations on zero page locations are faster and
                            \ have smaller opcodes than operations on the rest of
                            \ the addressable memory, Elite tends to store oft-used
                            \ data here. A lot of the routines in Elite need to
                            \ access and manipulate ship data, so to make this an
                            \ efficient exercise, the ship data is first copied from
                            \ the ship data blocks at K% into INWK (or, when new
                            \ ships are spawned, from the blueprints at XX21)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BRIEF](../subroutine/brief.md)
                            \   * [DEATH](../subroutine/death.md)
                            \   * [DELT](../subroutine/delt.md)
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [DVID3B2](../subroutine/dvid3b2.md)
                            \   * [ESCAPE](../subroutine/escape.md)
                            \   * [FAROF2](../subroutine/farof2.md)
                            \   * [FRS1](../subroutine/frs1.md)
                            \   * [GTDIR](../subroutine/gtdir.md)
                            \   * [GTHG](../subroutine/gthg.md)
                            \   * [GTNMEW](../subroutine/gtnmew.md)
                            \   * [HAS1](../subroutine/has1.md)
                            \   * [HITCH](../subroutine/hitch.md)
                            \   * [HME2](../subroutine/hme2.md)
                            \   * [KS4](../subroutine/ks4.md)
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [Main flight loop (Part 4 of 16)](../subroutine/main_flight_loop_part_4_of_16.md)
                            \   * [Main flight loop (Part 5 of 16)](../subroutine/main_flight_loop_part_5_of_16.md)
                            \   * [Main flight loop (Part 6 of 16)](../subroutine/main_flight_loop_part_6_of_16.md)
                            \   * [Main flight loop (Part 7 of 16)](../subroutine/main_flight_loop_part_7_of_16.md)
                            \   * [Main flight loop (Part 9 of 16)](../subroutine/main_flight_loop_part_9_of_16.md)
                            \   * [Main flight loop (Part 10 of 16)](../subroutine/main_flight_loop_part_10_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main flight loop (Part 14 of 16)](../subroutine/main_flight_loop_part_14_of_16.md)
                            \   * [Main game loop (Part 1 of 6)](../subroutine/main_game_loop_part_1_of_6.md)
                            \   * [Main game loop (Part 2 of 6)](../subroutine/main_game_loop_part_2_of_6.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \   * [MAS1](../subroutine/mas1.md)
                            \   * [MAS4](../subroutine/mas4.md)
                            \   * [MT26](../subroutine/mt26.md)
                            \   * [MV40](../subroutine/mv40.md)
                            \   * [MVEIT (Part 1 of 9)](../subroutine/mveit_part_1_of_9.md)
                            \   * [MVEIT (Part 2 of 9)](../subroutine/mveit_part_2_of_9.md)
                            \   * [MVEIT (Part 3 of 9)](../subroutine/mveit_part_3_of_9.md)
                            \   * [MVEIT (Part 4 of 9)](../subroutine/mveit_part_4_of_9.md)
                            \   * [MVEIT (Part 5 of 9)](../subroutine/mveit_part_5_of_9.md)
                            \   * [MVEIT (Part 8 of 9)](../subroutine/mveit_part_8_of_9.md)
                            \   * [MVEIT (Part 9 of 9)](../subroutine/mveit_part_9_of_9.md)
                            \   * [MVS4](../subroutine/mvs4.md)
                            \   * [MVS5](../subroutine/mvs5.md)
                            \   * [MVT1](../subroutine/mvt1.md)
                            \   * [MVT3](../subroutine/mvt3.md)
                            \   * [MVT6](../subroutine/mvt6.md)
                            \   * [NwS1](../subroutine/nws1.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [NWSPS](../subroutine/nwsps.md)
                            \   * [PAS1](../subroutine/pas1.md)
                            \   * [PAUSE](../subroutine/pause.md)
                            \   * [PL9 (Part 2 of 3)](../subroutine/pl9_part_2_of_3.md)
                            \   * [PL9 (Part 3 of 3)](../subroutine/pl9_part_3_of_3.md)
                            \   * [PLANET](../subroutine/planet.md)
                            \   * [PLS1](../subroutine/pls1.md)
                            \   * [PLS4](../subroutine/pls4.md)
                            \   * [PLUT](../subroutine/plut.md)
                            \   * [PROJ](../subroutine/proj.md)
                            \   * [rfile](../subroutine/rfile.md)
                            \   * [RLINE](../variable/rline.md)
                            \   * [SCAN](../subroutine/scan.md)
                            \   * [SFS1](../subroutine/sfs1.md)
                            \   * [SOLAR](../subroutine/solar.md)
                            \   * [SOS1](../subroutine/sos1.md)
                            \   * [TACTICS (Part 1 of 7)](../subroutine/tactics_part_1_of_7.md)
                            \   * [TACTICS (Part 2 of 7)](../subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 3 of 7)](../subroutine/tactics_part_3_of_7.md)
                            \   * [TACTICS (Part 4 of 7)](../subroutine/tactics_part_4_of_7.md)
                            \   * [TACTICS (Part 5 of 7)](../subroutine/tactics_part_5_of_7.md)
                            \   * [TACTICS (Part 6 of 7)](../subroutine/tactics_part_6_of_7.md)
                            \   * [TACTICS (Part 7 of 7)](../subroutine/tactics_part_7_of_7.md)
                            \   * [TAS3](../subroutine/tas3.md)
                            \   * [TIDY](../subroutine/tidy.md)
                            \   * [TIS3](../subroutine/tis3.md)
                            \   * [TITLE](../subroutine/title.md)
                            \   * [TR1](../subroutine/tr1.md)
                            \   * [TRNME](../subroutine/trnme.md)
                            \   * [TT110](../subroutine/tt110.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \   * [WPSHPS](../subroutine/wpshps.md)
                            \   * [Ze](../subroutine/ze.md)
                            \   * [ZINF](../subroutine/zinf.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .XX19
    
     SKIP NI% - 34          \ XX19(1 0) shares its location with INWK(34 33), which
                            \ contains the address of the ship line heap
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [LL9 (Part 12 of 12)](../subroutine/ll9_part_12_of_12.md)
                            \   * [LSPUT](../subroutine/lsput.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .NEWB
    
     SKIP 1                 \ The ship's "new byte flags" (or NEWB flags)
                            \
                            \ Contains details about the ship's type and associated
                            \ behaviour, such as whether they are a trader, a bounty
                            \ hunter, a pirate, currently hostile, in the process of
                            \ docking, inside the hold having been scooped, and so
                            \ on. The default values for each ship type are taken
                            \ from the table at E%
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOCKIT](../subroutine/dockit.md)
                            \   * [LL9 (Part 1 of 12)](../subroutine/ll9_part_1_of_12.md)
                            \   * [Main flight loop (Part 8 of 16)](../subroutine/main_flight_loop_part_8_of_16.md)
                            \   * [Main flight loop (Part 11 of 16)](../subroutine/main_flight_loop_part_11_of_16.md)
                            \   * [Main flight loop (Part 12 of 16)](../subroutine/main_flight_loop_part_12_of_16.md)
                            \   * [Main game loop (Part 1 of 6)](../subroutine/main_game_loop_part_1_of_6.md)
                            \   * [Main game loop (Part 4 of 6)](../subroutine/main_game_loop_part_4_of_6.md)
                            \   * [NWSHP](../subroutine/nwshp.md)
                            \   * [NWSPS](../subroutine/nwsps.md)
                            \   * [TACTICS (Part 2 of 7)](../subroutine/tactics_part_2_of_7.md)
                            \   * [TACTICS (Part 3 of 7)](../subroutine/tactics_part_3_of_7.md)
                            \   * [TACTICS (Part 4 of 7)](../subroutine/tactics_part_4_of_7.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTX
    
     SKIP 1                 \ Our current roll rate
                            \
                            \ This value is shown in the dashboard's RL indicator,
                            \ and determines the rate at which we are rolling
                            \
                            \ The value ranges from 1 to 255 with 128 as the centre
                            \ point, so 1 means roll is decreasing at the maximum
                            \ rate, 128 means roll is not changing, and 255 means
                            \ roll is increasing at the maximum rate
                            \
                            \ This value is updated by "<" and ">" key presses, or
                            \ if joysticks are enabled, from the joystick. If
                            \ keyboard damping is enabled (which it is by default),
                            \ the value is slowly moved towards the centre value of
                            \ 128 (no roll) if there are no key presses or joystick
                            \ movement
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \   * [TT17](../subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .JSTY
    
     SKIP 1                 \ Our current pitch rate
                            \
                            \ This value is shown in the dashboard's DC indicator,
                            \ and determines the rate at which we are pitching
                            \
                            \ The value ranges from 1 to 255 with 128 as the centre
                            \ point, so 1 means pitch is decreasing at the maximum
                            \ rate, 128 means pitch is not changing, and 255 means
                            \ pitch is increasing at the maximum rate
                            \
                            \ This value is updated by "S" and "X" key presses, or
                            \ if joysticks are enabled, from the joystick. If
                            \ keyboard damping is enabled (which it is by default),
                            \ the value is slowly moved towards the centre value of
                            \ 128 (no pitch) if there are no key presses or joystick
                            \ movement
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [Main flight loop (Part 2 of 16)](../subroutine/main_flight_loop_part_2_of_16.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [TT17](../subroutine/tt17.md)
                            \   * [ZEKTRAN](../subroutine/zektran.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KL
    
     SKIP 1                 \ The following bytes implement a key logger that
                            \ enables Elite to scan for concurrent key presses of
                            \ the primary flight keys, plus a secondary flight key
                            \
                            \ If a key is being pressed that is not in the keyboard
                            \ table at KYTB, it can be stored here (as seen in
                            \ routine DK4, for example)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DK4](../subroutine/dk4.md)
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [FILLKL](../subroutine/fillkl.md)
                            \   * [RDKEY](../subroutine/rdkey.md)
                            \   * [TT102](../subroutine/tt102.md)
                            \   * [TT17](../subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY17
    
     SKIP 1                 \ "E" is being pressed (activate E.C.M.)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FILLKL](../subroutine/fillkl.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY14
    
     SKIP 1                 \ "T" is being pressed (target missile)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY15
    
     SKIP 1                 \ "U" is being pressed (unarm missile)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY20
    
     SKIP 1                 \ "P" is being pressed (deactivate docking computer)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY7
    
     SKIP 1                 \ "A" is being pressed (fire lasers)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ This is also set when the joystick fire button has
                            \ been pressed
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \   * [TT17](../subroutine/tt17.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY5
    
     SKIP 1                 \ "X" is being pressed (pull up)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY18
    
     SKIP 1                 \ "J" is being pressed (in-system jump)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY6
    
     SKIP 1                 \ "S" is being pressed (pitch down)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [FILLKL](../subroutine/fillkl.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY19
    
     SKIP 1                 \ "C" is being pressed (activate docking computer)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FILLKL](../subroutine/fillkl.md)
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY12
    
     SKIP 1                 \ TAB is being pressed (energy bomb)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY2
    
     SKIP 1                 \ Space is being pressed (speed up)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY16
    
     SKIP 1                 \ "M" is being pressed (fire missile)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY3
    
     SKIP 1                 \ "<" is being pressed (roll left)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY4
    
     SKIP 1                 \ ">" is being pressed (roll right)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOKEY](../subroutine/dokey.md)
                            \   * [RDJOY](../subroutine/rdjoy.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY1
    
     SKIP 1                 \ "?" is being pressed (slow down)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .KY13
    
     SKIP 1                 \ ESCAPE is being pressed (launch escape pod)
                            \
                            \   * 0 = no
                            \
                            \   * Non-zero = yes
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Main flight loop (Part 3 of 16)](../subroutine/main_flight_loop_part_3_of_16.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .LSX
    
     SKIP 1                 \ LSX contains the status of the sun line heap at LSO
                            \
                            \   * &FF indicates the sun line heap is empty
                            \
                            \   * Otherwise the LSO heap contains the line data for
                            \     the sun
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FLFLLS](../subroutine/flflls.md)
                            \   * [SUN (Part 1 of 4)](../subroutine/sun_part_1_of_4.md)
                            \   * [WPLS](../subroutine/wpls.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .FSH
    
     SKIP 1                 \ Forward shield status
                            \
                            \   * 0 = empty
                            \
                            \   * &FF = full
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [Main flight loop (Part 13 of 16)](../subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [OOPS](../subroutine/oops.md)
                            \   * [RESET](../subroutine/reset.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ASH
    
     SKIP 1                 \ Aft shield status
                            \
                            \   * 0 = empty
                            \
                            \   * &FF = full
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DIALS (Part 4 of 4)](../subroutine/dials_part_4_of_4.md)
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [Main flight loop (Part 13 of 16)](../subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [OOPS](../subroutine/oops.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .ENERGY
    
     SKIP 1                 \ Energy bank status
                            \
                            \   * 0 = empty
                            \
                            \   * &FF = full
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DENGY](../subroutine/dengy.md)
                            \   * [DIALS (Part 3 of 4)](../subroutine/dials_part_3_of_4.md)
                            \   * [DOENTRY](../subroutine/doentry.md)
                            \   * [Main flight loop (Part 13 of 16)](../subroutine/main_flight_loop_part_13_of_16.md)
                            \   * [Main flight loop (Part 15 of 16)](../subroutine/main_flight_loop_part_15_of_16.md)
                            \   * [OOPS](../subroutine/oops.md)
                            \   * [STATUS](../subroutine/status.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ3
    
     SKIP 1                 \ The selected system's economy (0-7)
                            \
                            \   * 0 = Rich Industrial
                            \   * 1 = Average Industrial
                            \   * 2 = Poor Industrial
                            \   * 3 = Mainly Industrial
                            \   * 4 = Mainly Agricultural
                            \   * 5 = Rich Agricultural
                            \   * 6 = Average Agricultural
                            \   * 7 = Poor Agricultural
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \   * [TT24](../subroutine/tt24.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ4
    
     SKIP 1                 \ The selected system's government (0-7)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \   * [TT24](../subroutine/tt24.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ5
    
     SKIP 1                 \ The selected system's tech level (0-14)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [BR1 (Part 2 of 2)](../subroutine/br1_part_2_of_2.md)
                            \   * [hyp1](../subroutine/hyp1.md)
                            \   * [TT24](../subroutine/tt24.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ6
    
     SKIP 2                 \ The selected system's population in billions * 10
                            \ (1-71), so the maximum population is 7.1 billion
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT24](../subroutine/tt24.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ7
    
     SKIP 2                 \ The selected system's productivity in M CR (96-62480)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [TT24](../subroutine/tt24.md)
                            \   * [TT25](../subroutine/tt25.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ8
    
     SKIP 2                 \ The distance from the current system to the selected
                            \ system in light years * 10, stored as a 16-bit number
                            \
                            \ The distance will be 0 if the selected system is the
                            \ current system
                            \
                            \ The galaxy chart is 102.4 light years wide and 51.2
                            \ light years tall (see the intra-system distance
                            \ calculations in routine TT111 for details), which
                            \ equates to 1024 x 512 in terms of QQ8
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Ghy](../subroutine/ghy.md)
                            \   * [hyp](../subroutine/hyp.md)
                            \   * [PDESC](../subroutine/pdesc.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT146](../subroutine/tt146.md)
                            \   * [TT18](../subroutine/tt18.md)
                            \   * [TT23](../subroutine/tt23.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ9
    
     SKIP 1                 \ The galactic x-coordinate of the crosshairs in the
                            \ galaxy chart (and, most of the time, the selected
                            \ system's galactic x-coordinate)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Ghy](../subroutine/ghy.md)
                            \   * [HME2](../subroutine/hme2.md)
                            \   * [jmp](../subroutine/jmp.md)
                            \   * [ping](../subroutine/ping.md)
                            \   * [TT103](../subroutine/tt103.md)
                            \   * [TT105](../subroutine/tt105.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT16](../subroutine/tt16.md)
                            \   * [TT22](../subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .QQ10
    
     SKIP 1                 \ The galactic y-coordinate of the crosshairs in the
                            \ galaxy chart (and, most of the time, the selected
                            \ system's galactic y-coordinate)
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Ghy](../subroutine/ghy.md)
                            \   * [HME2](../subroutine/hme2.md)
                            \   * [jmp](../subroutine/jmp.md)
                            \   * [TT103](../subroutine/tt103.md)
                            \   * [TT105](../subroutine/tt105.md)
                            \   * [TT111](../subroutine/tt111.md)
                            \   * [TT16](../subroutine/tt16.md)
                            \   * [TT22](../subroutine/tt22.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .NOSTM
    
     SKIP 1                 \ The number of stardust particles shown on screen,
                            \ which is 20 (#NOST) for normal space, and 3 for
                            \ witchspace
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [FLIP](../subroutine/flip.md)
                            \   * [MJP](../subroutine/mjp.md)
                            \   * [nWq](../subroutine/nwq.md)
                            \   * [RES2](../subroutine/res2.md)
                            \   * [STARS1](../subroutine/stars1.md)
                            \   * [STARS2](../subroutine/stars2.md)
                            \   * [STARS6](../subroutine/stars6.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     PRINT "ZP workspace from ", ~ZP, "to ", ~P%-1, "inclusive"
    

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[MESS3 (Loader)](../../loader/variable/mess3.md "Previous routine")[XX3](xx3.md "Next routine")
