# Contains cleaning, transformation and detailed analysis of entire IIT Bombay Placement-2021 data. It contains exclusive key insights that are not available publicly.</br></br>
## Importing Libraries:-
- Pandas
- NumPy
- Regex


```python
import pandas as pd
import numpy as np
```


```python
import re
```

#### 
## Importing Data File:-
- Initial inspection of data.
- Understanding data.
#### 


```python
df = pd.read_csv("allocations.csv")
df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Slot</th>
      <th>Roll</th>
      <th>Student Name</th>
      <th>Department</th>
      <th>Program</th>
      <th>Company</th>
      <th>Job code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.1</td>
      <td>183059003</td>
      <td>ATHUL S NAMBIAR</td>
      <td>Computer Science &amp; Engineering</td>
      <td>M.Tech.</td>
      <td>Colortokens</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.1</td>
      <td>193100039</td>
      <td>Akash Nandalal Gajbhiye</td>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10.1</td>
      <td>195280009</td>
      <td>Akshi Bansal</td>
      <td>Applied Statistics and Informatics</td>
      <td>M.Sc.</td>
      <td>NeuralTechSoft</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.1</td>
      <td>193070021</td>
      <td>Aman Kumar</td>
      <td>Electrical Engineering</td>
      <td>M.Tech.</td>
      <td>Ignitarium Technology Solutions Pvt Ltd</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10.1</td>
      <td>193060032</td>
      <td>Anmol Mishra</td>
      <td>Earth Sciences</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.shape
```




    (1139, 7)




```python
df["Slot"].describe()
```




    count     1139
    unique      23
    top          .
    freq       314
    Name: Slot, dtype: object




```python
df["Slot"].value_counts()
```




    .            314
    3.1          100
    1.1          100
    2.2           94
    2.1           92
    5.1           68
    1.2           59
    10.1          45
    3.2           33
    4.2           30
    7.1           27
    6.1           25
    4.1           24
    8.1           23
    14.1          19
    9.1           19
    3 (IDC).1     18
    1 (IDC).1     12
    2 (IDC).1     11
    4 (IDC).1      7
    11.1           7
    12.1           7
    15.1           5
    Name: Slot, dtype: int64




```python
df["Slot"].dtypes
```




    dtype('O')



### 
## Transformation of data:-
### 


```python
def convert(x):
    x = x.replace(" (IDC)","00")
    if x == ".":
        x = "16.3"
    return x
```


```python
convert("3 (IDC).1")
```




    '300.1'




```python
df["Slot"] = df["Slot"].apply(convert)
```

#### 
### Day 16 means 2nd phase placements, Slot 1 is from morning to evening, slot 2 is from evening to late night, slot 3 is full day in phase 2. For IDC, days are multiplied by 100 to differentiate from general placements; like "Day 2' for IDC is represented as "Day 200" and so on.
#### 


