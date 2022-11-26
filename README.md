# Google-Data-Analytics-Capstone-Project---Case-study-1



## Introduction



This is my version of the Google Data Analytics Capstone - Case Study 1. The full documentation of the case study as well as the .csv files(I could not upload the .csv files here on GitHub because the sizes exceed 25mb) can be found in the 'Google Data Analytics Capstone: Complete a Case Study course'.

For this project, these steps below will be followed to ensure its completion:
•	the six Google data analysis phases: Ask, Prepare, Process, Analyze, Share, and Act.
•	Each step will also follow these roadmaps with: Guiding questions and answers, Key tasks as a checklist, Deliverable as a checklist and  Codings if needed.


### Ask Phase

For the Ask step, let's get some context from the Cyclistic Bike Share document:
- Scenario: As a junior data analyst working in the Marketing Analytics team at Cyclistic (a bike-share company in Chicago), the Director of Marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design new marketing strategies to convert casual riders into annual members. But firstly, Cyclistic Executives must approve your recommendations which must be backed up with compelling data insights and professional data visualizations.

- Characters:
1. Cyclistic: A bike-share program that features more than 5,800 bicycles and 600 docking stations. It sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.
2. Cyclistic Executive team: The notoriously detail-oriented Executive team will decide whether to approve the recommended marketing strategies.
3. Marketing Analytics team: A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. As a junior data analyst who joined this team six months ago, you have been busy learning about Cyclistic’s mission and business goals as well as how to help Cyclistic achieve them.
4. Lily Moreno: The director of Marketing Analytics team who is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.

- Guiding questions:
1. What is the problem you are trying to solve?
   The main objective is to determine a way to build a profile for annual members and the best marketing strategies to turn casual bike riders into annual members.
2. How can your insights drive business decisions?
   The insights will help the marketing team to increase annual members.
   
- Key tasks:
1. Identify the business task
2. Consider key stakeholders

- Deliverables: 
1. A clear statement of the business task.
2. Find the keys differences between casual and members riders and how digital media could influence them


### Prepare Phase

The project will use the data provided as provided by Google which is also linked to Kaggle.

- Guiding questions:
1. Where is your data located?
   The data is located in a Kaggle dataset.
2. How is the data organized?
   The data is separated by month, each on its own CSV
3. Are there issues with bias or credibility in this data?
   Does your data ROCCC?The dataset ROCCCs because it's reliable, original, comprehensive, current and cited.Bias isn't a problem.
4. How are you addressing licensing, privacy, security, and accessibility?
   The company has its own license over the dataset. Besides that, the dataset doesn't have any personal information about the riders.
5. How did you verify the data’s integrity?
   All the files have consistent columns and each column has the correct type of data.
6. How does it help you answer your question?
   It may have some key insights about the riders and their riding style
7. Are there any problems with the data?
   It would be good to have some updated information about the bike stations. Also, more information about the riders could be useful.

- Key tasks:
1. Download data and store it appropriately.
2. Identify how it’s organized.
3. Sort and filter the data.

- Deliverables:
1. A description of all data sources used. The main data source contains datasets of 12 months (Between June 2021 and May 2022) from the Cylistic company.


### Process Phase

This phase aims to prepare the data for analysis. All the csv files will be appeneded into one dataframe (since they all have the same column arrangements and column datatypes) to improve workflow. The datasets were imported into R studio from the 'environment' pane and appended with rbindlist from data.table package. Alternatively, the sheets can be first combined into a single excel workbook and imported with the excel_sheet function.

- Code Dependences: The main dependencies for the project will be tidyverse, tidyr, readxl, dplyr and data.table packages and their libraries.

installed.packages("tidyverse")
library(tidyverse)

install.packages("data.table")
library(data.table)

install.packages("tidyr")
library(tidyr)

install.packages("readxl")
library(readxl)

install.packages("dplyr")
library(dplyr)

- Appendation: All the csv files will be concatenated into one dataframe and also viewed using the code below
 
comb_files <- rbindlist(list(X202106_divvy_tripdata, X202107_divvy_tripdata, X202108_divvy_tripdata, X202109_divvy_tripdata, X202110_divvy_tripdata, X202111_divvy_tripdata, X202112_divvy_tripdata, X202201_divvy_tripdata, X202202_divvy_tripdata, X202203_divvy_tripdata, X202204_divvy_tripdata, X202205_divvy_tripdata))

View(comb_files)

- Removing Duplicates:
 
 unique_rides <- comb_files[!duplicated(comb_files$ride_id), ]

