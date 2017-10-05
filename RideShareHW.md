

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors
from matplotlib.legend_handler import HandlerLine2D
import seaborn
import os
#%matplotlib inline
```


```python
# Retrieve current working directory (`cwd`)
cwd = os.getcwd()
print(cwd)

path=os.getcwd()

# Change directory 
#os.chdir("/path/to/your/folder")


```

    C:\Users\Aurora Firebirdaur\Desktop
    


```python
#cdata_csv = os.path.join('Desktop', 'city_data.csv')
#rdata_csv = os.path.join('Desktop', 'ride_data.csv')
cdata_csv = os.path.join('./city_data.csv')
rdata_csv = os.path.join('./ride_data.csv')

```


```python
# Read file with Pandas, and store its contents in a new variable
cdata_csv_df = pd.read_csv(cdata_csv)
rdata_csv_df = pd.read_csv(rdata_csv)
#print(cdata_csv_df)
#print(rdata_csv_df)
print(cdata_csv_df.head())
print(rdata_csv_df.head())
```

                 city  driver_count   type
    0      Kelseyland            63  Urban
    1      Nguyenbury             8  Urban
    2    East Douglas            12  Urban
    3   West Dawnfurt            34  Urban
    4  Rodriguezburgh            52  Urban
              city                 date   fare        ride_id
    0     Sarabury  2016-01-16 13:49:27  38.35  5403689035038
    1    South Roy  2016-01-02 18:42:34  17.49  4036272335942
    2  Wiseborough  2016-01-21 17:35:29  44.18  3645042422587
    3  Spencertown  2016-07-31 14:53:22   6.87  2242596575892
    4   Nguyenbury  2016-07-09 04:42:44   6.28  1543057793673
    


```python
# Merge the two data frames on city 

merged_city_and_rides_df = pd.merge(left=cdata_csv_df, right=rdata_csv_df, how='left', on=["city","city"])
#print(merged_city_and_rides_df)
```


```python
merged_city_and_rides_df.columns
```




    Index(['city', 'driver_count', 'type', 'date', 'fare', 'ride_id'], dtype='object')




```python
scat_df=(merged_city_and_rides_df.groupby(['type','city', 'driver_count']).agg({'fare':'mean', 'ride_id':'count'}))
scat_df = scat_df.reset_index()
print(scat_df)

```

             type                  city  driver_count       fare  ride_id
    0       Rural           East Leslie             9  33.660909       11
    1       Rural          East Stephen             6  39.053000       10
    2       Rural         East Troybury             3  33.244286        7
    3       Rural              Erikport             3  30.043750        8
    4       Rural        Hernandezshire            10  32.002222        9
    5       Rural             Horneland             8  21.482500        4
    6       Rural           Jacksonfort             6  32.006667        6
    7       Rural          Kennethburgh             3  36.928000       10
    8       Rural             Kinghaven             3  34.980000        6
    9       Rural         Manuelchester             7  49.620000        1
    10      Rural           Matthewside             4  43.532500        4
    11      Rural          New Johnbury             6  35.042500        4
    12      Rural         North Whitney            10  38.146000       10
    13      Rural           Shelbyhaven             9  34.828333        6
    14      Rural  South Elizabethmouth             3  28.698000        5
    15      Rural          South Joseph             3  38.983333       12
    16      Rural           Stevensport             6  31.948000        5
    17      Rural        West Kevintown             5  21.528571        7
    18   Suburban            Anitamouth            16  37.315556        9
    19   Suburban          Campbellport            26  33.711333       15
    20   Suburban           Carrollbury             4  36.606000       10
    21   Suburban             Clarkstad            21  31.051667       12
    22   Suburban           Conwaymouth            18  34.591818       11
    23   Suburban       East Cherylfurt             9  31.416154       13
    24   Suburban  East Jenniferchester            22  32.599474       19
    25   Suburban            Floresberg             7  32.310000       10
    26   Suburban             Jasonfort            25  27.831667       12
    27   Suburban            Jeffreyton             8  33.165556       18
    28   Suburban              Johnland            13  28.752778       18
    29   Suburban               Kyleton            12  31.167500       16
    ..        ...                   ...           ...        ...      ...
    96      Urban       Port Martinberg            44  22.329524       21
    97      Urban         Port Samantha            55  27.047407       27
    98      Urban             Prattfurt            43  23.346667       24
    99      Urban        Rodriguezburgh            52  21.332609       23
    100     Urban           Russellport             9  22.486087       23
    101     Urban            Sandymouth            11  23.105926       27
    102     Urban              Sarabury            46  23.490000       27
    103     Urban            Smithhaven            67  22.788889       27
    104     Urban       South Bryanstad            73  24.598571       21
    105     Urban     South Josephville             4  26.823750       24
    106     Urban           South Louis            12  27.087500       32
    107     Urban             South Roy            35  26.031364       22
    108     Urban           Spencertown            68  23.681154       26
    109     Urban           Stewartview            49  21.614000       30
    110     Urban           Swansonbury            64  27.464706       34
    111     Urban           Torresshire            70  24.207308       26
    112     Urban           Travisville            37  27.220870       23
    113     Urban            Vickimouth            13  21.474667       15
    114     Urban           West Alexis            47  19.523000       20
    115     Urban           West Brandy            12  24.157667       30
    116     Urban      West Brittanyton             9  25.436250       24
    117     Urban         West Dawnfurt            34  22.330345       29
    118     Urban      West Jefferyfurt            65  21.072857       21
    119     Urban            West Oscar            11  24.280000       29
    120     Urban            West Peter            61  24.875484       31
    121     Urban      West Sydneyhaven            70  22.368333       18
    122     Urban          Williamshire            70  26.990323       31
    123     Urban           Wiseborough            55  22.676842       19
    124     Urban           Yolandafurt             7  27.205500       20
    125     Urban        Zimmermanmouth            45  28.301667       24
    
    [126 rows x 5 columns]
    


```python
suburban_df = scat_df[scat_df['type'] == 'Suburban']
rural_df = scat_df[scat_df['type'] == 'Rural']
urban_df = scat_df[scat_df['type'] == 'Urban']

