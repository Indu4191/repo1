
Introducing Pandas Objects
At the very basic level, Pandas objects can be thought of as enhanced versions of NumPy structured arrays in which the rows and columns are identified with labels rather than simple integer indices. As we will see during the course of this chapter, Pandas provides a host of useful tools, methods, and functionality on top of the basic data structures, but nearly everything that follows will require an understanding of what these structures are. Thus, before we go any further, let's introduce these three fundamental Pandas data structures: the Series, DataFrame, and Index.

We will start our code sessions with the standard NumPy and Pandas imports:


import numpy as np
import pandas as pd


```python
import pandas

```


```python
fatalities= pandas.Series(index=['Australia','Albania',"Burkino Faso",'Libya','Monaco'],
                          data=[5.4,15.1,30,73.4,"-"]
                         )
```


```python
fatalities

```




    Australia        5.4
    Albania         15.1
    Burkino Faso      30
    Libya           73.4
    Monaco             -
    dtype: object




```python
fatalities
```




    Australia        5.4
    Albania         15.1
    Burkino Faso      30
    Libya           73.4
    Monaco             -
    dtype: object




```python
#fatalities =fatalities.replace("-",0)

```


```python
fatalities.min()
```




    5.4




```python
fatalities.max()
```




    '-'




```python
fatalities.idxmax()
```




    'Libya'




```python
fatalities.idxmin()
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-8-0a421a1b0d93> in <module>()
    ----> 1 fatalities.idxmin()
    

    //anaconda/lib/python2.7/site-packages/pandas/core/series.pyc in idxmin(self, axis, skipna, *args, **kwargs)
       1281         """
       1282         skipna = nv.validate_argmin_with_skipna(skipna, args, kwargs)
    -> 1283         i = nanops.nanargmin(_values_from_object(self), skipna=skipna)
       1284         if i == -1:
       1285             return np.nan


    //anaconda/lib/python2.7/site-packages/pandas/core/nanops.pyc in nanargmin(values, axis, skipna)
        473     """
        474     values, mask, dtype, _ = _get_values(values, skipna, fill_value_typ='+inf',
    --> 475                                          isfinite=True)
        476     result = values.argmin(axis)
        477     result = _maybe_arg_null_out(result, axis, mask, skipna)


    //anaconda/lib/python2.7/site-packages/pandas/core/nanops.pyc in _get_values(values, skipna, fill_value, fill_value_typ, isfinite, copy)
        181     values = _values_from_object(values)
        182     if isfinite:
    --> 183         mask = _isfinite(values)
        184     else:
        185         mask = isnull(values)


    //anaconda/lib/python2.7/site-packages/pandas/core/nanops.pyc in _isfinite(values)
        224             is_integer_dtype(values) or is_bool_dtype(values)):
        225         return ~np.isfinite(values)
    --> 226     return ~np.isfinite(values.astype('float64'))
        227 
        228 


    ValueError: could not convert string to float: -



```python
fatalities.dtype

```




    dtype('O')




```python
fatalities.astype(float)
```




    Australia        5.4
    Albania         15.1
    Burkino Faso    30.0
    Libya           73.4
    Monaco           0.0
    dtype: float64




```python
fatalities.astype(int)
```




    Australia        5
    Albania         15
    Burkino Faso    30
    Libya           73
    Monaco           0
    dtype: int64




```python
fatalities3 = fatalities[fatalities>0]
```


```python
population = pandas.Series(index=['Australia','Burkino Faso','Libya'],
                           data=[24000000,19000000,63000000])
```


```python
population
```




    Australia       24000000
    Burkino Faso    19000000
    Libya           63000000
    dtype: int64




```python
population.replace(63000000,6300000)
```




    Australia       24000000
    Burkino Faso    19000000
    Libya            6300000
    dtype: int64




```python
population*fatalities/100000
```




    Albania             NaN
    Australia        1296.0
    Burkino Faso     5700.0
    Libya           46242.0
    Monaco              NaN
    dtype: float64




```python
fatality_count = (population*fatalities/10000).astype(float)
```


```python
fatality_count
```




    Albania              NaN
    Australia        12960.0
    Burkino Faso     57000.0
    Libya           462420.0
    Monaco               NaN
    dtype: float64




```python
fatality_count.isnull()
```




    Albania          True
    Australia       False
    Burkino Faso    False
    Libya           False
    Monaco           True
    dtype: bool




```python
fatality_count[fatality_count.notnull()]
```




    Australia        12960.0
    Burkino Faso     57000.0
    Libya           462420.0
    dtype: float64




```python
data= pandas.read_html('https://en.wikipedia.org/wiki/List_of_countries_and_dependencies_by_population',
                       header=0,
                       index_col=1
   
)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-2-e0c49b5cceb1> in <module>()
    ----> 1 data= pandas.read_html('https://en.wikipedia.org/wiki/List_of_countries_and_dependencies_by_population',
          2                        header=0,
          3                        index_col=1
          4 
          5 )


    NameError: name 'pandas' is not defined



```python
!pip install html5lib
!pip install BeautifulSoup4
```

    Requirement already satisfied: html5lib in /anaconda/lib/python2.7/site-packages
    Requirement already satisfied: setuptools>=18.5 in /anaconda/lib/python2.7/site-packages/setuptools-27.2.0-py2.7.egg (from html5lib)
    Requirement already satisfied: webencodings in /anaconda/lib/python2.7/site-packages (from html5lib)
    Requirement already satisfied: six in /anaconda/lib/python2.7/site-packages (from html5lib)
    Requirement already satisfied: BeautifulSoup4 in /anaconda/lib/python2.7/site-packages



```python
data= pandas.read_html('https://en.wikipedia.org/wiki/List_of_countries_and_dependencies_by_population',
                       header=0,
                       index_col=1
   
)
```


```python
data[1]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Population</th>
      <th>Date</th>
      <th>% of world population</th>
      <th>Source</th>
    </tr>
    <tr>
      <th>Country (or dependent territory)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>China[Note 2]</th>
      <td>1</td>
      <td>1381450000</td>
      <td>February 9, 2017</td>
      <td>18.5%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>India</th>
      <td>2</td>
      <td>1311740000</td>
      <td>February 9, 2017</td>
      <td>17.5%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>United States[Note 3]</th>
      <td>3</td>
      <td>324503000</td>
      <td>February 9, 2017</td>
      <td>4.34%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>Indonesia</th>
      <td>4</td>
      <td>260581000</td>
      <td>July 1, 2016</td>
      <td>3.48%</td>
      <td>UN Projection</td>
    </tr>
    <tr>
      <th>Brazil</th>
      <td>5</td>
      <td>207074000</td>
      <td>February 9, 2017</td>
      <td>2.77%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>Pakistan</th>
      <td>6</td>
      <td>196310000</td>
      <td>February 9, 2017</td>
      <td>2.62%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>Nigeria</th>
      <td>7</td>
      <td>186988000</td>
      <td>July 1, 2016</td>
      <td>2.5%</td>
      <td>UN Projection</td>
    </tr>
    <tr>
      <th>Bangladesh</th>
      <td>8</td>
      <td>161913000</td>
      <td>February 9, 2017</td>
      <td>2.16%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>Russia[Note 4]</th>
      <td>9</td>
      <td>146838993</td>
      <td>January 1, 2017</td>
      <td>1.96%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>Japan</th>
      <td>10</td>
      <td>126860000</td>
      <td>January 1, 2017</td>
      <td>1.70%</td>
      <td>Monthly provisional estimate</td>
    </tr>
    <tr>
      <th>Mexico</th>
      <td>11</td>
      <td>122273000</td>
      <td>July 1, 2016</td>
      <td>1.63%</td>
      <td>Official projection</td>
    </tr>
    <tr>
      <th>Philippines</th>
      <td>12</td>
      <td>103743000</td>
      <td>February 9, 2017</td>
      <td>1.39%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>Ethiopia</th>
      <td>13</td>
      <td>101853000</td>
      <td>July 1, 2016</td>
      <td>1.36%</td>
      <td>UN Projection</td>
    </tr>
    <tr>
      <th>Vietnam</th>
      <td>14</td>
      <td>92700000</td>
      <td>July 1, 2016</td>
      <td>1.24%</td>
      <td>Annual official projection</td>
    </tr>
    <tr>
      <th>Egypt</th>
      <td>15</td>
      <td>92448000</td>
      <td>February 9, 2017</td>
      <td>1.24%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>Democratic Republic of the Congo</th>
      <td>16</td>
      <td>82243000</td>
      <td>July 1, 2017</td>
      <td>1.10%</td>
      <td>UN Projection</td>
    </tr>
    <tr>
      <th>Germany</th>
      <td>17</td>
      <td>82175700</td>
      <td>December 31, 2015</td>
      <td>1.10%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>Iran</th>
      <td>18</td>
      <td>79897400</td>
      <td>February 9, 2017</td>
      <td>1.07%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>Turkey</th>
      <td>19</td>
      <td>79814871</td>
      <td>December 31, 2016</td>
      <td>1.07%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Thailand</th>
      <td>20</td>
      <td>68147000</td>
      <td>July 1, 2016</td>
      <td>0.91%</td>
      <td>UN Projection</td>
    </tr>
    <tr>
      <th>France[Note 5]</th>
      <td>21</td>
      <td>66984000</td>
      <td>January 1, 2017</td>
      <td>0.9%</td>
      <td>Monthly official estimate</td>
    </tr>
    <tr>
      <th>United Kingdom</th>
      <td>22</td>
      <td>65110000</td>
      <td>July 1, 2015</td>
      <td>0.87%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Italy</th>
      <td>23</td>
      <td>60589940</td>
      <td>September 30, 2016</td>
      <td>0.81%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>South Africa</th>
      <td>24</td>
      <td>55908000</td>
      <td>July 1, 2016</td>
      <td>0.75%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Tanzania[Note 6]</th>
      <td>25</td>
      <td>55155000</td>
      <td>July 1, 2016</td>
      <td>0.74%</td>
      <td>UN projection</td>
    </tr>
    <tr>
      <th>Myanmar</th>
      <td>26</td>
      <td>54363000</td>
      <td>July 1, 2016</td>
      <td>0.73%</td>
      <td>UN projection</td>
    </tr>
    <tr>
      <th>South Korea</th>
      <td>27</td>
      <td>50801405</td>
      <td>July 1, 2016</td>
      <td>0.68%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Colombia</th>
      <td>28</td>
      <td>49080500</td>
      <td>February 9, 2017</td>
      <td>0.656%</td>
      <td>Official population clock</td>
    </tr>
    <tr>
      <th>Kenya</th>
      <td>29</td>
      <td>47251000</td>
      <td>July 1, 2016</td>
      <td>0.63%</td>
      <td>UN projection</td>
    </tr>
    <tr>
      <th>Spain</th>
      <td>30</td>
      <td>46812000</td>
      <td>July 1, 2016</td>
      <td>0.63%</td>
      <td>Official estimate</td>
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
      <th>Saint Kitts and Nevis</th>
      <td>189</td>
      <td>46204</td>
      <td>May 15, 2011</td>
      <td>0.00062%</td>
      <td>2011 census result</td>
    </tr>
    <tr>
      <th>Sint Maarten (Netherlands)</th>
      <td>–</td>
      <td>39410</td>
      <td>January 1, 2016</td>
      <td>0.00053%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>Monaco</th>
      <td>190</td>
      <td>38400</td>
      <td>December 31, 2015</td>
      <td>0.00051%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Liechtenstein</th>
      <td>191</td>
      <td>37686</td>
      <td>June 30, 2016</td>
      <td>0.0005%</td>
      <td>Semi annual official estimate</td>
    </tr>
    <tr>
      <th>Saint-Martin (France)</th>
      <td>–</td>
      <td>36457</td>
      <td>January 1, 2015</td>
      <td>0.00049%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Gibraltar (UK)</th>
      <td>–</td>
      <td>33140</td>
      <td>December 31, 2014</td>
      <td>0.00044%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>San Marino</th>
      <td>192</td>
      <td>33121</td>
      <td>September 30, 2016</td>
      <td>0.00044%</td>
      <td>Monthly official estimate</td>
    </tr>
    <tr>
      <th>Turks and Caicos Islands (UK)</th>
      <td>–</td>
      <td>31458</td>
      <td>January 25, 2012</td>
      <td>0.00042%</td>
      <td>2012 census result</td>
    </tr>
    <tr>
      <th>British Virgin Islands (UK)</th>
      <td>–</td>
      <td>28514</td>
      <td>July 1, 2013</td>
      <td>0.00038%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>Bonaire (Netherlands)</th>
      <td>–</td>
      <td>18905</td>
      <td>January 1, 2015</td>
      <td>0.00025%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>Cook Islands (NZ)</th>
      <td>–</td>
      <td>18100</td>
      <td>March 1, 2016</td>
      <td>0.00024%</td>
      <td>Official quarterly estimate</td>
    </tr>
    <tr>
      <th>Palau</th>
      <td>193</td>
      <td>17950</td>
      <td>July 1, 2015</td>
      <td>0.00024%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Anguilla (UK)</th>
      <td>–</td>
      <td>13452</td>
      <td>May 11, 2011</td>
      <td>0.00018%</td>
      <td>Preliminary 2011 census result</td>
    </tr>
    <tr>
      <th>Wallis and Futuna (France)</th>
      <td>–</td>
      <td>11750</td>
      <td>July 1, 2015</td>
      <td>0.00016%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Tuvalu</th>
      <td>194</td>
      <td>10640</td>
      <td>November 4, 2012</td>
      <td>0.00014%</td>
      <td>2012 census result</td>
    </tr>
    <tr>
      <th>Nauru</th>
      <td>195</td>
      <td>10084</td>
      <td>October 30, 2011</td>
      <td>0.00013%</td>
      <td>2011 census result</td>
    </tr>
    <tr>
      <th>Saint Barthélemy (France)</th>
      <td>–</td>
      <td>9417</td>
      <td>January 1, 2015</td>
      <td>0.00013%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Saint Pierre and Miquelon (France)</th>
      <td>–</td>
      <td>6286</td>
      <td>January 1, 2015</td>
      <td>0.000084%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Saint Helena, Ascension and Tristan da Cunha (UK)</th>
      <td>–</td>
      <td>5633</td>
      <td>February 7, 2016</td>
      <td>0.000075%</td>
      <td>2016 census result</td>
    </tr>
    <tr>
      <th>Montserrat (UK)</th>
      <td>–</td>
      <td>4922</td>
      <td>May 12, 2011</td>
      <td>0.000066%</td>
      <td>2011 census result</td>
    </tr>
    <tr>
      <th>Sint Eustatius (Netherlands)</th>
      <td>–</td>
      <td>3877</td>
      <td>January 1, 2015</td>
      <td>0.000052%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>Falkland Islands (UK)</th>
      <td>–</td>
      <td>2563</td>
      <td>April 15, 2012</td>
      <td>0.000034%</td>
      <td>2012 census result</td>
    </tr>
    <tr>
      <th>Norfolk Island (Australia)</th>
      <td>–</td>
      <td>2302</td>
      <td>August 9, 2011</td>
      <td>0.000031%</td>
      <td>2011 census result</td>
    </tr>
    <tr>
      <th>Christmas Island (Australia)</th>
      <td>–</td>
      <td>2072</td>
      <td>August 9, 2011</td>
      <td>0.000028%</td>
      <td>2011 census result</td>
    </tr>
    <tr>
      <th>Saba (Netherlands)</th>
      <td>–</td>
      <td>1811</td>
      <td>January 1, 2015</td>
      <td>0.000024%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>Niue (NZ)</th>
      <td>–</td>
      <td>1470</td>
      <td>July 1, 2015</td>
      <td>0.00002%</td>
      <td>Annual official estimate</td>
    </tr>
    <tr>
      <th>Tokelau (NZ)</th>
      <td>–</td>
      <td>1411</td>
      <td>October 18, 2011</td>
      <td>0.000019%</td>
      <td>2011 census result</td>
    </tr>
    <tr>
      <th>Vatican City</th>
      <td>196</td>
      <td>842</td>
      <td>January 1, 2014</td>
      <td>0.000011%</td>
      <td>Official estimate</td>
    </tr>
    <tr>
      <th>Cocos (Keeling) Islands (Australia)</th>
      <td>–</td>
      <td>550</td>
      <td>August 9, 2011</td>
      <td>0.0000073%</td>
      <td>2011 census result</td>
    </tr>
    <tr>
      <th>Pitcairn Islands (UK)</th>
      <td>–</td>
      <td>57</td>
      <td>July 1, 2014</td>
      <td>0.00000075%</td>
      <td>Official estimate</td>
    </tr>
  </tbody>
</table>
<p>243 rows × 5 columns</p>
</div>




```python
data[0]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
    </tr>
    <tr>
      <th>This article needs additional citations for verification. Please help improve this article by adding citations to reliable sources. Unsourced material may be challenged and removed. (January 2017) (Learn how and when to remove this template message)</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
