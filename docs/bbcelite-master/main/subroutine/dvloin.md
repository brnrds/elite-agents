---
title: "The DVLOIN subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/dvloin.html"
---

[SCALEX](scalex.md "Previous routine")[tnpr1](tnpr1.md "Next routine")
    
    
           Name: DVLOIN                                                  [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a vertical line from (A, GCYT) to (A, GCYB)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_d.md#header-dvloin)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This routine is from the Apple II version of Elite and is not used in the BBC
     Master version.
    
    
    
    
    .DVLOIN
    
     STA X1                 \ Draw a vertical line from (A, GCYT) to (A, GCYB)
     STA X2
     LDA #GCYT
     STA Y1
     LDA #GCYB
     STA Y2
     JMP LOIN
    

[X]

Configuration variable [GCYB](../../all/workspaces.md#gcyb) = GCYT + 128

The y-coordinate of the bottom of the Long-range chart

[X]

Configuration variable [GCYT](../../all/workspaces.md#gcyt) = 24

The y-coordinate of the top of the Long-range Chart

[X]

Subroutine [LOIN](loin.md) (category: Drawing lines)

Draw a one-segment line

[X]

Variable [X1](../workspace/zp.md#x1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](../workspace/zp.md#x2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [Y1](../workspace/zp.md#y1) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](../workspace/zp.md#y2) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for y-coordinates in line-drawing routines

[SCALEX](scalex.md "Previous routine")[tnpr1](tnpr1.md "Next routine")
