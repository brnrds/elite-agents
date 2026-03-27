---
title: "Different variants of the BBC Master version of Elite - Elite on the 6502"
source: "https://elite.bbcelite.com/master/releases.html"
---

[Information on the BBC Master version](index.md "Previous routine")[Map of the source code](articles/map_of_the_source_code.md "Next page")

This site contains the source code for two different variants of the BBC Master version of Elite:

  * The variant on the official Acornsoft SNG47 release, which was the first appearance of BBC Master Elite, and the variant included on all subsequent discs (such as the Superior Software discs)
  * The variant that was released for the Master Compact by Superior Software



See below for comprehensive details of the differences between the variants, along with links to the relevant bits of the source code.

All these differences are implemented within the source code using BeebAsm IF statements, which determine which variant is compiled. These IF statements check the values of the relevant source code variables (_SNG47 and _COMPACT), which are themselves set by parameters to the build command (such as variant=compact). See the [associated repository](https://github.com/markmoxon/elite-source-code-bbc-master) for more about building different variants from the source.

To play Elite with an SSD disc image, load the disc image into drive 0 and press SHIFT-BREAK.

## The official Acornsoft SNG47 release  
\------------------------------------

The Acornsoft SNG47 release was the first official release of BBC Master Elite.

Default build in repository| Yes  
---|---  
Product details| Acornsoft SNG47  
Date| 1986  
Build command parameter| variant=sng47 (optional)  
Source code variable| _SNG47  
Verification checksums (crc32)| See the [GitHub repository](https://github.com/markmoxon/elite-source-code-bbc-master#building-the-sng47-variant)  
Download SSD disc image| [Original](https://elite.bbcelite.com/versions/elite/elite-master-sng47.ssd), [Flicker-free](https://elite.bbcelite.com/versions/flicker_free_elite/elite-master-flicker-free-sng47.ssd)  
  
## The Master Compact release  
\--------------------------

The Superior Software variant for the Master Compact is essentially BBC Master Elite, just tweaked for the Compact's digital joystick and ADFS disc drive.

Default build in repository| No  
---|---  
Product details| Superior Software  
Date| 1987  
Build command parameter| variant=compact  
Source code variable| _COMPACT  
Verification checksums (crc32)| See the [GitHub repository](https://github.com/markmoxon/elite-source-code-bbc-master#building-the-master-compact-variant)  
Download SSD disc image (for DFS)| [Original](https://elite.bbcelite.com/versions/elite/elite-master-compact.ssd), [Flicker-free](https://elite.bbcelite.com/versions/flicker_free_elite/elite-master-flicker-free-compact.ssd)  
Download ADL disc image (for ADFS)| [Original](https://elite.bbcelite.com/versions/elite/elite-master-compact.adl), [Flicker-free](https://elite.bbcelite.com/versions/flicker_free_elite/elite-master-flicker-free-compact.adl)  
  
It has the following features that differentiate it from the other variants:

  * Support for the Compact's digital joystick (see [DOKEY](main/subroutine/dokey.md), [DJOY](main/subroutine/djoy.md), [IRQ1](main/subroutine/irq1.md), [RDJOY](main/subroutine/rdjoy.md), [RDFIRE](main/subroutine/rdfire.md), [TITLE](main/subroutine/title.md), [TT17](main/subroutine/tt17.md), [TT17X](main/subroutine/tt17x.md))
  * Support for ADFS instead of DFS, including asking for a directory rather than a drive number (see [CATS](main/subroutine/cats.md), [CTLI](main/variable/ctli.md), [DELI](main/variable/deli.md), [DELT](main/subroutine/delt.md), [DIRI](main/variable/diri.md), [GTDIR](main/subroutine/gtdir.md), [lodosc](main/variable/lodosc.md), [rfile](main/subroutine/rfile.md), [savosc](main/variable/savosc.md), [TKN1](game_data/variable/tkn1.md), [wfile](main/subroutine/wfile.md))
  * The contents of the ADFS disc catalogue wrap on-screen, rather than having a column removed from the middle, like the DFS catalogue (see [CHPR](main/subroutine/chpr.md))
  * SHIFT-n characters (where n is a numeric key) are valid in commander filenames, directory names, filenames for deletion, and system search (see [MT26](main/subroutine/mt26.md))
  * The NMI workspace needs to be claimed and released for ADFS (see [NMI](main/workspace/wp.md#nmi), [NMICLAIM](main/subroutine/nmiclaim.md), [NMIRELEASE](main/subroutine/nmirelease.md))
  * The zero page swap workspace is different for ADFS than for DFS (see [getzp](main/subroutine/getzp.md), [setzp](main/subroutine/setzp.md), [SWAPPZERO](main/subroutine/swappzero.md))
  * The addresses of numerous workspace variables are different (see [WP](main/workspace/wp.md), [ZP](main/workspace/zp.md), [DOEXP](main/subroutine/doexp.md))
  * There is code to check whether this is a Master Compact (see [Elite loader](loader/subroutine/elite_loader.md), [MOS](main/workspace/zp.md#mos))
  * The Compact has a different keyboard to the Master, so the keyboard routines are different (see [CTRLmc](main/subroutine/ctrlmc.md), [DKS4mc](main/subroutine/dks4mc.md), [hyp](main/subroutine/hyp.md), [RETURN](main/subroutine/return.md), [SHIFT](main/subroutine/shift.md), [TT18](main/subroutine/tt18.md))
  * The main game binary is called ELITE rather than BCODE (see [MESS2](loader/variable/mess2.md))
  * There are some other minor code differences, such as different label names or rolled-up routines ([ECMOF](main/subroutine/ecmof.md), [MSBAR](main/subroutine/msbar.md), [HANGER](main/subroutine/hanger.md), [SFRMIS](main/subroutine/sfrmis.md), [TTX66](main/subroutine/ttx66.md))



[Information on the BBC Master version](index.md "Previous routine")[Map of the source code](articles/map_of_the_source_code.md "Next page")
