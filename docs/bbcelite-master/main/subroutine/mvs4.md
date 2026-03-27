---
title: "The MVS4 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/mvs4.html"
---

[MVT1](mvt1.md "Previous routine")[MVT6](mvt6.md "Next routine")
    
    
           Name: MVS4                                                    [Show more]
           Type: Subroutine
       Category: Moving
        Summary: Apply pitch and roll to an orientation vector
      Deep dive: [Orientation vectors](https://elite.bbcelite.com/deep_dives/orientation_vectors.html)
                 [Pitching and rolling](https://elite.bbcelite.com/deep_dives/pitching_and_rolling.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_h.md#header-mvs4)
     References: This subroutine is called as follows:
                 * [MVEIT (Part 7 of 9)](mveit_part_7_of_9.md) calls MVS4
    
    
    
    
    
    * * *
    
    
     Apply pitch and roll angles alpha and beta to the orientation vector in Y.
    
     Specifically, this routine rotates a point (x, y, z) around the origin by
     pitch alpha and roll beta, using the small angle approximation to make the
     maths easier, and incorporating the Minsky circle algorithm to make the
     rotation more stable (though more elliptic).
    
     If that paragraph makes sense to you, then you should probably be writing
     this commentary! For the rest of us, see the associated deep dives.
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    Determines which of the INWK orientation vectors to
                            transform:
    
                              * Y = 9 rotates nosev: (nosev_x, nosev_y, nosev_z)
    
                              * Y = 15 rotates roofv: (roofv_x, roofv_y, roofv_z)
    
                              * Y = 21 rotates sidev: (sidev_x, sidev_y, sidev_z)
    
    
    
    
    .MVS4
    
     LDA ALPHA              \ Set Q = alpha (the roll angle to rotate through)
     STA Q
    
     LDX INWK+2,Y           \ Set (S R) = nosev_y
     STX R
     LDX INWK+3,Y
     STX S
    
     LDX INWK,Y             \ These instructions have no effect as MAD overwrites
     STX P                  \ X and P when called, but they set X = P = nosev_x_lo
    
     LDA INWK+1,Y           \ Set A = -nosev_x_hi
     EOR #%10000000
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
     STA INWK+3,Y           \           = alpha * -nosev_x_hi + nosev_y
     STX INWK+2,Y           \
                            \ and store (A X) in nosev_y, so this does:
                            \
                            \ nosev_y = nosev_y - alpha * nosev_x_hi
    
     STX P                  \ This instruction has no effect as MAD overwrites P,
                            \ but it sets P = nosev_y_lo
    
     LDX INWK,Y             \ Set (S R) = nosev_x
     STX R
     LDX INWK+1,Y
     STX S
    
     LDA INWK+3,Y           \ Set A = nosev_y_hi
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
     STA INWK+1,Y           \           = alpha * nosev_y_hi + nosev_x
     STX INWK,Y             \
                            \ and store (A X) in nosev_x, so this does:
                            \
                            \ nosev_x = nosev_x + alpha * nosev_y_hi
    
     STX P                  \ This instruction has no effect as MAD overwrites P,
                            \ but it sets P = nosev_x_lo
    
     LDA BETA               \ Set Q = beta (the pitch angle to rotate through)
     STA Q
    
     LDX INWK+2,Y           \ Set (S R) = nosev_y
     STX R
     LDX INWK+3,Y
     STX S
     LDX INWK+4,Y
    
     STX P                  \ This instruction has no effect as MAD overwrites P,
                            \ but it sets P = nosev_y
    
     LDA INWK+5,Y           \ Set A = -nosev_z_hi
     EOR #%10000000
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
     STA INWK+3,Y           \           = beta * -nosev_z_hi + nosev_y
     STX INWK+2,Y           \
                            \ and store (A X) in nosev_y, so this does:
                            \
                            \ nosev_y = nosev_y - beta * nosev_z_hi
    
     STX P                  \ This instruction has no effect as MAD overwrites P,
                            \ but it sets P = nosev_y_lo
    
     LDX INWK+4,Y           \ Set (S R) = nosev_z
     STX R
     LDX INWK+5,Y
     STX S
    
     LDA INWK+3,Y           \ Set A = nosev_y_hi
    
     JSR MAD                \ Set (A X) = Q * A + (S R)
     STA INWK+5,Y           \           = beta * nosev_y_hi + nosev_z
     STX INWK+4,Y           \
                            \ and store (A X) in nosev_z, so this does:
                            \
                            \ nosev_z = nosev_z + beta * nosev_y_hi
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [ALPHA](../workspace/zp.md#alpha) in workspace [ZP](../workspace/zp.md)

The current roll angle alpha, which is reduced from JSTX to a sign-magnitude value between -31 and +31

[X]

Variable [BETA](../workspace/zp.md#beta) in workspace [ZP](../workspace/zp.md)

The current pitch angle beta, which is reduced from JSTY to a sign-magnitude value between -8 and +8

[X]

Variable [INWK](../workspace/zp.md#inwk) in workspace [ZP](../workspace/zp.md)

The zero-page internal workspace for the current ship data block

[X]

Subroutine [MAD](mad.md) (category: Maths (Arithmetic))

Calculate (A X) = Q * A + (S R)

[X]

Variable [P](../workspace/zp.md#p) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [Q](../workspace/zp.md#q) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [R](../workspace/zp.md#r) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[MVT1](mvt1.md "Previous routine")[MVT6](mvt6.md "Next routine")
