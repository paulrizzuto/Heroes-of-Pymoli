
# Heroes of Pymoli Data Analysis


- While the total purchase count is 780, the number of unique players is only 573. There is a good amount of repeat purchasers for the Heroes of Pymoli game.
- The best represented age bracket in the data is 20-24 years, accounting for 45% of the entire sample. Unsurprisingly, the bracket makes up the largest portion of Total Purchase Value, but while Average Purchase Price is middle of the pack, Normalized Totals places the bracket in the lower tier of values. Players in this age bracket spend less than other age brackets on a holistic basis.
- The list of most profitable items is led by some of the most expensive items, rather than those that were purchased the most often. Items priced lower must be purchased at a much higher rate to deliver the same value as higher priced items, and the data does not show this occurring.


```python
import pandas as pd
import numpy as np
```


```python
file = "purchase_data.json"
```


```python
df = pd.read_json(file)
df.head()
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




```python
df.columns
```




    Index(['Age', 'Gender', 'Item ID', 'Item Name', 'Price', 'SN'], dtype='object')



## Player Count


```python
#total number of players
uniqueplayers = df['SN'].nunique()
total_players_final = pd.DataFrame({"Total Players": [uniqueplayers]}, columns= ["Total Players"])
total_players_final
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



## Purchase Analysis (Total)


```python
#find number of unique items sold, purchase price, number of purchases, and total revenue for full dataframe
uniqueitems = df['Item ID'].nunique()
avgprice = (df['Price'].sum()/df['Price'].count()).round(2)
totalpurchases = df['Price'].count()
totalrevenue = df["Price"].sum()

total_analysis_df = pd.DataFrame({"Number of Unique Items": [uniqueitems], 
                              "Average Purchase Price": [avgprice],
                             "Number of Purchases": [totalpurchases],
                             "Total Revenue": [totalrevenue]}, columns= ["Number of Unique Items", "Average Purchase Price",
                            "Number of Purchases", "Total Revenue"])

total_analysis_df.style.format({"Average Purchase Price": "${:.2f}", "Total Revenue": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_08b96d46_8f53_11e7_87aa_f40f240c664b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Number of Unique Items</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Number of Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_08b96d46_8f53_11e7_87aa_f40f240c664b" class="row_heading level0 row0" >0</th> 
        <td id="T_08b96d46_8f53_11e7_87aa_f40f240c664brow0_col0" class="data row0 col0" >183</td> 
        <td id="T_08b96d46_8f53_11e7_87aa_f40f240c664brow0_col1" class="data row0 col1" >$2.93</td> 
        <td id="T_08b96d46_8f53_11e7_87aa_f40f240c664brow0_col2" class="data row0 col2" >780</td> 
        <td id="T_08b96d46_8f53_11e7_87aa_f40f240c664brow0_col3" class="data row0 col3" >$2286.33</td> 
    </tr></tbody> 
</table> 



## Gender Demographics


```python
fullcount = df["SN"].nunique()
malecount = df[df["Gender"] == "Male"]["SN"].nunique()
femalecount = df[df["Gender"] == "Female"]["SN"].nunique()
othercount = fullcount - malecount - femalecount
maleperc = ((malecount/fullcount)*100)
femaleperc = ((femalecount/fullcount)*100)
otherperc = ((othercount/fullcount)*100)

gender_demo_df = pd.DataFrame({"Gender": ["Male", "Female", "Other / Non-Disclosed"], "Percentage of Players": [maleperc, femaleperc, otherperc],
                                        "Total Count": [malecount, femalecount, othercount]}, columns = 
                                        ["Gender", "Percentage of Players", "Total Count"])
                                        
