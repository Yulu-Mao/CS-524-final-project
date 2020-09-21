```python
import os
import pandas as pd
cwd = os.path.abspath('/Users/baizishan/Documents/Library') 
files = os.listdir(cwd) 
```


```python
cwd
```




    '/Users/baizishan/Documents/Library'




```python
'''
#https://www.scls.info/sites/www.scls.info/files/ils/reports/circ/2019/Circulation%2004-2019.xlsx
from urllib.request import urlretrieve
year = "2019"
middle = "/Circulation%20"
tail = ".xlsx"
for i in range(1,13):
    month = "0"+str(i) if i < 10 else str(i)
    downloadURL = "https://www.scls.info/sites/www.scls.info/files/ils/reports/circ/"+year+middle+month+"-"+year+tail
    try:
        urlretrieve(downloadURL,".\"+"Circulation "+month+"-"+year+tail)
        print("Done "+month+year)
    except:
        print(downloadURL)
'''
```




    '\n#https://www.scls.info/sites/www.scls.info/files/ils/reports/circ/2019/Circulation%2004-2019.xlsx\nfrom urllib.request import urlretrieve\nyear = "2019"\nmiddle = "/Circulation%20"\ntail = ".xlsx"\nfor i in range(1,13):\n    month = "0"+str(i) if i < 10 else str(i)\n    downloadURL = "https://www.scls.info/sites/www.scls.info/files/ils/reports/circ/"+year+middle+month+"-"+year+tail\n    try:\n        urlretrieve(downloadURL,"."+"Circulation "+month+"-"+year+tail)\n        print("Done "+month+year)\n    except:\n        print(downloadURL)\n'




```python
'''
#https://www.scls.info/sites/www.scls.info/files/ils/reports/mostcirclib/2019/2019-10/BAR-most-circed-titles-2019-10.xlsx
#https://www.scls.info/sites/www.scls.info/files/ils/reports/mostcirclib/2019/2019-10/BRD-most-circed-titles-2019-10.xlsx
from urllib.request import urlretrieve
year = "2019"
middle = "-most-circed-titles-"
tail = ".xlsx"
name = "MAD"
for i in range(1,13):
    month = "0"+str(i) if i < 10 else str(i)
    downloadURL = "https://www.scls.info/sites/www.scls.info/files/ils/reports/mostcirclib/"+year+"/"+year+"-"+month+"/"+name+middle+year+"-"+month+tail
    try:
        urlretrieve(downloadURL,"./Users/baizishan/Documents/Library/Circulation/"+name+middle+year+"-"+month+tail)
        print("Done "+month+year)
    except:
        print(downloadURL)
    '''
```




    '\n#https://www.scls.info/sites/www.scls.info/files/ils/reports/mostcirclib/2019/2019-10/BAR-most-circed-titles-2019-10.xlsx\n#https://www.scls.info/sites/www.scls.info/files/ils/reports/mostcirclib/2019/2019-10/BRD-most-circed-titles-2019-10.xlsx\nfrom urllib.request import urlretrieve\nyear = "2019"\nmiddle = "-most-circed-titles-"\ntail = ".xlsx"\nname = "MAD"\nfor i in range(1,13):\n    month = "0"+str(i) if i < 10 else str(i)\n    downloadURL = "https://www.scls.info/sites/www.scls.info/files/ils/reports/mostcirclib/"+year+"/"+year+"-"+month+"/"+name+middle+year+"-"+month+tail\n    try:\n        urlretrieve(downloadURL,"./Users/baizishan/Documents/Library/Circulation/"+name+middle+year+"-"+month+tail)\n        print("Done "+month+year)\n    except:\n        print(downloadURL)\n    '




```python
import numpy as np
import os
import csv
import pandas as pd
from zipfile import ZipFile, ZIP_STORED, ZIP_DEFLATED
import math
from  matplotlib import cm
from matplotlib import pyplot as plt
from matplotlib import font_manager as fm
%matplotlib inline
```


