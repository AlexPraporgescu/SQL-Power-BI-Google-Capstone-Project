  # Cyclistic - Google Capstone Project (MySQL+Power BI)

  ## <ins>Table of Contents</ins>

- [Project Overview](#project-overview)
- [Scenario](#scenario)
- [Role](#role)
- [Ask](#ask)
- [Prepare](#prepare)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Process](#process)
- [Analyze and Act](#analyze-and-act)
- [Results and Findings](#results-and-findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)
- [Notes](#notes)

  ### <ins>Project Overview</ins>

![image](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/7cc2205f-6f42-4a39-a439-7271150b21e9) ![image](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/c60c7780-e175-4374-9018-5929bada35f8)

  ### <ins>Scenario</ins>

Cyclistic is a bike-share program that features a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago.
The bikes can be unlocked from one station and returned to any other station in the system anytime.
Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike.
Up to this point, Cyclistic’s marketing approach focused on raising public awareness and appealing to a wide range of consumer groups.
The price plans' flexibility, which included single-ride passes, full-day passes, and annual memberships, was one strategy that assisted in making these things possible.
Casual riders are those who buy one-ride or all-day passes from the company. Cyclistic members are customers who purchase annual memberships.

  ### <ins>Role</ins> 

I am a junior data analyst working in the marketing analyst team at Cyclistic, tasked with designing a new marketing strategy to convert casual riders into annual members.
For the project I followed the steps of the data analysis process: ask, prepare, process, analyze, share and act.

  ### <ins>Ask</ins>

Business question: How do annual members and casual riders use Cyclistic bikes differently?

Business task: To design a new marketing strategy to convert casual riders into annual members.

Key Stakeholders: Lily Moreno (director of marketing) and the Cyclistic executive team.

### <ins>Prepare</ins>

Each excel files contains 13 columns containing information such as ride id, rideable type, ride time, location, etc.
The data was downloaded and backed up on computer.

  ### <ins>Data sources</ins>

I used Cyclistic’s historical trip data from December 2022 to December 2023 to analyze and identify trends.
The data can be found [here](https://divvy-tripdata.s3.amazonaws.com/index.html). It has been made available and licenced by Motivate International Inc.

  ### <ins>Tools</ins>

 - Excel - For initial data manipulation (downloaded data was in .csv format) [Download here](https://www.microsoft.com/ro-ro/microsoft-365/excel?market=ro).
 - MySQL - Data cleaning and preparing data for analysis [Download here](https://dev.mysql.com/downloads/).
 - Power BI - Data analysis, visualisation and creating the [Dashboard](#project-overview) [Download here](https://www.microsoft.com/en-us/download/details.aspx?id=58494).

  ### <ins>Process</ins>

After checking the data for each individual month in Microsoft Excel, I discovered that the data was consistent across all files, having the same number of columns and consistent column nomenclature.
I therefore decided to consolidate all the files into one before cleaning. The large number of rows (over 4 million) in the aggregated file led me to select MySQL.

The initial table was created with each column containing text data (in order to prevent data loss) and was called 'cyclistic'.
Then the empty table was populated with the data from the .csv files:

```sql
#Importing data into the table.

LOAD DATA INFILE 'tripdata1.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata2.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata3.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata4.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata5.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata6.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata7.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata8.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata9.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata10.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata11.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata12.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;

LOAD DATA INFILE 'tripdata13.csv' INTO TABLE cyclistic
FIELDS TERMINATED BY ','
IGNORE 1 LINES;
```

After importing I noticed that MySQL introduced each piece of data between "" symbols, so that had to be addressed:

Before:

![Initial data](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/70edcb50-5446-43fc-b835-ba9b6b56208f)

```sql
#Cleaning data (eliminating "" inserted by MySQL Workbench)

UPDATE cyclistic
   SET ride_id = TRIM(BOTH '"' FROM ride_id);
   
UPDATE cyclistic
   SET rideable_type = TRIM(BOTH '"' FROM rideable_type);
   
UPDATE cyclistic
   SET started_at = TRIM(BOTH '"' FROM started_at);
   
UPDATE cyclistic
   SET ended_at = TRIM(BOTH '"' FROM ended_at);
   
UPDATE cyclistic
   SET start_station_name = TRIM(BOTH '"' FROM start_station_name);
   
UPDATE cyclistic
   SET start_station_id = TRIM(BOTH '"' FROM start_station_id);

UPDATE cyclistic
   SET end_station_name = TRIM(BOTH '"' FROM end_station_name);
   
UPDATE cyclistic
   SET end_station_id = TRIM(BOTH '"' FROM end_station_id);
   
UPDATE cyclistic
   SET member_casual = TRIM(BOTH '"' FROM member_casual);
```

After:

![final](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/227907bc-0c4e-4ef6-a7f2-8d6c6ebd370e)

Because all the data is still in the text format (varchar), I also had to replace the missing data with NULL values:

```sql
#Replacing empty spaces with NULL values
   
UPDATE cyclistic
	SET ride_id=NULL WHERE ride_id='';
   
UPDATE cyclistic
	SET rideable_type=NULL WHERE rideable_type='';
   
UPDATE cyclistic
	SET started_at=NULL WHERE started_at='';
  
UPDATE cyclistic
	SET ended_at=NULL WHERE ended_at='';
  
UPDATE cyclistic
	SET start_station_name=NULL WHERE start_station_name='';
 
UPDATE cyclistic
	SET start_station_id=NULL WHERE start_station_id='';
 
UPDATE cyclistic
	SET end_station_name=NULL WHERE end_station_name='';
 
UPDATE cyclistic
	SET end_station_id=NULL WHERE end_station_id='';
 
UPDATE cyclistic
	SET start_lat=NULL WHERE start_lat='';
 
UPDATE cyclistic
	SET start_lng=NULL WHERE start_lng='';
 
UPDATE cyclistic
	SET end_lat=NULL WHERE end_lat='';

UPDATE cyclistic
	SET end_lng=NULL WHERE end_lng='';
 
UPDATE cyclistic
	SET member_casual=NULL WHERE member_casual='';
```

Now, that all the inconsistencies were addressed, each column was assigned the corect data type:

```sql
 #Assigning each column the appropriate data type
 
ALTER TABLE cyclistic MODIFY COLUMN started_at DATETIME;
 
ALTER TABLE cyclistic MODIFY COLUMN ended_at DATETIME;
 
ALTER TABLE cyclistic MODIFY COLUMN start_lat DOUBLE;

ALTER TABLE cyclistic MODIFY COLUMN start_lng DOUBLE;

ALTER TABLE cyclistic MODIFY COLUMN end_lat DOUBLE;

ALTER TABLE cyclistic MODIFY COLUMN end_lng DOUBLE;
```

Then, some exploratory analysis was done by doing the following operations:

 - Finding the total number of rides:

```sql
#Counting the number of rides as Number_of_rides

SELECT
  COUNT(ride_id) AS Number_of_rides
FROM cyclistic;
```

 - Checking if there is duplicate data in the dataset:

```sql
#Finding duplicate rider_id

SELECT
  ride_id,
  COUNT(ride_id)
FROM cyclistic
GROUP BY ride_id
HAVING COUNT(ride_id)>1;
```

 - Displaying only the NULL values:

```sql
#Finding NULL values

SELECT *
FROM cyclistic
WHERE ride_id IS NULL
		OR rideable_type IS NULL
		OR started_at IS NULL
		OR ended_at IS NULL
		OR start_station_name IS NULL
		OR start_station_id IS NULL
		OR end_station_name IS NULL
		OR end_station_id IS NULL
		OR start_lat IS NULL
		OR start_lng IS NULL
		OR end_lat IS NULL
		OR end_lng IS NULL
		OR member_casual IS NULL;
```

 - Calculating each ride length in minutes:

```sql
#Finding ride_length in minutes

SELECT
  started_at,
  ended_at,
  TIMESTAMPDIFF(MINUTE,started_at,ended_at) AS ride_length
FROM cyclistic;
```

 - Finding in which week day each ride was taken:

```sql
#Finding the day_of_week the ride took place

SELECT
  started_at,
  WEEKDAY(started_at) AS day_of_week
FROM cyclistic;
```

 - Creating a new table (cyclistic2) containing only the cleaned data without NULL or duplicated values. In the table were also included two new columns: ride_length and day_of_week.

```sql
#Creating a new table that includes ride_length and day_of_week
    
CREATE TABLE cyclistic2 AS
SELECT
  DISTINCT ride_id,
	rideable_type,
	started_at,
	ended_at,
	TIMESTAMPDIFF(MINUTE,started_at,ended_at) AS ride_length,
	WEEKDAY(started_at) AS day_of_week,
	start_station_name,
	start_station_id,
	end_station_name,
	end_station_id,
	start_lat,
	start_lng,
	end_lat,
	end_lng,
	member_casual
FROM cyclistic
WHERE ride_id IS NOT NULL
  AND rideable_type IS NOT NULL
  AND started_at IS NOT NULL
  AND ended_at IS NOT NULL
  AND start_station_name IS NOT NULL
  AND start_station_id IS NOT NULL
  AND end_station_name IS NOT NULL
  AND end_station_id IS NOT NULL
  AND start_lat IS NOT NULL
  AND start_lng IS NOT NULL
  AND end_lat IS NOT NULL
  AND end_lng IS NOT NULL
  AND member_casual IS NOT NULL;
```

 - With the new table I had to remove the ride lengths shorter than 1 minute or longer than 24 hours, as these were not representative for the analysis:

```sql
#Finding and deleting ride lengths shorter than 1 minute or longer than 24 hours

SELECT *
FROM cyclistic2
WHERE ride_length<1
	OR ride_length>1440;

DELETE
FROM cyclistic2
WHERE ride_length<1
	OR ride_length>1440;
```

 - An optional step was to replace all the integer values from the day_of_week column (0-6) with the coresponding strings for ease of understanding:

```sql
#Formatting day_of_week into string type variables

ALTER TABLE cyclistic2 MODIFY COLUMN day_of_week varchar (10);

UPDATE cyclistic2
	SET day_of_week = 'Monday' WHERE day_of_week=0;
    
UPDATE cyclistic2
	SET day_of_week = 'Tuesday' WHERE day_of_week='1';
    
UPDATE cyclistic2
	SET day_of_week = 'Wednesday' WHERE day_of_week='2';
   
UPDATE cyclistic2
	SET day_of_week = 'Thursday' WHERE day_of_week='3';
    
UPDATE cyclistic2
	SET day_of_week = 'Friday' WHERE day_of_week='4';
    
UPDATE cyclistic2
	SET day_of_week = 'Saturday' WHERE day_of_week='5';
    
UPDATE cyclistic2
	SET day_of_week = 'Sunday' WHERE day_of_week='6';
```

After all these steps, the data was ready for analysis so the database was connected to Power BI for the analysis phase.

  ### <ins>Analyze and Act</ins>

I wanted to see how members and casual users used the bikes differently.
I compared how they used the service based on a variety of factors, including usage by weekday and time of day, monthly usage, preferred rideable types by each group, average and daily ride durations and popular stations.

![image](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/ec78b500-70b2-44ab-9f06-19f9518e343c)

From the **Number of Rides Per Day of The Week** it can be seen that members use the bikes more on weekdays with a drop in usage over the weekends.
This indicates that they use them for their commute.
In contrast, the casual users use the service mostly on the weekends especially Saturdays which implies they use the bikes for recreation.

![image](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/53e55492-d912-4d67-90c7-a9ffbe95ecba)

On a daily basis, annual members ride the bikes more frequently during the hours of 8 am and 5 pm, which suggests they commute to and from work.
The bikes are used most frequently in the afternoon by both members and casual users, with the peak number of riders each day occurring at 5 pm.

![image](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/053a65bf-ac21-49dd-8b53-bfd13531f7ba)

The busiest time of the year for both members and casual users was the summer, from July to September, with August seeing the highest levels of usage.
Winter, from December through February, saw the least rides in both groups, particularly in January.

![image](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/b62722df-ae34-46b3-ad97-6b501d193f6c)

On average Casual users take longer rides compared to annual users.

![image](https://github.com/AlexPraporgescu/SQL-Power-BI-Google-Capstone-Project/assets/158141333/6fd8a3fe-d59d-40ac-bf08-8f8df31982f0)

  ### <ins>Results and Findings</ins>

The analysis results are comprised in the created [Dashboard](#project-overview). Some of the most important insights are:
 - Annual members use the bikes more on weekdays, indicating that they use them
to commute, whereas casual users use them more on weekends, which implies
that they use them for recreation.
 - On a daily basis, annual members ride the bikes more frequently during the hours
of 8 am and 5 pm, which suggests they commute to and from work. The bikes are
used most frequently in the afternoon by both members and casual users, with
the peak number of riders each day occurring at 5 pm.
 - The busiest time of year for both members and casual users is summer, from July
to September, with August seeing the highest levels of usage. Winter, from
December to February, saw the least rides in both groups, particularly in January.
 - On average Casual users take longer rides compared to annual users.

  ### <ins>Recommendations</ins>

Based on the analysis, the following actions can be recommended:
 - Weekends should be used for marketing activities aimed at enticing non-members to join
because weekend usage of the bikes is higher for casual users.
 - Due to the increase in bike utilization among both members and casual users in the summer,
marketing efforts should be concentrated towards and during the summer season.
 - Outdoor advertising should be deployed around the most popular stations between casual
users.
 - The first step in turning casual users into annual members could be to employ weekend-only
memberships.

  ### <ins>Limitations</ins>

 - Data-privacy issues prohibit you from using riders’ personally identifiable information. This means that you won’t be able to connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if they have purchased multiple single passes.
 - The dataset comprised only 13 months of data, which my be not thruthful for the entire population that should be considered.

  ### <ins>References</ins>

1. [Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics?)

  ### <ins>Notes</ins>

- This project represents my practice work in the field of data analysis. The dataset used may not represent the reality and therefore the findings should be treated in accordance.
- For more details about me and my work, or if you want to get in touch with me, please access my [LinkedIn profile](https://www.linkedin.com/in/alexpraporgescu/).
