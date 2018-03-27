

```python
# Dependencies
import csv
import matplotlib.pyplot as plt
import numpy as np
import requests
import pandas as pd
import openweathermapy.core as owm
from citipy import citipy
from pprint import pprint
import seaborn as sns


#API config key
from config import api_key
```


```python
# Create latitudes and longitudes
# Latitudes range from -90 to 90 and longitudes range from -180 to 180

latitude = np.random.uniform(-90,90,2000) # Function to generate a Random list of 2500 latitude items with a range of -90 to +90
longitude = np.random.uniform(-180,180,2000) # Function to generate a Random list of 2500 longitude items with a range of -180 + 180

print (latitude) # Verifying results via output print
print (longitude) # Verifying results via output print
```

    [-47.18770857  64.32710532   0.67961935 ...  12.74534406  19.3089526
      45.93347111]
    [150.18967279 -10.18976839 -73.51575939 ...  78.20806889  20.49777418
      43.46294698]



```python
# Create initial Cities Dataframe for 10 Latitudes and Longitudes
City_Df = pd.DataFrame({"Latitude": latitude, "Longitude": longitude})
City_Df.head() # Display top 5 records of our newly created dataframe
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
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-47.187709</td>
      <td>150.189673</td>
    </tr>
    <tr>
      <th>1</th>
      <td>64.327105</td>
      <td>-10.189768</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.679619</td>
      <td>-73.515759</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.451855</td>
      <td>121.040502</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.892951</td>
      <td>-120.945678</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Add additional columns for Cities, Temperature, Humidity, Cloudiness and Wind Speed to our City_Df
# Note that we used "" to specify initial entries for each new coloumn being added below

City_Df["City"] = ""
City_Df["Country Code"] = ""
City_Df["Temperature"] = ""
City_Df["Cloudiness"] = ""
City_Df["Wind Speed"] = ""
City_Df["Humidity"] = ""
City_Df["URL"] = ""

City_Df.head() # Display top 5 records of our new created dataframe to display newly created columns
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
      <th>Latitude</th>
      <th>Longitude</th>
      <th>City</th>
      <th>Country Code</th>
      <th>Temperature</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Humidity</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-47.187709</td>
      <td>150.189673</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>64.327105</td>
      <td>-10.189768</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.679619</td>
      <td>-73.515759</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.451855</td>
      <td>121.040502</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.892951</td>
      <td>-120.945678</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
# Pull the values of Cities for each Latitude and Longitude and append those values in the City column
# This is being done using our iterrows() method, and using our 'citypy' library/module


# Loop through the Cities_Df and run a latitude/longitude search for each city and its respective country code,
# and append the corresponding values - via iterrows() function and citypy library

for index, row in City_Df.iterrows(): # Start iterating through all the records in our dataframe via iterrows() function
    latitude = row['Latitude'] # Define our latitude variable
    longitude = row ["Longitude"] # Define our longitude variable
    
    # Using .set_value function to populate the 'City' coloumn of our dataframe with the value returned from the citypy function based on input latitude and logitude passed to citpy function
    City_Df.set_value(index, "City", citipy.nearest_city(latitude, longitude).city_name) # function specific syntax
    # Using .set_value function to populate the 'Country Code' coloumn of our dataframe with the value returned from the citypy function based on input latitude and logitude passed to citpy function
    City_Df.set_value(index, "Country Code", citipy.nearest_city(latitude, longitude).country_code) #function specific syntax


City_Df.head(10) # Display top 10 records of our dataframe

```

    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:13: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      del sys.path[0]
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:15: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      from ipykernel import kernelapp as app





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
      <th>Latitude</th>
      <th>Longitude</th>
      <th>City</th>
      <th>Country Code</th>
      <th>Temperature</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>Humidity</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-47.187709</td>
      <td>150.189673</td>
      <td>hobart</td>
      <td>au</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>64.327105</td>
      <td>-10.189768</td>
      <td>sorvag</td>
      <td>fo</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.679619</td>
      <td>-73.515759</td>
      <td>cartagena del chaira</td>
      <td>co</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.451855</td>
      <td>121.040502</td>
      <td>manuk mangkaw</td>
      <td>ph</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.892951</td>
      <td>-120.945678</td>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>5</th>
      <td>-89.430427</td>
      <td>-3.871148</td>
      <td>hermanus</td>
      <td>za</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>6</th>
      <td>40.761096</td>
      <td>-172.357856</td>
      <td>bethel</td>
      <td>us</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>7</th>
      <td>30.261891</td>
      <td>68.544314</td>
      <td>duki</td>
      <td>pk</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>8</th>
      <td>26.354655</td>
      <td>75.556195</td>
      <td>malpura</td>
      <td>in</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>9</th>
      <td>7.044268</td>
      <td>-50.992446</td>
      <td>cayenne</td>
      <td>gf</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
