---
title: "The XX3 workspace - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/workspace/xx3.html"
---

[ZP](zp.md "Previous routine")[K%](k_per_cent.md "Next routine")
    
    
           Name: XX3                                                     [Show more]
           Type: Workspace
        Address: &0100 to the top of the descending stack
       Category: Workspaces
        Summary: Temporary storage space for complex calculations
    
    
        Context: See this workspace [in context in the source code](../../all/workspaces.md#header-xx3)
     References: This workspace is used as follows:
                 * [DOEXP](../subroutine/doexp.md) uses XX3
                 * [LL62](../subroutine/ll62.md) uses XX3
                 * [LL9 (Part 2 of 12)](../subroutine/ll9_part_2_of_12.md) uses XX3
                 * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md) uses XX3
                 * [LL9 (Part 9 of 12)](../subroutine/ll9_part_9_of_12.md) uses XX3
                 * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md) uses XX3
                 * [SFS1](../subroutine/sfs1.md) uses XX3
    
    
    
    
    
    * * *
    
    
     Used as heap space for storing temporary data during calculations. Shared with
     the descending 6502 stack, which works down from &01FF.
    
    
    
    
     ORG &0100              \ Set the assembly address to &0100
    
    .XX3
    
     SKIP 256               \ Temporary storage, typically used for storing tables
                            \ of values such as screen coordinates or ship data
                            \
                            \ [Show more]
    
    
                            \
                            \ This variable is used by the following:
                            \
                            \   * [DOEXP](../subroutine/doexp.md)
                            \   * [LL62](../subroutine/ll62.md)
                            \   * [LL9 (Part 2 of 12)](../subroutine/ll9_part_2_of_12.md)
                            \   * [LL9 (Part 8 of 12)](../subroutine/ll9_part_8_of_12.md)
                            \   * [LL9 (Part 9 of 12)](../subroutine/ll9_part_9_of_12.md)
                            \   * [LL9 (Part 10 of 12)](../subroutine/ll9_part_10_of_12.md)
                            \   * [SFS1](../subroutine/sfs1.md)
                            \
                            \ This list only includes code that refers to the
                            \ variable by name; there may be other references to
                            \ this memory location that don't use this label, and
                            \ these will not be mentioned above
    

[ZP](zp.md "Previous routine")[K%](k_per_cent.md "Next routine")
