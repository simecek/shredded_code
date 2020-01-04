---
title: Valentine's Day
author: ''
date: '2019-02-14'
slug: valentine-s-dat
categories: []
tags: []
---

You can run the script by

`python valentine.py.R`

or

`Rscript valentine.py.R`

For Windows or Mac, please, change the line #5 as instructed.

```

s = 'Python is awesome!'
red = "#FA5882"
a = eval("exec('import sys; import matplotlib.pyplot as plt; import numpy as np; print(s); t = np.arange(0,2*np.pi, 0.1); x = 16*np.sin(t)**3; y = 13*np.cos(t)-5*np.cos(2*t)-2*np.cos(3*t)-np.cos(4*t); plt.fill(x,y,color=red); plt.show(); sys.exit()')")
print('R is great too!')
X11()  # Use windows() or qartz() if on Windows or Mac
t = seq(0, 2*pi, by=0.1)
x = 16*sin(t)^3
y = 13*cos(t) - 5*cos(2*t) - 2*cos(3*t) - cos(4*t)
plot(x=x, y=y, type="n", xlab="", ylab="")
polygon(x=x, y=y, col=red, border=red)
cat("Press [Enter]: ")
x = readLines(file("stdin"),1)
```

Original Gist: https://gist.github.com/simecek/e6fc116d0f2ee971b980428568cdf316