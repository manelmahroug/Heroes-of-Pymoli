

```python
import os
import pandas as pd
import numpy as np
```


```python
#store file path in a variable  
json_path = os.path.join ("Resources","purchase_data.json") 
```


```python
#Import the purchase data json file as a DataFrame

purchase_data = pd.read_json(json_path)
purchase_data.head()
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



# Player Count


```python
#Player Count- Using the length of the unique screen names
player_count = len(purchase_data['SN'].unique())

#Convert the player count series into a dataframe
player_count_df = pd.DataFrame([{'Total Players': player_count}])

#resetting the index to total players
Total_players = player_count_df.set_index('Total Players')
Total_players
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
    </tr>
    <tr>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>573</th>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Total)


```python
# Number of Unique Items
unique_items = len(purchase_data["Item ID"].unique())
unique_items 

# Average Purchase Price
ave_price = round (purchase_data["Price"].mean(), 2)
ave_price

# Total Number of Purchases
tot_pur = purchase_data["Price"].count()
tot_pur

# Total Revenue
tot_rev = round (purchase_data["Price"].sum(),2)
tot_rev

total_analysis_df = pd.DataFrame ({ 'Number of Unique Items': [unique_items],
                                    'Average Purchase Price': [ave_price],
                                    'Total Number of Purchases': [tot_pur],
                                    'Total Revenue': [tot_rev]
                                   })

total_analysis_df.style.format({'Average Purchase Price': '${:.2f}', 'Total Revenue': '${:,.2f}'})


```




<style  type="text/css" >
</style>  
<table id="T_49358f38_1e3e_11e8_a3e8_9801a793d7db" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Number of Unique Items</th> 
        <th class="col_heading level0 col2" >Total Number of Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_49358f38_1e3e_11e8_a3e8_9801a793d7dblevel0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_49358f38_1e3e_11e8_a3e8_9801a793d7dbrow0_col0" class="data row0 col0" >$2.93</td> 
        <td id="T_49358f38_1e3e_11e8_a3e8_9801a793d7dbrow0_col1" class="data row0 col1" >183</td> 
        <td id="T_49358f38_1e3e_11e8_a3e8_9801a793d7dbrow0_col2" class="data row0 col2" >780</td> 
        <td id="T_49358f38_1e3e_11e8_a3e8_9801a793d7dbrow0_col3" class="data row0 col3" >$2,286.33</td> 
    </tr></tbody> 
</table> 



# Gender Demographic


```python
#Get the total number of unique players
total_count = len(purchase_data["SN"].unique())

#Number of male players
male_count = purchase_data[purchase_data["Gender"] == "Male"]["SN"].nunique()

#Number of female players
female_count = purchase_data[purchase_data["Gender"] == "Female"]["SN"].nunique()

#Number of non_disclosed players
non_disclosed = total_count - (male_count + female_count)

#Calculating percentages
perc_male = (male_count/total_count)*100
perc_female = (female_count/total_count)*100
perc_non_disclosed = (non_disclosed/total_count)*100

#Creating a data frame

gender_demo_df = pd.DataFrame ({"Gender":["Male","Female","Non-disclosed"],
                                           "Number of players":[male_count, female_count, non_disclosed],
                                           "Percentage of players":[perc_male, perc_female,perc_non_disclosed]})
                                           
gender_demo_df

reset_index = gender_demo_df.set_index("Gender")
reset_index

reset_index.style.format({"Percentage of Players": "{:.2f}%"})
 
```




<style  type="text/css" >
</style>  
<table id="T_658d06f4_1e3e_11e8_ab96_9801a793d7db" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Number of players</th> 
        <th class="col_heading level0 col1" >Percentage of players</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dblevel0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dbrow0_col0" class="data row0 col0" >465</td> 
        <td id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dbrow0_col1" class="data row0 col1" >81.1518</td> 
    </tr>    <tr> 
        <th id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dblevel0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dbrow1_col0" class="data row1 col0" >100</td> 
        <td id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dbrow1_col1" class="data row1 col1" >17.452</td> 
    </tr>    <tr> 
        <th id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dblevel0_row2" class="row_heading level0 row2" >Non-disclosed</th> 
        <td id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dbrow2_col0" class="data row2 col0" >8</td> 
        <td id="T_658d06f4_1e3e_11e8_ab96_9801a793d7dbrow2_col1" class="data row2 col1" >1.39616</td> 
    </tr></tbody> 
</table> 



# Purchase Analysis by Gender


