---
title: "The FILEPR subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/filepr.html"
---

[RLINE](../variable/rline.md "Previous routine")[OTHERFILEPR](otherfilepr.md "Next routine")
    
    
           Name: FILEPR                                                  [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Display the currently selected media (disk or tape)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-filepr)
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls FILEPR
    
    
    
    
    
    
    .FILEPR
    
     LDA #3                 \ Print extended token 3 + DISK, i.e. token 3 or 2 (as
     CLC                    \ DISK can be 0 or &FF). In other versions of the game,
     ADC DISK               \ such as the Commodore 64 version, token 2 is "disk"
     JMP DETOK              \ and token 3 is "tape", so this displays the currently
                            \ selected media, but this system is unused in the
                            \ Master version and tokens 2 and 3 contain different
                            \ text
    

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Variable [DISK](../workspace/up.md#disk) in workspace [UP](../workspace/up.md)

The configuration setting for toggle key "T", which isn't actually used but is still updated by pressing "T" while the game is paused. This is a configuration option from the Commodore 64 version of Elite that lets you switch between tape and disc

[RLINE](../variable/rline.md "Previous routine")[OTHERFILEPR](otherfilepr.md "Next routine")