gender_demo_final = gender_demo_df.set_index("Gender")
gender_demo_final.style.format({"Percentage of Players": "{:.2f}%"})                                      
```




<style  type="text/css" >
</style>  
<table id="T_fdd16cee_8f52_11e7_8f07_f40f240c664b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_fdd16cee_8f52_11e7_8f07_f40f240c664b" class="row_heading level0 row0" >Male</th> 
        <td id="T_fdd16cee_8f52_11e7_8f07_f40f240c664brow0_col0" class="data row0 col0" >81.15%</td> 
        <td id="T_fdd16cee_8f52_11e7_8f07_f40f240c664brow0_col1" class="data row0 col1" >465</td> 
    </tr>    <tr> 
        <th id="T_fdd16cee_8f52_11e7_8f07_f40f240c664b" class="row_heading level0 row1" >Female</th> 
        <td id="T_fdd16cee_8f52_11e7_8f07_f40f240c664brow1_col0" class="data row1 col0" >17.45%</td> 
        <td id="T_fdd16cee_8f52_11e7_8f07_f40f240c664brow1_col1" class="data row1 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_fdd16cee_8f52_11e7_8f07_f40f240c664b" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_fdd16cee_8f52_11e7_8f07_f40f240c664brow2_col0" class="data row2 col0" >1.40%</td> 
        <td id="T_fdd16cee_8f52_11e7_8f07_f40f240c664brow2_col1" class="data row2 col1" >8</td> 
    </tr></tbody> 
</table> 



## Purchasing Analysis (Gender)


```python
malepurch = df[df["Gender"] == "Male"]["Price"].count()
femalepurch = df[df["Gender"] == "Female"]["Price"].count()
otherpurch = totalpurchases - malepurch - femalepurch
mpriceavg = df[df["Gender"] == "Male"]['Price'].mean()
fpriceavg = df[df["Gender"] == "Female"]['Price'].mean()
opriceavg = df[df["Gender"] == "Other / Non-Disclosed"]['Price'].mean()
mpricetot = df[df["Gender"] == "Male"]['Price'].sum()
fpricetot = df[df["Gender"] == "Female"]['Price'].sum()
opricetot = df[df["Gender"] == "Other / Non-Disclosed"]['Price'].sum()
mnorm = mpricetot/malecount
fnorm = fpricetot/femalecount
onorm = opricetot/othercount

gender_purchase_df = pd.DataFrame({"Gender": ["Male", "Female", "Other / Non-Disclosed"], "Purchase Count": [malepurch, femalepurch, otherpurch],
                                        "Average Purchase Price": [mpriceavg, fpriceavg, opriceavg], "Total Purchase Value": [mpricetot, fpricetot, opricetot],
                                "Normalized Totals": [mnorm, fnorm, onorm]}, columns = 
                                        ["Gender", "Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"])
                                        
gender_purchase_final = gender_purchase_df.set_index("Gender")
gender_purchase_final.style.format({"Average Purchase Price": "${:.2f}", "Total Purchase Value": "${:.2f}", "Normalized Totals": "${:.2f}"})


```




<style  type="text/css" >
</style>  
<table id="T_f730b58c_8f52_11e7_9f4e_f40f240c664b" > 
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
        <th id="T_f730b58c_8f52_11e7_9f4e_f40f240c664b" class="row_heading level0 row0" >Male</th> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow0_col0" class="data row0 col0" >633</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow0_col1" class="data row0 col1" >$2.95</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow0_col2" class="data row0 col2" >$1867.68</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow0_col3" class="data row0 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_f730b58c_8f52_11e7_9f4e_f40f240c664b" class="row_heading level0 row1" >Female</th> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow1_col0" class="data row1 col0" >136</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow1_col1" class="data row1 col1" >$2.82</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow1_col2" class="data row1 col2" >$382.91</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow1_col3" class="data row1 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_f730b58c_8f52_11e7_9f4e_f40f240c664b" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow2_col0" class="data row2 col0" >11</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow2_col1" class="data row2 col1" >$3.25</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow2_col2" class="data row2 col2" >$35.74</td> 
        <td id="T_f730b58c_8f52_11e7_9f4e_f40f240c664brow2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 



## Age Demographics


```python
#create age parameters - 4 year length
#create dataframe of unique players in each age group, find percentage against full count of players