```python
#Calculate the purchase count for males, females and other
male_purchase_count = purchase_data[purchase_data["Gender"] == "Male"]["Price"].count()
female_purchase_count = purchase_data[purchase_data["Gender"] == "Female"]["Price"].count()
other_purchase = tot_pur - male_purchase_count - female_purchase_count

#Calculate the average price per gender
ave_price_male = purchase_data[purchase_data["Gender"] == "Male"]['Price'].mean()
ave_price_female = purchase_data[purchase_data["Gender"] == "Female"]['Price'].mean()
ave_price_other = other_purchase/non_disclosed
#Calculate the total purchase value per gender
tot_price_male = purchase_data[purchase_data ["Gender"] == "Male"]['Price'].sum()
tot_price_female = purchase_data[purchase_data["Gender"] == "Female"]['Price'].sum()
tot_price_other = tot_rev - tot_price_male - tot_price_female 
 
male_norm = tot_price_male/male_count
female_norm = tot_price_female/female_count
other_norm = tot_price_other/non_disclosed

gender_purchase = pd.DataFrame({"Gender": ["Male", "Female", "Non-Disclosed"], 
                                   "Purchase Count": [male_purchase_count, female_purchase_count, other_purchase],
                                    "Average Purchase Price": [ave_price_male,ave_price_female,ave_price_other], 
                                    "Total Purchase Value": [tot_price_male, tot_price_female, tot_price_other],
                                   "Normalized Totals": [male_norm, female_norm, other_norm]}, 
                                  columns = 
               ["Gender", "Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"])
                                        
gender_purchase = gender_purchase.set_index("Gender")
gender_purchase.style.format({"Average Purchase Price": "${:.2f}", 
                              "Total Purchase Value": "${:.2f}", "Normalized Totals": "${:.2f}"})

```




<style  type="text/css" >
</style>  
<table id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7db" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dblevel0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow0_col0" class="data row0 col0" >633</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow0_col1" class="data row0 col1" >$2.95</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow0_col2" class="data row0 col2" >$1867.68</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow0_col3" class="data row0 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dblevel0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow1_col0" class="data row1 col0" >136</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow1_col1" class="data row1 col1" >$2.82</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow1_col2" class="data row1 col2" >$382.91</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow1_col3" class="data row1 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dblevel0_row2" class="row_heading level0 row2" >Non-Disclosed</th> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow2_col0" class="data row2 col0" >11</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow2_col1" class="data row2 col1" >$1.38</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow2_col2" class="data row2 col2" >$35.74</td> 
        <td id="T_7d521b42_1e3e_11e8_8f5b_9801a793d7dbrow2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 



# Age Demographic


```python
a = purchase_data[purchase_data["Age"] <10]
b = purchase_data[(purchase_data["Age"] >=10) & (purchase_data["Age"] <=14)]
c = purchase_data[(purchase_data["Age"] >=15) & (purchase_data["Age"] <=19)]
d = purchase_data[(purchase_data["Age"] >=20) & (purchase_data["Age"] <=24)]
e = purchase_data[(purchase_data["Age"] >=25) & (purchase_data["Age"] <=29)]
f = purchase_data[(purchase_data["Age"] >=30) & (purchase_data["Age"] <=34)]
g = purchase_data[(purchase_data["Age"] >=35) & (purchase_data["Age"] <=39)]
h = purchase_data[(purchase_data["Age"] >=40) & (purchase_data["Age"] <=44)]
i = purchase_data[(purchase_data["Age"] >=45) & (purchase_data["Age"] <=49)]

age_demo_df = pd.DataFrame({"Age": ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45-49"],
                        "Percentage of Players": [(a["SN"].nunique()/total_count)*100, 
                                                  (b["SN"].nunique()/total_count)*100, 
                                                  (c["SN"].nunique()/total_count)*100,
                                                  (d["SN"].nunique()/total_count)*100, 
                                                  (e["SN"].nunique()/total_count)*100, 
                                                  (f["SN"].nunique()/total_count)*100, 
                                                  (g["SN"].nunique()/total_count)*100, 
                                                  (h["SN"].nunique()/total_count)*100, 
                                                  (i["SN"].nunique()/total_count)*100],
                        "Total Count": [a["SN"].nunique(), 
                                        b["SN"].nunique(), 
                                        c["SN"].nunique(), 
                                        d["SN"].nunique(), 
                                        e["SN"].nunique(), 
                                        f["SN"].nunique(), 
                                        g["SN"].nunique(), 
                                        h["SN"].nunique(), 
                                        i["SN"].nunique()]
                       })

age_demo_final = age_demo_df.set_index("Age")
age_demo_final.style.format({"Percentage of Players": "{:.2f}%"})
```




