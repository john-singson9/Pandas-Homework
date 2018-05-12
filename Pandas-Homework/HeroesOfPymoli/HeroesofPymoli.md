
# Heroes Of Pymoli Data Analysis
- There is a total of 573 players, with an overwhelming majority of players being male at 81%.  Female players only account for 17%, while the rest did not specify a gender.  The purchases for male players are also significantly larger at $1867.68.

- The most popular age demographic falls between the ages of 20 to 24 accounting for %45 of players.  The data shows that the age range has an effect on player count as well.  Player count increases up to the age range of 20 to 24, and declines after. The age range 20-24 has the highest total purchases of 336, totaling $978.77 out of 2,286.33.

- There is almost little to no correlation between the most popular items and the most profitable item.  Due to the lower prices in the most popular items, they are not able to bring higher profits than the items in the most profitable list with less purchase count.  The only exception is the Retribution Axe, which has a high purchase count of 9, and the most profitable item, bringing in $37.26 in revenue.


```python
#import modules
import pandas as pd
import numpy as np
```


```python
#link the data set
game_path = "purchase_data.json"

#read game path and put it in a data frame
game_file = pd.read_json(game_path)
game_file.head()
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




```python
#count all players and put it in a dataframe
player_count = game_file["SN"].value_counts().count()
player_count_df = pd.DataFrame({"Total Players": player_count}, index = [0])
player_count_df
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




```python
#Purchasing Analysis Calculations:
#number of unique items
unique_items = game_file["Item ID"].value_counts().count()
#average purchase price
average_purchase_price = game_file["Price"].mean()
#total purchases
total_purchases = game_file["Item ID"].count()
#total revenue
total_revenue = game_file["Price"].sum()
```


```python
#Purchasing Analysis DatFrame
purchasing_analysis_df = pd.DataFrame({"Number of Unique Items": unique_items,
                                      "Average Price": average_purchase_price,
                                      "Number of Purchases": total_purchases,
                                      "Total Revenue": total_revenue}, index=[0])
purchasing_analysis_final_df = purchasing_analysis_df[["Number of Unique Items", "Average Price", "Number of Purchases", "Total Revenue"]]
purchasing_analysis_final_df["Total Revenue"] = purchasing_analysis_final_df["Total Revenue"].map("${:.2f}".format)
purchasing_analysis_final_df["Average Price"] = purchasing_analysis_final_df["Average Price"].map("${:.2f}".format)
purchasing_analysis_final_df
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#group by gender
groupby_gender = game_file.groupby(["Gender"])
number_gender = groupby_gender["SN"].nunique()
percentage_gender = number_gender / player_count * 100
```


```python
#Create Gender Demographics DataFrame
gender_demographic_df = pd.DataFrame({"Percentage of Players": percentage_gender, "Total Count": number_gender})
gender_demographic_final_df = gender_demographic_df.sort_values(["Percentage of Players"], ascending = False)
gender_demographic_final_df["Percentage of Players"] = gender_demographic_final_df["Percentage of Players"].map("{:.2f}".format)
gender_demographic_final_df
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
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis Gender
#Purchase Count
gender_purchase_count = groupby_gender["Item ID"].count()
#Average Purchase Price
gender_average_purchase = groupby_gender["Price"].mean()
#Total Purchase Value
gender_total_purchase = groupby_gender["Price"].sum()
#Normalized Totals
gender_normalized_total = gender_total_purchase / number_gender

gender_purchasing_analysisdf = pd.DataFrame({"Purchase Count": gender_purchase_count,
                                     "Average Purchase Price": gender_average_purchase,
                                     "Total Purchase Value": gender_total_purchase,
                                     "Normalized Totals": gender_normalized_total})
gender_purchasing_analysis_finaldf = gender_purchasing_analysisdf[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
gender_purchasing_analysis_finaldf["Average Purchase Price"] = gender_purchasing_analysis_finaldf["Average Purchase Price"].map("${:.2f}".format)
gender_purchasing_analysis_finaldf["Total Purchase Value"] = gender_purchasing_analysis_finaldf["Total Purchase Value"].map("${:.2f}".format)
gender_purchasing_analysis_finaldf["Normalized Totals"] = gender_purchasing_analysis_finaldf["Normalized Totals"].map("${:.2f}".format)
gender_purchasing_analysis_finaldf
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
#Age Demographics**
#The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.) 

#Find max and min
print(game_file['Age'].max())
print(game_file['Age'].min())
```

    45
    7



```python
#Bin age Demographics
age_bins = [6, 9, 14, 19, 24, 29, 34, 39, 44]

#bin names
age_group = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']
age_demographics = pd.cut(game_file['Age'], age_bins, labels=age_group)
game_file['Age Demographic'] = age_demographics
```


