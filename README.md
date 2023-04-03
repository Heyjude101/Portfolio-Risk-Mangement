

## <a name="_cjzgm2fs02uy"></a>**Introduction:**
##
<a name="_z5mugefo9p5k"></a>**Risk management in portfolio optimization:**

Correlation diversification is a risk management technique used to protect portfolios against market volatility by spreading investments across different asset classes with low correlations to each other. The idea behind correlation diversification is that by investing in assets that are not highly correlated, the portfolio will be less vulnerable to large swings in any one asset class. For example, if an investor puts all their money in stocks, the portfolio is highly exposed to stock market risk. However, if the investor also invests in bonds or real estate, which have historically shown low correlation with stocks, the portfolio's overall risk may be reduced. The concept of correlation diversification can be applied to both individual portfolios and larger, more complex investment strategies such as mutual funds, hedge funds, and exchange-traded funds (ETFs). These investment vehicles often hold a mix of different asset classes to achieve diversification. It's worth noting that correlation diversification does not eliminate risk entirely, but it can help reduce risk and protect portfolios during times of market volatility. It is also important to regularly monitor and adjust the portfolio to ensure that the assets' correlation remains low and the diversification strategy is working effectively. Overall, correlation diversification is an effective tool for protecting portfolios and managing risk. By investing in a mix of assets with low correlations, investors can potentially achieve more stable returns and mitigate the impact of market fluctuations.
### <a name="_dhfrmc9hhzvk"></a>**PROBLEM STATEMENT:**

For the portfolio managers, this project of ours will provide insights and visuals on the correlation of various sectors in Nifty and they will be able to make wise decisions in order to safeguard portfolios from major swings.

### <a name="_5yug4iyc8cd"></a>**Index:**
1. Data gathering
1. Data Preparation
1. Correlation Calculation
1. Visualisation


### <a name="_vnnnoolgfzls"></a>**Data gathering:**
We used nsepy to get the data. Nsepy is a python module provided by nse to access data for algorithmic analysis. However sometimes, it gives rate limit error when we access from cloud engines so we redirect IP using Jugaad-Data Api.

#### <a name="_92ogniuj7or1"></a>Source code:

!pip install nsepy

from nsepy import get\_history

from datetime import date

start  =date(2018, 1,1)

end = date(2023 , 1 ,1)

symbol = 'NIFTY 50'

data = get\_history(symbol = symbol , start = start , end = end , index = True)

print(data)

data.to\_csv('NIFTY50.csv')

#Sample Source Code for Jugaad Data

from jugaad\_data.nse import index\_csv, index\_df

\# Download as pandas dataframe

df = index\_df(symbol="NIFTY 50", from\_date=date(2020,1,1),

`            `to\_date=date(2020,1,30))

print(df.head())








***Sample Output:***

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.001.png)
###
















<a name="_5bovj23rzxyt"></a>**Data Preparation:**

- Reading the csv file and removing all the unused columns and calculating the %candle change and change from previous day close and the columns in the csv .

- Loading the dataset and dropping out unnecessary columns

val='{name\_of\_the\_file}.csv'

dataset = pd.read\_csv(val)

dataset = dataset.drop(['Turnover','Volume'], axis=1)

















**Sample Output:**

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.002.png)

- Calculating candle change and storing it in the csv file as column 

dataset['%Cchange'] = ((dataset['Close']-dataset['Open'])/dataset['Open'])\*100;

dataset.to\_csv(val)


- Calculating the percentage change from the previous day close and storing it in csv 

dataset['%Pchange'] = ((dataset['Close'].diff())/dataset['Close'])\*100;

- There were some values missing as NaN. So we needed to fill those values with specific data.

f\_closeC = dataset.iloc[:, -3].values

f\_closeP = dataset.iloc[:, -2].values

f\_close = (f\_closeC[0]+f\_closeC[0]\*(f\_closeP[0]/100));

CX = ((f\_closeC[0]-f\_close)/f\_close)\*100

dataset.loc[0,'%Pchange'] = str(CX)

dataset.to\_csv(val)

print (dataset)






**Sample Output:**

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.003.png)

