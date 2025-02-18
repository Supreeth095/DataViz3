# Assignment 3 - Creating Charts in Tableau

### Learning Goal
Train yourself in using Tableau for creating effective visualizations


### Instructions

#### Step 1 - Loading and preparing the Atlanta Crime dataset

The dataset we are going to analyze details the crimes committed in the city/greater metropolitan area of Atlanta from the year 2009 to 2018. This will be very similar to the dataset we used in Assignment 2.

- Load the dataset in Tableau (you can just drag and drop the csv file on the main window)
- Select the type Extract as type of connection. This will force Tableau to extract the information from the .csv file and import it into its own format (as a result you will be using more memory but all the interactions will be faster).
- Take a look at the data extracted and familiarize with the dataset. We will need to solve a few issues before proceeding with producing some visualization.


- Problem 1, `Occur Date` and `Occur Time` are treated as different columns. As a result, `Occur Time` is not recognized as a variable indicating time. To solve this we will have to create a new field.
  - Click on `Occur Time` and select `Create a calculated field`. Now we have to write a formula for merging `Occur Date` and `Occur Time`. To do this we will be using the functions `MAKEDATETIME`, `MAKETIME` and `INT`.
  - Take your time to familiarize with these functions by writing them into the calculation field. The interpreter will tell you if there are syntax errors in your function. You want to write down something `MAKEDATETIME(the date goes here, MAKETIME(hour goes here, minutes go here, this can be always 0))`.
  - Repeat the same with `Possible Date` and `Possible Time`.


