---
title: Get S&P500 data with pandas_datareader
author: ''
date: '2019-11-12'
slug: get-s-p500-data-with-pandas-datareader
categories: []
tags: []
---

```
import pandas as pd
from pandas_datareader import data as wb
```

## Getting data


```
# ^GSPC is code for S&P500, see https://finance.yahoo.com/quote/%5EGSPC/
ticker_name = '^GSPC'

ticker = wb.DataReader(ticker_name, start='2010-1-1', data_source='yahoo')
print(ticker)
```

                       High          Low  ...      Volume    Adj Close
    Date                                  ...                         
    2010-01-04  1133.869995  1116.560059  ...  3991400000  1132.989990
    2010-01-05  1136.630005  1129.660034  ...  2491020000  1136.520020
    2010-01-06  1139.189941  1133.949951  ...  4972660000  1137.140015
    2010-01-07  1142.459961  1131.319946  ...  5270680000  1141.689941
    2010-01-08  1145.390015  1136.219971  ...  4389590000  1144.979980
    ...                 ...          ...  ...         ...          ...
    2019-11-05  3083.949951  3072.149902  ...  4486130000  3074.620117
    2019-11-06  3078.340088  3065.889893  ...  4458190000  3076.780029
    2019-11-07  3097.770020  3080.229980  ...  4144640000  3085.179932
    2019-11-08  3093.090088  3073.580078  ...  3499150000  3093.080078
    2019-11-11  3088.330078  3075.820068  ...  1407760565  3087.010010
    
    [2482 rows x 6 columns]


## Calculating the differences


```
# `Adj Close` -> daily difference
ticker['Close_diff'] = ticker['Adj Close'].diff()
ticker['Close_diff'].head()

starting_value = ticker['Adj Close'][0]
```

## Recreating the original column


```
# daily difference -> `Adj Close`
ticker['Close_diff'].cumsum().fillna(0) + starting_value

```




    Date
    2010-01-04    1132.989990
    2010-01-05    1136.520020
    2010-01-06    1137.140015
    2010-01-07    1141.689941
    2010-01-08    1144.979980
                     ...     
    2019-11-05    3074.620117
    2019-11-06    3076.780029
    2019-11-07    3085.179932
    2019-11-08    3093.080078
    2019-11-11    3087.010010
    Name: Close_diff, Length: 2482, dtype: float64




```

```

**Original Gist**: https://gist.github.com/simecek/c9cc0b8f336ad5ad1f987492391b4da2

**Try it in Colab**: https://colab.research.google.com/gist/simecek/c9cc0b8f336ad5ad1f987492391b4da2/s-p500-diff.ipynb