<style  type="text/css" >
</style>  
<table id="T_8dcde886_1e3e_11e8_a380_9801a793d7db" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow0_col0" class="data row0 col0" >3.32%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow0_col1" class="data row0 col1" >19</td> 
    </tr>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow1_col0" class="data row1 col0" >4.01%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow1_col1" class="data row1 col1" >23</td> 
    </tr>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow2_col0" class="data row2 col0" >17.45%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow2_col1" class="data row2 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow3_col0" class="data row3 col0" >45.20%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow3_col1" class="data row3 col1" >259</td> 
    </tr>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow4_col0" class="data row4 col0" >15.18%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow4_col1" class="data row4 col1" >87</td> 
    </tr>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow5_col0" class="data row5 col0" >8.20%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow5_col1" class="data row5 col1" >47</td> 
    </tr>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow6_col0" class="data row6 col0" >4.71%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow6_col1" class="data row6 col1" >27</td> 
    </tr>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row7" class="row_heading level0 row7" >40-44</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow7_col0" class="data row7 col0" >1.75%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow7_col1" class="data row7 col1" >10</td> 
    </tr>    <tr> 
        <th id="T_8dcde886_1e3e_11e8_a380_9801a793d7dblevel0_row8" class="row_heading level0 row8" >45-49</th> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow8_col0" class="data row8 col0" >0.17%</td> 
        <td id="T_8dcde886_1e3e_11e8_a380_9801a793d7dbrow8_col1" class="data row8 col1" >1</td> 
    </tr></tbody> 
</table> 



# Purchasing Analysis by Age


```python
 purchase_age_df = pd.DataFrame({"Age": ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45-49"],
                              "Purchase Count": [a["Price"].count(), b["Price"].count(), c["Price"].count(), d["Price"].count(), e["Price"].count(),f["Price"].count(), g["Price"].count(), h["Price"].count(), i["Price"].count()],
                              "Average Purchase Price": [a["Price"].mean(), b["Price"].mean(), c["Price"].mean(), d["Price"].mean(), e["Price"].mean(), f["Price"].mean(), g["Price"].mean(), h["Price"].mean(), i["Price"].mean()], 
                              "Total Purchase Value": [a["Price"].sum(), b["Price"].sum(), c["Price"].sum(), d["Price"].sum(), e["Price"].sum(), f["Price"].sum(), g["Price"].sum(), h["Price"].sum(), i["Price"].sum()],
                              "Normalized Totals": [a["Price"].sum()/a['SN'].nunique(), b["Price"].sum()/b['SN'].nunique(), c["Price"].sum()/c['SN'].nunique(), 
                                                    d["Price"].sum()/d['SN'].nunique(), e["Price"].sum()/e['SN'].nunique(), 
                                                    f["Price"].sum()/f['SN'].nunique(), g["Price"].sum()/g['SN'].nunique(), 
                                                    h["Price"].sum()/h['SN'].nunique(), i["Price"].sum()/i['SN'].nunique()]}, 
                             columns = 
                            ["Age", "Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"])

purchase_age_final = purchase_age_df.set_index("Age")

purchase_age_final.style.format({"Average Purchase Price": "${:.2f}", "Total Purchase Value": "${:.2f}", "Normalized Totals": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_bfc05c46_1e3e_11e8_840d_9801a793d7db" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow0_col0" class="data row0 col0" >28</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow0_col1" class="data row0 col1" >$2.98</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow0_col2" class="data row0 col2" >$83.46</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow0_col3" class="data row0 col3" >$4.39</td> 
    </tr>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow1_col0" class="data row1 col0" >35</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow1_col1" class="data row1 col1" >$2.77</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow1_col2" class="data row1 col2" >$96.95</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow1_col3" class="data row1 col3" >$4.22</td> 
    </tr>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow2_col0" class="data row2 col0" >133</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow2_col1" class="data row2 col1" >$2.91</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow2_col2" class="data row2 col2" >$386.42</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow2_col3" class="data row2 col3" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow3_col0" class="data row3 col0" >336</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow3_col1" class="data row3 col1" >$2.91</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow3_col2" class="data row3 col2" >$978.77</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow3_col3" class="data row3 col3" >$3.78</td> 
    </tr>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow4_col0" class="data row4 col0" >125</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow4_col1" class="data row4 col1" >$2.96</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow4_col2" class="data row4 col2" >$370.33</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow4_col3" class="data row4 col3" >$4.26</td> 
    </tr>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow5_col0" class="data row5 col0" >64</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow5_col1" class="data row5 col1" >$3.08</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow5_col2" class="data row5 col2" >$197.25</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow5_col3" class="data row5 col3" >$4.20</td> 
    </tr>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow6_col0" class="data row6 col0" >42</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow6_col1" class="data row6 col1" >$2.84</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow6_col2" class="data row6 col2" >$119.40</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow6_col3" class="data row6 col3" >$4.42</td> 
    </tr>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row7" class="row_heading level0 row7" >40-44</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow7_col0" class="data row7 col0" >16</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow7_col1" class="data row7 col1" >$3.19</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow7_col2" class="data row7 col2" >$51.03</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow7_col3" class="data row7 col3" >$5.10</td> 
    </tr>    <tr> 
        <th id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dblevel0_row8" class="row_heading level0 row8" >45-49</th> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow8_col0" class="data row8 col0" >1</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow8_col1" class="data row8 col1" >$2.72</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow8_col2" class="data row8 col2" >$2.72</td> 
        <td id="T_bfc05c46_1e3e_11e8_840d_9801a793d7dbrow8_col3" class="data row8 col3" >$2.72</td> 
    </tr></tbody> 
</table> 



# Top Spenders


```python
sn_price= purchase_data.groupby(["SN"])['Price'].sum()
sn_pur = purchase_data.groupby(["SN"])['Price'].count()
sn_users = purchase_data.groupby(["SN"])
avg_sn = round(sn_price/sn_pur,2)

