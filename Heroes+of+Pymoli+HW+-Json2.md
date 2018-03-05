

```python
import pandas as pd
```


```python
# read the purchase_data.json
jsonPath = "purchase_data2.json"
playerJson = pd.read_json(jsonPath)
playerJson.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
      <td>Iloni35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
      <td>Aidaira26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
      <td>Irim47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
      <td>Irith83</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
      <td>Philodil43</td>
    </tr>
  </tbody>
</table>
</div>



Total Number of Players


```python
#Total Number of Players
dedupePlayer = pd.DataFrame(playerJson.groupby('SN').count())
totalNumPlayers =len(dedupePlayer)
totalNumPlayers_df = pd.DataFrame({"Total Players":totalNumPlayers},index = {0})
totalNumPlayers_df
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
      <td>74</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Total)


```python
# Number of Unique Items
# Average Purchase Price
# Total Number of Purchases
# Total Revenue

#Number of Unique Items
dedupeItem = pd.DataFrame(playerJson.groupby('Item ID').count())
totalUniqueItems = len(dedupeItem)

#Average Purchase Price
purchasePriceSeries = playerJson.loc[:,'Price']
avgPurchasePrice = round(purchasePriceSeries.mean(),2)

#Total Number of Purchases
numberPurchases = len(playerJson)

#Total Revenue
totalRevenue = round(playerJson.loc[:,'Price'].sum(),2)

#Purchasing Analysis Summary
purchasingAnalysisSummary_df = pd.DataFrame({"Num of Unique Items":totalUniqueItems,\
                                        "Avg Purchase Price":avgPurchasePrice,\
                                        "Total Purchases":numberPurchases,\
                                        "Total Revenue":totalRevenue},index ={0})
purchasingAnalysisSummary_df = purchasingAnalysisSummary_df[["Num of Unique Items",\
                                        "Avg Purchase Price",\
                                        "Total Purchases",\
                                        "Total Revenue"]]
purchasingAnalysisSummary_df
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
      <th>Num of Unique Items</th>
      <th>Avg Purchase Price</th>
      <th>Total Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>64</td>
      <td>2.92</td>
      <td>78</td>
      <td>228.1</td>
    </tr>
  </tbody>
</table>
</div>



Gender Demographics


```python
#Percentage and Count of Gender

#Dedupe the players
dedupeGender = playerJson.groupby(['SN','Gender']).count()

#Count by Gender
dedupeGender_df = pd.DataFrame(dedupeGender.groupby('Gender').count())
dedupeGenderCount = dedupeGender_df['Age'].rename("Count")

#Percentage by Gender
dedupeGenderPercentage = round(dedupeGenderCount/totalNumPlayers * 100,2).rename("Percentage")

#Percentage and Count of Gender Table
genderCountSummary_df = pd.DataFrame({"Percentage of Players":dedupeGenderPercentage,\
                                        "Total Count":dedupeGenderCount})
genderCountSummary_df = genderCountSummary_df[["Percentage of Players",\
                                        "Total Count"]]
genderCountSummary_df

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
      <td>17.57</td>
      <td>13</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.08</td>
      <td>60</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.35</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Gender)