```python
df = pd.read_excel('/Users/baizishan/Documents/Library/Circulation 01-2020.xlsx')
#madMapping(df)

```


```python
def dataBrowse(year,category = None):
    df = pd.read_excel('/Users/baizishan/Documents/Library/Circulation 01-'+year+".xlsx")
    if category != None:
        df = df[["Library",category]]
    else:
        pass
    for i in range(2,9):
        month = "0"+str(i) if i < 10 else str(i)
        fileLink = "/Users/baizishan/Documents/Library/Circulation "+ month+"-"+year+".xlsx"
        if category != None:
            df1 = pd.read_excel(fileLink)[["Library",category]]
        else:
            df1 = pd.read_excel(fileLink)
        df = pd.merge(df,df1,on="Library",how="right",suffixes=(i-1,i))
    df["CircT"] = df.sum(axis= 1)
    return df
```


```python
def madMapping(df):
    madLib = [
    "HAW",
    "LAK",
    "MAD",
    "MEA",
    "MSB",
    "PIN",
    "SEQ",
    "SMB"]
    df_mad = pd.DataFrame()
    for lib in madLib:
        df1 = df[df["Library"]==lib]
        df_mad= pd.concat([df1,df_mad])
    return df_mad
```


```python
df_TC19 = dataBrowse("2019","Total Circ")
df_TC20 = dataBrowse("2020","Total Circ")
df_TC19 = madMapping(df_TC19)
df_TC20 = madMapping(df_TC20)
```


```python
total_circT20=df_TC20["CircT"].sum()
total_circT19=df_TC19["CircT"].sum()
df_TC20.CircT.iloc[0]
a=[]
for i in range(len(df_TC20)):
    a.append((df_TC20.CircT.iloc[i]/total_circT20)*100)
df_TC20["Circweight"]=a

b=[]
for i in range(len(df_TC19)):
    b.append((df_TC19.CircT.iloc[i]/total_circT19)*100)
df_TC19["Circweight"]=b

df_TC20_1=df_TC20.sort_values(by=["Circweight"],ascending=True)
df_TC19_1=df_TC19.sort_values(by=["Circweight"],ascending=True)

df_TC19_1

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Library</th>
      <th>Total Circ1</th>
      <th>Total Circ2</th>
      <th>Total Circ3</th>
      <th>Total Circ4</th>
      <th>Total Circ5</th>
      <th>Total Circ6</th>
      <th>Total Circ7</th>
      <th>Total Circ8</th>
      <th>CircT</th>
      <th>Circweight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>54</td>
      <td>SMB</td>
      <td>10748.0</td>
      <td>10162.0</td>
      <td>11353.0</td>
      <td>10323.0</td>
      <td>9978.0</td>
      <td>9831.0</td>
      <td>10644.0</td>
      <td>10867.0</td>
      <td>83906.0</td>
      <td>4.578840</td>
    </tr>
    <tr>
      <td>31</td>
      <td>MSB</td>
      <td>12025.0</td>
      <td>11718.0</td>
      <td>13088.0</td>
      <td>11689.0</td>
      <td>11681.0</td>
      <td>11881.0</td>
      <td>13418.0</td>
      <td>12525.0</td>
      <td>98025.0</td>
      <td>5.349328</td>
    </tr>
    <tr>
      <td>25</td>
      <td>MEA</td>
      <td>16040.0</td>
      <td>15876.0</td>
      <td>17779.0</td>
      <td>17007.0</td>
      <td>15892.0</td>
      <td>15819.0</td>
      <td>17781.0</td>
      <td>17401.0</td>
      <td>133595.0</td>
      <td>7.290421</td>
    </tr>
    <tr>
      <td>15</td>
      <td>HAW</td>
      <td>25363.0</td>
      <td>22137.0</td>
      <td>22579.0</td>
      <td>21711.0</td>
      <td>22022.0</td>
      <td>20826.0</td>
      <td>22749.0</td>
      <td>22434.0</td>
      <td>179821.0</td>
      <td>9.813023</td>
    </tr>
    <tr>
      <td>17</td>
      <td>LAK</td>
      <td>25096.0</td>
      <td>24280.0</td>
      <td>24195.0</td>
      <td>24430.0</td>
      <td>23665.0</td>
      <td>23604.0</td>
      <td>24988.0</td>
      <td>24788.0</td>
      <td>195046.0</td>
      <td>10.643868</td>
    </tr>
    <tr>
      <td>39</td>
      <td>PIN</td>
      <td>29630.0</td>
      <td>35606.0</td>
      <td>38384.0</td>
      <td>35654.0</td>
      <td>34850.0</td>
      <td>34957.0</td>
      <td>38875.0</td>
      <td>37591.0</td>
      <td>285547.0</td>
      <td>15.582603</td>
    </tr>
    <tr>
      <td>20</td>
      <td>MAD</td>
      <td>45839.0</td>
      <td>44393.0</td>
      <td>47601.0</td>
      <td>43326.0</td>
      <td>43680.0</td>
      <td>41211.0</td>
      <td>43233.0</td>
      <td>41892.0</td>
      <td>351175.0</td>
      <td>19.163993</td>
    </tr>
    <tr>
      <td>51</td>
      <td>SEQ</td>
      <td>63809.0</td>
      <td>60789.0</td>
      <td>66957.0</td>
      <td>61478.0</td>
      <td>60246.0</td>
      <td>61057.0</td>
      <td>66883.0</td>
      <td>64139.0</td>
      <td>505358.0</td>
      <td>27.577923</td>
    </tr>
  </tbody>
</table>
</div>



# Library Project##
[Zishan Week 1]
## Section 1


In this section, we exxplore which library is the most important library in the system of Madison Library. The following figure shows the distribution of total circle in each library in 2019 and 2020.


```python
plt.figure(figsize=(20,10))
plt.subplot(2,1,1)
bar1=plt.bar(df_TC19.Library,df_TC19.CircT)
plt.title('Total cricle in 2019 and 2020 ',fontsize=20)
plt.xticks(fontsize=18)
plt.yticks(fontsize=18)
plt.legend(['2019'],fontsize=15)
plt.subplot(2,1,2)
plt.bar(df_TC20.Library,df_TC20.CircT, color="green")
plt.xticks('2019',fontsize=18)
plt.yticks(fontsize=18)
plt.legend(['2020'], fontsize=15)
plt.show()