# Save config information
# This method is being used because in HW as they have asked us to print the URLs for each city
url = "http://api.openweathermap.org/data/2.5/weather?units=Imperial"
```


```python
# Create settings dictionary with information we're interested in
# This is because we will feed settings into the openweathermapy.core package 
# This is a good method but it will not help us out in our current assignment because we need to print the URLs
settings = {"units": "imperial", "appid": api_key} # Given a choice this is much easier for API URLs, but this will not help us for our homework
```


```python
# Loop through all the records of our City_Df via iterrows() method/function

for index, row in City_Df.iterrows():
    city = row['City'] # Define our city variable
    country = row ["Country Code"] # define our country variable
    city_country = str(city)+ "," + str(country) # define our city_country variable, which will the input to our URL
    
    #current_weather = owm.get_current(city_country, **settings) # This would have worked, but not for our HW as we need to print the URL
    
    current_weather_url = url + "&appid=" + api_key + "&q=" + city_country # Completing our input URL for JSON response
    current_weather = requests.get(current_weather_url).json() # This is the JSON Response back to us based on our input URL
    
    #print (current_weather_url) -> Commented this out, was used to verify our code function
    #print (current_weather) -> Commented this out, was used earlier to verify we are getting accurate JSON responses
        
    
    # Now Using Try and Except method to take care of exception responses with API doesn't return valid city/country temperature responses
    
    try:
        
        # Now, again, using .set_value function to populate the Temperature, Humidity, Cloudiness, Wind Speed
        # and URL columns of our DataFrame with the corresponding data being returned from the JSON responses
        
        City_Df.set_value(index, "Temperature", current_weather['main']['temp'])
        City_Df.set_value(index, "Humidity", current_weather['main']['humidity'])
        City_Df.set_value(index, "Cloudiness", current_weather['clouds']['all'])
        City_Df.set_value(index, "Wind Speed", current_weather['wind']['speed'])
        City_Df.set_value(index, "URL", current_weather_url)
        
        # Note - Instead of printing the URLs, I am populating the URL in the dataframe (as the URL column), which will be exported out in an excel csv
        # This makes for a very clean output for our code, versus printing 500+ lines out for each city in the terminal

        # Now, again, using .set_value function to populate the Temperature, Humidity, Cloudiness, Wind Speed 
        # and URL columns of our DataFrame with 
        # "No Data from API" response for all the items where JSON doesn't give us a valid response
        # This is achived via the 'except' function on the 'try and except' routine.
        
    except:
        City_Df.set_value(index, "Temperature", "No Data from API")
        City_Df.set_value(index, "Humidity", "No Data from API")
        City_Df.set_value(index, "Cloudiness", "No Data from API")
        City_Df.set_value(index, "Wind Speed", "No Data from API")
        City_Df.set_value(index, "URL", "No Data from API")
        # print("No Data found") # Just to show us roughly how many records have no valid responses from JSON. This was used
        # for initial debugging, now this has been commented out
        
# Now rearranging the sequence of columns in our City_Df dataframe
City_Df = City_Df[["City", "Country Code", "Latitude", "Longitude","Temperature", "Humidity", "Cloudiness", "Wind Speed", "URL"]]

