---
title: "The DETOK2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/detok2.html"
---

[DETOK](detok.md "Previous routine")[MT1](mt1.md "Next routine")
    
    
           Name: DETOK2                                                  [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print an extended text token (1-255)
      Deep dive: [Extended text tokens](https://elite.bbcelite.com/deep_dives/extended_text_tokens.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-detok2)
     References: This subroutine is called as follows:
                 * [DETOK](detok.md) calls DETOK2
                 * [PDESC](pdesc.md) calls DETOK2
                 * [MT18](mt18.md) calls via DTS
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The token to be printed (1-255)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is preserved
    
       Y                    Y is preserved
    
       V(1 0)               V(1 0) is preserved
    
    
    
    * * *
    
    
     Other entry points:
    
       DTS                  Print a single letter in the correct case
    
    
    
    
    .DETOK2
    
     CMP #32                \ If A < 32 then this is a jump token, so skip to DT3 to
     BCC DT3                \ process it
    
     BIT DTW3               \ If bit 7 of DTW3 is clear, then extended tokens are
     BPL DT8                \ enabled, so jump to DT8 to process them
    
                            \ If we get there then this is not a jump token and
                            \ extended tokens are not enabled, so we can call the
                            \ standard text token routine at TT27 to print the token
    
     TAX                    \ Copy the token number from A into X
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     TXA                    \ Copy the token number from X back into A
    
     JSR TT27               \ Call TT27 to print the text token
    
     JMP DT7                \ Jump to DT7 to restore V(1 0) and Y from the stack and
                            \ return from the subroutine
    
    .DT8
    
                            \ If we get here then this is not a jump token and
                            \ extended tokens are enabled
    
     CMP #'['               \ If A < ASCII "[" (i.e. A <= ASCII "Z", or 90) then
     BCC DTS                \ this is a printable ASCII character, so jump down to
                            \ DTS to print it
    
     CMP #129               \ If A < 129, so A is in the range 91-128, jump down to
     BCC DT6                \ DT6 to print a randomised token from the MTIN table
    
     CMP #215               \ If A < 215, so A is in the range 129-214, jump to
     BCC DETOK              \ DETOK as this is a recursive token, returning from the
                            \ subroutine using a tail call
    
                            \ If we get here then A >= 215, so this is a two-letter
                            \ token from the extended TKN2/QQ16 table
    
     SBC #215               \ Subtract 215 to get a token number in the range 0-12
                            \ (the C flag is set as we passed through the BCC above,
                            \ so this subtraction is correct)
    
     ASL A                  \ Set A = A * 2, so it can be used as a pointer into the
                            \ two-letter token tables at TKN2 and QQ16
    
     PHA                    \ Store A on the stack, so we can restore it for the
                            \ second letter below
    
     TAX                    \ Fetch the first letter of the two-letter token from
     LDA TKN2,X             \ TKN2, which is at TKN2 + X
    
     JSR DTS                \ Call DTS to print it
    
     PLA                    \ Restore A from the stack and transfer it into X
     TAX
    
     LDA TKN2+1,X           \ Fetch the second letter of the two-letter token from
                            \ TKN2, which is at TKN2 + X + 1, and fall through into
                            \ DTS to print it
    
    .DTS
    
     CMP #'A'               \ If A < ASCII "A", jump to DT9 to print this as ASCII
     BCC DT9
    
     BIT DTW6               \ If bit 7 of DTW6 is set, then lower case has been
     BMI DT10               \ enabled by jump token 13, {lower case}, so jump to
                            \ DT10 to apply the lower case and single cap masks
    
     BIT DTW2               \ If bit 7 of DTW2 is set, then we are not currently
     BMI DT5                \ printing a word, so jump to DT5 so we skip the setting
                            \ of lower case in Sentence Case (which we only want to
                            \ do when we are already printing a word)
    
    .DT10
    
     ORA DTW1               \ Convert the character to lower case if DTW1 is
                            \ %00100000 (i.e. if we are in {sentence case} mode)
    
    .DT5
    
     AND DTW8               \ Convert the character to upper case if DTW8 is
                            \ %11011111 (i.e. after a {single cap} token)
    
    .DT9
    
     JMP DASC               \ Jump to DASC to print the ASCII character in A,
                            \ returning from the routine using a tail call
    
    .DT3
    
                            \ If we get here then the token number in A is in the
                            \ range 1 to 32, so this is a jump token that should
                            \ call the corresponding address in the jump table at
                            \ JMTB
    
     TAX                    \ Copy the token number from A into X
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     TXA                    \ Copy the token number from X back into A
    
     ASL A                  \ Set A = A * 2, so it can be used as a pointer into the
                            \ jump table at JMTB, though because the original range
                            \ of values is 1-32, so the doubled range is 2-64, we
                            \ need to take the offset into the jump table from
                            \ JMTB-2 rather than JMTB
    
     TAX                    \ Copy the doubled token number from A into X
    
     LDA JMTB-2,X           \ Set DTM(2 1) to the X-th address from the table at
     STA DTM+1              \ JTM-2, which modifies the JSR DASC instruction at
     LDA JMTB-1,X           \ label DTM below so that it calls the subroutine at the
     STA DTM+2              \ relevant address from the JMTB table
    
     TXA                    \ Copy the doubled token number from X back into A
    
     LSR A                  \ Halve A to get the original token number
    
    .DTM
    
     JSR DASC               \ Call the relevant JMTB subroutine, as this instruction
                            \ will have been modified by the above to point to the
                            \ relevant address
    
    .DT7
    
     PLA                    \ Restore V(1 0) from the stack, so it is preserved
     STA V+1                \ through calls to this routine
     PLA
     STA V
    
     PLA                    \ Restore Y from the stack, so it is preserved through
     TAY                    \ calls to this routine
    
     RTS                    \ Return from the subroutine
    
    .DT6
    
                            \ If we get here then the token number in A is in the
                            \ range 91-128, which means we print a randomly picked
                            \ token from the token range given in the corresponding
                            \ entry in the MTIN table
    
     STA SC                 \ Store the token number in SC
    
     TYA                    \ Store Y on the stack
     PHA
    
     LDA V                  \ Store V(1 0) on the stack
     PHA
     LDA V+1
     PHA
    
     JSR DORND              \ Set X to a random number
     TAX
    
     LDA #0                 \ Set A to 0, so we can build a random number from 0 to
                            \ 4 in A plus the C flag, with each number being equally
                            \ likely
    
     CPX #51                \ Add 1 to A if X >= 51
     ADC #0
    
     CPX #102               \ Add 1 to A if X >= 102
     ADC #0
    
     CPX #153               \ Add 1 to A if X >= 153
     ADC #0
    
     CPX #204               \ Set the C flag if X >= 204
    
     LDX SC                 \ Fetch the token number from SC into X, so X is now in
                            \ the range 91-128
    
     ADC MTIN-91,X          \ Set A = MTIN-91 + token number (91-128) + random (0-4)
                            \       = MTIN + token number (0-37) + random (0-4)
    
     JSR DETOK              \ Call DETOK to print the extended recursive token in A
    
     JMP DT7                \ Jump to DT7 to restore V(1 0) and Y from the stack and
                            \ return from the subroutine using a tail call
    

[X]

Entry point [DASC](tt26.md#dasc) in subroutine [TT26](tt26.md) (category: Text)

DASC does exactly the same as TT26 and prints a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [DETOK](detok.md) (category: Text)

Print an extended recursive token from the TKN1 token table

[X]

Subroutine [DORND](dornd.md) (category: Maths (Arithmetic))

Generate random numbers

[X]

Label [DT10](detok2.md#dt10) is local to this routine

[X]

Label [DT3](detok2.md#dt3) is local to this routine

[X]

Label [DT5](detok2.md#dt5) is local to this routine

[X]

Label [DT6](detok2.md#dt6) is local to this routine

[X]

Label [DT7](detok2.md#dt7) is local to this routine

[X]

Label [DT8](detok2.md#dt8) is local to this routine

[X]

Label [DT9](detok2.md#dt9) is local to this routine

[X]

Label [DTM](detok2.md#dtm) is local to this routine

[X]

Entry point [DTS](detok2.md#dts) in subroutine [DETOK2](detok2.md) (category: Text)

Print a single letter in the correct case

[X]

Variable [DTW1](../variable/dtw1.md) (category: Text)

A mask for applying the lower case part of Sentence Case to extended text tokens

[X]

Variable [DTW2](../variable/dtw2.md) (category: Text)

A flag that indicates whether we are currently printing a word

[X]

Variable [DTW3](../variable/dtw3.md) (category: Text)

A flag for switching between standard and extended text tokens

[X]

Variable [DTW6](../variable/dtw6.md) (category: Text)

A flag to denote whether printing in lower case is enabled for extended text tokens

[X]

Variable [DTW8](../variable/dtw8.md) (category: Text)

A mask for capitalising the next letter in an extended text token

[X]

Variable [JMTB](../variable/jmtb.md) (category: Text)

The extended token table for jump tokens 1-32 (DETOK)

[X]

Variable [MTIN](../variable/mtin.md) (category: Text)

Lookup table for random tokens in the extended token table (0-37)

[X]

Variable [SC](../workspace/zp.md#sc) in workspace [ZP](../workspace/zp.md)

Screen address (low byte)

[X]

Variable [TKN2](../variable/tkn2.md) (category: Text)

The extended two-letter token lookup table

[X]

Subroutine [TT27](tt27.md) (category: Text)

Print a text token

[X]

Variable [V](../workspace/zp.md#v) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing an address pointer

[DETOK](detok.md "Previous routine")[MT1](mt1.md "Next routine")