pandas.read_csv('/Users/Mahendra/Desktop/GA/SYD_DAT_7/data/sydtrains.csv')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LINE</th>
      <th>STATION</th>
      <th>YEAR</th>
      <th>SURVEY_DATE_USED</th>
      <th>WHETHER_SURVEYED</th>
      <th>IN_0200_0600</th>
      <th>OUT_0200_0600</th>
      <th>IN_0600_0930</th>
      <th>OUT_0600_0930</th>
      <th>IN_0930_1500</th>
      <th>OUT_0930_1500</th>
      <th>IN_1500_1830</th>
      <th>OUT_1500_1830</th>
      <th>IN_1830_0200</th>
      <th>OUT_1830_0200</th>
      <th>IN_24_HOURS</th>
      <th>OUT_24_HOURS</th>
      <th>RANK</th>
      <th>STATION_SORT_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CBD</td>
      <td>Central</td>
      <td>2014</td>
      <td>6/05/2014</td>
      <td>Yes</td>
      <td>940</td>
      <td>920</td>
      <td>10390</td>
      <td>42070</td>
      <td>21400</td>
      <td>30150</td>
      <td>45370</td>
      <td>16270</td>
      <td>19010</td>
      <td>7700</td>
      <td>97,110</td>
      <td>97,110</td>
      <td>1</td>
      <td>101</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CBD</td>
      <td>Town Hall</td>
      <td>2014</td>
      <td>7/05/2014</td>
      <td>Yes</td>
      <td>880</td>
      <td>880</td>
      <td>6380</td>
      <td>43210</td>
      <td>16930</td>
      <td>23550</td>
      <td>43670</td>
      <td>15270</td>
      <td>21760</td>
      <td>6720</td>
      <td>89,620</td>
      <td>89,620</td>
      <td>2</td>
      <td>102</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CBD</td>
      <td>Wynyard</td>
      <td>2014</td>
      <td>20/05/2014</td>
      <td>Yes</td>
      <td>590</td>
      <td>570</td>
      <td>4710</td>
      <td>39620</td>
      <td>9200</td>
      <td>11180</td>
      <td>35010</td>
      <td>6510</td>
      <td>10690</td>
      <td>2330</td>
      <td>60,200</td>
      <td>60,200</td>
      <td>3</td>
      <td>103</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Western</td>
      <td>Parramatta</td>
      <td>2014</td>
      <td>19/11/2014</td>
      <td>Yes</td>
      <td>660</td>
      <td>330</td>
      <td>7790</td>
      <td>13700</td>
      <td>7400</td>
      <td>7020</td>
      <td>14960</td>
      <td>8660</td>
      <td>4150</td>
      <td>5260</td>
      <td>34,960</td>
      <td>34,960</td>
      <td>4</td>
      <td>905</td>
    </tr>
    <tr>
      <th>4</th>
      <td>North Shore</td>
      <td>North Sydney</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>270</td>
      <td>270</td>
      <td>2340</td>
      <td>18340</td>
      <td>4450</td>
      <td>5590</td>
      <td>17760</td>
      <td>3050</td>
      <td>3790</td>
      <td>1360</td>
      <td>28,610</td>
      <td>28,610</td>
      <td>5</td>
      <td>1402</td>
    </tr>
    <tr>
      <th>5</th>
      <td>CBD</td>
      <td>Redfern</td>
      <td>2014</td>
      <td>1/05/2014</td>
      <td>Yes</td>
      <td>250</td>
      <td>250</td>
      <td>3910</td>
      <td>10690</td>
      <td>5350</td>
      <td>8170</td>
      <td>12510</td>
      <td>4250</td>
      <td>3660</td>
      <td>2320</td>
      <td>25,680</td>
      <td>25,680</td>
      <td>6</td>
      <td>108</td>
    </tr>
    <tr>
      <th>6</th>
      <td>North Shore</td>
      <td>Chatswood</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>200</td>
      <td>5030</td>
      <td>8200</td>
      <td>4710</td>
      <td>4930</td>
      <td>9430</td>
      <td>6000</td>
      <td>2820</td>
      <td>2860</td>
      <td>22,200</td>
      <td>22,200</td>
      <td>7</td>
      <td>1407</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Eastern Suburbs</td>
      <td>Bondi Junction</td>
      <td>2014</td>
      <td>2/09/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>210</td>
      <td>10150</td>
      <td>4350</td>
      <td>4370</td>
      <td>4600</td>
      <td>5240</td>
      <td>8330</td>
      <td>1910</td>
      <td>4380</td>
      <td>21,880</td>
      <td>21,880</td>
      <td>8</td>
      <td>203</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Inner West</td>
      <td>Strathfield</td>
      <td>2014</td>
      <td>9/09/2014</td>
      <td>Yes</td>
      <td>190</td>
      <td>190</td>
      <td>7900</td>
      <td>4050</td>
      <td>4980</td>
      <td>3950</td>
      <td>5540</td>
      <td>7230</td>
      <td>2100</td>
      <td>5280</td>
      <td>20,710</td>
      <td>20,710</td>
      <td>9</td>
      <td>1110</td>
    </tr>
    <tr>
      <th>9</th>
      <td>CBD</td>
      <td>Circular Quay</td>
      <td>2014</td>
      <td>22/05/2014</td>
      <td>Yes</td>
      <td>200</td>
      <td>200</td>
      <td>2110</td>
      <td>8750</td>
      <td>4340</td>
      <td>6710</td>
      <td>9910</td>
      <td>3380</td>
      <td>4070</td>
      <td>1600</td>
      <td>20,630</td>
      <td>20,630</td>
      <td>10</td>
      <td>104</td>
    </tr>
    <tr>
      <th>10</th>
      <td>CBD</td>
      <td>Martin Place</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>200</td>
      <td>190</td>
      <td>590</td>
      <td>14250</td>
      <td>2900</td>
      <td>3420</td>
      <td>12340</td>
      <td>1790</td>
      <td>4600</td>
      <td>970</td>
      <td>20,630</td>
      <td>20,630</td>
      <td>10</td>
      <td>107</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Illawarra</td>
      <td>Hurstville</td>
      <td>2014</td>
      <td>28/05/2014</td>
      <td>Yes</td>
      <td>160</td>
      <td>160</td>
      <td>7790</td>
      <td>2200</td>
      <td>5230</td>
      <td>4180</td>
      <td>3840</td>
      <td>7450</td>
      <td>1270</td>
      <td>4290</td>
      <td>18,290</td>
      <td>18,290</td>
      <td>12</td>
      <td>310</td>
    </tr>
    <tr>
      <th>12</th>
      <td>North Shore</td>
      <td>St Leonards</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>170</td>
      <td>170</td>
      <td>3420</td>
      <td>8900</td>
      <td>3090</td>
      <td>3310</td>
      <td>8780</td>
      <td>3230</td>
      <td>2130</td>
      <td>1990</td>
      <td>17,590</td>
      <td>17,590</td>
      <td>13</td>
      <td>1405</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Western</td>
      <td>Blacktown</td>
      <td>2014</td>
      <td>14/10/2014</td>
      <td>Yes</td>
      <td>190</td>
      <td>210</td>
      <td>6460</td>
      <td>3370</td>
      <td>4270</td>
      <td>3450</td>
      <td>4600</td>
      <td>7090</td>
      <td>1040</td>
      <td>2450</td>
      <td>16,560</td>
      <td>16,560</td>
      <td>14</td>
      <td>911</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Inner West</td>
      <td>Burwood</td>
      <td>2014</td>
      <td>9/09/2014</td>
      <td>Yes</td>
      <td>140</td>
      <td>140</td>
      <td>4850</td>
      <td>2930</td>
      <td>4470</td>
      <td>3670</td>
      <td>4020</td>
      <td>5060</td>
      <td>1510</td>
      <td>3190</td>
      <td>14,990</td>
      <td>14,990</td>
      <td>15</td>
      <td>1109</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Eastern Suburbs</td>
      <td>Kings Cross</td>
      <td>2014</td>
      <td>3/09/2013</td>
      <td>No</td>
      <td>120</td>
      <td>120</td>
      <td>3690</td>
      <td>3820</td>
      <td>3110</td>
      <td>2670</td>
      <td>4400</td>
      <td>3640</td>
      <td>1550</td>
      <td>2620</td>
      <td>12,870</td>
      <td>12,870</td>
      <td>16</td>
      <td>201</td>
    </tr>
    <tr>
      <th>16</th>
      <td>North Shore</td>
      <td>Hornsby</td>
      <td>2014</td>
      <td>14/05/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>100</td>
      <td>5550</td>
      <td>2260</td>
      <td>3060</td>
      <td>2390</td>
      <td>2750</td>
      <td>5200</td>
      <td>680</td>
      <td>2290</td>
      <td>12,250</td>
      <td>12,250</td>
      <td>17</td>
      <td>1417</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Inner West</td>
      <td>Ashfield</td>
      <td>2014</td>
      <td>4/09/2013</td>
      <td>No</td>
      <td>100</td>
      <td>100</td>
      <td>5090</td>
      <td>2010</td>
      <td>2690</td>
      <td>2220</td>
      <td>2500</td>
      <td>4500</td>
      <td>820</td>
      <td>2370</td>
      <td>11,200</td>
      <td>11,200</td>
      <td>18</td>
      <td>1107</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Illawarra</td>
      <td>Kogarah</td>
      <td>2014</td>
      <td>19/06/2014</td>
      <td>Yes</td>
      <td>90</td>
      <td>100</td>
      <td>4000</td>
      <td>2970</td>
      <td>3150</td>
      <td>2320</td>
      <td>3210</td>
      <td>3450</td>
      <td>650</td>
      <td>2260</td>
      <td>11,100</td>
      <td>11,100</td>
      <td>19</td>
      <td>307</td>
    </tr>
    <tr>
      <th>19</th>
      <td>South</td>
      <td>Lidcombe</td>
      <td>2014</td>
      <td>5/06/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>4570</td>
      <td>1620</td>
      <td>2870</td>
      <td>2210</td>
      <td>2550</td>
      <td>3900</td>
      <td>820</td>
      <td>3080</td>
      <td>10,910</td>
      <td>10,910</td>
      <td>20</td>
      <td>702</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Western</td>
      <td>Auburn</td>
      <td>2014</td>
      <td>19/11/2014</td>
      <td>Yes</td>
      <td>130</td>
      <td>60</td>
      <td>3880</td>
      <td>1790</td>
      <td>3220</td>
      <td>2900</td>
      <td>2700</td>
      <td>3890</td>
      <td>960</td>
      <td>2260</td>
      <td>10,890</td>
      <td>10,890</td>
      <td>21</td>
      <td>901</td>
    </tr>
    <tr>
      <th>21</th>
      <td>CBD</td>
      <td>Museum</td>
      <td>2014</td>
      <td>22/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>800</td>
      <td>6290</td>
      <td>2220</td>
      <td>2340</td>
      <td>5980</td>
      <td>1230</td>
      <td>1760</td>
      <td>910</td>
      <td>10,860</td>
      <td>10,860</td>
      <td>22</td>
      <td>106</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Northern via Strathfield</td>
      <td>Epping</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>170</td>
      <td>90</td>
      <td>5700</td>
      <td>1580</td>
      <td>2130</td>
      <td>1600</td>
      <td>1970</td>
      <td>4920</td>
      <td>600</td>
      <td>2380</td>
      <td>10,570</td>
      <td>10,570</td>
      <td>23</td>
      <td>1208</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Northern via Macquarie Park</td>
      <td>Macquarie University</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>1310</td>
      <td>3220</td>
      <td>2570</td>
      <td>3940</td>
      <td>4530</td>
      <td>1970</td>
      <td>1580</td>
      <td>860</td>
      <td>10,090</td>
      <td>10,090</td>
      <td>24</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>24</th>
      <td>CBD</td>
      <td>St James</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>90</td>
      <td>780</td>
      <td>5880</td>
      <td>1900</td>
      <td>2160</td>
      <td>5630</td>
      <td>1120</td>
      <td>1440</td>
      <td>580</td>
      <td>9,850</td>
      <td>9,850</td>
      <td>25</td>
      <td>105</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Illawarra</td>
      <td>Rockdale</td>
      <td>2014</td>
      <td>28/05/2014</td>
      <td>Yes</td>
      <td>90</td>
      <td>90</td>
      <td>4660</td>
      <td>780</td>
      <td>2610</td>
      <td>2170</td>
      <td>1670</td>
      <td>3770</td>
      <td>610</td>
      <td>2820</td>
      <td>9,640</td>
      <td>9,640</td>
      <td>26</td>
      <td>306</td>
    </tr>
    <tr>
      <th>26</th>
      <td>South</td>
      <td>Cabramatta</td>
      <td>2014</td>
      <td>11/06/2014</td>
      <td>Yes</td>
      <td>150</td>
      <td>80</td>
      <td>3780</td>
      <td>1070</td>
      <td>2760</td>
      <td>2820</td>
      <td>1750</td>
      <td>3760</td>
      <td>290</td>
      <td>1000</td>
      <td>8,730</td>
      <td>8,730</td>
      <td>27</td>
      <td>708</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Bankstown</td>
      <td>Bankstown</td>
      <td>2014</td>
      <td>13/05/2014</td>
      <td>Yes</td>
      <td>80</td>
      <td>60</td>
      <td>3010</td>
      <td>1810</td>
      <td>2400</td>
      <td>2570</td>
      <td>2840</td>
      <td>3130</td>
      <td>300</td>
      <td>1050</td>
      <td>8,630</td>
      <td>8,630</td>
      <td>28</td>
      <td>612</td>
    </tr>
    <tr>
      <th>28</th>
      <td>South</td>
      <td>Liverpool</td>
      <td>2014</td>
      <td>4/06/2014</td>
      <td>Yes</td>
      <td>150</td>
      <td>80</td>
      <td>2510</td>
      <td>2270</td>
      <td>2510</td>
      <td>2260</td>
      <td>2610</td>
      <td>2650</td>
      <td>570</td>
      <td>1100</td>
      <td>8,350</td>
      <td>8,350</td>
      <td>29</td>
      <td>710</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Illawarra</td>
      <td>Sutherland</td>
      <td>2014</td>
      <td>12/06/2014</td>
      <td>Yes</td>
      <td>140</td>
      <td>70</td>
      <td>4260</td>
      <td>1460</td>
      <td>1920</td>
      <td>1650</td>
      <td>1750</td>
      <td>3900</td>
      <td>250</td>
      <td>1240</td>
      <td>8,320</td>
      <td>8,320</td>
      <td>30</td>
      <td>316</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3337</th>
      <td>Newcastle</td>
      <td>Newcastle</td>
      <td>2004</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>50</td>
      <td>20</td>
      <td>260</td>
      <td>320</td>
      <td>720</td>
      <td>850</td>
      <td>430</td>
      <td>200</td>
      <td>70</td>
      <td>140</td>
      <td>1530</td>
      <td>1530</td>
      <td>123</td>
      <td>1917</td>
    </tr>
    <tr>
      <th>3338</th>
      <td>Hunter</td>
      <td>Waratah</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>100</td>
      <td>100</td>
      <td>150</td>
      <td>150</td>
      <td>120</td>
      <td>120</td>
      <td>40</td>
      <td>30</td>
      <td>410</td>
      <td>410</td>
      <td>193</td>
      <td>2001</td>
    </tr>
    <tr>
      <th>3339</th>
      <td>Hunter</td>
      <td>Warabrook (University)</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>250</td>
      <td>120</td>
      <td>200</td>
      <td>300</td>
      <td>70</td>
      <td>90</td>
      <td>20</td>
      <td>540</td>
      <td>540</td>
      <td>185</td>
      <td>2002</td>
    </tr>
    <tr>
      <th>3340</th>
      <td>Hunter</td>
      <td>Sandgate</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>40</td>
      <td>267</td>
      <td>2003</td>
    </tr>
    <tr>
      <th>3341</th>
      <td>Hunter</td>
      <td>Hexham</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>20</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>50</td>
      <td>50</td>
      <td>258</td>
      <td>2004</td>
    </tr>
    <tr>
      <th>3342</th>
      <td>Hunter</td>
      <td>Tarro</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>278</td>
      <td>2005</td>
    </tr>
    <tr>
      <th>3343</th>
      <td>Hunter</td>
      <td>Beresfield</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>180</td>
      <td>80</td>
      <td>140</td>
      <td>110</td>
      <td>70</td>
      <td>180</td>
      <td>10</td>
      <td>40</td>
      <td>400</td>
      <td>400</td>
      <td>195</td>
      <td>2006</td>
    </tr>
    <tr>
      <th>3344</th>
      <td>Hunter</td>
      <td>Thornton</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>150</td>
      <td>20</td>
      <td>60</td>
      <td>80</td>
      <td>50</td>
      <td>150</td>
      <td>10</td>
      <td>20</td>
      <td>270</td>
      <td>270</td>
      <td>211</td>
      <td>2007</td>
    </tr>
    <tr>
      <th>3345</th>
      <td>Hunter</td>
      <td>Metford</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>100</td>
      <td>20</td>
      <td>50</td>
      <td>50</td>
      <td>20</td>
      <td>90</td>
      <td>10</td>
      <td>10</td>
      <td>180</td>
      <td>180</td>
      <td>231</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>3346</th>
      <td>Hunter</td>
      <td>Victoria Street</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>190</td>
      <td>50</td>
      <td>120</td>
      <td>120</td>
      <td>70</td>
      <td>180</td>
      <td>20</td>
      <td>50</td>
      <td>400</td>
      <td>400</td>
      <td>195</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>3347</th>
      <td>Hunter</td>
      <td>East Maitland</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>50</td>
      <td>50</td>
      <td>40</td>
      <td>50</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>120</td>
      <td>120</td>
      <td>240</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>3348</th>
      <td>Hunter</td>
      <td>High Street</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>30</td>
      <td>40</td>
      <td>40</td>
      <td>40</td>
      <td>30</td>
      <td>40</td>
      <td>10</td>
      <td>0</td>
      <td>110</td>
      <td>110</td>
      <td>248</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>3349</th>
      <td>Hunter</td>
      <td>Maitland</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>10</td>
      <td>190</td>
      <td>100</td>
      <td>180</td>
      <td>210</td>
      <td>150</td>
      <td>180</td>
      <td>20</td>
      <td>40</td>
      <td>540</td>
      <td>540</td>
      <td>185</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>3350</th>
      <td>Hunter</td>
      <td>Telarah</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>110</td>
      <td>30</td>
      <td>60</td>
      <td>60</td>
      <td>50</td>
      <td>120</td>
      <td>20</td>
      <td>30</td>
      <td>240</td>
      <td>240</td>
      <td>218</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>3351</th>
      <td>Hunter</td>
      <td>Mindaribba</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>3352</th>
      <td>Hunter</td>
      <td>Paterson</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>3353</th>
      <td>Hunter</td>
      <td>Martins Creek</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>3354</th>
      <td>Hunter</td>
      <td>Hilldale</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>3355</th>
      <td>Hunter</td>
      <td>Wallarobba</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3356</th>
      <td>Hunter</td>
      <td>Wirragulla</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>3357</th>
      <td>Hunter</td>
      <td>Dungog</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>20</td>
      <td>10</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>40</td>
      <td>40</td>
      <td>267</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>3358</th>
      <td>Hunter</td>
      <td>Lochinvar</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>3359</th>
      <td>Hunter</td>
      <td>Allandale</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2022</td>
    </tr>
    <tr>
      <th>3360</th>
      <td>Hunter</td>
      <td>Greta</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2023</td>
    </tr>
    <tr>
      <th>3361</th>
      <td>Hunter</td>
      <td>Branxton</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>278</td>
      <td>2024</td>
    </tr>
    <tr>
      <th>3362</th>
      <td>Hunter</td>
      <td>Belford</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2025</td>
    </tr>
    <tr>
      <th>3363</th>
      <td>Hunter</td>
      <td>Singleton</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>30</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>30</td>
      <td>10</td>
      <td>10</td>
      <td>50</td>
      <td>50</td>
      <td>258</td>
      <td>2026</td>
    </tr>
    <tr>
      <th>3364</th>
      <td>Hunter</td>
      <td>Muswellbrook</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>30</td>
      <td>10</td>
      <td>0</td>
      <td>40</td>
      <td>10</td>
      <td>20</td>
      <td>50</td>
      <td>50</td>
      <td>258</td>
      <td>2027</td>
    </tr>
    <tr>
      <th>3365</th>
      <td>Hunter</td>
      <td>Aberdeen</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2028</td>
    </tr>
    <tr>
      <th>3366</th>
      <td>Hunter</td>
      <td>Scone</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2029</td>
    </tr>
  </tbody>
