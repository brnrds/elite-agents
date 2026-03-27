---
title: "The ZP (Loader) workspace - Elite on the 6502"
source: "https://elite.bbcelite.com/master/loader/workspace/zp.html"
---

[B% (Loader)](../variable/b_per_cent.md "Next routine")
    
    
           Name: ZP                                                      [Show more]
           Type: Workspace
        Address: &0070 to &0075
       Category: Workspaces
        Summary: Important variables used by the loader
    
    
        Context: See this workspace [in context in the source code](../../all/loader.md#header-zp)
     Variations: See [code variations](../../related/compare/main/workspace/zp.md) for this workspace in the different versions
     References: This workspace is used as follows:
                 * [Elite loader](../subroutine/elite_loader.md) uses ZP
                 * [PIX](../subroutine/pix.md) uses ZP
                 * [PLL1 (Part 1 of 3)](../subroutine/pll1_part_1_of_3.md) uses ZP
                 * [PLL1 (Part 2 of 3)](../subroutine/pll1_part_2_of_3.md) uses ZP
                 * [PLL1 (Part 3 of 3)](../subroutine/pll1_part_3_of_3.md) uses ZP
                 * [ROOT](../subroutine/root.md) uses ZP
    
    
    
    
    
    
     ORG &0002              \ Set the assembly address to &0002
    
    IF _COMPACT
    
    .MOS
    
     SKIP 1                 \ Determines whether we are running on a Master Compact
                            \
                            \   * 0 = This is a Master Compact
                            \
                            \   * &FF = This is not a Master Compact
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Elite loader](../subroutine/elite_loader.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    ENDIF
    
     ORG &0070              \ Set the assembly address to &0070
    
    .ZP
    
     SKIP 2                 \ Stores addresses used for moving content around
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Elite loader](../subroutine/elite_loader.md)
                            \   * [PIX](../subroutine/pix.md)
                            \   * [PLL1 (Part 1 of 3)](../subroutine/pll1_part_1_of_3.md)
                            \   * [PLL1 (Part 2 of 3)](../subroutine/pll1_part_2_of_3.md)
                            \   * [PLL1 (Part 3 of 3)](../subroutine/pll1_part_3_of_3.md)
                            \   * [ROOT](../subroutine/root.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .P
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Elite loader](../subroutine/elite_loader.md)
                            \   * [PLL1 (Part 1 of 3)](../subroutine/pll1_part_1_of_3.md)
                            \   * [ROOT](../subroutine/root.md)
                            \   * [SQUA2](../subroutine/squa2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .Q
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [ROOT](../subroutine/root.md)
                            \   * [SQUA2](../subroutine/squa2.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .YY
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [PLL1 (Part 1 of 3)](../subroutine/pll1_part_1_of_3.md)
                            \   * [PLL1 (Part 2 of 3)](../subroutine/pll1_part_2_of_3.md)
                            \   * [PLL1 (Part 3 of 3)](../subroutine/pll1_part_3_of_3.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
    .T
    
     SKIP 1                 \ Temporary storage, used in a number of places
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [PLL1 (Part 3 of 3)](../subroutine/pll1_part_3_of_3.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    
    
    
     ORG &00F4              \ Set the assembly address to &00F4
    
    .LATCH
    
     SKIP 2                 \ The RAM copy of the currently selected paged ROM/RAM
                            \ in SHEILA &30
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [Elite loader](../subroutine/elite_loader.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    

[B% (Loader)](../variable/b_per_cent.md "Next routine")
