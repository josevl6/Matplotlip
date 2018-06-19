
# Pyber Data Analysis




```python
#Import modules
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

## Trends

##### We can 


```python
# Read csv files
city_data = pd.read_csv("Pyber/raw_data/city_data.csv")
ride_data = pd.read_csv("Pyber/raw_data/ride_data.csv")
```


```python
# create dataframe
table_1 = pd.merge(ride_data, city_data, how = "outer", on = "city")
table_1.head()
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
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lake Jonathanshire</td>
      <td>2018-01-14 10:14:22</td>
      <td>13.83%</td>
      <td>5739410935873</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Lake Jonathanshire</td>
      <td>2018-04-07 20:51:11</td>
      <td>31.25%</td>
      <td>4441251834598</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lake Jonathanshire</td>
      <td>2018-03-09 23:45:55</td>
      <td>19.89%</td>
      <td>2389495660448</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Lake Jonathanshire</td>
      <td>2018-04-07 18:09:21</td>
      <td>24.28%</td>
      <td>7796805191168</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Lake Jonathanshire</td>
      <td>2018-01-02 14:14:50</td>
      <td>13.89%</td>
      <td>424254840012</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>



## Find:
##### Average Fare  in dollars Per City
##### Total Number of Rides Per City
##### Total Number of Drivers Per City
##### City Type (Urban, Suburban, Rural)


```python
# Sort the table by city in an ascending way
# Reset index to start from 0

table_1_sorted = table_1.sort_values("city", ascending = True)
new_table_1 = table_1_sorted.reset_index(drop=True)
new_table_1.head(4)
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
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Amandaburgh</td>
      <td>2018-01-11 02:22:07</td>
      <td>29.24%</td>
      <td>7279902884763</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Amandaburgh</td>
      <td>2018-03-05 02:15:38</td>
      <td>26.28%</td>
      <td>906850928986</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Amandaburgh</td>
      <td>2018-02-24 23:10:49</td>
      <td>43.66%</td>
      <td>6573820412437</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Amandaburgh</td>
      <td>2018-02-10 20:42:46</td>
      <td>36.17%</td>
      <td>6455620849753</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
group_by = new_table_1.groupby(["city"])

# Get average fare by city
average_by_fare = group_by["fare"].mean()

# Total Number of Rides Per City
# rides_per_city = new_table_1["city"].value_counts()

rides_per_city = group_by["ride_id"].count()


# Total Number of Drivers Per City
drivers_per_city = group_by["driver_count"].mean()

# Get City Type
city_type = list(group_by.apply(lambda x: x["type"].unique()))

# Create new DataFrame and insert all new variables
table_2 = pd.DataFrame({"Average Fare":average_by_fare,"Rides per City":rides_per_city,
                        "Drivers per City":drivers_per_city, "City Type": city_type}
                       ,columns=["Average Fare","Rides per City", "Drivers per City", "City Type"])

# Formatting 
pd.set_option("display.float_format", "${:,.2f}".format)
table_2["City Type"] = table_2["City Type"].str.get(0)

# Reset index "City"
table_2.index.name = "City"
table_2.reset_index(inplace=True)

table_2.head()
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
      <th>Average Fare</th>
      <th>Rides per City</th>
      <th>Drivers per City</th>
      <th>City Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Amandaburgh</td>
      <td>$24.64</td>
      <td>18</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barajasview</td>
      <td>$25.33</td>
      <td>22</td>
      <td>26</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Barronchester</td>
      <td>$36.42</td>
      <td>16</td>
      <td>11</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bethanyland</td>
      <td>$32.96</td>
      <td>18</td>
      <td>22</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bradshawfurt</td>
      <td>$40.06</td>
      <td>10</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
  </tbody>
</table>
</div>



## Build a Bubble Plot that showcases the relationship between four key variables:

##### Average Fare Per City
##### Total Number of Rides Per City
##### Total Number of Drivers Per City
##### City Type (Urban, Suburban, Rural)


```python
# Define the variables: Rides per city and average fare per city for each city type
rides_per_city_rural = table_2.loc[table_2["City Type"] == "Rural","Rides per City"]
average_by_fare_rural = table_2.loc[table_2["City Type"] == "Rural","Average Fare"]

rides_per_city_suburban = table_2.loc[table_2["City Type"] == "Suburban","Rides per City"]
average_by_fare_suburban = table_2.loc[table_2["City Type"] == "Suburban","Average Fare"]

rides_per_city_urban = table_2.loc[table_2["City Type"] == "Urban","Rides per City"]
average_by_fare_urban = table_2.loc[table_2["City Type"] == "Urban","Average Fare"]


