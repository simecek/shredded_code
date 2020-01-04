---
title: Set options to save all your data when the script crashes
author: ''
date: '2018-08-18'
slug: set-options-to-save-all-your-data-when-the-script-crashes
categories: []
tags: []
---

If the running time of your R script is in hours and there is a danger of crash:

```
save_all <- function() {
  save.image("recover.rda")
}
options(error = save_all)
```

I wish I discovered this before  

```
"Error in pdf(...): failed to load default encoding"
```

appeared on my screen.