tenyears = df[df["Age"] <10]
loteens = df[(df["Age"] >=10) & (df["Age"] <=14)]
hiteens = df[(df["Age"] >=15) & (df["Age"] <=19)]
lotwent = df[(df["Age"] >=20) & (df["Age"] <=24)]
hitwent = df[(df["Age"] >=25) & (df["Age"] <=29)]
lothirt = df[(df["Age"] >=30) & (df["Age"] <=34)]
hithirt = df[(df["Age"] >=35) & (df["Age"] <=39)]
loforty = df[(df["Age"] >=40) & (df["Age"] <=44)]
hiforty = df[(df["Age"] >=45) & (df["Age"] <=49)]

age_demo_df = pd.DataFrame({"Age": ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45-49"],
                        "Percentage of Players": [(tenyears["SN"].nunique()/fullcount)*100, (loteens["SN"].nunique()/fullcount)*100, (hiteens["SN"].nunique()/fullcount)*100, (lotwent["SN"].nunique()/fullcount)*100, (hitwent["SN"].nunique()/fullcount)*100, (lothirt["SN"].nunique()/fullcount)*100, (hithirt["SN"].nunique()/fullcount)*100, (loforty["SN"].nunique()/fullcount)*100, (hiforty["SN"].nunique()/fullcount)*100],
                        "Total Count": [tenyears["SN"].nunique(), loteens["SN"].nunique(), hiteens["SN"].nunique(), lotwent["SN"].nunique(), hitwent["SN"].nunique(), lothirt["SN"].nunique(), hithirt["SN"].nunique(), loforty["SN"].nunique(), hiforty["SN"].nunique()]
                       })

age_demo_final = age_demo_df.set_index("Age")
age_demo_final.style.format({"Percentage of Players": "{:.2f}%"})  
                        
```




<style  type="text/css" >
</style>  
<table id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" > 
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
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row0" ><10</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow0_col0" class="data row0 col0" >3.32%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow0_col1" class="data row0 col1" >19</td> 
    </tr>    <tr> 
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row1" >10-14</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow1_col0" class="data row1 col0" >4.01%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow1_col1" class="data row1 col1" >23</td> 
    </tr>    <tr> 
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row2" >15-19</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow2_col0" class="data row2 col0" >17.45%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow2_col1" class="data row2 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row3" >20-24</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow3_col0" class="data row3 col0" >45.20%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow3_col1" class="data row3 col1" >259</td> 
    </tr>    <tr> 
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row4" >25-29</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow4_col0" class="data row4 col0" >15.18%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow4_col1" class="data row4 col1" >87</td> 
    </tr>    <tr> 
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row5" >30-34</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow5_col0" class="data row5 col0" >8.20%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow5_col1" class="data row5 col1" >47</td> 
    </tr>    <tr> 
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row6" >35-39</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow6_col0" class="data row6 col0" >4.71%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow6_col1" class="data row6 col1" >27</td> 
    </tr>    <tr> 
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row7" >40-44</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow7_col0" class="data row7 col0" >1.75%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow7_col1" class="data row7 col1" >10</td> 
    </tr>    <tr> 
        <th id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664b" class="row_heading level0 row8" >45-49</th> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow8_col0" class="data row8 col0" >0.17%</td> 
        <td id="T_fed8d762_8f4d_11e7_b3ec_f40f240c664brow8_col1" class="data row8 col1" >1</td> 
    </tr></tbody> 
</table> 



## Purchasing Analysis (Age)


```python