print ("Process for populating JSON Responses is complete") # Marker to show that the code is complete
```

    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:24: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:25: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:26: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:27: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:28: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:39: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:40: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:41: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:42: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/tushaar/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:43: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead


    Process for populating JSON Responses is complete



```python
City_Df.head(10) # Displaying the first 10 records
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
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>hobart</td>
      <td>au</td>
      <td>-47.187709</td>
      <td>150.189673</td>
      <td>57.2</td>
      <td>67</td>
      <td>90</td>
      <td>8.05</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>sorvag</td>
      <td>fo</td>
      <td>64.327105</td>
      <td>-10.189768</td>
      <td>No Data from API</td>
      <td>No Data from API</td>
      <td>No Data from API</td>
      <td>No Data from API</td>
      <td>No Data from API</td>
    </tr>
    <tr>
      <th>2</th>
      <td>cartagena del chaira</td>
      <td>co</td>
      <td>0.679619</td>
      <td>-73.515759</td>
      <td>72.97</td>
      <td>99</td>
      <td>92</td>
      <td>2.93</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>manuk mangkaw</td>
      <td>ph</td>
      <td>3.451855</td>
      <td>121.040502</td>
      <td>83.01</td>
      <td>100</td>
      <td>76</td>
      <td>9.75</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td>7.892951</td>
      <td>-120.945678</td>
      <td>68</td>
      <td>72</td>
      <td>5</td>
      <td>11.41</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>hermanus</td>
      <td>za</td>
      <td>-89.430427</td>
      <td>-3.871148</td>
      <td>77.11</td>
      <td>29</td>
      <td>8</td>
      <td>9.31</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>bethel</td>
      <td>us</td>
      <td>40.761096</td>
      <td>-172.357856</td>
      <td>33.8</td>
      <td>74</td>
      <td>90</td>
      <td>10.29</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>duki</td>
      <td>pk</td>
      <td>30.261891</td>
      <td>68.544314</td>
      <td>68.43</td>
      <td>15</td>
      <td>0</td>
      <td>4.72</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>malpura</td>
      <td>in</td>
      <td>26.354655</td>
      <td>75.556195</td>
      <td>79.05</td>
      <td>25</td>
      <td>0</td>
      <td>5.95</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>cayenne</td>
      <td>gf</td>
      <td>7.044268</td>
      <td>-50.992446</td>
      <td>84.2</td>
      <td>70</td>
      <td>75</td>
      <td>11.41</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
  </tbody>
</table>
</div>




```python
City_Df.to_csv("City_Df.csv", header = True) # Writing the output of our DataFrame to a csv file
```