```python
#Create a purchase table with Gender as groupby
genderGroupby = playerJson.groupby(['Gender'])

#Purchase Count
genderPurchaseCount = genderGroupby["Item ID"].count()

#Average Purchase Price
genderAvgPurchase = round(genderGroupby["Price"].mean(),2)

#Total Purchase Value
genderTotalPurchase = round(genderGroupby["Price"].sum(),2)

#Normalized Totals
genderNorPurchase = round(genderTotalPurchase/dedupeGenderCount,2)

#Gender Purchasing Summary
genderPurchaseSummary_df = pd.DataFrame({"Purchase Count":genderPurchaseCount,\
                                        "Avg Purchase Price":genderAvgPurchase,\
                                        "Total Purchase Value":genderTotalPurchase,\
                                        "Normalized Totals":genderNorPurchase})
genderPurchaseSummary_df = genderPurchaseSummary_df[["Purchase Count",\
                                                     "Avg Purchase Price",\
                                                     "Total Purchase Value",\
                                                     "Normalized Totals"]]
genderPurchaseSummary_df

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
      <th>Avg Purchase Price</th>
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
      <td>13</td>
      <td>3.18</td>
      <td>41.38</td>
      <td>3.18</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>64</td>
      <td>2.88</td>
      <td>184.60</td>
      <td>3.08</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1</td>
      <td>2.12</td>
      <td>2.12</td>
      <td>2.12</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
#The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.) 

#Add an extra column to raw table with the bins for age
bins = [0, 10, 14, 19, 24, 29, 34, 39,playerJson["Age"].max()]
group_names = ["<10", "10-14", "15-19", "20-24","25-29","30-34","35-39","40+"]
playerJson["Age Bin"] = pd.cut( playerJson["Age"], bins, labels=group_names)
ageBinGroupby = playerJson.groupby(['Age Bin'])

#Purchase Count
agePurchaseCount = ageBinGroupby["Item ID"].count()

#Average Purchase Price
ageAvgPurchase = round(ageBinGroupby["Price"].mean(),2)

#Total Purchase Value
ageTotalPurchase = round(ageBinGroupby["Price"].sum(),2)

#Normalized Totals
#dedupe age using the player dedupe table
groupPlayerAge = playerJson.groupby(["SN","Age"]).count()
groupPlayerAge = groupPlayerAge.reset_index(level='Age', col_level=1)
groupPlayerAge["Age Bin"] =  pd.cut( groupPlayerAge["Age"], bins, labels=group_names)
groupAgeBin = groupPlayerAge.groupby(['Age Bin']).count()

ageNorPurchase = round(ageTotalPurchase/groupAgeBin['Age'],2)

#Age Demographic Summary
ageDemographicSummary_df = pd.DataFrame({"Purchase Count":agePurchaseCount,\
                                        "Avg Purchase Price":ageAvgPurchase,\
                                        "Total Purchase Value":ageTotalPurchase,\
                                        "Normalized Totals":ageNorPurchase})
ageDemographicSummary_df = ageDemographicSummary_df[["Purchase Count",\
                                                     "Avg Purchase Price",\
                                                     "Total Purchase Value",\
                                                     "Normalized Totals"]]
ageDemographicSummary_df

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
      <th>Avg Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Bin</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>2.76</td>
      <td>13.82</td>
      <td>2.76</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>2.99</td>
      <td>8.96</td>
      <td>2.99</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>2.76</td>
      <td>30.41</td>
      <td>2.76</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>36</td>
      <td>3.02</td>
      <td>108.89</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9</td>
      <td>2.90</td>
      <td>26.11</td>
      <td>3.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7</td>
      <td>1.98</td>
      <td>13.89</td>
      <td>2.32</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>3.56</td>
      <td>21.37</td>
      <td>3.56</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>4.65</td>
      <td>4.65</td>
      <td>4.65</td>
    </tr>
  </tbody>
</table>
</div>



Top Spenders


```python
#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
# SN, Purchase Count, Average Purchase Value, Total Purchase Value

#Create table that groupby SN
snGroupby = playerJson.groupby(['SN'])
snPurchaseCount = round(snGroupby["Item ID"].count(),2)
snAvgPurchase = round(snGroupby["Price"].mean(),2)
snTotalPurchase = round(snGroupby["Price"].sum(),2)
snSummary_df = pd.DataFrame({"Purchase Count":snPurchaseCount,\
                             "Average Purchase Value":snAvgPurchase,\
                             "Total Purchase Value":snTotalPurchase})

#Sort by total purchase value then take the top 5
snTop5Summary_df = snSummary_df.nlargest(5,'Total Purchase Value', keep='first')
snTop5Summary_df = snTop5Summary_df[["Purchase Count",\
                                    "Average Purchase Value",\
                                    "Total Purchase Value"]]