```


![png](output_11_0.png)



Then we calculate the propotions of total circle of each library in 2019 and 2020 to check wheather how much propotion of SEQ is in the total circle.


```python
labels1=df_TC19_1.Library
labels2=df_TC20_1.Library
sizes1=df_TC19_1.Circweight
sizes2=df_TC20_1.Circweight
explode = (0, 0, 0, 0,0,0,0,0.1) 
fig, (ax1, ax2) = plt.subplots(1, 2,figsize=(20,10))
#colors = ['#8d46c7', '#7f57cf', '#7168d7', '#6379de', '#568ae6', '#489bee', '#3aacf6', '#2cbdfe']
colors = cm.rainbow(np.arange(len(sizes1))/len(sizes1)) # colormaps: Paired, autumn, rainbow, gray,spring,Darks
patches1, texts1, autotexts1 = ax1.pie(sizes1, labels=labels1, explode=explode,autopct='%1.0f%%',
        shadow=False, startangle=170, colors=colors,wedgeprops={'alpha':0.5},labeldistance=0.8,
                                       textprops = {'fontsize':29, 'color':'black','alpha':0.8})

for t in texts1:
    t.set_size(30)
for t in autotexts1:
    t.set_size(10)
ax1.axis('equal')



proptease = fm.FontProperties()
proptease.set_size('xx-large')
plt.setp(autotexts1, fontproperties=proptease)
plt.setp(texts1, fontproperties=proptease)
ax1.set_title('Weighted Total Circle in 2019', loc='center',size=27)