```python
df[["Day","Slot No."]] = df["Slot"].str.split(".",expand = True)
df.head(10)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Slot</th>
      <th>Roll</th>
      <th>Student Name</th>
      <th>Department</th>
      <th>Program</th>
      <th>Company</th>
      <th>Job code</th>
      <th>Day</th>
      <th>Slot No.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.1</td>
      <td>183059003</td>
      <td>ATHUL S NAMBIAR</td>
      <td>Computer Science &amp; Engineering</td>
      <td>M.Tech.</td>
      <td>Colortokens</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.1</td>
      <td>193100039</td>
      <td>Akash Nandalal Gajbhiye</td>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10.1</td>
      <td>195280009</td>
      <td>Akshi Bansal</td>
      <td>Applied Statistics and Informatics</td>
      <td>M.Sc.</td>
      <td>NeuralTechSoft</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.1</td>
      <td>193070021</td>
      <td>Aman Kumar</td>
      <td>Electrical Engineering</td>
      <td>M.Tech.</td>
      <td>Ignitarium Technology Solutions Pvt Ltd</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10.1</td>
      <td>193060032</td>
      <td>Anmol Mishra</td>
      <td>Earth Sciences</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10.1</td>
      <td>183310001</td>
      <td>Anshul Shrivastava</td>
      <td>Geoinformatics and Natural Resources Engineering</td>
      <td>M.Tech.</td>
      <td>Regent Climate Connect Knowledge Solutions Pri...</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10.1</td>
      <td>193050057</td>
      <td>Ashwani Yadav</td>
      <td>Computer Science &amp; Engineering</td>
      <td>M.Tech.</td>
      <td>Colortokens</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10.1</td>
      <td>170070003</td>
      <td>Atharv Amar Pawar</td>
      <td>Electrical Engineering</td>
      <td>B.Tech.</td>
      <td>BeeHyv</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10.1</td>
      <td>193170012</td>
      <td>BHAVIK DURUGKAR</td>
      <td>Energy Science and Engineering</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10.1</td>
      <td>183109005</td>
      <td>Birla Sandesh Sandeep</td>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>Applied Materials</td>
      <td>2</td>
      <td>10</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.tail()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Slot</th>
      <th>Roll</th>
      <th>Student Name</th>
      <th>Department</th>
      <th>Program</th>
      <th>Company</th>
      <th>Job code</th>
      <th>Day</th>
      <th>Slot No.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1134</th>
      <td>16.3</td>
      <td>170050005</td>
      <td>Yateesh Agrawal</td>
      <td>Computer Science &amp; Engineering</td>
      <td>B.Tech.</td>
      <td>Placement Office</td>
      <td>4</td>
      <td>16</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1135</th>
      <td>16.3</td>
      <td>170100028</td>
      <td>Yogender Singh</td>
      <td>Mechanical Engineering</td>
      <td>B.Tech.</td>
      <td>Placement Office</td>
      <td>4</td>
      <td>16</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1136</th>
      <td>16.3</td>
      <td>193100059</td>
      <td>Yogesh Supe</td>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>BYJU'S - The Learning App</td>
      <td>1</td>
      <td>16</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1137</th>
      <td>16.3</td>
      <td>193100075</td>
      <td>Zeba Malik</td>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>Solar Energy Corporation Of India Limited</td>
      <td>2</td>
      <td>16</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1138</th>
      <td>16.3</td>
      <td>16D070061</td>
      <td>chandan kumar</td>
      <td>Electrical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>Accenture</td>
      <td>1</td>
      <td>16</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.drop(columns = "Slot", inplace=True)
```


```python
df["Day"] = pd.to_numeric(df["Day"])
```


```python
df["Slot No."] = pd.to_numeric(df["Slot No."])
```


```python
df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Roll</th>
      <th>Student Name</th>
      <th>Department</th>
      <th>Program</th>
      <th>Company</th>
      <th>Job code</th>
      <th>Day</th>
      <th>Slot No.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183059003</td>
      <td>ATHUL S NAMBIAR</td>
      <td>Computer Science &amp; Engineering</td>
      <td>M.Tech.</td>
      <td>Colortokens</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>193100039</td>
      <td>Akash Nandalal Gajbhiye</td>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>195280009</td>
      <td>Akshi Bansal</td>
      <td>Applied Statistics and Informatics</td>
      <td>M.Sc.</td>
      <td>NeuralTechSoft</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>193070021</td>
      <td>Aman Kumar</td>
      <td>Electrical Engineering</td>
      <td>M.Tech.</td>
      <td>Ignitarium Technology Solutions Pvt Ltd</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>193060032</td>
      <td>Anmol Mishra</td>
      <td>Earth Sciences</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>1</td>
      <td>10</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.drop(columns = "Job code", inplace=True)
```