</table>
<p>3367 rows × 19 columns</p>
</div>




```python
sydtrains= pandas.read_csv('/Users/Mahendra/Desktop/GA/SYD_DAT_7/data/sydtrains.csv',index_col=1, thousands=",")
```


```python
sydtrains.YEAR
```




    STATION
    Central                   2014
    Town Hall                 2014
    Wynyard                   2014
    Parramatta                2014
    North Sydney              2014
    Redfern                   2014
    Chatswood                 2014
    Bondi Junction            2014
    Strathfield               2014
    Circular Quay             2014
    Martin Place              2014
    Hurstville                2014
    St Leonards               2014
    Blacktown                 2014
    Burwood                   2014
    Kings Cross               2014
    Hornsby                   2014
    Ashfield                  2014
    Kogarah                   2014
    Lidcombe                  2014
    Auburn                    2014
    Museum                    2014
    Epping                    2014
    Macquarie University      2014
    St James                  2014
    Rockdale                  2014
    Cabramatta                2014
    Bankstown                 2014
    Liverpool                 2014
    Sutherland                2014
                              ... 
    Newcastle                 2004
    Waratah                   2004
    Warabrook (University)    2004
    Sandgate                  2004
    Hexham                    2004
    Tarro                     2004
    Beresfield                2004
    Thornton                  2004
    Metford                   2004
    Victoria Street           2004
    East Maitland             2004
    High Street               2004
    Maitland                  2004
    Telarah                   2004
    Mindaribba                2004
    Paterson                  2004
    Martins Creek             2004
    Hilldale                  2004
    Wallarobba                2004
    Wirragulla                2004
    Dungog                    2004
    Lochinvar                 2004
    Allandale                 2004
    Greta                     2004
    Branxton                  2004
    Belford                   2004
    Singleton                 2004
    Muswellbrook              2004
    Aberdeen                  2004
    Scone                     2004
    Name: YEAR, dtype: int64




```python
trains2014=sydtrains[sydtrains.YEAR==2014]
```


