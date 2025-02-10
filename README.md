# Glaciers

Project 4 - Group 2 Predicting Glacier Change in Alaska with Machine Learning

This project aims to develop machine learning models to understand glacier evolution in Alaska.

Data Sources:

a. Glacier data: “Glacier Covered Area for the State of Alaska, 1985-2020” (Dataset ID: G10040) created by the National Snow and Ice Data Center (NSIDC) (https://nsidc.org/data/g10040/versions/1). Registration on the Earthdata Login portal is required for data access.


b. Weather data: Alaska climate research center (ACRC) of the Alaska State Climate Center at the University of Alaska Fairbanks website (https://akclimate.org/data/)

Code Execution:

I. Data exploration and pre-preprocessing: 


a. Dataset Preparation:

Glacier data: Import the glacier area CSV file into PostgreSQL using pgAdmin. 

Weather data: Fetch historical weather data for 7 Alaskan cities from the ACRC data portal. Combine the CSV files into a CSV ‘Weather_data_1940_to_2024.csv’ in Excel.

b. Data cleaning:

•	Data cleaning for Unsupervised Machine Learning:

Glacier data:

1.	Read “AK_1985_to_2000_overall_glacier_covered_area.csv” into a pandas dataframe.
2.	Convert height, length and area columns to imperial units. Rename columns for clarity.
3.	Remove all columns except the following: 'Year', 'Lat', 'Lon', 'Area (mi^2)', 'Height (ft)' and 'Length (ft)'.
   
Weather data:

1.	Read the historical weather data for Juneau, Fairbanks, Anchorage and Valdez from ‘Weather_data_1940_to_2024.csv’ into a pandas dataframe.
2.	Read additional city data for Kodiak, Ketchikan and Delta Junction from “three more cities.csv” into a dataFrame.
3.	Merge dataFrames into a single weather dataFrame based on 'Date', 'Monthly Total Precipitation (in)', 'Monthly Average Mean Temperature (degF)', 'Monthly Total Snowfall (in)', and 'City’.
4.	Add latitude and longitude information for each city.
5.	Filter the data for the period 1986-2020 and sort by city and year.
6.	Group the data by year and compute averages for each year.
7.	Export the results to a CSV file titled ‘weathercombined.csv’ and save the file to your Data folder.
8.	Create a function to find the closest city to each glacier within a 3-degree threshold using geopy’s geodesic model.
9.	Apply the function to the glaciers DataFrame to add ‘closest_lat’ and ‘closest_lon’ columns.
10.	Drop rows with missing city locations.
11.	Create a dataframe named close_glaciers and replace the glacier’s latitude and longitudes with the corresponding values from closest_lat and closest_lon . Include the following columns: 'Year', 'Lat', 'Lon', 'Area (mi^2)', 'Height (ft)', 'Length (ft)'.
12.	Merge the close_glaciers dataframe with the annual_weather dataframe on 'Lat', 'Lon' and ‘Year’. Include the following columns in your merged dataframe: 'Year', 'Monthly Average Mean Temperature (degF)', 'Monthly Total Precipitation (in)', 'Monthly Total Snowfall (in)', 'Area (mi^2)', 'Height (ft)', 'Length (ft)'.
13.	Save the merged dataframe to a CSV file named ‘UMLprep.csv’ in your ‘Data’ folder.
    
•	Data Cleaning for 2nd Unsupervised Machine Learning

1.	Connect to the database using psychopg2. 
2.	Select all from the table. 
3.	Close the connection. 
4.	Read “AK_1985_to_2000_overall_glacier_covered_area.csv” into a pandas dataframe.
5.	Clarify column names and convert units to Imperial. 
6.	Remove excess columns and sort by glacier ID.
7.	Filter the data for the period 1986-2020 and sort by city and year.
8.	Calculate changes in area between 1985 and 2020.
9.	Read in the combined weather data in weathercombined.csv.
10.	Create a function to find the closest city to each glacier within a 3-degree threshold using geopy’s geodesic model.
11.	Apply the function to the glaciers DataFrame to add ‘closest_lat’ and ‘closest_lon’ columns.
12.	Drop rows with missing city locations.
13.	Create a dataframe named close_glaciers and replace the glacier’s latitude and longitudes with the corresponding values from closest_lat and closest_lon . Include the following columns: 'Year', 'Lat', 'Lon', 'Area (mi^2)', 'Height (ft)', 'Length (ft)'.
14.	Merge the close_glaciers dataframe with the annual_weather dataframe on 'Lat', 'Lon' and ‘Year’. Include the following columns in your merged dataframe: 'Year', 'Monthly Average Mean Temperature (degF)', 'Monthly Total Precipitation (in)', 'Monthly Total Snowfall (in)', 'Area (mi^2)', 'Height (ft)', 'Length (ft)'.
15.	Export the merged dataframe to ‘SecUMLprep.csv’ in your ‘Data’ folder.

•	Data Cleaning for Supervised Machine Learning:

1.	Read “AK_1985_to_2000_overall_glacier_covered_area.csv” into a pandas dataframe.
2.	Convert height, length and area to imperial units. Rename columns for clarity.
3.	Remove all columns except the following: 'Year', 'Lat', 'Lon', 'Area (mi^2)', 'Height (ft)' and 'Length (ft)'.
4.	Filter the dataframe to include data recorded in 1986 and 2020.
5.	Merge the dataframes for 1986 and 2020.
6.	Create a new column named “Shrinkage” that indicates whether the glacier area has decreased between 1985 and 2020 (True/False).
7.	Save the results in a dataframe named SMLprep with the following columns: Id', 'Name', 'Lat',' Lon',' Area (mi^2)_1986', 'Area (mi^2)_2020', 'Area Shrinkage (mi^2)', 'Shrinkage', 'Height (ft)', 'Length (ft)'.
8.	Compute the total shrinkage and the percentage of glaciers that shrunk between 1986 and 2020.
9.	Export the SMPprep dataframe to a CSV file named ‘SMLprep.csv’ in the “Data” folder.

    
II. Machine Learning Models


a. Building the Unsupervised Learning Model

1.	Read the ‘UMLprep.csv’ file into a pandas dataframe named raw_df.
2.	Create a list of potential k values from 1 to 11.
3.	Create an empty list to store the inertia values.
4.	Compute for each k using a for loop.
5.	Create a dictionary for k values and their corresponding inertia to plot the elbow curve.
6.	Plot a line chart with all the inertia values you have computed to identify the optimal k value.
7.	Initialize the K-means model with the selected k value.
8.	Fit the K-means model using to the dataframe.
9.	Create a copy of the DataFrame and add a new column with the predicted clusters.
10.	Plot the clusters with hvplot for the following x and y axes: “Area” – “Monthly Average Mean Temperature (degF)”; "Area (mi^2)" - "Monthly Total Precipitation (in)"; "Area (mi^2)"- "Monthly Total Snowfall (in)";"Height (ft)" - "Monthly Average Mean Temperature (degF)"; "Height (ft)"- "Monthly Total Precipitation (in)"; "Height (ft)" - "Monthly Total Snowfall (in)"; "Length (ft)" - "Monthly Average Mean Temperature (degF)"; "Length (ft)"- "Monthly Total Precipitation (in)"; "Length (ft)"- "Monthly Total Snowfall (in)".
    
b. Building the Supervised Learning Model

1.	Read SMLprep.csv into a pandas dataframe.
2.	Drop all columns except the following : 'Lat', 'Lon',' Area (mi^2)_1986',' Area (mi^2)_2020', 'Shrinkage', 'Height (ft)', 'Length (ft)'.
3.	Select the “Shrinkage” column as the target variable.
4.	Split your data into training and test datasets using train_test_split.
5.	Fit a logistic regression model by using the training data (X_train and y_train).
6.	Evaluate the performance of your model by generating a confusion matrix and a classification report.
   
c. Building the Second Supervised Learning Model

1.	Connect to the database using psychopg2. 
2.	Select all from the table. 
3.	Close the connection.
4.	Read “SecUMLprep.csv
5.	Read “SecUMLprep.csv” into a pandas dataframe.
6.	Use the StandardScaler module and fit_transform function to scale all columns with numerical values.
7.	Display the first three rows of the scaled data.
8.	Compute inertia for all k values for use in elbow graphing.
9.	Plot the elbow curve.
10. Define the model with the lower value of k clusters.
11. Use a random_state of 1 to generate the model.
12. Fit the model.
13. Make predictions.
14. Create a copy of the DataFrame and name it as predictions_df.
15. Add a class column with the labels to the predictions_df DataFrame.
16. Plot the clusters - Area Loss & Temperature, Area Loss & Precipitation, Area Loss & Snow.
17. Create dataframe that compares weather data and area.
18. Compute inertia for all k values for use in elbow graphing.
19. Plot the elbow curve.
20. Define the model with the lower value of k clusters.
21. Use a random_state of 1 to generate the model
22. Fit the model.
23. Make predictions.
24. Create a copy of the DataFrame and name it as predictions_df.
25. Add a class column with the labels to the predictions_df DataFrame.
26. Plot the clusters - Area Loss & Temperature, Area Loss & Precipitation, Area Loss & Snow, Area Loss & Initial Area.
27. Plot the clusters for Area Loss vs. Initial Area plot with the initial dataframe that has length and height to compare the plots.\
    
III. Geospatial Data Analysis and Visualization 

a. Data Visualization with Geopandas

1.	Download shapefiles from the NSIDC website.
2.	Read shapefiles from the local directory within a for loop.
3.	Create an empty list to store the shapefile dataframes.
4.	Use the ‘.glob’ module to find shapefiles paths.
5.	Append each shapefile dataframe to your list using geopandas.
6.	Concatenate all dataframes into a single dataframe.
7.	Get the shape, columns, and number of glaciers in the dataframe.
8.	Drop irrelevant columns.
9.	Compute the change in area between 1986 and 2020 for each glacier.
10.	Determine the number of glacier ids for 1986 and 2020.
11.	Compute the total change in area as well as the percentage of area change.
12.	Determine the number of glaciers that shrank, grew or experienced no change in area size during the same period.
13.	Filter your data to plot the change in area size between 1985 and 2020. Please note that the 1986 data covers the data for 1985 as well as the data for 1986.
14.	Create a map using geopandas and contextily to visualize the area changes for both years.

b. Data Cleaning for QGIS

1.	Get the EPSG information for the shapefiles by running a .crs command.
2.	Get the EPSG information for the shapefiles.
3.	Save the glaciers ids to separate geodataframes for 1986 and 2020.
4.	Export your GeoDataFrames to shapefiles.
5.	Export the concatenated dataframes into a CSV file.

c. QGIS

1.	Install QGIS.
2.	Import the shapefiles for the top 20 glaciers into QGIS.
3.	Add Openmaps to your map as a base layer.
4.	Add simple blue fill to the 1986 layer and a line pattern fill to the 2020 layer.
5.	Go to the attribute table of the 1986 layers.
6.	Go to the field calculator and add a field that shows the area change rounded to the nearest integer.
7.	Add labels to each glacier by concatenating the "name" field and the rounded area change field.
8.	Render your map using the Layout Loader plug-in.
9.	Add a title and a legend to the map.
10.	Save the map to your local directory as an image from the Layout tab.
    
Dependencies

1.	Python:
•	pathlib import Path, geodesic from geopy.distance, numpy, import hvplot.pandas
•	sklearn.cluster Kmeans, tree, sklearn.preprocessing StandardScaler, sklearn.model_selection train_test_split
•	sklearn.metrics confusion_matrix, confusion_matrix, accuracy_score, classification_report
•	geopandas, matplotlib.pyplot as plt, glob, plotly.express , contextily
2.	QGIS
   
Authors:

Project 4 - Group 2 Team Members: 

Didem Blum 
Heidi Fox 
Adam Raffel

Code Sources: 

SQL Server Connection with psychopg2 : https://github.com/jroelofsz/mobile_phone_data_dashboard














 