- Problem 2, `NPU` refers to the Neighborhood Planning Unit. The City of Atlanta is divided into twenty-five (25) Neighborhood Planning Units (NPUs), which are citizen advisory councils that make recommendations to the Mayor and City Council on zoning, land use, and other planning-related matters. In the current dataset we do have the name of the NPU where a crime has been committed but we do not have any geometry telling us where the NPU is in practice. We need those information if we want to create a map.

  - Luckily, we live in a world where open data are more and more common. These type of geometrical data (used in Geographical Information Systems) are generally encoded as Shapefiles (.shp). With a quick search on google we can find the Shapefile we need [here](https://dcp-coaplangis.opendata.arcgis.com/datasets/npu).

  - I have already downloaded the files for you and you can find them in the folder NPU. Now, go back to Tableau to the `Data source` view.
  - Add the NPU.shp file here. There will be no need to read the other files. They will be included automatically.
  - Now you should be in the situation where the two datasets are linked by a inner join. That means that for each row of the `COBRA` dataset we have additional attributes telling us information about the geometry of the NPU. However, we do have an issue. The join is happening on attributes that do not match and, for this reason, we no longer see any values in the table.
  - To solve this, click on the icon of the inner join and select `Name` as the attribute to be used for the NPU dataset, to perform the join. Now your table rows should be back


- Problem 3, `Neighborhood` refer to an organization of the city of Atlanta which is at a smaller granularity with respect to the NPUs. Creating maps based on this subdivision could be helpful too. Replicate the same pipeline used for the NPUs. Shape files for the Neighborhoods can be found in the folder Neighborhood (or downloaded from the same website as before).


#### Step 2 - Playing with the brand new geometries.

Let's try to create a heatmap simply showing the number of crimes reported since 2009 on each NPU.

- Start a new Worksheet.
- From NPU.shp drag and drop the `Geometry` field. This should get you a nice representation of each NPU based on their boundaries.
- To create the heatmap we need to show the total number of crimes reported on each NPU. We need the field `Number of records` for doing this. Drag and drop such field into the Marks area and select `Color` as a channel to be used.
- Sadly, Tableau does not understand how to aggregate data, but that is going to be a very easy fix. Just drop and drop the `NPU` field from `COBRA` into the Marks area and BOOM, we will get what we were searching for.

![chart1](/Sheet1.png)




Design choices: 


1.I have used the amount of luminance to display the heat map for the number of crimes. 

A higher luminance implies high crime rate.

A lower luminance implies lower crime rate.

This is intuitive to the observer.


2.I have used Area to display the various NPUs. This is the best for of representation as all the details such as size, shape of the NPU etc. can be seen by the user. 


3. Since the data set focuses on crime records since 2008, I have filtered out the previous years' records for all visualizations. 


4. Interactions: Hovering over an NPU area will give the details:

    1. Name of the NPU
    
    2. Year of occur date
    
    3. Number of records.
    
    

Attributes and their marks and channels are:

1.NPU Name

Type:Categorical

Mark: Area

Channel: Shape

2.Number of records

Type:Ordinal

Mark: Area

Channel: color







#### Step 3 - From sketches to visualizations.

Take the sketches you made during assignment 2 for answering the following questions

1. What are the most dangerous neighborhoods?

![chart1](/Sheet2.png)


Design choices: 


1.I have used the amount of saturation+ hue (yellow to red) to display the heat map for the number of crimes. 

A higher luminance implies high crime rate.

A lower luminance implies lower crime rate.

This is intuitive to the observer.


2.I have used Area to display the various Neighborhoods. This is the best for of representation as all the details such as size, shape of the Neighborhood etc., can be seen by the user. 



3.I have added a time drop down so that the user can see the values based on the year. 


4. Interactions: Hovering over an Neigborhood area will give the details:

    1. Name of the Neigborhood
    
    2. Year of occur date
    
    3. Number of records.
    



Attributes and their marks and channels are:

1.Neighborhood Name

Type:Categorical

Mark: Area

Channel: Shape

2.Number of records

Type:Ordinal

Mark: Area

Channel: color





2. What are the most dangerous times of day?


![chart1](/Sheet3.png)



Design choices: 

I have used hour(time) as a position on the Y axis and number of crimes on the X axis. 

I have used a bar chart so that it is easy to get the trends of crimes in the day and over the years. 

I have added a time drop down so that the user can see the values based on the year. 

Interactions: Hovering over a bar area will give the details:

    1. Hour part in the time interval
    
    2. Year of occur date
    
    3. Number of records.



Attributes and their marks and channels are:

1.Time (hour):

Type:Ordinal

Mark: point

Channel: Position(Vertical)

2.Number of Crimes records in each interval

Type:Quantitative

Mark: point

Channel: Position(Horizontal)






3. Are crimes increasing or decreasing since 2008?

![chart1](/Sheet4.png)


Design choices: 

I have used year(time) as a position on the X axis and number of crime records on the Y axis. 

I have used a line chart so that it is easy to get the trends of total yearly crimes.

Interactions: Hovering over a line area will give the details:

    1. Year of occur date of the next point.
    
    2. Number of records.



Attributes and their marks and channels are:

1.Occur Date(Year)

Type:Ordinal

Mark: Point

Channel: Position(Horizontal)

2.Number of Crime records in each year

Type:Quantitative

Mark: Point

Channel: Position(Vertical) 




Create three worksheets either by implementing your original sketches submitted for Assignment 2 or by creating brand new visualizations.


#### Step 4 - Create a Dashboard

Create a dashboard for studying the crimes in the city of Atlanta. The dashboard will have to integrate filtering and interactive techniques for studying how crimes changed over time (years). The dashboard will have to include also a map for showing the geographical distribution of the crimes.

![chart1](/Dashboard1.png)


Interactions provided by the Dashboard:


Dashboard contains 3 worsheets:

Worksheet 1. Most Dangerous Neighbourhoods in Atlanta.

Worksheet 2. Most Dangerous times of day in Atlanta.

Worksheet 3. Crime trends since 2008 in Atlanta.


Interactions:



In Worksheets 1:

    Hovering over an Neigborhood area will give the details:
    
    1. Name of the Neigborhood
    
    2. Year of occur date
    
    3. Number of records.
    

In Worksheet 2:

  Interactions: Hovering over a bar area will give the details:
  
    1. Hour part in the time interval
    
    2. Year of occur date
    
    3. Number of records.
    


In Worksheet 3:

Interactions: Hovering over a line area will give the details:

    1. Year of occur date of the next point.
    
    2. Number of records.
    

    
Year Drop-down interactions:


Worksheets 1 and 2 change with year selected in the drop-down and user interaction is possible. 

Worksheet 3 remains independant of the year drop down.
  


### Submission
My Tablue Workbook has been uploaded to the repository. 


### Submission

Upload your Tableau workbook.

Modify this README file adding the visualization produced as images. For each image explain your design choices. For the dashboard, explain the interactions provided by the dashboard.