```


```python
age_purchasing_df = pd.DataFrame({"Age": ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45-49"],
                              "Purchase Count": [tenyears["Price"].count(), loteens["Price"].count(), hiteens["Price"].count(), lotwent["Price"].count(), hitwent["Price"].count(), lothirt["Price"].count(), hithirt["Price"].count(), loforty["Price"].count(), hiforty["Price"].count()],
                              "Average Purchase Price": [tenyears["Price"].mean(), loteens["Price"].mean(), hiteens["Price"].mean(), lotwent["Price"].mean(), hitwent["Price"].mean(), lothirt["Price"].mean(), hithirt["Price"].mean(), loforty["Price"].mean(), hiforty["Price"].mean()], 
                              "Total Purchase Value": [tenyears["Price"].sum(), loteens["Price"].sum(), hiteens["Price"].sum(), lotwent["Price"].sum(), hitwent["Price"].sum(), lothirt["Price"].sum(), hithirt["Price"].sum(), loforty["Price"].sum(), hiforty["Price"].sum()],
                              "Normalized Totals": [tenyears["Price"].sum()/tenyears['SN'].nunique(), loteens["Price"].sum()/loteens['SN'].nunique(), hiteens["Price"].sum()/hiteens['SN'].nunique(), 
                                                    lotwent["Price"].sum()/lotwent['SN'].nunique(), hitwent["Price"].sum()/hitwent['SN'].nunique(), 
                                                    lothirt["Price"].sum()/lothirt['SN'].nunique(), hithirt["Price"].sum()/hithirt['SN'].nunique(), 
                                                    loforty["Price"].sum()/loforty['SN'].nunique(), hiforty["Price"].sum()/hiforty['SN'].nunique()]}, 
                             columns = 
                            ["Age", "Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"])

age_purchasing_final = age_purchasing_df.set_index("Age")

