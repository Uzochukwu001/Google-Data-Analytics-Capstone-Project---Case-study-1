# Google-Data-Analytics-Capstone-Project---Case-study-1



## Introduction



This is my version of the Google Data Analytics Capstone - Case Study 1. The full documentation of the case study as well as the .csv files(I could not upload the .csv files here on GitHub because the sizes exceed 25mb) can be found in the 'Google Data Analytics Capstone: Complete a Case Study course'.

For this project, these steps below will be followed to ensure its completion:
- the six Google data analysis phases: Ask, Prepare, Process, Analyze, Share, and Act.
- Each step will also follow these roadmaps with: Guiding questions and answers, Key tasks as a checklist, Deliverable as a checklist and  Codings if needed.


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
2. Have you ensured your datasets' integrity?
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


### Analyze Phase

The data exploration will check how annual members differ from casual riders.

- Code: Here we want to try to answer the most basic questions about how the data is distributed.

- What is the total number of rides over the 12 months?

nrow(unique_rides)

After running the above code, it can be seen there was a total of 5,860,776 rides in the past 12 months.

- What is the distribution of the total rides between members and casual riders?

members_data <- unique_rides %>% group_by(member_casual) %>% summarise(count = n_distinct(ride_id))

-- to arrange in decreasing order

members_data[order(members_data$count, decreasing = TRUE),] 

-- to view the distribution in the dataset