colors = cm.rainbow(np.arange(len(sizes2))/len(sizes2)) # colormaps: Paired, autumn, rainbow, gray,spring,Darks
patches2, texts2, autotexts2 = ax2.pie(sizes2, labels=labels2, explode=explode,autopct='%1.0f%%',
        shadow=False, startangle=170, colors=colors,wedgeprops={'alpha':0.5},labeldistance=0.8,
                                      textprops = {'fontsize':29, 'color':'black','alpha':0.8})

ax2.axis('equal')

proptease = fm.FontProperties()
proptease.set_size('xx-large')
plt.setp(autotexts2, fontproperties=proptease)
plt.setp(texts2, fontproperties=proptease)
ax2.set_title('Weighted Total Circle in 2020', loc='center',size=27)

plt.axis("off")
plt.tight_layout()


plt.show()


```


![png](output_13_0.png)


We observe that from 2019 to 2020, SEQ is always the most popular library that poeple prefer to go, so we conclude that it is possible to use SEQ as an estimator in the following analysis.

## Section 2
In this section we want to find the most busy time of SEQ library in a day. The following figure shows the distribution by time on each weekday and the distribution of total by time.


```python
def dataBrowse(year,category = None):
    df = pd.read_excel('/Users/baizishan/Documents/Library/SEQ-circ-by-time-wkday-2020-01.xlsx')
    if category != None:
        df = df[["Timeslot",category]]
    else:
        pass
    for i in range(2,9):
        month = "0"+str(i) if i < 10 else str(i)
        fileLink = "/Users/baizishan/Documents/Library/SEQ-circ-by-time-wkday-2020-"+ month +".xlsx"
        if category != None:
            df1 = pd.read_excel(fileLink)[["Timeslot",category]]
        else:
            df1 = pd.read_excel(fileLink)
        df = pd.merge(df,df1,on="Timeslot",how="right",suffixes=(i-1,i))
    df["Totalbytime"] = df.sum(axis= 1)
    return df
```


```python
def opentime(wkd):
    df_SEQtime=dataBrowse(2020,wkd)
    df_SEQopentime=df_SEQtime[11:19]
    return df_SEQopentime
df_total=opentime('SEQ Total Circ')
df_Mondays=opentime('Mondays')
df_Tuesdays=opentime('Tuesdays')
df_Wednesdays=opentime('Wednesdays')
df_Thursdays=opentime("Thursdays")
df_Fridays=opentime('Fridays')
```


```python
plt.style.use('seaborn')
plt.figure(figsize=(8,4),dpi=300)
plt.plot(df_total.Timeslot,df_total.Totalbytime,'o-')
plt.plot(df_Mondays.Timeslot,df_Mondays.Totalbytime,'o-')
plt.plot(df_Tuesdays.Timeslot,df_Tuesdays.Totalbytime,'o-')
plt.plot(df_Wednesdays.Timeslot,df_Wednesdays.Totalbytime,'o-')
plt.plot(df_Thursdays.Timeslot,df_Thursdays.Totalbytime,'o-')
plt.plot(df_Fridays.Timeslot,df_Fridays.Totalbytime,'o-')
plt.xticks(fontsize=10)
plt.title("Total cycle of SEQ by time", color = "#33539E",size=13)
plt.text("16:00-17:00",df_total.Totalbytime.loc[16], "highest (20761.0)",color="black",style='italic',fontsize=10)
plt.legend(['Total','Mondays','Tuesdays','Wednesdays','Thursdays','Fridays'])
plt.axvline(x='16:00-17:00',color = 'r')
plt.axvline(x='17:00-18:00', color = 'r')
plt.grid(True)
```


![png](output_18_0.png)


We observe that if we see each weekday seperately, the most busy time is 4pm to 5pm or 5pm to 6pm, and if we see the total circle by time, the most busy time is 4pm to 5pm. Maybe this is because that people can finish the work at this range and students finish the study at this range. Therefore, we conclude that it is better to have more staffs between 4pm to 5pm on help desk in order to improve the service in SEQ Library.

END


```python
pip install seaborn
```

    Requirement already satisfied: seaborn in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (0.11.0)
    Requirement already satisfied: numpy>=1.15 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from seaborn) (1.17.1)
    Requirement already satisfied: matplotlib>=2.2 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from seaborn) (3.1.1)
    Requirement already satisfied: scipy>=1.0 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from seaborn) (1.4.1)
    Requirement already satisfied: pandas>=0.23 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from seaborn) (0.25.1)
    Requirement already satisfied: cycler>=0.10 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from matplotlib>=2.2->seaborn) (0.10.0)
    Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from matplotlib>=2.2->seaborn) (2.4.2)
    Requirement already satisfied: kiwisolver>=1.0.1 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from matplotlib>=2.2->seaborn) (1.1.0)
    Requirement already satisfied: python-dateutil>=2.1 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from matplotlib>=2.2->seaborn) (2.8.0)
    Requirement already satisfied: pytz>=2017.2 in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from pandas>=0.23->seaborn) (2019.2)
    Requirement already satisfied: six in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from cycler>=0.10->matplotlib>=2.2->seaborn) (1.12.0)
    Requirement already satisfied: setuptools in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (from kiwisolver>=1.0.1->matplotlib>=2.2->seaborn) (49.2.1)
    [33mYou are using pip version 19.0.3, however version 20.2.3 is available.
    You should consider upgrading via the 'pip install --upgrade pip' command.[0m
    Note: you may need to restart the kernel to use updated packages.



```python
import seaborn as sns
```


```python
def wkd(weekdays):
    df_wkd=dataBrowse(2020,weekdays)
    return list(df_wkd.Totalbytime)

