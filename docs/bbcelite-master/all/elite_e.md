---
title: "Elite E source in the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/all/elite_e.html"
---

[Elite D source](elite_d.md "Previous routine")[Elite F source](elite_f.md "Next routine")
    
    
     ELITE E FILE
    
    
    
    
     CODE_E% = P%
    
     LOAD_E% = LOAD% + P% - CODE%
    
    
    
    
           Name: cpl                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Print the selected system name
      Deep dive: [Generating system names](https://elite.bbcelite.com/deep_dives/generating_system_names.html)
                 [Galaxy and system seeds](https://elite.bbcelite.com/deep_dives/galaxy_and_system_seeds.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/cpl.md)
     References: This subroutine is called as follows:
                 * [HME2](../main/subroutine/hme2.md) calls cpl
                 * [hyp](../main/subroutine/hyp.md) calls cpl
                 * [TT102](../main/subroutine/tt102.md) calls cpl
                 * [TT23](../main/subroutine/tt23.md) calls cpl
                 * [TT27](../main/subroutine/tt27.md) calls cpl
                 * [ypl](../main/subroutine/ypl.md) calls cpl
    
    
    
    
    
    * * *
    
    
     Print control code 3 (the selected system name, i.e. the one in the crosshairs
     in the Short-range Chart).
    
    
    
    
    .cpl
    
     LDX #5                 \ First we need to back up the seeds in QQ15, so set up
                            \ a counter in X to cover three 16-bit seeds (i.e.
                            \ 6 bytes)
    
    .TT53
    
     LDA QQ15,X             \ Copy byte X from QQ15 to QQ19
     STA QQ19,X
    
     DEX                    \ Decrement the loop counter
    
     BPL TT53               \ Loop back for the next byte to back up
    
     LDY #3                 \ Step 1: Now that the seeds are backed up, we can
                            \ start the name-generation process. We will either
                            \ need to loop three or four times, so for now set
                            \ up a counter in Y to loop four times
    
     BIT QQ15               \ Check bit 6 of s0_lo, which is stored in QQ15
    
     BVS P%+3               \ If bit 6 is set then skip over the next instruction
    
     DEY                    \ Bit 6 is clear, so we only want to loop three times,
                            \ so decrement the loop counter in Y
    
     STY T                  \ Store the loop counter in T
    
    .TT55
    
     LDA QQ15+5             \ Step 2: Load s2_hi, which is stored in QQ15+5, and
     AND #%00011111         \ extract bits 0-4 by AND'ing with %11111
    
     BEQ P%+7               \ If all those bits are zero, then skip the following
                            \ two instructions to go to step 3
    
     ORA #%10000000         \ We now have a number in the range 1-31, which we can
                            \ easily convert into a two-letter token, but first we
                            \ need to add 128 (or set bit 7) to get a range of
                            \ 129-159
    
     JSR TT27               \ Print the two-letter token in A
    
     JSR TT54               \ Step 3: twist the seeds in QQ15
    
     DEC T                  \ Decrement the loop counter
    
     BPL TT55               \ Loop back for the next two letters
    
     LDX #5                 \ We have printed the system name, so we can now
                            \ restore the seeds we backed up earlier. Set up a
                            \ counter in X to cover three 16-bit seeds (i.e. 6
                            \ bytes)
    
    .TT56
    
     LDA QQ19,X             \ Copy byte X from QQ19 to QQ15
     STA QQ15,X
    
     DEX                    \ Decrement the loop counter
    
     BPL TT56               \ Loop back for the next byte to restore
    
     RTS                    \ Once all the seeds are restored, return from the
                            \ subroutine
    
    
    
    
           Name: cmn                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Print the commander's name
    
    
        Context: See this subroutine [on its own page](../main/subroutine/cmn.md)
     Variations: See [code variations](../related/compare/main/subroutine/cmn.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls cmn
    
    
    
    
    
    * * *
    
    
     Print control code 4 (the commander's name).
    
    
    
    * * *
    
    
     Other entry points:
    
       cmn-1                Contains an RTS
    
    
    
    
    .cmn
    
     LDY #0                 \ Set up a counter in Y, starting from 0
    
    .QUL4
    
     LDA NAME,Y             \ The commander's name is stored at NAME, so load the
                            \ Y-th character from NAME
    
     CMP #13                \ If we have reached the end of the name, return from
     BEQ ypl-1              \ the subroutine (ypl-1 points to the RTS below)
    
     JSR TT26               \ Print the character we just loaded
    
     INY                    \ Increment the loop counter
    
     BNE QUL4               \ Loop back for the next character
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: ypl                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Print the current system name
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ypl.md)
     Variations: See [code variations](../related/compare/main/subroutine/ypl.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls ypl
                 * [cmn](../main/subroutine/cmn.md) calls via ypl-1
    
    
    
    
    
    * * *
    
    
     Print control code 2 (the current system name).
    
    
    
    * * *
    
    
     Other entry points:
    
       ypl-1                Contains an RTS
    
    
    
    
    .ypl
    
     BIT MJ                 \ Check the mis-jump flag at MJ, and if bit 7 is set
     BMI ypl16              \ then we are in witchspace, and witchspace doesn't have
                            \ a system name, so jump to ypl16 to return from the
                            \ subroutine
    
     JSR TT62               \ Call TT62 below to swap the three 16-bit seeds in
                            \ QQ2 and QQ15 (before the swap, QQ2 contains the seeds
                            \ for the current system, while QQ15 contains the seeds
                            \ for the selected system)
    
     JSR cpl                \ Call cpl to print out the system name for the seeds
                            \ in QQ15 (which now contains the seeds for the current
                            \ system)
    
                            \ Now we fall through into the TT62 subroutine, which
                            \ will swap QQ2 and QQ15 once again, so everything goes
                            \ back into the right place, and the RTS at the end of
                            \ TT62 will return from the subroutine
    
    .TT62
    
     LDX #5                 \ Set up a counter in X for the three 16-bit seeds we
                            \ want to swap (i.e. 6 bytes)
    
    .TT78
    
     LDA QQ15,X             \ Swap byte X between QQ2 and QQ15
     LDY QQ2,X
     STA QQ2,X
     STY QQ15,X
    
     DEX                    \ Decrement the loop counter
    
     BPL TT78               \ Loop back for the next byte to swap
    
    .ypl16
    
     RTS                    \ Once all bytes are swapped, return from the
                            \ subroutine
    
    
    
    
           Name: tal                                                     [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Print the current galaxy number
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tal.md)
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls tal
    
    
    
    
    
    * * *
    
    
     Print control code 1 (the current galaxy number, right-aligned to width 3).
    
    
    
    
    .tal
    
     CLC                    \ We don't want to print the galaxy number with a
                            \ decimal point, so clear the C flag for pr2 to take as
                            \ an argument
    
     LDX GCNT               \ Load the current galaxy number from GCNT into X
    
     INX                    \ Add 1 to the galaxy number, as the galaxy numbers
                            \ are 0-7 internally, but we want to display them as
                            \ galaxy 1 through 8
    
     JMP pr2                \ Jump to pr2, which prints the number in X to a width
                            \ of 3 figures, left-padding with spaces to a width of
                            \ 3, and return from the subroutine using a tail call
    
    
    
    
           Name: fwl                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Print fuel and cash levels
    
    
        Context: See this subroutine [on its own page](../main/subroutine/fwl.md)
     References: This subroutine is called as follows:
                 * [TT213](../main/subroutine/tt213.md) calls fwl
                 * [TT27](../main/subroutine/tt27.md) calls fwl
    
    
    
    
    
    * * *
    
    
     Print control code 5 ("FUEL: ", fuel level, " LIGHT YEARS", newline, "CASH:",
     control code 0).
    
    
    
    
    .fwl
    
     LDA #105               \ Print recursive token 105 ("FUEL") followed by a
     JSR TT68               \ colon
    
     LDX QQ14               \ Load the current fuel level from QQ14
    
     SEC                    \ We want to print the fuel level with a decimal point,
                            \ so set the C flag for pr2 to take as an argument
    
     JSR pr2                \ Call pr2, which prints the number in X to a width of
                            \ 3 figures (i.e. in the format x.x, which will always
                            \ be exactly 3 characters as the maximum fuel is 7.0)
    
     LDA #195               \ Print recursive token 35 ("LIGHT YEARS") followed by
     JSR plf                \ a newline
    
    .PCASH
    
     LDA #119               \ Print recursive token 119 ("CASH:" then control code
     BNE TT27               \ 0, which prints cash levels, then " CR" and newline)
    
    
    
    
           Name: csh                                                     [Show more]
           Type: Subroutine
       Category: Status
        Summary: Print the current amount of cash
    
    
        Context: See this subroutine [on its own page](../main/subroutine/csh.md)
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls csh
    
    
    
    
    
    * * *
    
    
     Print control code 0 (the current amount of cash, right-aligned to width 9,
     followed by " CR" and a newline).
    
    
    
    
    .csh
    
     LDX #3                 \ We are going to use the BPRNT routine to print out
                            \ the current amount of cash, which is stored as a
                            \ 32-bit number at location CASH. BPRNT prints out
                            \ the 32-bit number stored in K, so before we call
                            \ BPRNT, we need to copy the four bytes from CASH into
                            \ K, so first we set up a counter in X for the 4 bytes
    
    .pc1
    
     LDA CASH,X             \ Copy byte X from CASH to K
     STA K,X
    
     DEX                    \ Decrement the loop counter
    
     BPL pc1                \ Loop back for the next byte to copy
    
     LDA #9                 \ We want to print the cash amount using up to 9 digits
     STA U                  \ (including the decimal point), so store this in U
                            \ for BRPNT to take as an argument
    
     SEC                    \ We want to print the cash amount with a decimal point,
                            \ so set the C flag for BRPNT to take as an argument
    
     JSR BPRNT              \ Print the amount of cash to 9 digits with a decimal
                            \ point
    
     LDA #226               \ Print recursive token 66 (" CR") followed by a
                            \ newline by falling through into plf
    
    
    
    
           Name: plf                                                     [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token followed by a newline
    
    
        Context: See this subroutine [on its own page](../main/subroutine/plf.md)
     References: This subroutine is called as follows:
                 * [fwl](../main/subroutine/fwl.md) calls plf
                 * [plf2](../main/subroutine/plf2.md) calls plf
                 * [STATUS](../main/subroutine/status.md) calls plf
                 * [TITLE](../main/subroutine/title.md) calls plf
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .plf
    
     JSR TT27               \ Print the text token in A
    
     JMP TT67               \ Jump to TT67 to print a newline and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: TT68                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token followed by a colon
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt68.md)
     References: This subroutine is called as follows:
                 * [fwl](../main/subroutine/fwl.md) calls TT68
                 * [TT146](../main/subroutine/tt146.md) calls TT68
                 * [TT25](../main/subroutine/tt25.md) calls TT68
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .TT68
    
     JSR TT27               \ Print the text token in A and fall through into TT73
                            \ to print a colon
    
    
    
    
           Name: TT73                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a colon
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt73.md)
     References: This subroutine is called as follows:
                 * [crlf](../main/subroutine/crlf.md) calls TT73
    
    
    
    
    
    
    .TT73
    
     LDA #':'               \ Set A to ASCII ":" and fall through into TT27 to
                            \ actually print the colon
    
    
    
    
           Name: TT27                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a text token
      Deep dive: [Printing text tokens](https://elite.bbcelite.com/deep_dives/printing_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt27.md)
     References: This subroutine is called as follows:
                 * [cpl](../main/subroutine/cpl.md) calls TT27
                 * [DETOK2](../main/subroutine/detok2.md) calls TT27
                 * [EQSHP](../main/subroutine/eqshp.md) calls TT27
                 * [ex](../main/subroutine/ex.md) calls TT27
                 * [fwl](../main/subroutine/fwl.md) calls TT27
                 * [hyp](../main/subroutine/hyp.md) calls TT27
                 * [JMTB](../main/variable/jmtb.md) calls TT27
                 * [mes9](../main/subroutine/mes9.md) calls TT27
                 * [MESS](../main/subroutine/mess.md) calls TT27
                 * [MT17](../main/subroutine/mt17.md) calls TT27
                 * [NLIN3](../main/subroutine/nlin3.md) calls TT27
                 * [plf](../main/subroutine/plf.md) calls TT27
                 * [prq](../main/subroutine/prq.md) calls TT27
                 * [qv](../main/subroutine/qv.md) calls TT27
                 * [spc](../main/subroutine/spc.md) calls TT27
                 * [TT151](../main/subroutine/tt151.md) calls TT27
                 * [TT162](../main/subroutine/tt162.md) calls TT27
                 * [TT208](../main/subroutine/tt208.md) calls TT27
                 * [TT210](../main/subroutine/tt210.md) calls TT27
                 * [TT213](../main/subroutine/tt213.md) calls TT27
                 * [TT214](../main/subroutine/tt214.md) calls TT27
                 * [TT219](../main/subroutine/tt219.md) calls TT27
                 * [TT22](../main/subroutine/tt22.md) calls TT27
                 * [TT25](../main/subroutine/tt25.md) calls TT27
                 * [TT43](../main/subroutine/tt43.md) calls TT27
                 * [TT60](../main/subroutine/tt60.md) calls TT27
                 * [TT67](../main/subroutine/tt67.md) calls TT27
                 * [TT68](../main/subroutine/tt68.md) calls TT27
                 * [TT70](../main/subroutine/tt70.md) calls TT27
                 * [TTX66K](../main/subroutine/ttx66k.md) calls TT27
    
    
    
    
    
    * * *
    
    
     Print a text token (i.e. a character, control code, two-letter token or
     recursive token).
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The text token to be printed
    
    
    
    
    .TT27
    
     TAX                    \ Copy the token number from A to X. We can then keep
                            \ decrementing X and testing it against zero, while
                            \ keeping the original token number intact in A; this
                            \ effectively implements a switch statement on the
                            \ value of the token
    
     BEQ csh                \ If token = 0, this is control code 0 (current amount
                            \ of cash and newline), so jump to csh to print the
                            \ amount of cash and return from the subroutine using
                            \ a tail call
    
     BMI TT43               \ If token > 127, this is either a two-letter token
                            \ (128-159) or a recursive token (160-255), so jump
                            \ to TT43 to process tokens
    
     DEX                    \ If token = 1, this is control code 1 (current galaxy
     BEQ tal                \ number), so jump to tal to print the galaxy number and
                            \ return from the subroutine using a tail call
    
     DEX                    \ If token = 2, this is control code 2 (current system
     BEQ ypl                \ name), so jump to ypl to print the current system name
                            \ and return from the subroutine using a tail call
    
     DEX                    \ If token > 3, skip the following instruction
     BNE P%+5
    
     JMP cpl                \ This token is control code 3 (selected system name)
                            \ so jump to cpl to print the selected system name
                            \ and return from the subroutine using a tail call
    
     DEX                    \ If token = 4, this is control code 4 (commander
     BEQ cmn                \ name), so jump to cmn to print the commander name
                            \ and return from the subroutine using a tail call
    
     DEX                    \ If token = 5, this is control code 5 (fuel, newline,
     BEQ fwl                \ cash, newline), so jump to fwl to print the fuel level
                            \ and return from the subroutine using a tail call
    
     DEX                    \ If token > 6, skip the following three instructions
     BNE P%+7
    
     LDA #%10000000         \ This token is control code 6 (switch to Sentence
     STA QQ17               \ Case), so set bit 7 of QQ17 to switch to Sentence Case
     RTS                    \ and return from the subroutine as we are done
    
     DEX                    \ If token > 8, skip the following two instructions
     DEX
     BNE P%+5
    
     STX QQ17               \ This token is control code 8 (switch to ALL CAPS), so
     RTS                    \ set QQ17 to 0 to switch to ALL CAPS and return from
                            \ the subroutine as we are done
    
     DEX                    \ If token = 9, this is control code 9 (tab to column
     BEQ crlf               \ 21 and print a colon), so jump to crlf
    
     CMP #96                \ By this point, token is either 7, or in 10-127.
     BCS ex                 \ Check token number in A and if token >= 96, then the
                            \ token is in 96-127, which is a recursive token, so
                            \ jump to ex, which prints recursive tokens in this
                            \ range (i.e. where the recursive token number is
                            \ correct and doesn't need correcting)
    
     CMP #14                \ If token < 14, skip the following two instructions
     BCC P%+6
    
     CMP #32                \ If token < 32, then this means token is in 14-31, so
     BCC qw                 \ this is a recursive token that needs 114 adding to it
                            \ to get the recursive token number, so jump to qw
                            \ which will do this
    
                            \ By this point, token is either 7 (beep) or in 10-13
                            \ (line feeds and carriage returns), or in 32-95
                            \ (ASCII letters, numbers and punctuation)
    
     LDX QQ17               \ Fetch QQ17, which controls letter case, into X
    
     BEQ TT74               \ If QQ17 = 0, then ALL CAPS is set, so jump to TT74
                            \ to print this character as is (i.e. as a capital)
    
     BMI TT41               \ If QQ17 has bit 7 set, then we are using Sentence
                            \ Case, so jump to TT41, which will print the
                            \ character in upper or lower case, depending on
                            \ whether this is the first letter in a word
    
     BIT QQ17               \ If we get here, QQ17 is not 0 and bit 7 is clear, so
     BVS TT46               \ either it is bit 6 that is set, or some other flag in
                            \ QQ17 is set (bits 0-5). So check whether bit 6 is set.
                            \ If it is, then ALL CAPS has been set (as bit 7 is
                            \ clear) but bit 6 is still indicating that the next
                            \ character should be printed in lower case, so we need
                            \ to fix this. We do this with a jump to TT46, which
                            \ will print this character in upper case and clear bit
                            \ 6, so the flags are consistent with ALL CAPS going
                            \ forward
    
                            \ If we get here, some other flag is set in QQ17 (one
                            \ of bits 0-5 is set), which shouldn't happen in this
                            \ version of Elite. If this were the case, then we
                            \ would fall through into TT42 to print in lower case,
                            \ which is how printing all words in lower case could
                            \ be supported (by setting QQ17 to 1, say)
    
    
    
    
           Name: TT42                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a letter in lower case
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt42.md)
     References: This subroutine is called as follows:
                 * [TT45](../main/subroutine/tt45.md) calls TT42
                 * [TT41](../main/subroutine/tt41.md) calls via TT44
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be printed. Can be one of the
                            following:
    
                              * 7 (beep)
    
                              * 10-13 (line feeds and carriage returns)
    
                              * 32-95 (ASCII capital letters, numbers and
                                punctuation)
    
    
    
    * * *
    
    
     Other entry points:
    
       TT44                 Jumps to TT26 to print the character in A (used to
                            enable us to use a branch instruction to jump to TT26)
    
    
    
    
    .TT42
    
     CMP #'A'               \ If A < ASCII "A", then this is punctuation, so jump
     BCC TT44               \ to TT26 (via TT44) to print the character as is, as
                            \ we don't care about the character's case
    
     CMP #'Z'+1             \ If A >= (ASCII "Z" + 1), then this is also
     BCS TT44               \ punctuation, so jump to TT26 (via TT44) to print the
                            \ character as is, as we don't care about the
                            \ character's case
    
     ADC #32                \ Add 32 to the character, to convert it from upper to
                            \ lower case
    
    .TT44
    
     JMP TT26               \ Print the character in A
    
    
    
    
           Name: TT41                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a letter according to Sentence Case
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt41.md)
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls TT41
    
    
    
    
    
    * * *
    
    
     The rules for printing in Sentence Case are as follows:
    
       * If QQ17 bit 6 is set, print lower case (via TT45)
    
       * If QQ17 bit 6 is clear, then:
    
           * If character is punctuation, just print it
    
           * If character is a letter, set QQ17 bit 6 and print letter as a capital
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be printed. Can be one of the
                            following:
    
                              * 7 (beep)
    
                              * 10-13 (line feeds and carriage returns)
    
                              * 32-95 (ASCII capital letters, numbers and
                                punctuation)
    
       X                    Contains the current value of QQ17
    
       QQ17                 Bit 7 is set
    
    
    
    
    .TT41
    
                            \ If we get here, then QQ17 has bit 7 set, so we are in
                            \ Sentence Case
    
     BIT QQ17               \ If QQ17 also has bit 6 set, jump to TT45 to print
     BVS TT45               \ this character in lower case
    
                            \ If we get here, then QQ17 has bit 6 clear and bit 7
                            \ set, so we are in Sentence Case and we need to print
                            \ the next letter in upper case
    
     CMP #'A'               \ If A < ASCII "A", then this is punctuation, so jump
     BCC TT74               \ to TT26 (via TT44) to print the character as is, as
                            \ we don't care about the character's case
    
     PHA                    \ Otherwise this is a letter, so store the token number
    
     TXA                    \ Set bit 6 in QQ17 (X contains the current QQ17)
     ORA #%1000000          \ so the next letter after this one is printed in lower
     STA QQ17               \ case
    
     PLA                    \ Restore the token number into A
    
     BNE TT44               \ Jump to TT26 (via TT44) to print the character in A
                            \ (this BNE is effectively a JMP as A will never be
                            \ zero)
    
    
    
    
           Name: qw                                                      [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a recursive token in the range 128-145
    
    
        Context: See this subroutine [on its own page](../main/subroutine/qw.md)
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls qw
    
    
    
    
    
    * * *
    
    
     Print a recursive token where the token number is in 128-145 (so the value
     passed to TT27 is in the range 14-31).
    
    
    
    * * *
    
    
     Arguments:
    
       A                    A value from 128-145, which refers to a recursive token
                            in the range 14-31
    
    
    
    
    .qw
    
     ADC #114               \ This is a recursive token in the range 0-95, so add
     BNE ex                 \ 114 to the argument to get the token number 128-145
                            \ and jump to ex to print it
    
    
    
    
           Name: crlf                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Tab to column 21 and print a colon
    
    
        Context: See this subroutine [on its own page](../main/subroutine/crlf.md)
     Variations: See [code variations](../related/compare/main/subroutine/crlf.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls crlf
    
    
    
    
    
    * * *
    
    
     Print control code 9 (tab to column 21 and print a colon). The subroutine
     name is pretty misleading, as it doesn't have anything to do with carriage
     returns or line feeds.
    
    
    
    
    .crlf
    
     LDA #21                \ Set the X-column in XC to 21
     JSR DOXC
    
     JMP TT73               \ Jump to TT73, which prints a colon (this BNE is
                            \ effectively a JMP as A will never be zero)
    
    
    
    
           Name: TT45                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a letter in lower case
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt45.md)
     References: This subroutine is called as follows:
                 * [TT41](../main/subroutine/tt41.md) calls TT45
    
    
    
    
    
    * * *
    
    
     This routine prints a letter in lower case. Specifically:
    
       * If QQ17 = 255, abort printing this character as printing is disabled
    
       * If this is a letter then print in lower case
    
       * Otherwise this is punctuation, so clear bit 6 in QQ17 and print
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be printed. Can be one of the
                            following:
    
                              * 7 (beep)
    
                              * 10-13 (line feeds and carriage returns)
    
                              * 32-95 (ASCII capital letters, numbers and
                                punctuation)
    
       X                    Contains the current value of QQ17
    
       QQ17                 Bits 6 and 7 are set
    
    
    
    
    .TT45
    
                            \ If we get here, then QQ17 has bit 6 and 7 set, so we
                            \ are in Sentence Case and we need to print the next
                            \ letter in lower case
    
     CPX #255               \ If QQ17 = 255 then printing is disabled, so return
     BEQ TT48               \ from the subroutine (as TT48 contains an RTS)
    
     CMP #'A'               \ If A >= ASCII "A", then jump to TT42, which will
     BCS TT42               \ print the letter in lowercase
    
                            \ Otherwise this is not a letter, it's punctuation, so
                            \ this is effectively a word break. We therefore fall
                            \ through to TT46 to print the character and set QQ17
                            \ to ensure the next word starts with a capital letter
    
    
    
    
           Name: TT46                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a character and switch to capitals
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt46.md)
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls TT46
    
    
    
    
    
    * * *
    
    
     Print a character and clear bit 6 in QQ17, so that the next letter that gets
     printed after this will start with a capital letter.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be printed. Can be one of the
                            following:
    
                              * 7 (beep)
    
                              * 10-13 (line feeds and carriage returns)
    
                              * 32-95 (ASCII capital letters, numbers and
                                punctuation)
    
       X                    Contains the current value of QQ17
    
       QQ17                 Bits 6 and 7 are set
    
    
    
    
    .TT46
    
     PHA                    \ Store the token number
    
     TXA                    \ Clear bit 6 in QQ17 (X contains the current QQ17) so
     AND #%10111111         \ the next letter after this one is printed in upper
     STA QQ17               \ case
    
     PLA                    \ Restore the token number into A
    
                            \ Now fall through into TT74 to print the character
    
    
    
    
           Name: TT74                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a character
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt74.md)
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls TT74
                 * [TT41](../main/subroutine/tt41.md) calls TT74
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The character to be printed
    
    
    
    
    .TT74
    
     JMP TT26               \ Print the character in A
    
    
    
    
           Name: TT43                                                    [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a two-letter token or recursive token 0-95
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt43.md)
     References: This subroutine is called as follows:
                 * [TT27](../main/subroutine/tt27.md) calls TT43
    
    
    
    
    
    * * *
    
    
     Print a two-letter token, or a recursive token where the token number is in
     0-95 (so the value passed to TT27 is in the range 160-255).
    
    
    
    * * *
    
    
     Arguments:
    
       A                    One of the following:
    
                              * 128-159 (two-letter token)
    
                              * 160-255 (the argument to TT27 that refers to a
                                recursive token in the range 0-95)
    
    
    
    
    .TT43
    
     CMP #160               \ If token >= 160, then this is a recursive token, so
     BCS TT47               \ jump to TT47 below to process it
    
     AND #127               \ This is a two-letter token with number 128-159. The
     ASL A                  \ set of two-letter tokens is stored in a lookup table
                            \ at QQ16, with each token taking up two bytes, so to
                            \ convert this into the token's position in the table,
                            \ we subtract 128 (or just clear bit 7) and multiply
                            \ by 2 (or shift left)
    
     TAY                    \ Transfer the token's position into Y so we can look
                            \ up the token using absolute indexed mode
    
     LDA QQ16,Y             \ Get the first letter of the token and print it
     JSR TT27
    
     LDA QQ16+1,Y           \ Get the second letter of the token
    
     CMP #'?'               \ If the second letter of the token is a question mark
     BEQ TT48               \ then this is a one-letter token, so just return from
                            \ the subroutine without printing (as TT48 contains an
                            \ RTS)
    
     JMP TT27               \ Print the second letter and return from the
                            \ subroutine
    
    .TT47
    
     SBC #160               \ This is a recursive token in the range 160-255, so
                            \ subtract 160 from the argument to get the token
                            \ number 0-95 and fall through into ex to print it
    
    
    
    
           Name: ex                                                      [Show more]
           Type: Subroutine
       Category: Text
        Summary: Print a recursive token
      Deep dive: [Printing text tokens](https://elite.bbcelite.com/deep_dives/printing_text_tokens.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/ex.md)
     References: This subroutine is called as follows:
                 * [DEATH](../main/subroutine/death.md) calls ex
                 * [qw](../main/subroutine/qw.md) calls ex
                 * [TT27](../main/subroutine/tt27.md) calls ex
                 * [DOEXP](../main/subroutine/doexp.md) calls via TT48
                 * [TT43](../main/subroutine/tt43.md) calls via TT48
                 * [TT45](../main/subroutine/tt45.md) calls via TT48
    
    
    
    
    
    * * *
    
    
     This routine works its way through the recursive text tokens that are stored
     in tokenised form in the table at QQ18, and when it finds token number A,
     it prints it. Tokens are null-terminated in memory and fill three pages,
     but there is no lookup table as that would consume too much memory, so the
     only way to find the correct token is to start at the beginning and look
     through the table byte by byte, counting tokens as we go until we are in the
     right place. This approach might not be terribly speed efficient, but it is
     certainly memory-efficient.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The recursive token to be printed, in the range 0-148
    
    
    
    * * *
    
    
     Other entry points:
    
       TT48                 Contains an RTS
    
    
    
    
    .ex
    
     TAX                    \ Copy the token number into X
    
     LDA #LO(QQ18)          \ Set V(1 0) to point to the recursive token table at
     STA V                  \ location QQ18
     LDA #HI(QQ18)
     STA V+1
    
     LDY #0                 \ Set a counter Y to point to the character offset
                            \ as we scan through the table
    
     TXA                    \ Copy the token number back into A, so both A and X
                            \ now contain the token number we want to print
    
     BEQ TT50               \ If the token number we want is 0, then we have
                            \ already found the token we are looking for, so jump
                            \ to TT50, otherwise start working our way through the
                            \ null-terminated token table until we find the X-th
                            \ token
    
    .TT51
    
     LDA (V),Y              \ Fetch the Y-th character from the token table page
                            \ we are currently scanning
    
     BEQ TT49               \ If the character is null, we've reached the end of
                            \ this token, so jump to TT49
    
     INY                    \ Increment character pointer and loop back around for
     BNE TT51               \ the next character in this token, assuming Y hasn't
                            \ yet wrapped around to 0
    
     INC V+1                \ If it has wrapped round to 0, we have just crossed
     BNE TT51               \ into a new page, so increment V+1 so that V points
                            \ to the start of the new page
    
    .TT49
    
     INY                    \ Increment the character pointer
    
     BNE TT59               \ If Y hasn't just wrapped around to 0, skip the next
                            \ instruction
    
     INC V+1                \ We have just crossed into a new page, so increment
                            \ V+1 so that V points to the start of the new page
    
    .TT59
    
     DEX                    \ We have just reached a new token, so decrement the
                            \ token number we are looking for
    
     BNE TT51               \ Assuming we haven't yet reached the token number in
                            \ X, look back up to keep fetching characters
    
    .TT50
    
                            \ We have now reached the correct token in the token
                            \ table, with Y pointing to the start of the token as
                            \ an offset within the page pointed to by V, so let's
                            \ print the recursive token. Because recursive tokens
                            \ can contain other recursive tokens, we need to store
                            \ our current state on the stack, so we can retrieve
                            \ it after printing each character in this token
    
     TYA                    \ Store the offset in Y on the stack
     PHA
    
     LDA V+1                \ Store the high byte of V (the page containing the
     PHA                    \ token we have found) on the stack, so the stack now
                            \ contains the address of the start of this token
    
     LDA (V),Y              \ Load the character at offset Y in the token table,
                            \ which is the next character of this token that we
                            \ want to print
    
     EOR #RE                \ Tokens are stored in memory having been EOR'd with the
                            \ value of RE - which is 35 for all versions of Elite
                            \ except for NES, where RE is 62 - so we repeat the
                            \ EOR to get the actual character to print
    
     JSR TT27               \ Print the text token in A, which could be a letter,
                            \ number, control code, two-letter token or another
                            \ recursive token
    
     PLA                    \ Restore the high byte of V (the page containing the
     STA V+1                \ token we have found) into V+1
    
     PLA                    \ Restore the offset into Y
     TAY
    
     INY                    \ Increment Y to point to the next character in the
                            \ token we are printing
    
     BNE P%+4               \ If Y is zero then we have just crossed into a new
     INC V+1                \ page, so increment V+1 so that V points to the start
                            \ of the new page
    
     LDA (V),Y              \ Load the next character we want to print into A
    
     BNE TT50               \ If this is not the null character at the end of the
                            \ token, jump back up to TT50 to print the next
                            \ character, otherwise we are done printing
    
    .TT48
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SWAPPZERO                                               [Show more]
           Type: Subroutine
       Category: Utility routines
        Summary: An unused routine that swaps bytes in and out of zero page
    
    
        Context: See this subroutine [on its own page](../main/subroutine/swappzero.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    
    .SWAPPZERO
    
     LDX #K3+1              \ This routine starts copying zero page from the byte
                            \ after K3 and up, using X as an index
    
    .SWPZL
    
     LDA ZP,X               \ These instructions have no effect, as they simply swap
     LDY ZP,X               \ a byte with itself
     STA ZP,X
     STY ZP,X
    
     INX                    \ Increment the loop counter
    
     BNE SWPZL              \ Loop back for the next byte
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DOEXP                                                   [Show more]
           Type: Subroutine
       Category: Drawing ships
        Summary: Draw an exploding ship
      Deep dive: [Drawing explosion clouds](https://elite.bbcelite.com/deep_dives/drawing_explosion_clouds.html)
                 [Generating random numbers](https://elite.bbcelite.com/deep_dives/generating_random_numbers.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/doexp.md)
     Variations: See [code variations](../related/compare/main/subroutine/doexp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md) calls DOEXP
                 * [LL9 (Part 9 of 12)](../main/subroutine/ll9_part_9_of_12.md) calls DOEXP
    
    
    
    
    
    
    .EX2
    
     LDA INWK+31            \ Set bits 5 and 7 of the ship's byte #31 to denote that
     ORA #%10100000         \ the ship is exploding and has been killed
     STA INWK+31
    
     RTS                    \ Return from the subroutine
    
    .DOEXP
    
     LDA INWK+31            \ If bit 6 of the ship's byte #31 is clear, then the
     AND #%01000000         \ ship is not already exploding so there is no existing
     BEQ P%+5               \ explosion cloud to remove, so skip the following
                            \ instruction
    
     JSR PTCLS              \ Call PTCLS to remove the existing cloud by drawing it
                            \ again
    
     LDA INWK+6             \ Set T = z_lo
     STA T
    
     LDA INWK+7             \ Set A = z_hi, so (A T) = z
    
     CMP #32                \ If z_hi < 32, skip the next two instructions
     BCC P%+6
    
     LDA #&FE               \ Set A = 254 and jump to yy (this BNE is effectively a
     BNE yy                 \ JMP, as A is never zero)
    
     ASL T                  \ Shift (A T) left twice
     ROL A
     ASL T
     ROL A
    
     SEC                    \ And then shift A left once more, inserting a 1 into
     ROL A                  \ bit 0
    
                            \ Overall, the above multiplies A by 8 and makes sure it
                            \ is at least 1, to leave a one-byte distance in A. We
                            \ can use this as the distance for our cloud, to ensure
                            \ that the explosion cloud is visible even for ships
                            \ that blow up a long way away
    
    .yy
    
     STA Q                  \ Store the distance to the explosion in Q
    
     LDY #1                 \ Fetch byte #1 of the ship line heap, which contains
     LDA (XX19),Y           \ the cloud counter
    
     STA frump              \ Store the cloud counter in frump (though this value is
                            \ never read, so this has no effect)
    
     ADC #4                 \ Add 4 to the cloud counter, so it ticks onwards every
                            \ we redraw it
    
     BCS EX2                \ If the addition overflowed, jump up to EX2 to update
                            \ the explosion flags and return from the subroutine
    
     STA (XX19),Y           \ Store the updated cloud counter in byte #1 of the ship
                            \ line heap
    
     JSR DVID4              \ Calculate the following:
                            \
                            \   (P R) = 256 * A / Q
                            \         = 256 * cloud counter / distance
                            \
                            \ We are going to use this as our cloud size, so the
                            \ further away the cloud, the smaller it is, and as the
                            \ cloud counter ticks onward, the cloud expands
    
     LDA P                  \ Set A = P, so we now have:
                            \
                            \   (A R) = 256 * cloud counter / distance
    
     CMP #&1C               \ If A < 28, skip the next two instructions
     BCC P%+6
    
     LDA #&FE               \ Set A = 254 and skip the following (this BNE is
     BNE LABEL_1            \ effectively a JMP as A is never zero)
    
     ASL R                  \ Shift (A R) left three times to multiply by 8
     ROL A
     ASL R
     ROL A
     ASL R
     ROL A
    
                            \ Overall, the above multiplies (A R) by 8 to leave a
                            \ one-byte cloud size in A, given by the following:
                            \
                            \   A = 8 * cloud counter / distance
    
    .LABEL_1
    
     DEY                    \ Decrement Y to 0
    
     STA (XX19),Y           \ Store the cloud size in byte #0 of the ship line heap
    
     LDA INWK+31            \ Clear bit 6 of the ship's byte #31 to denote that the
     AND #%10111111         \ explosion has not yet been drawn
     STA INWK+31
    
     AND #%00001000         \ If bit 3 of the ship's byte #31 is clear, then nothing
     BEQ TT48               \ is being drawn on-screen for this ship anyway, so
                            \ return from the subroutine (as TT48 contains an RTS)
    
     LDY #2                 \ Otherwise it's time to draw an explosion cloud, so
     LDA (XX19),Y           \ fetch byte #2 of the ship line heap into Y, which we
     TAY                    \ set to the explosion count for this ship (i.e. the
                            \ number of vertices used as origins for explosion
                            \ clouds)
                            \
                            \ The explosion count is stored as 4 * n + 6, where n is
                            \ the number of vertices, so the following loop copies
                            \ the coordinates of the first n vertices from the heap
                            \ at XX3, which is where we stored all the visible
                            \ vertex coordinates in part 8 of the LL9 routine, and
                            \ sticks them in the ship line heap pointed to by XX19,
                            \ starting at byte #7 (so it leaves the first 6 bytes of
                            \ the ship line heap alone)
    
    .EXL1
    
     LDA XX3-7,Y            \ Copy byte Y-7 from the XX3 heap, into the Y-th byte of
     STA (XX19),Y           \ the ship line heap
    
     DEY                    \ Decrement the loop counter
    
     CPY #6                 \ Keep copying vertex coordinates into the ship line
     BNE EXL1               \ heap until Y = 6 (which will copy n vertices, where n
                            \ is the number of vertices we should be exploding)
    
     LDA INWK+31            \ Set bit 6 of the ship's byte #31 to denote that the
     ORA #%01000000         \ explosion has been drawn (as it's about to be)
     STA INWK+31
    
    .PTCLS
    
                            \ This part of the routine actually draws the explosion
                            \ cloud
    
     LDY #0                 \ Fetch byte #0 of the ship line heap, which contains
     LDA (XX19),Y           \ the cloud size we stored above, and store it in Q
     STA Q
    
     INY                    \ Increment the index in Y to point to byte #1
    
     LDA (XX19),Y           \ Fetch byte #1 of the ship line heap, which contains
                            \ the cloud counter. We are now going to process this
                            \ into the number of particles in each vertex's cloud
    
     BPL P%+4               \ If the cloud counter < 128, then we are in the first
                            \ half of the cloud's existence, so skip the next
                            \ instruction
    
     EOR #&FF               \ Flip the value of A so that in the second half of the
                            \ cloud's existence, A counts down instead of up
    
     LSR A                  \ Divide A by 16 so that is has a maximum value of 7
     LSR A
     LSR A
     LSR A
    
     ORA #1                 \ Make sure A is at least 1 and store it in U, to
     STA U                  \ give us the number of particles in the explosion for
                            \ each vertex
    
     INY                    \ Increment the index in Y to point to byte #2
    
     LDA (XX19),Y           \ Fetch byte #2 of the ship line heap, which contains
     STA TGT                \ the explosion count for this ship (i.e. the number of
                            \ vertices used as origins for explosion clouds) and
                            \ store it in TGT
    
     LDA RAND+1             \ Fetch the current random number seed in RAND+1 and
     PHA                    \ store it on the stack, so we can re-randomise the
                            \ seeds when we are done
    
     LDY #6                 \ Set Y = 6 to point to the byte before the first vertex
                            \ coordinate we stored on the ship line heap above (we
                            \ increment it below so it points to the first vertex)
    
    .EXL5
    
     LDX #3                 \ We are about to fetch a pair of coordinates from the
                            \ ship line heap, so set a counter in X for 4 bytes
    
    .EXL3
    
     INY                    \ Increment the index in Y so it points to the next byte
                            \ from the coordinate we are copying
    
     LDA (XX19),Y           \ Copy the Y-th byte from the ship line heap to the X-th
     STA K3,X               \ byte of K3
    
     DEX                    \ Decrement the X index
    
     BPL EXL3               \ Loop back to EXL3 until we have copied all four bytes
    
                            \ The above loop copies the vertex coordinates from the
                            \ ship line heap to K3, reversing them as we go, so it
                            \ sets the following:
                            \
                            \   K3+3 = x_lo
                            \   K3+2 = x_hi
                            \   K3+1 = y_lo
                            \   K3+0 = y_hi
    
     STY CNT                \ Set CNT to the index that points to the next vertex on
                            \ the ship line heap
    
     LDY #2                 \ Set Y = 2, which we will use to point to bytes #3 to
                            \ #6, after incrementing it
    
                            \ This next loop copies bytes #3 to #6 from the ship
                            \ line heap into the four random number seeds in RAND to
                            \ RAND+3, EOR'ing them with the vertex index so they are
                            \ different for every vertex. This enables us to
                            \ generate random numbers for drawing each vertex that
                            \ are random but repeatable, which we need when we
                            \ redraw the cloud to remove it
                            \
                            \ Note that we haven't actually set the values of bytes
                            \ #3 to #6 in the ship line heap, so we have no idea
                            \ what they are, we just use what's already there. But
                            \ the fact that those bytes are stored for this ship
                            \ means we can repeat the random generation of the
                            \ cloud, which is the important bit
    
    .EXL2
    
     INY                    \ Increment the index in Y so it points to the next
                            \ random number seed to copy
    
     LDA (XX19),Y           \ Fetch the Y-th byte from the ship line heap
    
     EOR CNT                \ EOR with the vertex index, so the seeds are different
                            \ for each vertex
    
    IF _SNG47
    
     STA &FFFF,Y            \ Y is going from 3 to 6, so this stores the four bytes
                            \ in memory locations &02, &03, &04 and &05, which are
                            \ the memory locations of RAND through RAND+3
    
    ELIF _COMPACT
    
     STA &0000,Y            \ Y is going from 3 to 6, so this stores the four bytes
                            \ in memory locations &03, &04, &05 and &06, which are
                            \ the memory locations of RAND through RAND+3
    
    ENDIF
    
     CPY #6                 \ Loop back to EXL2 until Y = 6, which means we have
     BNE EXL2               \ copied four bytes
    
     LDY U                  \ Set Y to the number of particles in the explosion for
                            \ each vertex, which we stored in U above. We will now
                            \ use this as a loop counter to iterate through all the
                            \ particles in the explosion
    
     STY CNT2               \ Store the number of explosion particles in CNT2, to
                            \ use this as a loop counter to iterate through all the
                            \ particles in the explosion
    
    .EXL4
    
     CLC                    \ This contains the code from the DORND2 routine, so
     LDA RAND               \ this section is exactly equivalent to a JSR DORND2
     ROL A                  \ call, but is slightly faster as it's been inlined
     TAX                    \ (so it sets A and X to random values, making sure
     ADC RAND+2             \ the C flag doesn't affect the outcome)
     STA RAND
     STX RAND+2
     LDA RAND+1
     TAX
     ADC RAND+3
     STA RAND+1
     STX RAND+3
    
     STA ZZ                 \ Set ZZ to a random number
    
     AND #3                 \ Set X to this random number, reduced to be in the
     TAX                    \ range 0-3
    
     LDA coltabl,X          \ Set the colour randomly to one of the four colours
     STA COL                \ in the coltabl table, so explosions are made up of
                            \ yellow, red and cyan particles
    
     LDA K3+1               \ Set (A R) = (y_hi y_lo)
     STA R                  \           = y
     LDA K3
    
     JSR EXS1               \ Set (A X) = (A R) +/- random * cloud size
                            \           = y +/- random * cloud size
    
     BNE EX11               \ If A is non-zero, the particle is off-screen as the
                            \ coordinate is bigger than 255), so jump to EX11 to do
                            \ the next particle
    
     CPX #2*Y-1             \ If X > the y-coordinate of the bottom of the screen,
     BCS EX11               \ the particle is off the bottom of the screen, so jump
                            \ to EX11 to do the next particle
    
                            \ Otherwise X contains a random y-coordinate within the
                            \ cloud
    
     STX Y1                 \ Set Y1 = our random y-coordinate within the cloud
    
     LDA K3+3               \ Set (A R) = (x_hi x_lo)
     STA R
     LDA K3+2
    
     JSR EXS1               \ Set (A X) = (A R) +/- random * cloud size
                            \           = x +/- random * cloud size
    
     BNE EX4                \ If A is non-zero, the particle is off-screen as the
                            \ coordinate is bigger than 255), so jump to EX4 to do
                            \ the next particle
    
                            \ Otherwise X contains a random x-coordinate within the
                            \ cloud
    
     LDA Y1                 \ Set A = our random y-coordinate within the cloud
    
     JSR PIXEL              \ Draw a point at screen coordinate (X, A) with the
                            \ point size determined by the distance in ZZ
    
    .EX4
    
     DEC CNT2               \ Decrement the loop counter in CNT2 for the next
                            \ particle
    
     BPL EXL4               \ Loop back to EXL4 until we have done all the particles
                            \ in the cloud
    
     LDY CNT                \ Set Y to the index that points to the next vertex on
                            \ the ship line heap
    
     CPY TGT                \ If Y < TGT, which we set to the explosion count for
     BCC EXL5               \ this ship (i.e. the number of vertices used as origins
                            \ for explosion clouds), loop back to EXL5 to do a cloud
                            \ for the next vertex
    
     PLA                    \ Restore the current random number seed to RAND+1 that
     STA RAND+1             \ we stored at the start of the routine
    
     LDA K%+6               \ Store the z_lo coordinate for the planet (which will
     STA RAND+3             \ be pretty random) in the RAND+3 seed
    
     RTS                    \ Return from the subroutine
    
    .EX11
    
     CLC                    \ This contains the code from the DORND2 routine, so
     LDA RAND               \ this section is exactly equivalent to a JSR DORND2
     ROL A                  \ call, but is slightly faster as it's been inlined
     TAX                    \ (so it sets A and X to random values, making sure
     ADC RAND+2             \ the C flag doesn't affect the outcome)
     STA RAND
     STX RAND+2
     LDA RAND+1
     TAX
     ADC RAND+3
     STA RAND+1
     STX RAND+3
    
     JMP EX4                \ We just skipped a particle, so jump up to EX4 to do
                            \ the next one
    
    .EXS1
    
                            \ This routine calculates the following:
                            \
                            \   (A X) = (A R) +/- random * cloud size
                            \
                            \ returning with the flags set for the high byte in A
    
     STA S                  \ Store A in S so we can use it later
    
     CLC                    \ This contains the code from the DORND2 routine, so
     LDA RAND               \ this section is exactly equivalent to a JSR DORND2
     ROL A                  \ call, but is slightly faster as it's been inlined
     TAX                    \ (so it sets A and X to random values, making sure
     ADC RAND+2             \ the C flag doesn't affect the outcome)
     STA RAND
     STX RAND+2
     LDA RAND+1
     TAX
     ADC RAND+3
     STA RAND+1
     STX RAND+3
    
     ROL A                  \ Set A = A * 2
    
     BCS EX5                \ If bit 7 of A was set (50% chance), jump to EX5
    
     JSR FMLTU              \ Set A = A * Q / 256
                            \       = random << 1 * projected cloud size / 256
    
     ADC R                  \ Set (A X) = (S R) + A
     TAX                    \           = (S R) + random * projected cloud size
                            \
                            \ where S contains the argument A, starting with the low
                            \ bytes
    
     LDA S                  \ And then the high bytes
     ADC #0
    
     RTS                    \ Return from the subroutine
    
    .EX5
    
     JSR FMLTU              \ Set T = A * Q / 256
     STA T                  \       = random << 1 * projected cloud size / 256
    
     LDA R                  \ Set (A X) = (S R) - T
     SBC T                  \
     TAX                    \ where S contains the argument A, starting with the low
                            \ bytes
    
     LDA S                  \ And then the high bytes
     SBC #0
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: exlook                                                  [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: A table to shift X left by one place when X is 0 or 1
    
    
        Context: See this variable [on its own page](../main/variable/exlook.md)
     References: No direct references to this variable in this source file
    
    
    
    
    
    
    .exlook
    
     EQUB %00               \ Looking up exlook,X will return X shifted left by one
     EQUB %10               \ place, where X is 0 or 1
                            \
                            \ This is not used in this version of Elite; it is left
                            \ over from the Commodore 64 version of Elite
    
    
    
    
           Name: coltabl                                                 [Show more]
           Type: Variable
       Category: Drawing ships
        Summary: Colours for ship explosions
    
    
        Context: See this variable [on its own page](../main/variable/coltabl.md)
     References: This variable is used as follows:
                 * [DOEXP](../main/subroutine/doexp.md) uses coltabl
    
    
    
    
    
    
    .coltabl
    
     EQUB YELLOW
     EQUB RED
     EQUB YELLOW
     EQUB CYAN
    
    
    
    
           Name: SOS1                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Update the missile indicators, set up the planet data block
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sos1.md)
     References: This subroutine is called as follows:
                 * [SOLAR](../main/subroutine/solar.md) calls SOS1
                 * [TT110](../main/subroutine/tt110.md) calls SOS1
    
    
    
    
    
    * * *
    
    
     Update the missile indicators, and set up a data block for the planet, but
     only setting the pitch and roll counters to 127 (no damping).
    
    
    
    
    .SOS1
    
     JSR msblob             \ Reset the dashboard's missile indicators so none of
                            \ them are targeted
    
     LDA #127               \ Set the pitch and roll counters to 127, so that's a
     STA INWK+29            \ clockwise roll and a diving pitch with no damping, so
     STA INWK+30            \ the planet's rotation doesn't slow down
    
     LDA tek                \ Set A = 128 or 130 depending on bit 1 of the system's
     AND #%00000010         \ tech level in tek
     ORA #%10000000
    
     JMP NWSHP              \ Add a new planet to our local bubble of universe,
                            \ with the planet type defined by A (128 is a planet
                            \ with an equator and meridian, 130 is a planet with
                            \ a crater)
    
    
    
    
           Name: SOLAR                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Set up various aspects of arriving in a new system
      Deep dive: [A sense of scale](https://elite.bbcelite.com/deep_dives/a_sense_of_scale.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/solar.md)
     Variations: See [code variations](../related/compare/main/subroutine/solar.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [TT18](../main/subroutine/tt18.md) calls SOLAR
    
    
    
    
    
    * * *
    
    
     Halve our legal status, update the missile indicators, and set up data blocks
     and slots for the planet and sun.
    
    
    
    
    .SOLAR
    
     LDA TRIBBLE            \ If we have no Trumbles in the hold, skip to nobirths
     BEQ nobirths
    
                            \ If we get here then we have Trumbles in the hold, so
                            \ this is where they breed (though we never get here in
                            \ the Master version as the number of Trumbles is always
                            \ zero)
    
     LDA #0                 \ Trumbles eat food and narcotics during the hyperspace
     STA QQ20               \ journey, so zero the amount of food and narcotics in
     STA QQ20+6             \ the hold
    
     JSR DORND              \ Take the number of Trumbles from TRIBBLE(1 0), add a
     AND #15                \ random number between 4 and 15, and double the result,
     ADC TRIBBLE            \ storing the resulting number in TRIBBLE(1 0)
     ORA #4                 \
     ROL A                  \ We start with the low byte
     STA TRIBBLE
    
     ROL TRIBBLE+1          \ And then do the high byte
    
     BPL P%+5               \ If bit 7 of the high byte is set, then rotate the high
     ROR TRIBBLE+1          \ byte back to the right, so the number of Trumbles is
                            \ always positive
    
    .nobirths
    
     LSR FIST               \ Halve our legal status in FIST, making us less bad,
                            \ and moving bit 0 into the C flag (so every time we
                            \ arrive in a new system, our legal status improves a
                            \ bit)
    
     JSR ZINF               \ Call ZINF to reset the INWK ship workspace, which
                            \ doesn't affect the C flag
    
     LDA QQ15+1             \ Fetch s0_hi
    
     AND #%00000011         \ Extract bits 0-1 (which also help to determine the
                            \ economy), which will be between 0 and 3
    
     ADC #3                 \ Add 3 + C, to get a result between 3 and 7, clearing
                            \ the C flag in the process
    
     STA INWK+8             \ Store the result in z_sign in byte #6
    
     ROR A                  \ Halve A, rotating in the C flag (which is clear) and
     STA INWK+2             \ store in both x_sign and y_sign, moving the planet to
     STA INWK+5             \ the upper right
    
     JSR SOS1               \ Call SOS1 to set up the planet's data block and add it
                            \ to FRIN, where it will get put in the first slot as
                            \ it's the first one to be added to our local bubble of
                            \ this new system's universe
    
     LDA QQ15+3             \ Fetch s1_hi, extract bits 0-2, set bits 0 and 7 and
     AND #%00000111         \ store in z_sign, so the sun is behind us at a distance
     ORA #%10000001         \ of 1 to 7
     STA INWK+8
    
     LDA QQ15+5             \ Fetch s2_hi, extract bits 0-1 and store in x_sign and
     AND #%00000011         \ y_sign, so the sun is either dead centre in our rear
     STA INWK+2             \ laser crosshairs, or off to the top left by a distance
     STA INWK+1             \ of 1 or 2 when we look out the back
    
     LDA #0                 \ Set the pitch and roll counters to 0 (no rotation)
     STA INWK+29
     STA INWK+30
    
     LDA #129               \ Set A = 129, the ship type for the sun
    
     JSR NWSHP              \ Call NWSHP to set up the sun's data block and add it
                            \ to FRIN, where it will get put in the second slot as
                            \ it's the second one to be added to our local bubble
                            \ of this new system's universe
    
    
    
    
           Name: NWSTARS                                                 [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Initialise the stardust field
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nwstars.md)
     Variations: See [code variations](../related/compare/main/subroutine/nwstars.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOOK1](../main/subroutine/look1.md) calls NWSTARS
    
    
    
    
    
    * * *
    
    
     This routine is called when the space view is initialised in routine LOOK1.
    
    
    
    
    .NWSTARS
    
     LDA QQ11               \ If this is not a space view, jump to WPSHPS to skip
    \ORA MJ                 \ the initialisation of the SX, SY and SZ tables. The OR
     BNE WPSHPS             \ instruction is commented out in the original source,
                            \ but it would have the effect of also skipping the
                            \ initialisation if we had mis-jumped into witchspace
    
    
    
    
           Name: nWq                                                     [Show more]
           Type: Subroutine
       Category: Stardust
        Summary: Create a random cloud of stardust
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nwq.md)
     Variations: See [code variations](../related/compare/main/subroutine/nwq.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](../main/subroutine/death.md) calls nWq
    
    
    
    
    
    * * *
    
    
     Create a random cloud of stardust containing the correct number of dust
     particles, i.e. NOSTM of them, which is 3 in witchspace and 18 (#NOST) in
     normal space. Also clears the scanner and initialises the LSO block.
    
     This is called by the DEATH routine when it displays our untimely demise.
    
    
    
    
    .nWq
    
     LDA #DUST              \ Switch to stripe 3-2-3-2, which is cyan/red in the
     STA COL                \ space view
    
     LDY NOSTM              \ Set Y to the current number of stardust particles, so
                            \ we can use it as a counter through all the stardust
    
    .SAL4
    
     JSR DORND              \ Set A and X to random numbers
    
     ORA #8                 \ Set A so that it's at least 8
    
     STA SZ,Y               \ Store A in the Y-th particle's z_hi coordinate at
                            \ SZ+Y, so the particle appears in front of us
    
     STA ZZ                 \ Set ZZ to the particle's z_hi coordinate
    
     JSR DORND              \ Set A and X to random numbers
    
     STA SX,Y               \ Store A in the Y-th particle's x_hi coordinate at
                            \ SX+Y, so the particle appears in front of us
    
     STA X1                 \ Set X1 to the particle's x_hi coordinate
    
     JSR DORND              \ Set A and X to random numbers
    
     STA SY,Y               \ Store A in the Y-th particle's y_hi coordinate at
                            \ SY+Y, so the particle appears in front of us
    
     STA Y1                 \ Set Y1 to the particle's y_hi coordinate
    
     JSR PIXEL2             \ Draw a stardust particle at (X1,Y1) with distance ZZ
    
     DEY                    \ Decrement the counter to point to the next particle of
                            \ stardust
    
     BNE SAL4               \ Loop back to SAL4 until we have randomised all the
                            \ stardust particles
    
    \JSR PBFL               \ This instruction is commented out in the original
                            \ source
    
                            \ Fall through into WPSHPS to clear the scanner and
                            \ reset the LSO block
    
    
    
    
           Name: WPSHPS                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Clear the scanner, reset the ball line and sun line heaps
    
    
        Context: See this subroutine [on its own page](../main/subroutine/wpshps.md)
     Variations: See [code variations](../related/compare/main/subroutine/wpshps.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LOOK1](../main/subroutine/look1.md) calls WPSHPS
                 * [NWSTARS](../main/subroutine/nwstars.md) calls WPSHPS
                 * [RES2](../main/subroutine/res2.md) calls WPSHPS
    
    
    
    
    
    * * *
    
    
     Remove all ships from the scanner, reset the sun line heap at LSO, and reset
     the ball line heap at LSX2 and LSY2.
    
    
    
    
    .WPSHPS
    
     LDX #0                 \ Set up a counter in X to work our way through all the
                            \ ship slots in FRIN
    
    .WSL1
    
     LDA FRIN,X             \ Fetch the ship type in slot X
    
     BEQ WS2                \ If the slot contains 0 then it is empty and we have
                            \ checked all the slots (as they are always shuffled
                            \ down in the main loop to close up and gaps), so jump
                            \ to WS2 as we are done
    
     BMI WS1                \ If the slot contains a ship type with bit 7 set, then
                            \ it contains the planet or the sun, so jump down to WS1
                            \ to skip this slot, as the planet and sun don't appear
                            \ on the scanner
    
     STA TYPE               \ Store the ship type in TYPE
    
     JSR GINF               \ Call GINF to get the address of the data block for
                            \ ship slot X and store it in INF
    
     LDY #31                \ We now want to copy the first 32 bytes from the ship's
                            \ data block into INWK, so set a counter in Y
    
    .WSL2
    
     LDA (INF),Y            \ Copy the Y-th byte from the data block pointed to by
     STA INWK,Y             \ INF into the Y-th byte of INWK workspace
    
     DEY                    \ Decrement the counter to point at the next byte
    
     BPL WSL2               \ Loop back to WSL2 until we have copied all 32 bytes
    
     STX XSAV               \ Store the ship slot number in XSAV while we call SCAN
    
     JSR SCAN               \ Call SCAN to plot this ship on the scanner, which will
                            \ remove it as it's plotted with EOR logic
    
     LDX XSAV               \ Restore the ship slot number from XSAV into X
    
     LDY #31                \ Clear bits 3, 4 and 6 in the ship's byte #31, which
     LDA (INF),Y            \ stops drawing the ship on-screen (bit 3), hides it
     AND #%10100111         \ from the scanner (bit 4) and stops any lasers firing
     STA (INF),Y            \ (bit 6)
    
    .WS1
    
     INX                    \ Increment X to point to the next ship slot
    
     BNE WSL1               \ Loop back up to process the next slot (this BNE is
                            \ effectively a JMP as X will never be zero)
    
    .WS2
    
     LDX #0                 \ Reset the ball line heap by setting the ball line heap
     STX LSP                \ pointer to 0
    
     DEX                    \ Set X = &FF
    
     STX LSX2               \ Set LSX2 = LSY2 = &FF to clear the ball line heap
     STX LSY2
    
                            \ Fall through into FLFLLS to reset the LSO block
    
    
    
    
           Name: FLFLLS                                                  [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Reset the sun line heap
    
    
        Context: See this subroutine [on its own page](../main/subroutine/flflls.md)
     Variations: See [code variations](../related/compare/main/subroutine/flflls.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [KS4](../main/subroutine/ks4.md) calls FLFLLS
                 * [TT23](../main/subroutine/tt23.md) calls FLFLLS
                 * [TTX66K](../main/subroutine/ttx66k.md) calls FLFLLS
    
    
    
    
    
    * * *
    
    
     Reset the sun line heap at LSO by zero-filling it and setting the first byte
     to &FF.
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is set to 0
    
    
    
    
    .FLFLLS
    
     LDY #2*Y-1+8           \ #Y is the y-coordinate of the centre of the space
                            \ view, so this sets Y as a counter for the number of
                            \ lines in the space view (i.e. 191) - which is also the
                            \ number of lines in the LSO block - plus an extra 8
                            \ bytes
    
     LDA #0                 \ Set A to 0 so we can zero-fill the LSO block
    
    .SAL6
    
     STA LSO,Y              \ Set the Y-th byte of the LSO block to 0
    
     DEY                    \ Decrement the counter
    
     BNE SAL6               \ Loop back until we have filled all the way to LSO+1
    
     DEY                    \ Decrement Y to value of &FF (as we exit the above loop
                            \ with Y = 0)
    
     STY LSX                \ Set the first byte of the LSO block, which has its own
                            \ label LSX, to &FF, to indicate that the sun line heap
                            \ is empty
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: DET1                                                    [Show more]
           Type: Subroutine
       Category: Drawing the screen
        Summary: Show or hide the dashboard (for when we die)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/det1.md)
     Variations: See [code variations](../related/compare/main/subroutine/det1-dodials.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DEATH](../main/subroutine/death.md) calls DET1
    
    
    
    
    
    * * *
    
    
     This routine sets the screen to show the number of text rows given in X.
    
     It is used when we are killed, as reducing the number of rows from the usual
     31 to 24 has the effect of hiding the dashboard, leaving a monochrome image
     of ship debris and explosion clouds. Increasing the rows back up to 31 makes
     the dashboard reappear, as the dashboard's screen memory doesn't get touched
     by this process.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The number of text rows to display on the screen (24
                            will hide the dashboard, 31 will make it reappear)
    
    
    
    * * *
    
    
     Returns:
    
       A                    A is set to 6
    
    
    
    
    .DET1
    
     LDA #6                 \ Set A to 6 so we can update 6845 register R6 below
    
     SEI                    \ Disable interrupts so we can update the 6845
    
     STA VIA+&00            \ Set 6845 register R6 to the value in X. Register R6
     STX VIA+&01            \ is the "vertical displayed" register, which sets the
                            \ number of rows shown on the screen
    
     CLI                    \ Re-enable interrupts
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SHD                                                     [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Charge a shield and drain some energy from the energy banks
    
    
        Context: See this subroutine [on its own page](../main/subroutine/shd.md)
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 13 of 16)](../main/subroutine/main_flight_loop_part_13_of_16.md) calls SHD
    
    
    
    
    
    * * *
    
    
     Charge up a shield, and if it needs charging, drain some energy from the
     energy banks.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The value of the shield to recharge
    
    
    
    
     DEX                    \ If we get here then we just incremented the shield
                            \ value back around to zero, so decrement it back down
                            \ to 255 so it stays at the maximum value of 255
    
     RTS                    \ Return from the subroutine
    
    .SHD
    
     INX                    \ Increment the shield value
    
     BEQ SHD-2              \ If the shield value is 0 then this means it was 255
                            \ before, which is the maximum value, so jump to SHD-2
                            \ to bring it back down to 255 and return without
                            \ draining our energy banks
    
                            \ Otherwise fall through into DENGY to drain our energy
                            \ to pay for all this shield charging
    
    
    
    
           Name: DENGY                                                   [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Drain some energy from the energy banks
    
    
        Context: See this subroutine [on its own page](../main/subroutine/dengy.md)
     Variations: See [code variations](../related/compare/main/subroutine/dengy.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LASLI](../main/subroutine/lasli.md) calls DENGY
                 * [Main flight loop (Part 16 of 16)](../main/subroutine/main_flight_loop_part_16_of_16.md) calls DENGY
    
    
    
    
    
    * * *
    
    
     Returns:
    
       Z flag               Set if we have no energy left, clear otherwise
    
    
    
    
    .DENGY
    
     DEC ENERGY             \ Decrement the energy banks in ENERGY
    
     PHP                    \ Save the flags on the stack
    
     BNE paen2              \ If the energy levels are not yet zero, skip the
                            \ following instruction
    
     INC ENERGY             \ The minimum allowed energy level is 1, and we just
                            \ reached 0, so increment ENERGY back to 1
    
    .paen2
    
     PLP                    \ Restore the flags from the stack, so we return with
                            \ the Z flag from the DEC instruction above
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: COMPAS                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Update the compass
    
    
        Context: See this subroutine [on its own page](../main/subroutine/compas.md)
     Variations: See [code variations](../related/compare/main/subroutine/compas.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [DIALS (Part 4 of 4)](../main/subroutine/dials_part_4_of_4.md) calls COMPAS
    
    
    
    
    
    
    .COMPAS
    
     JSR DOT                \ Call DOT to redraw (i.e. remove) the current compass
                            \ dot
    
     LDA SSPR               \ If we are inside the space station safe zone, jump to
     BNE SP1                \ SP1 to draw the space station on the compass
    
     JSR SPS1               \ Otherwise we need to draw the planet on the compass,
                            \ so first call SPS1 to calculate the vector to the
                            \ planet and store it in XX15
    
     JMP SP2                \ Jump to SP2 to draw XX15 on the compass, returning
                            \ from the subroutine using a tail call
    
    
    
    
           Name: SPS2                                                    [Show more]
           Type: Subroutine
       Category: Maths (Arithmetic)
        Summary: Calculate (Y X) = A / 10
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sps2.md)
     Variations: See [code variations](../related/compare/main/subroutine/sps2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [SP2](../main/subroutine/sp2.md) calls SPS2
    
    
    
    
    
    * * *
    
    
     Calculate the following, where A is a sign-magnitude 8-bit integer and the
     result is a signed 16-bit integer:
    
       (Y X) = A / 10
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .SPS2
    
     ASL A                  \ Set X = |A| * 2, and set the C flag to the sign bit of
     TAX                    \ A
    
     LDA #0                 \ Set Y to have the sign bit from A in bit 7, with the
     ROR A                  \ rest of its bits zeroed, so Y now contains the sign of
     TAY                    \ the original argument
    
     LDA #20                \ Set Q = 20
     STA Q
    
     TXA                    \ Copy X into A, so A now contains the argument A * 2
    
     JSR DVID4              \ Calculate the following:
                            \
                            \   P = A / Q
                            \     = |argument A| * 2 / 20
                            \     = |argument A| / 10
    
     LDX P                  \ Set X to the result
    
     TYA                    \ If the sign of the original argument A is negative,
     BMI LL163              \ jump to LL163 to flip the sign of the result
    
     LDY #0                 \ Set the high byte of the result to 0, as the result is
                            \ positive
    
     RTS                    \ Return from the subroutine
    
    .LL163
    
     LDY #&FF               \ The result is negative, so set the high byte to &FF
    
     TXA                    \ Flip the low byte and add 1 to get the negated low
     EOR #&FF               \ byte, using two's complement
     TAX
     INX
    
    .COR1
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SPS4                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Calculate the vector to the space station
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sps4.md)
     References: This subroutine is called as follows:
                 * [SP1](../main/subroutine/sp1.md) calls SPS4
    
    
    
    
    
    * * *
    
    
     Calculate the vector between our ship and the space station and store it in
     XX15.
    
    
    
    
    .SPS4
    
     LDX #8                 \ First we need to copy the space station's coordinates
                            \ into K3, so set a counter to copy the first 9 bytes
                            \ (the three-byte x, y and z coordinates) from the
                            \ station's data block at K% + NI% into K3
    
    .SPL1
    
     LDA K%+NI%,X           \ Copy the X-th byte from the station's data block at
     STA K3,X               \ K% + NI% to the X-th byte of K3
    
     DEX                    \ Decrement the loop counter
    
     BPL SPL1               \ Loop back to SPL1 until we have copied all 9 bytes
    
     JMP TAS2               \ Call TAS2 to build XX15 from K3, returning from the
                            \ subroutine using a tail call
    
    
    
    
           Name: SP1                                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Draw the space station on the compass
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sp1.md)
     References: This subroutine is called as follows:
                 * [COMPAS](../main/subroutine/compas.md) calls SP1
    
    
    
    
    
    
    .SP1
    
     JSR SPS4               \ Call SPS4 to calculate the vector to the space station
                            \ and store it in XX15
    
                            \ Fall through into SP2 to draw XX15 on the compass
    
    
    
    
           Name: SP2                                                     [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Draw a dot on the compass, given the planet/station vector
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sp2.md)
     Variations: See [code variations](../related/compare/main/subroutine/sp2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [COMPAS](../main/subroutine/compas.md) calls SP2
    
    
    
    
    
    * * *
    
    
     Draw a dot on the compass to represent the planet or station, whose normalised
     vector is in XX15.
    
       XX15 to XX15+2      The normalised vector to the planet or space station,
                           stored as x in XX15, y in XX15+1 and z in XX15+2
    
    
    
    
    .SP2
    
     LDA XX15               \ Set A to the x-coordinate of the planet or station to
                            \ show on the compass, which will be in the range -96 to
                            \ +96 as the vector has been normalised
    
     JSR SPS2               \ Set (Y X) = A / 10, so X will be from -9 to +9, which
                            \ is the x-offset from the centre of the compass of the
                            \ dot we want to draw. Returns with the C flag clear
    
     TXA                    \ Set COMX = 195 + X, as 186 is the pixel x-coordinate
     ADC #195               \ of the leftmost dot possible on the compass, and X can
     STA COMX               \ be -9, which would be 195 - 9 = 186. This also means
                            \ that the highest value for COMX is 195 + 9 = 204,
                            \ which is the pixel x-coordinate of the rightmost dot
                            \ in the compass... but the compass dot is actually two
                            \ pixels wide, so the compass dot can overlap the right
                            \ edge of the compass, but not the left edge
    
     LDA XX15+1             \ Set A to the y-coordinate of the planet or station to
                            \ show on the compass, which will be in the range -96 to
                            \ +96 as the vector has been normalised
    
     JSR SPS2               \ Set (Y X) = A / 10, so X will be from -9 to +9, which
                            \ is the x-offset from the centre of the compass of the
                            \ dot we want to draw. Returns with the C flag clear
    
     STX T                  \ Set COMY = 204 - X, as 203 is the pixel y-coordinate
     LDA #204               \ of the centre of the compass, the C flag is clear,
     SBC T                  \ and the y-axis needs to be flipped around (because
     STA COMY               \ when the planet or station is above us, and the
                            \ vector is therefore positive, we want to show the dot
                            \ higher up on the compass, which has a smaller pixel
                            \ y-coordinate). So this calculation does this:
                            \
                            \   COMY = 204 - X - (1 - 0) = 203 - X
    
     LDA #YELLOW2           \ Set A to yellow, the colour for when the planet or
                            \ station in the compass is in front of us
    
     LDX XX15+2             \ If the z-coordinate of the XX15 vector is positive,
     BPL P%+4               \ skip the following instruction
    
     LDA #GREEN2            \ The z-coordinate of XX15 is negative, so the planet or
                            \ station is behind us and the compass dot should be in
                            \ green, so set A accordingly
    
     STA COMC               \ Store the compass colour in COMC
    
     JMP DOT                \ Jump to DOT to draw the dot on the compass and return
                            \ from the subroutine using a tail call
    
    
    
    
           Name: OOPS                                                    [Show more]
           Type: Subroutine
       Category: Flight
        Summary: Take some damage
    
    
        Context: See this subroutine [on its own page](../main/subroutine/oops.md)
     Variations: See [code variations](../related/compare/main/subroutine/oops.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 10 of 16)](../main/subroutine/main_flight_loop_part_10_of_16.md) calls OOPS
                 * [TACTICS (Part 1 of 7)](../main/subroutine/tactics_part_1_of_7.md) calls OOPS
                 * [TACTICS (Part 6 of 7)](../main/subroutine/tactics_part_6_of_7.md) calls OOPS
    
    
    
    
    
    * * *
    
    
     We just took some damage, so reduce the shields if we have any, or reduce the
     energy levels and potentially take some damage to the cargo if we don't.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The amount of damage to take
    
       INF                  The address of the ship block for the ship that attacked
                            us, or the ship that we just ran into
    
    
    
    
    .OOPS
    
     STA T                  \ Store the amount of damage in T
    
     LDX #0                 \ Fetch byte #8 (z_sign) for the ship attacking us, and
     LDY #8                 \ set X = 0
     LDA (INF),Y
    
     BMI OO1                \ If A is negative, then we got hit in the rear, so jump
                            \ to OO1 to process damage to the aft shield
    
     LDA FSH                \ Otherwise the forward shield was damaged, so fetch the
     SBC T                  \ shield strength from FSH and subtract the damage in T
    
     BCC OO2                \ If the C flag is clear then this amount of damage was
                            \ too much for the shields, so jump to OO2 to set the
                            \ shield level to 0 and start taking damage directly
                            \ from the energy banks
    
     STA FSH                \ Store the new value of the forward shield in FSH
    
     RTS                    \ Return from the subroutine
    
    .OO2
    
     LDX #0                 \ Set the forward shield to 0
     STX FSH
    
     BCC OO3                \ Jump to OO3 to start taking damage directly from the
                            \ energy banks (this BCC is effectively a JMP as the C
                            \ flag is clear, as we jumped to OO2 with a BCC)
    
    .OO1
    
     LDA ASH                \ The aft shield was damaged, so fetch the shield
     SBC T                  \ strength from ASH and subtract the damage in T
    
     BCC OO5                \ If the C flag is clear then this amount of damage was
                            \ too much for the shields, so jump to OO5 to set the
                            \ shield level to 0 and start taking damage directly
                            \ from the energy banks
    
     STA ASH                \ Store the new value of the aft shield in ASH
    
     RTS                    \ Return from the subroutine
    
    .OO5
    
     LDX #0                 \ Set the forward shield to 0
     STX ASH
    
    .OO3
    
     ADC ENERGY             \ A is negative and contains the amount by which the
     STA ENERGY             \ damage overwhelmed the shields, so this drains the
                            \ energy banks by that amount (and because the energy
                            \ banks are shown over four indicators rather than one,
                            \ but with the same value range of 0-255, energy will
                            \ appear to drain away four times faster than the
                            \ shields did)
    
     BEQ P%+4               \ If we have just run out of energy, skip the next
                            \ instruction to jump straight to our death
    
     BCS P%+5               \ If the C flag is set, then subtracting the damage from
                            \ the energy banks didn't underflow, so we had enough
                            \ energy to survive, and we can skip the next
                            \ instruction to make a sound and take some damage
    
     JMP DEATH              \ Otherwise our energy levels are either 0 or negative,
                            \ and in either case that means we jump to our DEATH,
                            \ returning from the subroutine using a tail call
    
     JSR EXNO3              \ We didn't die, so call EXNO3 to make the sound of a
                            \ collision
    
     JMP OUCH               \ And jump to OUCH to take damage and return from the
                            \ subroutine using a tail call
    
    
    
    
           Name: SPS3                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Copy a space coordinate from the K% block into K3
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sps3.md)
     References: This subroutine is called as follows:
                 * [SPS1](../main/subroutine/sps1.md) calls SPS3
    
    
    
    
    
    * * *
    
    
     Copy one of the planet's coordinates into the corresponding location in the
     temporary variable K3. The high byte and absolute value of the sign byte are
     copied into the first two K3 bytes, and the sign of the sign byte is copied
     into the highest K3 byte.
    
     The comments below are written for copying the planet's x-coordinate into
     K3(2 1 0).
    
    
    
    * * *
    
    
     Arguments:
    
       X                    Determines which coordinate to copy, and to where:
    
                              * X = 0 copies (x_sign, x_hi) into K3(2 1 0)
    
                              * X = 3 copies (y_sign, y_hi) into K3(5 4 3)
    
                              * X = 6 copies (z_sign, z_hi) into K3(8 7 6)
    
    
    
    
    .SPS3
    
     LDA K%+1,X             \ Copy x_hi into K3+X
     STA K3,X
    
     LDA K%+2,X             \ Set A = Y = x_sign
     TAY
    
     AND #%01111111         \ Set K3+1 = |x_sign|
     STA K3+1,X
    
     TYA                    \ Set K3+2 = the sign of x_sign
     AND #%10000000
     STA K3+2,X
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: NWSPS                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Add a new space station to our local bubble of universe
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nwsps.md)
     Variations: See [code variations](../related/compare/main/subroutine/nwsps.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md) calls NWSPS
                 * [TT110](../main/subroutine/tt110.md) calls NWSPS
    
    
    
    
    
    
    .NWSPS
    
     JSR SPBLB              \ Light up the space station bulb on the dashboard
    
     LDX #%10000001         \ Set the AI flag in byte #32 to %10000001 (AI enabled,
     STX INWK+32            \ has an E.C.M.)
    
     LDX #0                 \ Set pitch counter to 0 (no pitch, roll only)
     STX INWK+30
    
     STX NEWB               \ Set NEWB to %00000000, though this gets overridden by
                            \ the default flags from E% in NWSHP below
    
    \STX INWK+31            \ This instruction is commented out in the original
                            \ source. It would set the exploding state and missile
                            \ count to 0
    
     STX FRIN+1             \ Set the second slot in the FRIN table to 0, so when we
                            \ fall through into NWSHP below, the new station that
                            \ gets created will go into slot FRIN+1, as this will be
                            \ the first empty slot that the routine finds
    
     DEX                    \ Set the roll counter to 255 (maximum anti-clockwise
     STX INWK+29            \ roll with no damping)
    
     LDX #10                \ Call NwS1 to flip the sign of nosev_x_hi (byte #10)
     JSR NwS1
    
     JSR NwS1               \ And again to flip the sign of nosev_y_hi (byte #12)
    
     JSR NwS1               \ And again to flip the sign of nosev_z_hi (byte #14)
    
     LDA spasto             \ Copy the address of the Coriolis space station's ship
     STA XX21+2*SST-2       \ blueprint from spasto to the #SST entry in the
     LDA spasto+1           \ blueprint lookup table at XX21, so when we spawn a
     STA XX21+2*SST-1       \ ship of type #SST, it will be a Coriolis station
    
     LDA tek                \ If the system's tech level in tek is less than 10,
     CMP #10                \ jump to notadodo, so tech levels 0 to 9 have Coriolis
     BCC notadodo           \ stations, while 10 and above will have Dodo stations
    
     LDA XX21+2*DOD-2       \ Copy the address of the Dodo space station's ship
     STA XX21+2*SST-2       \ blueprint from spasto to the #SST entry in the
     LDA XX21+2*DOD-1       \ blueprint lookup table at XX21, so when we spawn a
     STA XX21+2*SST-1       \ ship of type #SST, it will be a Dodo station
    
    .notadodo
    
     LDA #LO(LSO)           \ Set bytes #33 and #34 to point to LSO for the ship
     STA INWK+33            \ line heap for the space station
     LDA #HI(LSO)
     STA INWK+34
    
     LDA #SST               \ Set A to the space station type, and fall through
                            \ into NWSHP to finish adding the space station to the
                            \ universe
    
    
    
    
           Name: NWSHP                                                   [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Add a new ship to our local bubble of universe
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nwshp.md)
     Variations: See [code variations](../related/compare/main/subroutine/nwshp.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [BRIEF](../main/subroutine/brief.md) calls NWSHP
                 * [FRS1](../main/subroutine/frs1.md) calls NWSHP
                 * [GTHG](../main/subroutine/gthg.md) calls NWSHP
                 * [KS4](../main/subroutine/ks4.md) calls NWSHP
                 * [Main game loop (Part 1 of 6)](../main/subroutine/main_game_loop_part_1_of_6.md) calls NWSHP
                 * [Main game loop (Part 2 of 6)](../main/subroutine/main_game_loop_part_2_of_6.md) calls NWSHP
                 * [Main game loop (Part 3 of 6)](../main/subroutine/main_game_loop_part_3_of_6.md) calls NWSHP
                 * [Main game loop (Part 4 of 6)](../main/subroutine/main_game_loop_part_4_of_6.md) calls NWSHP
                 * [SFS1](../main/subroutine/sfs1.md) calls NWSHP
                 * [SOLAR](../main/subroutine/solar.md) calls NWSHP
                 * [SOS1](../main/subroutine/sos1.md) calls NWSHP
                 * [TITLE](../main/subroutine/title.md) calls NWSHP
    
    
    
    
    
    * * *
    
    
     This creates a new block of ship data in the K% workspace, allocates a new
     block in the ship line heap at WP, adds the new ship's type into the first
     empty slot in FRIN, and adds a pointer to the ship data into UNIV. If there
     isn't enough free memory for the new ship, it isn't added.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The type of the ship to add (see variable XX21 for a
                            list of ship types)
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the ship was successfully added, clear if it
                            wasn't (as there wasn't enough free memory)
    
       INF                  Points to the new ship's data block in K%
    
    
    
    
    .NWSHP
    
     STA T                  \ Store the ship type in location T
    
     LDX #0                 \ Before we can add a new ship, we need to check
                            \ whether we have an empty slot we can put it in. To do
                            \ this, we need to loop through all the slots to look
                            \ for an empty one, so set a counter in X that starts
                            \ from the first slot at 0. When ships are killed, then
                            \ the slots are shuffled down by the KILLSHP routine, so
                            \ the first empty slot will always come after the last
                            \ filled slot. This allows us to tack the new ship's
                            \ data block and ship line heap onto the end of the
                            \ existing ship data and heap, as shown in the memory
                            \ map below
    
    .NWL1
    
     LDA FRIN,X             \ Load the ship type for the X-th slot
    
     BEQ NW1                \ If it is zero, then this slot is empty and we can use
                            \ it for our new ship, so jump down to NW1
    
     INX                    \ Otherwise increment X to point to the next slot
    
     CPX #NOSH              \ If we haven't reached the last slot yet, loop back up
     BCC NWL1               \ to NWL1 to check the next slot (note that this means
                            \ only slots from 0 to #NOSH - 1 are populated by this
                            \ routine, but there is one more slot reserved in FRIN,
                            \ which is used to identify the end of the slot list
                            \ when shuffling the slots down in the KILLSHP routine)
    
    .NW3
    
     CLC                    \ Otherwise we don't have an empty slot, so we can't
     RTS                    \ add a new ship, so clear the C flag to indicate that
                            \ we have not managed to create the new ship, and return
                            \ from the subroutine
    
    .NW1
    
                            \ If we get here, then we have found an empty slot at
                            \ index X, so we can go ahead and create our new ship.
                            \ We do that by creating a ship data block at INWK and,
                            \ when we are done, copying the block from INWK into
                            \ the K% workspace (specifically, to INF)
    
     JSR GINF               \ Get the address of the data block for ship slot X
                            \ (which is in workspace K%) and store it in INF
    
     LDA T                  \ If the type of ship that we want to create is
     BMI NW2                \ negative, then this indicates a planet or sun, so
                            \ jump down to NW2, as the next section sets up a ship
                            \ data block, which doesn't apply to planets and suns,
                            \ as they don't have things like shields, missiles,
                            \ vertices and edges
    
                            \ This is a ship, so first we need to set up various
                            \ pointers to the ship blueprint we will need. The
                            \ blueprints for each ship type in Elite are stored
                            \ in a table at location XX21, so refer to the comments
                            \ on that variable for more details on the data we're
                            \ about to access
    
     ASL A                  \ Set Y = ship type * 2
     TAY
    
     LDA XX21-1,Y           \ The ship blueprints at XX21 start with a lookup
                            \ table that points to the individual ship blueprints,
                            \ so this fetches the high byte of this particular ship
                            \ type's blueprint
    
     BEQ NW3                \ If the high byte is 0 then this is not a valid ship
                            \ type, so jump to NW3 to clear the C flag and return
                            \ from the subroutine
    
     STA XX0+1              \ This is a valid ship type, so store the high byte in
                            \ XX0+1
    
     LDA XX21-2,Y           \ Fetch the low byte of this particular ship type's
     STA XX0                \ blueprint and store it in XX0, so XX0(1 0) now
                            \ contains the address of this ship's blueprint
    
     CPY #2*SST             \ If the ship type is a space station (SST), then jump
     BEQ NW6                \ to NW6, skipping the heap space steps below, as the
                            \ space station has its own line heap at LSO (which it
                            \ shares with the sun)
    
                            \ We now want to allocate space for a heap that we can
                            \ use to store the lines we draw for our new ship (so it
                            \ can easily be erased from the screen again). SLSP
                            \ points to the start of the current heap space, and we
                            \ can extend it downwards with the heap for our new ship
                            \ (as the heap space always ends just before the WP
                            \ workspace)
    
     LDY #5                 \ Fetch ship blueprint byte #5, which contains the
     LDA (XX0),Y            \ maximum heap size required for plotting the new ship,
     STA T1                 \ and store it in T1
    
     LDA SLSP               \ Take the 16-bit address in SLSP and subtract T1,
     SEC                    \ storing the 16-bit result in INWK(34 33), so this now
     SBC T1                 \ points to the start of the line heap for our new ship
     STA INWK+33
     LDA SLSP+1
     SBC #0
     STA INWK+34
    
                            \ We now need to check that there is enough free space
                            \ for both this new line heap and the new data block
                            \ for our ship. In memory, this is the layout of the
                            \ ship data blocks and ship line heaps:
                            \
                            \   +-----------------------------------+   &1034
                            \   |                                   |
                            \   | WP workspace                      |
                            \   |                                   |
                            \   +-----------------------------------+   &0800 = WP
                            \   |                                   |
                            \   | Current ship line heap            |
                            \   |                                   |
                            \   +-----------------------------------+   SLSP
                            \   |                                   |
                            \   | Proposed heap for new ship        |
                            \   |                                   |
                            \   +-----------------------------------+   INWK(34 33)
                            \   |                                   |
                            \   .                                   .
                            \   .                                   .
                            \   .                                   .
                            \   .                                   .
                            \   .                                   .
                            \   |                                   |
                            \   +-----------------------------------+   INF + NI%
                            \   |                                   |
                            \   | Proposed data block for new ship  |
                            \   |                                   |
                            \   +-----------------------------------+   INF
                            \   |                                   |
                            \   | Existing ship data blocks         |
                            \   |                                   |
                            \   +-----------------------------------+   &0400 = K%
                            \
                            \ So, to work out if we have enough space, we have to
                            \ make sure there is room between the end of our new
                            \ ship data block at INF + NI%, and the start of the
                            \ proposed heap for our new ship at the address we
                            \ stored in INWK(34 33). Or, to put it another way, we
                            \ and to make sure that:
                            \
                            \   INWK(34 33) > INF + NI%
                            \
                            \ which is the same as saying:
                            \
                            \   INWK+33 - INF > NI%
                            \
                            \ because INWK is in zero page, so INWK+34 = 0
    
     LDA INWK+33            \ Calculate INWK+33 - INF, again using 16-bit
    \SEC                    \ arithmetic, and put the result in (A Y), so the high
     SBC INF                \ byte is in A and the low byte in Y. The SEC
     TAY                    \ instruction is commented out in the original source;
     LDA INWK+34            \ as the previous subtraction will never underflow, it
     SBC INF+1              \ is superfluous
    
     BCC NW3+1              \ If we have an underflow from the subtraction, then
                            \ INF > INWK+33 and we definitely don't have enough
                            \ room for this ship, so jump to NW3+1, which returns
                            \ from the subroutine (with the C flag already cleared)
    
     BNE NW4                \ If the subtraction of the high bytes in A is not
                            \ zero, and we don't have underflow, then we definitely
                            \ have enough space, so jump to NW4 to continue setting
                            \ up the new ship
    
     CPY #NI%               \ Otherwise the high bytes are the same in our
     BCC NW3+1              \ subtraction, so now we compare the low byte of the
                            \ result (which is in Y) with NI%. This is the same as
                            \ doing INWK+33 - INF > NI% (see above). If this isn't
                            \ true, the C flag will be clear and we don't have
                            \ enough space, so we jump to NW3+1, which returns
                            \ from the subroutine (with the C flag already cleared)
    
    .NW4
    
     LDA INWK+33            \ If we get here then we do have enough space for our
     STA SLSP               \ new ship, so store the new bottom of the ship line
     LDA INWK+34            \ heap (i.e. INWK+33) in SLSP, doing both the high and
     STA SLSP+1             \ low bytes
    
    .NW6
    
     LDY #14                \ Fetch ship blueprint byte #14, which contains the
     LDA (XX0),Y            \ ship's energy, and store it in byte #35
     STA INWK+35
    
     LDY #19                \ Fetch ship blueprint byte #19, which contains the
     LDA (XX0),Y            \ number of missiles and laser power, and AND with %111
     AND #%00000111         \ to extract the number of missiles before storing in
     STA INWK+31            \ byte #31
    
     LDA T                  \ Restore the ship type we stored above
    
    .NW2
    
     STA FRIN,X             \ Store the ship type in the X-th byte of FRIN, so the
                            \ slot is now shown as occupied in the index table
    
     TAX                    \ Copy the ship type into X
    
     BMI NW8                \ If the ship type is negative (planet or sun), then
                            \ jump to NW8 to skip the following instructions
    
     CPX #HER               \ If the ship type is a rock hermit, jump to gangbang
     BEQ gangbang           \ to increase the junk count
    
     CPX #JL                \ If JL <= X < JH, i.e. the type of ship we added in X
     BCC NW7                \ is junk (escape pod, alloy plate, cargo canister,
     CPX #JH                \ asteroid, splinter, Shuttle or Transporter), then keep
     BCS NW7                \ going, otherwise jump to NW7
    
    .gangbang
    
     INC JUNK               \ We're adding junk, so increase the junk counter
    
    .NW7
    
     INC MANY,X             \ Increment the total number of ships of type X
    
    .NW8
    
     LDY T                  \ Restore the ship type we stored above
    
     LDA E%-1,Y             \ Fetch the E% byte for this ship to get the default
                            \ settings for the ship's NEWB flags
    
     AND #%01101111         \ Zero bits 4 and 7 (so the new ship is not docking, has
                            \ not been scooped, and has not just docked)
    
     ORA NEWB               \ Apply the result to the ship's NEWB flags, which sets
     STA NEWB               \ bits 0-3 and 5-6 in NEWB if they are set in the E%
                            \ byte
    
     LDY #NI%-1             \ The final step is to copy the new ship's data block
                            \ from INWK to INF, so set up a counter for NI% bytes
                            \ in Y
    
    .NWL3
    
     LDA INWK,Y             \ Load the Y-th byte of INWK and store in the Y-th byte
     STA (INF),Y            \ of the workspace pointed to by INF
    
     DEY                    \ Decrement the loop counter
    
     BPL NWL3               \ Loop back for the next byte until we have copied them
                            \ all over
    
     SEC                    \ We have successfully created our new ship, so set the
                            \ C flag to indicate success
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: NwS1                                                    [Show more]
           Type: Subroutine
       Category: Universe
        Summary: Flip the sign and double an INWK byte
    
    
        Context: See this subroutine [on its own page](../main/subroutine/nws1.md)
     References: This subroutine is called as follows:
                 * [NWSPS](../main/subroutine/nwsps.md) calls NwS1
    
    
    
    
    
    * * *
    
    
     Flip the sign of the INWK byte at offset X, and increment X by 2. This is
     used by the space station creation routine at NWSPS.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The offset of the INWK byte to be flipped
    
    
    
    * * *
    
    
     Returns:
    
       X                    X is incremented by 2
    
    
    
    
    .NwS1
    
     LDA INWK,X             \ Load the X-th byte of INWK into A and flip bit 7,
     EOR #%10000000         \ storing the result back in the X-th byte of INWK
     STA INWK,X
    
     INX                    \ Add 2 to X
     INX
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: ABORT                                                   [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Disarm missiles and update the dashboard indicators
    
    
        Context: See this subroutine [on its own page](../main/subroutine/abort.md)
     Variations: See [code variations](../related/compare/main/subroutine/abort.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [FRMIS](../main/subroutine/frmis.md) calls ABORT
                 * [KILLSHP](../main/subroutine/killshp.md) calls ABORT
                 * [Main flight loop (Part 3 of 16)](../main/subroutine/main_flight_loop_part_3_of_16.md) calls ABORT
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       Y                    The new colour of the missile indicator:
    
                              * &00 = black (no missile)
    
                              * #RED2 = red (armed and locked)
    
                              * #YELLOW2 = yellow/white (armed)
    
                              * #GREEN2 = green (disarmed)
    
    
    
    
    .ABORT
    
     LDX #&FF               \ Set X to &FF, which is the value of MSTG when we have
                            \ no target lock for our missile
    
                            \ Fall through into ABORT2 to set the missile lock to
                            \ the value in X, which effectively disarms the missile
    
    
    
    
           Name: ABORT2                                                  [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Set/unset the lock target for a missile and update the dashboard
    
    
        Context: See this subroutine [on its own page](../main/subroutine/abort2.md)
     Variations: See [code variations](../related/compare/main/subroutine/abort2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 11 of 16)](../main/subroutine/main_flight_loop_part_11_of_16.md) calls ABORT2
    
    
    
    
    
    * * *
    
    
     Set the lock target for the leftmost missile and update the dashboard.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    The slot number of the ship to lock our missile onto, or
                            &FF to remove missile lock
    
       Y                    The new colour of the missile indicator:
    
                              * &00 = black (no missile)
    
                              * #RED2 = red (armed and locked)
    
                              * #YELLOW2 = yellow/white (armed)
    
                              * #GREEN2 = green (disarmed)
    
    
    
    
    .ABORT2
    
     STX MSTG               \ Store the target of our missile lock in MSTG
    
     LDX NOMSL              \ Call MSBAR to update the leftmost indicator in the
     JSR MSBAR              \ dashboard's missile bar, which returns with Y = 0
    
     STY MSAR               \ Set MSAR = 0 to indicate that the leftmost missile
                            \ is no longer seeking a target lock
    
     RTS                    \ Return from the subroutine
    
    .msbpars
    
     EQUB 4, 0, 0, 0, 0     \ These bytes appear to be unused (they are left over
                            \ from the 6502 Second Processor version of Elite)
    
    
    
    
           Name: PROJ                                                    [Show more]
           Type: Subroutine
       Category: Maths (Geometry)
        Summary: Project the current ship or planet onto the screen
      Deep dive: [Extended screen coordinates](https://elite.bbcelite.com/deep_dives/extended_screen_coordinates.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/proj.md)
     Variations: See [code variations](../related/compare/main/subroutine/proj.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLANET](../main/subroutine/planet.md) calls PROJ
                 * [SHPPT](../main/subroutine/shppt.md) calls PROJ
    
    
    
    
    
    * * *
    
    
     Project the current ship's location or the planet onto the screen, either
     returning the screen coordinates of the projection (if it's on-screen), or
     returning an error via the C flag.
    
     In this context, "on-screen" means that the point is projected into the
     following range:
    
       centre of screen - 1024 < x < centre of screen + 1024
       centre of screen - 1024 < y < centre of screen + 1024
    
     This is to cater for ships (and, more likely, planets and suns) whose centres
     are off-screen but whose edges may still be visible.
    
     The projection calculation is:
    
       K3(1 0) = #X + x / z
       K4(1 0) = #Y + y / z
    
     where #X and #Y are the pixel x-coordinate and y-coordinate of the centre of
     the screen.
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The ship data block for the ship to project on-screen
    
    
    
    * * *
    
    
     Returns:
    
       K3(1 0)              The x-coordinate of the ship's projection on-screen
    
       K4(1 0)              The y-coordinate of the ship's projection on-screen
    
       C flag               Set if the ship's projection doesn't fit on the screen,
                            clear if it does project onto the screen
    
       A                    Contains K4+1, the high byte of the y-coordinate
    
    
    
    
    .PROJ
    
     LDA INWK               \ Set P(1 0) = (x_hi x_lo)
     STA P                  \            = x
     LDA INWK+1
     STA P+1
    
     LDA INWK+2             \ Set A = x_sign
    
     JSR PLS6               \ Call PLS6 to calculate:
                            \
                            \   (X K) = (A P+1 P) / (z_sign z_hi z_lo)
                            \         = (x_sign x_hi x_lo) / (z_sign z_hi z_lo)
                            \         = x / z
    
     BCS PL2-1              \ If the C flag is set then the result overflowed and
                            \ the coordinate doesn't fit on the screen, so return
                            \ from the subroutine with the C flag set (as PL2-1
                            \ contains an RTS)
    
     LDA K                  \ Set K3(1 0) = (X K) + #X
     ADC #X                 \             = #X + x / z
     STA K3                 \
                            \ first doing the low bytes
    
     TXA                    \ And then the high bytes. #X is the x-coordinate of
     ADC #0                 \ the centre of the space view, so this converts the
     STA K3+1               \ space x-coordinate into a screen x-coordinate
    
     LDA INWK+3             \ Set P(1 0) = (y_hi y_lo)
     STA P
     LDA INWK+4
     STA P+1
    
     LDA INWK+5             \ Set A = -y_sign
     EOR #%10000000
    
     JSR PLS6               \ Call PLS6 to calculate:
                            \
                            \   (X K) = (A P+1 P) / (z_sign z_hi z_lo)
                            \         = -(y_sign y_hi y_lo) / (z_sign z_hi z_lo)
                            \         = -y / z
    
     BCS PL2-1              \ If the C flag is set then the result overflowed and
                            \ the coordinate doesn't fit on the screen, so return
                            \ from the subroutine with the C flag set (as PL2-1
                            \ contains an RTS)
    
     LDA K                  \ Set K4(1 0) = (X K) + #Y
     ADC #Y                 \             = #Y - y / z
     STA K4                 \
                            \ first doing the low bytes
    
     TXA                    \ And then the high bytes. #Y is the y-coordinate of
     ADC #0                 \ the centre of the space view, so this converts the
     STA K4+1               \ space x-coordinate into a screen y-coordinate
    
     CLC                    \ Clear the C flag to indicate success
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PL2                                                     [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Remove the planet or sun from the screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pl2.md)
     Variations: See [code variations](../related/compare/main/subroutine/pl2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLANET](../main/subroutine/planet.md) calls PL2
                 * [PROJ](../main/subroutine/proj.md) calls via PL2-1
    
    
    
    
    
    * * *
    
    
     Other entry points:
    
       PL2-1                Contains an RTS
    
    
    
    
    .PL2
    
     LDA TYPE               \ Shift bit 0 of the planet/sun's type into the C flag
     LSR A
    
     BCS P%+5               \ If the planet/sun's type has bit 0 clear, then it's
                            \ either 128 or 130, which is a planet; meanwhile, the
                            \ sun has type 129, which has bit 0 set. So if this is
                            \ the sun, skip the following instruction
    
     JMP WPLS2              \ This is the planet, so jump to WPLS2 to remove it from
                            \ screen, returning from the subroutine using a tail
                            \ call
    
     JMP WPLS               \ This is the sun, so jump to WPLS to remove it from
                            \ screen, returning from the subroutine using a tail
                            \ call
    
    
    
    
           Name: PLANET                                                  [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw the planet or sun
    
    
        Context: See this subroutine [on its own page](../main/subroutine/planet.md)
     Variations: See [code variations](../related/compare/main/subroutine/planet.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [LL9 (Part 1 of 12)](../main/subroutine/ll9_part_1_of_12.md) calls PLANET
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The planet or sun's ship data block
    
    
    
    
    .PLANET
    
     LDA #GREEN             \ Switch to stripe 3-1-3-1, which is cyan/yellow in the
     STA COL                \ space view
    
     LDA INWK+8             \ Set A = z_sign (the highest byte in the planet/sun's
                            \ coordinates)
    
    \BMI PL2                \ This instruction is commented out in the original
                            \ source. It would remove the planet from the screen
                            \ when it's behind us
    
     CMP #48                \ If A >= 48 then the planet/sun is too far away to be
     BCS PL2                \ seen, so jump to PL2 to remove it from the screen,
                            \ returning from the subroutine using a tail call
    
     ORA INWK+7             \ Set A to 0 if both z_sign and z_hi are 0
    
     BEQ PL2                \ If both z_sign and z_hi are 0, then the planet/sun is
                            \ too close to be shown, so jump to PL2 to remove it
                            \ from the screen, returning from the subroutine using a
                            \ tail call
    
     JSR PROJ               \ Project the planet/sun onto the screen, returning the
                            \ centre's coordinates in K3(1 0) and K4(1 0)
    
     BCS PL2                \ If the C flag is set by PROJ then the planet/sun is
                            \ not visible on-screen, so jump to PL2 to remove it
                            \ from the screen, returning from the subroutine using
                            \ a tail call
    
     LDA #96                \ Set (A P+1 P) = (0 96 0) = 24576
     STA P+1                \
     LDA #0                 \ This represents the planet/sun's radius at a distance
     STA P                  \ of z = 1
    
     JSR DVID3B2            \ Call DVID3B2 to calculate:
                            \
                            \   K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)
                            \              = (0 96 0) / z
                            \              = 24576 / z
                            \
                            \ so K now contains the planet/sun's radius, reduced by
                            \ the actual distance to the planet/sun. We know that
                            \ K+3 and K+2 will be 0, as the number we are dividing,
                            \ (0 96 0), fits into the two bottom bytes, so the
                            \ result is actually in K(1 0)
    
     LDA K+1                \ If the high byte of the reduced radius is zero, jump
     BEQ PL82               \ to PL82, as K contains the radius on its own
    
     LDA #248               \ Otherwise set K = 248, to round up the radius in
     STA K                  \ K(1 0) to the nearest integer (if we consider the low
                            \ byte to be the fractional part)
    
    .PL82
    
     LDA TYPE               \ If the planet/sun's type has bit 0 clear, then it's
     LSR A                  \ either 128 or 130, which is a planet (the sun has type
     BCC PL9                \ 129, which has bit 0 set). So jump to PL9 to draw the
                            \ planet with radius K, returning from the subroutine
                            \ using a tail call
    
     JMP SUN                \ Otherwise jump to SUN to draw the sun with radius K,
                            \ returning from the subroutine using a tail call
    
    
    
    
           Name: PL9 (Part 1 of 3)                                       [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw the planet, with either an equator and meridian, or a crater
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pl9_part_1_of_3.md)
     Variations: See [code variations](../related/compare/main/subroutine/pl9_part_1_of_3.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLANET](../main/subroutine/planet.md) calls PL9
    
    
    
    
    
    * * *
    
    
     Draw the planet with radius K at pixel coordinate (K3, K4), and with either an
     equator and meridian, or a crater.
    
    
    
    * * *
    
    
     Arguments:
    
       K(1 0)               The planet's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the planet
    
       K4(1 0)              Pixel y-coordinate of the centre of the planet
    
       INWK                 The planet's ship data block
    
    
    
    
    .PL9
    
     JSR WPLS2              \ Call WPLS2 to remove the planet from the screen
    
     JSR CIRCLE             \ Call CIRCLE to draw the planet's new circle
    
     BCS PL20               \ If the call to CIRCLE returned with the C flag set,
                            \ then the circle does not fit on-screen, so jump to
                            \ PL20 to return from the subroutine
    
     LDA K+1                \ If K+1 is zero, jump to PL25 as K(1 0) < 256, so the
     BEQ PL25               \ planet fits on the screen and we can draw meridians or
                            \ craters
    
    .PL20
    
     RTS                    \ The planet doesn't fit on-screen, so return from the
                            \ subroutine
    
    .PL25
    
     LDA TYPE               \ If the planet type is 128 then it has an equator and
     CMP #128               \ a meridian, so this jumps to PL26 if this is not a
     BNE PL26               \ planet with an equator - in other words, if it is a
                            \ planet with a crater
    
                            \ Otherwise this is a planet with an equator and
                            \ meridian, so fall through into the following to draw
                            \ them
    
    
    
    
           Name: PL9 (Part 2 of 3)                                       [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw the planet's equator and meridian
      Deep dive: [Drawing meridians and equators](https://elite.bbcelite.com/deep_dives/drawing_meridians_and_equators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pl9_part_2_of_3.md)
     Variations: See [code variations](../related/compare/main/subroutine/pl9_part_2_of_3.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Draw the planet's equator and meridian.
    
    
    
    * * *
    
    
     Arguments:
    
       K(1 0)               The planet's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the planet
    
       K4(1 0)              Pixel y-coordinate of the centre of the planet
    
       INWK                 The planet's ship data block
    
    
    
    
     LDA K                  \ If the planet's radius is less than 6, the planet is
     CMP #6                 \ too small to show a meridian, so jump to PL20 to
     BCC PL20               \ return from the subroutine
    
     LDA INWK+14            \ Set P = -nosev_z_hi
     EOR #%10000000
     STA P
    
     LDA INWK+20            \ Set A = roofv_z_hi
    
     JSR PLS4               \ Call PLS4 to calculate the following:
                            \
                            \   CNT2 = arctan(P / A) / 4
                            \        = arctan(-nosev_z_hi / roofv_z_hi) / 4
                            \
                            \ and do the following if nosev_z_hi >= 0:
                            \
                            \   CNT2 = CNT2 + PI
    
     LDX #9                 \ Set X to 9 so the call to PLS1 divides nosev_x
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA K2                 \
     STY XX16               \   (XX16 K2) = nosev_x / z
                            \
                            \ and increment X to point to nosev_y for the next call
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA K2+1               \
     STY XX16+1             \   (XX16+1 K2+1) = nosev_y / z
    
     LDX #15                \ Set X to 15 so the call to PLS5 divides roofv_x
    
     JSR PLS5               \ Call PLS5 to calculate the following:
                            \
                            \   (XX16+2 K2+2) = roofv_x / z
                            \
                            \   (XX16+3 K2+3) = roofv_y / z
    
     JSR PLS2               \ Call PLS2 to draw the first meridian
    
     LDA INWK+14            \ Set P = -nosev_z_hi
     EOR #%10000000
     STA P
    
     LDA INWK+26            \ Set A = sidev_z_hi, so the second meridian will be at
                            \ 90 degrees to the first
    
     JSR PLS4               \ Call PLS4 to calculate the following:
                            \
                            \   CNT2 = arctan(P / A) / 4
                            \        = arctan(-nosev_z_hi / sidev_z_hi) / 4
                            \
                            \ and do the following if nosev_z_hi >= 0:
                            \
                            \   CNT2 = CNT2 + PI
    
     LDX #21                \ Set X to 21 so the call to PLS5 divides sidev_x
    
     JSR PLS5               \ Call PLS5 to calculate the following:
                            \
                            \   (XX16+2 K2+2) = sidev_x / z
                            \
                            \   (XX16+3 K2+3) = sidev_y / z
    
     JMP PLS2               \ Jump to PLS2 to draw the second meridian, returning
                            \ from the subroutine using a tail call
    
    
    
    
           Name: PL9 (Part 3 of 3)                                       [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw the planet's crater
      Deep dive: [Drawing craters](https://elite.bbcelite.com/deep_dives/drawing_craters.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pl9_part_3_of_3.md)
     Variations: See [code variations](../related/compare/main/subroutine/pl9_part_3_of_3.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     Draw the planet's crater.
    
    
    
    * * *
    
    
     Arguments:
    
       K(1 0)               The planet's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the planet
    
       K4(1 0)              Pixel y-coordinate of the centre of the planet
    
       INWK                 The planet's ship data block
    
    
    
    
    .PL26
    
     LDA INWK+20            \ Set A = roofv_z_hi
    
     BMI PL20               \ If A is negative, the crater is on the far side of the
                            \ planet, so return from the subroutine (as PL2
                            \ contains an RTS)
    
     LDX #15                \ Set X = 15, so the following call to PLS3 operates on
                            \ roofv
    
     JSR PLS3               \ Call PLS3 to calculate:
                            \
                            \   (Y A P) = 222 * roofv_x / z
                            \
                            \ to give the x-coordinate of the crater offset and
                            \ increment X to point to roofv_y for the next call
    
     CLC                    \ Calculate:
     ADC K3                 \
     STA K3                 \   K3(1 0) = (Y A) + K3(1 0)
                            \           = 222 * roofv_x / z + x-coordinate of planet
                            \             centre
                            \
                            \ starting with the high bytes
    
     TYA                    \ And then doing the low bytes, so now K3(1 0) contains
     ADC K3+1               \ the x-coordinate of the crater offset plus the planet
     STA K3+1               \ centre to give the x-coordinate of the crater's centre
    
     JSR PLS3               \ Call PLS3 to calculate:
                            \
                            \   (Y A P) = 222 * roofv_y / z
                            \
                            \ to give the y-coordinate of the crater offset
    
     STA P                  \ Calculate:
     LDA K4                 \
     SEC                    \   K4(1 0) = K4(1 0) - (Y A)
     SBC P                  \           = 222 * roofv_y / z - y-coordinate of planet
     STA K4                 \             centre
                            \
                            \ starting with the low bytes
    
     STY P                  \ And then doing the low bytes, so now K4(1 0) contains
     LDA K4+1               \ the y-coordinate of the crater offset plus the planet
     SBC P                  \ centre to give the y-coordinate of the crater's centre
     STA K4+1
    
     LDX #9                 \ Set X = 9, so the following call to PLS1 operates on
                            \ nosev
    
     JSR PLS1               \ Call PLS1 to calculate the following:
                            \
                            \   (Y A) = nosev_x / z
                            \
                            \ and increment X to point to nosev_y for the next call
    
     LSR A                  \ Set (XX16 K2) = (Y A) / 2
     STA K2
     STY XX16
    
     JSR PLS1               \ Call PLS1 to calculate the following:
                            \
                            \   (Y A) = nosev_y / z
                            \
                            \ and increment X to point to nosev_z for the next call
    
     LSR A                  \ Set (XX16+1 K2+1) = (Y A) / 2
     STA K2+1
     STY XX16+1
    
     LDX #21                \ Set X = 21, so the following call to PLS1 operates on
                            \ sidev
    
     JSR PLS1               \ Call PLS1 to calculate the following:
                            \
                            \   (Y A) = sidev_x / z
                            \
                            \ and increment X to point to sidev_y for the next call
    
     LSR A                  \ Set (XX16+2 K2+2) = (Y A) / 2
     STA K2+2
     STY XX16+2
    
     JSR PLS1               \ Call PLS1 to calculate the following:
                            \
                            \   (Y A) = sidev_y / z
                            \
                            \ and increment X to point to sidev_z for the next call
    
     LSR A                  \ Set (XX16+3 K2+3) = (Y A) / 2
     STA K2+3
     STY XX16+3
    
     LDA #64                \ Set TGT = 64, so we draw a full ellipse in the call to
     STA TGT                \ PLS22 below
    
     LDA #0                 \ Set CNT2 = 0 as we are drawing a full ellipse, so we
     STA CNT2               \ don't need to apply an offset
    
     JMP PLS22              \ Jump to PLS22 to draw the crater, returning from the
                            \ subroutine using a tail call
    
    
    
    
           Name: PLS1                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Calculate (Y A) = nosev_x / z
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pls1.md)
     References: This subroutine is called as follows:
                 * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md) calls PLS1
                 * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md) calls PLS1
                 * [PLS3](../main/subroutine/pls3.md) calls PLS1
                 * [PLS5](../main/subroutine/pls5.md) calls PLS1
    
    
    
    
    
    * * *
    
    
     Calculate the following division of a specified value from one of the
     orientation vectors (in this example, nosev_x):
    
       (Y A) = nosev_x / z
    
     where z is the z-coordinate of the planet from INWK. The result is an 8-bit
     magnitude in A, with maximum value 254, and just a sign bit (bit 7) in Y.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    Determines which of the INWK orientation vectors to
                            divide:
    
                              * X = 9, 11, 13: divides nosev_x, nosev_y, nosev_z
    
                              * X = 15, 17, 19: divides roofv_x, roofv_y, roofv_z
    
                              * X = 21, 23, 25: divides sidev_x, sidev_y, sidev_z
    
       INWK                 The planet's ship data block
    
    
    
    * * *
    
    
     Returns:
    
       A                    The result as an 8-bit magnitude with maximum value 254
    
       Y                    The sign of the result in bit 7
    
       K+3                  Also the sign of the result in bit 7
    
       X                    X gets incremented by 2 so it points to the next
                            coordinate in this orientation vector (so consecutive
                            calls to the routine will start with x, then move onto y
                            and then z)
    
    
    
    
    .PLS1
    
     LDA INWK,X             \ Set P = nosev_x_lo
     STA P
    
     LDA INWK+1,X           \ Set P+1 = |nosev_x_hi|
     AND #%01111111
     STA P+1
    
     LDA INWK+1,X           \ Set A = sign bit of nosev_x_lo
     AND #%10000000
    
     JSR DVID3B2            \ Call DVID3B2 to calculate:
                            \
                            \   K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)
    
     LDA K                  \ Fetch the lowest byte of the result into A
    
     LDY K+1                \ Fetch the second byte of the result into Y
    
     BEQ P%+4               \ If the second byte is 0, skip the next instruction
    
     LDA #254               \ The second byte is non-zero, so the result won't fit
                            \ into one byte, so set A = 254 as our maximum one-byte
                            \ value to return
    
     LDY K+3                \ Fetch the sign of the result from K+3 into Y
    
     INX                    \ Add 2 to X so the index points to the next coordinate
     INX                    \ in this orientation vector (so consecutive calls to
                            \ the routine will start with x, then move onto y and z)
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PLS2                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw a half-ellipse
      Deep dive: [Drawing ellipses](https://elite.bbcelite.com/deep_dives/drawing_ellipses.html)
                 [Drawing meridians and equators](https://elite.bbcelite.com/deep_dives/drawing_meridians_and_equators.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pls2.md)
     References: This subroutine is called as follows:
                 * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md) calls PLS2
    
    
    
    
    
    * * *
    
    
     Draw a half-ellipse, used for the planet's equator and meridian.
    
    
    
    
    .PLS2
    
     LDA #31                \ Set TGT = 31, so we only draw half an ellipse
     STA TGT
    
                            \ Fall through into PLS22 to draw the half-ellipse
    
    
    
    
           Name: PLS22                                                   [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Draw an ellipse or half-ellipse
      Deep dive: [Drawing ellipses](https://elite.bbcelite.com/deep_dives/drawing_ellipses.html)
                 [Drawing meridians and equators](https://elite.bbcelite.com/deep_dives/drawing_meridians_and_equators.html)
                 [Drawing craters](https://elite.bbcelite.com/deep_dives/drawing_craters.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pls22.md)
     References: This subroutine is called as follows:
                 * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md) calls PLS22
    
    
    
    
    
    * * *
    
    
     Draw an ellipse or half-ellipse, to be used for the planet's equator and
     meridian (in which case we draw half an ellipse), or crater (in which case we
     draw a full ellipse).
    
     The ellipse is defined by a centre point, plus two conjugate radius vectors,
     u and v, where:
    
       u = [ u_x ]       v = [ v_x ]
           [ u_y ]           [ v_y ]
    
     The individual components of these 2D vectors (i.e. u_x, u_y etc.) are 16-bit
     sign-magnitude numbers, where the high bytes contain only the sign bit (in
     bit 7), with bits 0 to 6 being clear. This means that as we store u_x as
     (XX16 K2), for example, we know that |u_x| = K2.
    
     This routine calls BLINE to draw each line segment in the ellipse, passing the
     coordinates as follows:
    
       K6(1 0) = K3(1 0) + u_x * cos(CNT2) + v_x * sin(CNT2)
    
       K6(3 2) = K4(1 0) - u_y * cos(CNT2) - v_y * sin(CNT2)
    
     The y-coordinates are negated because BLINE expects pixel coordinates but the
     u and v vectors are extracted from the orientation vector. The y-axis runs
     in the opposite direction in 3D space to that on the screen, so we need to
     negate the 3D space coordinates before we can combine them with the ellipse's
     centre coordinates.
    
    
    
    * * *
    
    
     Arguments:
    
       K(1 0)               The planet's radius
    
       K3(1 0)              The pixel x-coordinate of the centre of the ellipse
    
       K4(1 0)              The pixel y-coordinate of the centre of the ellipse
    
       (XX16 K2)            The x-component of u (i.e. u_x), where XX16 contains
                            just the sign of the sign-magnitude number
    
       (XX16+1 K2+1)        The y-component of u (i.e. u_y), where XX16+1 contains
                            just the sign of the sign-magnitude number
    
       (XX16+2 K2+2)        The x-component of v (i.e. v_x), where XX16+2 contains
                            just the sign of the sign-magnitude number
    
       (XX16+3 K2+3)        The y-component of v (i.e. v_y), where XX16+3 contains
                            just the sign of the sign-magnitude number
    
       TGT                  The number of segments to draw:
    
                              * 32 for a half ellipse (a meridian)
    
                              * 64 for a full ellipse (a crater)
    
       CNT2                 The starting segment for drawing the half-ellipse
    
    
    
    
    .PLS22
    
     LDX #0                 \ Set CNT = 0
     STX CNT
    
     DEX                    \ Set FLAG = &FF to start a new line in the ball line
     STX FLAG               \ heap when calling BLIN below, so the crater or
                            \ meridian is separate from any previous ellipses
    
    .PLL4
    
     LDA CNT2               \ Set X = CNT2 mod 32
     AND #31                \
     TAX                    \ So X is the starting segment, reduced to the range 0
                            \ to 32, so as there are 64 segments in the circle, this
                            \ reduces the starting angle to 0 to 180 degrees, so we
                            \ can use X as an index into the sine table (which only
                            \ contains values for segments 0 to 31)
                            \
                            \ Also, because CNT2 mod 32 is in the range 0 to 180
                            \ degrees, we know that sin(CNT2 mod 32) is always
                            \ positive, or to put it another way:
                            \
                            \   sin(CNT2 mod 32) = |sin(CNT2)|
    
     LDA SNE,X              \ Set Q = sin(X)
     STA Q                  \       = sin(CNT2 mod 32)
                            \       = |sin(CNT2)|
    
     LDA K2+2               \ Set A = K2+2
                            \       = |v_x|
    
     JSR FMLTU              \ Set R = A * Q / 256
     STA R                  \       = |v_x| * |sin(CNT2)|
    
     LDA K2+3               \ Set A = K2+3
                            \       = |v_y|
    
     JSR FMLTU              \ Set K = A * Q / 256
     STA K                  \       = |v_y| * |sin(CNT2)|
    
     LDX CNT2               \ If CNT2 >= 33 then this sets the C flag, otherwise
     CPX #33                \ it's clear, so this means that:
                            \
                            \   * C is clear if the segment starts in the first half
                            \     of the circle, 0 to 180 degrees
                            \
                            \   * C is set if the segment starts in the second half
                            \     of the circle, 180 to 360 degrees
                            \
                            \ In other words, the C flag contains the sign bit for
                            \ sin(CNT2), which is positive for 0 to 180 degrees
                            \ and negative for 180 to 360 degrees
    
     LDA #0                 \ Shift the C flag into the sign bit of XX16+5, so
     ROR A                  \ XX16+5 has the correct sign for sin(CNT2)
     STA XX16+5             \
                            \ Because we set the following above:
                            \
                            \   K = |v_y| * |sin(CNT2)|
                            \   R = |v_x| * |sin(CNT2)|
                            \
                            \ we can add XX16+5 as the high byte to give us the
                            \ following:
                            \
                            \   (XX16+5 K) = |v_y| * sin(CNT2)
                            \   (XX16+5 R) = |v_x| * sin(CNT2)
    
     LDA CNT2               \ Set X = (CNT2 + 16) mod 32
     CLC                    \
     ADC #16                \ So we can use X as a lookup index into the SNE table
     AND #31                \ to get the cosine (as there are 16 segments in a
     TAX                    \ quarter-circle)
                            \
                            \ Also, because the sine table only contains positive
                            \ values, we know that sin((CNT2 + 16) mod 32) will
                            \ always be positive, or to put it another way:
                            \
                            \   sin((CNT2 + 16) mod 32) = |cos(CNT2)|
    
     LDA SNE,X              \ Set Q = sin(X)
     STA Q                  \       = sin((CNT2 + 16) mod 32)
                            \       = |cos(CNT2)|
    
     LDA K2+1               \ Set A = K2+1
                            \       = |u_y|
    
     JSR FMLTU              \ Set K+2 = A * Q / 256
     STA K+2                \         = |u_y| * |cos(CNT2)|
    
     LDA K2                 \ Set A = K2
                            \       = |u_x|
    
     JSR FMLTU              \ Set P = A * Q / 256
     STA P                  \       = |u_x| * |cos(CNT2)|
                            \
                            \ The call to FMLTU also sets the C flag, so in the
                            \ following, ADC #15 adds 16 rather than 15
    
     LDA CNT2               \ If (CNT2 + 16) mod 64 >= 33 then this sets the C flag,
     ADC #15                \ otherwise it's clear, so this means that:
     AND #63                \
     CMP #33                \   * C is clear if the segment starts in the first or
                            \     last quarter of the circle, 0 to 90 degrees or 270
                            \     to 360 degrees
                            \
                            \   * C is set if the segment starts in the second or
                            \     third quarter of the circle, 90 to 270 degrees
                            \
                            \ In other words, the C flag contains the sign bit for
                            \ cos(CNT2), which is positive for 0 to 90 degrees or
                            \ 270 to 360 degrees, and negative for 90 to 270 degrees
    
     LDA #0                 \ Shift the C flag into the sign bit of XX16+4, so:
     ROR A                  \ XX16+4 has the correct sign for cos(CNT2)
     STA XX16+4             \
                            \ Because we set the following above:
                            \
                            \   K+2 = |u_y| * |cos(CNT2)|
                            \   P   = |u_x| * |cos(CNT2)|
                            \
                            \ we can add XX16+4 as the high byte to give us the
                            \ following:
                            \
                            \   (XX16+4 K+2) = |u_y| * cos(CNT2)
                            \   (XX16+4 P)   = |u_x| * cos(CNT2)
    
     LDA XX16+5             \ Set S = the sign of XX16+2 * XX16+5
     EOR XX16+2             \       = the sign of v_x * XX16+5
     STA S                  \
                            \ So because we set this above:
                            \
                            \   (XX16+5 R) = |v_x| * sin(CNT2)
                            \
                            \ we now have this:
                            \
                            \   (S R) = v_x * sin(CNT2)
    
     LDA XX16+4             \ Set A = the sign of XX16 * XX16+4
     EOR XX16               \       = the sign of u_x * XX16+4
                            \
                            \ So because we set this above:
                            \
                            \   (XX16+4 P)   = |u_x| * cos(CNT2)
                            \
                            \ we now have this:
                            \
                            \   (A P) = u_x * cos(CNT2)
    
     JSR ADD                \ Set (A X) = (A P) + (S R)
                            \           = u_x * cos(CNT2) + v_x * sin(CNT2)
    
     STA T                  \ Store the high byte in T, so the result is now:
                            \
                            \   (T X) = u_x * cos(CNT2) + v_x * sin(CNT2)
    
     BPL PL42               \ If the result is positive, jump down to PL42
    
     TXA                    \ The result is negative, so we need to negate the
     EOR #%11111111         \ magnitude using two's complement, first doing the low
     CLC                    \ byte in X
     ADC #1
     TAX
    
     LDA T                  \ And then the high byte in T, making sure to leave the
     EOR #%01111111         \ sign bit alone
     ADC #0
     STA T
    
    .PL42
    
     TXA                    \ Set K6(1 0) = K3(1 0) + (T X)
     ADC K3                 \
     STA K6                 \ starting with the low bytes
    
     LDA T                  \ And then doing the high bytes, so we now get:
     ADC K3+1               \
     STA K6+1               \   K6(1 0) = K3(1 0) + (T X)
                            \           = K3(1 0) + u_x * cos(CNT2)
                            \                     + v_x * sin(CNT2)
                            \
                            \ K3(1 0) is the x-coordinate of the centre of the
                            \ ellipse, so we now have the correct x-coordinate for
                            \ our ellipse segment that we can pass to BLINE below
    
     LDA K                  \ Set R = K = |v_y| * sin(CNT2)
     STA R
    
     LDA XX16+5             \ Set S = the sign of XX16+3 * XX16+5
     EOR XX16+3             \       = the sign of v_y * XX16+5
     STA S                  \
                            \ So because we set this above:
                            \
                            \   (XX16+5 K) = |v_y| * sin(CNT2)
                            \
                            \ and we just set R = K, we now have this:
                            \
                            \   (S R) = v_y * sin(CNT2)
    
     LDA K+2                \ Set P = K+2 = |u_y| * cos(CNT2)
     STA P
    
     LDA XX16+4             \ Set A = the sign of XX16+1 * XX16+4
     EOR XX16+1             \       = the sign of u_y * XX16+4
                            \
                            \ So because we set this above:
                            \
                            \   (XX16+4 K+2) = |u_y| * cos(CNT2)
                            \
                            \ and we just set P = K+2, we now have this:
                            \
                            \   (A P) = u_y * cos(CNT2)
    
     JSR ADD                \ Set (A X) = (A P) + (S R)
                            \           =  u_y * cos(CNT2) + v_y * sin(CNT2)
    
     EOR #%10000000         \ Store the negated high byte in T, so the result is
     STA T                  \ now:
                            \
                            \   (T X) = - u_y * cos(CNT2) - v_y * sin(CNT2)
                            \
                            \ This negation is necessary because BLINE expects us
                            \ to pass pixel coordinates, where y-coordinates get
                            \ larger as we go down the screen; u_y and v_y, on the
                            \ other hand, are extracted from the orientation
                            \ vectors, where y-coordinates get larger as we go up
                            \ in space, so to rectify this we need to negate the
                            \ result in (T X) before we can add it to the
                            \ y-coordinate of the ellipse's centre in BLINE
    
     BPL PL43               \ If the result is positive, jump down to PL43
    
     TXA                    \ The result is negative, so we need to negate the
     EOR #%11111111         \ magnitude using two's complement, first doing the low
     CLC                    \ byte in X
     ADC #1
     TAX
    
     LDA T                  \ And then the high byte in T, making sure to leave the
     EOR #%01111111         \ sign bit alone
     ADC #0
     STA T
    
    .PL43
    
                            \ We now call BLINE to draw the ellipse line segment
                            \
                            \ The first few instructions of BLINE do the following:
                            \
                            \   K6(3 2) = K4(1 0) + (T X)
                            \
                            \ which gives:
                            \
                            \   K6(3 2) = K4(1 0) - u_y * cos(CNT2)
                            \                     - v_y * sin(CNT2)
                            \
                            \ K4(1 0) is the pixel y-coordinate of the centre of the
                            \ ellipse, so this gives us the correct y-coordinate for
                            \ our ellipse segment (we already calculated the
                            \ x-coordinate in K3(1 0) above)
    
     JSR BLINE              \ Call BLINE to draw this segment, which also returns
                            \ the updated value of CNT in A
    
     CMP TGT                \ If CNT > TGT then jump to PL40 to stop drawing the
     BEQ P%+4               \ ellipse (which is how we draw half-ellipses)
     BCS PL40
    
     LDA CNT2               \ Set CNT2 = (CNT2 + STP) mod 64
     CLC
     ADC STP
     AND #63
     STA CNT2
    
     JMP PLL4               \ Jump back to PLL4 to draw the next segment
    
    .PL40
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: SUN (Part 1 of 4)                                       [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Draw the sun: Set up all the variables needed to draw the sun
      Deep dive: [Drawing the sun](https://elite.bbcelite.com/deep_dives/drawing_the_sun.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sun_part_1_of_4.md)
     Variations: See [code variations](../related/compare/main/subroutine/sun_part_1_of_4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PLANET](../main/subroutine/planet.md) calls SUN
                 * [TT23](../main/subroutine/tt23.md) calls SUN
    
    
    
    
    
    * * *
    
    
     Draw a new sun with radius K at pixel coordinate (K3, K4), removing the old
     sun if there is one. This routine is used to draw the sun, as well as the
     star systems on the Short-range Chart.
    
     The first part sets up all the variables needed to draw the new sun.
    
    
    
    * * *
    
    
     Arguments:
    
       K                    The new sun's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the new sun
    
       K4(1 0)              Pixel y-coordinate of the centre of the new sun
    
       SUNX(1 0)            The x-coordinate of the vertical centre axis of the old
                            sun (the one currently on-screen)
    
    
    
    
    .PLF3M3
    
     JMP WPLS               \ Jump to WPLS to remove the old sun from the screen. We
                            \ only get here via the BCS just after the SUN entry
                            \ point below, when there is no new sun to draw
    
    .PLF3
    
                            \ This is called from below to negate X and set A to
                            \ &FF, for when the new sun's centre is off the bottom
                            \ of the screen (so we don't need to draw its bottom
                            \ half)
                            \
                            \ This happens when the y-coordinate of the centre of
                            \ the sun is bigger than the y-coordinate of the bottom
                            \ of the space view
    
     TXA                    \ Negate X using two's complement, so X = ~X + 1
     EOR #%11111111
     CLC
     ADC #1
     TAX
    
    .PLF17
    
                            \ This is called from below to set A to &FF, for when
                            \ the new sun's centre is right on the bottom of the
                            \ screen (so we don't need to draw its bottom half)
    
     LDA #&FF               \ Set A = &FF
    
     BNE PLF5               \ Jump to PLF5 (this BNE is effectively a JMP as A is
                            \ never zero)
    
    .SUN
    
     LDA #RED               \ Switch to colour 2, which is red in the space view
     STA COL
    
     LDA #1                 \ Set LSX = 1 to indicate the sun line heap is about to
     STA LSX                \ be filled up
    
     JSR CHKON              \ Call CHKON to check whether any part of the new sun's
                            \ circle appears on-screen, and if it does, set P(2 1)
                            \ to the maximum y-coordinate of the new sun on-screen
    
     BCS PLF3M3             \ If CHKON set the C flag then the new sun's circle does
                            \ not appear on-screen, so jump to WPLS (via the JMP at
                            \ the top of this routine) to remove the sun from the
                            \ screen, returning from the subroutine using a tail
                            \ call
    
     LDA #0                 \ Set A = 0
    
     LDX K                  \ Set X = K = radius of the new sun
    
     CPX #96                \ If X >= 96, set the C flag and rotate it into bit 0
     ROL A                  \ of A, otherwise rotate a 0 into bit 0
    
     CPX #40                \ If X >= 40, set the C flag and rotate it into bit 0
     ROL A                  \ of A, otherwise rotate a 0 into bit 0
    
     CPX #16                \ If X >= 16, set the C flag and rotate it into bit 0
     ROL A                  \ of A, otherwise rotate a 0 into bit 0
    
                            \ By now, A contains the following:
                            \
                            \   * If radius is 96-255 then A = %111 = 7
                            \
                            \   * If radius is 40-95  then A = %11  = 3
                            \
                            \   * If radius is 16-39  then A = %1   = 1
                            \
                            \   * If radius is 0-15   then A = %0   = 0
                            \
                            \ The value of A determines the size of the new sun's
                            \ ragged fringes - the bigger the sun, the bigger the
                            \ fringes
    
    .PLF18
    
     STA CNT                \ Store the fringe size in CNT
    
                            \ We now calculate the highest pixel y-coordinate of the
                            \ new sun, given that P(2 1) contains the 16-bit maximum
                            \ y-coordinate of the new sun on-screen
    
     LDA Yx2M1              \ Set Y to the y-coordinate of the bottom of the space
                            \ view
    
     LDX P+2                \ If P+2 is non-zero, the maximum y-coordinate is off
     BNE PLF2               \ the bottom of the screen, so skip to PLF2 with A set
                            \ to the y-coordinate of the bottom of the space view
    
     CMP P+1                \ If A < P+1, the maximum y-coordinate is underneath the
     BCC PLF2               \ dashboard, so skip to PLF2 with A set to the
                            \ y-coordinate of the bottom of the space view
    
     LDA P+1                \ Set A = P+1, the low byte of the maximum y-coordinate
                            \ of the sun on-screen
    
     BNE PLF2               \ If A is non-zero, skip to PLF2 as it contains the
                            \ value we are after
    
     LDA #1                 \ Otherwise set A = 1, the top line of the screen
    
    .PLF2
    
     STA TGT                \ Set TGT to A, the maximum y-coordinate of the sun on
                            \ screen
    
                            \ We now calculate the number of lines we need to draw
                            \ and the direction in which we need to draw them, both
                            \ from the centre of the new sun
    
     LDA Yx2M1              \ Set (A X) = y-coordinate of bottom of screen - K4(1 0)
     SEC                    \
     SBC K4                 \ Starting with the low bytes
     TAX
    
     LDA #0                 \ And then doing the high bytes, so (A X) now contains
     SBC K4+1               \ the number of lines between the centre of the sun and
                            \ the bottom of the screen. If it is positive then the
                            \ centre of the sun is above the bottom of the screen,
                            \ if it is negative then the centre of the sun is below
                            \ the bottom of the screen
    
     BMI PLF3               \ If A < 0, then this means the new sun's centre is off
                            \ the bottom of the screen, so jump up to PLF3 to negate
                            \ the height in X (so it becomes positive), set A to &FF
                            \ and jump down to PLF5
    
     BNE PLF4               \ If A > 0, then the new sun's centre is at least a full
                            \ screen above the bottom of the space view, so jump
                            \ down to PLF4 to set X = radius and A = 0
    
     INX                    \ Set the flags depending on the value of X
     DEX
    
     BEQ PLF17              \ If X = 0 (we already know A = 0 by this point) then
                            \ jump up to PLF17 to set A to &FF before jumping down
                            \ to PLF5
    
     CPX K                  \ If X < the radius in K, jump down to PLF5, so if
     BCC PLF5               \ X >= the radius in K, we set X = radius and A = 0
    
    .PLF4
    
     LDX K                  \ Set X to the radius
    
     LDA #0                 \ Set A = 0
    
    .PLF5
    
     STX V                  \ Store the height in V
    
     STA V+1                \ Store the direction in V+1
    
     LDA K                  \ Set (A P) = K * K
     JSR SQUA2
    
     STA K2+1               \ Set K2(1 0) = (A P) = K * K
     LDA P
     STA K2
    
                            \ By the time we get here, the variables should be set
                            \ up as shown in the header for part 3 below
    
    
    
    
           Name: SUN (Part 2 of 4)                                       [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Draw the sun: Start from the bottom of the screen and erase the
                 old sun line by line
      Deep dive: [Drawing the sun](https://elite.bbcelite.com/deep_dives/drawing_the_sun.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sun_part_2_of_4.md)
     Variations: See [code variations](../related/compare/main/subroutine/sun_part_2_of_4.md) for this subroutine in the different versions
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part erases the old sun, starting at the bottom of the screen and working
     upwards until we reach the bottom of the new sun.
    
    
    
    
     LDY Yx2M1              \ Set Y = y-coordinate of the bottom of the screen,
                            \ which we use as a counter in the following routine to
                            \ redraw the old sun
    
     LDA SUNX               \ Set YY(1 0) = SUNX(1 0), the x-coordinate of the
     STA YY                 \ vertical centre axis of the old sun that's currently
     LDA SUNX+1             \ on-screen
     STA YY+1
    
    .PLFL2
    
     CPY TGT                \ If Y = TGT, we have reached the line where we will
     BEQ PLFL               \ start drawing the new sun, so there is no need to
                            \ keep erasing the old one, so jump down to PLFL
    
     LDA LSO,Y              \ Fetch the Y-th point from the sun line heap, which
                            \ gives us the half-width of the old sun's line on this
                            \ line of the screen
    
     BEQ PLF13              \ If A = 0, skip the following call to HLOIN2 as there
                            \ is no sun line on this line of the screen
    
     JSR HLOIN2             \ Call HLOIN2 to draw a horizontal line on pixel line Y,
                            \ with centre point YY(1 0) and half-width A, and remove
                            \ the line from the sun line heap once done
    
    .PLF13
    
     DEY                    \ Decrement the loop counter
    
     BNE PLFL2              \ Loop back for the next line in the line heap until
                            \ we have either gone through the entire heap, or
                            \ reached the bottom row of the new sun
    
    
    
    
           Name: SUN (Part 3 of 4)                                       [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Draw the sun: Continue to move up the screen, drawing the new sun
                 line by line
      Deep dive: [Drawing the sun](https://elite.bbcelite.com/deep_dives/drawing_the_sun.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sun_part_3_of_4.md)
     References: No direct references to this subroutine in this source file
    
    
    
    
    
    * * *
    
    
     This part draws the new sun. By the time we get to this point, the following
     variables should have been set up by parts 1 and 2:
    
    
    
    * * *
    
    
     Arguments:
    
       V                    As we draw lines for the new sun, V contains the
                            vertical distance between the line we're drawing and the
                            centre of the new sun. As we draw lines and move up the
                            screen, we either decrement (bottom half) or increment
                            (top half) this value
    
       V+1                  This determines which half of the new sun we are drawing
                            as we work our way up the screen, line by line:
    
                              * 0 means we are drawing the bottom half, so the lines
                                get wider as we work our way up towards the centre,
                                at which point we will move into the top half, and
                                V+1 will switch to &FF
    
                              * &FF means we are drawing the top half, so the lines
                                get smaller as we work our way up, away from the
                                centre
    
       TGT                  The maximum y-coordinate of the new sun on-screen (i.e.
                            the screen y-coordinate of the bottom row of the new
                            sun)
    
       CNT                  The fringe size of the new sun
    
       K2(1 0)              The new sun's radius squared, i.e. K^2
    
       Y                    The y-coordinate of the bottom row of the new sun
    
    
    
    
    .PLFL
    
     LDA V                  \ Set (T P) = V * V
     JSR SQUA2              \           = V^2
     STA T
    
     LDA K2                 \ Set (R Q) = K^2 - V^2
     SEC                    \
     SBC P                  \ First calculating the low bytes
     STA Q
    
     LDA K2+1               \ And then doing the high bytes
     SBC T
     STA R
    
     STY Y1                 \ Store Y in Y1, so we can restore it after the call to
                            \ LL5
    
     JSR LL5                \ Set Q = SQRT(R Q)
                            \       = SQRT(K^2 - V^2)
                            \
                            \ So Q contains the half-width of the new sun's line at
                            \ height V from the sun's centre - in other words, it
                            \ contains the half-width of the sun's line on the
                            \ current pixel row Y
    
     LDY Y1                 \ Restore Y from Y1
    
     JSR DORND              \ Set A and X to random numbers
    
     AND CNT                \ Reduce A to a random number in the range 0 to CNT,
                            \ where CNT is the fringe size of the new sun
    
     CLC                    \ Set A = A + Q
     ADC Q                  \
                            \ So A now contains the half-width of the sun on row
                            \ V, plus a random variation based on the fringe size
    
     BCC PLF44              \ If the above addition did not overflow, skip the
                            \ following instruction
    
     LDA #255               \ The above overflowed, so set the value of A to 255
    
                            \ So A contains the half-width of the new sun on pixel
                            \ line Y, changed by a random amount within the size of
                            \ the sun's fringe
    
    .PLF44
    
     LDX LSO,Y              \ Set X to the line heap value for the old sun's line
                            \ at row Y
    
     STA LSO,Y              \ Store the half-width of the new row Y line in the line
                            \ heap
    
     BEQ PLF11              \ If X = 0 then there was no sun line on pixel row Y, so
                            \ jump to PLF11
    
     LDA SUNX               \ Set YY(1 0) = SUNX(1 0), the x-coordinate of the
     STA YY                 \ vertical centre axis of the old sun that's currently
     LDA SUNX+1             \ on-screen
     STA YY+1
    
     TXA                    \ Transfer the line heap value for the old sun's line
                            \ from X into A
    
     JSR EDGES              \ Call EDGES to calculate X1 and X2 for the horizontal
                            \ line centred on YY(1 0) and with half-width A, i.e.
                            \ the line for the old sun
    
     LDA X1                 \ Store X1 and X2, the ends of the line for the old sun,
     STA XX                 \ in XX and XX+1
     LDA X2
     STA XX+1
    
     LDA K3                 \ Set YY(1 0) = K3(1 0), the x-coordinate of the centre
     STA YY                 \ of the new sun
     LDA K3+1
     STA YY+1
    
     LDA LSO,Y              \ Fetch the half-width of the new row Y line from the
                            \ line heap (which we stored above)
    
     JSR EDGES              \ Call EDGES to calculate X1 and X2 for the horizontal
                            \ line centred on YY(1 0) and with half-width A, i.e.
                            \ the line for the new sun
    
     BCS PLF23              \ If the C flag is set, the new line doesn't fit on the
                            \ screen, so jump to PLF23 to just draw the old line
                            \ without drawing the new one
    
                            \ At this point the old line is from XX to XX+1 and the
                            \ new line is from X1 to X2, and both fit on-screen. We
                            \ now want to remove the old line and draw the new one.
                            \ We could do this by simply drawing the old one then
                            \ drawing the new one, but instead Elite does this by
                            \ drawing first from X1 to XX and then from X2 to XX+1,
                            \ which you can see in action by looking at all the
                            \ permutations below of the four points on the line and
                            \ imagining what happens if you draw from X1 to XX and
                            \ X2 to XX+1 using EOR logic. The six possible
                            \ permutations are as follows, along with the result of
                            \ drawing X1 to XX and then X2 to XX+1:
                            \
                            \   X1    X2    XX____XX+1      ->      +__+  +  +
                            \
                            \   X1    XX____X2____XX+1      ->      +__+__+  +
                            \
                            \   X1    XX____XX+1  X2        ->      +__+__+__+
                            \
                            \   XX____X1____XX+1  X2        ->      +  +__+__+
                            \
                            \   XX____XX+1  X1    X2        ->      +  +  +__+
                            \
                            \   XX____X1____X2____XX+1      ->      +  +__+  +
                            \
                            \ They all end up with a line between X1 and X2, which
                            \ is what we want. There's probably a mathematical proof
                            \ of why this works somewhere, but the above is probably
                            \ easier to follow.
                            \
                            \ We can draw from X1 to XX and X2 to XX+1 by swapping
                            \ XX and X2 and drawing from X1 to X2, and then drawing
                            \ from XX to XX+1, so let's do this now
    
     LDA X2                 \ Swap XX and X2
     LDX XX
     STX X2
     STA XX
    
     JSR HLOIN              \ Draw a horizontal line from (X1, Y1) to (X2, Y1)
    
    .PLF23
    
                            \ If we jump here from the BCS above when there is no
                            \ new line this will just draw the old line
    
     LDA XX                 \ Set X1 = XX
     STA X1
    
     LDA XX+1               \ Set X2 = XX+1
     STA X2
    
    .PLF16
    
     JSR HLOIN              \ Draw a horizontal line from (X1, Y1) to (X2, Y1)
    
    .PLF6
    
     DEY                    \ Decrement the line number in Y to move to the line
                            \ above
    
     BEQ PLF8               \ If we have reached the top of the screen, jump to PLF8
                            \ as we are done drawing (the top line of the screen is
                            \ the border, so we don't draw there)
    
     LDA V+1                \ If V+1 is non-zero then we are doing the top half of
     BNE PLF10              \ the new sun, so jump down to PLF10 to increment V and
                            \ decrease the width of the line we draw
    
     DEC V                  \ Decrement V, the height of the sun that we use to work
                            \ out the width, so this makes the line get wider, as we
                            \ move up towards the sun's centre
    
     BNE PLFL               \ If V is non-zero, jump back up to PLFL to do the next
                            \ screen line up
    
     DEC V+1                \ Otherwise V is 0 and we have reached the centre of the
                            \ sun, so decrement V+1 to -1 so we start incrementing V
                            \ each time, thus doing the top half of the new sun
    
    .PLFLS
    
     JMP PLFL               \ Jump back up to PLFL to do the next screen line up
    
    .PLF11
    
                            \ If we get here then there is no old sun line on this
                            \ line, so we can just draw the new sun's line
    
     LDX K3                 \ Set YY(1 0) = K3(1 0), the x-coordinate of the centre
     STX YY                 \ of the new sun's line
     LDX K3+1
     STX YY+1
    
     JSR EDGES              \ Call EDGES to calculate X1 and X2 for the horizontal
                            \ line centred on YY(1 0) and with half-width A, i.e.
                            \ the line for the new sun
    
     BCC PLF16              \ If the line is on-screen, jump up to PLF16 to draw the
                            \ line and loop round for the next line up
    
     LDA #0                 \ The line is not on-screen, so set the line heap for
     STA LSO,Y              \ line Y to 0, which means there is no sun line here
    
     BEQ PLF6               \ Jump up to PLF6 to loop round for the next line up
                            \ (this BEQ is effectively a JMP as A is always zero)
    
    .PLF10
    
     LDX V                  \ Increment V, the height of the sun that we use to work
     INX                    \ out the width, so this makes the line get narrower, as
     STX V                  \ we move up and away from the sun's centre
    
     CPX K                  \ If V <= the radius of the sun, we still have lines to
     BCC PLFLS              \ draw, so jump up to PLFL (via PLFLS) to do the next
     BEQ PLFLS              \ screen line up
    
    
    
    
           Name: SUN (Part 4 of 4)                                       [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Draw the sun: Continue to the top of the screen, erasing the old
                 sun line by line
      Deep dive: [Drawing the sun](https://elite.bbcelite.com/deep_dives/drawing_the_sun.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/sun_part_4_of_4.md)
     Variations: See [code variations](../related/compare/main/subroutine/sun_part_4_of_4.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CIRCLE](../main/subroutine/circle.md) calls via RTS2
    
    
    
    
    
    * * *
    
    
     This part erases any remaining traces of the old sun, now that we have drawn
     all the way to the top of the new sun.
    
    
    
    * * *
    
    
     Other entry points:
    
       RTS2                 Contains an RTS
    
    
    
    
     LDA SUNX               \ Set YY(1 0) = SUNX(1 0), the x-coordinate of the
     STA YY                 \ vertical centre axis of the old sun that's currently
     LDA SUNX+1             \ on-screen
     STA YY+1
    
    .PLFL3
    
     LDA LSO,Y              \ Fetch the Y-th point from the sun line heap, which
                            \ gives us the half-width of the old sun's line on this
                            \ line of the screen
    
     BEQ PLF9               \ If A = 0, skip the following call to HLOIN2 as there
                            \ is no sun line on this line of the screen
    
     JSR HLOIN2             \ Call HLOIN2 to draw a horizontal line on pixel line Y,
                            \ with centre point YY(1 0) and half-width A, and remove
                            \ the line from the sun line heap once done
    
    .PLF9
    
     DEY                    \ Decrement the line number in Y to move to the line
                            \ above
    
     BNE PLFL3              \ Jump up to PLFL3 to redraw the next line up, until we
                            \ have reached the top of the screen
    
    .PLF8
    
                            \ If we get here, we have successfully made it from the
                            \ bottom line of the screen to the top, and the old sun
                            \ has been replaced by the new one
    
     CLC                    \ Clear the C flag to indicate success in drawing the
                            \ sun
    
     LDA K3                 \ Set SUNX(1 0) = K3(1 0)
     STA SUNX
     LDA K3+1
     STA SUNX+1
    
    .RTS2
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: CIRCLE                                                  [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw a circle for the planet
      Deep dive: [Drawing circles](https://elite.bbcelite.com/deep_dives/drawing_circles.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/circle.md)
     Variations: See [code variations](../related/compare/main/subroutine/circle.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PL9 (Part 1 of 3)](../main/subroutine/pl9_part_1_of_3.md) calls CIRCLE
    
    
    
    
    
    * * *
    
    
     Draw a circle with the centre at (K3, K4) and radius K. Used to draw the
     planet's main outline.
    
    
    
    * * *
    
    
     Arguments:
    
       K                    The planet's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the planet
    
       K4(1 0)              Pixel y-coordinate of the centre of the planet
    
    
    
    
    .CIRCLE
    
     JSR CHKON              \ Call CHKON to check whether the circle fits on-screen
    
     BCS RTS2               \ If CHKON set the C flag then the circle does not fit
                            \ on-screen, so return from the subroutine (as RTS2
                            \ contains an RTS)
    
     LDA #0                 \ Set LSX2 = 0 to indicate that the ball line heap is
     STA LSX2               \ not empty, as we are about to fill it
    
     LDX K                  \ Set X = K = radius
    
     LDA #8                 \ Set A = 8
    
     CPX #8                 \ If the radius < 8, skip to PL89
     BCC PL89
    
     LSR A                  \ Halve A so A = 4
    
     CPX #60                \ If the radius < 60, skip to PL89
     BCC PL89
    
     LSR A                  \ Halve A so A = 2
    
    .PL89
    
     STA STP                \ Set STP = A. STP is the step size for the circle, so
                            \ the above sets a smaller step size for bigger circles
    
                            \ Fall through into CIRCLE2 to draw the circle with the
                            \ correct step size
    
    
    
    
           Name: CIRCLE2                                                 [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Draw a circle (for the planet or chart)
      Deep dive: [Drawing circles](https://elite.bbcelite.com/deep_dives/drawing_circles.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/circle2.md)
     Variations: See [code variations](../related/compare/main/subroutine/circle2.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HFS2](../main/subroutine/hfs2.md) calls CIRCLE2
                 * [TT128](../main/subroutine/tt128.md) calls CIRCLE2
    
    
    
    
    
    * * *
    
    
     Draw a circle with the centre at (K3, K4) and radius K. Used to draw the
     planet and the chart circles.
    
    
    
    * * *
    
    
     Arguments:
    
       STP                  The step size for the circle
    
       K                    The circle's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the circle
    
       K4(1 0)              Pixel y-coordinate of the centre of the circle
    
    
    
    * * *
    
    
     Returns:
    
       C flag               The C flag is cleared
    
    
    
    
    .CIRCLE2
    
     LDX #&FF               \ Set FLAG = &FF to reset the ball line heap in the call
     STX FLAG               \ to the BLINE routine below
    
     INX                    \ Set CNT = 0, our counter that goes up to 64, counting
     STX CNT                \ segments in our circle
    
    .PLL3
    
     LDA CNT                \ Set A = CNT
    
     JSR FMLTU2             \ Call FMLTU2 to calculate:
                            \
                            \   A = K * sin(A)
                            \     = K * sin(CNT)
    
     LDX #0                 \ Set T = 0, so we have the following:
     STX T                  \
                            \   (T A) = K * sin(CNT)
                            \
                            \ which is the x-coordinate of the circle for this count
    
     LDX CNT                \ If CNT < 33 then jump to PL37, as this is the right
     CPX #33                \ half of the circle and the sign of the x-coordinate is
     BCC PL37               \ correct
    
     EOR #%11111111         \ This is the left half of the circle, so we want to
     ADC #0                 \ flip the sign of the x-coordinate in (T A) using two's
     TAX                    \ complement, so we start with the low byte and store it
                            \ in X (the ADC adds 1 as we know the C flag is set)
    
     LDA #&FF               \ And then we flip the high byte in T
     ADC #0
     STA T
    
     TXA                    \ Finally, we restore the low byte from X, so we have
                            \ now negated the x-coordinate in (T A)
    
     CLC                    \ Clear the C flag so we can do some more addition below
    
    .PL37
    
     ADC K3                 \ We now calculate the following:
     STA K6                 \
                            \   K6(1 0) = (T A) + K3(1 0)
                            \
                            \ to add the coordinates of the centre to our circle
                            \ point, starting with the low bytes
    
     LDA K3+1               \ And then doing the high bytes, so we now have:
     ADC T                  \
     STA K6+1               \   K6(1 0) = K * sin(CNT) + K3(1 0)
                            \
                            \ which is the result we want for the x-coordinate
    
     LDA CNT                \ Set A = CNT + 16
     CLC
     ADC #16
    
     JSR FMLTU2             \ Call FMLTU2 to calculate:
                            \
                            \   A = K * sin(A)
                            \     = K * sin(CNT + 16)
                            \     = K * cos(CNT)
    
     TAX                    \ Set X = A
                            \       = K * cos(CNT)
    
     LDA #0                 \ Set T = 0, so we have the following:
     STA T                  \
                            \   (T X) = K * cos(CNT)
                            \
                            \ which is the y-coordinate of the circle for this count
    
     LDA CNT                \ Set A = (CNT + 15) mod 64
     ADC #15
     AND #63
    
     CMP #33                \ If A < 33 (i.e. CNT is 0-16 or 48-64) then jump to
     BCC PL38               \ PL38, as this is the bottom half of the circle and the
                            \ sign of the y-coordinate is correct
    
     TXA                    \ This is the top half of the circle, so we want to
     EOR #%11111111         \ flip the sign of the y-coordinate in (T X) using two's
     ADC #0                 \ complement, so we start with the low byte in X (the
     TAX                    \ ADC adds 1 as we know the C flag is set)
    
     LDA #&FF               \ And then we flip the high byte in T, so we have
     ADC #0                 \ now negated the y-coordinate in (T X)
     STA T
    
     CLC                    \ Clear the C flag so the addition at the start of BLINE
                            \ will work
    
    .PL38
    
     JSR BLINE              \ Call BLINE to draw this segment, which also increases
                            \ CNT by STP, the step size
    
     CMP #65                \ If CNT >= 65 then skip the next instruction
     BCS P%+5
    
     JMP PLL3               \ Jump back for the next segment
    
     CLC                    \ Clear the C flag to indicate success
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: WPLS2                                                   [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Remove the planet from the screen
      Deep dive: [The ball line heap](https://elite.bbcelite.com/deep_dives/the_ball_line_heap.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/wpls2.md)
     References: This subroutine is called as follows:
                 * [PL2](../main/subroutine/pl2.md) calls WPLS2
                 * [PL9 (Part 1 of 3)](../main/subroutine/pl9_part_1_of_3.md) calls WPLS2
    
    
    
    
    
    * * *
    
    
     We do this by redrawing it using the lines stored in the ball line heap when
     the planet was originally drawn by the BLINE routine.
    
    
    
    
    .WPLS2
    
     LDY LSX2               \ If LSX2 is non-zero (which indicates the ball line
     BNE WP1                \ heap is empty), jump to WP1 to reset the line heap
                            \ without redrawing the planet
    
                            \ Otherwise Y is now 0, so we can use it as a counter to
                            \ loop through the lines in the line heap, redrawing
                            \ each one to remove the planet from the screen, before
                            \ resetting the line heap once we are done
    
    .WPL1
    
     CPY LSP                \ If Y >= LSP then we have reached the end of the line
     BCS WP1                \ heap and have finished redrawing the planet (as LSP
                            \ points to the end of the heap), so jump to WP1 to
                            \ reset the line heap, returning from the subroutine
                            \ using a tail call
    
     LDA LSY2,Y             \ Set A to the y-coordinate of the current heap entry
    
     CMP #&FF               \ If the y-coordinate is &FF, this indicates that the
     BEQ WP2                \ next point in the heap denotes the start of a line
                            \ segment, so jump to WP2 to put it into (X1, Y1)
    
     STA Y2                 \ Set (X2, Y2) to the x- and y-coordinates from the
     LDA LSX2,Y             \ heap
     STA X2
    
     JSR LOIN               \ Draw a line from (X1, Y1) to (X2, Y2)
    
     INY                    \ Increment the loop counter to point to the next point
    
     LDA SWAP               \ If SWAP is non-zero then we swapped the coordinates
     BNE WPL1               \ when filling the heap in BLINE, so loop back WPL1
                            \ for the next point in the heap
    
     LDA X2                 \ Swap (X1, Y1) and (X2, Y2), so the next segment will
     STA X1                 \ be drawn from the current (X2, Y2) to the next point
     LDA Y2                 \ in the heap
     STA Y1
    
     JMP WPL1               \ Loop back to WPL1 for the next point in the heap
    
    .WP2
    
     INY                    \ Increment the loop counter to point to the next point
    
     LDA LSX2,Y             \ Set (X1, Y1) to the x- and y-coordinates from the
     STA X1                 \ heap
     LDA LSY2,Y
     STA Y1
    
     INY                    \ Increment the loop counter to point to the next point
    
     JMP WPL1               \ Loop back to WPL1 for the next point in the heap
    
    
    
    
           Name: WP1                                                     [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Reset the ball line heap
    
    
        Context: See this subroutine [on its own page](../main/subroutine/wp1.md)
     References: This subroutine is called as follows:
                 * [WPLS2](../main/subroutine/wpls2.md) calls WP1
    
    
    
    
    
    
    .WP1
    
     LDA #1                 \ Set LSP = 1 to reset the ball line heap pointer
     STA LSP
    
     LDA #&FF               \ Set LSX2 = &FF to indicate the ball line heap is empty
     STA LSX2
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: WPLS                                                    [Show more]
           Type: Subroutine
       Category: Drawing suns
        Summary: Remove the sun from the screen
      Deep dive: [Drawing the sun](https://elite.bbcelite.com/deep_dives/drawing_the_sun.html)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/wpls.md)
     Variations: See [code variations](../related/compare/main/subroutine/wpls.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main flight loop (Part 14 of 16)](../main/subroutine/main_flight_loop_part_14_of_16.md) calls WPLS
                 * [PL2](../main/subroutine/pl2.md) calls WPLS
                 * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md) calls WPLS
    
    
    
    
    
    * * *
    
    
     We do this by redrawing it using the lines stored in the sun line heap when
     the sun was originally drawn by the SUN routine.
    
    
    
    * * *
    
    
     Arguments:
    
       SUNX(1 0)            The x-coordinate of the vertical centre axis of the sun
    
    
    
    * * *
    
    
     Other entry points:
    
       WPLS-1               Contains an RTS
    
    
    
    
    .WPLS
    
     LDA LSX                \ If LSX < 0, the sun line heap is empty, so return from
     BMI WPLS-1             \ the subroutine (as WPLS-1 contains an RTS)
    
     LDA SUNX               \ Set YY(1 0) = SUNX(1 0), the x-coordinate of the
     STA YY                 \ vertical centre axis of the sun that's currently on
     LDA SUNX+1             \ screen
     STA YY+1
    
     LDY #2*Y-1             \ #Y is the y-coordinate of the centre of the space
                            \ view, so this sets Y as a counter for the number of
                            \ lines in the space view (i.e. 191), which is also the
                            \ number of lines in the LSO block
    
    .WPL2
    
     LDA LSO,Y              \ Fetch the Y-th point from the sun line heap, which
                            \ gives us the half-width of the sun's line on this line
                            \ of the screen
    
     BEQ P%+5               \ If A = 0, skip the following call to HLOIN2 as there
                            \ is no sun line on this line of the screen
    
     JSR HLOIN2             \ Call HLOIN2 to draw a horizontal line on pixel line Y,
                            \ with centre point YY(1 0) and half-width A, and remove
                            \ the line from the sun line heap once done
    
     DEY                    \ Decrement the loop counter
    
     BNE WPL2               \ Loop back for the next line in the line heap until
                            \ we have gone through the entire heap
    
     DEY                    \ This sets Y to &FF, as we end the loop with Y = 0
    
     STY LSX                \ Set LSX to &FF to indicate the sun line heap is empty
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: EDGES                                                   [Show more]
           Type: Subroutine
       Category: Drawing lines
        Summary: Draw a horizontal line given a centre and a half-width
    
    
        Context: See this subroutine [on its own page](../main/subroutine/edges.md)
     Variations: See [code variations](../related/compare/main/subroutine/edges.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [HLOIN2](../main/subroutine/hloin2.md) calls EDGES
                 * [SUN (Part 3 of 4)](../main/subroutine/sun_part_3_of_4.md) calls EDGES
    
    
    
    
    
    * * *
    
    
     Set X1 and X2 to the x-coordinates of the ends of the horizontal line with
     centre x-coordinate YY(1 0), and length A in either direction from the centre
     (so a total line length of 2 * A). In other words, this line:
    
       X1             YY(1 0)             X2
       +-----------------+-----------------+
             <- A ->           <- A ->
    
     The resulting line gets clipped to the edges of the screen, if needed. If the
     calculation doesn't overflow, we return with the C flag clear, otherwise the C
     flag gets set to indicate failure and the Y-th LSO entry gets set to 0.
    
    
    
    * * *
    
    
     Arguments:
    
       A                    The half-length of the line
    
       YY(1 0)              The centre x-coordinate
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Clear if the line fits on-screen, set if it doesn't
    
       X1, X2               The x-coordinates of the clipped line
    
       LSO+Y                If the line doesn't fit, LSO+Y is set to 0
    
       Y                    Y is preserved
    
    
    
    
    .EDGES
    
     STA T                  \ Set T to the line's half-length in argument A
    
     CLC                    \ We now calculate:
     ADC YY                 \
     STA X2                 \  (A X2) = YY(1 0) + A
                            \
                            \ to set X2 to the x-coordinate of the right end of the
                            \ line, starting with the low bytes
    
     LDA YY+1               \ And then adding the high bytes
     ADC #0
    
     BMI ED1                \ If the addition is negative then the calculation has
                            \ overflowed, so jump to ED1 to return a failure
    
     BEQ P%+6               \ If the high byte A from the result is 0, skip the
                            \ next two instructions, as the result already fits on
                            \ the screen
    
     LDA #255               \ The high byte is positive and non-zero, so we went
     STA X2                 \ past the right edge of the screen, so clip X2 to the
                            \ x-coordinate of the right edge of the screen
    
     LDA YY                 \ We now calculate:
     SEC                    \
     SBC T                  \   (A X1) = YY(1 0) - argument A
     STA X1                 \
                            \ to set X1 to the x-coordinate of the left end of the
                            \ line, starting with the low bytes
    
     LDA YY+1               \ And then subtracting the high bytes
     SBC #0
    
     BNE ED3                \ If the high byte subtraction is non-zero, then skip
                            \ to ED3
    
     CLC                    \ Otherwise the high byte of the subtraction was zero,
                            \ so the line fits on-screen and we clear the C flag to
                            \ indicate success
    
     RTS                    \ Return from the subroutine
    
    .ED3
    
     BPL ED1                \ If the addition is positive then the calculation has
                            \ underflowed, so jump to ED1 to return a failure
    
     LDA #0                 \ The high byte is negative and non-zero, so we went
     STA X1                 \ past the left edge of the screen, so clip X1 to the
                            \ x-coordinate of the left edge of the screen
    
     CLC                    \ The line does fit on-screen, so clear the C flag to
                            \ indicate success
    
     RTS                    \ Return from the subroutine
    
    .ED1
    
     LDA #0                 \ Set the Y-th byte of the LSO block to 0
     STA LSO,Y
    
     SEC                    \ The line does not fit on the screen, so set the C flag
                            \ to indicate this result
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: CHKON                                                   [Show more]
           Type: Subroutine
       Category: Drawing circles
        Summary: Check whether any part of a circle appears on the extended screen
    
    
        Context: See this subroutine [on its own page](../main/subroutine/chkon.md)
     Variations: See [code variations](../related/compare/main/subroutine/chkon.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CIRCLE](../main/subroutine/circle.md) calls CHKON
                 * [SUN (Part 1 of 4)](../main/subroutine/sun_part_1_of_4.md) calls CHKON
    
    
    
    
    
    * * *
    
    
     Arguments:
    
       K                    The circle's radius
    
       K3(1 0)              Pixel x-coordinate of the centre of the circle
    
       K4(1 0)              Pixel y-coordinate of the centre of the circle
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Clear if any part of the circle appears on-screen, set
                            if none of the circle appears on-screen
    
       (A X)                Minimum y-coordinate of the circle on-screen (i.e. the
                            y-coordinate of the top edge of the circle)
    
       P(2 1)               Maximum y-coordinate of the circle on-screen (i.e. the
                            y-coordinate of the bottom edge of the circle)
    
    
    
    
    .CHKON
    
     LDA K3                 \ Set A = K3 + K
     CLC
     ADC K
    
     LDA K3+1               \ Set A = K3+1 + 0 + any carry from above, so this
     ADC #0                 \ effectively sets A to the high byte of K3(1 0) + K:
                            \
                            \   (A ?) = K3(1 0) + K
                            \
                            \ so A is the high byte of the x-coordinate of the right
                            \ edge of the circle
    
     BMI PL21               \ If A is negative then the right edge of the circle is
                            \ to the left of the screen, so jump to PL21 to set the
                            \ C flag and return from the subroutine, as the whole
                            \ circle is off-screen to the left
    
     LDA K3                 \ Set A = K3 - K
     SEC
     SBC K
    
     LDA K3+1               \ Set A = K3+1 - 0 - any carry from above, so this
     SBC #0                 \ effectively sets A to the high byte of K3(1 0) - K:
                            \
                            \   (A ?) = K3(1 0) - K
                            \
                            \ so A is the high byte of the x-coordinate of the left
                            \ edge of the circle
    
     BMI PL31               \ If A is negative then the left edge of the circle is
                            \ to the left of the screen, and we already know the
                            \ right edge is either on-screen or off-screen to the
                            \ right, so skip to PL31 to move on to the y-coordinate
                            \ checks, as at least part of the circle is on-screen in
                            \ terms of the x-axis
    
     BNE PL21               \ If A is non-zero, then the left edge of the circle is
                            \ to the right of the screen, so jump to PL21 to set the
                            \ C flag and return from the subroutine, as the whole
                            \ circle is off-screen to the right
    
    .PL31
    
     LDA K4                 \ Set P+1 = K4 + K
     CLC
     ADC K
     STA P+1
    
     LDA K4+1               \ Set A = K4+1 + 0 + any carry from above, so this
     ADC #0                 \ does the following:
                            \
                            \   (A P+1) = K4(1 0) + K
                            \
                            \ so A is the high byte of the y-coordinate of the
                            \ bottom edge of the circle
    
     BMI PL21               \ If A is negative then the bottom edge of the circle is
                            \ above the top of the screen, so jump to PL21 to set
                            \ the C flag and return from the subroutine, as the
                            \ whole circle is off-screen to the top
    
     STA P+2                \ Store the high byte in P+2, so now we have:
                            \
                            \   P(2 1) = K4(1 0) + K
                            \
                            \ i.e. the maximum y-coordinate of the circle on-screen
                            \ (which we return)
    
     LDA K4                 \ Set X = K4 - K
     SEC
     SBC K
     TAX
    
     LDA K4+1               \ Set A = K4+1 - 0 - any carry from above, so this
     SBC #0                 \ does the following:
                            \
                            \   (A X) = K4(1 0) - K
                            \
                            \ so A is the high byte of the y-coordinate of the top
                            \ edge of the circle
    
     BMI PL44               \ If A is negative then the top edge of the circle is
                            \ above the top of the screen, and we already know the
                            \ bottom edge is either on-screen or below the bottom
                            \ of the screen, so skip to PL44 to clear the C flag and
                            \ return from the subroutine using a tail call, as part
                            \ of the circle definitely appears on-screen
    
     BNE PL21               \ If A is non-zero, then the top edge of the circle is
                            \ below the bottom of the screen, so jump to PL21 to set
                            \ the C flag and return from the subroutine, as the
                            \ whole circle is off-screen to the bottom
    
     CPX Yx2M1              \ If we get here then A is zero, which means the top
                            \ edge of the circle is within the screen boundary, so
                            \ now we need to check whether it is in the space view
                            \ (in which case it is on-screen) or the dashboard (in
                            \ which case the top of the circle is hidden by the
                            \ dashboard, so the circle isn't on-screen). We do this
                            \ by checking the low byte of the result in X against
                            \ Yx2M1, and returning the C flag from this comparison.
                            \ The value in Yx2M1 is the y-coordinate of the bottom
                            \ pixel row of the space view, so this does the
                            \ following:
                            \
                            \   * The C flag is set if coordinate (A X) is below the
                            \     bottom row of the space view, i.e. the top edge of
                            \     the circle is hidden by the dashboard
                            \
                            \   * The C flag is clear if coordinate (A X) is above
                            \     the bottom row of the space view, i.e. the top
                            \     edge of the circle is on-screen
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PL21                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Return from a planet/sun-drawing routine with a failure flag
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pl21.md)
     Variations: See [code variations](../related/compare/main/subroutine/pl21.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [CHKON](../main/subroutine/chkon.md) calls PL21
                 * [PLS6](../main/subroutine/pls6.md) calls PL21
    
    
    
    
    
    * * *
    
    
     Set the C flag and return from the subroutine. This is used to return from a
     planet- or sun-drawing routine with the C flag indicating an overflow in the
     calculation.
    
    
    
    
    .PL21
    
     SEC                    \ Set the C flag to indicate an overflow
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PLS3                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Calculate (Y A P) = 222 * roofv_x / z
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pls3.md)
     References: This subroutine is called as follows:
                 * [PL9 (Part 3 of 3)](../main/subroutine/pl9_part_3_of_3.md) calls PLS3
    
    
    
    
    
    * * *
    
    
     Calculate the following, with X determining the vector to use:
    
       (Y A P) = 222 * roofv_x / z
    
     though in reality only (Y A) is used.
    
     Although the code below supports a range of values of X, in practice the
     routine is only called with X = 15, and then again after X has been
     incremented to 17. So the values calculated by PLS1 use roofv_x first, then
     roofv_y. The comments below refer to roofv_x, for the first call.
    
    
    
    * * *
    
    
     Arguments:
    
       X                    Determines which of the INWK orientation vectors to
                            divide:
    
                              * X = 15: divides roofv_x
    
                              * X = 17: divides roofv_y
    
    
    
    * * *
    
    
     Returns:
    
       X                    X gets incremented by 2 so it points to the next
                            coordinate in this orientation vector (so consecutive
                            calls to the routine will start with x, then move onto y
                            and then z)
    
    
    
    
    .PLS3
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA P                  \
                            \   P = |roofv_x / z|
                            \   K+3 = sign of roofv_x / z
                            \
                            \ and increment X to point to roofv_y for the next call
    
     LDA #222               \ Set Q = 222, the offset to the crater
     STA Q
    
     STX U                  \ Store the vector index X in U for retrieval after the
                            \ call to MULTU
    
     JSR MULTU              \ Call MULTU to calculate
                            \
                            \   (A P) = P * Q
                            \         = 222 * |roofv_x / z|
    
     LDX U                  \ Restore the vector index from U into X
    
     LDY K+3                \ If the sign of the result in K+3 is positive, skip to
     BPL PL12               \ PL12 to return with Y = 0
    
     EOR #&FF               \ Otherwise the result should be negative, so negate the
     CLC                    \ high byte of the result using two's complement with
     ADC #1                 \ A = ~A + 1
    
     BEQ PL12               \ If A = 0, jump to PL12 to return with (Y A) = 0
    
     LDY #&FF               \ Set Y = &FF to be a negative high byte
    
     RTS                    \ Return from the subroutine
    
    .PL12
    
     LDY #0                 \ Set Y = 0 to be a positive high byte
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PLS4                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Calculate CNT2 = arctan(P / A) / 4
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pls4.md)
     References: This subroutine is called as follows:
                 * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md) calls PLS4
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       CNT2 = arctan(P / A) / 4
    
     and do the following if nosev_z_hi >= 0:
    
       CNT2 = CNT2 + 32
    
     which is the equivalent of adding 180 degrees to the result (or PI radians),
     as there are 64 segments in a full circle.
    
     This routine is called with the following arguments when calculating the
     equator and meridian for planets:
    
       * A = roofv_z_hi, P = -nosev_z_hi
    
       * A = sidev_z_hi, P = -nosev_z_hi
    
     So it calculates the angle between the planet's orientation vectors, in the
     z-axis.
    
    
    
    
    .PLS4
    
     STA Q                  \ Set Q = A
    
     JSR ARCTAN             \ Call ARCTAN to calculate:
                            \
                            \   A = arctan(P / Q)
                            \       arctan(P / A)
                            \
                            \ The result in A will be in the range 0 to 128, which
                            \ represents an angle of 0 to 180 degrees (or 0 to PI
                            \ radians)
    
     LDX INWK+14            \ If nosev_z_hi is negative, skip the following
     BMI P%+4               \ instruction to leave the angle in A as a positive
                            \ integer in the range 0 to 128 (so when we calculate
                            \ CNT2 below, it will be in the right half of the
                            \ anti-clockwise arc that we describe when drawing
                            \ circles, i.e. from 6 o'clock, through 3 o'clock and
                            \ on to 12 o'clock)
    
     EOR #%10000000         \ If we get here then nosev_z_hi is positive, so flip
                            \ bit 7 of the angle in A, which is the same as adding
                            \ 128 to give a result in the range 129 to 256 (i.e. 129
                            \ to 0), or 180 to 360 degrees (so when we calculate
                            \ CNT2 below, it will be in the left half of the
                            \ anti-clockwise arc that we describe when drawing
                            \ circles, i.e. from 12 o'clock, through 9 o'clock and
                            \ on to 6 o'clock)
    
     LSR A                  \ Set CNT2 = A / 4
     LSR A
     STA CNT2
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PLS5                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Calculate roofv_x / z and roofv_y / z
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pls5.md)
     References: This subroutine is called as follows:
                 * [PL9 (Part 2 of 3)](../main/subroutine/pl9_part_2_of_3.md) calls PLS5
    
    
    
    
    
    * * *
    
    
     Calculate the following divisions of a specified value from one of the
     orientation vectors (in this example, roofv):
    
       (XX16+2 K2+2) = roofv_x / z
    
       (XX16+3 K2+3) = roofv_y / z
    
    
    
    * * *
    
    
     Arguments:
    
       X                    Determines which of the INWK orientation vectors to
                            divide:
    
                              * X = 15: divides roofv_x and roofv_y
    
                              * X = 21: divides sidev_x and sidev_y
    
       INWK                 The planet's ship data block
    
    
    
    
    .PLS5
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA K2+2               \
     STY XX16+2             \   K+2    = |roofv_x / z|
                            \   XX16+2 = sign of roofv_x / z
                            \
                            \ i.e. (XX16+2 K2+2) = roofv_x / z
                            \
                            \ and increment X to point to roofv_y for the next call
    
     JSR PLS1               \ Call PLS1 to calculate the following:
     STA K2+3               \
     STY XX16+3             \   K+3    = |roofv_y / z|
                            \   XX16+3 = sign of roofv_y / z
                            \
                            \ i.e. (XX16+3 K2+3) = roofv_y / z
                            \
                            \ and increment X to point to roofv_z for the next call
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: PLS6                                                    [Show more]
           Type: Subroutine
       Category: Drawing planets
        Summary: Calculate (X K) = (A P+1 P) / (z_sign z_hi z_lo)
    
    
        Context: See this subroutine [on its own page](../main/subroutine/pls6.md)
     Variations: See [code variations](../related/compare/main/subroutine/pls6.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [PROJ](../main/subroutine/proj.md) calls PLS6
                 * [CHKON](../main/subroutine/chkon.md) calls via PL44
                 * [YESNO](../main/subroutine/yesno.md) calls via PL6
    
    
    
    
    
    * * *
    
    
     Calculate the following:
    
       (X K) = (A P+1 P) / (z_sign z_hi z_lo)
    
     returning an overflow in the C flag if the result is >= 1024.
    
    
    
    * * *
    
    
     Arguments:
    
       INWK                 The planet or sun's ship data block
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if the result >= 1024, clear otherwise
    
    
    
    * * *
    
    
     Other entry points:
    
       PL44                 Clear the C flag and return from the subroutine
    
       PL6                  Contains an RTS
    
    
    
    
    .PLS6
    
     JSR DVID3B2            \ Call DVID3B2 to calculate:
                            \
                            \   K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)
    
     LDA K+3                \ Set A = |K+3| OR K+2
     AND #%01111111
     ORA K+2
    
     BNE PL21               \ If A is non-zero then the two high bytes of K(3 2 1 0)
                            \ are non-zero, so jump to PL21 to set the C flag and
                            \ return from the subroutine
    
                            \ We can now just consider K(1 0), as we know the top
                            \ two bytes of K(3 2 1 0) are both 0
    
     LDX K+1                \ Set X = K+1, so now (X K) contains the result in
                            \ K(1 0), which is the format we want to return the
                            \ result in
    
     CPX #4                 \ If the high byte of K(1 0) >= 4 then the result is
     BCS PL6                \ >= 1024, so return from the subroutine with the C flag
                            \ set to indicate an overflow (as PL6 contains an RTS)
    
     LDA K+3                \ Fetch the sign of the result from K+3 (which we know
                            \ has zeroes in bits 0-6, so this just fetches the sign)
    
    \CLC                    \ This instruction is commented out in the original
                            \ source. It would have no effect as we know the C flag
                            \ is already clear, as we skipped past the BCS above
    
     BPL PL6                \ If the sign bit is clear and the result is positive,
                            \ then the result is already correct, so return from
                            \ the subroutine with the C flag clear to indicate
                            \ success (as PL6 contains an RTS)
    
     LDA K                  \ Otherwise we need to negate the result, which we do
     EOR #%11111111         \ using two's complement, starting with the low byte:
     ADC #1                 \
     STA K                  \   K = ~K + 1
    
     TXA                    \ And then the high byte:
     EOR #%11111111         \
     ADC #0                 \   X = ~X
     TAX
    
    .PL44
    
     CLC                    \ Clear the C flag to indicate success
    
    .PL6
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: YESNO                                                   [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Wait until either "Y" or "N" is pressed
    
    
        Context: See this subroutine [on its own page](../main/subroutine/yesno.md)
     References: This subroutine is called as follows:
                 * [SVE](../main/subroutine/sve.md) calls YESNO
    
    
    
    
    
    * * *
    
    
     Returns:
    
       C flag               Set if "Y" was pressed, clear if "N" was pressed
    
    
    
    
    .YESNO
    
     JSR t                  \ Scan the keyboard until a key is pressed, returning
                            \ the ASCII code in A and X
    
     CMP #'Y'               \ If "Y" was pressed, return from the subroutine with
     BEQ PL6                \ the C flag set (as the CMP sets the C flag, and PL6
                            \ contains an RTS)
    
     CMP #'N'               \ If "N" was not pressed, loop back to keep scanning
     BNE YESNO              \ for key presses
    
     CLC                    \ Clear the C flag
    
     RTS                    \ Return from the subroutine
    
    
    
    
           Name: TT17                                                    [Show more]
           Type: Subroutine
       Category: Keyboard
        Summary: Scan the keyboard for cursor key or joystick movement
    
    
        Context: See this subroutine [on its own page](../main/subroutine/tt17.md)
     Variations: See [code variations](../related/compare/main/subroutine/tt17.md) for this subroutine in the different versions
     References: This subroutine is called as follows:
                 * [Main game loop (Part 5 of 6)](../main/subroutine/main_game_loop_part_5_of_6.md) calls TT17
                 * [DJOY](../main/subroutine/djoy.md) calls via TJ1
    
    
    
    
    
    * * *
    
    
     Scan the keyboard and joystick for cursor key or stick movement, and return
     the result as deltas (changes) in x- and y-coordinates as follows:
    
       * For joystick, X and Y are integers between -2 and +2 depending on how far
         the stick has moved
    
       * For keyboard, X and Y are integers between -1 and +1 depending on which
         keys are pressed
    
    
    
    * * *
    
    
     Returns:
    
       A                    The key pressed, if the arrow keys were used
    
       X                    Change in the x-coordinate according to the cursor keys
                            being pressed or joystick movement, as an integer (see
                            above)
    
       Y                    Change in the y-coordinate according to the cursor keys
                            being pressed or joystick movement, as an integer (see
                            above)
    
    
    
    * * *
    
    
     Other entry points:
    
       TJ1                  Check for cursor key presses and return the combined
                            deltas for the digital joystick and cursor keys (Master
                            Compact only)
    
    
    
    
    .TT17
    
     LDA QQ11               \ If this not the space view, skip the following three
     BNE TT17afterall       \ instructions to move onto the cursor key logic
    
     JSR DOKEY              \ This is the space view, so scan the keyboard for
                            \ flight controls and pause keys, (or the equivalent on
                            \ joystick) and update the key logger, setting KL to the
                            \ key pressed
    
     TXA                    \ Transfer the value of the key pressed from X to A
    
     RTS                    \ Return from the subroutine
    
    .TT17afterall
    
     JSR DOKEY              \ Scan the keyboard for flight controls and pause keys,
                            \ (or the equivalent on joystick) and update the key
                            \ logger, setting KL to the key pressed
    
    IF _COMPACT
    
     LDX #0                 \ Set the initial values for the results, X = Y = 0,
     LDY #0                 \ which we now increase or decrease appropriately
    
    ENDIF
    
     LDA JSTK               \ If the joystick is not configured, jump down to TJ1,
     BEQ TJ1                \ otherwise we move the cursor with the joystick
    
    IF _COMPACT
    
     LDA MOS                \ If MOS = 0 then this is a Master Compact, so jump to
     BEQ DJOY               \ DJOY to read the digital joystick before rejoining the
                            \ routine below at TJ1
    
    ENDIF
    
     LDA JSTY               \ Fetch the joystick pitch, ranging from 1 to 255 with
                            \ 128 as the centre point
    
     JSR TJS1               \ Call TJS1 just below to set A to a value between -4
                            \ and +4 depending on the joystick pitch value (moving
                            \ the stick up and down)
    
     TAY                    \ Copy the result into Y
    
     LDA JSTX               \ Fetch the joystick roll, ranging from 1 to 255 with
                            \ 128 as the centre point
    
     EOR #&FF               \ Flip the sign so A = -JSTX, because the joystick roll
                            \ works in the opposite way to moving a cursor on-screen
                            \ in terms of left and right
    
     JSR TJS1               \ Call TJS1 just below to set A to a value between -4
                            \ and +4 depending on the joystick roll value (moving
                            \ the stick sideways)
    
     TAX                    \ Copy the value of A into X
    
    IF _SNG47
    
     LDA KL                 \ Set A to the value of KL (the key pressed)
    
     RTS                    \ Return from the subroutine
    
    ENDIF
    
    .TJ1
    
     LDA KL                 \ Set A to the value of KL (the key pressed)
    
    IF _SNG47
    
     LDX #0                 \ Set the results, X = Y = 0
     LDY #0
    
    ENDIF
    
     CMP #&8C               \ If left arrow was pressed, set X = X - 1
     BNE P%+3
     DEX
    
     CMP #&8D               \ If right arrow was pressed, set X = X + 1
     BNE P%+3
     INX
    
     CMP #&8E               \ If down arrow was pressed, set Y = Y - 1
     BNE P%+3
     DEY
    
     CMP #&8F               \ If up arrow was pressed, set Y = Y + 1
     BNE P%+3
     INY
    
     PHX                    \ Store X (which contains the change in the
                            \ x-coordinate) on the stack so we can retrieve it later
    
    IF _SNG47
    
     LDA #0                 \ Call DKS5 to check whether the SHIFT key is being
     JSR DKS5               \ pressed
    
    ELIF _COMPACT
    
     LDA #0                 \ Call DKS4mc to check whether the SHIFT key is being
     JSR DKS4mc             \ pressed
    
    ENDIF
    
     BMI speedup            \ If SHIFT is being pressed, skip the next three
                            \ instructions
    
     PLX                    \ SHIFT is not being pressed, so retrieve the value of X
                            \ we stored above so we can return it
    
     LDA KL                 \ Set A to the value of KL (the key pressed)
    
     RTS                    \ Return from the subroutine
    
    .speedup
    
     PLA                    \ Pull the value of X from the stack into A, so A now
                            \ contains the change in the x-coordinate
    
     ASL A                  \ SHIFT is being held down, so quadruple the value of A
     ASL A                  \ (i.e. SHIFT moves the cursor at four times the speed
                            \ when using the joystick)
    
     TAX                    \ Put the amended value of A back into X
    
     TYA                    \ Now to do the same with the change in y-coordinate, so
                            \ fetch the value of Y into A
    
     ASL A                  \ SHIFT is being held down, so quadruple the value of A
     ASL A                  \ (i.e. SHIFT moves the cursor at four times the speed
                            \ when using the joystick)
    
     TAY                    \ Put the amended value of A back into Y
    
    IF _COMPACT
    
     STZ KY7                \ Clear the key logger at KY7 to reset the "A" (fire)
                            \ button to "not pressed"
    
    ENDIF
    
     LDA KL                 \ Set A to the value of KL (the key pressed)
    
     RTS                    \ Return from the subroutine
    
    .TJS1
    
                            \ This routine calculates the following:
                            \
                            \   A = round(A / 16) - 4
                            \
                            \ This sets A to a value between -4 and +4, given an
                            \ initial value ranging from 1 to 255 with 128 as
                            \ the centre point
    
     LSR A                  \ Set A = A / 16
     LSR A                  \
     LSR A                  \ and C contains the last bit to be shifted out
     LSR A
     LSR A
    
     ADC #0                 \ If that last bit was a 1, this increments A, so
                            \ this effectively implements a rounding function,
                            \ where 0.5 and above get rounded up
    
     SBC #3                 \ The addition will not overflow, so the C flag is
                            \ clear at this point, so this performs:
                            \
                            \   A = A - 3 - (1 - C)
                            \     = A - 3 - (1 - 0)
                            \     = A - 3 - 1
                            \     = A - 4
    
     RTS                    \ Return from the subroutine
    
    
    
     Save ELTE.bin
    
    
    
    
     PRINT "ELITE E"
     PRINT "Assembled at ", ~CODE_E%
     PRINT "Ends at ", ~P%
     PRINT "Code size is ", ~(P% - CODE_E%)
     PRINT "Execute at ", ~LOAD%
     PRINT "Reload at ", ~LOAD_E%
    
     PRINT "S.ELTE ", ~CODE_E%, " ", ~P%, " ", ~LOAD%, " ", ~LOAD_E%
    \SAVE "3-assembled-output/ELTE.bin", CODE_E%, P%, LOAD%
    
    
    

[X]

Subroutine [ADD](elite_c.md#header-add) (category: Maths (Arithmetic))

Calculate (A X) = (A P) + (S R)

[X]

Subroutine [ARCTAN](elite_c.md#header-arctan) (category: Maths (Geometry))

Calculate A = arctan(P / Q)

[X]

Variable [ASH](workspaces.md#ash) in workspace [ZP](workspaces.md#header-zp)

Aft shield status

[X]

Subroutine [BLINE](elite_b.md#header-bline) (category: Drawing circles)

Draw a circle segment and add it to the ball line heap

[X]

Subroutine [BPRNT](elite_b.md#header-bprnt) (category: Text)

Print a 32-bit number, left-padded to a specific number of digits, with an optional decimal point

[X]

Variable [CASH](workspaces.md#cash) in workspace [WP](workspaces.md#header-wp)

Our current cash pot

[X]

Subroutine [CHKON](elite_e.md#header-chkon) (category: Drawing circles)

Check whether any part of a circle appears on the extended screen

[X]

Subroutine [CIRCLE](elite_e.md#header-circle) (category: Drawing circles)

Draw a circle for the planet

[X]

Variable [CNT](workspaces.md#cnt) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the number of iterations required when looping

[X]

Variable [CNT2](workspaces.md#cnt2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in the planet-drawing routine to store the segment number where the arc of a partial circle should start

[X]

Variable [COL](workspaces.md#col) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store colour information when drawing pixels in the dashboard

[X]

Variable [COMC](elite_a.md#comc) in workspace [UP](elite_a.md#header-up)

The colour of the dot on the compass

[X]

Variable [COMX](workspaces.md#comx) in workspace [WP](workspaces.md#header-wp)

The x-coordinate of the compass dot

[X]

Variable [COMY](workspaces.md#comy) in workspace [WP](workspaces.md#header-wp)

The y-coordinate of the compass dot

[X]

Configuration variable [CYAN](workspaces.md#cyan)

Four mode 1 pixels of colour 3 (cyan or white)

[X]

Subroutine [DEATH](elite_f.md#header-death) (category: Start and end)

Display the death screen

[X]

Subroutine [DJOY](elite_f.md#header-djoy) (category: Keyboard)

Scan the keyboard for cursor key or digital joystick movement

[X]

Subroutine [DKS4mc](elite_a.md#header-dks4mc) (category: Keyboard)

Scan the Master Compact keyboard to see if a specific key is being pressed

[X]

Subroutine [DKS5](elite_h.md#header-dks5) (category: Keyboard)

Scan the keyboard to see if a specific key is being pressed

[X]

Configuration variable [DOD](workspaces.md#dod)

Ship type for a Dodecahedron ("Dodo") space station

[X]

Subroutine [DOKEY](elite_f.md#header-dokey) (category: Keyboard)

Scan for the seven primary flight controls and apply the docking computer manoeuvring code

[X]

Subroutine [DORND](elite_f.md#header-dornd) (category: Maths (Arithmetic))

Generate random numbers

[X]

Subroutine [DOT](elite_a.md#header-dot) (category: Dashboard)

Draw a dash on the compass

[X]

Subroutine [DOXC](elite_d.md#header-doxc) (category: Text)

Move the text cursor to a specific column

[X]

Configuration variable [DUST](workspaces.md#dust)

Four mode 1 pixels of colour 3, 2, 3, 2 (cyan/red)

[X]

Subroutine [DVID3B2](elite_c.md#header-dvid3b2) (category: Maths (Arithmetic))

Calculate K(3 2 1 0) = (A P+1 P) / (z_sign z_hi z_lo)

[X]

Subroutine [DVID4](elite_c.md#header-dvid4) (category: Maths (Arithmetic))

Calculate (P R) = 256 * A / Q

[X]

Configuration variable [E%](workspaces.md#e-per-cent)

The address of the default NEWB ship bytes, as set in elite-data.asm

[X]

Label [ED1](elite_e.md#ed1) in subroutine [EDGES](elite_e.md#header-edges)

[X]

Label [ED3](elite_e.md#ed3) in subroutine [EDGES](elite_e.md#header-edges)

[X]

Subroutine [EDGES](elite_e.md#header-edges) (category: Drawing lines)

Draw a horizontal line given a centre and a half-width

[X]

Variable [ENERGY](workspaces.md#energy) in workspace [ZP](workspaces.md#header-zp)

Energy bank status

[X]

Label [EX11](elite_e.md#ex11) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [EX2](elite_e.md#ex2) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [EX4](elite_e.md#ex4) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [EX5](elite_e.md#ex5) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [EXL1](elite_e.md#exl1) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [EXL2](elite_e.md#exl2) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [EXL3](elite_e.md#exl3) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [EXL4](elite_e.md#exl4) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [EXL5](elite_e.md#exl5) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Subroutine [EXNO3](elite_h.md#header-exno3) (category: Sound)

Make an explosion sound

[X]

Label [EXS1](elite_e.md#exs1) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Variable [FIST](workspaces.md#fist) in workspace [WP](workspaces.md#header-wp)

Our legal status (FIST stands for "fugitive/innocent status")

[X]

Variable [FLAG](workspaces.md#flag) in workspace [ZP](workspaces.md#header-zp)

A flag that's used to define whether this is the first call to the ball line routine in BLINE, so it knows whether to wait for the second call before storing segment data in the ball line heap

[X]

Subroutine [FMLTU](elite_c.md#header-fmltu) (category: Maths (Arithmetic))

Calculate A = A * Q / 256

[X]

Subroutine [FMLTU2](elite_c.md#header-fmltu2) (category: Maths (Arithmetic))

Calculate A = K * sin(A)

[X]

Variable [FRIN](workspaces.md#frin) in workspace [WP](workspaces.md#header-wp)

Slots for the ships in the local bubble of universe

[X]

Variable [FSH](workspaces.md#fsh) in workspace [ZP](workspaces.md#header-zp)

Forward shield status

[X]

Variable [GCNT](workspaces.md#gcnt) in workspace [WP](workspaces.md#header-wp)

The number of the current galaxy (0-7)

[X]

Subroutine [GINF](elite_c.md#header-ginf) (category: Universe)

Fetch the address of a ship's data block into INF

[X]

Configuration variable [GREEN](workspaces.md#green)

Four mode 1 pixels of colour 3, 1, 3, 1 (cyan/yellow)

[X]

Configuration variable [GREEN2](workspaces.md#green2)

Two mode 2 pixels of colour 2 (green)

[X]

Configuration variable [HER](workspaces.md#her)

Ship type for a rock hermit (asteroid)

[X]

Subroutine [HLOIN](elite_a.md#header-hloin) (category: Drawing lines)

Draw a horizontal line from (X1, Y1) to (X2, Y1)

[X]

Subroutine [HLOIN2](elite_b.md#header-hloin2) (category: Drawing lines)

Remove a line from the sun line heap and draw it on-screen

[X]

Variable [INF](workspaces.md#inf) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing the address of a ship's data block, so it can be copied to and from the internal workspace at INWK

[X]

Variable [INWK](workspaces.md#inwk) in workspace [ZP](workspaces.md#header-zp)

The zero-page internal workspace for the current ship data block

[X]

Configuration variable [JH](workspaces.md#jh)

Junk is defined as ending before the Cobra Mk III

[X]

Configuration variable [JL](workspaces.md#jl)

Junk is defined as starting from the escape pod

[X]

Variable [JSTK](elite_a.md#jstk) in workspace [UP](elite_a.md#header-up)

Keyboard or joystick configuration setting

[X]

Variable [JSTX](workspaces.md#jstx) in workspace [ZP](workspaces.md#header-zp)

Our current roll rate

[X]

Variable [JSTY](workspaces.md#jsty) in workspace [ZP](workspaces.md#header-zp)

Our current pitch rate

[X]

Variable [JUNK](workspaces.md#junk) in workspace [WP](workspaces.md#header-wp)

The amount of junk in the local bubble

[X]

Variable [K](workspaces.md#k) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Workspace [K%](workspaces.md#header-k-per-cent) (category: Workspaces)

Ship data blocks and ship line heaps

[X]

Variable [K2](workspaces.md#k2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K3](workspaces.md#k3) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K4](workspaces.md#k4) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [K6](workspaces.md#k6) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing coordinates during vector calculations

[X]

Variable [KL](workspaces.md#kl) in workspace [ZP](workspaces.md#header-zp)

The following bytes implement a key logger that enables Elite to scan for concurrent key presses of the primary flight keys, plus a secondary flight key

[X]

Variable [KY7](workspaces.md#ky7) in workspace [ZP](workspaces.md#header-zp)

"A" is being pressed (fire lasers)

[X]

Label [LABEL_1](elite_e.md#label_1) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Label [LL163](elite_e.md#ll163) in subroutine [SPS2](elite_e.md#header-sps2)

[X]

Subroutine [LL5](elite_g.md#header-ll5) (category: Maths (Arithmetic))

Calculate Q = SQRT(R Q)

[X]

Subroutine [LOIN](elite_a.md#header-loin) (category: Drawing lines)

Draw a one-segment line

[X]

Variable [LSO](workspaces.md#lso) in workspace [WP](workspaces.md#header-wp)

The ship line heap for the space station (see NWSPS) and the sun line heap (see SUN)

[X]

Variable [LSP](workspaces.md#lsp) in workspace [ZP](workspaces.md#header-zp)

The ball line heap pointer, which contains the number of the first free byte after the end of the LSX2 and LSY2 heaps

[X]

Variable [LSX](workspaces.md#lsx) in workspace [ZP](workspaces.md#header-zp)

LSX contains the status of the sun line heap at LSO

[X]

Variable [LSX2](workspaces.md#lsx2) in workspace [WP](workspaces.md#header-wp)

The ball line heap for storing x-coordinates

[X]

Variable [LSY2](workspaces.md#lsy2) in workspace [WP](workspaces.md#header-wp)

The ball line heap for storing y-coordinates

[X]

Variable [MANY](workspaces.md#many) in workspace [WP](workspaces.md#header-wp)

The number of ships of each type in the local bubble of universe

[X]

Variable [MJ](workspaces.md#mj) in workspace [WP](workspaces.md#header-wp)

Are we in witchspace (i.e. have we mis-jumped)?

[X]

Variable [MOS](workspaces.md#mos) in workspace [ZP](workspaces.md#header-zp)

Determines whether we are running on a Master Compact

[X]

Variable [MSAR](workspaces.md#msar) in workspace [WP](workspaces.md#header-wp)

The targeting state of our leftmost missile

[X]

Subroutine [MSBAR](elite_a.md#header-msbar) (category: Dashboard)

Draw a specific indicator in the dashboard's missile bar

[X]

Variable [MSTG](workspaces.md#mstg) in workspace [ZP](workspaces.md#header-zp)

The current missile lock target

[X]

Subroutine [MULTU](elite_c.md#header-multu) (category: Maths (Arithmetic))

Calculate (A P) = P * Q

[X]

Variable [NAME](workspaces.md#name) in workspace [WP](workspaces.md#header-wp)

The current commander name

[X]

Variable [NEWB](workspaces.md#newb) in workspace [ZP](workspaces.md#header-zp)

The ship's "new byte flags" (or NEWB flags)

[X]

Configuration variable [NI%](workspaces.md#ni-per-cent)

The number of bytes in each ship's data block (as stored in INWK and K%)

[X]

Variable [NOMSL](workspaces.md#nomsl) in workspace [WP](workspaces.md#header-wp)

The number of missiles we have fitted (0-4)

[X]

Configuration variable [NOSH](workspaces.md#nosh)

The maximum number of ships in our local bubble of universe

[X]

Variable [NOSTM](workspaces.md#nostm) in workspace [ZP](workspaces.md#header-zp)

The number of stardust particles shown on screen, which is 20 (#NOST) for normal space, and 3 for witchspace

[X]

Label [NW1](elite_e.md#nw1) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Label [NW2](elite_e.md#nw2) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Label [NW3](elite_e.md#nw3) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

[X]

Label [NW4](elite_e.md#nw4) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Label [NW6](elite_e.md#nw6) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Label [NW7](elite_e.md#nw7) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Label [NW8](elite_e.md#nw8) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Label [NWL1](elite_e.md#nwl1) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Label [NWL3](elite_e.md#nwl3) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Subroutine [NWSHP](elite_e.md#header-nwshp) (category: Universe)

Add a new ship to our local bubble of universe

[X]

Subroutine [NwS1](elite_e.md#header-nws1) (category: Universe)

Flip the sign and double an INWK byte

[X]

Label [OO1](elite_e.md#oo1) in subroutine [OOPS](elite_e.md#header-oops)

[X]

Label [OO2](elite_e.md#oo2) in subroutine [OOPS](elite_e.md#header-oops)

[X]

Label [OO3](elite_e.md#oo3) in subroutine [OOPS](elite_e.md#header-oops)

[X]

Label [OO5](elite_e.md#oo5) in subroutine [OOPS](elite_e.md#header-oops)

[X]

Subroutine [OUCH](elite_f.md#header-ouch) (category: Flight)

Potentially lose cargo or equipment following damage

[X]

Variable [P](workspaces.md#p) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [PIXEL](elite_a.md#header-pixel) (category: Drawing pixels)

Draw a one-pixel dot, two-pixel dash or four-pixel square

[X]

Subroutine [PIXEL2](elite_a.md#header-pixel2) (category: Drawing pixels)

Draw a stardust particle relative to the screen centre

[X]

Label [PL12](elite_e.md#pl12) in subroutine [PLS3](elite_e.md#header-pls3)

[X]

Subroutine [PL2](elite_e.md#header-pl2) (category: Drawing planets)

Remove the planet or sun from the screen

[X]

Entry point [PL2-1](elite_e.md#pl2) in subroutine [PL2](elite_e.md#header-pl2) (category: Drawing planets)

Contains an RTS

[X]

Label [PL20](elite_e.md#pl20) in subroutine [PL9 (Part 1 of 3)](elite_e.md#header-pl9-part-1-of-3)

[X]

Subroutine [PL21](elite_e.md#header-pl21) (category: Drawing planets)

Return from a planet/sun-drawing routine with a failure flag

[X]

Label [PL25](elite_e.md#pl25) in subroutine [PL9 (Part 1 of 3)](elite_e.md#header-pl9-part-1-of-3)

[X]

Label [PL26](elite_e.md#pl26) in subroutine [PL9 (Part 3 of 3)](elite_e.md#header-pl9-part-3-of-3)

[X]

Label [PL31](elite_e.md#pl31) in subroutine [CHKON](elite_e.md#header-chkon)

[X]

Label [PL37](elite_e.md#pl37) in subroutine [CIRCLE2](elite_e.md#header-circle2)

[X]

Label [PL38](elite_e.md#pl38) in subroutine [CIRCLE2](elite_e.md#header-circle2)

[X]

Label [PL40](elite_e.md#pl40) in subroutine [PLS22](elite_e.md#header-pls22)

[X]

Label [PL42](elite_e.md#pl42) in subroutine [PLS22](elite_e.md#header-pls22)

[X]

Label [PL43](elite_e.md#pl43) in subroutine [PLS22](elite_e.md#header-pls22)

[X]

Entry point [PL44](elite_e.md#pl44) in subroutine [PLS6](elite_e.md#header-pls6) (category: Drawing planets)

Clear the C flag and return from the subroutine

[X]

Entry point [PL6](elite_e.md#pl6) in subroutine [PLS6](elite_e.md#header-pls6) (category: Drawing planets)

Contains an RTS

[X]

Label [PL82](elite_e.md#pl82) in subroutine [PLANET](elite_e.md#header-planet)

[X]

Label [PL89](elite_e.md#pl89) in subroutine [CIRCLE](elite_e.md#header-circle)

[X]

Subroutine [PL9 (Part 1 of 3)](elite_e.md#header-pl9-part-1-of-3) (category: Drawing planets)

Draw the planet, with either an equator and meridian, or a crater

[X]

Label [PLF10](elite_e.md#plf10) in subroutine [SUN (Part 3 of 4)](elite_e.md#header-sun-part-3-of-4)

[X]

Label [PLF11](elite_e.md#plf11) in subroutine [SUN (Part 3 of 4)](elite_e.md#header-sun-part-3-of-4)

[X]

Label [PLF13](elite_e.md#plf13) in subroutine [SUN (Part 2 of 4)](elite_e.md#header-sun-part-2-of-4)

[X]

Label [PLF16](elite_e.md#plf16) in subroutine [SUN (Part 3 of 4)](elite_e.md#header-sun-part-3-of-4)

[X]

Label [PLF17](elite_e.md#plf17) in subroutine [SUN (Part 1 of 4)](elite_e.md#header-sun-part-1-of-4)

[X]

Label [PLF2](elite_e.md#plf2) in subroutine [SUN (Part 1 of 4)](elite_e.md#header-sun-part-1-of-4)

[X]

Label [PLF23](elite_e.md#plf23) in subroutine [SUN (Part 3 of 4)](elite_e.md#header-sun-part-3-of-4)

[X]

Label [PLF3](elite_e.md#plf3) in subroutine [SUN (Part 1 of 4)](elite_e.md#header-sun-part-1-of-4)

[X]

Label [PLF3M3](elite_e.md#plf3m3) in subroutine [SUN (Part 1 of 4)](elite_e.md#header-sun-part-1-of-4)

[X]

Label [PLF4](elite_e.md#plf4) in subroutine [SUN (Part 1 of 4)](elite_e.md#header-sun-part-1-of-4)

[X]

Label [PLF44](elite_e.md#plf44) in subroutine [SUN (Part 3 of 4)](elite_e.md#header-sun-part-3-of-4)

[X]

Label [PLF5](elite_e.md#plf5) in subroutine [SUN (Part 1 of 4)](elite_e.md#header-sun-part-1-of-4)

[X]

Label [PLF6](elite_e.md#plf6) in subroutine [SUN (Part 3 of 4)](elite_e.md#header-sun-part-3-of-4)

[X]

Label [PLF8](elite_e.md#plf8) in subroutine [SUN (Part 4 of 4)](elite_e.md#header-sun-part-4-of-4)

[X]

Label [PLF9](elite_e.md#plf9) in subroutine [SUN (Part 4 of 4)](elite_e.md#header-sun-part-4-of-4)

[X]

Label [PLFL](elite_e.md#plfl) in subroutine [SUN (Part 3 of 4)](elite_e.md#header-sun-part-3-of-4)

[X]

Label [PLFL2](elite_e.md#plfl2) in subroutine [SUN (Part 2 of 4)](elite_e.md#header-sun-part-2-of-4)

[X]

Label [PLFL3](elite_e.md#plfl3) in subroutine [SUN (Part 4 of 4)](elite_e.md#header-sun-part-4-of-4)

[X]

Label [PLFLS](elite_e.md#plfls) in subroutine [SUN (Part 3 of 4)](elite_e.md#header-sun-part-3-of-4)

[X]

Label [PLL3](elite_e.md#pll3) in subroutine [CIRCLE2](elite_e.md#header-circle2)

[X]

Label [PLL4](elite_e.md#pll4) in subroutine [PLS22](elite_e.md#header-pls22)

[X]

Subroutine [PLS1](elite_e.md#header-pls1) (category: Drawing planets)

Calculate (Y A) = nosev_x / z

[X]

Subroutine [PLS2](elite_e.md#header-pls2) (category: Drawing planets)

Draw a half-ellipse

[X]

Subroutine [PLS22](elite_e.md#header-pls22) (category: Drawing planets)

Draw an ellipse or half-ellipse

[X]

Subroutine [PLS3](elite_e.md#header-pls3) (category: Drawing planets)

Calculate (Y A P) = 222 * roofv_x / z

[X]

Subroutine [PLS4](elite_e.md#header-pls4) (category: Drawing planets)

Calculate CNT2 = arctan(P / A) / 4

[X]

Subroutine [PLS5](elite_e.md#header-pls5) (category: Drawing planets)

Calculate roofv_x / z and roofv_y / z

[X]

Subroutine [PLS6](elite_e.md#header-pls6) (category: Drawing planets)

Calculate (X K) = (A P+1 P) / (z_sign z_hi z_lo)

[X]

Subroutine [PROJ](elite_e.md#header-proj) (category: Maths (Geometry))

Project the current ship or planet onto the screen

[X]

Label [PTCLS](elite_e.md#ptcls) in subroutine [DOEXP](elite_e.md#header-doexp)

[X]

Variable [Q](workspaces.md#q) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [QQ11](workspaces.md#qq11) in workspace [ZP](workspaces.md#header-zp)

The type of the current view

[X]

Variable [QQ14](workspaces.md#qq14) in workspace [WP](workspaces.md#header-wp)

Our current fuel level (0-70)

[X]

Variable [QQ15](workspaces.md#qq15) in workspace [ZP](workspaces.md#header-zp)

The three 16-bit seeds for the selected system, i.e. the one in the crosshairs in the Short-range Chart

[X]

Variable [QQ16](elite_a.md#header-qq16) (category: Text)

The two-letter token lookup table

[X]

Variable [QQ17](workspaces.md#qq17) in workspace [ZP](workspaces.md#header-zp)

Contains a number of flags that affect how text tokens are printed, particularly capitalisation

[X]

Configuration variable [QQ18](workspaces.md#qq18)

The address of the text token table, as set in elite-data.asm

[X]

Variable [QQ19](workspaces.md#qq19) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [QQ2](workspaces.md#qq2) in workspace [WP](workspaces.md#header-wp)

The three 16-bit seeds for the current system, i.e. the one we are currently in

[X]

Variable [QQ20](workspaces.md#qq20) in workspace [WP](workspaces.md#header-wp)

The contents of our cargo hold

[X]

Label [QUL4](elite_e.md#qul4) in subroutine [cmn](elite_e.md#header-cmn)

[X]

Variable [R](workspaces.md#r) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [RAND](workspaces.md#rand) in workspace [ZP](workspaces.md#header-zp)

Four 8-bit seeds for the random number generation system implemented in the DORND routine

[X]

Configuration variable [RE](workspaces.md#re)

The obfuscation byte used to hide the recursive tokens table from crackers viewing the binary code

[X]

Configuration variable [RED](workspaces.md#red)

Four mode 1 pixels of colour 2 (red, magenta or white)

[X]

Entry point [RTS2](elite_e.md#rts2) in subroutine [SUN (Part 4 of 4)](elite_e.md#header-sun-part-4-of-4) (category: Drawing suns)

Contains an RTS

[X]

Variable [S](workspaces.md#s) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Label [SAL4](elite_e.md#sal4) in subroutine [nWq](elite_e.md#header-nwq)

[X]

Label [SAL6](elite_e.md#sal6) in subroutine [FLFLLS](elite_e.md#header-flflls)

[X]

Subroutine [SCAN](elite_a.md#header-scan) (category: Dashboard)

Display the current ship on the scanner

[X]

Subroutine [SHD](elite_e.md#header-shd) (category: Flight)

Charge a shield and drain some energy from the energy banks

[X]

[X]

Variable [SLSP](workspaces.md#slsp) in workspace [WP](workspaces.md#header-wp)

The address of the bottom of the ship line heap

[X]

Configuration variable [SNE](workspaces.md#sne)

The address of the sine lookup table, as set in elite-data.asm

[X]

Subroutine [SOS1](elite_e.md#header-sos1) (category: Universe)

Update the missile indicators, set up the planet data block

[X]

Subroutine [SP1](elite_e.md#header-sp1) (category: Dashboard)

Draw the space station on the compass

[X]

Subroutine [SP2](elite_e.md#header-sp2) (category: Dashboard)

Draw a dot on the compass, given the planet/station vector

[X]

Subroutine [SPBLB](elite_a.md#header-spblb) (category: Dashboard)

Light up the space station indicator ("S") on the dashboard

[X]

Label [SPL1](elite_e.md#spl1) in subroutine [SPS4](elite_e.md#header-sps4)

[X]

Subroutine [SPS1](elite_f.md#header-sps1) (category: Maths (Geometry))

Calculate the vector to the planet and store it in XX15

[X]

Subroutine [SPS2](elite_e.md#header-sps2) (category: Maths (Arithmetic))

Calculate (Y X) = A / 10

[X]

Subroutine [SPS4](elite_e.md#header-sps4) (category: Maths (Geometry))

Calculate the vector to the space station

[X]

Subroutine [SQUA2](elite_c.md#header-squa2) (category: Maths (Arithmetic))

Calculate (A P) = A * A

[X]

Variable [SSPR](workspaces.md#sspr) in workspace [WP](workspaces.md#header-wp)

"Space station present" flag

[X]

Configuration variable [SST](workspaces.md#sst)

Ship type for a Coriolis space station

[X]

Variable [STP](workspaces.md#stp) in workspace [ZP](workspaces.md#header-zp)

The step size for drawing circles

[X]

Subroutine [SUN (Part 1 of 4)](elite_e.md#header-sun-part-1-of-4) (category: Drawing suns)

Draw the sun: Set up all the variables needed to draw the sun

[X]

Variable [SUNX](workspaces.md#sunx) in workspace [ZP](workspaces.md#header-zp)

The 16-bit x-coordinate of the vertical centre axis of the sun (which might be off-screen)

[X]

Variable [SWAP](workspaces.md#swap) in workspace [WP](workspaces.md#header-wp)

Temporary storage, used to store a flag that records whether or not we had to swap a line's start and end coordinates around when clipping the line in routine LL145 (the flag is used in places like BLINE to swap them back)

[X]

Label [SWPZL](elite_e.md#swpzl) in subroutine [SWAPPZERO](elite_e.md#header-swappzero)

[X]

Variable [SX](workspaces.md#sx) in workspace [WP](workspaces.md#header-wp)

This is where we store the x_hi coordinates for all the stardust particles

[X]

Variable [SY](workspaces.md#sy) in workspace [WP](workspaces.md#header-wp)

This is where we store the y_hi coordinates for all the stardust particles

[X]

Variable [SZ](workspaces.md#sz) in workspace [WP](workspaces.md#header-wp)

This is where we store the z_hi coordinates for all the stardust particles

[X]

Variable [T](workspaces.md#t) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [T1](workspaces.md#t1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Subroutine [TAS2](elite_f.md#header-tas2) (category: Maths (Geometry))

Normalise the three-coordinate vector in K3

[X]

Variable [TGT](workspaces.md#tgt) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used as a target value for counters when drawing explosion clouds and partial circles

[X]

Entry point [TJ1](elite_e.md#tj1) in subroutine [TT17](elite_e.md#header-tt17) (category: Keyboard)

Check for cursor key presses and return the combined deltas for the digital joystick and cursor keys (Master Compact only)

[X]

Label [TJS1](elite_e.md#tjs1) in subroutine [TT17](elite_e.md#header-tt17)

[X]

Variable [TRIBBLE](workspaces.md#tribble) in workspace [WP](workspaces.md#header-wp)

The number of Trumbles in the cargo hold

[X]

Label [TT17afterall](elite_e.md#tt17afterall) in subroutine [TT17](elite_e.md#header-tt17)

[X]

Subroutine [TT26](elite_b.md#header-tt26) (category: Text)

Print a character at the text cursor, with support for verified text in extended tokens

[X]

Subroutine [TT27](elite_e.md#header-tt27) (category: Text)

Print a text token

[X]

Subroutine [TT41](elite_e.md#header-tt41) (category: Text)

Print a letter according to Sentence Case

[X]

Subroutine [TT42](elite_e.md#header-tt42) (category: Text)

Print a letter in lower case

[X]

Subroutine [TT43](elite_e.md#header-tt43) (category: Text)

Print a two-letter token or recursive token 0-95

[X]

Entry point [TT44](elite_e.md#tt44) in subroutine [TT42](elite_e.md#header-tt42) (category: Text)

Jumps to TT26 to print the character in A (used to enable us to use a branch instruction to jump to TT26)

[X]

Subroutine [TT45](elite_e.md#header-tt45) (category: Text)

Print a letter in lower case

[X]

Subroutine [TT46](elite_e.md#header-tt46) (category: Text)

Print a character and switch to capitals

[X]

Label [TT47](elite_e.md#tt47) in subroutine [TT43](elite_e.md#header-tt43)

[X]

Entry point [TT48](elite_e.md#tt48) in subroutine [ex](elite_e.md#header-ex) (category: Text)

Contains an RTS

[X]

Label [TT49](elite_e.md#tt49) in subroutine [ex](elite_e.md#header-ex)

[X]

Label [TT50](elite_e.md#tt50) in subroutine [ex](elite_e.md#header-ex)

[X]

Label [TT51](elite_e.md#tt51) in subroutine [ex](elite_e.md#header-ex)

[X]

Label [TT53](elite_e.md#tt53) in subroutine [cpl](elite_e.md#header-cpl)

[X]

Subroutine [TT54](elite_d.md#header-tt54) (category: Universe)

Twist the selected system's seeds

[X]

Label [TT55](elite_e.md#tt55) in subroutine [cpl](elite_e.md#header-cpl)

[X]

Label [TT56](elite_e.md#tt56) in subroutine [cpl](elite_e.md#header-cpl)

[X]

Label [TT59](elite_e.md#tt59) in subroutine [ex](elite_e.md#header-ex)

[X]

Label [TT62](elite_e.md#tt62) in subroutine [ypl](elite_e.md#header-ypl)

[X]

Subroutine [TT67](elite_d.md#header-tt67) (category: Text)

Print a newline

[X]

Subroutine [TT68](elite_e.md#header-tt68) (category: Text)

Print a text token followed by a colon

[X]

Subroutine [TT73](elite_e.md#header-tt73) (category: Text)

Print a colon

[X]

Subroutine [TT74](elite_e.md#header-tt74) (category: Text)

Print a character

[X]

Label [TT78](elite_e.md#tt78) in subroutine [ypl](elite_e.md#header-ypl)

[X]

Variable [TYPE](workspaces.md#type) in workspace [ZP](workspaces.md#header-zp)

The current ship type

[X]

Variable [U](workspaces.md#u) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used in a number of places

[X]

Variable [V](workspaces.md#v) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing an address pointer

[X]

Configuration variable [VIA](workspaces.md#via)

Memory-mapped space for accessing internal hardware, such as the video ULA, 6845 CRTC and 6522 VIAs (also known as SHEILA)

[X]

Subroutine [WP1](elite_e.md#header-wp1) (category: Drawing planets)

Reset the ball line heap

[X]

Label [WP2](elite_e.md#wp2) in subroutine [WPLS2](elite_e.md#header-wpls2)

[X]

Label [WPL1](elite_e.md#wpl1) in subroutine [WPLS2](elite_e.md#header-wpls2)

[X]

Label [WPL2](elite_e.md#wpl2) in subroutine [WPLS](elite_e.md#header-wpls)

[X]

Subroutine [WPLS](elite_e.md#header-wpls) (category: Drawing suns)

Remove the sun from the screen

[X]

Entry point [WPLS-1](elite_e.md#wpls) in subroutine [WPLS](elite_e.md#header-wpls) (category: Drawing suns)

Contains an RTS

[X]

Subroutine [WPLS2](elite_e.md#header-wpls2) (category: Drawing planets)

Remove the planet from the screen

[X]

Subroutine [WPSHPS](elite_e.md#header-wpshps) (category: Dashboard)

Clear the scanner, reset the ball line and sun line heaps

[X]

Label [WS1](elite_e.md#ws1) in subroutine [WPSHPS](elite_e.md#header-wpshps)

[X]

Label [WS2](elite_e.md#ws2) in subroutine [WPSHPS](elite_e.md#header-wpshps)

[X]

Label [WSL1](elite_e.md#wsl1) in subroutine [WPSHPS](elite_e.md#header-wpshps)

[X]

Label [WSL2](elite_e.md#wsl2) in subroutine [WPSHPS](elite_e.md#header-wpshps)

[X]

Configuration variable [X](workspaces.md#x)

The centre x-coordinate of the 256 x 192 space view

[X]

Variable [X1](workspaces.md#x1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [X2](workspaces.md#x2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for x-coordinates in the line-drawing routines

[X]

Variable [XSAV](workspaces.md#xsav) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for saving the value of the X register, used in a number of places

[X]

Variable [XX](workspaces.md#xx) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing a 16-bit x-coordinate

[X]

Variable [XX0](workspaces.md#xx0) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, used to store the address of a ship blueprint. For example, it is used when we add a new ship to the local bubble in routine NWSHP, and it contains the address of the current ship's blueprint as we loop through all the nearby ships in the main flight loop

[X]

Variable [XX15](workspaces.md#xx15) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing screen coordinates in line-drawing routines

[X]

Variable [XX16](workspaces.md#xx16) in workspace [ZP](workspaces.md#header-zp)

Temporary storage for a block of values, used in a number of places

[X]

Variable [XX19](workspaces.md#xx19) in workspace [ZP](workspaces.md#header-zp)

XX19(1 0) shares its location with INWK(34 33), which contains the address of the ship line heap

[X]

Configuration variable [XX21](workspaces.md#xx21)

The address of the ship blueprints lookup table, as set in elite-data.asm

[X]

Workspace [XX3](workspaces.md#header-xx3) (category: Workspaces)

Temporary storage space for complex calculations

[X]

Configuration variable [Y](workspaces.md#y)

The centre y-coordinate of the 256 x 192 space view

[X]

Variable [Y1](workspaces.md#y1) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Variable [Y2](workspaces.md#y2) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for y-coordinates in line-drawing routines

[X]

Configuration variable [YELLOW](workspaces.md#yellow)

Four mode 1 pixels of colour 1 (yellow)

[X]

Configuration variable [YELLOW2](workspaces.md#yellow2)

Two mode 2 pixels of colour 3 (yellow)

[X]

Subroutine [YESNO](elite_e.md#header-yesno) (category: Keyboard)

Wait until either "Y" or "N" is pressed

[X]

Variable [YY](workspaces.md#yy) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for storing a 16-bit y-coordinate

[X]

Variable [Yx2M1](workspaces.md#yx2m1) in workspace [ZP](workspaces.md#header-zp)

This is used to store the number of pixel rows in the space view minus 1, which is also the y-coordinate of the bottom pixel row of the space view (it is set to 191 in the RES2 routine)

[X]

Subroutine [ZINF](elite_f.md#header-zinf) (category: Universe)

Reset the INWK workspace and orientation vectors

[X]

Workspace [ZP](workspaces.md#header-zp) (category: Workspaces)

Lots of important variables are stored in the zero page workspace as it is quicker and more space-efficient to access memory here

[X]

Variable [ZZ](workspaces.md#zz) in workspace [ZP](workspaces.md#header-zp)

Temporary storage, typically used for distance values

[X]

Subroutine [cmn](elite_e.md#header-cmn) (category: Status)

Print the commander's name

[X]

Variable [coltabl](elite_e.md#header-coltabl) (category: Drawing ships)

Colours for ship explosions

[X]

Subroutine [cpl](elite_e.md#header-cpl) (category: Universe)

Print the selected system name

[X]

Subroutine [crlf](elite_e.md#header-crlf) (category: Text)

Tab to column 21 and print a colon

[X]

Subroutine [csh](elite_e.md#header-csh) (category: Status)

Print the current amount of cash

[X]

Subroutine [ex](elite_e.md#header-ex) (category: Text)

Print a recursive token

[X]

Variable [frump](workspaces.md#frump) in workspace [WP](workspaces.md#header-wp)

Used to store the number of particles in the explosion cloud, though the number is never actually used

[X]

Subroutine [fwl](elite_e.md#header-fwl) (category: Status)

Print fuel and cash levels

[X]

Label [gangbang](elite_e.md#gangbang) in subroutine [NWSHP](elite_e.md#header-nwshp)

[X]

Subroutine [msblob](elite_f.md#header-msblob) (category: Dashboard)

Display the dashboard's missile indicators in green

[X]

Label [nobirths](elite_e.md#nobirths) in subroutine [SOLAR](elite_e.md#header-solar)

[X]

Label [notadodo](elite_e.md#notadodo) in subroutine [NWSPS](elite_e.md#header-nwsps)

[X]

Label [paen2](elite_e.md#paen2) in subroutine [DENGY](elite_e.md#header-dengy)

[X]

Label [pc1](elite_e.md#pc1) in subroutine [csh](elite_e.md#header-csh)

[X]

Subroutine [plf](elite_e.md#header-plf) (category: Text)

Print a text token followed by a newline

[X]

Subroutine [pr2](elite_b.md#header-pr2) (category: Text)

Print an 8-bit number, left-padded to 3 digits, and optional point

[X]

Subroutine [qw](elite_e.md#header-qw) (category: Text)

Print a recursive token in the range 128-145

[X]

Variable [spasto](elite_f.md#header-spasto) (category: Universe)

Contains the address of the Coriolis space station's ship blueprint

[X]

Label [speedup](elite_e.md#speedup) in subroutine [TT17](elite_e.md#header-tt17)

[X]

Entry point [t](elite_f.md#t) in subroutine [TT217](elite_f.md#header-tt217) (category: Keyboard)

As TT217 but don't preserve Y, set it to YSAV instead

[X]

Subroutine [tal](elite_e.md#header-tal) (category: Universe)

Print the current galaxy number

[X]

Variable [tek](workspaces.md#tek) in workspace [WP](workspaces.md#header-wp)

The current system's tech level (0-14)

[X]

Subroutine [ypl](elite_e.md#header-ypl) (category: Universe)

Print the current system name

[X]

Entry point [ypl-1](elite_e.md#ypl) in subroutine [ypl](elite_e.md#header-ypl) (category: Universe)

Contains an RTS

[X]

Label [ypl16](elite_e.md#ypl16) in subroutine [ypl](elite_e.md#header-ypl)

[X]

Label [yy](elite_e.md#yy) in subroutine [DOEXP](elite_e.md#header-doexp)

[Elite D source](elite_d.md "Previous routine")[Elite F source](elite_f.md "Next routine")