```python
df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Roll</th>
      <th>Student Name</th>
      <th>Department</th>
      <th>Program</th>
      <th>Company</th>
      <th>Day</th>
      <th>Slot No.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183059003</td>
      <td>ATHUL S NAMBIAR</td>
      <td>Computer Science &amp; Engineering</td>
      <td>M.Tech.</td>
      <td>Colortokens</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>193100039</td>
      <td>Akash Nandalal Gajbhiye</td>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>195280009</td>
      <td>Akshi Bansal</td>
      <td>Applied Statistics and Informatics</td>
      <td>M.Sc.</td>
      <td>NeuralTechSoft</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>193070021</td>
      <td>Aman Kumar</td>
      <td>Electrical Engineering</td>
      <td>M.Tech.</td>
      <td>Ignitarium Technology Solutions Pvt Ltd</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>193060032</td>
      <td>Anmol Mishra</td>
      <td>Earth Sciences</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>10</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### 
## Final inspection of data after the tranformation:-
### 


```python
df_BTech = df[df.Program == "B.Tech."]
df_BTech.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Roll</th>
      <th>Student Name</th>
      <th>Department</th>
      <th>Program</th>
      <th>Company</th>
      <th>Day</th>
      <th>Slot No.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>170070003</td>
      <td>Atharv Amar Pawar</td>
      <td>Electrical Engineering</td>
      <td>B.Tech.</td>
      <td>BeeHyv</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>160110047</td>
      <td>Lokendra Kumar Pushpaj</td>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>B.Tech.</td>
      <td>Agrifi</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>170050085</td>
      <td>Lukka Govinda Siva Nagadev</td>
      <td>Computer Science &amp; Engineering</td>
      <td>B.Tech.</td>
      <td>Agrifi</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19</th>
      <td>170020120</td>
      <td>Manish Saich</td>
      <td>Chemical Engineering</td>
      <td>B.Tech.</td>
      <td>BeeHyv</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>23</th>
      <td>170100087</td>
      <td>Pavankumar Kotnana</td>
      <td>Mechanical Engineering</td>
      <td>B.Tech.</td>
      <td>BeeHyv</td>
      <td>10</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_MTech = df[df.Program == "M.Tech."]
df_MTech.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Roll</th>
      <th>Student Name</th>
      <th>Department</th>
      <th>Program</th>
      <th>Company</th>
      <th>Day</th>
      <th>Slot No.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183059003</td>
      <td>ATHUL S NAMBIAR</td>
      <td>Computer Science &amp; Engineering</td>
      <td>M.Tech.</td>
      <td>Colortokens</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>193100039</td>
      <td>Akash Nandalal Gajbhiye</td>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>193070021</td>
      <td>Aman Kumar</td>
      <td>Electrical Engineering</td>
      <td>M.Tech.</td>
      <td>Ignitarium Technology Solutions Pvt Ltd</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>193060032</td>
      <td>Anmol Mishra</td>
      <td>Earth Sciences</td>
      <td>M.Tech.</td>
      <td>Impact Guru Technology Ventures Pvt. Ltd.</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>183310001</td>
      <td>Anshul Shrivastava</td>
      <td>Geoinformatics and Natural Resources Engineering</td>
      <td>M.Tech.</td>
      <td>Regent Climate Connect Knowledge Solutions Pri...</td>
      <td>10</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_Dual = df[df.Program.str.contains("Dual Degree")]
df_Dual.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Roll</th>
      <th>Student Name</th>
      <th>Department</th>
      <th>Program</th>
      <th>Company</th>
      <th>Day</th>
      <th>Slot No.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>16D070026</td>
      <td>Ghanshyam</td>
      <td>Electrical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>Ignitarium Technology Solutions Pvt Ltd</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>36</th>
      <td>16D070027</td>
      <td>Sudhir Kumar Suman</td>
      <td>Electrical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>Agrifi</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>38</th>
      <td>160110023</td>
      <td>Tuhin Saha</td>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>BeeHyv</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>55</th>
      <td>160110071</td>
      <td>Adarsh Kumar</td>
      <td>Electrical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>Boston Consulting Group</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>56</th>
      <td>16D170019</td>
      <td>Aditi Mahajan</td>
      <td>Energy Science and Engineering, Energy Science...</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>Procter and Gamble</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1139 entries, 0 to 1138
    Data columns (total 7 columns):
     #   Column        Non-Null Count  Dtype 
    ---  ------        --------------  ----- 
     0   Roll          1139 non-null   object
     1   Student Name  1139 non-null   object
     2   Department    1139 non-null   object
     3   Program       1139 non-null   object
     4   Company       1139 non-null   object
     5   Day           1139 non-null   int64 
     6   Slot No.      1139 non-null   int64 
    dtypes: int64(2), object(5)
    memory usage: 62.4+ KB
    

#### 
##  Applying aggregation functions to group the data with similar features:-
### 1. Drawing insights from the data.
### 2. Storing insights in form of DataFrames.
#### 

## Slot wise analysis:-


