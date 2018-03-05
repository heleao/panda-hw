

```python
import pandas as pd
```


```python
# read the purchase_data.json
jsonPath = "purchase_data.json"
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
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
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
      <td>573</td>
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
#reformat
purchasingAnalysisSummary_df["Total Purchases"] = purchasingAnalysisSummary_df["Total Purchases"].map("${:.2f}".format)
purchasingAnalysisSummary_df["Avg Purchase Price"] = purchasingAnalysisSummary_df["Avg Purchase Price"].map("${:.2f}".format)
purchasingAnalysisSummary_df["Total Revenue"] = purchasingAnalysisSummary_df["Total Revenue"].map("${:,.2f}".format)

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
      <td>183</td>
      <td>$2.93</td>
      <td>$780.00</td>
      <td>$2,286.33</td>
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
#reformat
genderCountSummary_df["Percentage of Players"] = genderCountSummary_df["Percentage of Players"].map("{0:.2f}%".format)

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
#reformat
genderPurchaseSummary_df["Total Purchase Value"] = genderPurchaseSummary_df["Total Purchase Value"].map("${:.2f}".format)
genderPurchaseSummary_df["Avg Purchase Price"] = genderPurchaseSummary_df["Avg Purchase Price"].map("${:.2f}".format)
genderPurchaseSummary_df["Normalized Totals"] = genderPurchaseSummary_df["Normalized Totals"].map("${:.2f}".format)
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
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1867.68</td>
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
#reformat
ageDemographicSummary_df["Total Purchase Value"] = ageDemographicSummary_df["Total Purchase Value"].map("${:.2f}".format)
ageDemographicSummary_df["Avg Purchase Price"] = ageDemographicSummary_df["Avg Purchase Price"].map("${:.2f}".format)
ageDemographicSummary_df["Normalized Totals"] = ageDemographicSummary_df["Normalized Totals"].map("${:.2f}".format)
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
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
      <td>$4.19</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$3.78</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$4.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$4.20</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$4.42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$4.89</td>
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
#reformat
snTop5Summary_df["Total Purchase Value"] = snTop5Summary_df["Total Purchase Value"].map("${:.2f}".format)
snTop5Summary_df["Average Purchase Value"] = snTop5Summary_df["Average Purchase Value"].map("${:.2f}".format)                   
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
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
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
#reformat
itemTop5CountSummary_df["Total Purchase Value"] = itemTop5CountSummary_df["Total Purchase Value"].map("${:.2f}".format)
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
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <th>2.35</th>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <th>2.23</th>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <th>1.49</th>
      <td>9</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <th>2.07</th>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <th>4.14</th>
      <td>9</td>
      <td>$37.26</td>
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
#reformat
itemTop5ValueSummary_df["Total Purchase Value"] = itemTop5ValueSummary_df["Total Purchase Value"].map("${:.2f}".format)
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
      <th>34</th>
      <th>Retribution Axe</th>
      <th>4.14</th>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <th>4.25</th>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <th>4.95</th>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <th>4.87</th>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <th>3.61</th>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>



Observations of three trends:
1. Males buy more products, more expensive items, and more frequently than Females.
2. 20-24 age group buy the most products but 40+ buy more expensive products and more frequently out of any age group.
3. The frequency of purchases per user is low since the top user in purchase value only bought 5 times.
