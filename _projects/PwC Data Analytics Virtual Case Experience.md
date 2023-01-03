---
title: PwC Data Analytics Virtual Case Experience
image:/assets/images/PwC-Switzerland-and-ImmuniWeb-are-teaming-up.jpg
description: The virtual case is about an inquiry from a big telecom company that needs to know the customer trend. The participant must create a dashboard in Power BI for the client that reflects all relevant Key Performance Indicators (KPIs) and metrics in the dataset.
company:  PwC Switzerland
date: 2022-12-21
layout: post
---

The project originated from PwC’s vision to further provide a ‘career pivot’ for upskilling employees, making them what we call ‘digital accelerators’. The program is specifically designed to rapidly deepen employees’ skills in digital specialities, such as data, automation, artificial intelligence, and digital storytelling, by learning a variety of self-service tools and coding languages and applying those skills to day-to-day business.


## Overview of the dataset

The case study dataset is an xlsx file called “Call Centre Database” and can be downloaded directly and credited by PwC Switzerland. The dataset consists of 10 columns and 5001 rows with basic records of the call centre's daily operation.
The dataset has been updated since I finished the work, and made it easier for the further participant to analyze without several data cleaning steps displayed below.

![image](https://user-images.githubusercontent.com/120835197/209665345-865871ce-52b8-4bb8-bec5-371fe51427e7.png)


## Data Preparation
Some cleaning should be done before visualizing the data while reviewing the database. Therefore, the dataset has been transformed to the power query editor first.

Replace the value: There are some blank units in the database, so I decided to replace them with the value 0 further to facilitate the analysis for average talk duration and Satisfaction rating.

![image](https://user-images.githubusercontent.com/120835197/209665471-e9a481cc-36c9-4a6b-9fb0-37233a105763.png)

Change the data type: Due to all the calling records duration happening for a particular day, there is no need to keep the date and time in the column, so the data type has been changed with the function to change the data type only to show the time of the duration. Similar jobs have been done for the time column.

![image](https://user-images.githubusercontent.com/120835197/209665508-e5c7e10b-2678-4053-b988-a43cd53e6247.png "Change the datatype for average talk duration")


![image](https://user-images.githubusercontent.com/120835197/209666697-484b9dcf-1a8a-4698-bc1f-1b969d6e0f49.png "Change the data type for time")

A condition column has also been added to indicate the satisfaction rating in a more understandable range.

```
Table.AddColumn(#"Changed Type2", "Job Satisfaction ", each if [Satisfaction rating] >= 4 then "high" else if [Satisfaction rating] = 3 then "medium" else "low")
```

![image](https://user-images.githubusercontent.com/120835197/209668776-b4dc76d5-f329-4101-942c-47e21cdf6835.png "Add a new condition column")

## Data Analysis
Then I created some DAX formulas as new measures to get deep insight and better define the KPIs that were requested in the task.

```
#To calculate the average speed of answer in seconds
Average of Speed of answer in seconds total for Speed of answer in seconds = 
CALCULATE(
 AVERAGE('Sheet1'[Speed of answer in seconds]),
 ALLSELECTED('Sheet1'[Speed of answer in seconds])
)

#To count the overall satisfaction rating and total call
Count of Satisfaction Rating = COUNT(Sheet1[Satisfaction rating])
Total calls = count (Sheet1[Call Id])

#To get the percentage of the overall satisfaction rating
Overall Customer Satisfaction = DIVIDE([Positive Satisfaction Rating], [Count of Satisfaction Rating], 0)

#To see how many satisfaction Rating is positive
Positive Satisfaction Rating = CALCULATE(COUNT(Sheet1[Satisfaction rating]),FILTER('Sheet1',Sheet1[Satisfaction rating] IN {4,5}))

#To get how many call inquiries were resolved
Resolved Calls = COUNTX(FILTER('Sheet1',Sheet1[Resolved] ="Yes"), Sheet1[Resolved])

#To see how many calls are answered
Total Calls Answered = COUNTX(FILTER('Sheet1',Sheet1[Answered (Y/N)] ="Yes"), Sheet1[Answered (Y/N)])
```
## Data Visualization
A dashboard has been built at these steps to meet the stakeholders' inquiries by visualizing those KPIs. The first step was creating Key performance indicators with the “Card” function at the head of the dashboard and using some filters to help display specific performances for certain topics or agents.

![image](https://user-images.githubusercontent.com/120835197/209669855-ae248c10-61d5-48be-91d0-fb3cbbbaa9c2.png "Key performance indicators")

![image](https://user-images.githubusercontent.com/120835197/209669918-31b7dd59-0fba-4d70-ae33-d33b4e4b2516.png "Filters for certain topics or agent")

Then four main charts are created to show some overall trends in the daily operation of the call centre, including the total speed of answers, calls relevant to different topics, ranks of satisfaction ratings, and performance matrix of different agents.

![image](https://user-images.githubusercontent.com/120835197/209669973-1b0d6f8c-ed5d-4c87-921d-4d1ea4096399.png "Line chart")

![image](https://user-images.githubusercontent.com/120835197/209670002-bf8fea6a-08a9-4bf9-a81d-1c953ba53a85.png "Donut chart")

![image](https://user-images.githubusercontent.com/120835197/209670100-36688dcf-e3fb-4308-9c72-d35ffcc0e867.png "Bar chart")

![image](https://user-images.githubusercontent.com/120835197/209670159-0a2803ba-55c2-4fbc-ae7b-982ad52815ca.png "Matrix chart")

## Summary
![image](https://user-images.githubusercontent.com/120835197/209670293-588502c9-84e3-46f2-b406-dc63de2fd756.png "The full view of the dashboard")

Insights can be retrieved from the [dashboard](Call center trend.pbix). For example, most calls were made regarding streaming because more people had problems with it. Becky received the highest rating, and Stewart had the lowest customer satisfaction, which might be explained by the fact that he took his time returning calls.

![image](https://user-images.githubusercontent.com/120835197/209670835-440a9d1c-4a0f-40e5-80c4-fdc84752d90d.png)

## Serval strategies for improvement
* The call centre should do more research on the low satisfaction rating calls to define the problem.

* Agent training should be taken as the target to improve pick-up efficiencies and customer services (especially the main knowledge about Streaming and technical support)

* The call centre can add AI menus to the hotline to increase efficiency and ensure that questions are answered correctly.

