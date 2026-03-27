---
title: "The Dashboard image (Game data) variable - Elite on the 6502"
source: "https://elite.bbcelite.com/master/game_data/variable/dashboard_image.html"
---

[F%](../../main/variable/f_per_cent.md "Previous routine")[XX21 (Game data)](xx21.md "Next routine")
    
    
           Name: Dashboard image                                         [Show more]
           Type: Variable
       Category: Loader
        Summary: The binary for the dashboard image
    
    
        Context: See this variable [in context in the source code](../../all/elite_data.md#header-dashboard-image)
     References: No direct references to this variable in this source file
    
    
    
    
    
    * * *
    
    
     The data file contains the dashboard binary, which gets moved into screen
     memory by the loader:
    
       * P.DIALS2P.bin contains the dashboard, which gets moved to screen address
         &7000, which is the starting point of the eight-colour mode 2 portion at
         the bottom of the split screen
    
    
    
    
    .DIALS
    
     INCBIN "1-source-files/images/P.DIALS2P.bin"
    
     SKIP 256               \ These bytes appear to be unused, but they get moved to
                            \ &7E00-&7EFF along with the dashboard
    

[F%](../../main/variable/f_per_cent.md "Previous routine")[XX21 (Game data)](xx21.md "Next routine")