- Parsing datetime columns: Separate the datetime columns into different date and time columns( with correct date and time formats respectively) and view the result using the codes below.

unique_rides$start_date <- as.Date(unique_rides$started_at)                                   
unique_rides$end_date <- as.Date(unique_rides$ended_at)  

unique_rides$started_at <- format(as.POSIXct(unique_rides$started_at), format = "%H:%M:%S")
unique_rides$ended_at <- format(as.POSIXct(unique_rides$ended_at), format = "%H:%M:%S")

View(unique_rides)


- Derived columns: Create new columns for better data exploration such as daynuumber and monthnumber and view the result using the codes below..

unique_rides <- unique_rides %>% mutate(start_month = format(unique_rides$start_date, "%mmm"))   
unique_rides <- unique_rides %>% mutate(end_month = format(unique_rides$end_date, "%mmm"))

unique_rides <- unique_rides %>% mutate(start_day = format(unique_rides$start_date, "%ddd"))
unique_rides <- unique_rides %>% mutate(end_day = format(unique_rides$end_date, "%ddd"))

View(unique_rides)

- Removing Irrelevant Columns:

col <- c("unique_rides$start_station_name", "unique_rides$start_station_id", "unique_rides$start_lat", "unique_rides$start_lng", "unique_rides$end_station_name", "unique_rides$end_lat", "unique_rides$end_station_id", "unique_rides$end_lng", "unique_rides$start_mins", "unique_rides$end_mins")

- Save the dataframe as a .csv file:

unique_rides %>% write.csv("clean_cyclist_dataset.csv")
 
- Guiding questions:
1. What tools are you choosing and why?
   I'm using R for this project, for two main reasons: Because of the large dataset and to gather experience with the language.
22. Have you ensured your datasets' integrity?
   Yes, the data is consistent throughout the columns.
3. What steps have you taken to ensure that your data is clean?
   First the duplicated values were removed and the relevant columns were formatted to their correct format.
4. How can you verify that your data is clean and ready to analyze?
   It can be verified from the cleaned and saved .csv file
5. Have you documented your cleaning process so you can review and share those results?
   Yes, it was all documented.

- Key tasks:
1. Choose your data exploratory tool.
2. Check the data for errors.
3. Transform the data so you can work with it effectively.
4. Document the cleaning process.

- Deliverables:
1. Documentation of all data cleaning and transformation steps.


   
   
   
   
   






## DATA DISTRIBUTION AND VISUALISATION


#7 Find the total number of rides recorded.

nrow(unique_rides)

#8 Find the distribution of rides according to the member groups and make its bar chart visual.

members_data <- unique_rides %>% group_by(member_casual) %>% summarise(count = n_distinct(ride_id))
members_data[order(members_data$count, decreasing = TRUE),]   #to arrange in decreasing order
view(members_data)

ggplot(unique_rides, aes(member_casual, fill=member_casual)) +
  geom_bar() +
  labs(x="Casuals vs Members", title="Chart 01 - Casuals vs Members Distribution")

#9 Find the distribution of rides according to the rideable types and make its bar chart visual.

ridetype_data <- unique_rides %>% group_by(rideable_type) %>% summarise(count = n_distinct(ride_id))
ridetype_data[order(ridetype_data$count, decreasing = TRUE),]   #to arrange in decreasing order
view(ridetype_data)

ggplot(unique_rides, aes(rideable_type, fill=rideable_type)) +
  geom_bar() +
  labs(x="Different types of rides", title="Chart 02 - Distribution per Rideable Types")

#10 Find the distribution of rides according to the month and make its bar chart visual.

months_data <- unique_rides %>% group_by(start_month) %>% summarise(count = n_distinct(ride_id))
months_data[order(months_data$count, decreasing = TRUE),]   #to arrange in decreasing order
view(months_data)

ggplot(unique_rides, aes(start_month, fill=start_month)) +
  geom_bar() +
  labs(x="Month NumberCodes", title="Chart 03 - Distribution per Month")

#11 Find the distribution of rides according to the days and make its bar chart visual.

days_data <- unique_rides %>% group_by(start_day) %>% summarise(count = n_distinct(ride_id))
days_data[order(days_data$count, decreasing = TRUE),]   #to arrange in decreasing order
view(days_data)

ggplot(unique_rides, aes(y = start_day, fill=start_day)) +
  geom_bar() +
  labs(y="Day NumberCodes", title="Chart 04 - Distribution per Day")

## CREATION OF A DASHBOARD TO CONTAIN ALL THE DIFFERENT VISUALS AND SAVING IT AS A PICTURE OR R-FILE.
