---
title: "The BPRNT subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/bprnt.html"
---

[TT11](tt11.md "Previous routine")[DTW1](../variable/dtw1.md "Next routine")
    
    
           Name: BPRNT                                                   [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a 32-bit number, left-padded to a specific number of digits,
                 with an optional decimal point
      Deep dive: [Printing decimal numbers](https://elite.bbcelite.com/deep_dives/printing_decimal_numbers.html)
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_b.md#header-bprnt)
     Variations: See [code variations](../../related/compare/main/subroutine/bprnt.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [csh](csh.md) calls BPRNT
    
    
    
    
    
    * * *
    
    
     Print the 32-bit number stored in K(0 1 2 3) to a specific number of digits,
     left-padding with spaces for numbers with fewer digits (so lower numbers are
     right-aligned). Optionally include a decimal point.
    
    
    
    * * *
    
    
     Arguments:
    
       K(0 1 2 3)           The number to print, stored with the most significant
                            byte in K and the least significant in K+3 (i.e. as a
                            big-endian number, which is the opposite way to how the
                            6502 assembler stores addresses, for example)
    
       U                    The maximum number of digits to print, including the
                            decimal point (spaces will be used on the left to pad
                            out the result to this width, so the number is right-
                            aligned to this width). U must be 11 or less
    
       C flag               If set, include a decimal point followed by one
                            fractional digit (i.e. show the number to 1 decimal
                            place). In this case, the number in K(0 1 2 3) contains
                            10 * the number we end up printing, so to print 123.4,
                            we would pass 1234 in K(0 1 2 3) and would set the C
                            flag to include the decimal point
    
    
    
    
    .BPRNT
    
     LDX #11                \ Set T to the maximum number of digits allowed (11
     STX T                  \ characters, which is the number of digits in 10
                            \ billion). We will use this as a flag when printing
                            \ characters in TT37 below
    
     PHP                    \ Make a copy of the status register (in particular
                            \ the C flag) so we can retrieve it later
    
     BCC TT30               \ If the C flag is clear, we do not want to print a
                            \ decimal point, so skip the next two instructions
    
     DEC T                  \ As we are going to show a decimal point, decrement
     DEC U                  \ both the number of characters and the number of
                            \ digits (as one of them is now a decimal point)
    
    .TT30
    
     LDA #11                \ Set A to 11, the maximum number of digits allowed
    
     SEC                    \ Set the C flag so we can do subtraction without the
                            \ C flag affecting the result
    
     STA XX17               \ Store the maximum number of digits allowed (11) in
                            \ XX17
    
     SBC U                  \ Set U = 11 - U + 1, so U now contains the maximum
     STA U                  \ number of digits minus the number of digits we want
     INC U                  \ to display, plus 1 (so this is the number of digits
                            \ we should skip before starting to print the number
                            \ itself, and the plus 1 is there to ensure we print at
                            \ least one digit)
    
     LDY #0                 \ In the main loop below, we use Y to count the number
                            \ of times we subtract 10 billion to get the leftmost
                            \ digit, so set this to zero
    
     STY S                  \ In the main loop below, we use location S as an
                            \ 8-bit overflow for the 32-bit calculations, so
                            \ we need to set this to 0 before joining the loop
    
     JMP TT36               \ Jump to TT36 to start the process of printing this
                            \ number's digits
    
    .TT35
    
                            \ This subroutine multiplies K(S 0 1 2 3) by 10 and
                            \ stores the result back in K(S 0 1 2 3), using the fact
                            \ that K * 10 = (K * 2) + (K * 2 * 2 * 2)
    
     ASL K+3                \ Set K(S 0 1 2 3) = K(S 0 1 2 3) * 2 by rotating left
     ROL K+2
     ROL K+1
     ROL K
     ROL S
    
     LDX #3                 \ Now we want to make a copy of the newly doubled K in
                            \ XX15, so we can use it for the first (K * 2) in the
                            \ equation above, so set up a counter in X for copying
                            \ four bytes, starting with the last byte in memory
                            \ (i.e. the least significant)
    
    .tt35
    
     LDA K,X                \ Copy the X-th byte of K(0 1 2 3) to the X-th byte of
     STA XX15,X             \ XX15(0 1 2 3), so that XX15 will contain a copy of
                            \ K(0 1 2 3) once we've copied all four bytes
    
     DEX                    \ Decrement the loop counter
    
     BPL tt35               \ Loop back to copy the next byte until we have copied
                            \ all four
    
     LDA S                  \ Store the value of location S, our overflow byte, in
     STA XX15+4             \ XX15+4, so now XX15(4 0 1 2 3) contains a copy of
                            \ K(S 0 1 2 3), which is the value of (K * 2) that we
                            \ want to use in our calculation
    
     ASL K+3                \ Now to calculate the (K * 2 * 2 * 2) part. We still
     ROL K+2                \ have (K * 2) in K(S 0 1 2 3), so we just need to shift
     ROL K+1                \ it twice. This is the first one, so we do this:
     ROL K                  \
     ROL S                  \   K(S 0 1 2 3) = K(S 0 1 2 3) * 2 = K * 4
    
     ASL K+3                \ And then we do it again, so that means:
     ROL K+2                \
     ROL K+1                \   K(S 0 1 2 3) = K(S 0 1 2 3) * 2 = K * 8
     ROL K
     ROL S
    
     CLC                    \ Clear the C flag so we can do addition without the
                            \ C flag affecting the result
    
     LDX #3                 \ By now we've got (K * 2) in XX15(4 0 1 2 3) and
                            \ (K * 8) in K(S 0 1 2 3), so the final step is to add
                            \ these two 32-bit numbers together to get K * 10.
                            \ So we set a counter in X for four bytes, starting
                            \ with the last byte in memory (i.e. the least
                            \ significant)
    
    .tt36
    
     LDA K,X                \ Fetch the X-th byte of K into A
    
     ADC XX15,X             \ Add the X-th byte of XX15 to A, with carry
    
     STA K,X                \ Store the result in the X-th byte of K
    
     DEX                    \ Decrement the loop counter
    
     BPL tt36               \ Loop back to add the next byte, moving from the least
                            \ significant byte to the most significant, until we
                            \ have added all four
    
     LDA XX15+4             \ Finally, fetch the overflow byte from XX15(4 0 1 2 3)
    
     ADC S                  \ And add it to the overflow byte from K(S 0 1 2 3),
                            \ with carry
    
     STA S                  \ And store the result in the overflow byte from
                            \ K(S 0 1 2 3), so now we have our desired result, i.e.
                            \
                            \   K(S 0 1 2 3) = K(S 0 1 2 3) * 10
    
     LDY #0                 \ In the main loop below, we use Y to count the number
                            \ of times we subtract 10 billion to get the leftmost
                            \ digit, so set this to zero so we can rejoin the main
                            \ loop for another subtraction process
    
    .TT36
    
                            \ This is the main loop of our digit-printing routine.
                            \ In the following loop, we are going to count the
                            \ number of times that we can subtract 10 million and
                            \ store that count in Y, which we have already set to 0
    
     LDX #3                 \ Our first calculation concerns 32-bit numbers, so
                            \ set up a counter for a four-byte loop
    
     SEC                    \ Set the C flag so we can do subtraction without the
                            \ C flag affecting the result
    
    .tt37
    
                            \ We now loop through each byte in turn to do this:
                            \
                            \   XX15(4 0 1 2 3) = K(S 0 1 2 3) - 100,000,000,000
    
     LDA K,X                \ Subtract the X-th byte of TENS (i.e. 10 billion) from
     SBC TENS,X             \ the X-th byte of K
    
     STA XX15,X             \ Store the result in the X-th byte of XX15
    
     DEX                    \ Decrement the loop counter
    
     BPL tt37               \ Loop back to subtract the next byte, moving from the
                            \ least significant byte to the most significant, until
                            \ we have subtracted all four
    
     LDA S                  \ Subtract the fifth byte of 10 billion (i.e. &17) from
     SBC #&17               \ the fifth (overflow) byte of K, which is S
    
     STA XX15+4             \ Store the result in the overflow byte of XX15
    
     BCC TT37               \ If subtracting 10 billion took us below zero, jump to
                            \ TT37 to print out this digit, which is now in Y
    
     LDX #3                 \ We now want to copy XX15(4 0 1 2 3) back into
                            \ K(S 0 1 2 3), so we can loop back up to do the next
                            \ subtraction, so set up a counter for a four-byte loop
    
    .tt38
    
     LDA XX15,X             \ Copy the X-th byte of XX15(0 1 2 3) to the X-th byte
     STA K,X                \ of K(0 1 2 3), so that K(0 1 2 3) will contain a copy
                            \ of XX15(0 1 2 3) once we've copied all four bytes
    
     DEX                    \ Decrement the loop counter
    
     BPL tt38               \ Loop back to copy the next byte, until we have copied
                            \ all four
    
     LDA XX15+4             \ Store the value of location XX15+4, our overflow
     STA S                  \ byte in S, so now K(S 0 1 2 3) contains a copy of
                            \ XX15(4 0 1 2 3)
    
     INY                    \ We have now managed to subtract 10 billion from our
                            \ number, so increment Y, which is where we are keeping
                            \ a count of the number of subtractions so far
    
     JMP TT36               \ Jump back to TT36 to subtract the next 10 billion
    
    .TT37
    
     TYA                    \ If we get here then Y contains the digit that we want
                            \ to print (as Y has now counted the total number of
                            \ subtractions of 10 billion), so transfer Y into A
    
     BNE TT32               \ If the digit is non-zero, jump to TT32 to print it
    
     LDA T                  \ Otherwise the digit is zero. If we are already
                            \ printing the number then we will want to print a 0,
                            \ but if we haven't started printing the number yet,
                            \ then we probably don't, as we don't want to print
                            \ leading zeroes unless this is the only digit before
                            \ the decimal point
                            \
                            \ To help with this, we are going to use T as a flag
                            \ that tells us whether we have already started
                            \ printing digits:
                            \
                            \   * If T <> 0 we haven't printed anything yet
                            \
                            \   * If T = 0 then we have started printing digits
                            \
                            \ We initially set T above to the maximum number of
                            \ characters allowed, less 1 if we are printing a
                            \ decimal point, so the first time we enter the digit
                            \ printing routine at TT37, it is definitely non-zero
    
     BEQ TT32               \ If T = 0, jump straight to the print routine at TT32,
                            \ as we have already started printing the number, so we
                            \ definitely want to print this digit too
    
     DEC U                  \ We initially set U to the number of digits we want to
     BPL TT34               \ skip before starting to print the number. If we get
                            \ here then we haven't printed any digits yet, so
                            \ decrement U to see if we have reached the point where
                            \ we should start printing the number, and if not, jump
                            \ to TT34 to set up things for the next digit
    
     LDA #' '               \ We haven't started printing any digits yet, but we
     BNE tt34               \ have reached the point where we should start printing
                            \ our number, so call TT26 (via tt34) to print a space
                            \ so that the number is left-padded with spaces (this
                            \ BNE is effectively a JMP as A will never be zero)
    
    .TT32
    
     LDY #0                 \ We are printing an actual digit, so first set T to 0,
     STY T                  \ to denote that we have now started printing digits as
                            \ opposed to spaces
    
     CLC                    \ The digit value is in A, so add ASCII "0" to get the
     ADC #'0'               \ ASCII character number to print
    
    .tt34
    
     JSR TT26               \ Call TT26 to print the character in A and fall through
                            \ into TT34 to get things ready for the next digit
    
    .TT34
    
     DEC T                  \ Decrement T but keep T >= 0 (by incrementing it
     BPL P%+4               \ again if the above decrement made T negative)
     INC T
    
     DEC XX17               \ Decrement the total number of characters left to
                            \ print, which we stored in XX17
    
     BMI rT10               \ If the result is negative, we have printed all the
                            \ characters, so jump down to rT10 to return from the
                            \ subroutine
    
     BNE P%+10              \ If the result is positive (> 0) then we still have
                            \ characters left to print, so loop back to TT35 (via
                            \ the JMP TT35 instruction below) to print the next
                            \ digit
    
     PLP                    \ If we get here then we have printed the exact number
                            \ of digits that we wanted to, so restore the C flag
                            \ that we stored at the start of the routine
    
     BCC P%+7               \ If the C flag is clear, we don't want a decimal point,
                            \ so loop back to TT35 (via the JMP TT35 instruction
                            \ below) to print the next digit
    
     LDA #'.'               \ Otherwise the C flag is set, so print the decimal
     JSR TT26               \ point
    
     JMP TT35               \ Loop back to TT35 to print the next digit
    
    .rT10
    
     RTS                    \ Return from the subroutine
    

[X]

Variable [K](../workspace/zp.md#k) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [S](../workspace/zp.md#s) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [T](../workspace/zp.md#t) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [TENS](../variable/tens.md) (category: Text)

A constant used when printing large numbers in BPRNT

[X]

Subroutine [TT26](tt26.md) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[X]

Label [TT30](bprnt.md#tt30) is local to this routine

[X]

Label [TT32](bprnt.md#tt32) is local to this routine

[X]

Label [TT34](bprnt.md#tt34) is local to this routine

[X]

Label [TT35](bprnt.md#tt35) is local to this routine

[X]

Label [TT36](bprnt.md#tt36) is local to this routine

[X]

Label [TT37](bprnt.md#tt37) is local to this routine

[X]

Variable [U](../workspace/zp.md#u) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in a number of places

[X]

Variable [XX15](../workspace/zp.md#xx15) in workspace [ZP](../workspace/zp.md)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Variable [XX17](../workspace/zp.md#xx17) in workspace [ZP](../workspace/zp.md)

Temporary storage, used in BPRNT to store the number of characters to print, and as the edge counter in the main ship-drawing routine

[X]

Label [rT10](bprnt.md#rt10) is local to this routine

[X]

Label [tt34](bprnt.md#tt34) is local to this routine

[X]

Label [tt35](bprnt.md#tt35) is local to this routine

[X]

Label [tt36](bprnt.md#tt36) is local to this routine

[X]

Label [tt37](bprnt.md#tt37) is local to this routine

[X]

Label [tt38](bprnt.md#tt38) is local to this routine

[TT11](tt11.md "Previous routine")[DTW1](../variable/dtw1.md "Next routine")