#print(suburban_df)
#print(urban_df)
```


```python
fig, ax = plt.subplots(1,1)
# set the figure boundaries

# Build a scatter plot for each City type

suburban_df.plot.scatter(x='ride_id',y='fare', s=suburban_df['driver_count']*10, color='lightskyblue',
                            label='Suburban',alpha=0.5, marker = 'o', ax=ax)
rural_df.plot.scatter(x='ride_id',y='fare', s=rural_df['driver_count']*10, color='gold',
                      label='Rural', alpha=0.5, marker = 'o',ax=ax)
urban_df.plot.scatter(x='ride_id',y='fare', s=urban_df['driver_count']*10, color='lightcoral', 
                      label='Urban', alpha=0.5, marker = 'o', ax=ax)

plt.style.use('seaborn-white')

plt.xlim([1, 40])
plt.ylim([1, 60])

```




    (1, 60)




```python
plt.title("Pyber Ride Sharing Data(2016)")
plt.xlabel("Total Number Rides Per City Type")
plt.ylabel("Average Fare by city type")
plt.grid(True)

```


```python
lgnd=plt.legend(scatterpoints=1, markerscale = 1.0,
           loc='upper right', ncol=1, 
           fontsize=10, title='City Types', labelspacing=0.5)

lgnd.legendHandles[0]._sizes = [30]
lgnd.legendHandles[1]._sizes = [30]
lgnd.legendHandles[2]._sizes = [30]

plt.show()

# Save the figure
plt.savefig("City_RideShare.png")

```


![png](output_10_0.png)



```python
fig = plt.figure()
ax = fig.add_subplot(111)

labels = ["Rural", "Suburban", "Urban"]
fares = (merged_city_and_rides_df.groupby('type').agg({'fare':'sum'}))

colors = ["gold", "lightskyblue", "lightcoral"]
# how big is the explosion, and what should explode?
explode = (0.1, 0, 0)

ax.set_title("% of Total Fares per City Type")
# the first two %'s format the number. The last one is what is displayed on the graph
ax.pie(fares, explode=explode, labels=labels,autopct="%1.1f%%",colors=colors, shadow=True, startangle=160)

plt.savefig("pie_fares.png")
plt.show()

```


    <matplotlib.figure.Figure at 0x1f9a9248f28>



![png](output_11_1.png)



```python
fig = plt.figure()
ax = fig.add_subplot(111)

labels = ["Rural", "Suburban", "Urban"]
rides = (merged_city_and_rides_df.groupby('type').agg({'ride_id':'count'}))

colors = ["gold", "lightskyblue", "lightcoral"]
# how big is the explosion, and what should explode?
explode = (0.1, 0, 0)

ax.set_title("% of Total Rides per City Type")

# the first two %'s format the number. The last one is what is displayed on the graph
ax.pie(rides, explode=explode, labels=labels,autopct="%1.1f%%",colors=colors, shadow=True, startangle=160)

plt.savefig("pie_rides.png")
plt.show()

```


![png](output_12_0.png)



```python
fig = plt.figure()
ax = fig.add_subplot(111)

labels = ["Rural", "Suburban", "Urban"]
drivers = (cdata_csv_df.groupby("type").agg({"driver_count":"sum"}))

colors = ["gold", "lightskyblue", "lightcoral"]
# how big is the explosion, and what should explode?
explode = (0.1, 0, 0)

ax.set_title("% of Total Drivers per City Type")

# the first two %'s format the number. The last one is what is displayed on the graph
ax.pie(drivers, explode=explode, labels=labels,autopct="%1.1f%%",colors=colors, shadow=True, startangle=160)

plt.savefig("pie_drivers.png")
plt.show()

```


![png](output_13_0.png)



```python

```