```python
slot_data = df.groupby("Slot No.",as_index=0)["Roll"].count()
slot_data = slot_data.reset_index(drop = 1)
slot_data.rename({"Roll":"No. of Students"},axis=1,inplace=True)
slot_data                                                                                         # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Slot No.</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>609</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>216</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>314</td>
    </tr>
  </tbody>
</table>
</div>



## Department and Program wise analysis:-


```python
Department_Stats_Breakdown = df.groupby(["Department","Program"], as_index = False)["Roll"].count().sort_values(["Roll","Program","Department"], ascending = [False,True,True])
Department_Stats_Breakdown.reset_index(drop = 1, inplace = True)
Department_Stats_Breakdown.head(15)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Department</th>
      <th>Program</th>
      <th>Roll</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Computer Science &amp; Engineering</td>
      <td>B.Tech.</td>
      <td>108</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Electrical Engineering</td>
      <td>M.Tech.</td>
      <td>101</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Computer Science &amp; Engineering</td>
      <td>M.Tech.</td>
      <td>88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mechanical Engineering</td>
      <td>B.Tech.</td>
      <td>78</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chemical Engineering</td>
      <td>B.Tech.</td>
      <td>73</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Electrical Engineering</td>
      <td>B.Tech.</td>
      <td>57</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Civil Engineering</td>
      <td>B.Tech.</td>
      <td>56</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Electrical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>48</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Industrial Design Centre</td>
      <td>M.Des.</td>
      <td>48</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>B.Tech.</td>
      <td>47</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Civil Engineering</td>
      <td>M.Tech.</td>
      <td>36</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>36</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Aerospace Engineering</td>
      <td>B.Tech.</td>
      <td>28</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>M.Tech.</td>
      <td>24</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Mechanical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>



## Program wise analysis:-
### 1. Number count.
### 2. Percentage count.


```python
Program_Stats = df.groupby("Program",as_index = 0)["Roll"].count().sort_values("Roll",ascending = False)
Program_Stats.reset_index(drop = True, inplace= True)
Program_Stats.rename({"Roll":"No. of Students"},axis=1,inplace=True)
Program_Stats                                                                                    # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Program</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>B.Tech.</td>
      <td>466</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M.Tech.</td>
      <td>398</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>132</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M.Des.</td>
      <td>48</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M.Sc.</td>
      <td>37</td>
    </tr>
    <tr>
      <th>5</th>
      <td>B.S.</td>
      <td>20</td>
    </tr>
    <tr>
      <th>6</th>
      <td>B.Des.</td>
      <td>12</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ph.D.</td>
      <td>10</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Dual Degree (B.Des. + M. Des.)</td>
      <td>9</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Dual Degree (M.Tech. + Ph.D.)</td>
      <td>4</td>
    </tr>
    <tr>
      <th>10</th>
      <td>M.P.P</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>M.Phil.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>M.S. by research (Exit Degree)</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
Program_Stats_Percentage = pd.DataFrame(round(df.value_counts("Program", normalize = True)*100,2))
```


```python
Program_Stats_Percentage.reset_index(inplace = True)
Program_Stats_Percentage.rename({0:"% of Students"},axis=1,inplace=True)
Program_Stats_Percentage                                                                       # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Program</th>
      <th>% of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>B.Tech.</td>
      <td>40.91</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M.Tech.</td>
      <td>34.94</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>11.59</td>
    </tr>
    <tr>
      <th>3</th>
      <td>M.Des.</td>
      <td>4.21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>M.Sc.</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>B.S.</td>
      <td>1.76</td>
    </tr>
    <tr>
      <th>6</th>
      <td>B.Des.</td>
      <td>1.05</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ph.D.</td>
      <td>0.88</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Dual Degree (B.Des. + M. Des.)</td>
      <td>0.79</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Dual Degree (M.Tech. + Ph.D.)</td>
      <td>0.35</td>
    </tr>
    <tr>
      <th>10</th>
      <td>M.P.P</td>
      <td>0.09</td>
    </tr>
    <tr>
      <th>11</th>
      <td>M.Phil.</td>
      <td>0.09</td>
    </tr>
    <tr>
      <th>12</th>
      <td>M.S. by research (Exit Degree)</td>
      <td>0.09</td>
    </tr>
  </tbody>
</table>
</div>



## Company, Day and Slot wise mixed analysis:-


```python
company_stats = df.groupby(["Company","Day","Slot No."],as_index=False)["Roll"].count().sort_values(["Roll","Company"], ascending = [0,1])
company_stats.reset_index(drop = True, inplace=True)
company_stats.shape
```




    (285, 4)




```python
company_stats[company_stats["Company"].str.contains("Hp",flags=re.I, regex=True)]
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Company</th>
      <th>Day</th>
      <th>Slot No.</th>
      <th>Roll</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