```python
trains2014
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LINE</th>
      <th>YEAR</th>
      <th>SURVEY_DATE_USED</th>
      <th>WHETHER_SURVEYED</th>
      <th>IN_0200_0600</th>
      <th>OUT_0200_0600</th>
      <th>IN_0600_0930</th>
      <th>OUT_0600_0930</th>
      <th>IN_0930_1500</th>
      <th>OUT_0930_1500</th>
      <th>IN_1500_1830</th>
      <th>OUT_1500_1830</th>
      <th>IN_1830_0200</th>
      <th>OUT_1830_0200</th>
      <th>IN_24_HOURS</th>
      <th>OUT_24_HOURS</th>
      <th>RANK</th>
      <th>STATION_SORT_ID</th>
    </tr>
    <tr>
      <th>STATION</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Central</th>
      <td>CBD</td>
      <td>2014</td>
      <td>6/05/2014</td>
      <td>Yes</td>
      <td>940</td>
      <td>920</td>
      <td>10390</td>
      <td>42070</td>
      <td>21400</td>
      <td>30150</td>
      <td>45370</td>
      <td>16270</td>
      <td>19010</td>
      <td>7700</td>
      <td>97110</td>
      <td>97110</td>
      <td>1</td>
      <td>101</td>
    </tr>
    <tr>
      <th>Town Hall</th>
      <td>CBD</td>
      <td>2014</td>
      <td>7/05/2014</td>
      <td>Yes</td>
      <td>880</td>
      <td>880</td>
      <td>6380</td>
      <td>43210</td>
      <td>16930</td>
      <td>23550</td>
      <td>43670</td>
      <td>15270</td>
      <td>21760</td>
      <td>6720</td>
      <td>89620</td>
      <td>89620</td>
      <td>2</td>
      <td>102</td>
    </tr>
    <tr>
      <th>Wynyard</th>
      <td>CBD</td>
      <td>2014</td>
      <td>20/05/2014</td>
      <td>Yes</td>
      <td>590</td>
      <td>570</td>
      <td>4710</td>
      <td>39620</td>
      <td>9200</td>
      <td>11180</td>
      <td>35010</td>
      <td>6510</td>
      <td>10690</td>
      <td>2330</td>
      <td>60200</td>
      <td>60200</td>
      <td>3</td>
      <td>103</td>
    </tr>
    <tr>
      <th>Parramatta</th>
      <td>Western</td>
      <td>2014</td>
      <td>19/11/2014</td>
      <td>Yes</td>
      <td>660</td>
      <td>330</td>
      <td>7790</td>
      <td>13700</td>
      <td>7400</td>
      <td>7020</td>
      <td>14960</td>
      <td>8660</td>
      <td>4150</td>
      <td>5260</td>
      <td>34960</td>
      <td>34960</td>
      <td>4</td>
      <td>905</td>
    </tr>
    <tr>
      <th>North Sydney</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>270</td>
      <td>270</td>
      <td>2340</td>
      <td>18340</td>
      <td>4450</td>
      <td>5590</td>
      <td>17760</td>
      <td>3050</td>
      <td>3790</td>
      <td>1360</td>
      <td>28610</td>
      <td>28610</td>
      <td>5</td>
      <td>1402</td>
    </tr>
    <tr>
      <th>Redfern</th>
      <td>CBD</td>
      <td>2014</td>
      <td>1/05/2014</td>
      <td>Yes</td>
      <td>250</td>
      <td>250</td>
      <td>3910</td>
      <td>10690</td>
      <td>5350</td>
      <td>8170</td>
      <td>12510</td>
      <td>4250</td>
      <td>3660</td>
      <td>2320</td>
      <td>25680</td>
      <td>25680</td>
      <td>6</td>
      <td>108</td>
    </tr>
    <tr>
      <th>Chatswood</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>200</td>
      <td>5030</td>
      <td>8200</td>
      <td>4710</td>
      <td>4930</td>
      <td>9430</td>
      <td>6000</td>
      <td>2820</td>
      <td>2860</td>
      <td>22200</td>
      <td>22200</td>
      <td>7</td>
      <td>1407</td>
    </tr>
    <tr>
      <th>Bondi Junction</th>
      <td>Eastern Suburbs</td>
      <td>2014</td>
      <td>2/09/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>210</td>
      <td>10150</td>
      <td>4350</td>
      <td>4370</td>
      <td>4600</td>
      <td>5240</td>
      <td>8330</td>
      <td>1910</td>
      <td>4380</td>
      <td>21880</td>
      <td>21880</td>
      <td>8</td>
      <td>203</td>
    </tr>
    <tr>
      <th>Strathfield</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>9/09/2014</td>
      <td>Yes</td>
      <td>190</td>
      <td>190</td>
      <td>7900</td>
      <td>4050</td>
      <td>4980</td>
      <td>3950</td>
      <td>5540</td>
      <td>7230</td>
      <td>2100</td>
      <td>5280</td>
      <td>20710</td>
      <td>20710</td>
      <td>9</td>
      <td>1110</td>
    </tr>
    <tr>
      <th>Circular Quay</th>
      <td>CBD</td>
      <td>2014</td>
      <td>22/05/2014</td>
      <td>Yes</td>
      <td>200</td>
      <td>200</td>
      <td>2110</td>
      <td>8750</td>
      <td>4340</td>
      <td>6710</td>
      <td>9910</td>
      <td>3380</td>
      <td>4070</td>
      <td>1600</td>
      <td>20630</td>
      <td>20630</td>
      <td>10</td>
      <td>104</td>
    </tr>
    <tr>
      <th>Martin Place</th>
      <td>CBD</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>200</td>
      <td>190</td>
      <td>590</td>
      <td>14250</td>
      <td>2900</td>
      <td>3420</td>
      <td>12340</td>
      <td>1790</td>
      <td>4600</td>
      <td>970</td>
      <td>20630</td>
      <td>20630</td>
      <td>10</td>
      <td>107</td>
    </tr>
    <tr>
      <th>Hurstville</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>28/05/2014</td>
      <td>Yes</td>
      <td>160</td>
      <td>160</td>
      <td>7790</td>
      <td>2200</td>
      <td>5230</td>
      <td>4180</td>
      <td>3840</td>
      <td>7450</td>
      <td>1270</td>
      <td>4290</td>
      <td>18290</td>
      <td>18290</td>
      <td>12</td>
      <td>310</td>
    </tr>
    <tr>
      <th>St Leonards</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>170</td>
      <td>170</td>
      <td>3420</td>
      <td>8900</td>
      <td>3090</td>
      <td>3310</td>
      <td>8780</td>
      <td>3230</td>
      <td>2130</td>
      <td>1990</td>
      <td>17590</td>
      <td>17590</td>
      <td>13</td>
      <td>1405</td>
    </tr>
    <tr>
      <th>Blacktown</th>
      <td>Western</td>
      <td>2014</td>
      <td>14/10/2014</td>
      <td>Yes</td>
      <td>190</td>
      <td>210</td>
      <td>6460</td>
      <td>3370</td>
      <td>4270</td>
      <td>3450</td>
      <td>4600</td>
      <td>7090</td>
      <td>1040</td>
      <td>2450</td>
      <td>16560</td>
      <td>16560</td>
      <td>14</td>
      <td>911</td>
    </tr>
    <tr>
      <th>Burwood</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>9/09/2014</td>
      <td>Yes</td>
      <td>140</td>
      <td>140</td>
      <td>4850</td>
      <td>2930</td>
      <td>4470</td>
      <td>3670</td>
      <td>4020</td>
      <td>5060</td>
      <td>1510</td>
      <td>3190</td>
      <td>14990</td>
      <td>14990</td>
      <td>15</td>
      <td>1109</td>
    </tr>
    <tr>
      <th>Kings Cross</th>
      <td>Eastern Suburbs</td>
      <td>2014</td>
      <td>3/09/2013</td>
      <td>No</td>
      <td>120</td>
      <td>120</td>
      <td>3690</td>
      <td>3820</td>
      <td>3110</td>
      <td>2670</td>
      <td>4400</td>
      <td>3640</td>
      <td>1550</td>
      <td>2620</td>
      <td>12870</td>
      <td>12870</td>
      <td>16</td>
      <td>201</td>
    </tr>
    <tr>
      <th>Hornsby</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>14/05/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>100</td>
      <td>5550</td>
      <td>2260</td>
      <td>3060</td>
      <td>2390</td>
      <td>2750</td>
      <td>5200</td>
      <td>680</td>
      <td>2290</td>
      <td>12250</td>
      <td>12250</td>
      <td>17</td>
      <td>1417</td>
    </tr>
    <tr>
      <th>Ashfield</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>4/09/2013</td>
      <td>No</td>
      <td>100</td>
      <td>100</td>
      <td>5090</td>
      <td>2010</td>
      <td>2690</td>
      <td>2220</td>
      <td>2500</td>
      <td>4500</td>
      <td>820</td>
      <td>2370</td>
      <td>11200</td>
      <td>11200</td>
      <td>18</td>
      <td>1107</td>
    </tr>
    <tr>
      <th>Kogarah</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>19/06/2014</td>
      <td>Yes</td>
      <td>90</td>
      <td>100</td>
      <td>4000</td>
      <td>2970</td>
      <td>3150</td>
      <td>2320</td>
      <td>3210</td>
      <td>3450</td>
      <td>650</td>
      <td>2260</td>
      <td>11100</td>
      <td>11100</td>
      <td>19</td>
      <td>307</td>
    </tr>
    <tr>
      <th>Lidcombe</th>
      <td>South</td>
      <td>2014</td>
      <td>5/06/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>4570</td>
      <td>1620</td>
      <td>2870</td>
      <td>2210</td>
      <td>2550</td>
      <td>3900</td>
      <td>820</td>
      <td>3080</td>
      <td>10910</td>
      <td>10910</td>
      <td>20</td>
      <td>702</td>
    </tr>
    <tr>
      <th>Auburn</th>
      <td>Western</td>
      <td>2014</td>
      <td>19/11/2014</td>
      <td>Yes</td>
      <td>130</td>
      <td>60</td>
      <td>3880</td>
      <td>1790</td>
      <td>3220</td>
      <td>2900</td>
      <td>2700</td>
      <td>3890</td>
      <td>960</td>
      <td>2260</td>
      <td>10890</td>
      <td>10890</td>
      <td>21</td>
      <td>901</td>
    </tr>
    <tr>
      <th>Museum</th>
      <td>CBD</td>
      <td>2014</td>
      <td>22/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>800</td>
      <td>6290</td>
      <td>2220</td>
      <td>2340</td>
      <td>5980</td>
      <td>1230</td>
      <td>1760</td>
      <td>910</td>
      <td>10860</td>
      <td>10860</td>
      <td>22</td>
      <td>106</td>
    </tr>
    <tr>
      <th>Epping</th>
      <td>Northern via Strathfield</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>170</td>
      <td>90</td>
      <td>5700</td>
      <td>1580</td>
      <td>2130</td>
      <td>1600</td>
      <td>1970</td>
      <td>4920</td>
      <td>600</td>
      <td>2380</td>
      <td>10570</td>
      <td>10570</td>
      <td>23</td>
      <td>1208</td>
    </tr>
    <tr>
      <th>Macquarie University</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>1310</td>
      <td>3220</td>
      <td>2570</td>
      <td>3940</td>
      <td>4530</td>
      <td>1970</td>
      <td>1580</td>
      <td>860</td>
      <td>10090</td>
      <td>10090</td>
      <td>24</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>St James</th>
      <td>CBD</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>90</td>
      <td>780</td>
      <td>5880</td>
      <td>1900</td>
      <td>2160</td>
      <td>5630</td>
      <td>1120</td>
      <td>1440</td>
      <td>580</td>
      <td>9850</td>
      <td>9850</td>
      <td>25</td>
      <td>105</td>
    </tr>
    <tr>
      <th>Rockdale</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>28/05/2014</td>
      <td>Yes</td>
      <td>90</td>
      <td>90</td>
      <td>4660</td>
      <td>780</td>
      <td>2610</td>
      <td>2170</td>
      <td>1670</td>
      <td>3770</td>
      <td>610</td>
      <td>2820</td>
      <td>9640</td>
      <td>9640</td>
      <td>26</td>
      <td>306</td>
    </tr>
    <tr>
      <th>Cabramatta</th>
      <td>South</td>
      <td>2014</td>
      <td>11/06/2014</td>
      <td>Yes</td>
      <td>150</td>
      <td>80</td>
      <td>3780</td>
      <td>1070</td>
      <td>2760</td>
      <td>2820</td>
      <td>1750</td>
      <td>3760</td>
      <td>290</td>
      <td>1000</td>
      <td>8730</td>
      <td>8730</td>
      <td>27</td>
      <td>708</td>
    </tr>
    <tr>
      <th>Bankstown</th>
      <td>Bankstown</td>
      <td>2014</td>
      <td>13/05/2014</td>
      <td>Yes</td>
      <td>80</td>
      <td>60</td>
      <td>3010</td>
      <td>1810</td>
      <td>2400</td>
      <td>2570</td>
      <td>2840</td>
      <td>3130</td>
      <td>300</td>
      <td>1050</td>
      <td>8630</td>
      <td>8630</td>
      <td>28</td>
      <td>612</td>
    </tr>
    <tr>
      <th>Liverpool</th>
      <td>South</td>
      <td>2014</td>
      <td>4/06/2014</td>
      <td>Yes</td>
      <td>150</td>
      <td>80</td>
      <td>2510</td>
      <td>2270</td>
      <td>2510</td>
      <td>2260</td>
      <td>2610</td>
      <td>2650</td>
      <td>570</td>
      <td>1100</td>
      <td>8350</td>
      <td>8350</td>
      <td>29</td>
      <td>710</td>
    </tr>
    <tr>
      <th>Sutherland</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>12/06/2014</td>
      <td>Yes</td>
      <td>140</td>
      <td>70</td>
      <td>4260</td>
      <td>1460</td>
      <td>1920</td>
      <td>1650</td>
      <td>1750</td>
      <td>3900</td>
      <td>250</td>
      <td>1240</td>
      <td>8320</td>
      <td>8320</td>
      <td>30</td>
      <td>316</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Menangle Park</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>19/06/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>277</td>
      <td>1601</td>
    </tr>
    <tr>
      <th>Port Kembla North</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>18/09/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>277</td>
      <td>1521</td>
    </tr>
    <tr>
      <th>Scone</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>277</td>
      <td>2029</td>
    </tr>
    <tr>
      <th>Tarro</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>277</td>
      <td>2005</td>
    </tr>
    <tr>
      <th>Aberdeen</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2028</td>
    </tr>
    <tr>
      <th>Bombo</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>18/09/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1530</td>
    </tr>
    <tr>
      <th>Branxton</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2024</td>
    </tr>
    <tr>
      <th>Burradoo</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1610</td>
    </tr>
    <tr>
      <th>Coalcliff</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>9/08/2012</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1504</td>
    </tr>
    <tr>
      <th>Dunmore (Shellharbour)</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>18/06/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1528</td>
    </tr>
    <tr>
      <th>Exeter</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1612</td>
    </tr>
    <tr>
      <th>Greta</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2023</td>
    </tr>
    <tr>
      <th>Linden</th>
      <td>Blue Mountains</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1708</td>
    </tr>
    <tr>
      <th>Marulan</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>17/06/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1617</td>
    </tr>
    <tr>
      <th>Menangle</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>17/06/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1602</td>
    </tr>
    <tr>
      <th>Paterson</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>Scarborough</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>22/08/2013</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1505</td>
    </tr>
    <tr>
      <th>Tallong</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1616</td>
    </tr>
    <tr>
      <th>Wondabyne</th>
      <td>Central Coast</td>
      <td>2014</td>
      <td>21/11/2012</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1803</td>
    </tr>
    <tr>
      <th>Bell</th>
      <td>Blue Mountains</td>
      <td>2014</td>
      <td>0</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1719</td>
    </tr>
    <tr>
      <th>Hilldale</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>Kembla Grange Racecourse</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>15/02/1996</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1524</td>
    </tr>
    <tr>
      <th>Lochinvar</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>Lysaghts</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>12/11/2013</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1519</td>
    </tr>
    <tr>
      <th>Mindaribba</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>Penrose</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1614</td>
    </tr>
    <tr>
      <th>Wallarobba</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>Wingello</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1615</td>
    </tr>
    <tr>
      <th>Wirragulla</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>Zig Zag</th>
      <td>Blue Mountains</td>
      <td>2014</td>
      <td>9/10/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1720</td>
    </tr>
  </tbody>
</table>
<p>308 rows × 18 columns</p>
</div>




```python
sydtrains.LINE.unique()
```




    array(['CBD', 'Western', 'North Shore', 'Eastern Suburbs', 'Inner West',
           'Illawarra', 'South', 'Northern via Strathfield',
           'Northern via Macquarie Park', 'Bankstown', 'Airport', 'East Hills',
           'Central Coast', 'Olympic Park', 'South Coast', 'Blue Mountains',
           'Newcastle', 'Hunter', 'Carlingford', 'Southern Highlands'], dtype=object)




```python
sydtrains[sydtrains.LINE.str.contains('c')]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LINE</th>
      <th>YEAR</th>
      <th>SURVEY_DATE_USED</th>
      <th>WHETHER_SURVEYED</th>
      <th>IN_0200_0600</th>
      <th>OUT_0200_0600</th>
      <th>IN_0600_0930</th>
      <th>OUT_0600_0930</th>
      <th>IN_0930_1500</th>
      <th>OUT_0930_1500</th>
      <th>IN_1500_1830</th>
      <th>OUT_1500_1830</th>
      <th>IN_1830_0200</th>
      <th>OUT_1830_0200</th>
      <th>IN_24_HOURS</th>
      <th>OUT_24_HOURS</th>
      <th>RANK</th>
      <th>STATION_SORT_ID</th>
    </tr>
    <tr>
      <th>STATION</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Macquarie University</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>1310</td>
      <td>3220</td>
      <td>2570</td>
      <td>3940</td>
      <td>4530</td>
      <td>1970</td>
      <td>1580</td>
      <td>860</td>
      <td>10090</td>
      <td>10090</td>
      <td>24</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>Olympic Park</th>
      <td>Olympic Park</td>
      <td>2014</td>
      <td>18/06/2013</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>270</td>
      <td>2230</td>
      <td>510</td>
      <td>1040</td>
      <td>2890</td>
      <td>290</td>
      <td>420</td>
      <td>530</td>
      <td>4090</td>
      <td>4090</td>
      <td>62</td>
      <td>801</td>
    </tr>
    <tr>
      <th>Macquarie Park</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>40</td>
      <td>30</td>
      <td>440</td>
      <td>2130</td>
      <td>460</td>
      <td>460</td>
      <td>2360</td>
      <td>360</td>
      <td>600</td>
      <td>900</td>
      <td>3900</td>
      <td>3900</td>
      <td>65</td>
      <td>1302</td>
    </tr>
    <tr>
      <th>Pennant Hills</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>29/05/2014</td>
      <td>Yes</td>
      <td>40</td>
      <td>30</td>
      <td>1760</td>
      <td>730</td>
      <td>610</td>
      <td>600</td>
      <td>840</td>
      <td>1570</td>
      <td>60</td>
      <td>400</td>
      <td>3310</td>
      <td>3310</td>
      <td>77</td>
      <td>1306</td>
    </tr>
    <tr>
      <th>Thornleigh</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>27/03/2013</td>
      <td>No</td>
      <td>50</td>
      <td>20</td>
      <td>1560</td>
      <td>180</td>
      <td>480</td>
      <td>300</td>
      <td>260</td>
      <td>1400</td>
      <td>70</td>
      <td>500</td>
      <td>2420</td>
      <td>2420</td>
      <td>104</td>
      <td>1307</td>
    </tr>
    <tr>
      <th>Beecroft</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>20/08/2013</td>
      <td>No</td>
      <td>30</td>
      <td>20</td>
      <td>1630</td>
      <td>150</td>
      <td>330</td>
      <td>340</td>
      <td>190</td>
      <td>1230</td>
      <td>40</td>
      <td>480</td>
      <td>2220</td>
      <td>2220</td>
      <td>116</td>
      <td>1305</td>
    </tr>
    <tr>
      <th>Normanhurst</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>26/03/2013</td>
      <td>No</td>
      <td>20</td>
      <td>10</td>
      <td>1020</td>
      <td>490</td>
      <td>420</td>
      <td>290</td>
      <td>290</td>
      <td>690</td>
      <td>30</td>
      <td>310</td>
      <td>1780</td>
      <td>1780</td>
      <td>127</td>
      <td>1308</td>
    </tr>
    <tr>
      <th>North Ryde</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>230</td>
      <td>990</td>
      <td>260</td>
      <td>280</td>
      <td>980</td>
      <td>250</td>
      <td>170</td>
      <td>110</td>
      <td>1640</td>
      <td>1640</td>
      <td>133</td>
      <td>1301</td>
    </tr>
    <tr>
      <th>Cheltenham</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>27/06/2013</td>
      <td>No</td>
      <td>10</td>
      <td>10</td>
      <td>740</td>
      <td>480</td>
      <td>300</td>
      <td>200</td>
      <td>420</td>
      <td>540</td>
      <td>10</td>
      <td>250</td>
      <td>1480</td>
      <td>1480</td>
      <td>141</td>
      <td>1304</td>
    </tr>
    <tr>
      <th>Civic</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>22/10/2014</td>
      <td>Yes</td>
      <td>10</td>
      <td>20</td>
      <td>170</td>
      <td>430</td>
      <td>330</td>
      <td>310</td>
      <td>510</td>
      <td>180</td>
      <td>80</td>
      <td>160</td>
      <td>1100</td>
      <td>1100</td>
      <td>154</td>
      <td>1916</td>
    </tr>
    <tr>
      <th>Morisset</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>20/11/2012</td>
      <td>No</td>
      <td>70</td>
      <td>10</td>
      <td>630</td>
      <td>110</td>
      <td>220</td>
      <td>180</td>
      <td>110</td>
      <td>580</td>
      <td>30</td>
      <td>180</td>
      <td>1060</td>
      <td>1060</td>
      <td>157</td>
      <td>1903</td>
    </tr>
    <tr>
      <th>Broadmeadow</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>23/10/2014</td>
      <td>Yes</td>
      <td>50</td>
      <td>20</td>
      <td>470</td>
      <td>170</td>
      <td>270</td>
      <td>290</td>
      <td>230</td>
      <td>370</td>
      <td>20</td>
      <td>170</td>
      <td>1040</td>
      <td>1040</td>
      <td>158</td>
      <td>1913</td>
    </tr>
    <tr>
      <th>Newcastle</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>21/10/2014</td>
      <td>Yes</td>
      <td>20</td>
      <td>10</td>
      <td>220</td>
      <td>240</td>
      <td>440</td>
      <td>460</td>
      <td>230</td>
      <td>170</td>
      <td>60</td>
      <td>90</td>
      <td>970</td>
      <td>970</td>
      <td>159</td>
      <td>1917</td>
    </tr>
    <tr>
      <th>Cardiff</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>4/12/2012</td>
      <td>No</td>
      <td>70</td>
      <td>20</td>
      <td>400</td>
      <td>130</td>
      <td>170</td>
      <td>160</td>
      <td>160</td>
      <td>440</td>
      <td>70</td>
      <td>150</td>
      <td>870</td>
      <td>870</td>
      <td>164</td>
      <td>1910</td>
    </tr>
    <tr>
      <th>Hamilton</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>15/10/2014</td>
      <td>Yes</td>
      <td>30</td>
      <td>10</td>
      <td>220</td>
      <td>200</td>
      <td>290</td>
      <td>250</td>
      <td>220</td>
      <td>270</td>
      <td>100</td>
      <td>140</td>
      <td>860</td>
      <td>860</td>
      <td>166</td>
      <td>1914</td>
    </tr>
    <tr>
      <th>Wickham</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>16/10/2014</td>
      <td>Yes</td>
      <td>10</td>
      <td>10</td>
      <td>130</td>
      <td>290</td>
      <td>190</td>
      <td>170</td>
      <td>300</td>
      <td>150</td>
      <td>40</td>
      <td>50</td>
      <td>670</td>
      <td>670</td>
      <td>177</td>
      <td>1915</td>
    </tr>
    <tr>
      <th>Fassifern</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>22/08/2013</td>
      <td>No</td>
      <td>30</td>
      <td>10</td>
      <td>310</td>
      <td>110</td>
      <td>160</td>
      <td>120</td>
      <td>110</td>
      <td>290</td>
      <td>10</td>
      <td>100</td>
      <td>620</td>
      <td>620</td>
      <td>181</td>
      <td>1906</td>
    </tr>
    <tr>
      <th>Wyee</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>21/11/2012</td>
      <td>No</td>
      <td>50</td>
      <td>0</td>
      <td>210</td>
      <td>20</td>
      <td>80</td>
      <td>70</td>
      <td>40</td>
      <td>230</td>
      <td>10</td>
      <td>60</td>
      <td>390</td>
      <td>390</td>
      <td>195</td>
      <td>1902</td>
    </tr>
    <tr>
      <th>Warnervale</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>21/11/2012</td>
      <td>No</td>
      <td>40</td>
      <td>0</td>
      <td>240</td>
      <td>20</td>
      <td>50</td>
      <td>20</td>
      <td>20</td>
      <td>230</td>
      <td>10</td>
      <td>80</td>
      <td>360</td>
      <td>360</td>
      <td>201</td>
      <td>1901</td>
    </tr>
    <tr>
      <th>Adamstown</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>22/08/2013</td>
      <td>No</td>
      <td>10</td>
      <td>0</td>
      <td>60</td>
      <td>40</td>
      <td>30</td>
      <td>20</td>
      <td>40</td>
      <td>70</td>
      <td>10</td>
      <td>30</td>
      <td>150</td>
      <td>150</td>
      <td>239</td>
      <td>1912</td>
    </tr>
    <tr>
      <th>Booragul</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>28/10/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>70</td>
      <td>10</td>
      <td>10</td>
      <td>70</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>100</td>
      <td>100</td>
      <td>249</td>
      <td>1907</td>
    </tr>
    <tr>
      <th>Dora Creek</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>11/12/2012</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>10</td>
      <td>40</td>
      <td>0</td>
      <td>10</td>
      <td>70</td>
      <td>70</td>
      <td>257</td>
      <td>1904</td>
    </tr>
    <tr>
      <th>Kotara</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>22/08/2013</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>30</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>50</td>
      <td>50</td>
      <td>263</td>
      <td>1911</td>
    </tr>
    <tr>
      <th>Teralba</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>4/12/2012</td>
      <td>No</td>
      <td>0</td>
      <td>10</td>
      <td>30</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>30</td>
      <td>0</td>
      <td>10</td>
      <td>50</td>
      <td>50</td>
      <td>263</td>
      <td>1908</td>
    </tr>
    <tr>
      <th>Awaba</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>22/10/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>30</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>30</td>
      <td>30</td>
      <td>271</td>
      <td>1905</td>
    </tr>
    <tr>
      <th>Cockle Creek</th>
      <td>Newcastle</td>
      <td>2014</td>
      <td>28/11/2012</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>30</td>
      <td>30</td>
      <td>271</td>
      <td>1909</td>
    </tr>
    <tr>
      <th>Macquarie University</th>
      <td>Northern via Macquarie Park</td>
      <td>2013</td>
      <td>14/05/2013</td>
      <td>Yes</td>
      <td>80</td>
      <td>80</td>
      <td>1130</td>
      <td>2990</td>
      <td>2040</td>
      <td>3150</td>
      <td>4250</td>
      <td>1790</td>
      <td>1240</td>
      <td>730</td>
      <td>8740</td>
      <td>8740</td>
      <td>26</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>Pennant Hills</th>
      <td>Northern via Macquarie Park</td>
      <td>2013</td>
      <td>17/05/2012</td>
      <td>No</td>
      <td>50</td>
      <td>30</td>
      <td>1820</td>
      <td>580</td>
      <td>650</td>
      <td>550</td>
      <td>770</td>
      <td>1680</td>
      <td>80</td>
      <td>540</td>
      <td>3370</td>
      <td>3370</td>
      <td>70</td>
      <td>1306</td>
    </tr>
    <tr>
      <th>Macquarie Park</th>
      <td>Northern via Macquarie Park</td>
      <td>2013</td>
      <td>15/05/2013</td>
      <td>Yes</td>
      <td>30</td>
      <td>40</td>
      <td>430</td>
      <td>2070</td>
      <td>380</td>
      <td>640</td>
      <td>2150</td>
      <td>430</td>
      <td>370</td>
      <td>190</td>
      <td>3360</td>
      <td>3360</td>
      <td>72</td>
      <td>1302</td>
    </tr>
    <tr>
      <th>Olympic Park</th>
      <td>Olympic Park</td>
      <td>2013</td>
      <td>18/06/2013</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>170</td>
      <td>1420</td>
      <td>320</td>
      <td>660</td>
      <td>1840</td>
      <td>190</td>
      <td>270</td>
      <td>340</td>
      <td>2600</td>
      <td>2600</td>
      <td>95</td>
      <td>801</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Kotara</th>
      <td>Newcastle</td>
      <td>2005</td>
      <td>18/07/1995</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>40</td>
      <td>40</td>
      <td>264</td>
      <td>1911</td>
    </tr>
    <tr>
      <th>Adamstown</th>
      <td>Newcastle</td>
      <td>2005</td>
      <td>18/07/1995</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>30</td>
      <td>30</td>
      <td>20</td>
      <td>20</td>
      <td>10</td>
      <td>10</td>
      <td>80</td>
      <td>80</td>
      <td>252</td>
      <td>1912</td>
    </tr>
    <tr>
      <th>Broadmeadow</th>
      <td>Newcastle</td>
      <td>2005</td>
      <td>20/07/2004</td>
      <td>No</td>
      <td>60</td>
      <td>10</td>
      <td>290</td>
      <td>350</td>
      <td>220</td>
      <td>180</td>
      <td>330</td>
      <td>280</td>
      <td>30</td>
      <td>110</td>
      <td>930</td>
      <td>930</td>
      <td>150</td>
      <td>1913</td>
    </tr>
    <tr>
      <th>Hamilton</th>
      <td>Newcastle</td>
      <td>2005</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>40</td>
      <td>10</td>
      <td>180</td>
      <td>230</td>
      <td>350</td>
      <td>290</td>
      <td>270</td>
      <td>260</td>
      <td>90</td>
      <td>140</td>
      <td>930</td>
      <td>930</td>
      <td>150</td>
      <td>1914</td>
    </tr>
    <tr>
      <th>Wickham</th>
      <td>Newcastle</td>
      <td>2005</td>
      <td>9/12/2003</td>
      <td>No</td>
      <td>10</td>
      <td>0</td>
      <td>30</td>
      <td>160</td>
      <td>110</td>
      <td>100</td>
      <td>170</td>
      <td>70</td>
      <td>20</td>
      <td>10</td>
      <td>340</td>
      <td>340</td>
      <td>201</td>
      <td>1915</td>
    </tr>
    <tr>
      <th>Civic</th>
      <td>Newcastle</td>
      <td>2005</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>30</td>
      <td>10</td>
      <td>80</td>
      <td>320</td>
      <td>270</td>
      <td>250</td>
      <td>370</td>
      <td>180</td>
      <td>40</td>
      <td>20</td>
      <td>790</td>
      <td>790</td>
      <td>165</td>
      <td>1916</td>
    </tr>
    <tr>
      <th>Newcastle</th>
      <td>Newcastle</td>
      <td>2005</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>50</td>
      <td>20</td>
      <td>250</td>
      <td>300</td>
      <td>680</td>
      <td>810</td>
      <td>410</td>
      <td>190</td>
      <td>70</td>
      <td>130</td>
      <td>1460</td>
      <td>1460</td>
      <td>127</td>
      <td>1917</td>
    </tr>
    <tr>
      <th>Olympic Park</th>
      <td>Olympic Park</td>
      <td>2004</td>
      <td>2/06/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>170</td>
      <td>190</td>
      <td>210</td>
      <td>210</td>
      <td>90</td>
      <td>80</td>
      <td>30</td>
      <td>500</td>
      <td>500</td>
      <td>187</td>
      <td>801</td>
    </tr>
    <tr>
      <th>Cheltenham</th>
      <td>Northern via Macquarie Park</td>
      <td>2004</td>
      <td>1/04/2003</td>
      <td>No</td>
      <td>10</td>
      <td>10</td>
      <td>620</td>
      <td>510</td>
      <td>240</td>
      <td>130</td>
      <td>440</td>
      <td>510</td>
      <td>20</td>
      <td>160</td>
      <td>1330</td>
      <td>1330</td>
      <td>135</td>
      <td>1304</td>
    </tr>
    <tr>
      <th>Beecroft</th>
      <td>Northern via Macquarie Park</td>
      <td>2004</td>
      <td>27/03/2003</td>
      <td>No</td>
      <td>30</td>
      <td>20</td>
      <td>1680</td>
      <td>120</td>
      <td>280</td>
      <td>300</td>
      <td>190</td>
      <td>1360</td>
      <td>40</td>
      <td>440</td>
      <td>2220</td>
      <td>2220</td>
      <td>90</td>
      <td>1305</td>
    </tr>
    <tr>
      <th>Pennant Hills</th>
      <td>Northern via Macquarie Park</td>
      <td>2004</td>
      <td>2/04/2003</td>
      <td>No</td>
      <td>60</td>
      <td>30</td>
      <td>2330</td>
      <td>680</td>
      <td>600</td>
      <td>540</td>
      <td>990</td>
      <td>2220</td>
      <td>100</td>
      <td>620</td>
      <td>4080</td>
      <td>4080</td>
      <td>52</td>
      <td>1306</td>
    </tr>
    <tr>
      <th>Thornleigh</th>
      <td>Northern via Macquarie Park</td>
      <td>2004</td>
      <td>25/03/2003</td>
      <td>No</td>
      <td>40</td>
      <td>20</td>
      <td>1170</td>
      <td>140</td>
      <td>380</td>
      <td>330</td>
      <td>220</td>
      <td>1020</td>
      <td>60</td>
      <td>350</td>
      <td>1870</td>
      <td>1870</td>
      <td>104</td>
      <td>1307</td>
    </tr>
    <tr>
      <th>Normanhurst</th>
      <td>Northern via Macquarie Park</td>
      <td>2004</td>
      <td>26/03/2003</td>
      <td>No</td>
      <td>20</td>
      <td>10</td>
      <td>670</td>
      <td>760</td>
      <td>290</td>
      <td>220</td>
      <td>700</td>
      <td>530</td>
      <td>30</td>
      <td>190</td>
      <td>1710</td>
      <td>1710</td>
      <td>113</td>
      <td>1308</td>
    </tr>
    <tr>
      <th>Warnervale</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>16/01/2003</td>
      <td>No</td>
      <td>10</td>
      <td>0</td>
      <td>130</td>
      <td>10</td>
      <td>30</td>
      <td>30</td>
      <td>10</td>
      <td>110</td>
      <td>0</td>
      <td>40</td>
      <td>180</td>
      <td>180</td>
      <td>231</td>
      <td>1901</td>
    </tr>
    <tr>
      <th>Wyee</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>16/01/2003</td>
      <td>No</td>
      <td>10</td>
      <td>0</td>
      <td>170</td>
      <td>40</td>
      <td>110</td>
      <td>70</td>
      <td>50</td>
      <td>170</td>
      <td>10</td>
      <td>70</td>
      <td>350</td>
      <td>350</td>
      <td>202</td>
      <td>1902</td>
    </tr>
    <tr>
      <th>Morisset</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>2/03/2000</td>
      <td>No</td>
      <td>30</td>
      <td>10</td>
      <td>580</td>
      <td>90</td>
      <td>150</td>
      <td>160</td>
      <td>110</td>
      <td>490</td>
      <td>20</td>
      <td>150</td>
      <td>890</td>
      <td>890</td>
      <td>158</td>
      <td>1903</td>
    </tr>
    <tr>
      <th>Dora Creek</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>Estimate 2004</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>70</td>
      <td>10</td>
      <td>30</td>
      <td>20</td>
      <td>20</td>
      <td>60</td>
      <td>0</td>
      <td>20</td>
      <td>120</td>
      <td>120</td>
      <td>240</td>
      <td>1904</td>
    </tr>
    <tr>
      <th>Awaba</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>Estimate 2004</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>40</td>
      <td>40</td>
      <td>267</td>
      <td>1905</td>
    </tr>
    <tr>
      <th>Fassifern</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>22/07/2004</td>
      <td>Yes</td>
      <td>20</td>
      <td>0</td>
      <td>290</td>
      <td>70</td>
      <td>70</td>
      <td>60</td>
      <td>80</td>
      <td>260</td>
      <td>10</td>
      <td>80</td>
      <td>470</td>
      <td>470</td>
      <td>188</td>
      <td>1906</td>
    </tr>
    <tr>
      <th>Booragul</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>18/07/1995</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>30</td>
      <td>30</td>
      <td>40</td>
      <td>50</td>
      <td>40</td>
      <td>30</td>
      <td>10</td>
      <td>10</td>
      <td>120</td>
      <td>120</td>
      <td>240</td>
      <td>1907</td>
    </tr>
    <tr>
      <th>Teralba</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>18/07/1995</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>30</td>
      <td>0</td>
      <td>10</td>
      <td>60</td>
      <td>60</td>
      <td>257</td>
      <td>1908</td>
    </tr>
    <tr>
      <th>Cockle Creek</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>18/07/1995</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>40</td>
      <td>267</td>
      <td>1909</td>
    </tr>
    <tr>
      <th>Cardiff</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>21/07/2004</td>
      <td>Yes</td>
      <td>50</td>
      <td>10</td>
      <td>250</td>
      <td>90</td>
      <td>140</td>
      <td>90</td>
      <td>100</td>
      <td>290</td>
      <td>40</td>
      <td>110</td>
      <td>580</td>
      <td>580</td>
      <td>182</td>
      <td>1910</td>
    </tr>
    <tr>
      <th>Kotara</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>18/07/1995</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>40</td>
      <td>40</td>
      <td>267</td>
      <td>1911</td>
    </tr>
    <tr>
      <th>Adamstown</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>18/07/1995</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>30</td>
      <td>30</td>
      <td>20</td>
      <td>20</td>
      <td>10</td>
      <td>10</td>
      <td>80</td>
      <td>80</td>
      <td>252</td>
      <td>1912</td>
    </tr>
    <tr>
      <th>Broadmeadow</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>20/07/2004</td>
      <td>Yes</td>
      <td>20</td>
      <td>10</td>
      <td>290</td>
      <td>350</td>
      <td>220</td>
      <td>170</td>
      <td>330</td>
      <td>270</td>
      <td>30</td>
      <td>100</td>
      <td>890</td>
      <td>890</td>
      <td>158</td>
      <td>1913</td>
    </tr>
    <tr>
      <th>Hamilton</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>30</td>
      <td>10</td>
      <td>170</td>
      <td>220</td>
      <td>340</td>
      <td>280</td>
      <td>260</td>
      <td>250</td>
      <td>90</td>
      <td>140</td>
      <td>890</td>
      <td>890</td>
      <td>158</td>
      <td>1914</td>
    </tr>
    <tr>
      <th>Wickham</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>10</td>
      <td>0</td>
      <td>30</td>
      <td>160</td>
      <td>110</td>
      <td>100</td>
      <td>180</td>
      <td>70</td>
      <td>20</td>
      <td>10</td>
      <td>350</td>
      <td>350</td>
      <td>202</td>
      <td>1915</td>
    </tr>
    <tr>
      <th>Civic</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>30</td>
      <td>10</td>
      <td>80</td>
      <td>330</td>
      <td>270</td>
      <td>260</td>
      <td>370</td>
      <td>180</td>
      <td>40</td>
      <td>20</td>
      <td>790</td>
      <td>790</td>
      <td>166</td>
      <td>1916</td>
    </tr>
    <tr>
      <th>Newcastle</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>50</td>
      <td>20</td>
      <td>260</td>
      <td>320</td>
      <td>720</td>
      <td>850</td>
      <td>430</td>
      <td>200</td>
      <td>70</td>
      <td>140</td>
      <td>1530</td>
      <td>1530</td>
      <td>123</td>
      <td>1917</td>
    </tr>
  </tbody>
