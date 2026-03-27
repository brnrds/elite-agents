---
title: "The BRP subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/brp.html"
---

[BRIEF2](brief2.md "Previous routine")[BRIEF3](brief3.md "Next routine")
    
    
           Name: BRP                                                     [Show more]
           Type: Subroutine
       Category: Missions
        Summary: Print an extended token and show the Status Mode screen
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-brp)
     Variations: See [code variations](../../related/compare/main/subroutine/brp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF3](brief3.md) calls BRP
                 * [DEBRIEF](debrief.md) calls BRP
                 * [DEBRIEF2](debrief2.md) calls BRP
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       BAYSTEP              Go to the docking bay (i.e. show the Status Mode screen)
    
    
    
    
    .BRP
    
     LDX #CYAN              \ Switch to colour 3, which is white or cyan
     STX COL
    
     JSR DETOK              \ Print the extended token in A
    
    .BAYSTEP
    
     JMP BAY                \ Jump to BAY to go to the docking bay (i.e. show the
                            \ Status Mode screen) and return from the subroutine
                            \ using a tail call
    

[X]

Subroutine [BAY](bay.md) (category: Status)

Go to the docking bay (i.e. show the Status Mode screen)

[X]

Variable [COL](../workspace/zp.md#col) in workspace [ZP](../workspace/zp.md)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Configuration variable [CYAN](../../all/workspaces.md#cyan) = %11111111

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[BRIEF2](brief2.md "Previous routine")[BRIEF3](brief3.md "Next routine")