rural = table_2.loc[table_2["City Type"] == "Rural","Drivers per City"]
suburban = table_2.loc[table_2["City Type"] == "Suburban","Drivers per City"]
urban = table_2.loc[table_2["City Type"] == "Urban","Drivers per City"]


# Plot the scatter chart
plt.scatter(rides_per_city_rural, average_by_fare_rural, s= rural*5, marker = "o", facecolors = "gold", 
            edgecolors = "black", alpha = 0.75, label = "Rural", linewidth = 1.5)

plt.scatter(rides_per_city_suburban , average_by_fare_suburban, s= suburban*5, marker = "o", facecolors = "lightskyblue", 
            edgecolors = "black", alpha = 0.75, label = "Suburban", linewidth = 1.5)

plt.scatter(rides_per_city_urban, average_by_fare_urban, s= urban*5, marker = "o", facecolors = "lightcoral", 
            edgecolors = "black", alpha = 0.75, label = "Urban",  linewidth = 1.5)

# Define limits, labels, title, and note(figtext)
x_limit = 40
y_limit = 45
plt.xlim(0,x_limit)
plt.ylim(15,y_limit)

plt.title("Ride Sharing Market Analysis (2018)")


plt.xlabel("Total Numbers of Rides (Per City)")
plt.ylabel("Average Fare ($)")
lgnd = plt.legend(loc=1, borderpad=.5, frameon=False, title="City Types")
lgnd.legendHandles[0]._sizes = [30]
lgnd.legendHandles[1]._sizes = [30]
lgnd.legendHandles[2]._sizes = [30]

plt.figtext(.92, .5, "Note : Size of Bubble Corresponds to Number of Drivers per City", rotation='horizontal', clip_on=True)

# Format chart using seaborn => Gray grid
sns.set_style(style='darkgrid')

# Plot the chart
plt.show()
 
```


![png](output_9_0.png)


## Now Get:
##### % of Total Fares by City Type
##### % of Total Rides by City Type
##### % of Total Drivers by City Type



```python
# Create a variable for group by "Type"
group = table_1.groupby(["type"])

# Create variables for different headers in the new dataframe
total_fare = table_1["fare"].sum()
fare_by_type = group["fare"].sum()

total_rides = table_1["ride_id"].count()
rides_by_type = group["ride_id"].count()

total_drivers = table_1["driver_count"].sum()
drivers_by_type = group["driver_count"].sum()

# Define formulas
percentage_fare_by_type = list(fare_by_type / total_fare * 100)
percentage_rides_by_type = rides_by_type / total_rides * 100
percentage_drivers_by_type = drivers_by_type / total_drivers * 100

# Format data in the dataframe 
pd.set_option("display.float_format","{:,.2f}%".format)

# Create dataframe and assign variables 
percentages_by_type_df = pd.DataFrame({"% of Total Fares by City Type":percentage_fare_by_type, "% of Total Rides by City Type":percentage_rides_by_type
                                      , "% of Total Drivers by City Type":percentage_drivers_by_type}
                                      ,columns=["% of Total Fares by City Type","% of Total Rides by City Type","% of Total Drivers by City Type"])

# Display new dataframe
percentages_by_type_df
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
      <th>% of Total Fares by City Type</th>
      <th>% of Total Rides by City Type</th>
      <th>% of Total Drivers by City Type</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>6.81%</td>
      <td>5.26%</td>
      <td>0.78%</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>30.46%</td>
      <td>26.32%</td>
      <td>12.47%</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>62.72%</td>
      <td>68.42%</td>
      <td>86.75%</td>
    </tr>
  </tbody>
</table>
</div>



## Build Pie Charts that represent the 3 different category percentages: 

##### % of Total Fares by City Type
##### % of Total Rides by City Type
##### % of Total Drivers by City Type


```python
# Create Pie charts for "Total Fares by City Type"
labels = ["Rural","Suburban", "Urban"]
colors = ["gold","lightskyblue","lightcoral"]
explode = (0, 0, 0.1)

plt.pie(percentage_fare_by_type, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=115)

plt.title("% of Total Fares by City Type")

# Plot Pie chart
plt.show()
```


![png](output_13_0.png)



```python
# Create Pie charts for "Total Rides by City Type"
plt.pie(percentage_rides_by_type, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=125)

plt.title("% of Total Rides by City Type")

# Plot Pie chart
plt.show()
```


![png](output_14_0.png)



```python
# Create Pie charts for "Total Drivers by City Type"
plt.pie(percentage_drivers_by_type, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=155)

plt.title("% of Total Drivers by City Type")

# Plot Pie chart
plt.show()
```


![png](output_15_0.png)