</table>
<p>271 rows × 18 columns</p>
</div>




```python
sydtrains.LINE.nunique()
```




    20




```python
trains2014.dtypes
```




    LINE                object
    YEAR                 int64
    SURVEY_DATE_USED    object
    WHETHER_SURVEYED    object
    IN_0200_0600         int64
    OUT_0200_0600        int64
    IN_0600_0930         int64
    OUT_0600_0930        int64
    IN_0930_1500         int64
    OUT_0930_1500        int64
    IN_1500_1830         int64
    OUT_1500_1830        int64
    IN_1830_0200         int64
    OUT_1830_0200        int64
    IN_24_HOURS          int64
    OUT_24_HOURS         int64
    RANK                 int64
    STATION_SORT_ID      int64
    dtype: object




```python
trains2014.IN_24_HOURS
```




    STATION
    Central                     97110
    Town Hall                   89620
    Wynyard                     60200
    Parramatta                  34960
    North Sydney                28610
    Redfern                     25680
    Chatswood                   22200
    Bondi Junction              21880
    Strathfield                 20710
    Circular Quay               20630
    Martin Place                20630
    Hurstville                  18290
    St Leonards                 17590
    Blacktown                   16560
    Burwood                     14990
    Kings Cross                 12870
    Hornsby                     12250
    Ashfield                    11200
    Kogarah                     11100
    Lidcombe                    10910
    Auburn                      10890
    Museum                      10860
    Epping                      10570
    Macquarie University        10090
    St James                     9850
    Rockdale                     9640
    Cabramatta                   8730
    Bankstown                    8630
    Liverpool                    8350
    Sutherland                   8320
                                ...  
    Menangle Park                  20
    Port Kembla North              20
    Scone                          20
    Tarro                          20
    Aberdeen                       10
    Bombo                          10
    Branxton                       10
    Burradoo                       10
    Coalcliff                      10
    Dunmore (Shellharbour)         10
    Exeter                         10
    Greta                          10
    Linden                         10
    Marulan                        10
    Menangle                       10
    Paterson                       10
    Scarborough                    10
    Tallong                        10
    Wondabyne                      10
    Bell                            0
    Hilldale                        0
    Kembla Grange Racecourse        0
    Lochinvar                       0
    Lysaghts                        0
    Mindaribba                      0
    Penrose                         0
    Wallarobba                      0
    Wingello                        0
    Wirragulla                      0
    Zig Zag                         0
    Name: IN_24_HOURS, dtype: int64