age_purchasing_final.style.format({"Average Purchase Price": "${:.2f}", "Total Purchase Value": "${:.2f}", "Normalized Totals": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" > 
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
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row0" ><10</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow0_col0" class="data row0 col0" >28</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow0_col1" class="data row0 col1" >$2.98</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow0_col2" class="data row0 col2" >$83.46</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow0_col3" class="data row0 col3" >$4.39</td> 
    </tr>    <tr> 
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row1" >10-14</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow1_col0" class="data row1 col0" >35</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow1_col1" class="data row1 col1" >$2.77</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow1_col2" class="data row1 col2" >$96.95</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow1_col3" class="data row1 col3" >$4.22</td> 
    </tr>    <tr> 
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row2" >15-19</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow2_col0" class="data row2 col0" >133</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow2_col1" class="data row2 col1" >$2.91</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow2_col2" class="data row2 col2" >$386.42</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow2_col3" class="data row2 col3" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row3" >20-24</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow3_col0" class="data row3 col0" >336</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow3_col1" class="data row3 col1" >$2.91</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow3_col2" class="data row3 col2" >$978.77</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow3_col3" class="data row3 col3" >$3.78</td> 
    </tr>    <tr> 
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row4" >25-29</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow4_col0" class="data row4 col0" >125</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow4_col1" class="data row4 col1" >$2.96</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow4_col2" class="data row4 col2" >$370.33</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow4_col3" class="data row4 col3" >$4.26</td> 
    </tr>    <tr> 
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row5" >30-34</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow5_col0" class="data row5 col0" >64</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow5_col1" class="data row5 col1" >$3.08</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow5_col2" class="data row5 col2" >$197.25</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow5_col3" class="data row5 col3" >$4.20</td> 
    </tr>    <tr> 
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row6" >35-39</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow6_col0" class="data row6 col0" >42</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow6_col1" class="data row6 col1" >$2.84</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow6_col2" class="data row6 col2" >$119.40</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow6_col3" class="data row6 col3" >$4.42</td> 
    </tr>    <tr> 
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row7" >40-44</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow7_col0" class="data row7 col0" >16</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow7_col1" class="data row7 col1" >$3.19</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow7_col2" class="data row7 col2" >$51.03</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow7_col3" class="data row7 col3" >$5.10</td> 
    </tr>    <tr> 
        <th id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664b" class="row_heading level0 row8" >45-49</th> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow8_col0" class="data row8 col0" >1</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow8_col1" class="data row8 col1" >$2.72</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow8_col2" class="data row8 col2" >$2.72</td> 
        <td id="T_d94f0b9c_8f52_11e7_b9bd_f40f240c664brow8_col3" class="data row8 col3" >$2.72</td> 
    </tr></tbody> 
</table> 



## Top Spenders


```python
sn_total_purchase = df.groupby('SN')['Price'].sum().to_frame()
sn_purchase_count = df.groupby('SN')['Price'].count().to_frame()
sn_purchase_avg = df.groupby('SN')['Price'].mean().to_frame()

sn_total_purchase.columns=["Total Purchase Value"]
join_one = sn_total_purchase.join(sn_purchase_count, how="left")
join_one.columns=["Total Purchase Value", "Purchase Count"]

join_two = join_one.join(sn_purchase_avg, how="inner")
join_two.columns=["Total Purchase Value", "Purchase Count", "Average Purchase Price"]

top_spenders_df = join_two[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
top_spenders_final = top_spenders_df.sort_values('Total Purchase Value', ascending=False).head()
top_spenders_final.style.format({"Average Purchase Price": "${:.2f}", "Total Purchase Value": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_77a0d628_8f52_11e7_b361_f40f240c664b" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_77a0d628_8f52_11e7_b361_f40f240c664b" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow0_col0" class="data row0 col0" >5</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow0_col1" class="data row0 col1" >$3.41</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow0_col2" class="data row0 col2" >$17.06</td> 
    </tr>    <tr> 
        <th id="T_77a0d628_8f52_11e7_b361_f40f240c664b" class="row_heading level0 row1" >Saedue76</th> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow1_col0" class="data row1 col0" >4</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow1_col1" class="data row1 col1" >$3.39</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow1_col2" class="data row1 col2" >$13.56</td> 
    </tr>    <tr> 
        <th id="T_77a0d628_8f52_11e7_b361_f40f240c664b" class="row_heading level0 row2" >Mindimnya67</th> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow2_col0" class="data row2 col0" >4</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow2_col1" class="data row2 col1" >$3.18</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow2_col2" class="data row2 col2" >$12.74</td> 
    </tr>    <tr> 
        <th id="T_77a0d628_8f52_11e7_b361_f40f240c664b" class="row_heading level0 row3" >Haellysu29</th> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow3_col0" class="data row3 col0" >3</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow3_col1" class="data row3 col1" >$4.24</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow3_col2" class="data row3 col2" >$12.73</td> 
    </tr>    <tr> 
        <th id="T_77a0d628_8f52_11e7_b361_f40f240c664b" class="row_heading level0 row4" >Eoda93</th> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow4_col0" class="data row4 col0" >3</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow4_col1" class="data row4 col1" >$3.86</td> 
        <td id="T_77a0d628_8f52_11e7_b361_f40f240c664brow4_col2" class="data row4 col2" >$11.58</td> 
    </tr></tbody> 
</table> 



## Most Popular Items


```python
#merge dataframes to find purchase count, total purchase value for items
#reset indices to dataframes can be merged on specific elements
premergeone = df.groupby("Item Name").sum().reset_index()
premergetwo = df.groupby("Item ID").sum().reset_index()
premergethree = df.groupby("Item Name").count().reset_index()

#merge dataframes
mergeone = pd.merge(premergeone, premergetwo, on="Price")
mergetwo = pd.merge(premergethree, mergeone, on="Item Name")

#start to create final dataframe by manipulating data
mergetwo["Gender"] = (mergetwo["Price_y"]/mergetwo["Item ID"]).round(2)

mergetwo_renamed = mergetwo.rename(columns={"Age": "Purchase Count", "Gender": "Item Price", "Item ID": "null", "Price_y": "Total Purchase Value", "Item ID_y": "Item ID"})

#grab columns we are looking for
clean_df = mergetwo_renamed[["Item ID", "Item Name", "Purchase Count", "Item Price", "Total Purchase Value"]]

prefinal_df = clean_df.set_index(['Item Name', 'Item ID'])
popular_items_final = prefinal_df.sort_values('Purchase Count', ascending=False).head(6)
popular_items_final.style.format({"Item Price": "${:.2f}", "Total Purchase Value": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item Name</th> 
        <th class="index_name level1" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level0 row0" >Arcane Gem</th> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level1 row0" >84</th> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow0_col0" class="data row0 col0" >11</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow0_col1" class="data row0 col1" >$2.23</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow0_col2" class="data row0 col2" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level0 row1" >Betrayal, Whisper of Grieving Widows</th> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level1 row1" >39</th> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow1_col0" class="data row1 col0" >11</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow1_col1" class="data row1 col1" >$2.35</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow1_col2" class="data row1 col2" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level0 row2" >Trickster</th> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level1 row2" >31</th> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow2_col0" class="data row2 col0" >9</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow2_col1" class="data row2 col1" >$2.07</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow2_col2" class="data row2 col2" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level0 row3" >Woeful Adamantite Claymore</th> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level1 row3" >175</th> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow3_col0" class="data row3 col0" >9</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow3_col1" class="data row3 col1" >$1.24</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow3_col2" class="data row3 col2" >$11.16</td> 
    </tr>    <tr> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level0 row4" >Serenity</th> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level1 row4" >13</th> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow4_col0" class="data row4 col0" >9</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow4_col1" class="data row4 col1" >$1.49</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow4_col2" class="data row4 col2" >$13.41</td> 
    </tr>    <tr> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level0 row5" >Retribution Axe</th> 
        <th id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664b" class="row_heading level1 row5" >34</th> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow5_col0" class="data row5 col0" >9</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow5_col1" class="data row5 col1" >$4.14</td> 
        <td id="T_6f34cc74_8f4f_11e7_8fb1_f40f240c664brow5_col2" class="data row5 col2" >$37.26</td> 
    </tr></tbody> 
</table> 



## Most Profitable Items


```python
#use prefinal dataframe from prior to step to find most profitable items

profit_items_final = prefinal_df.sort_values('Total Purchase Value', ascending=False).head()
profit_items_final.style.format({"Item Price": "${:.2f}", "Total Purchase Value": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item Name</th> 
        <th class="index_name level1" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level0 row0" >Retribution Axe</th> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level1 row0" >34</th> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow0_col0" class="data row0 col0" >9</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow0_col1" class="data row0 col1" >$4.14</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow0_col2" class="data row0 col2" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level0 row1" >Spectral Diamond Doomblade</th> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level1 row1" >115</th> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow1_col0" class="data row1 col0" >7</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow1_col1" class="data row1 col1" >$4.25</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow1_col2" class="data row1 col2" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level0 row2" >Orenmir</th> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level1 row2" >32</th> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow2_col0" class="data row2 col0" >6</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow2_col1" class="data row2 col1" >$4.95</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow2_col2" class="data row2 col2" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level0 row3" >Singed Scalpel</th> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level1 row3" >103</th> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow3_col0" class="data row3 col0" >6</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow3_col1" class="data row3 col1" >$4.87</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow3_col2" class="data row3 col2" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level0 row4" >Splitter, Foe Of Subtlety</th> 
        <th id="T_a283684a_8f4f_11e7_9ce3_f40f240c664b" class="row_heading level1 row4" >107</th> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow4_col0" class="data row4 col0" >8</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow4_col1" class="data row4 col1" >$3.61</td> 
        <td id="T_a283684a_8f4f_11e7_9ce3_f40f240c664brow4_col2" class="data row4 col2" >$28.88</td> 
    </tr></tbody> 
</table> 


