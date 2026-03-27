---
title: "The TRTB% variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/trtb_per_cent.html"
---

[TTX66K](../subroutine/ttx66k.md "Previous routine")[IKNS](ikns.md "Next routine")
    
    
           Name: TRTB%                                                   [Show more]
           Type: Variable
       Category: Keyboard
        Summary: Translation table from internal key number to ASCII
    
    
        Context: See this variable [in context in the source code](../../all/elite_h.md#header-trtb-per-cent)
     Variations: See [code variations](../../related/compare/main/variable/trantable-trtb_per_cent.md) for this variable in the different versions
     References: This variable is used as follows:
                 * [RDKEY](../subroutine/rdkey.md) uses TRTB%
    
    
    
    
    
    * * *
    
    
     This is a copy of the keyboard translation table from the BBC Master's MOS
     3.20 ROM. The value at offset n is the upper-case ASCII value of the key with
     internal key number n, so for example the value at offset &10 is &51, which is
     81, or ASCII "Q", so internal key number &10 is the key number of the "Q"
     key.
    
     Valid internal key numbers are Binary Coded Decimal (BCD) numbers in the range
     &10 top &79, so they're in the ranges &10 to &19, then &20 to &29, then &30 to
     &39, and so on. This means that the other locations - i.e. &1A to &1F, &2A to
     &2F and so on - aren't used by the lookup table, but the MOS doesn't let this
     space go to waste; instead, those gaps contain MOS code, which is replicated
     below as the table contains a copy of this entire block of the MOS, not just
     the table entries.
    
     This table allows code running on the parasite to convert internal key numbers
     into ASCII codes in an efficient way. Without this table we would have to do a
     lookup from the table in the I/O processor's MOS ROM, which we would have to
     access from across the Tube, and this would be a lot slower than doing a
     simple table lookup in the parasite's user RAM.
    
    
    
    
    .TRTB%
    
     EQUB &00, &40, &FE     \ MOS code
     EQUB &A0, &5F, &8C
     EQUB &43, &FE, &8E
     EQUB &4F, &FE, &EA
     EQUB &AE, &4F, &FE
     EQUB &60
    
                            \ Internal key numbers &10 to &19:
                            \
     EQUB &51, &33          \ Q             3
     EQUB &34, &35          \ 4             5
     EQUB &84, &38          \ f4            8
     EQUB &87, &2D          \ f7            -
     EQUB &5E, &8C          \ ^             Left arrow
    
     EQUB &36, &37, &BC     \ MOS code
     EQUB &00, &FC, &60
    
                            \ Internal key numbers &20 to &29:
                            \
     EQUB &80, &57          \ f0            W
     EQUB &45, &54          \ E             T
     EQUB &37, &49          \ 7             I
     EQUB &39, &30          \ 9             0
     EQUB &5F, &8E          \ _             Down arrow
    
     EQUB &38, &39, &BC     \ MOS code
     EQUB &00, &FD, &60
    
                            \ Internal key numbers &30 to &39:
                            \
     EQUB &31, &32          \ 1             2
     EQUB &44, &52          \ D             R
     EQUB &36, &55          \ 6             U
     EQUB &4F, &50          \ O             P
     EQUB &5B, &8F          \ [             Up arrow
    
     EQUB &81, &82, &0D     \ MOS code
     EQUB &4C, &20, &02
    
                            \ Internal key numbers &40 to &49:
                            \
     EQUB &01, &41          \ CAPS LOCK     A
     EQUB &58, &46          \ X             F
     EQUB &59, &4A          \ Y             J
     EQUB &4B, &40          \ K             @
     EQUB &3A, &0D          \ :             RETURN
    
     EQUB &83, &7F, &AE     \ MOS code
     EQUB &4C, &FE, &FD
    
                            \ Internal key numbers &50 to &59:
                            \
     EQUB &02, &53          \ SHIFT LOCK    S
     EQUB &43, &47          \ C             G
     EQUB &48, &4E          \ H             N
     EQUB &4C, &3B          \ L             ;
     EQUB &5D, &7F          \ ]             DELETE
    
     EQUB &85, &84, &86     \ MOS code
     EQUB &4C, &FA, &00
    
                            \ Internal key numbers &60 to &69:
                            \
     EQUB &00, &5A          \ TAB           Z
     EQUB &20, &56          \ Space         V
     EQUB &42, &4D          \ B             M
     EQUB &2C, &2E          \ ,             .
     EQUB &2F, &8B          \ /             COPY
    
     EQUB &30, &31, &33     \ MOS code
     EQUB &00, &00, &00
    
                            \ Internal key numbers &70 to &79:
                            \
     EQUB &1B, &81          \ ESCAPE        f1
     EQUB &82, &83          \ f2            f3
     EQUB &85, &86          \ f5            f6
     EQUB &88, &89          \ f8            f9
     EQUB &5C, &8D          \ \             Right arrow
    
     EQUB &34, &35, &32     \ MOS code
     EQUB &2C, &4E, &E3
    

[TTX66K](../subroutine/ttx66k.md "Previous routine")[IKNS](ikns.md "Next routine")