top_sn = pd.DataFrame({"Purchase Count": sn_pur, "Average Purchase Price":avg_sn, "Total Purchase Value":sn_price})
top_sn= top_sn.sort_values("Total Purchase Value", ascending=False)
top_sn= top_sn[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

top_sn.reset_index(inplace=True)
top_sn.round(2)
top_sn.head(5)
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
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Undirrala66</td>
      <td>5</td>
      <td>3.41</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Saedue76</td>
      <td>4</td>
      <td>3.39</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mindimnya67</td>
      <td>4</td>
      <td>3.18</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Haellysu29</td>
      <td>3</td>
      <td>4.24</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eoda93</td>
      <td>3</td>
      <td>3.86</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>



# Most Popular Items


```python
items_purchase_count = purchase_data.groupby(["Item ID", "Item Name"]).count()["Price"].rename("Purchase Count")
items_average_price= purchase_data.groupby(["Item ID", "Item Name"]).mean()["Price"].rename("Average Purchase Price")
items_value_total = purchase_data.groupby(["Item ID", "Item Name"]).sum()["Price"].rename("Total Purchase Value")

# Convert to DataFrame

items_purchased = pd.DataFrame({"Purchase Count":items_purchase_count,
                                   "Item Price":items_average_price,
                                   "Total Purchase Value":items_value_total})

most_popular_items = items_purchased.sort_values("Purchase Count", ascending=False)
most_popular_items.head(5)
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
      <th>Item Price</th>
      <th>Purchase Count</th>
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
      <td>2.35</td>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>2.23</td>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>2.07</td>
      <td>9</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>1.24</td>
      <td>9</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>1.49</td>
      <td>9</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>



# Most Profitable Items


```python
items_purchase_count = purchase_data.groupby(["Item ID", "Item Name"]).count()["Price"].rename("Purchase Count")
items_average_price = purchase_data.groupby(["Item ID", "Item Name"]).mean()["Price"].rename("Average Purchase Price")
items_value_total = purchase_data.groupby(["Item ID", "Item Name"]).sum()["Price"].rename("Total Purchase Value")

# Convert to DataFrame
items_purchased = pd.DataFrame({"Purchase Count":items_purchase_count,
                                   "Item Price":items_average_price,
                                   "Total Purchase Value":items_value_total})

#items_purchased.head()
most_profitable_items = items_purchased.sort_values("Total Purchase Value", ascending=False)
most_profitable_items.head(5)

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
      <th>Item Price</th>
      <th>Purchase Count</th>
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
      <td>4.14</td>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>4.25</td>
      <td>7</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>4.95</td>
      <td>6</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>4.87</td>
      <td>6</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>3.61</td>
      <td>8</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