ggplot(unique_rides, aes(member_casual, fill=member_casual)) +
  geom_bar() +
  labs(x="Casuals vs Members", title="Chart 01 - Casuals vs Members Distribution")
  
  
  ![Cyclistic Members](https://user-images.githubusercontent.com/112668327/204116478-b1a7a91c-7d14-4cd7-ace7-d6317b4a8b85.jpeg)


-- it can be seen that:
1. there were about 2,559,857 Casual rides and 3,300,919 Member rides. 

- What is the distribution of the total rides per month?
 
months_data <- unique_rides %>% group_by(start_month) %>% summarise(count = n_distinct(ride_id))

-- to view the dataset in the  distribution

ggplot(unique_rides, aes(start_month, fill=start_month)) +
  geom_bar() +
  labs(x="Month NumberCodes", title="Chart 03 - Distribution per Month")


![Cyclistic MonthDistribution](https://user-images.githubusercontent.com/112668327/204116593-f7714f24-0833-4042-a72b-2effdf091922.jpeg)


It can be seen that:
1. There was more data points in the second half of 2021.
2. The month with the biggest count of rides was July (with 14% of the total dataset).
3. The month with the smallest count of rides was January with (with 1.8% of the total dataset).
4. In all months we have more members' rides than casual rides. This can be as a result of returning members).

- What is the distribution of the total rides per day of the week?

days_data <- unique_rides %>% group_by(start_day) %>% summarise(count = n_distinct(ride_id))

-- to view the dataset in the  distribution

ggplot(unique_rides, aes(y = start_day, fill=start_day)) +
  geom_bar() +
  labs(y="Day NumberCodes", title="Chart 04 - Distribution per Day")


![Cyclistic DayDistribution](https://user-images.githubusercontent.com/112668327/204116909-7d4b5c75-9a98-4170-a7b4-e01e51e04050.jpeg)


It can be seen that:
1. there was more data points in the first three weeks of each month.
2. we usually record the lowest number of rides towards the end of each month (further investigation can be done on this).

- What is the distribution of the total rides per type of bike?

ridetype_data <- unique_rides %>% group_by(rideable_type) %>% summarise(count = n_distinct(ride_id))

-- To view the dataset in the  distribution

ggplot(unique_rides, aes(rideable_type, fill=rideable_type)) +
  geom_bar() +
  labs(x="Different types of rides", title="Chart 02 - Distribution per Rideable Types")


![Cyclistic Ridetypes](https://user-images.githubusercontent.com/112668327/204117196-c8b540d9-c03d-4b52-b929-dd9227ff0b59.jpeg)



It can be seen that:
1. Classic bikes have the highest volume of rides(3,217,737 ride count) closely followed by Electric bikes(with 2,368,592 ride count) while Docked bikes have the smallest volume of rides(with 274,447 ride count). This can be either because the company have more classic bikes than other bikes or it is a clear indication of riders’ preference.

- Guiding questions:
1. How should you organize your data to perform analysis on it?
   The data has been organized into a single CSV by appending all the files from the dataset into a single dataframe.
3. Has your data been properly formatted?
   Yes, all the columns have their correct data type.
5. What surprises did you discover in the data?
   One of the main surprises is the margin of difference among the three types of bikes. 
7. What trends or relationships did you find in the data?
-- There are more members than casuals in the dataset.
-- There are more data points in the last semester of 2021.
-- Majority of the riders tend to prefer classic bikes.
9. How will these insights help answer your business questions?
   These insights help to understand riders' behaviours across days and months in the considered period in time, which can serve as a guide to devising better marketing strategies (if further investigations can be carried out) in order to build a profile for members.

- Key tasks:
1. Organize and format your data.
2. Perform calculations.
3. Aggregate your data so it’s useful and accessible.
4. Identify trends and relationships.

- Deliverables:
•	  A summary of the analysis


### Share Phase

The share phase is usually done by building a presentation as well as asking further questions making suggestions or conclusions.

- What are the major findings from the dataset?
1. Members have the biggest proportion of the dataset.
2. There's more data points at the last semester of 2021.
3. The month with the biggest count of data points was July.
4. The month with the lowest count of data points was January.
5. In all months, we have more members' rides than casual rides.

- What further questions can be asked from the dataset as well as possible reasons/suggestions?
-- The big question is; why are there more members than casual? 
   One possible answer(due to lack of complete information from the dataset) is that members have a bigger need for the bikes than casuals, especially during the cold months as can be seen from the dataset. Also, it is possible that members use the bikes for fixed and personal activities, which can be recreational or work-related.

- Guiding questions:
1. Were you able to answer the question of how annual members and casual riders use Cyclistic bikes differently?
   Yes. The dataset points to several differences between casuals and members according to days and months of the considered period in time.
3. What story does your data tell?
   The main story the data tells is that these timestamps point out that members use the bikes for routine activities, like going to work or for recreational purposes.
5. How do your findings relate to your original question?
   These insights help to understand riders' behaviours across days and months which can serve as a guide to devising better marketing strategies (if further investigations can be carried out) in order to build a profile for members and knowing how digital media could influence them.
7. Who is your audience and what is the best way to communicate with them?
   The target audience is Lily Moreno and the Cyclistic Marketing Analytics team. The best way to communicate is through a slide presentation of the findings.
9. Can data visualization help you share your findings?
   Yes, the core of the findings is through its visualizations.
11. Is your presentation accessible to your audience?
   Yes, the plots were made using vibrant colors and corresponding labels.

- Key tasks:
1. Determine the best way to share your findings.
2. Create effective data visualizations.
3. Present your findings.
4. Ensure your work is accessible.

- Deliverables:
1. Supporting visualizations and key findings.


### Act Phase

This phase will be carried out by the Marketing Team of the company. The main takeaways for them will be the recommendations made by the Marketing Analytics team.

- Guiding questions:
1. What is your final conclusion based on your analysis?
   Members and casual have different habits when using the bikes. The conclusion is further stated on the share phase.
3. How could your team and business apply your insights?
   The insights could be implemented when preparing a marketing campaign for turning casual into members. The marketing can have a focus on workers as a green way to get to work.
5. What next steps would you or your stakeholders take based on your findings?
   Further analysis could be done to improve the findings, besides that, the marketing team can take the main information to build a marketing campaign.
7. Is there additional data you could use to expand on your findings?
-- Mobility data.
-- Climate data.
-- More information from members and casuals such as: reasons for bike preference.

- Key tasks:
1. Create a portfolio.
2. Add the case study.
3. Practice presenting the case study to a family, friends or colleagues.

- Deliverables:
The top three recommendations based on the analysis:
1.	Build a marketing campaign focusing on how bikes help people to get to work faster by avoiding traffic while keeping the planet green and safe. The ads could be shown on professional social networks.
2.	Increase benefits for riding during cold months. Coupons and discounts could be handed out to users with high ride counts.
3.	As the bikes are also used for recreational activities, ads campaigns could be made showing people how practical and consistent the bikes can be for exercises during the week.


### CONCLUSION
The Google Analytics Professional Certificate teached me a lot about R language which  is really useful for analysing data. Though this took me more time than I expected(and not so easy like Python) but it was fun while it lasted. 
