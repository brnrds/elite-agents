---
title: "The MT6 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mt6.html"
---

[MT13](mt13.md "Previous routine")[MT5](mt5.md "Next routine")
    
    
           Name: MT6                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Switch to standard tokens in Sentence Case
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-mt6)
     References: This subroutine is called as follows:
                 * [JMTB](../variable/jmtb.md) calls MT6
    
    
    
    
    
    * * *
    
    
     This routine sets the following:
    
       * QQ17 = %10000000 (set Sentence Case for standard tokens)
    
       * DTW3 = %11111111 (print standard tokens)
    
    
    
    
    .MT6
    
     LDA #%10000000         \ Set bit 7 of QQ17 to switch standard tokens to
     STA QQ17               \ Sentence Case
    
     LDA #%11111111         \ Set A = %11111111, so when we fall through into MT5,
                            \ DTW3 gets set to %11111111 and calls to DETOK print
                            \ standard tokens
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &00, or BIT &00A9, which does nothing apart
                            \ from affect the flags
    

[X]

Variable [QQ17](../workspace/zp.md#qq17) in workspace [ZP](../workspace/zp.md)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[MT13](mt13.md "Previous routine")[MT5](mt5.md "Next routine")
