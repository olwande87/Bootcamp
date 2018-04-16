

```python
# Dependencies
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

```


```python
# Import our data into pandas from CSV
city_data = pd.read_csv('raw_data/city_data.csv')
riders_data = pd.read_csv('raw_data/ride_data.csv')
```


```python
city_data = pd.DataFrame(city_data)
riders_data = pd.DataFrame(riders_data)
```


```python
df = pd.merge(city_data, riders_data, on='city', how='left')

df = df.groupby(['city', "type"])
df = pd.DataFrame(round(df.mean(),2))

df = df.reset_index()

df.head()
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
      <th>city</th>
      <th>type</th>
      <th>driver_count</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>Urban</td>
      <td>21.0</td>
      <td>23.93</td>
      <td>5.351586e+12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>Urban</td>
      <td>67.0</td>
      <td>20.61</td>
      <td>3.536678e+12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>Suburban</td>
      <td>16.0</td>
      <td>37.32</td>
      <td>4.195870e+12</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>Urban</td>
      <td>21.0</td>
      <td>23.62</td>
      <td>5.086800e+12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>Urban</td>
      <td>49.0</td>
      <td>21.98</td>
      <td>4.574788e+12</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Find out total number of rides per city
ridesPerCity = riders_data.copy()

ridesPerCity = ridesPerCity.groupby('city')['ride_id'].count()
ridesPerCity = pd.DataFrame(ridesPerCity)
ridesPerCity = ridesPerCity.rename(columns={'ride_id':'Total rides'})
ridesPerCity = ridesPerCity.reset_index()

ridesPerCity.head()
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
      <th>city</th>
      <th>Total rides</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.merge(df,ridesPerCity, on='city')

df = df.loc[:,['city', 'type','Total rides', 'driver_count', 'fare',]]

df = pd.DataFrame(df)
df.rename(columns={'city':'City',
                   'type':'Types',
                   'fare':'Average fare',
                   'driver_count':'Total drivers'
                  })



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
      <th>City</th>
      <th>Types</th>
      <th>Total rides</th>
      <th>Total drivers</th>
      <th>Average fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>Urban</td>
      <td>31</td>
      <td>21.0</td>
      <td>23.93</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>Urban</td>
      <td>26</td>
      <td>67.0</td>
      <td>20.61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>Suburban</td>
      <td>9</td>
      <td>16.0</td>
      <td>37.32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>Urban</td>
      <td>22</td>
      <td>21.0</td>
      <td>23.62</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>Urban</td>
      <td>19</td>
      <td>49.0</td>
      <td>21.98</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Arnoldview</td>
      <td>Urban</td>
      <td>31</td>
      <td>41.0</td>
      <td>25.11</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Campbellport</td>
      <td>Suburban</td>
      <td>15</td>
      <td>26.0</td>
      <td>33.71</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Carrollbury</td>
      <td>Suburban</td>
      <td>10</td>
      <td>4.0</td>
      <td>36.61</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Carrollfort</td>
      <td>Urban</td>
      <td>29</td>
      <td>55.0</td>
      <td>25.40</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Clarkstad</td>
      <td>Suburban</td>
      <td>12</td>
      <td>21.0</td>
      <td>31.05</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Conwaymouth</td>
      <td>Suburban</td>
      <td>11</td>
      <td>18.0</td>
      <td>34.59</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Davidtown</td>
      <td>Urban</td>
      <td>21</td>
      <td>73.0</td>
      <td>22.98</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Davistown</td>
      <td>Urban</td>
      <td>25</td>
      <td>25.0</td>
      <td>21.50</td>
    </tr>
    <tr>
      <th>13</th>
      <td>East Cherylfurt</td>
      <td>Suburban</td>
      <td>13</td>
      <td>9.0</td>
      <td>31.42</td>
    </tr>
    <tr>
      <th>14</th>
      <td>East Douglas</td>
      <td>Urban</td>
      <td>22</td>
      <td>12.0</td>
      <td>26.17</td>
    </tr>
    <tr>
      <th>15</th>
      <td>East Erin</td>
      <td>Urban</td>
      <td>28</td>
      <td>43.0</td>
      <td>24.48</td>
    </tr>
    <tr>
      <th>16</th>
      <td>East Jenniferchester</td>
      <td>Suburban</td>
      <td>19</td>
      <td>22.0</td>
      <td>32.60</td>
    </tr>
    <tr>
      <th>17</th>
      <td>East Leslie</td>
      <td>Rural</td>
      <td>11</td>
      <td>9.0</td>
      <td>33.66</td>
    </tr>
    <tr>
      <th>18</th>
      <td>East Stephen</td>
      <td>Rural</td>
      <td>10</td>
      <td>6.0</td>
      <td>39.05</td>
    </tr>
    <tr>
      <th>19</th>
      <td>East Troybury</td>
      <td>Rural</td>
      <td>7</td>
      <td>3.0</td>
      <td>33.24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Edwardsbury</td>
      <td>Urban</td>
      <td>27</td>
      <td>11.0</td>
      <td>26.88</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Erikport</td>
      <td>Rural</td>
      <td>8</td>
      <td>3.0</td>
      <td>30.04</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Eriktown</td>
      <td>Urban</td>
      <td>19</td>
      <td>15.0</td>
      <td>25.48</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Floresberg</td>
      <td>Suburban</td>
      <td>10</td>
      <td>7.0</td>
      <td>32.31</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Fosterside</td>
      <td>Urban</td>
      <td>24</td>
      <td>69.0</td>
      <td>23.03</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Hernandezshire</td>
      <td>Rural</td>
      <td>9</td>
      <td>10.0</td>
      <td>32.00</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Horneland</td>
      <td>Rural</td>
      <td>4</td>
      <td>8.0</td>
      <td>21.48</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Jacksonfort</td>
      <td>Rural</td>
      <td>6</td>
      <td>6.0</td>
      <td>32.01</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Jacobfort</td>
      <td>Urban</td>
      <td>31</td>
      <td>52.0</td>
      <td>24.78</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Jasonfort</td>
      <td>Suburban</td>
      <td>12</td>
      <td>25.0</td>
      <td>27.83</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>South Roy</td>
      <td>Urban</td>
      <td>22</td>
      <td>35.0</td>
      <td>26.03</td>
    </tr>
    <tr>
      <th>96</th>
      <td>South Shannonborough</td>
      <td>Suburban</td>
      <td>15</td>
      <td>9.0</td>
      <td>26.52</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Spencertown</td>
      <td>Urban</td>
      <td>26</td>
      <td>68.0</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Stevensport</td>
      <td>Rural</td>
      <td>5</td>
      <td>6.0</td>
      <td>31.95</td>
    </tr>
    <tr>
      <th>99</th>
      <td>Stewartview</td>
      <td>Urban</td>
      <td>30</td>
      <td>49.0</td>
      <td>21.61</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Swansonbury</td>
      <td>Urban</td>
      <td>34</td>
      <td>64.0</td>
      <td>27.46</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Thomastown</td>
      <td>Suburban</td>
      <td>24</td>
      <td>1.0</td>
      <td>30.31</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Tiffanyton</td>
      <td>Suburban</td>
      <td>13</td>
      <td>21.0</td>
      <td>28.51</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Torresshire</td>
      <td>Urban</td>
      <td>26</td>
      <td>70.0</td>
      <td>24.21</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Travisville</td>
      <td>Urban</td>
      <td>23</td>
      <td>37.0</td>
      <td>27.22</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Vickimouth</td>
      <td>Urban</td>
      <td>15</td>
      <td>13.0</td>
      <td>21.47</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Webstertown</td>
      <td>Suburban</td>
      <td>16</td>
      <td>26.0</td>
      <td>29.72</td>
    </tr>
    <tr>
      <th>107</th>
      <td>West Alexis</td>
      <td>Urban</td>
      <td>20</td>
      <td>47.0</td>
      <td>19.52</td>
    </tr>
    <tr>
      <th>108</th>
      <td>West Brandy</td>
      <td>Urban</td>
      <td>30</td>
      <td>12.0</td>
      <td>24.16</td>
    </tr>
    <tr>
      <th>109</th>
      <td>West Brittanyton</td>
      <td>Urban</td>
      <td>24</td>
      <td>9.0</td>
      <td>25.44</td>
    </tr>
    <tr>
      <th>110</th>
      <td>West Dawnfurt</td>
      <td>Urban</td>
      <td>29</td>
      <td>34.0</td>
      <td>22.33</td>
    </tr>
    <tr>
      <th>111</th>
      <td>West Evan</td>
      <td>Suburban</td>
      <td>12</td>
      <td>4.0</td>
      <td>27.01</td>
    </tr>
    <tr>
      <th>112</th>
      <td>West Jefferyfurt</td>
      <td>Urban</td>
      <td>21</td>
      <td>65.0</td>
      <td>21.07</td>
    </tr>
    <tr>
      <th>113</th>
      <td>West Kevintown</td>
      <td>Rural</td>
      <td>7</td>
      <td>5.0</td>
      <td>21.53</td>
    </tr>
    <tr>
      <th>114</th>
      <td>West Oscar</td>
      <td>Urban</td>
      <td>29</td>
      <td>11.0</td>
      <td>24.28</td>
    </tr>
    <tr>
      <th>115</th>
      <td>West Pamelaborough</td>
      <td>Suburban</td>
      <td>14</td>
      <td>27.0</td>
      <td>33.80</td>
    </tr>
    <tr>
      <th>116</th>
      <td>West Paulport</td>
      <td>Suburban</td>
      <td>17</td>
      <td>5.0</td>
      <td>33.28</td>
    </tr>
    <tr>
      <th>117</th>
      <td>West Peter</td>
      <td>Urban</td>
      <td>31</td>
      <td>61.0</td>
      <td>24.88</td>
    </tr>
    <tr>
      <th>118</th>
      <td>West Sydneyhaven</td>
      <td>Urban</td>
      <td>18</td>
      <td>70.0</td>
      <td>22.37</td>
    </tr>
    <tr>
      <th>119</th>
      <td>West Tony</td>
      <td>Suburban</td>
      <td>19</td>
      <td>17.0</td>
      <td>29.61</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Williamchester</td>
      <td>Suburban</td>
      <td>11</td>
      <td>26.0</td>
      <td>34.28</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Williamshire</td>
      <td>Urban</td>
      <td>31</td>
      <td>70.0</td>
      <td>26.99</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Wiseborough</td>
      <td>Urban</td>
      <td>19</td>
      <td>55.0</td>
      <td>22.68</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Yolandafurt</td>
      <td>Urban</td>
      <td>20</td>
      <td>7.0</td>
      <td>27.21</td>
    </tr>
    <tr>
      <th>124</th>
      <td>Zimmermanmouth</td>
      <td>Urban</td>
      <td>24</td>
      <td>45.0</td>
      <td>28.30</td>
    </tr>
  </tbody>
</table>
<p>125 rows × 5 columns</p>
</div>



# Bubble Plot 


```python
u = df.type.str.count(r'Urban')
s = df.type.str.count(r'Suburban')
r = df.type.str.count(r'Rural')
x = (df['Total rides'] * u) 
y = (df['fare'] * u)

