# Case-Study-How-Does-a-Bike-Share-Navigate-Speedy-Success?
Pawin Urapevatcharewan December 2021

Case Study: How Does a Bike-Share Navigate Speedy Success?

Introduction

   Welcome to the Cyclistic bike-share analysis case study! In this case study, you will perform many real-world tasks of a junior data analyst. You will work for a fictional company, Cyclistic, and meet different characters and team members. To answer the key business questions, you will follow the steps of the data analysis process: ask, prepare, process, analyze, share, and act. Along the way, the Case Study Roadmap tables — including guiding questions and key tasks — will help you stay on the right path. By the end of this lesson, you will have a portfolio-ready case study. Download the packet and reference the details of this case study anytime. Then, when you begin your job hunt, your case study will be a tangible way to demonstrate your knowledge and skills to potential employers.

Scenario

   You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations

What is Cyclistic
   Cyclistic is a bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day. 
● Lily Moreno, the director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.
 ● Cyclistic marketing analytics team is a team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. You joined this team six months ago and have been busy learning about Cyclistic’s mission and business goals — as well as how you, as a junior data analyst, can help Cyclistic achieve them. 
● Cyclistic executive team is the notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.

Ask Three questions will guide the future marketing program:
1. How do annual members and casual riders use Cyclistic bikes differently?
- Casual customers’ ride lengths are significantly longer.
- Both types of customer ride lengths on the weekend are slightly increased.
- Subscriber customers prefer to use the classic bike more than the casual customer.
- Only 0.3% of subscriber customers used docked bikes.
Appreciation of electric bikes of both customer types is at the same level.
2. Why would casual riders buy Cyclistic annual memberships? 
- The subscription fee is significantly low than a single ride and day pass.
 Subscriber has unlimited rides.
3. How can Cyclistic use digital media to influence casual riders to become members?
- The data that I have could not verify this matter.
 
Prepare

● Where is your data located? 
The data has been made available by Motivate International Inc. https://divvy-tripdata.s3.amazonaws.com/index.html
● How is the data organized? 
The data is a monthly report collected in CSV files 
● Are there issues with bias or credibility in this data? Does your data ROCCC?
 * Reliable: Data was collected by the first party, the company. Therefore, the information is reliable.
 * Original: The data is original.
 * Comprehensive: The data contain information explaining how users use the bike
 * Current: The last update was November 2021
 * Cited: These data sets have been used many times
● How are you addressing licensing, privacy, security, and accessibility? 
- Data was protected by https://ride.divvybikes.com/data-license-agreement
● Are there any problems with the data?
- Data was collected into 12 different files. That data size is very big. Therefore, I need to use the premium package of Rstudio to work with these data sets. 
 Many data were missing( started station, ended station, station id)
- Some of the records of start time and end time were the same. Therefore, I will affect the result of the analysis.

##The followings are all the packages used in this project 

>install.packages("tidyverse")
>install.packages("janitor")
>library(ggplot2)
>library(dplyr)
>library(skimr)
>library(janitor)
>library(readr)
# use SQL in R
>install.packages("sqldf")
>library(sqldf)

##Upload 12 file in to R public studio
>tripdata_202012 <- read_csv("Bike/202012-divvy-tripdata.csv")
>tripdata_202101 <- read_csv("Bike/202101-divvy-tripdata.csv")
>tripdata_202102 <- read_csv("Bike/202102-divvy-tripdata.csv")
>tripdata_202103 <- read_csv("Bike/202103-divvy-tripdata.csv")
>tripdata_202104 <- read_csv("Bike/202104-divvy-tripdata.csv")
>tripdata_202105 <- read_csv("Bike/202105-divvy-tripdata.csv")
>tripdata_202106 <- read_csv("Bike/202106-divvy-tripdata.csv")
>tripdata_202107 <- read_csv("Bike/202107-divvy-tripdata.csv")
>tripdata_202108 <- read_csv("Bike/202108-divvy-tripdata.csv")
>tripdata_202109 <- read_csv("Bike/202109-divvy-tripdata.csv")
>tripdata_202110 <- read_csv("Bike/202110-divvy-tripdata.csv")
>tripdata_202111 <- read_csv("Bike/202111-divvy-tripdata.csv")

