# Highest-Death-Count
which country has highest death count in covid
1.2 Problem Description:
A pharmaceutical company is launching a new immunity booster syrup that helps to boost your immunity to fight against coronavirus and helps you recover quickly if you have coronavirus. To increase sales, the company wants to know 
which country has the highest death count?
As a result, they begin production in that country to maximize profits rather than investing in a low-death-count country. also, they start their production on a similar range of products to increase sales and profit.
To answer the above question Map-reduce is used to know the country which has the maximum death count.

1.2.1. But why high computation infrastructure to answer this?
Corona has spread throughout the world, and everyone is suffering from Covid-19. In such a case, there is a new case count, a new death count, a new count of recovered people, different symptoms, and patients of different variants of the same virus generated every minute, and all this data is generated according to country, state, region, district, city, and area. And producing every second. So, because a massive amount of data is generated every minute all over the world, we need parallel processing and batch processing on a daily, weekly, and monthly basis to process this data. 
So, using the Hadoop technique, MapReduce jobs are designed to run on a daily, weekly, or monthly basis, depending on the need. As a result, the mapper and reducer will provide a death count based on country. so that we can get updated statistics on a daily or monthly basis. In this case, we can use India as an example, which is the world's second-most populous country; in such a case, a large amount of data is generated every second, so this will be useful.



1.3 Associated Dataset:
The dataset contains records of covid-19 cases and deaths from 211 countries and territories around the world. The dataset is obtained from the Kaggle website.https://www.kaggle.com/datasets/yaxin153537/covid19-death-and-cases-by-country
The Dataset contains the following details:
Name	Description 	Datatype
Date	Date	numeric
Day	Day split out from the date	numeric
Month	Month split out from the date	numeric
Year	Year split out from the date	numeric
Deaths	No of deaths recorded	numeric
Country and territories	Name of the country	string
geoId	Two-letter codes of countries and territories	string
Population	The population as reported in 2019	numeric
continent	Continent the country is in	string
Cumulative number	The cumulative number for 14days of COVID-19 cases per 1m people	numeric

The dataset contains missing values, so the data cleaning process is done before using it in RStudio. The original dataset contains 51041 rows after removing Na values 48040 rows have remained.








2. Design & Implementation 
2.1 Design:
MapReduce allows its users to process data on a large volume of data concurrently, and with parallel working (nodes), it can process the same data on multiple machines at the same time.
The data goes through the following phases of MapReduce in Big Data:
1.Input splits:  Data has been inserted into the framework and is ready for processing.
2. Mapper: This is the first stage of the map-reduce program's execution. During this phase, data from each input is passed to a mapping function, which generates output values.
3. Shuffle and sorting: This phase consumes the Mapping phase's output. Its job is to collect the relevant records from the Mapping phase output. It grouped a key-value pair according to the initial indexing
4. Reducer: The output values from the Shuffling phase are aggregated in this phase. This phase takes the values from the Shuffling phase and combines them into a single output value. In a nutshell, this phase summarizes the entire dataset.
5. Output: final key-value pairs are coming from the reducer and written to the output file as a result

Component	Input	Input types	Output	Output types
Mapper	Key: chosen column
Value: Data row	Key: Longwritable
Value: Text	Key: country name
Value: death count	Key: Text
Value: Double writable
Reducer	Key: country name
Value: death count list	Key: Text
Value: list of Double writable	Key: Country name
Value: Max Death count	Key: Text
Value: doublewritable




2.2 Implementation
Following are the step involved in running the program successfully:
1. write the mapper class
2.write the reducer class
3. write the Driver class
These are three java file which is created and exported into.JAR file and run on the HDFS 
First start with Mappr class:
1. Mapper class
 
 


Explanation on mapper class:
•	We begin by specifying a name of package for our class. Org.myorg is a name of our package. output of compilation, MaxDeathsMapper.class will go into a directory named by this package name: org.myorg
•	Followed by this, we import library packages.
•	Create a child class from the parent class. Every mapper class must extend MapReduce Base and implement the Mapper interface.
•	The main part of the Mapper class is a ‘map ()’ method that accepts arguments.
•	At every call to the ‘map ()’ method, a key-value pair (‘key’ and ‘value’ in this code) is passed
•	The ‘map()' method begins by splitting the input text received as an argument. Under the map method, there is an assignment statement 
•	String[] line = value.toString().split(",");  declaration of array
•	here its stating that country name  is element 6th in the array line
•	Here, ‘,’ is used as a delimiter. We are choosing the record at the 6th index because we need the Country name, and it is located at the 6th index in the array ‘MaxDeaths’.
•	extracting the element which is in the 5th index and saved in Deaths variable
•	Trim method is used to delete any space before and after the value.
•	The last line of code indicates that county name and deaths writable send them to the reducer.









2. Reducer class:
 



Explanation of Reducer class:
•	We begin by importing a name of package for our class. Org.myorg is a name of our package. output of compilation, MaxDeathsMapper.class will go into a directory named by this package name: org.myorg
•	Followed by this, we import library packages.
•	Here, the first two data types, ‘Text’ and ‘doubleWritable’ are data types of input key-value to the reducer.
•	Here, the first two data types, ‘Text’ and ‘doublewritable’ are data types of input key-value to the reducer.
•	Output of mapper is in the form of <CountryName1, 5>, <CountryName1, 2>. This output of the mapper becomes an input to the reducer. So, to align with its data type, Text and doubleWritable are used as data types here.
•	The last two data types, ‘Text’ and ‘doubleWritable’ are data types of output generated by the reducer in the form of key-value pair.
•	double maxValue=0; here its intialising 0
		for (DoubleWritable value : values) 
		{maxValue = Math.max(maxValue, value.get());
		}
		context.write(key, new DoubleWritable(maxValue));
•	this for loop execute as each value find the max value between two values.
•	On the math method compare which one is higher assign it to max value.
3. Driver class

 
 



Explanation of Driver class:
•	In this section, we will understand the implementation of MaxDeaths driver class
•	Importing packages
•	Define a driver class which will create a new client job, configuration object, and advertise Mapper and Reducer classes.
•	The driver class is responsible for setting our MapReduce job to run in Hadoop. In this class, we specify job name, the data type of input/output, and the names of the mapper and reducer classes.
•	This is the main method
•	Creating a conf object from configuration
•	Need to check three arguments
•	First name of driver class MaxDeaths, input folder where txt file is stored and output folder
•	If there is a missing in the argument it will give an error
•	Creating a job object to assign the job here in the above example Max Deaths is the job name
•	  It assigns a job to create a jar file 
•	Checking an argument where argument one is the input path i.e.
•	Hadoop jar Downloads/MaxDeaths.jar MaxDeaths /input/covid1.txt output this is an input path 
•	And argument 2 is output path
•	Set mapper class and reducer class
•	Last line of code state that if there is output folder already exist in HDFS delete that


3. Results & Evaluation
After the writing java file export, the java file and open the terminal
1.	Start the terminal
2.	Start -dfs.sh
3.	Start-yarn.sh
4.	JPS check that all nodes are working
5.	Making a directory
6.	Putting a text file into a directory

 



7.	execute the program on Hadoop 
 

Output:
 


Evaluation:
Looking at output, it can be said that the United States of America, Peru, Mexico, India, and France have the highest number of covid deaths cases, so starting production in this country first will be profitable for the company. The output also indicates which countries have the lowest death rate, allowing them to begin production in high-priority regions while investing at a low cost in low-death countries.