plt.scatter(x, y, alpha=0.7, c='lightcoral', edgecolors='coral', s = df['driver_count']*10, label="Urban", linewidths=2)


x = (df['Total rides'] * s)
y = (df['fare'] * s)

plt.scatter(x, y, alpha=0.7, c='lightskyblue', edgecolors='skyblue', s = df['driver_count']*10, label='Suburban', linewidths=2)


x = (df['Total rides'] * r)
y = (df['fare'] * r)

plt.scatter(x, y, alpha=0.7, c='gold', edgecolors='orange', s = df['driver_count']*10, label='Rural', linewidths=2)

plt.ylim(5, 52)
plt.xlim(0, 40)
plt.ylabel('Average Fare ($)', fontsize = 12)
plt.xlabel('Total rides (Per city)', fontsize = 12)
plt.title("Pyber ride sharing data (2016)", fontsize = 15)
plt.grid()
plt.legend(loc="upper right", scatterpoints=1, fontsize=10, markerscale=0.5)
note = ("Note:\n Circle size correlates with driver count per city")
plt.text(45,35,note)
plt.show()
```


![png](output_7_0.png)


# % of Total Fares by City Type


```python

df1 = pd.merge(city_data, riders_data, on='city')
df1 = df1.groupby('type')['fare'].sum()
df1 = pd.DataFrame(df1)
df1['% of total fare'] = df1['fare']/df1['fare'].sum()*100
df1 = df1.reset_index()
types = df1['type']
total_fare = df1['% of total fare'] 
colors = ["gold", "lightcoral", "lightskyblue"]
explode = (0, 0, 0.05)
plt.pie(total_fare, explode=explode, labels=types, colors=colors, autopct="%1.1f%%", shadow=True, startangle=50)
```




    ([<matplotlib.patches.Wedge at 0x1e558f21278>,
      <matplotlib.patches.Wedge at 0x1e558f21c18>,
      <matplotlib.patches.Wedge at 0x1e558f295f8>],
     [Text(0.519068,0.969829,'Rural'),
      Text(-0.711316,0.839065,'Suburban'),
      Text(0.547779,-1.01116,'Urban')],
     [Text(0.283128,0.528998,'6.6%'),
      Text(-0.387991,0.457672,'31.4%'),
      Text(0.309614,-0.571523,'62.0%')])




![png](output_9_1.png)


# % of Total Rides by City Type


```python
df2 = df.groupby('type')['Total rides'].sum()