company_stats.rename({"Roll":"No. of Students"},axis=1,inplace=True)
company_stats.head(10)                                                                                 # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Company</th>
      <th>Day</th>
      <th>Slot No.</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Placement Office</td>
      <td>16</td>
      <td>3</td>
      <td>147</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Oracle</td>
      <td>2</td>
      <td>1</td>
      <td>23</td>
    </tr>
    <tr>
      <th>2</th>
      <td>TATA Projects</td>
      <td>3</td>
      <td>1</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Texas Instruments</td>
      <td>1</td>
      <td>1</td>
      <td>15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Axis Bank</td>
      <td>3</td>
      <td>1</td>
      <td>13</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Google India Pvt. Ltd.</td>
      <td>1</td>
      <td>1</td>
      <td>13</td>
    </tr>
    <tr>
      <th>6</th>
      <td>ICICI Lombard GIC Ltd.</td>
      <td>5</td>
      <td>1</td>
      <td>12</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Micron Technologies Operation India LLP</td>
      <td>3</td>
      <td>1</td>
      <td>12</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Bridgei2i Analytics Solutions</td>
      <td>16</td>
      <td>3</td>
      <td>11</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ICICI Bank</td>
      <td>3</td>
      <td>1</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
company_stats.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 285 entries, 0 to 284
    Data columns (total 4 columns):
     #   Column           Non-Null Count  Dtype 
    ---  ------           --------------  ----- 
     0   Company          285 non-null    object
     1   Day              285 non-null    int64 
     2   Slot No.         285 non-null    int64 
     3   No. of Students  285 non-null    int64 
    dtypes: int64(3), object(1)
    memory usage: 9.0+ KB
    


```python
company_stats["Company"].nunique()
```




    258




```python
company_stats["Company"].count()
```




    285



## Finding all the companies who recruited more than once:-


```python
repeated_companies = company_stats[company_stats.duplicated(subset="Company", keep=False)]
repeated_companies = repeated_companies.sort_values("Company")
repeated_companies.reset_index(drop=True,inplace=True)
repeated_companies.head()                                                                            # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Company</th>
      <th>Day</th>
      <th>Slot No.</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ANZ Bank</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ANZ Bank</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ANZ Bank</td>
      <td>3</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Applied Materials</td>
      <td>10</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Applied Materials</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
repeated_companies_total = repeated_companies.groupby(["Company"],as_index=False)["No. of Students"].sum()
repeated_companies_total = repeated_companies_total.sort_values(["No. of Students","Company"],ascending=[0,1])
repeated_companies_total.reset_index(drop=True,inplace=True)
repeated_companies_total                                                                            # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Company</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Tata Consultancy Services</td>
      <td>26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Axis Bank</td>
      <td>20</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Intel</td>
      <td>12</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Microsoft India Pvt. Ltd.</td>
      <td>12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ANZ Bank</td>
      <td>11</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Qualcomm</td>
      <td>11</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Enphase Energy</td>
      <td>10</td>
    </tr>
    <tr>
      <th>7</th>
      <td>HCL Technologies Ltd.</td>
      <td>10</td>
    </tr>
    <tr>
      <th>8</th>
      <td>IDFC First Bank</td>
      <td>10</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Jaguar Land Rover India Limited</td>
      <td>10</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Bajaj Auto Ltd</td>
      <td>8</td>
    </tr>
    <tr>
      <th>11</th>
      <td>LnT Infotech</td>
      <td>8</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Samsung Research Institute, Bangalore</td>
      <td>8</td>
    </tr>
    <tr>
      <th>13</th>
      <td>pharmaACE- Analytics Centre of Excellence</td>
      <td>8</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Flipkart</td>
      <td>6</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Infosys</td>
      <td>5</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Navi Technologies Pvt Ltd</td>
      <td>5</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Applied Materials</td>
      <td>4</td>
    </tr>
    <tr>
      <th>18</th>
      <td>HiLabs Inc.</td>
      <td>4</td>
    </tr>
    <tr>
      <th>19</th>
      <td>KLA Tencor</td>
      <td>4</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Procter and Gamble</td>
      <td>4</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Sprinklr</td>
      <td>4</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Quantsapp Private limited</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



## Top recruiter Companies for the entire season:-


```python
top_companies = company_stats.groupby("Company",as_index=False)["No. of Students"].sum()
top_companies = top_companies.sort_values(["No. of Students","Company"],ascending=[0,1])
top_companies.reset_index(drop=True,inplace=True)
top_companies.head(20)                                                                          # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Company</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Placement Office</td>
      <td>147</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Tata Consultancy Services</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Oracle</td>
      <td>23</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Axis Bank</td>
      <td>20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>TATA Projects</td>
      <td>16</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Texas Instruments</td>
      <td>15</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Google India Pvt. Ltd.</td>
      <td>13</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ICICI Lombard GIC Ltd.</td>
      <td>12</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Intel</td>
      <td>12</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Micron Technologies Operation India LLP</td>
      <td>12</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Microsoft India Pvt. Ltd.</td>
      <td>12</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ANZ Bank</td>
      <td>11</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Bridgei2i Analytics Solutions</td>
      <td>11</td>
    </tr>
    <tr>
      <th>13</th>
      <td>ICICI Bank</td>
      <td>11</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Qualcomm</td>
      <td>11</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Accenture</td>
      <td>10</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Enphase Energy</td>
      <td>10</td>
    </tr>
    <tr>
      <th>17</th>
      <td>ExxonMobil Lubricants Private Limited</td>
      <td>10</td>
    </tr>
    <tr>
      <th>18</th>
      <td>HCL Technologies Ltd.</td>
      <td>10</td>
    </tr>
    <tr>
      <th>19</th>
      <td>IDFC First Bank</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>