```python
# Now filtering out the rows which do not have any API data in our City Dataframe
City_Df_Filter = City_Df[City_Df["Temperature"]!="No Data from API"]
City_Df_Filter.head(10) #Displaying first 10 records
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
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>hobart</td>
      <td>au</td>
      <td>-47.187709</td>
      <td>150.189673</td>
      <td>57.2</td>
      <td>67</td>
      <td>90</td>
      <td>8.05</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>cartagena del chaira</td>
      <td>co</td>
      <td>0.679619</td>
      <td>-73.515759</td>
      <td>72.97</td>
      <td>99</td>
      <td>92</td>
      <td>2.93</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>manuk mangkaw</td>
      <td>ph</td>
      <td>3.451855</td>
      <td>121.040502</td>
      <td>83.01</td>
      <td>100</td>
      <td>76</td>
      <td>9.75</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td>7.892951</td>
      <td>-120.945678</td>
      <td>68</td>
      <td>72</td>
      <td>5</td>
      <td>11.41</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>hermanus</td>
      <td>za</td>
      <td>-89.430427</td>
      <td>-3.871148</td>
      <td>77.11</td>
      <td>29</td>
      <td>8</td>
      <td>9.31</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>bethel</td>
      <td>us</td>
      <td>40.761096</td>
      <td>-172.357856</td>
      <td>33.8</td>
      <td>74</td>
      <td>90</td>
      <td>10.29</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>duki</td>
      <td>pk</td>
      <td>30.261891</td>
      <td>68.544314</td>
      <td>68.43</td>
      <td>15</td>
      <td>0</td>
      <td>4.72</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>malpura</td>
      <td>in</td>
      <td>26.354655</td>
      <td>75.556195</td>
      <td>79.05</td>
      <td>25</td>
      <td>0</td>
      <td>5.95</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>cayenne</td>
      <td>gf</td>
      <td>7.044268</td>
      <td>-50.992446</td>
      <td>84.2</td>
      <td>70</td>
      <td>75</td>
      <td>11.41</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-73.906078</td>
      <td>-88.602061</td>
      <td>45.43</td>
      <td>52</td>
      <td>75</td>
      <td>17.22</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
  </tbody>
</table>
</div>




```python
City_Df_Filter.to_csv("City_Df_Clean.csv", header = True) # Writing the output of the dataframe to csv file
```


```python
# Checking the data types for our dataframe
City_Df_Filter.dtypes
```




    City             object
    Country Code     object
    Latitude        float64
    Longitude       float64
    Temperature      object
    Humidity         object
    Cloudiness       object
    Wind Speed       object
    URL              object
    dtype: object




```python
# Drop duplicate cities from our dataframe. This is because we may have some dupicate cities in our data based on our random selction of latitude and longititude
City_Df_Filter_Unique = City_Df_Filter.drop_duplicates(subset = ["City", "Country Code"], keep = 'first')
City_Df_Filter_Unique.head(10) # Displaying top 10 records
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
      <th>Country Code</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>hobart</td>
      <td>au</td>
      <td>-47.187709</td>
      <td>150.189673</td>
      <td>57.2</td>
      <td>67</td>
      <td>90</td>
      <td>8.05</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>cartagena del chaira</td>
      <td>co</td>
      <td>0.679619</td>
      <td>-73.515759</td>
      <td>72.97</td>
      <td>99</td>
      <td>92</td>
      <td>2.93</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>manuk mangkaw</td>
      <td>ph</td>
      <td>3.451855</td>
      <td>121.040502</td>
      <td>83.01</td>
      <td>100</td>
      <td>76</td>
      <td>9.75</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>cabo san lucas</td>
      <td>mx</td>
      <td>7.892951</td>
      <td>-120.945678</td>
      <td>68</td>
      <td>72</td>
      <td>5</td>
      <td>11.41</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>hermanus</td>
      <td>za</td>
      <td>-89.430427</td>
      <td>-3.871148</td>
      <td>77.11</td>
      <td>29</td>
      <td>8</td>
      <td>9.31</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>bethel</td>
      <td>us</td>
      <td>40.761096</td>
      <td>-172.357856</td>
      <td>33.8</td>
      <td>74</td>
      <td>90</td>
      <td>10.29</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>duki</td>
      <td>pk</td>
      <td>30.261891</td>
      <td>68.544314</td>
      <td>68.43</td>
      <td>15</td>
      <td>0</td>
      <td>4.72</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>malpura</td>
      <td>in</td>
      <td>26.354655</td>
      <td>75.556195</td>
      <td>79.05</td>
      <td>25</td>
      <td>0</td>
      <td>5.95</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>cayenne</td>
      <td>gf</td>
      <td>7.044268</td>
      <td>-50.992446</td>
      <td>84.2</td>
      <td>70</td>
      <td>75</td>
      <td>11.41</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>punta arenas</td>
      <td>cl</td>
      <td>-73.906078</td>
      <td>-88.602061</td>
      <td>45.43</td>
      <td>52</td>
      <td>75</td>
      <td>17.22</td>
      <td>http://api.openweathermap.org/data/2.5/weather...</td>
    </tr>
  </tbody>
</table>
</div>




```python
City_Df_Filter_Unique.to_csv("City_Df_Clean_Unique.csv", header = True) # Writing the output of the dataframe to csv file
```


```python
# Verifying the lengths of the 3 dataframes - to prove that as we clean out data, our dataframes get smaller in size
print ("Length of our original dataframe is: " + str (len(City_Df))) # Length of our original dataframe
print ("Length of our new dataframe with clean records: " + str (len(City_Df_Filter))) # Length of our new dataframe with records for no API responses removed
print ("Length of our new dataframe with unique cities is: " + str (len(City_Df_Filter_Unique))) # Lenght of our new dataframe with duplicate cities removed
```

    Length of our original dataframe is: 2000
    Length of our new dataframe with clean records: 1741
    Length of our new dataframe with unique cities is: 662



