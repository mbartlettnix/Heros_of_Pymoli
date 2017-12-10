
### Heroes Of Pymoli Data Analysis
 - Above 80% of all Heros of Pymoli players identify as male, but spend the least per user referencing normalized totals 
 - On average, no gender category purchases more items/person than another
 - 55% of all players are between the ages of 18-26


```python
import pandas as pd
import numpy as np
```


```python
data_file = "purchase_data.json"
data = pd.read_json(data_file,orient = 'columns')
#data.columns
```

** Player Count 


```python
uc = len(data['SN'].unique())
usercount = pd.DataFrame({'Total Players':uc},index=[0])
usercount
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



** Purchasing Analysis (Total)


```python
# Number of Unique Items 
itemcount = len(data['Item Name'].unique())

# Average Purchase Price 
avgprice = data['Price'].mean()

# Total Number of Purchases
purchasecnt = len(data['Price'])

# Total Revenue sum of purchases
totalRevenue = data['Price'].sum()

#push into DF
purchAnalysis = pd.DataFrame({'Number of Unique Items':itemcount,'Average Price':avgprice,
                              'Number of Purchases':purchasecnt,'Total Revenue':totalRevenue},index=[0])
#format currency
purchAnalysis['Average Price'] = purchAnalysis['Average Price'].map("${0:,.2f}".format)
purchAnalysis['Total Revenue'] = purchAnalysis['Total Revenue'].map("${0:,.2f}".format)

#format for correct order and new object format
purchAnalysis = purchAnalysis[['Number of Unique Items','Average Price','Number of Purchases','Total Revenue']]
purchAnalysis
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



** Gender Demographics


```python
# unique SN grouped by gender
new_data = data[['Gender', 'SN']].drop_duplicates().groupby(['Gender'])
#gender counts
gen_count = new_data['Gender'].count()
#gender percents
gen_perc = gen_count/gen_count.sum()*100
unique_gender = pd.DataFrame({"Percentage of Players":gen_perc,
                            "Total Count": gen_count})
unique_gender["Percentage of Players"] = unique_gender["Percentage of Players"].map("{0:,.2f}%".format)
# # unique_gender = unique_gender.set_index('Gender')

unique_gender
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



** Purchasing Analysis (Gender)


```python
gender_group = data.groupby(['Gender'])
gender_cnt = gender_group['Price'].count()
gender_avg = gender_group['Price'].mean()
gender_sum = gender_group['Price'].sum()
gender_norm = gender_sum/gen_count

gender_purchase = pd.DataFrame({"Purchase Count": gender_cnt,
                                "Average Purchase Price": gender_avg,
                                "Total Purchase Value":gender_sum,
                                "Normalized Totals": gender_norm
                                })

#Format
gender_purchase ['Average Purchase Price'] = gender_purchase ['Average Purchase Price'].map("${0:,.2f}".format)
gender_purchase ['Total Purchase Value'] = gender_purchase ['Total Purchase Value'].map("${0:,.2f}".format)
gender_purchase ['Normalized Totals'] = gender_purchase ['Normalized Totals'].map("${0:,.2f}".format)
#Reorder columns
gender_purchase = gender_purchase[['Purchase Count','Average Purchase Price','Total Purchase Value','Normalized Totals']] 
#Set Index
# gender_purchase = gender_purchase.set_index('Gender')
gender_purchase 

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>



**Age Demographics


```python
# Unique Users
uniqueuser=pd.DataFrame(data.groupby('SN')['Gender'].max())
uniqueuser.reset_index(inplace=True)

# Age broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.) 
maxage = data["Age"].max()
# determine how many bins will be needed
i = ((maxage/4).round())-1

# generate bins 
bins = [age*4+10 for age in range(int(i))] 

#change tuple into list to append 0 to the front of the list and cast back into a tuple
bins = list(bins)
bins.insert(0,0)
bins = tuple(bins)

# must use loop to create dynamic naming for different datasets
group_names = []
for item in range(len(bins)):
    group_names.append(str(bins[item-1])+' - '+ str(bins[item]))
#remove group name that called -1
del group_names[0]

uniqueuser["Age Range"] = pd.cut(data["Age"], bins, labels=group_names)
# cut for later
data["Age Range"] = pd.cut(data["Age"], bins, labels=group_names)
```