- Removing extra data from csv by taking date as a reference . Here we are considering only those days whose information is present in all the csv and removing all the extra data . 

- Taking all csv file and converting it into array for further processing

dataset1=pd.read\_csv('AUTO\_U.csv')

d1=np.array(dataset1)

print(d1)

dataset2=pd.read\_csv('CNXIT\_U.csv')

d2=np.array(dataset2)

print(d2)

dataset3=pd.read\_csv('INFRA\_U.csv')

d3=np.array(dataset3)

print(d3)

dataset4=pd.read\_csv('NIFTYBANK\_U.csv')

d4=np.array(dataset4)

print(d4)

dataset5=pd.read\_csv('NIFTYENERGY\_U.csv')

d5=np.array(dataset5)

print(d5)

dataset6=pd.read\_csv('NIFTYMETAL\_U.csv')

d6=np.array(dataset6)

print(d6)

dataset7=pd.read\_csv('NIFTYPSE\_U.csv')

d7=np.array(dataset7)

print(d7)

dataset8=pd.read\_csv('NIFTY\_U.csv')

d8=np.array(dataset8)

print(d8)

dataset9=pd.read\_csv('PHARMA\_U.csv')

d9=np.array(dataset9)

print(d9)


- By using reduce class we are taking the intersection of all classes and storing it in I

from functools import reduce

I = reduce(

`    `lambda l,r: np.intersect1d(l,r,True),

`    `(i[:,1] for i in (d1,d2,d3,d4,d5,d6,d7,d8,d9)))

- Create a new array by keeping only those dates which are present with I and converting it into csv and storing it in files

d11=d1[np.searchsorted(d1[:,1], I)]

pd.DataFrame(d11).to\_csv('AUTO\_U.csv', index=False)

d12=d2[np.searchsorted(d2[:,1], I)]

pd.DataFrame(d12).to\_csv('CNXIT\_U.csv', index=False)

d13=d3[np.searchsorted(d3[:,1], I)]

pd.DataFrame(d13).to\_csv('INFRA\_U.csv' ,index=False)

d14=d4[np.searchsorted(d4[:,1], I)]

pd.DataFrame(d14).to\_csv('NIFTYBANK\_U.csv' ,index=False)

d15=d5[np.searchsorted(d5[:,1], I)]

pd.DataFrame(d15).to\_csv('NIFTYENERGY\_U.csv', index=False)

d16=d6[np.searchsorted(d6[:,1], I)]

pd.DataFrame(d16).to\_csv('NIFTYMETAL\_U.csv', index=False)

d17=d7[np.searchsorted(d7[:,1], I)]

pd.DataFrame(d17).to\_csv('NIFTYPSE\_U.csv', index=False)

d18=d8[np.searchsorted(d8[:,1], I)]

pd.DataFrame(d18).to\_csv('NIFTY\_U.csv', index=False)

d19=d9[np.searchsorted(d9[:,1], I)]

pd.DataFrame(d19).to\_csv('PHARMA\_U.csv', index=False)

- Confirming whether the above data is consistent or not:

cols= ['AUTO\_U.csv','CNXIT\_U.csv','INFRA\_U.csv','NIFTYBANK\_U.csv','NIFTYENERGY\_U.csv','NIFTYMETAL\_U.csv','NIFTY\_U.csv','PHARMA\_U.csv']

dataset =[]

f=True

for i in cols:

`  `dataf = pd.read\_csv(i)

`  `df = np.array(dataf)

`  `for j in cols:

`    `dataf1=pd.read\_csv(j)

`    `df1 = np.array(dataf1)

`    `for k in df[:,1]:

`      `if k in df1[:,1]:

`        `continue

`      `else:

`        `f=False

print(f)


**Correlation Calculation:**

The correlation calculation phase involves determining the degree to which two variables are related to each other. In statistical terms, correlation refers to the measure of the linear relationship between two variables. Here, we calculate correlation based on daily change from the previous day close of all sectors with each other. 







Source code:

import pandas as pd

import numpy as np

from scipy.stats import pearsonr

dataNiftyC = pd.read\_csv('NIFTY\_U.csv')['%Pchange'].to\_numpy()