df2 = pd.DataFrame(df2)

df2 = df2.reset_index()
df2['% of total rides'] = df2['Total rides']/df2['Total rides'].sum()* 100

types = df2['type']
total_rides = df2['% of total rides'] # Data which we want to show as a pie chart
colors = ["gold", "lightcoral", "lightskyblue"]
explode = (0, 0, 0.05)
plt.pie(total_rides, explode=explode, labels=types, colors=colors, autopct="%1.1f%%", shadow=True, startangle=50)
```




    ([<matplotlib.patches.Wedge at 0x1e558e74fd0>,
      <matplotlib.patches.Wedge at 0x1e558e7f9b0>,
      <matplotlib.patches.Wedge at 0x1e558e87390>],
     [Text(0.558727,0.947536,'Rural'),
      Text(-0.48765,0.986001,'Suburban'),
      Text(0.333196,-1.10067,'Urban')],
     [Text(0.30476,0.516838,'5.3%'),
      Text(-0.265991,0.537819,'26.3%'),
      Text(0.188328,-0.622119,'68.4%')])




![png](output_11_1.png)


# % of Total Drivers by City Type


```python
df3 = df.groupby('type')['driver_count'].sum()
df3 = pd.DataFrame(df3)
df3 = df3.reset_index()
df3['% of total drivers'] = df3['driver_count']/df3['driver_count'].sum()* 100
types = df3['type']
total_drivers = df3['% of total drivers'] # Data which we want to show as a pie chart
colors = ["gold", "lightcoral", "lightskyblue"]
explode = (0, 0, 0.08)
plt.pie(total_drivers, explode=explode, labels=types, colors=colors, autopct="%1.1f%%", shadow=True, startangle=50)
```




    ([<matplotlib.patches.Wedge at 0x1e558ecf080>,
      <matplotlib.patches.Wedge at 0x1e558ecfa20>,
      <matplotlib.patches.Wedge at 0x1e558ed9400>],
     [Text(0.621388,0.907677,'Rural'),
      Text(-0.0979326,1.09563,'Suburban'),
      Text(-0.0102357,-1.17996,'Urban')],
     [Text(0.338939,0.495096,'3.1%'),
      Text(-0.0534178,0.597617,'18.8%'),
      Text(-0.00589855,-0.679974,'78.1%')])




![png](output_13_1.png)


# Observations 
Fares in rural areas are cheaper compared to suburban and urban areas
There are more drivers in urban areas than bothe rural and surbaban areas combined
The higher the number of rides, the higher the price
