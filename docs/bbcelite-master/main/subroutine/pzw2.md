---
title: "The PZW2 subroutine - Elite on the 6502"
source: "https://elite.bbcelite.com/master/main/subroutine/pzw2.html"
---

[DIALS (Part 4 of 4)](dials_part_4_of_4.md "Previous routine")[PZW](pzw.md "Next routine")
    
    
           Name: PZW2                                                    [Show more]
           Type: Subroutine
       Category: Dashboard
        Summary: Fetch the current dashboard colours for non-striped indicators, to
                 support flashing
    
    
        Context: See this subroutine [in context in the source code](../../all/elite_a.md#header-pzw2)
     References: This subroutine is called as follows:
                 * [DIALS (Part 1 of 4)](dials_part_1_of_4.md) calls PZW2
                 * [DIALS (Part 4 of 4)](dials_part_4_of_4.md) calls PZW2
    
    
    
    
    
    
    .PZW2
    
     LDX #WHITE2            \ Set X to white, so we can return that as the safe
                            \ colour in PZW below
    
     EQUB &2C               \ Skip the next instruction by turning it into
                            \ &2C &A9 &23, or BIT &23A9, which does nothing apart
                            \ from affect the flags
    
                            \ Fall through into PZW to fetch the current dashboard
                            \ colours, returning white for safe colours rather than
                            \ stripes
    

[X]

Configuration variable [WHITE2](../../all/workspaces.md#white2) = %00111111

Two mode 2 pixels of colour 7 (white)

[DIALS (Part 4 of 4)](dials_part_4_of_4.md "Previous routine")[PZW](pzw.md "Next routine")