```python
sydtrains1= pandas.read_csv(
'/Users/Mahendra/Desktop/GA/SYD_DAT_7/data/sydtrains.csv',
    index_col=1,
    thousands=","
)
```


```python
sydtrains1

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LINE</th>
      <th>YEAR</th>
      <th>SURVEY_DATE_USED</th>
      <th>WHETHER_SURVEYED</th>
      <th>IN_0200_0600</th>
      <th>OUT_0200_0600</th>
      <th>IN_0600_0930</th>
      <th>OUT_0600_0930</th>
      <th>IN_0930_1500</th>
      <th>OUT_0930_1500</th>
      <th>IN_1500_1830</th>
      <th>OUT_1500_1830</th>
      <th>IN_1830_0200</th>
      <th>OUT_1830_0200</th>
      <th>IN_24_HOURS</th>
      <th>OUT_24_HOURS</th>
      <th>RANK</th>
      <th>STATION_SORT_ID</th>
    </tr>
    <tr>
      <th>STATION</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Central</th>
      <td>CBD</td>
      <td>2014</td>
      <td>6/05/2014</td>
      <td>Yes</td>
      <td>940</td>
      <td>920</td>
      <td>10390</td>
      <td>42070</td>
      <td>21400</td>
      <td>30150</td>
      <td>45370</td>
      <td>16270</td>
      <td>19010</td>
      <td>7700</td>
      <td>97110</td>
      <td>97110</td>
      <td>1</td>
      <td>101</td>
    </tr>
    <tr>
      <th>Town Hall</th>
      <td>CBD</td>
      <td>2014</td>
      <td>7/05/2014</td>
      <td>Yes</td>
      <td>880</td>
      <td>880</td>
      <td>6380</td>
      <td>43210</td>
      <td>16930</td>
      <td>23550</td>
      <td>43670</td>
      <td>15270</td>
      <td>21760</td>
      <td>6720</td>
      <td>89620</td>
      <td>89620</td>
      <td>2</td>
      <td>102</td>
    </tr>
    <tr>
      <th>Wynyard</th>
      <td>CBD</td>
      <td>2014</td>
      <td>20/05/2014</td>
      <td>Yes</td>
      <td>590</td>
      <td>570</td>
      <td>4710</td>
      <td>39620</td>
      <td>9200</td>
      <td>11180</td>
      <td>35010</td>
      <td>6510</td>
      <td>10690</td>
      <td>2330</td>
      <td>60200</td>
      <td>60200</td>
      <td>3</td>
      <td>103</td>
    </tr>
    <tr>
      <th>Parramatta</th>
      <td>Western</td>
      <td>2014</td>
      <td>19/11/2014</td>
      <td>Yes</td>
      <td>660</td>
      <td>330</td>
      <td>7790</td>
      <td>13700</td>
      <td>7400</td>
      <td>7020</td>
      <td>14960</td>
      <td>8660</td>
      <td>4150</td>
      <td>5260</td>
      <td>34960</td>
      <td>34960</td>
      <td>4</td>
      <td>905</td>
    </tr>
    <tr>
      <th>North Sydney</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>270</td>
      <td>270</td>
      <td>2340</td>
      <td>18340</td>
      <td>4450</td>
      <td>5590</td>
      <td>17760</td>
      <td>3050</td>
      <td>3790</td>
      <td>1360</td>
      <td>28610</td>
      <td>28610</td>
      <td>5</td>
      <td>1402</td>
    </tr>
    <tr>
      <th>Redfern</th>
      <td>CBD</td>
      <td>2014</td>
      <td>1/05/2014</td>
      <td>Yes</td>
      <td>250</td>
      <td>250</td>
      <td>3910</td>
      <td>10690</td>
      <td>5350</td>
      <td>8170</td>
      <td>12510</td>
      <td>4250</td>
      <td>3660</td>
      <td>2320</td>
      <td>25680</td>
      <td>25680</td>
      <td>6</td>
      <td>108</td>
    </tr>
    <tr>
      <th>Chatswood</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>200</td>
      <td>5030</td>
      <td>8200</td>
      <td>4710</td>
      <td>4930</td>
      <td>9430</td>
      <td>6000</td>
      <td>2820</td>
      <td>2860</td>
      <td>22200</td>
      <td>22200</td>
      <td>7</td>
      <td>1407</td>
    </tr>
    <tr>
      <th>Bondi Junction</th>
      <td>Eastern Suburbs</td>
      <td>2014</td>
      <td>2/09/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>210</td>
      <td>10150</td>
      <td>4350</td>
      <td>4370</td>
      <td>4600</td>
      <td>5240</td>
      <td>8330</td>
      <td>1910</td>
      <td>4380</td>
      <td>21880</td>
      <td>21880</td>
      <td>8</td>
      <td>203</td>
    </tr>
    <tr>
      <th>Strathfield</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>9/09/2014</td>
      <td>Yes</td>
      <td>190</td>
      <td>190</td>
      <td>7900</td>
      <td>4050</td>
      <td>4980</td>
      <td>3950</td>
      <td>5540</td>
      <td>7230</td>
      <td>2100</td>
      <td>5280</td>
      <td>20710</td>
      <td>20710</td>
      <td>9</td>
      <td>1110</td>
    </tr>
    <tr>
      <th>Circular Quay</th>
      <td>CBD</td>
      <td>2014</td>
      <td>22/05/2014</td>
      <td>Yes</td>
      <td>200</td>
      <td>200</td>
      <td>2110</td>
      <td>8750</td>
      <td>4340</td>
      <td>6710</td>
      <td>9910</td>
      <td>3380</td>
      <td>4070</td>
      <td>1600</td>
      <td>20630</td>
      <td>20630</td>
      <td>10</td>
      <td>104</td>
    </tr>
    <tr>
      <th>Martin Place</th>
      <td>CBD</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>200</td>
      <td>190</td>
      <td>590</td>
      <td>14250</td>
      <td>2900</td>
      <td>3420</td>
      <td>12340</td>
      <td>1790</td>
      <td>4600</td>
      <td>970</td>
      <td>20630</td>
      <td>20630</td>
      <td>10</td>
      <td>107</td>
    </tr>
    <tr>
      <th>Hurstville</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>28/05/2014</td>
      <td>Yes</td>
      <td>160</td>
      <td>160</td>
      <td>7790</td>
      <td>2200</td>
      <td>5230</td>
      <td>4180</td>
      <td>3840</td>
      <td>7450</td>
      <td>1270</td>
      <td>4290</td>
      <td>18290</td>
      <td>18290</td>
      <td>12</td>
      <td>310</td>
    </tr>
    <tr>
      <th>St Leonards</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>170</td>
      <td>170</td>
      <td>3420</td>
      <td>8900</td>
      <td>3090</td>
      <td>3310</td>
      <td>8780</td>
      <td>3230</td>
      <td>2130</td>
      <td>1990</td>
      <td>17590</td>
      <td>17590</td>
      <td>13</td>
      <td>1405</td>
    </tr>
    <tr>
      <th>Blacktown</th>
      <td>Western</td>
      <td>2014</td>
      <td>14/10/2014</td>
      <td>Yes</td>
      <td>190</td>
      <td>210</td>
      <td>6460</td>
      <td>3370</td>
      <td>4270</td>
      <td>3450</td>
      <td>4600</td>
      <td>7090</td>
      <td>1040</td>
      <td>2450</td>
      <td>16560</td>
      <td>16560</td>
      <td>14</td>
      <td>911</td>
    </tr>
    <tr>
      <th>Burwood</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>9/09/2014</td>
      <td>Yes</td>
      <td>140</td>
      <td>140</td>
      <td>4850</td>
      <td>2930</td>
      <td>4470</td>
      <td>3670</td>
      <td>4020</td>
      <td>5060</td>
      <td>1510</td>
      <td>3190</td>
      <td>14990</td>
      <td>14990</td>
      <td>15</td>
      <td>1109</td>
    </tr>
    <tr>
      <th>Kings Cross</th>
      <td>Eastern Suburbs</td>
      <td>2014</td>
      <td>3/09/2013</td>
      <td>No</td>
      <td>120</td>
      <td>120</td>
      <td>3690</td>
      <td>3820</td>
      <td>3110</td>
      <td>2670</td>
      <td>4400</td>
      <td>3640</td>
      <td>1550</td>
      <td>2620</td>
      <td>12870</td>
      <td>12870</td>
      <td>16</td>
      <td>201</td>
    </tr>
    <tr>
      <th>Hornsby</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>14/05/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>100</td>
      <td>5550</td>
      <td>2260</td>
      <td>3060</td>
      <td>2390</td>
      <td>2750</td>
      <td>5200</td>
      <td>680</td>
      <td>2290</td>
      <td>12250</td>
      <td>12250</td>
      <td>17</td>
      <td>1417</td>
    </tr>
    <tr>
      <th>Ashfield</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>4/09/2013</td>
      <td>No</td>
      <td>100</td>
      <td>100</td>
      <td>5090</td>
      <td>2010</td>
      <td>2690</td>
      <td>2220</td>
      <td>2500</td>
      <td>4500</td>
      <td>820</td>
      <td>2370</td>
      <td>11200</td>
      <td>11200</td>
      <td>18</td>
      <td>1107</td>
    </tr>
    <tr>
      <th>Kogarah</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>19/06/2014</td>
      <td>Yes</td>
      <td>90</td>
      <td>100</td>
      <td>4000</td>
      <td>2970</td>
      <td>3150</td>
      <td>2320</td>
      <td>3210</td>
      <td>3450</td>
      <td>650</td>
      <td>2260</td>
      <td>11100</td>
      <td>11100</td>
      <td>19</td>
      <td>307</td>
    </tr>
    <tr>
      <th>Lidcombe</th>
      <td>South</td>
      <td>2014</td>
      <td>5/06/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>4570</td>
      <td>1620</td>
      <td>2870</td>
      <td>2210</td>
      <td>2550</td>
      <td>3900</td>
      <td>820</td>
      <td>3080</td>
      <td>10910</td>
      <td>10910</td>
      <td>20</td>
      <td>702</td>
    </tr>
    <tr>
      <th>Auburn</th>
      <td>Western</td>
      <td>2014</td>
      <td>19/11/2014</td>
      <td>Yes</td>
      <td>130</td>
      <td>60</td>
      <td>3880</td>
      <td>1790</td>
      <td>3220</td>
      <td>2900</td>
      <td>2700</td>
      <td>3890</td>
      <td>960</td>
      <td>2260</td>
      <td>10890</td>
      <td>10890</td>
      <td>21</td>
      <td>901</td>
    </tr>
    <tr>
      <th>Museum</th>
      <td>CBD</td>
      <td>2014</td>
      <td>22/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>800</td>
      <td>6290</td>
      <td>2220</td>
      <td>2340</td>
      <td>5980</td>
      <td>1230</td>
      <td>1760</td>
      <td>910</td>
      <td>10860</td>
      <td>10860</td>
      <td>22</td>
      <td>106</td>
    </tr>
    <tr>
      <th>Epping</th>
      <td>Northern via Strathfield</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>170</td>
      <td>90</td>
      <td>5700</td>
      <td>1580</td>
      <td>2130</td>
      <td>1600</td>
      <td>1970</td>
      <td>4920</td>
      <td>600</td>
      <td>2380</td>
      <td>10570</td>
      <td>10570</td>
      <td>23</td>
      <td>1208</td>
    </tr>
    <tr>
      <th>Macquarie University</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>1310</td>
      <td>3220</td>
      <td>2570</td>
      <td>3940</td>
      <td>4530</td>
      <td>1970</td>
      <td>1580</td>
      <td>860</td>
      <td>10090</td>
      <td>10090</td>
      <td>24</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>St James</th>
      <td>CBD</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>90</td>
      <td>780</td>
      <td>5880</td>
      <td>1900</td>
      <td>2160</td>
      <td>5630</td>
      <td>1120</td>
      <td>1440</td>
      <td>580</td>
      <td>9850</td>
      <td>9850</td>
      <td>25</td>
      <td>105</td>
    </tr>
    <tr>
      <th>Rockdale</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>28/05/2014</td>
      <td>Yes</td>
      <td>90</td>
      <td>90</td>
      <td>4660</td>
      <td>780</td>
      <td>2610</td>
      <td>2170</td>
      <td>1670</td>
      <td>3770</td>
      <td>610</td>
      <td>2820</td>
      <td>9640</td>
      <td>9640</td>
      <td>26</td>
      <td>306</td>
    </tr>
    <tr>
      <th>Cabramatta</th>
      <td>South</td>
      <td>2014</td>
      <td>11/06/2014</td>
      <td>Yes</td>
      <td>150</td>
      <td>80</td>
      <td>3780</td>
      <td>1070</td>
      <td>2760</td>
      <td>2820</td>
      <td>1750</td>
      <td>3760</td>
      <td>290</td>
      <td>1000</td>
      <td>8730</td>
      <td>8730</td>
      <td>27</td>
      <td>708</td>
    </tr>
    <tr>
      <th>Bankstown</th>
      <td>Bankstown</td>
      <td>2014</td>
      <td>13/05/2014</td>
      <td>Yes</td>
      <td>80</td>
      <td>60</td>
      <td>3010</td>
      <td>1810</td>
      <td>2400</td>
      <td>2570</td>
      <td>2840</td>
      <td>3130</td>
      <td>300</td>
      <td>1050</td>
      <td>8630</td>
      <td>8630</td>
      <td>28</td>
      <td>612</td>
    </tr>
    <tr>
      <th>Liverpool</th>
      <td>South</td>
      <td>2014</td>
      <td>4/06/2014</td>
      <td>Yes</td>
      <td>150</td>
      <td>80</td>
      <td>2510</td>
      <td>2270</td>
      <td>2510</td>
      <td>2260</td>
      <td>2610</td>
      <td>2650</td>
      <td>570</td>
      <td>1100</td>
      <td>8350</td>
      <td>8350</td>
      <td>29</td>
      <td>710</td>
    </tr>
    <tr>
      <th>Sutherland</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>12/06/2014</td>
      <td>Yes</td>
      <td>140</td>
      <td>70</td>
      <td>4260</td>
      <td>1460</td>
      <td>1920</td>
      <td>1650</td>
      <td>1750</td>
      <td>3900</td>
      <td>250</td>
      <td>1240</td>
      <td>8320</td>
      <td>8320</td>
      <td>30</td>
      <td>316</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Newcastle</th>
      <td>Newcastle</td>
      <td>2004</td>
      <td>19/06/2003</td>
      <td>No</td>
      <td>50</td>
      <td>20</td>
      <td>260</td>
      <td>320</td>
      <td>720</td>
      <td>850</td>
      <td>430</td>
      <td>200</td>
      <td>70</td>
      <td>140</td>
      <td>1530</td>
      <td>1530</td>
      <td>123</td>
      <td>1917</td>
    </tr>
    <tr>
      <th>Waratah</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>100</td>
      <td>100</td>
      <td>150</td>
      <td>150</td>
      <td>120</td>
      <td>120</td>
      <td>40</td>
      <td>30</td>
      <td>410</td>
      <td>410</td>
      <td>193</td>
      <td>2001</td>
    </tr>
    <tr>
      <th>Warabrook (University)</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>250</td>
      <td>120</td>
      <td>200</td>
      <td>300</td>
      <td>70</td>
      <td>90</td>
      <td>20</td>
      <td>540</td>
      <td>540</td>
      <td>185</td>
      <td>2002</td>
    </tr>
    <tr>
      <th>Sandgate</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>0</td>
      <td>0</td>
      <td>40</td>
      <td>40</td>
      <td>267</td>
      <td>2003</td>
    </tr>
    <tr>
      <th>Hexham</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>20</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>50</td>
      <td>50</td>
      <td>258</td>
      <td>2004</td>
    </tr>
    <tr>
      <th>Tarro</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>278</td>
      <td>2005</td>
    </tr>
    <tr>
      <th>Beresfield</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>180</td>
      <td>80</td>
      <td>140</td>
      <td>110</td>
      <td>70</td>
      <td>180</td>
      <td>10</td>
      <td>40</td>
      <td>400</td>
      <td>400</td>
      <td>195</td>
      <td>2006</td>
    </tr>
    <tr>
      <th>Thornton</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>150</td>
      <td>20</td>
      <td>60</td>
      <td>80</td>
      <td>50</td>
      <td>150</td>
      <td>10</td>
      <td>20</td>
      <td>270</td>
      <td>270</td>
      <td>211</td>
      <td>2007</td>
    </tr>
    <tr>
      <th>Metford</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>100</td>
      <td>20</td>
      <td>50</td>
      <td>50</td>
      <td>20</td>
      <td>90</td>
      <td>10</td>
      <td>10</td>
      <td>180</td>
      <td>180</td>
      <td>231</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>Victoria Street</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>190</td>
      <td>50</td>
      <td>120</td>
      <td>120</td>
      <td>70</td>
      <td>180</td>
      <td>20</td>
      <td>50</td>
      <td>400</td>
      <td>400</td>
      <td>195</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>East Maitland</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>50</td>
      <td>50</td>
      <td>40</td>
      <td>50</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>120</td>
      <td>120</td>
      <td>240</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>High Street</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>30</td>
      <td>40</td>
      <td>40</td>
      <td>40</td>
      <td>30</td>
      <td>40</td>
      <td>10</td>
      <td>0</td>
      <td>110</td>
      <td>110</td>
      <td>248</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>Maitland</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>10</td>
      <td>190</td>
      <td>100</td>
      <td>180</td>
      <td>210</td>
      <td>150</td>
      <td>180</td>
      <td>20</td>
      <td>40</td>
      <td>540</td>
      <td>540</td>
      <td>185</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>Telarah</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>110</td>
      <td>30</td>
      <td>60</td>
      <td>60</td>
      <td>50</td>
      <td>120</td>
      <td>20</td>
      <td>30</td>
      <td>240</td>
      <td>240</td>
      <td>218</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>Mindaribba</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>Paterson</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>Martins Creek</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>Hilldale</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>Wallarobba</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>Wirragulla</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>Dungog</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>20</td>
      <td>10</td>
      <td>20</td>
      <td>0</td>
      <td>10</td>
      <td>40</td>
      <td>40</td>
      <td>267</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>Lochinvar</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>Allandale</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>16/10/2001</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2022</td>
    </tr>
    <tr>
      <th>Greta</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2023</td>
    </tr>
    <tr>
      <th>Branxton</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>278</td>
      <td>2024</td>
    </tr>
    <tr>
      <th>Belford</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>295</td>
      <td>2025</td>
    </tr>
    <tr>
      <th>Singleton</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>30</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>30</td>
      <td>10</td>
      <td>10</td>
      <td>50</td>
      <td>50</td>
      <td>258</td>
      <td>2026</td>
    </tr>
    <tr>
      <th>Muswellbrook</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>30</td>
      <td>10</td>
      <td>0</td>
      <td>40</td>
      <td>10</td>
      <td>20</td>
      <td>50</td>
      <td>50</td>
      <td>258</td>
      <td>2027</td>
    </tr>
    <tr>
      <th>Aberdeen</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2028</td>
    </tr>
    <tr>
      <th>Scone</th>
      <td>Hunter</td>
      <td>2004</td>
      <td>24/08/2004</td>
      <td>Yes</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2029</td>
    </tr>
  </tbody>
</table>
<p>3367 rows × 18 columns</p>
</div>




```python
trains2014.IN_24_HOURS.idxmax()
```




    'Central'




```python
trains2014
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LINE</th>
      <th>YEAR</th>
      <th>SURVEY_DATE_USED</th>
      <th>WHETHER_SURVEYED</th>
      <th>IN_0200_0600</th>
      <th>OUT_0200_0600</th>
      <th>IN_0600_0930</th>
      <th>OUT_0600_0930</th>
      <th>IN_0930_1500</th>
      <th>OUT_0930_1500</th>
      <th>IN_1500_1830</th>
      <th>OUT_1500_1830</th>
      <th>IN_1830_0200</th>
      <th>OUT_1830_0200</th>
      <th>IN_24_HOURS</th>
      <th>OUT_24_HOURS</th>
      <th>RANK</th>
      <th>STATION_SORT_ID</th>
    </tr>
    <tr>
      <th>STATION</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Central</th>
      <td>CBD</td>
      <td>2014</td>
      <td>6/05/2014</td>
      <td>Yes</td>
      <td>940</td>
      <td>920</td>
      <td>10390</td>
      <td>42070</td>
      <td>21400</td>
      <td>30150</td>
      <td>45370</td>
      <td>16270</td>
      <td>19010</td>
      <td>7700</td>
      <td>97110</td>
      <td>97110</td>
      <td>1</td>
      <td>101</td>
    </tr>
    <tr>
      <th>Town Hall</th>
      <td>CBD</td>
      <td>2014</td>
      <td>7/05/2014</td>
      <td>Yes</td>
      <td>880</td>
      <td>880</td>
      <td>6380</td>
      <td>43210</td>
      <td>16930</td>
      <td>23550</td>
      <td>43670</td>
      <td>15270</td>
      <td>21760</td>
      <td>6720</td>
      <td>89620</td>
      <td>89620</td>
      <td>2</td>
      <td>102</td>
    </tr>
    <tr>
      <th>Wynyard</th>
      <td>CBD</td>
      <td>2014</td>
      <td>20/05/2014</td>
      <td>Yes</td>
      <td>590</td>
      <td>570</td>
      <td>4710</td>
      <td>39620</td>
      <td>9200</td>
      <td>11180</td>
      <td>35010</td>
      <td>6510</td>
      <td>10690</td>
      <td>2330</td>
      <td>60200</td>
      <td>60200</td>
      <td>3</td>
      <td>103</td>
    </tr>
    <tr>
      <th>Parramatta</th>
      <td>Western</td>
      <td>2014</td>
      <td>19/11/2014</td>
      <td>Yes</td>
      <td>660</td>
      <td>330</td>
      <td>7790</td>
      <td>13700</td>
      <td>7400</td>
      <td>7020</td>
      <td>14960</td>
      <td>8660</td>
      <td>4150</td>
      <td>5260</td>
      <td>34960</td>
      <td>34960</td>
      <td>4</td>
      <td>905</td>
    </tr>
    <tr>
      <th>North Sydney</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>270</td>
      <td>270</td>
      <td>2340</td>
      <td>18340</td>
      <td>4450</td>
      <td>5590</td>
      <td>17760</td>
      <td>3050</td>
      <td>3790</td>
      <td>1360</td>
      <td>28610</td>
      <td>28610</td>
      <td>5</td>
      <td>1402</td>
    </tr>
    <tr>
      <th>Redfern</th>
      <td>CBD</td>
      <td>2014</td>
      <td>1/05/2014</td>
      <td>Yes</td>
      <td>250</td>
      <td>250</td>
      <td>3910</td>
      <td>10690</td>
      <td>5350</td>
      <td>8170</td>
      <td>12510</td>
      <td>4250</td>
      <td>3660</td>
      <td>2320</td>
      <td>25680</td>
      <td>25680</td>
      <td>6</td>
      <td>108</td>
    </tr>
    <tr>
      <th>Chatswood</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>200</td>
      <td>5030</td>
      <td>8200</td>
      <td>4710</td>
      <td>4930</td>
      <td>9430</td>
      <td>6000</td>
      <td>2820</td>
      <td>2860</td>
      <td>22200</td>
      <td>22200</td>
      <td>7</td>
      <td>1407</td>
    </tr>
    <tr>
      <th>Bondi Junction</th>
      <td>Eastern Suburbs</td>
      <td>2014</td>
      <td>2/09/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>210</td>
      <td>10150</td>
      <td>4350</td>
      <td>4370</td>
      <td>4600</td>
      <td>5240</td>
      <td>8330</td>
      <td>1910</td>
      <td>4380</td>
      <td>21880</td>
      <td>21880</td>
      <td>8</td>
      <td>203</td>
    </tr>
    <tr>
      <th>Strathfield</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>9/09/2014</td>
      <td>Yes</td>
      <td>190</td>
      <td>190</td>
      <td>7900</td>
      <td>4050</td>
      <td>4980</td>
      <td>3950</td>
      <td>5540</td>
      <td>7230</td>
      <td>2100</td>
      <td>5280</td>
      <td>20710</td>
      <td>20710</td>
      <td>9</td>
      <td>1110</td>
    </tr>
    <tr>
      <th>Circular Quay</th>
      <td>CBD</td>
      <td>2014</td>
      <td>22/05/2014</td>
      <td>Yes</td>
      <td>200</td>
      <td>200</td>
      <td>2110</td>
      <td>8750</td>
      <td>4340</td>
      <td>6710</td>
      <td>9910</td>
      <td>3380</td>
      <td>4070</td>
      <td>1600</td>
      <td>20630</td>
      <td>20630</td>
      <td>10</td>
      <td>104</td>
    </tr>
    <tr>
      <th>Martin Place</th>
      <td>CBD</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>200</td>
      <td>190</td>
      <td>590</td>
      <td>14250</td>
      <td>2900</td>
      <td>3420</td>
      <td>12340</td>
      <td>1790</td>
      <td>4600</td>
      <td>970</td>
      <td>20630</td>
      <td>20630</td>
      <td>10</td>
      <td>107</td>
    </tr>
    <tr>
      <th>Hurstville</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>28/05/2014</td>
      <td>Yes</td>
      <td>160</td>
      <td>160</td>
      <td>7790</td>
      <td>2200</td>
      <td>5230</td>
      <td>4180</td>
      <td>3840</td>
      <td>7450</td>
      <td>1270</td>
      <td>4290</td>
      <td>18290</td>
      <td>18290</td>
      <td>12</td>
      <td>310</td>
    </tr>
    <tr>
      <th>St Leonards</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>170</td>
      <td>170</td>
      <td>3420</td>
      <td>8900</td>
      <td>3090</td>
      <td>3310</td>
      <td>8780</td>
      <td>3230</td>
      <td>2130</td>
      <td>1990</td>
      <td>17590</td>
      <td>17590</td>
      <td>13</td>
      <td>1405</td>
    </tr>
    <tr>
      <th>Blacktown</th>
      <td>Western</td>
      <td>2014</td>
      <td>14/10/2014</td>
      <td>Yes</td>
      <td>190</td>
      <td>210</td>
      <td>6460</td>
      <td>3370</td>
      <td>4270</td>
      <td>3450</td>
      <td>4600</td>
      <td>7090</td>
      <td>1040</td>
      <td>2450</td>
      <td>16560</td>
      <td>16560</td>
      <td>14</td>
      <td>911</td>
    </tr>
    <tr>
      <th>Burwood</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>9/09/2014</td>
      <td>Yes</td>
      <td>140</td>
      <td>140</td>
      <td>4850</td>
      <td>2930</td>
      <td>4470</td>
      <td>3670</td>
      <td>4020</td>
      <td>5060</td>
      <td>1510</td>
      <td>3190</td>
      <td>14990</td>
      <td>14990</td>
      <td>15</td>
      <td>1109</td>
    </tr>
    <tr>
      <th>Kings Cross</th>
      <td>Eastern Suburbs</td>
      <td>2014</td>
      <td>3/09/2013</td>
      <td>No</td>
      <td>120</td>
      <td>120</td>
      <td>3690</td>
      <td>3820</td>
      <td>3110</td>
      <td>2670</td>
      <td>4400</td>
      <td>3640</td>
      <td>1550</td>
      <td>2620</td>
      <td>12870</td>
      <td>12870</td>
      <td>16</td>
      <td>201</td>
    </tr>
    <tr>
      <th>Hornsby</th>
      <td>North Shore</td>
      <td>2014</td>
      <td>14/05/2014</td>
      <td>Yes</td>
      <td>210</td>
      <td>100</td>
      <td>5550</td>
      <td>2260</td>
      <td>3060</td>
      <td>2390</td>
      <td>2750</td>
      <td>5200</td>
      <td>680</td>
      <td>2290</td>
      <td>12250</td>
      <td>12250</td>
      <td>17</td>
      <td>1417</td>
    </tr>
    <tr>
      <th>Ashfield</th>
      <td>Inner West</td>
      <td>2014</td>
      <td>4/09/2013</td>
      <td>No</td>
      <td>100</td>
      <td>100</td>
      <td>5090</td>
      <td>2010</td>
      <td>2690</td>
      <td>2220</td>
      <td>2500</td>
      <td>4500</td>
      <td>820</td>
      <td>2370</td>
      <td>11200</td>
      <td>11200</td>
      <td>18</td>
      <td>1107</td>
    </tr>
    <tr>
      <th>Kogarah</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>19/06/2014</td>
      <td>Yes</td>
      <td>90</td>
      <td>100</td>
      <td>4000</td>
      <td>2970</td>
      <td>3150</td>
      <td>2320</td>
      <td>3210</td>
      <td>3450</td>
      <td>650</td>
      <td>2260</td>
      <td>11100</td>
      <td>11100</td>
      <td>19</td>
      <td>307</td>
    </tr>
    <tr>
      <th>Lidcombe</th>
      <td>South</td>
      <td>2014</td>
      <td>5/06/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>4570</td>
      <td>1620</td>
      <td>2870</td>
      <td>2210</td>
      <td>2550</td>
      <td>3900</td>
      <td>820</td>
      <td>3080</td>
      <td>10910</td>
      <td>10910</td>
      <td>20</td>
      <td>702</td>
    </tr>
    <tr>
      <th>Auburn</th>
      <td>Western</td>
      <td>2014</td>
      <td>19/11/2014</td>
      <td>Yes</td>
      <td>130</td>
      <td>60</td>
      <td>3880</td>
      <td>1790</td>
      <td>3220</td>
      <td>2900</td>
      <td>2700</td>
      <td>3890</td>
      <td>960</td>
      <td>2260</td>
      <td>10890</td>
      <td>10890</td>
      <td>21</td>
      <td>901</td>
    </tr>
    <tr>
      <th>Museum</th>
      <td>CBD</td>
      <td>2014</td>
      <td>22/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>800</td>
      <td>6290</td>
      <td>2220</td>
      <td>2340</td>
      <td>5980</td>
      <td>1230</td>
      <td>1760</td>
      <td>910</td>
      <td>10860</td>
      <td>10860</td>
      <td>22</td>
      <td>106</td>
    </tr>
    <tr>
      <th>Epping</th>
      <td>Northern via Strathfield</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>170</td>
      <td>90</td>
      <td>5700</td>
      <td>1580</td>
      <td>2130</td>
      <td>1600</td>
      <td>1970</td>
      <td>4920</td>
      <td>600</td>
      <td>2380</td>
      <td>10570</td>
      <td>10570</td>
      <td>23</td>
      <td>1208</td>
    </tr>
    <tr>
      <th>Macquarie University</th>
      <td>Northern via Macquarie Park</td>
      <td>2014</td>
      <td>21/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>100</td>
      <td>1310</td>
      <td>3220</td>
      <td>2570</td>
      <td>3940</td>
      <td>4530</td>
      <td>1970</td>
      <td>1580</td>
      <td>860</td>
      <td>10090</td>
      <td>10090</td>
      <td>24</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>St James</th>
      <td>CBD</td>
      <td>2014</td>
      <td>27/05/2014</td>
      <td>Yes</td>
      <td>100</td>
      <td>90</td>
      <td>780</td>
      <td>5880</td>
      <td>1900</td>
      <td>2160</td>
      <td>5630</td>
      <td>1120</td>
      <td>1440</td>
      <td>580</td>
      <td>9850</td>
      <td>9850</td>
      <td>25</td>
      <td>105</td>
    </tr>
    <tr>
      <th>Rockdale</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>28/05/2014</td>
      <td>Yes</td>
      <td>90</td>
      <td>90</td>
      <td>4660</td>
      <td>780</td>
      <td>2610</td>
      <td>2170</td>
      <td>1670</td>
      <td>3770</td>
      <td>610</td>
      <td>2820</td>
      <td>9640</td>
      <td>9640</td>
      <td>26</td>
      <td>306</td>
    </tr>
    <tr>
      <th>Cabramatta</th>
      <td>South</td>
      <td>2014</td>
      <td>11/06/2014</td>
      <td>Yes</td>
      <td>150</td>
      <td>80</td>
      <td>3780</td>
      <td>1070</td>
      <td>2760</td>
      <td>2820</td>
      <td>1750</td>
      <td>3760</td>
      <td>290</td>
      <td>1000</td>
      <td>8730</td>
      <td>8730</td>
      <td>27</td>
      <td>708</td>
    </tr>
    <tr>
      <th>Bankstown</th>
      <td>Bankstown</td>
      <td>2014</td>
      <td>13/05/2014</td>
      <td>Yes</td>
      <td>80</td>
      <td>60</td>
      <td>3010</td>
      <td>1810</td>
      <td>2400</td>
      <td>2570</td>
      <td>2840</td>
      <td>3130</td>
      <td>300</td>
      <td>1050</td>
      <td>8630</td>
      <td>8630</td>
      <td>28</td>
      <td>612</td>
    </tr>
    <tr>
      <th>Liverpool</th>
      <td>South</td>
      <td>2014</td>
      <td>4/06/2014</td>
      <td>Yes</td>
      <td>150</td>
      <td>80</td>
      <td>2510</td>
      <td>2270</td>
      <td>2510</td>
      <td>2260</td>
      <td>2610</td>
      <td>2650</td>
      <td>570</td>
      <td>1100</td>
      <td>8350</td>
      <td>8350</td>
      <td>29</td>
      <td>710</td>
    </tr>
    <tr>
      <th>Sutherland</th>
      <td>Illawarra</td>
      <td>2014</td>
      <td>12/06/2014</td>
      <td>Yes</td>
      <td>140</td>
      <td>70</td>
      <td>4260</td>
      <td>1460</td>
      <td>1920</td>
      <td>1650</td>
      <td>1750</td>
      <td>3900</td>
      <td>250</td>
      <td>1240</td>
      <td>8320</td>
      <td>8320</td>
      <td>30</td>
      <td>316</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Menangle Park</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>19/06/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>277</td>
      <td>1601</td>
    </tr>
    <tr>
      <th>Port Kembla North</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>18/09/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>277</td>
      <td>1521</td>
    </tr>
    <tr>
      <th>Scone</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>20</td>
      <td>20</td>
      <td>277</td>
      <td>2029</td>
    </tr>
    <tr>
      <th>Tarro</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>277</td>
      <td>2005</td>
    </tr>
    <tr>
      <th>Aberdeen</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2028</td>
    </tr>
    <tr>
      <th>Bombo</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>18/09/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1530</td>
    </tr>
    <tr>
      <th>Branxton</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2024</td>
    </tr>
    <tr>
      <th>Burradoo</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1610</td>
    </tr>
    <tr>
      <th>Coalcliff</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>9/08/2012</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1504</td>
    </tr>
    <tr>
      <th>Dunmore (Shellharbour)</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>18/06/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1528</td>
    </tr>
    <tr>
      <th>Exeter</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1612</td>
    </tr>
    <tr>
      <th>Greta</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2023</td>
    </tr>
    <tr>
      <th>Linden</th>
      <td>Blue Mountains</td>
      <td>2014</td>
      <td>19/08/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1708</td>
    </tr>
    <tr>
      <th>Marulan</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>17/06/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1617</td>
    </tr>
    <tr>
      <th>Menangle</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>17/06/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1602</td>
    </tr>
    <tr>
      <th>Paterson</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>Scarborough</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>22/08/2013</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1505</td>
    </tr>
    <tr>
      <th>Tallong</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1616</td>
    </tr>
    <tr>
      <th>Wondabyne</th>
      <td>Central Coast</td>
      <td>2014</td>
      <td>21/11/2012</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>10</td>
      <td>283</td>
      <td>1803</td>
    </tr>
    <tr>
      <th>Bell</th>
      <td>Blue Mountains</td>
      <td>2014</td>
      <td>0</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1719</td>
    </tr>
    <tr>
      <th>Hilldale</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>Kembla Grange Racecourse</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>15/02/1996</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1524</td>
    </tr>
    <tr>
      <th>Lochinvar</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>Lysaghts</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>12/11/2013</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1519</td>
    </tr>
    <tr>
      <th>Mindaribba</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>Penrose</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1614</td>
    </tr>
    <tr>
      <th>Wallarobba</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>Wingello</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1615</td>
    </tr>
    <tr>
      <th>Wirragulla</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>Zig Zag</th>
      <td>Blue Mountains</td>
      <td>2014</td>
      <td>9/10/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1720</td>
    </tr>
  </tbody>
