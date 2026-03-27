---
title: "The TAS4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tas4.html"
---

[TAS1](tas1.md "Previous routine")[TAS6](tas6.md "Next routine")
    
    
           Name: TAS4                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the dot product of XX15 and one of the space station's
                 orientation vectors
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tas4)
     References: This subroutine is called as follows:
                 * [DOCKIT](dockit.md) calls TAS4
    
    
    
    
    
    * * *
    
    
     Calculate the dot product of the vector in XX15 and one of the space station's
     orientation vectors, as determined by the value of Y. If vect is the space
     station orientation vector, we calculate this:
    
       (A X) = vect . XX15
             = vect_x * XX15 + vect_y * XX15+1 + vect_z * XX15+2
    
     Technically speaking, this routine can also calculate the dot product between
     XX15 and the sun's orientation vectors, as the sun and space station share the
     same ship data slot (the second ship data block at K%). However, the sun
     doesn't have orientation vectors, so this only gets called when that slot is
     being used for the space station.
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The space station's orientation vector:
    
                              * If Y = 10, calculate nosev . XX15
    
                              * If Y = 16, calculate roofv . XX15
    
                              * If Y = 22, calculate sidev . XX15
    
    
    
    * * *
    
    
     Returns:
    
       (A X)                The result of the dot product
    
    
    
    
    .TAS4
    
     LDX K%+NI%,Y           \ Set Q = the Y-th byte of K%+NI%, i.e. vect_x from the
     STX Q                  \ second ship data block at K%
    
     LDA XX15               \ Set A = XX15
    
     JSR MULT12             \ Set (S R) = Q * A
                            \           = vect_x * XX15
    
     LDX K%+NI%+2,Y         \ Set Q = the Y+2-th byte of K%+NI%, i.e. vect_y
     STX Q
    
     LDA XX15+1             \ Set A = XX15+1
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
                            \           = vect_y * XX15+1 + vect_x * XX15
    
     STA S                  \ Set (S R) = (A X)
     STX R
    
     LDX K%+NI%+4,Y         \ Set Q = the Y+2-th byte of K%+NI%, i.e. vect_z
     STX Q
    
     LDA XX15+2             \ Set A = XX15+2
    
     JMP MAD                \ Set:
                            \
                            \   (A X) = Q * A + (S R)
                            \           = vect_z * XX15+2 + vect_y * XX15+1 +
                            \             vect_x * XX15
                            \
                            \ and return from the subroutine using a tail call
    

[X]

Workspace [K%](../workspace/k_per_cent.md) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Subroutine [MAD](mad.md) (category: Maths (Arithmetic))

Calculate (A X) = Q * A + (S R)

[X]

Subroutine [MULT12](mult12.md) (category: Maths (Arithmetic))

Calculate (S R) = Q * A

[X]

Configuration variable [NI%](../../all/workspaces.md#ni-per-cent) = 37

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[TAS1](tas1.md "Previous routine")[TAS6](tas6.md "Next routine")
