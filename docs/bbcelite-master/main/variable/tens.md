---
title: "The TENS variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/variable/tens.html"
---

[MVS5](../subroutine/mvs5.md "Previous routine")[pr2](../subroutine/pr2.md "Next routine")
    
    
           Name: TENS                                                    [Show more]
           Type: Variable
       Category: Text
        Summary: A constant used when printing large numbers in BPRNT
      Deep dive: [Printing decimal numbers](https://elite.bbcelite.com/deep_dives/printing_decimal_numbers.html)
    
    
        Context: See this variable [in context in the source code](../../all/elite_b.md#header-tens)
     References: This variable is used as follows:
                 * [BPRNT](../subroutine/bprnt.md) uses TENS
    
    
    
    
    
    * * *
    
    
     Contains the four low bytes of the value 100,000,000,000 (100 billion).
    
     The maximum number of digits that we can print with the BPRNT routine is 11,
     so the biggest number we can print is 99,999,999,999. This maximum number
     plus 1 is 100,000,000,000, which in hexadecimal is:
    
       17 48 76 E8 00
    
     The TENS variable contains the lowest four bytes in this number, with the
     most significant byte first, i.e. 48 76 E8 00. This value is used in the
     BPRNT routine when working out which decimal digits to print when printing a
     number.
    
    
    
    
    .TENS
    
     EQUD &00E87648
    

[MVS5](../subroutine/mvs5.md "Previous routine")[pr2](../subroutine/pr2.md "Next routine")