Mondays=wkd('Mondays')
Tuesdays=wkd('Tuesdays')
Wednesdays=wkd('Wednesdays')
Thursdays=wkd("Thursdays")
Fridays=wkd('Fridays')
time=df_SEQopentime.Timeslot


list_of_tuples = list(zip(time,Mondays,Tuesdays,Wednesdays,Thursdays,Fridays))
                
df=pd.DataFrame(list_of_tuples, columns=['Timeslot','Mondays','Tuesdays','Wednesdays','Thursdays','Fridays'])                        
#df=df.drop(["Timeslot"],axis=1)
#df["Total by time"] = df.sum(axis= 1)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Timeslot</th>
      <th>Mondays</th>
      <th>Tuesdays</th>
      <th>Wednesdays</th>
      <th>Thursdays</th>
      <th>Fridays</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>11:00-12:00</td>
      <td>72.0</td>
      <td>824.0</td>
      <td>890.0</td>
      <td>987.0</td>
      <td>840.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>12:00-13:00</td>
      <td>21.0</td>
      <td>570.0</td>
      <td>672.0</td>
      <td>660.0</td>
      <td>859.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>13:00-14:00</td>
      <td>21.0</td>
      <td>334.0</td>
      <td>358.0</td>
      <td>401.0</td>
      <td>408.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>14:00-15:00</td>
      <td>8.0</td>
      <td>16.0</td>
      <td>10.0</td>
      <td>15.0</td>
      <td>17.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>15:00-16:00</td>
      <td>29.0</td>
      <td>28.0</td>
      <td>38.0</td>
      <td>37.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <td>5</td>
      <td>16:00-17:00</td>
      <td>76.0</td>
      <td>90.0</td>
      <td>85.0</td>
      <td>157.0</td>
      <td>44.0</td>
    </tr>
    <tr>
      <td>6</td>
      <td>17:00-18:00</td>
      <td>238.0</td>
      <td>206.0</td>
      <td>230.0</td>
      <td>241.0</td>
      <td>219.0</td>
    </tr>
    <tr>
      <td>7</td>
      <td>18:00-19:00</td>
      <td>530.0</td>
      <td>505.0</td>
      <td>303.0</td>
      <td>424.0</td>
      <td>312.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#sns.catplot(data=df, orient="h", kind="box")

```


```python

```


```python

```


```python

```


```python

```