```python
age_data = pd.DataFrame(uniqueuser.groupby('Age Range')['Gender'].count())
percent = age_data['Gender']/age_data['Gender'].sum()*100
age_data["Percentage of Players"] = percent.map("{0:,.2f}%".format)
age_data = age_data.rename(columns={'Gender': 'Total Count'})
age_data = age_data[['Percentage of Players','Total Count']]
age_data

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0 - 10</th>
      <td>3.84%</td>
      <td>22</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>4.01%</td>
      <td>23</td>
    </tr>
    <tr>
      <th>14 - 18</th>
      <td>13.79%</td>
      <td>79</td>
    </tr>
    <tr>
      <th>18 - 22</th>
      <td>28.80%</td>
      <td>165</td>
    </tr>
    <tr>
      <th>22 - 26</th>
      <td>27.92%</td>
      <td>160</td>
    </tr>
    <tr>
      <th>26 - 30</th>
      <td>8.38%</td>
      <td>48</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>5.93%</td>
      <td>34</td>
    </tr>
    <tr>
      <th>34 - 38</th>
      <td>4.19%</td>
      <td>24</td>
    </tr>
    <tr>
      <th>38 - 42</th>
      <td>2.97%</td>
      <td>17</td>
    </tr>
    <tr>
      <th>42 - 46</th>
      <td>0.17%</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



** Purchasing Analysis Age


```python
age_total = data.groupby(['Age Range'])
age_sum = age_total['Price'].sum()
age_avg = age_total['Price'].mean()
age_count = age_total['Price'].count()
age_norm = age_sum/age_data['Total Count']

age_purchase = pd.DataFrame({"Total Purchase Value":age_sum,"Average Purchase Price":age_avg,"Purchase Count":age_count,"Normalized Totals":age_norm})
age_purchase = age_purchase[["Purchase Count","Average Purchase Price","Total Purchase Value","Normalized Totals"]]
age_purchase["Average Purchase Price"] = age_purchase["Average Purchase Price"].map("${0:,.2f}".format)
age_purchase["Total Purchase Value"] = age_purchase["Total Purchase Value"].map("${0:,.2f}".format)
age_purchase["Normalized Totals"] = age_purchase["Normalized Totals"].map("${0:,.2f}".format)

age_purchase
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0 - 10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
      <td>$3.64</td>
    </tr>
    <tr>
      <th>14 - 18</th>
      <td>111</td>
      <td>$2.88</td>
      <td>$319.32</td>
      <td>$4.04</td>
    </tr>
    <tr>
      <th>18 - 22</th>
      <td>231</td>
      <td>$2.93</td>
      <td>$676.20</td>
      <td>$4.10</td>
    </tr>
    <tr>
      <th>22 - 26</th>
      <td>207</td>
      <td>$2.94</td>
      <td>$608.02</td>
      <td>$3.80</td>
    </tr>
    <tr>
      <th>26 - 30</th>
      <td>63</td>
      <td>$2.98</td>
      <td>$187.99</td>
      <td>$3.92</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>46</td>
      <td>$3.07</td>
      <td>$141.24</td>
      <td>$4.15</td>
    </tr>
    <tr>
      <th>34 - 38</th>
      <td>37</td>
      <td>$2.81</td>
      <td>$104.06</td>
      <td>$4.34</td>
    </tr>
    <tr>
      <th>38 - 42</th>
      <td>20</td>
      <td>$3.13</td>
      <td>$62.56</td>
      <td>$3.68</td>
    </tr>
    <tr>
      <th>42 - 46</th>
      <td>2</td>
      <td>$3.26</td>
      <td>$6.53</td>
      <td>$6.53</td>
    </tr>
  </tbody>
</table>
</div>



** Top Spenders


```python
# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
spenders = data.groupby(['SN'])

spend_count = spenders['Item Name'].count()
spend_average = spenders['Price'].mean()
spend_total = spenders['Price'].sum()

top_spenders = pd.DataFrame({"Total Purchase Value":spend_total,
                             "Average Purchase Price":spend_average,
                             "Purchase Count":spend_count})
top_spenders = top_spenders[["Purchase Count","Average Purchase Price","Total Purchase Value"]]
top_spenders["Average Purchase Price"] = top_spenders["Average Purchase Price"].map("${0:,.2f}".format)
top_spenders["Total Purchase Value"] = top_spenders["Total Purchase Value"].map("${0:,.2f}".format)
top_spenders = top_spenders.sort_values("Total Purchase Value", ascending = False) 
top_spenders.head(5)

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>3</td>
      <td>$3.13</td>
      <td>$9.38</td>
    </tr>
    <tr>
      <th>Tillyrin30</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Lisistaya47</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Tyisriphos58</th>
      <td>2</td>
      <td>$4.59</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>



** Most Popular Items                


```python
items = data.groupby(['Item ID','Item Name'])
item_count = items['Price'].count()
item_max = items['Price'].max() 
item_sum = items['Price'].sum()

item_pop = pd. DataFrame({"Purchase Count":item_count,"Item Price":item_max,"Total Purchase Value": item_sum})
item_pop["Item Price"] = item_pop["Item Price"].map("${0:,.2f}".format)
item_pop["Total Purchase Value"] = item_pop["Total Purchase Value"].map("${0:,.2f}".format)
item_pop = item_pop [["Purchase Count","Item Price","Total Purchase Value"]]
item_pop = item_pop.sort_values("Purchase Count", ascending = False)
item_pop.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>



** Most Profitable Items


```python
#  I think I am not examining this deeply enough
item_prof = item_pop.sort_values("Total Purchase Value", ascending = False)
item_pop.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>