</table>
<p>308 rows × 18 columns</p>
</div>




```python

```


```python

```


```python

```


```python
trains2014[trains2014.IN_24_HOURS == 0]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LINE</th>
      <th>YEAR</th>
      <th>SURVEY_DATE_USED</th>
      <th>WHETHER_SURVEYED</th>
      <th>IN_0200_0600</th>
      <th>OUT_0200_0600</th>
      <th>IN_0600_0930</th>
      <th>OUT_0600_0930</th>
      <th>IN_0930_1500</th>
      <th>OUT_0930_1500</th>
      <th>IN_1500_1830</th>
      <th>OUT_1500_1830</th>
      <th>IN_1830_0200</th>
      <th>OUT_1830_0200</th>
      <th>IN_24_HOURS</th>
      <th>OUT_24_HOURS</th>
      <th>RANK</th>
      <th>STATION_SORT_ID</th>
    </tr>
    <tr>
      <th>STATION</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bell</th>
      <td>Blue Mountains</td>
      <td>2014</td>
      <td>0</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1719</td>
    </tr>
    <tr>
      <th>Hilldale</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>Kembla Grange Racecourse</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>15/02/1996</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1524</td>
    </tr>
    <tr>
      <th>Lochinvar</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>Lysaghts</th>
      <td>South Coast</td>
      <td>2014</td>
      <td>12/11/2013</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1519</td>
    </tr>
    <tr>
      <th>Mindaribba</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>Penrose</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1614</td>
    </tr>
    <tr>
      <th>Wallarobba</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>Wingello</th>
      <td>Southern Highlands</td>
      <td>2014</td>
      <td>25/05/2010</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1615</td>
    </tr>
    <tr>
      <th>Wirragulla</th>
      <td>Hunter</td>
      <td>2014</td>
      <td>3/06/2009</td>
      <td>No</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>Zig Zag</th>
      <td>Blue Mountains</td>
      <td>2014</td>
      <td>9/10/2014</td>
      <td>Yes</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>298</td>
      <td>1720</td>
    </tr>
  </tbody>
</table>
</div>




```python
trains2014.to_csv('2014-data.csv')
```


```python
pandas.read_csv('/Users/Mahendra/Desktop/GA/SYD_DAT_7/data/2014-data.csv')
```


    ---------------------------------------------------------------------------

    IOError                                   Traceback (most recent call last)

    <ipython-input-69-bd3c6c26360f> in <module>()
    ----> 1 pandas.read_csv('/Users/Mahendra/Desktop/GA/SYD_DAT_7/data/2014-data.csv')
    

    //anaconda/lib/python2.7/site-packages/pandas/io/parsers.pyc in parser_f(filepath_or_buffer, sep, delimiter, header, names, index_col, usecols, squeeze, prefix, mangle_dupe_cols, dtype, engine, converters, true_values, false_values, skipinitialspace, skiprows, nrows, na_values, keep_default_na, na_filter, verbose, skip_blank_lines, parse_dates, infer_datetime_format, keep_date_col, date_parser, dayfirst, iterator, chunksize, compression, thousands, decimal, lineterminator, quotechar, quoting, escapechar, comment, encoding, dialect, tupleize_cols, error_bad_lines, warn_bad_lines, skipfooter, skip_footer, doublequote, delim_whitespace, as_recarray, compact_ints, use_unsigned, low_memory, buffer_lines, memory_map, float_precision)
        644                     skip_blank_lines=skip_blank_lines)
        645 
    --> 646         return _read(filepath_or_buffer, kwds)
        647 
        648     parser_f.__name__ = name


    //anaconda/lib/python2.7/site-packages/pandas/io/parsers.pyc in _read(filepath_or_buffer, kwds)
        387 
        388     # Create the parser.
    --> 389     parser = TextFileReader(filepath_or_buffer, **kwds)
        390 
        391     if (nrows is not None) and (chunksize is not None):


    //anaconda/lib/python2.7/site-packages/pandas/io/parsers.pyc in __init__(self, f, engine, **kwds)
        728             self.options['has_index_names'] = kwds['has_index_names']
        729 
    --> 730         self._make_engine(self.engine)
        731 
        732     def close(self):


    //anaconda/lib/python2.7/site-packages/pandas/io/parsers.pyc in _make_engine(self, engine)
        921     def _make_engine(self, engine='c'):
        922         if engine == 'c':
    --> 923             self._engine = CParserWrapper(self.f, **self.options)
        924         else:
        925             if engine == 'python':


    //anaconda/lib/python2.7/site-packages/pandas/io/parsers.pyc in __init__(self, src, **kwds)
       1388         kwds['allow_leading_cols'] = self.index_col is not False
       1389 
    -> 1390         self._reader = _parser.TextReader(src, **kwds)
       1391 
       1392         # XXX


    pandas/parser.pyx in pandas.parser.TextReader.__cinit__ (pandas/parser.c:4184)()


    pandas/parser.pyx in pandas.parser.TextReader._setup_parser_source (pandas/parser.c:8449)()


    IOError: File /Users/Mahendra/Desktop/GA/SYD_DAT_7/data/2014-data.csv does not exist



```python
trains2014.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YEAR</th>
      <th>IN_0200_0600</th>
      <th>OUT_0200_0600</th>
      <th>IN_0600_0930</th>
      <th>OUT_0600_0930</th>
      <th>IN_0930_1500</th>
      <th>OUT_0930_1500</th>
      <th>IN_1500_1830</th>
      <th>OUT_1500_1830</th>
      <th>IN_1830_0200</th>
      <th>OUT_1830_0200</th>
      <th>IN_24_HOURS</th>
      <th>OUT_24_HOURS</th>
      <th>RANK</th>
      <th>STATION_SORT_ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>308.0</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
      <td>308.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2014.0</td>
      <td>52.142857</td>
      <td>33.084416</td>
      <td>1176.266234</td>
      <td>1130.422078</td>
      <td>777.954545</td>
      <td>809.545455</td>
      <td>1226.818182</td>
      <td>1131.785714</td>
      <td>385.194805</td>
      <td>513.668831</td>
      <td>3618.538961</td>
      <td>3618.538961</td>
      <td>153.516234</td>
      <td>1191.840909</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.0</td>
      <td>107.324369</td>
      <td>90.671936</td>
      <td>1626.940742</td>
      <td>4482.948456</td>
      <td>1901.729550</td>
      <td>2470.684364</td>
      <td>4508.392060</td>
      <td>1869.717983</td>
      <td>1835.041549</td>
      <td>944.926715</td>
      <td>9291.017771</td>
      <td>9291.017771</td>
      <td>87.893460</td>
      <td>577.163228</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2014.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>101.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2014.0</td>
      <td>10.000000</td>
      <td>0.000000</td>
      <td>97.500000</td>
      <td>20.000000</td>
      <td>40.000000</td>
      <td>30.000000</td>
      <td>30.000000</td>
      <td>87.500000</td>
      <td>10.000000</td>
      <td>30.000000</td>
      <td>190.000000</td>
      <td>190.000000</td>
      <td>77.750000</td>
      <td>617.750000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2014.0</td>
      <td>20.000000</td>
      <td>10.000000</td>
      <td>520.000000</td>
      <td>150.000000</td>
      <td>245.000000</td>
      <td>195.000000</td>
      <td>200.000000</td>
      <td>475.000000</td>
      <td>40.000000</td>
      <td>175.000000</td>
      <td>1090.000000</td>
      <td>1090.000000</td>
      <td>154.500000</td>
      <td>1307.500000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2014.0</td>
      <td>50.000000</td>
      <td>30.000000</td>
      <td>1650.000000</td>
      <td>602.500000</td>
      <td>662.500000</td>
      <td>627.500000</td>
      <td>657.500000</td>
      <td>1392.500000</td>
      <td>120.000000</td>
      <td>550.000000</td>
      <td>3265.000000</td>
      <td>3265.000000</td>
      <td>231.000000</td>
      <td>1703.250000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2014.0</td>
      <td>940.000000</td>
      <td>920.000000</td>
      <td>10390.000000</td>
      <td>43210.000000</td>
      <td>21400.000000</td>
      <td>30150.000000</td>
      <td>45370.000000</td>
      <td>16270.000000</td>
      <td>21760.000000</td>
      <td>7700.000000</td>
      <td>97110.000000</td>
      <td>97110.000000</td>
      <td>298.000000</td>
      <td>2029.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
trains2014.LINE.value_counts()
```




    South Coast                    34
    Western                        29
    Hunter                         27
    Illawarra                      26
    Blue Mountains                 22
    North Shore                    21
    Bankstown                      21
    East Hills                     19
    Southern Highlands             18
    Newcastle                      17
    Central Coast                  14
    Inner West                     11
    South                          11
    Northern via Strathfield        8
    CBD                             8
    Northern via Macquarie Park     8
    Carlingford                     6
    Airport                         4
    Eastern Suburbs                 3
    Olympic Park                    1
    Name: LINE, dtype: int64




```python

```