dataPharmaC = pd.read\_csv('PHARMA\_U.csv')['%Pchange'].to\_numpy()

dataPseC  =pd.read\_csv('NIFTYPSE\_U.csv')['%Pchange'].to\_numpy()

dataMetalC = pd.read\_csv('NIFTYMETAL\_U.csv')['%Pchange'].to\_numpy()

dataEnergyC  =pd.read\_csv('NIFTYENERGY\_U.csv')['%Pchange'].to\_numpy()

dataBankC = pd.read\_csv('NIFTYBANK\_U.csv')['%Pchange'].to\_numpy()

dataInfraC = pd.read\_csv('INFRA\_U.csv')['%Pchange'].to\_numpy()

dataItC  =pd.read\_csv('CNXIT\_U.csv')['%Pchange'].to\_numpy()

dataAutoC = pd.read\_csv('AUTO\_U.csv')['%Pchange'].to\_numpy()

datalist =[dataNiftyC , dataPharmaC , dataPseC , dataMetalC , dataEnergyC , dataBankC , dataInfraC ,dataItC ,dataAutoC]

ll = []

for i in datalist:

`  `l = []

`  `for j in datalist:

`    `r,p  = pearsonr(i,j)

`    `l.append('%.2f' % r)

`  `ll.append(l)

df = pd.DataFrame(ll, columns =['NIFTY\_U' , 'PHARMA\_U' , 'NIFTYPSE\_U', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'NIFTYBANK\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U'])

df[' ']= ['NIFTY\_U' , 'PHARMA\_U' , 'NIFTYPSE\_U', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'NIFTYBANK\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U']

df = df[[' ' , 'NIFTY\_U', 'PHARMA\_U' , 'NIFTYPSE\_U', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'NIFTYBANK\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U']]

df.to\_csv('Final\_pearson.csv')

**OUTPUT:**

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.004.png)

- Overall, the correlation calculation phase is an important step in analysing the relationship between two variables, and can help to identify patterns and trends in data.
- This coefficient ranges from -1 to +1, with values of -1 indicating a perfect negative correlation, values of +1 indicating a perfect positive correlation, and values of 0 indicating no correlation.




### <a name="_ryac0rf6c4c7"></a>**DATA VISUALIZATION:**


import matplotlib.pyplot as plt

import pandas as pd

df=pd.read\_csv("Final\_pearson.csv")

**OUTPUT:**

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.005.jpeg)




### <a name="_jv8kyeyl0i0"></a>BAR PLOTS:
import numpy as np

import matplotlib.pyplot as plt

import pandas as pd

\# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['NIFTY\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['PHARMA\_U' , 'NIFTYPSE\_', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'NIFTYBANK\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U']

y = data

print(data)


\# creating the dataset# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Corealtion of Nifty with Other Indices")

plt.show()

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.006.png)# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['PHARMA\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['NIFTY\_U ' , 'NIFTYPSE\_', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'NIFTYBANK\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U']

y = data

print(data)

\# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Corealtion of PHARMA with Other Indices")

plt.show()

[1.   0.48 0.5  0.47 0.41 0.56 0.44 0.49]

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.007.png)

\# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['NIFTYPSE\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['NIFTY\_U ' , 'PHARMA\_U', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'NIFTYBANK\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U']

y = data

print(data)

\# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Corealtion of NIFTYPSE with Other Indices")

plt.show()

[0.48 1.   0.76 0.84 0.61 0.8  0.39 0.65]

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.008.png)

\# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['NIFTYMETAL\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['NIFTY\_U ' , 'NIFTYPSE\_U', 'PHARMA\_U' ,'NIFTYENERGY\_U', 'NIFTYBANK\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U']

y = data

print(data)

\# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Corealtion of NIFTYMETAL with Other Indices")

plt.show()

[0.5  0.76 1.   0.67 0.61 0.74 0.44 0.65]

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.009.png)

\# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['NIFTYENERGY\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['NIFTY\_U ' , 'NIFTYPSE\_', 'NIFTYMETAL\_U' ,'PHARMA\_U', 'NIFTYBANK\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U']

y = data

print(data)

\# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Correlation of NIFTYENERGY with Other Indices")

plt.show()

[0.47 0.84 0.67 1.   0.62 0.82 0.43 0.65]

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.010.png)

\# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['NIFTYBANK\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['NIFTY\_U ' , 'NIFTYPSE\_', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'PHARMA\_U' , 'INFRA\_U' , 'CNXIT\_U' , 'AUTO\_U']

y = data

print(data)

\# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Correlation of NIFTYBANK with Other Indices")

plt.show()

[0.41 0.61 0.61 0.62 1.   0.76 0.44 0.72]

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.011.png)

\# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['INFRA\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['NIFTY\_U ' , 'NIFTYPSE\_', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'PHARMA\_U' , 'NIFTYBANK\_U' , 'CNXIT\_U' , 'AUTO\_U']

y = data

print(data)

\# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Corealtion of INFRA with Other Indices")

plt.show()

[0.56 0.8  0.74 0.82 0.76 1.   0.51 0.79]

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.012.png)

\# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['CNXIT\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['NIFTY\_U ' , 'NIFTYPSE\_', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'PHARMA\_U' , 'NIFTYBANK\_U' , 'INFRA\_U' , 'AUTO\_U']

y = data

print(data)

\# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Corealtion of CNXIT with Other Indices")

plt.show()

[0.44 0.39 0.44 0.43 0.44 0.51 1.   0.46]

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.013.png)

\# creating the dataset

data = pd.read\_csv('Final\_pearson.csv')['AUTO\_U'].to\_numpy()

data  =np.delete(data , [0])

x = ['NIFTY\_U ' , 'NIFTYPSE\_', 'NIFTYMETAL\_U' ,'NIFTYENERGY\_U', 'PHARMA\_U' , 'NIFTYBANK\_U' , 'CNXIT\_U' , 'INFRA\_U']

y = data

print(data)

\# creating the bar plot

plt.figure(figsize=(12,6))

plt.barh(x, y, color ='green',height = 0.4)

plt.xlabel("Pearson's Coeff")

plt.title("Corealtion of AUTO with Other Indices")

plt.show()

[0.49 0.65 0.65 0.65 0.72 0.79 0.46 1.  ]

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.014.png)


#### <a name="_23kofub6ei2"></a>LINE PLOTS:


![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.015.png)




#### <a name="_51eal23musn9"></a>HEAT MAPS

import numpy as np

import seaborn as sns

from matplotlib.colors import ListedColormap

print("The data to be plotted:\n")

print(df10)

data = np.asarray(df10)

sns.heatmap( data,cmap=ListedColormap(['green', 'yellow', 'red']))

\# plotting the heatmap

\# hm = sns.heatmap(data = df10)



\# displaying the plotted heatmap

plt.show()

\# cols = ["NAMES","NIFTY\_U", "PHARMA\_U", "NIFTYPSE\_U", "NIFTYMETAL\_U", "NIFTYENERGY\_U", "NIFTYBANK\_U", "INFRA\_U", "CNXIT\_U", "AUTO\_U"]

df=pd.read\_csv("Final\_pearson.csv")

import numpy as np

import seaborn as sn

import matplotlib.pyplot as plt



\# generating 2-D 10x10 matrix of random numbers

\# from 1 to 100

data = df[['NIFTY\_U','PHARMA\_U','NIFTYPSE\_U','NIFTYMETAL\_U','NIFTYENERGY\_U','NIFTYBANK\_U','INFRA\_U','CNXIT\_U','AUTO\_U']]

print("The data to be plotted:\n")

print(data)

\# plotting the heatmap

sns.palplot(sn.color\_palette("coolwarm",7))

plt.figure(figsize=(7, 7))

\# plt.yticks(list(reversed(range(len(df)))), ['Index '+str(x) for x in indices], rotation='horizontal')

plt.title('Correlation');

hm = sn.heatmap(data = data,cmap="coolwarm")



\# displaying the plotted heatmap

plt.show()

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.016.jpeg)


![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.017.png)

*Reference:*

![](Aspose.Words.00d30626-92c3-4e62-a5c8-c844d6df9a68.018.png)