## Top branches:-


```python
def dept_splitter(x):
    splitted = x.split(", ")
    if len(splitted)==1:
        return x
    elif splitted[0]==splitted[1]:
        return splitted[0]
    else:
        return x
```


```python
top_branches = Department_Stats_Breakdown.copy()
top_branches["Department"] = top_branches["Department"].apply(dept_splitter)
top_branches = top_branches.rename({"Roll":"No. of Students"},axis=1)
top_branches.head(20)                                                                           # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Department</th>
      <th>Program</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Computer Science &amp; Engineering</td>
      <td>B.Tech.</td>
      <td>108</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Electrical Engineering</td>
      <td>M.Tech.</td>
      <td>101</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Computer Science &amp; Engineering</td>
      <td>M.Tech.</td>
      <td>88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mechanical Engineering</td>
      <td>B.Tech.</td>
      <td>78</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chemical Engineering</td>
      <td>B.Tech.</td>
      <td>73</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Electrical Engineering</td>
      <td>B.Tech.</td>
      <td>57</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Civil Engineering</td>
      <td>B.Tech.</td>
      <td>56</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Electrical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>48</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Industrial Design Centre</td>
      <td>M.Des.</td>
      <td>48</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>B.Tech.</td>
      <td>47</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Civil Engineering</td>
      <td>M.Tech.</td>
      <td>36</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Mechanical Engineering</td>
      <td>M.Tech.</td>
      <td>36</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Aerospace Engineering</td>
      <td>B.Tech.</td>
      <td>28</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>M.Tech.</td>
      <td>24</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Mechanical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Applied Statistics and Informatics</td>
      <td>M.Sc.</td>
      <td>23</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Geoinformatics and Natural Resources Engineering</td>
      <td>M.Tech.</td>
      <td>23</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Aerospace Engineering</td>
      <td>M.Tech.</td>
      <td>20</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Engineering Physics</td>
      <td>B.Tech.</td>
      <td>19</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Energy Science and Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>16</td>
    </tr>
  </tbody>
</table>
</div>



## Irrespective of Programs:-


```python
top_branches_all = top_branches.groupby("Department",as_index=False)["No. of Students"].sum()
top_branches_all = top_branches_all.sort_values(["No. of Students","Department"],ascending=[0,1])
top_branches_all.reset_index(drop = True, inplace = True)                         # EXPORT AS SHEET
top_branches_all.head(10)
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Department</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Electrical Engineering</td>
      <td>219</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Computer Science &amp; Engineering</td>
      <td>198</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mechanical Engineering</td>
      <td>144</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Civil Engineering</td>
      <td>97</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chemical Engineering</td>
      <td>89</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>86</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Industrial Design Centre</td>
      <td>69</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Aerospace Engineering</td>
      <td>54</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Energy Science and Engineering</td>
      <td>30</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Engineering Physics</td>
      <td>24</td>
    </tr>
  </tbody>
