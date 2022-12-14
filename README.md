# Fortune 500 Exploratory Data Analysis

1. Import Libraries

```
import pandas as pd
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

import warnings
warnings.filterwarnings('ignore')

plt.rcParams['figure.figsize'] = (20,20)
plt.rcParams['figure.dpi'] = 1000
pd.options.display.max_rows = 200
```
2. Import Dataset
```

df0 = pd.read_csv('Fortune500.csv')
df = df0.copy()
df.head()
```
|index|rank|company|revenue|profit|num\. of employees|sector|city|state|website|
|---|---|---|---|---|---|---|---|---|---|
|0|1|Walmart|523964\.0|14881\.0|2200000|Retailing|Bentonville|AR|https://www\.stock\.walmart\.com|
|1|2|Amazon|280522\.0|11588\.0|798000|Retailing|Seattle|WA|https://www\.amazon\.com|
|2|3|Exxon Mobil|264938\.0|14340\.0|74900|Energy|Irving|TX|https://www\.exxonmobil\.com|
|3|4|Apple|260174\.0|55256\.0|137000|Technology|Cupertino|CA|https://www\.apple\.com|
|4|5|CVS Health|256776\.0|6634\.0|290000|Health Care|Woonsocket|RI|https://www\.cvshealth\.com|

3. Data Preparation
```
df.info() # no null, great
```
![Screen Shot 2022-08-22 at 6 53 03 PM](https://user-images.githubusercontent.com/108639250/186032562-2803f852-791d-4080-819f-43c5147df4d3.png)

4. Search for NaN
```
df.isna() # no NaN
```
|index|rank|company|revenue|profit|num\. of employees|sector|city|state|website|
|---|---|---|---|---|---|---|---|---|---|
|0|false|false|false|false|false|false|false|false|false|
|1|false|false|false|false|false|false|false|false|false|
|2|false|false|false|false|false|false|false|false|false|
|496|false|false|false|false|false|false|false|false|false|
|497|false|false|false|false|false|false|false|false|false|
|498|false|false|false|false|false|false|false|false|false|
|499|false|false|false|false|false|false|false|false|false|

5. Display technology firms

```
df[df['sector'] == 'Technology'].head(6)
```
| rank |           company |  revenue |  profit | num. of employees |     sector |          city | state |                          website |
|-----:|------------------:|---------:|--------:|------------------:|-----------:|--------------:|------:|---------------------------------:|
|    4 |             Apple | 260174.0 | 55256.0 |            137000 | Technology |     Cupertino |    CA |            https://www.apple.com |
|   11 |          Alphabet | 161857.0 | 34343.0 |            118899 | Technology | Mountain View |    CA |              https://www.abc.xyz |
|   21 |         Microsoft | 125843.0 | 39240.0 |            144000 | Technology |       Redmond |    WA |        https://www.microsoft.com |
|   34 | Dell Technologies |  92154.0 |  4616.0 |            165000 | Technology |    Round Rock |    TX | https://www.delltechnologies.com |
|   38 |               IBM |  77147.0 |  9431.0 |            383800 | Technology |        Armonk |    NY |              https://www.ibm.com |
|   45 |             Intel |  71965.0 | 21048.0 |            110800 | Technology |   Santa Clara |    CA |            https://www.intel.com |

6. Group major sectors

```
group = df['sector'].value_counts([:8]).index
group
```
Index(['Financials', 'Energy', 'Retailing', 'Technology', 'Health Care',
       'Food, Beverages & Tobacco', 'Wholesalers', 'Materials'],
      dtype='object')

7. Visualize num. of companies per sector
```
fig, ax = plt.subplots(figsize=(25,15))
ax = sns.countplot(x='sector', data=df, order=group)
ax.set_xticklabels(group, rotation=50, size=20)
for p in ax.patches:
  ax.annotate(p.get_height(), (p.get_x()+0.28, p.get_height()-25), size=15, c='white')
ax.set_title('Top 10 sectors', size=30)
ax.set_yticklabels([])
ax.set_xlabel('')
ax.set_ylabel('Number of Companies', size=20)
```
![Screen Shot 2022-08-22 at 10 06 10 PM](https://user-images.githubusercontent.com/108639250/186052800-51bbf16b-73aa-42a9-beb9-8857f50540e2.png)

8. Group data by state instead of rank

```
state = df.groupby('state', as_index=False).sum()
state.drop(['rank'], axis=1, inplace=True)
state.head()
```
| state |   revenue |   profit | num. of employees |
|------:|----------:|---------:|------------------:|
|    AL |    6755.0 |   1582.0 |             19564 |
|    AR |  593978.8 |  17685.2 |           2407706 |
|    AZ |   59110.6 |   1225.9 |            100361 |
|    CA | 1642245.5 | 243037.7 |           2655976 |
|    CO |  139591.0 |   6779.9 |            264788 |


9. Compare top 6 firms by profit
```
top_profit = df.sort_values('profit', ascending=False)[:6]
top_profit
```
| rank |       company      |  revenue |  profit | num. of employees |   sector   |      city     | state |              website              |
|:----:|:------------------:|:--------:|:-------:|:-----------------:|:----------:|:-------------:|:-----:|:---------------------------------:|
|    6 | Berkshire Hathaway | 254616.0 | 81417.0 |            391500 | Financials | Omaha         | NE    | https://www.berkshirehathaway.com |
|    4 | Apple              | 260174.0 | 55256.0 |            137000 | Technology | Cupertino     | CA    | https://www.apple.com             |
|   21 | Microsoft          | 125843.0 | 39240.0 |            144000 | Technology | Redmond       | WA    | https://www.microsoft.com         |
|   17 | JPMorgan Chase     | 142422.0 | 36431.0 |            256981 | Financials | New York      | NY    | https://www.jpmorganchase.com     |
|   11 | Alphabet           | 161857.0 | 34343.0 |            118899 | Technology | Mountain View | CA    | https://www.abc.xyz               |
|   25 | Bank of America    | 113589.0 | 27430.0 |            208131 | Financials | Charlotte     | NC    | https://www.bankofamerica.com     |