## checking data 
>colnames(tripdata_202012)
>colnames(tripdata_202101)
>colnames(tripdata_202102)
>colnames(tripdata_202103)
>colnames(tripdata_202104)
>colnames(tripdata_202105)
>colnames(tripdata_202106)
>colnames(tripdata_202107)
>colnames(tripdata_202108)
>colnames(tripdata_202109)
>colnames(tripdata_202110)
>colnames(tripdata_202011)

>str(tripdata_202012)
>str(tripdata_202101)
>str(tripdata_202102)
>str(tripdata_202103)
>str(tripdata_202104)
>str(tripdata_202105)
>str(tripdata_202106)
>str(tripdata_202107)
>str(tripdata_202108)
>str(tripdata_202109)
>str(tripdata_202110)
>str(tripdata_202111)


##Merge 12 data sets into 1 file.

>tripdata <-    bind_rows(tripdata_202012,tripdata_202101,tripdata_202102,tripdata_202103,tripdata_202104,tripdata_202105,tripdata_202106,tripdata_202107,tripdata_202108,tripdata_202109,tripdata_202110,tripdata_202111)

##Calculate ride length by second

>tripdata <- tripdata %>%mutate(ride_length = ended_at - started_at)

>tripdata$ride_length <- as.numeric(as.character(tripdata$ride_length))

##The following codes is the extraction of only necessary data which is essential to the completion of this analysis
 These are column names that which are not necessary for this project.
 start_lat, start_lng, end_lat, end_lng 

##These columns have too many data missing
 start_station_name, start_station_id, end_station_name, end_station_id 

>tripdata <- tripdata %>%  
   select(-c(start_lat, start_lng, end_lat, end_lng, start_station_name, start_station_id, end_station_name, end_station_id))

##Filter records that have  ride_length = 0.

>sqldf("select count (ride_length) from tripdata_2 where ride_length == 0")

count (ride_length)
1                 493

##Create new file that ride length filtered

>tripdata_2<-filter(tripdata,ride_length>0)

##Prepare started_at for analysis

>tripdata_2$date <- as.Date(tripdata_2$started_at)
>tripdata_2$day_of_week <- format(tripdata_2$started_at,"%A")
>tripdata_2$month<-format(as.Date(tripdata_2$started_at),"%B")
>tripdata_2$year<-format(as.Date(tripdata_2$started_at),"%Y")
>tripdata_2$started_time <- format(tripdata_2$started_at,"%H:%M:%S")
>tripdata_2$ended_time <- format(tripdata_2$ended_at,"%H:%M:%S")

Analyze.

## find the mean, min, max of ride_length
>tripdata_2%>%summarise(avg_length_ride=mean(ride_length),min_ride_length=min(ride_length),max_ride_length=max(ride_length))

avg_length_ride min_ride_length max_ride_length
<drtn>          <drtn>          <drtn>         
  1 1327.833 secs   1 secs          3356649 secs

>sqldf("select count (member_casual) from tripdata_2 where member_casual == 'member'")

>sqldf("select count (member_casual) from tripdata_2 where member_casual == 'casual'")


>aggregate(tripdata_2$ride_length ~ tripdata_2$member_casual, FUN = mean)
### casual 1930.9227 secs member 825.6588 secs

>aggregate(tripdata_2$ride_length ~ tripdata_2$member_casual, FUN = max)
### casual 3356649 secs, member  93596 secs

>aggregate(tripdata_2$ride_length ~ tripdata_2$member_casual, FUN = min)
### casual 1secs, member  1 secs


##Find average of ride_length by day_of_week. 
>tripdata_2$day_of_week<-ordered(tripdata_2$day_of_week,levels=c('Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday'))

>user_summary<-aggregate(tripdata_2$ride_length~tripdata_2$member_casual+tripdata_2$day_of_week,FUN=mean)

##Find average of ride_length by month.
>tripdata_2$month<-ordered(tripdata_2$month,levels=c('December','January','February','March','April','May','June','July','August','September','October','November'))

>month_summary<-aggregate(tripdata_2$ride_length~tripdata_2$member_casual+tripdata_2$month,FUN=mean)

## count bike_type
>rideable_type_summary<- tripdata_2 %>%count(member_casual,rideable_type)
write.table(month_summary,file="month_summary.csv",sep=",")

Download tripdata_2
>write_csv(tripdata_2, path = "tripdata_2.csv")