snTop5Summary_df
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
      <th>Average Purchase Value</th>
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
      <th>Sundaky74</th>
      <td>2</td>
      <td>3.70</td>
      <td>7.41</td>
    </tr>
    <tr>
      <th>Aidaira26</th>
      <td>2</td>
      <td>2.56</td>
      <td>5.13</td>
    </tr>
    <tr>
      <th>Eusty71</th>
      <td>1</td>
      <td>4.81</td>
      <td>4.81</td>
    </tr>
    <tr>
      <th>Chanirra64</th>
      <td>1</td>
      <td>4.78</td>
      <td>4.78</td>
    </tr>
    <tr>
      <th>Alarap40</th>
      <td>1</td>
      <td>4.71</td>
      <td>4.71</td>
    </tr>
  </tbody>
</table>
</div>



Most Popular Items


```python
#Identify the 5 most popular items by purchase count, then list (in a table):
# Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value

#Create table that groupby Item ID
itemGroupby = playerJson.groupby(['Item ID','Item Name', 'Price'])
itemPurchaseCount = round(itemGroupby["Price"].count(),2)
itemAvgPurchase = round(itemGroupby["Price"].mean(),2)
itemTotalPurchase = round(itemGroupby["Price"].sum(),2)
itemSummary_df = pd.DataFrame({"Purchase Count":itemPurchaseCount,\
                               "Total Purchase Value":itemTotalPurchase})

#Sort by total purchase count then take the top 5
itemTop5CountSummary_df = itemSummary_df.nlargest(5,'Purchase Count', keep='first')
itemTop5CountSummary_df = itemTop5CountSummary_df[["Purchase Count",\
                                    "Total Purchase Value"]]
itemTop5CountSummary_df
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
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>94</th>
      <th>Mourning Blade</th>
      <th>3.64</th>
      <td>3</td>
      <td>10.92</td>
    </tr>
    <tr>
      <th>60</th>
      <th>Wolf</th>
      <th>2.70</th>
      <td>2</td>
      <td>5.40</td>
    </tr>
    <tr>
      <th>64</th>
      <th>Fusion Pummel</th>
      <th>2.42</th>
      <td>2</td>
      <td>4.84</td>
    </tr>
    <tr>
      <th>90</th>
      <th>Betrayer</th>
      <th>4.12</th>
      <td>2</td>
      <td>8.24</td>
    </tr>
    <tr>
      <th>93</th>
      <th>Apocalyptic Battlescythe</th>
      <th>4.49</th>
      <td>2</td>
      <td>8.98</td>
    </tr>
  </tbody>
</table>
</div>



Most Profitable Items


```python
#Identify the 5 most profitable items by total purchase value, then list (in a table):
# Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value
itemTop5ValueSummary_df = itemSummary_df.nlargest(5,'Total Purchase Value', keep='first')
itemTop5ValueSummary_df = itemTop5ValueSummary_df[["Purchase Count",\
                                    "Total Purchase Value"]]
itemTop5ValueSummary_df
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
      <th></th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>94</th>
      <th>Mourning Blade</th>
      <th>3.64</th>
      <td>3</td>
      <td>10.92</td>
    </tr>
    <tr>
      <th>117</th>
      <th>Heartstriker, Legacy of the Light</th>
      <th>4.71</th>
      <td>2</td>
      <td>9.42</td>
    </tr>
    <tr>
      <th>93</th>
      <th>Apocalyptic Battlescythe</th>
      <th>4.49</th>
      <td>2</td>
      <td>8.98</td>
    </tr>
    <tr>
      <th>90</th>
      <th>Betrayer</th>
      <th>4.12</th>
      <td>2</td>
      <td>8.24</td>
    </tr>
    <tr>
      <th>154</th>
      <th>Feral Katana</th>
      <th>4.11</th>
      <td>2</td>
      <td>8.22</td>
    </tr>
  </tbody>
</table>
</div>



Observations of three trends:
1. Males buy more products.
2. 20-24 age group buy the most products.
3. The frequency of purchases per user is low since the top user in purchase value only bought 2 times.
