---
title: "The NWDAV4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/nwdav4.html"
---

[gnum](gnum.md "Previous routine")[OUTK](outk.md "Next routine")
    
    
           Name: NWDAV4                                                  [Show more]
           Type: Subroutine
       Category: Market
        Summary: Print an "ITEM?" error, make a beep and rejoin the TT210 routine
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-nwdav4)
     References: This subroutine is called as follows:
                 * [TT210](tt210.md) calls NWDAV4
    
    
    
    
    
    
    .NWDAV4
    
     JSR TT67               \ Print a newline
    
     LDA #176               \ Print recursive token 127 ("ITEM") followed by a
     JSR prq                \ question mark
    
     JSR dn2                \ Call dn2 to make a short, high beep and delay for 1
                            \ second
    
     LDY QQ29               \ Fetch the item number we are selling from QQ29
    
     JMP NWDAVxx            \ Jump back into the TT210 routine that called NWDAV4
    

[X]

Entry point [NWDAVxx](tt210.md#nwdavxx) in subroutine [TT210](tt210.md) (category: Market)

Used to rejoin this routine from the call to NWDAV4

[X]

Variable [QQ29](../workspace/wp.md#qq29) in workspace [WP](../workspace/wp.md)

Temporary storage, used in a number of places

[X]

Subroutine [TT67](tt67.md) (category: Text)

Print a newline

[X]

Subroutine [dn2](dn2.md) (category: Text)

Make a short, high beep and delay for 0.5 seconds

[X]

Subroutine [prq](prq.md) (category: Text)

Print a text token followed by a question mark

[gnum](gnum.md "Previous routine")[OUTK](outk.md "Next routine")
