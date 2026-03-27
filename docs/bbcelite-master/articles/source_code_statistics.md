---
title: "Source code statistics - Elite on the 6502"
source: "https://elite.bbcelite.com/master/articles/source_code_statistics.html"
---

[Statistics for the Apple II version](https://elite.bbcelite.com/apple/articles/source_code_statistics.html "Previous routine")[Statistics for the NES version](https://elite.bbcelite.com/nes/articles/source_code_statistics.html "Next routine")

Here's a statistical breakdown of the source code for the BBC Master version of Elite. Click on the table headers to sort by that statistic. For more information, see the notes after the table.

Category| Instructions| Subroutines| Variables| Data (bytes)  
---|---|---|---|---  
Charts| 284  (2.3%)| 9  (1.8%)| 0  (0.0%)| 0  (0.0%)  
Dashboard| 474  (3.8%)| 25  (5.0%)| 2  (0.5%)| 38  (0.2%)  
Drawing circles| 263  (2.1%)| 9  (1.8%)| 0  (0.0%)| 0  (0.0%)  
Drawing lines| 1349 (10.9%)| 27  (5.4%)| 5  (1.4%)| 38  (0.2%)  
Drawing pixels| 120  (1.0%)| 4  (0.8%)| 9  (2.5%)| 298  (1.6%)  
Drawing planets| 394  (3.2%)| 18  (3.6%)| 4  (1.1%)| 10  (0.1%)  
Drawing ships| 937  (7.6%)| 14  (2.8%)| 39 (10.6%)| 8154 (43.7%)  
Drawing suns| 176  (1.4%)| 6  (1.2%)| 0  (0.0%)| 0  (0.0%)  
Drawing the screen| 187  (1.5%)| 10  (2.0%)| 7  (1.9%)| 663  (3.6%)  
Equipment| 229  (1.9%)| 5  (1.0%)| 1  (0.3%)| 28  (0.2%)  
Flight| 658  (5.3%)| 29  (5.8%)| 0  (0.0%)| 0  (0.0%)  
Keyboard| 526  (4.3%)| 23  (4.6%)| 4  (1.1%)| 178  (1.0%)  
Loader| 145  (1.2%)| 5  (1.0%)| 4  (1.1%)| 87  (0.5%)  
Main loop| 689  (5.6%)| 22  (4.4%)| 0  (0.0%)| 1  (0.0%)  
Market| 361  (2.9%)| 17  (3.4%)| 1  (0.3%)| 68  (0.4%)  
Maths (Arithmetic)| 946  (7.7%)| 54 (10.8%)| 3  (0.8%)| 768  (4.1%)  
Maths (Geometry)| 348  (2.8%)| 20  (4.0%)| 2  (0.5%)| 96  (0.5%)  
Missions| 104  (0.8%)| 11  (2.2%)| 5  (1.4%)| 12  (0.1%)  
Moving| 545  (4.4%)| 16  (3.2%)| 0  (0.0%)| 0  (0.0%)  
Save and load| 290  (2.4%)| 17  (3.4%)| 14  (3.8%)| 421  (2.3%)  
Ship hangar| 219  (1.8%)| 6  (1.2%)| 2  (0.5%)| 37  (0.2%)  
Sound| 123  (1.0%)| 11  (2.2%)| 6  (1.6%)| 78  (0.4%)  
Stardust| 384  (3.1%)| 7  (1.4%)| 0  (0.0%)| 0  (0.0%)  
Start and end| 248  (2.0%)| 9  (1.8%)| 0  (0.0%)| 0  (0.0%)  
Status| 154  (1.2%)| 7  (1.4%)| 2  (0.5%)| 66  (0.4%)  
Tactics| 431  (3.5%)| 14  (2.8%)| 0  (0.0%)| 0  (0.0%)  
Text| 765  (6.2%)| 58 (11.6%)| 19  (5.2%)| 5499 (29.5%)  
Universe| 860  (7.0%)| 32  (6.4%)| 2  (0.5%)| 29  (0.2%)  
Utility routines| 117  (0.9%)| 15  (3.0%)| 2  (0.5%)| 23  (0.1%)  
Workspaces| 0  (0.0%)| 0  (0.0%)| 234 (63.8%)| 2064 (11.1%)  
Totals| 12326| 500| 367| 18656  
  
Some notes on the above:

  * The instruction count does not include EQUB, EQUW, EQUD, EQUS or SKIP operatives; these are counted as data even when they are buried in code (so EQUB &2C "BIT skip" instructions are counted as data, for example).
  * INCBIN files are not included in the counts, so the data count doesn't include bytes from binary source files.
  * Each part of a multi-part subroutine counts as an individual subroutine.
  * The statistics are produced by a relatively simple static analysis of the source code. They are not 100% accurate, though they are pretty close.
  * The totals cover all code in the project, including loaders, docked and flight code, Tube code and ship data files.



[Statistics for the Apple II version](https://elite.bbcelite.com/apple/articles/source_code_statistics.html "Previous routine")[Statistics for the NES version](https://elite.bbcelite.com/nes/articles/source_code_statistics.html "Next routine")
