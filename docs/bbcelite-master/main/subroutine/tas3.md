---
title: "The TAS3 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/tas3.html"
---

[MULT12](mult12.md "Previous routine")[MAD](mad.md "Next routine")
    
    
           Name: TAS3                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the dot product of XX15 and an orientation vector
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_c.md#header-tas3)
     Variations: See [code variations](../../related/compare/main/subroutine/tas3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DOCKIT](dockit.md) calls TAS3
                 * [TACTICS (Part 3 of 7)](tactics_part_3_of_7.md) calls TAS3
                 * [TACTICS (Part 7 of 7)](tactics_part_7_of_7.md) calls TAS3
    
    
    
    
    
    * * *
    
    
     Calculate the dot product of the vector in XX15 and one of the orientation
     vectors, as determined by the value of Y. If vect is the orientation vector,
     we calculate this:
    
       (A X) = vect . XX15
             = vect_x * XX15 + vect_y * XX15+1 + vect_z * XX15+2
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The orientation vector:
    
                              * If Y = 10, calculate nosev . XX15
    
                              * If Y = 16, calculate roofv . XX15
    
                              * If Y = 22, calculate sidev . XX15
    
    
    
    * * *
    
    
     Returns:
    
       (A X)                The result of the dot product
    
    
    
    
    .TAS3
    
     LDX INWK,Y             \ Set Q = the Y-th byte of INWK, i.e. vect_x
     STX Q
    
     LDA XX15               \ Set A = XX15
    
     JSR MULT12             \ Set (S R) = Q * A
                            \           = vect_x * XX15
    
     LDX INWK+2,Y           \ Set Q = the Y+2-th byte of INWK, i.e. vect_y
     STX Q
    
     LDA XX15+1             \ Set A = XX15+1
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
                            \           = vect_y * XX15+1 + vect_x * XX15
    
     STA S                  \ Set (S R) = (A X)
     STX R
    
     LDX INWK+4,Y           \ Set Q = the Y+2-th byte of INWK, i.e. vect_z
     STX Q
    
     LDA XX15+2             \ Set A = XX15+2
    
                            \ Fall through into MAD to set:
                            \
                            \   (A X) = Q * A + (S R)
                            \           = vect_z * XX15+2 + vect_y * XX15+1 +
                            \             vect_x * XX15
    

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [MAD](mad.md) (category: Maths (Arithmetic))

Calculate (A X) = Q * A + (S R)

[X]

Subroutine [MULT12](mult12.md) (category: Maths (Arithmetic))

Calculate (S R) = Q * A

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

[MULT12](mult12.md "Previous routine")[MAD](mad.md "Next routine")