```python
#groupby age demographic to figure out calculations
groupby_age_demo = game_file.groupby(['Age Demographic'])
number_by_age = groupby_age_demo["SN"].nunique()
percentage_age = number_by_age / player_count * 100
```


```python
#Age Dataframe
age_demographic_df = pd.DataFrame({"Percentage of Players": percentage_age,
                                  "Total Count": number_by_age})
age_demographic_df["Percentage of Players"] = age_demographic_df["Percentage of Players"].map("{:.2f}".format)
age_demographic_df
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Demographic</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.75</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchase Count
age_purchase_count = groupby_age_demo["Item ID"].count()
#Average Purchase Price
age_average_purchase = groupby_age_demo["Price"].mean()
#Total Purchase Value
age_total_purchase = groupby_age_demo["Price"].sum()
#Normalized Totals
age_normalized_total = age_total_purchase / number_by_age

age_purchasing_analysisdf = pd.DataFrame({"Purchase Count": age_purchase_count,
                                         "Average Purchase Price": age_average_purchase,
                                         "Total Purchase Value": age_total_purchase,
                                         "Normalized Totals": age_normalized_total})
age_purchasing_analysis_finaldf = age_purchasing_analysisdf[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
age_purchasing_analysis_finaldf["Average Purchase Price"] = age_purchasing_analysis_finaldf["Average Purchase Price"].map("${:.2f}".format)
age_purchasing_analysis_finaldf["Total Purchase Value"] = age_purchasing_analysis_finaldf["Total Purchase Value"].map("${:.2f}".format)
age_purchasing_analysis_finaldf["Normalized Totals"] = age_purchasing_analysis_finaldf["Normalized Totals"].map("${:.2f}".format)
age_purchasing_analysis_finaldf
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Demographic</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$4.22</td>
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
      <td>16</td>
      <td>$3.19</td>
      <td>$51.03</td>
      <td>$5.10</td>
    </tr>
  </tbody>
</table>
</div>




```python
#top spenders:  Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
#groupby SN
groupby_SN = game_file.groupby(["SN"])
#Total Purchase Value
topSP_total_sum = groupby_SN["Price"].sum()
#Purchase Count
topSP_purchase_count = groupby_SN["Item Name"].count()
#Average Purchase Price
topSP_average_purchase = topSP_total_sum / topSP_purchase_count

top_spenders_df = pd.DataFrame({"Purchase Count": topSP_purchase_count,
                               "Average Purchase Price":  topSP_average_purchase,
                               "Total Purchase Value": topSP_total_sum})
top_spenders_df = top_spenders_df.sort_values(["Total Purchase Value"], ascending = False).head()
top_spenders_finaldf = top_spenders_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
top_spenders_finaldf["Average Purchase Price"] = top_spenders_finaldf["Average Purchase Price"].map("${:.2f}".format)
top_spenders_finaldf["Total Purchase Value"] = top_spenders_finaldf["Total Purchase Value"].map("${:.2f}".format)
top_spenders_finaldf
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




```python
#Most Popular Items

#groupby Item ID and Item Name
groupby_ID_name = game_file.groupby(["Item ID", "Item Name"])
#Total Count
popular_item = groupby_ID_name["Price"].count()
#Total Purchase Value 
popular_total_value = groupby_ID_name["Price"].sum()
#Item Price
popular_price = popular_total_value / popular_item

popular_item_df = pd.DataFrame({"Purchase Count": popular_item,
                               "Item Price": popular_price,
                               "Total Purchase Value": popular_total_value})
popular_item_df = popular_item_df.sort_values(["Purchase Count"], ascending = False).head(6)
popular_item_finaldf = popular_item_df[["Purchase Count", "Item Price", "Total Purchase Value"]]
popular_item_finaldf["Item Price"] = popular_item_finaldf["Item Price"].map("${:.2f}".format)
popular_item_finaldf["Total Purchase Value"] = popular_item_finaldf["Total Purchase Value"].map("${:.2f}".format)
popular_item_finaldf
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
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most profitable Items
profitable_item_df = pd.DataFrame({"Purchase Count": popular_item,
                               "Item Price": popular_price,
                               "Total Purchase Value": popular_total_value})
profitable_item_df = profitable_item_df.sort_values(["Total Purchase Value"], ascending = False).head()
profitable_item_finaldf = profitable_item_df[["Purchase Count", "Item Price", "Total Purchase Value"]]
profitable_item_finaldf["Item Price"] = profitable_item_finaldf["Item Price"].map("${:.2f}".format)
profitable_item_finaldf["Total Purchase Value"] = profitable_item_finaldf["Total Purchase Value"].map("${:.2f}".format)
profitable_item_finaldf
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
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