</table>
</div>



## Program wise:-


```python
top_branches_BTech = top_branches.loc[top_branches.Program == "B.Tech.",["Department","No. of Students"]]
top_branches_BTech.reset_index(drop = True, inplace=True)
top_branches_BTech                                                                                # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Department</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Computer Science &amp; Engineering</td>
      <td>108</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mechanical Engineering</td>
      <td>78</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Chemical Engineering</td>
      <td>73</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Electrical Engineering</td>
      <td>57</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Civil Engineering</td>
      <td>56</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>47</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Aerospace Engineering</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Engineering Physics</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_branches_MTech = top_branches.loc[top_branches.Program == "M.Tech.",["Department","No. of Students"]]
top_branches_MTech.reset_index(drop = True, inplace=True)
top_branches_MTech                                                                                # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Department</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Electrical Engineering</td>
      <td>101</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Computer Science &amp; Engineering</td>
      <td>88</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Civil Engineering</td>
      <td>36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mechanical Engineering</td>
      <td>36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Metallurgical Engineering and Materials Science</td>
      <td>24</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Geoinformatics and Natural Resources Engineering</td>
      <td>23</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Aerospace Engineering</td>
      <td>20</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Chemical Engineering</td>
      <td>15</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Industrial Engineering &amp; Operations Research</td>
      <td>15</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Energy Science and Engineering</td>
      <td>14</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Technology and Development</td>
      <td>8</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Environmental Science &amp; Engineering</td>
      <td>6</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Earth Sciences</td>
      <td>5</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Systems &amp; Control Engineering</td>
      <td>5</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Biosciences and Bioengineering</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_branches_other = top_branches.loc[~top_branches.Program.isin(["M.Tech.","B.Tech."])]
top_branches_other.reset_index(drop=True,inplace=True)
top_branches_other.head()                                                                     # EXPORT AS SHEET
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Department</th>
      <th>Program</th>
      <th>No. of Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Electrical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Industrial Design Centre</td>
      <td>M.Des.</td>
      <td>48</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mechanical Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>23</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Applied Statistics and Informatics</td>
      <td>M.Sc.</td>
      <td>23</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Energy Science and Engineering</td>
      <td>Dual Degree (B.Tech. + M.Tech.)</td>
      <td>16</td>
    </tr>
  </tbody>
</table>
</div>



#### 
# Exporting DataFrames into multiple Excel sheets of a single file:-
#### 


```python
'''
with pd.ExcelWriter("Allocation_Analysis.xlsx") as w:
    df_BTech.to_excel(w,sheet_name="B.Tech Details",index=False,freeze_panes=(1,0))
    df_MTech.to_excel(w,sheet_name="M.Tech Details",index=False,freeze_panes=(1,0))
    df_Dual.to_excel(w,sheet_name="Dual Degree Details",index=False,freeze_panes=(1,0))
    slot_data.to_excel(w,sheet_name="Slot Analysis",index=False,freeze_panes=(1,0))
    Program_Stats.to_excel(w,sheet_name="Program_Stats",index=False,freeze_panes=(1,0))
    Program_Stats_Percentage.to_excel(w,sheet_name="Program_Stats_Percentage",index=False,freeze_panes=(1,0))
    company_stats.to_excel(w,sheet_name="Company_Stats",index=False,freeze_panes=(1,0))
    repeated_companies.to_excel(w,sheet_name="Repeating Companies",index=False,freeze_panes=(1,0))
    repeated_companies_total.to_excel(w,sheet_name="Best repeating Companies",index=False,freeze_panes=(1,0))
    top_companies.to_excel(w,sheet_name="Top Recruiters",index=False,freeze_panes=(1,0))
    top_branches.to_excel(w,sheet_name="Best Program & Department",index=False,freeze_panes=(1,0))
    top_branches_all.to_excel(w,sheet_name="Overall Best Department",index=False,freeze_panes=(1,0))
    top_branches_BTech.to_excel(w,sheet_name="Best B.Tech Department",index=False,freeze_panes=(1,0))
    top_branches_MTech.to_excel(w,sheet_name="Best M.Tech Department",index=False,freeze_panes=(1,0))
    top_branches_other.to_excel(w,sheet_name="Best_Dept_other_Programs",index=False,freeze_panes=(1,0))
'''
```


```python

```