```python
## The code for Plots follow
```


```python
# Use Seaborn
sns.set()

# Build a scatter plot for Temperature (F) vs. Latitude

plt.scatter(City_Df_Filter_Unique["Latitude"], City_Df_Filter_Unique["Temperature"], marker = "o" , color = "maroon",)
plt.xlim(-100,100)

# Incorporate the other graph properties
plt.title("Temperature in World Cities") # Create the title
plt.ylabel("Temperature (Farenheit)") # Create the y label
plt.xlabel("Latitude") # Create the x label
plt.grid(True) # set the grid for our plot (without this command there will no grid)
fig_size = plt.rcParams["figure.figsize"] # Syntax to scale the fix size, by assigning a variable
fig_size[0] = 20 # setting x scale
fig_size[1] = 9 # setting y scale
plt.rcParams["figure.figsize"] = fig_size # writing values back to fig size on the adjusted x and y scale of the variable
plt.savefig("Images/Tushaar_LatitudeVsTemperature.png") # saving the graph as a .png image 
plt.show() # showing our chart/plot/graph

```


![png](output_17_0.png)



```python
# Use Seaborn
sns.set()

# Build a scatter plot for Humidity (Percentage) vs. Latitude

plt.scatter(City_Df_Filter_Unique["Latitude"], City_Df_Filter_Unique["Humidity"], marker = "o" , color = "coral",)
plt.xlim(-100,100)

# Incorporate the other graph properties
plt.title("Humidity in World Cities")  # Create the title
plt.ylabel("Humidity (Percentage)") # Create the y label
plt.xlabel("Latitude")  # Create the x label
plt.grid(True)  # set the grid for our plot (without this command there will no grid)
fig_size = plt.rcParams["figure.figsize"] # Syntax to scale the fix size, by assigning a variable
fig_size[0] = 20 # setting x scale
fig_size[1] = 9 # setting y scale
plt.rcParams["figure.figsize"] = fig_size # writing values back to fig size on the adjusted x and y scale of the variable
plt.savefig("Images/Tushaar_LatitudeVsHumidity.png") # saving the graph as a .png image
plt.show()  # displaying our chart/plot/graph

```


![png](output_18_0.png)



```python
# Use Seaborn
sns.set()

# Build a scatter plot for Cloudiness (Percentage) vs. Latitude

plt.scatter(City_Df_Filter_Unique["Latitude"], City_Df_Filter_Unique["Cloudiness"], marker = "o" , color = "turquoise",)
plt.xlim(-100,100)

# Incorporate the other graph properties
# Comments for the below line of code is exactly same as the above 2 charts
plt.title("Cloudiness in World Cities")
plt.ylabel("Cloudiness (Percentage)")
plt.xlabel("Latitude")
plt.grid(True)
fig_size = plt.rcParams["figure.figsize"]
fig_size[0] = 20 
fig_size[1] = 9
plt.rcParams["figure.figsize"] = fig_size
plt.savefig("Images/Tushaar_LatitudeVsCloudiness.png")
plt.show()
```


![png](output_19_0.png)



```python
# Use Seaborn
sns.set()

# Build a scatter plot for Windspeed (mph) (F) vs. Latitude

plt.scatter(City_Df_Filter_Unique["Latitude"], City_Df_Filter_Unique["Wind Speed"], marker = "o" , color = "darkblue",)
plt.xlim(-100,100)

# Incorporate the other graph properties
# Comments for the below line of code is exactly same as the above 2 charts

plt.title("Windspeed in World Cities")
plt.ylabel("Windspeed (mph)")
plt.xlabel("Latitude")
plt.grid(True)
fig_size = plt.rcParams["figure.figsize"]
fig_size[0] = 20 
fig_size[1] = 9
plt.rcParams["figure.figsize"] = fig_size
plt.savefig("Images/Tushaar_LatitudeVsWind_Speed.png")
plt.show()
```


![png](output_20_0.png)

