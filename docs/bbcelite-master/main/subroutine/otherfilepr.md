---
title: "The OTHERFILEPR subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/otherfilepr.html"
---

[FILEPR](filepr.md "Previous routine")[ZERO](zero.md "Next routine")
    
    
           Name: OTHERFILEPR                                             [Show more]
           Type: Subroutine
       Category: Save and load
        Summary: Display the non-selected media (disk or tape)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_f.md#header-otherfilepr)
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls OTHERFILEPR
    
    
    
    
    
    
    .OTHERFILEPR
    
     LDA #2                 \ Print extended token 2 - DISK, i.e. token 2 or 3 (as
     SEC                    \ DISK can be 0 or &FF). In other versions of the game,
     SBC DISK               \ such as the Commodore 64 version, token 2 is "disk"
     JMP DETOK              \ and token 3 is "tape", so this displays the other,
                            \ non-selected media, but this system is unused in the
                            \ Master version and tokens 2 and 3 contain different
                            \ text
    

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Variable [DISK](../workspace/up.md#disk) in workspace [UP](../workspace/up.md)

The configuration setting for toggle key "T", which isn't actually used but is still updated by pressing "T" while the game is paused. This is a configuration option from the Commodore 64 version of Elite that lets you switch between tape and disc

[FILEPR](filepr.md "Previous routine")[ZERO](zero.md "Next routine")
