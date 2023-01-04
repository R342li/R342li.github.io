---
title: HR Analytic with Power BI
image: R342li.github.io/assets/images/hr analytics.png
description: Analysis of Employee information with Data Visualization
Company: Power BI
date: 2022-12-03
layout: post
---


With the rapid innovation of technology, more innovative, more strategic, and data-supported talent decisions are within reach. HR analytics is a deep data-driven and goal-focused study of all human processes, functions, challenges and opportunities at work to improve those systems and achieve sustainable business success. This applies across the entire employee lifecycle — from making better hiring decisions to more effective performance management to better employee retention.

![image](https://user-images.githubusercontent.com/120835197/210654381-c4b20d32-f2d4-4db5-b932-811a428b8e05.png)

This case study will serve as an example of how gathering and evaluating human capital analytics can lead to better decision-making through the application of statistics and other data interpretation techniques. In the case study, the tool PowerBI will be utilized After a quick view of the data, the data will be imported into Power BI. In Power BI, the data will be organized with DAX functions and visualized in the dashboard with the help of the function "Build Visual"

The inspiration is coming from the Youtube channel [Data with Decision](https://www.youtube.com/@datalab365)and the accesss to the dataset can also be founded there.

### Overview of the dataset
The dataset of the case study is made up of two files. A quick view to see file type (CSV file), what types of data they consist of, the number of rows and columns, and if there is any "null" value...

![image](https://user-images.githubusercontent.com/120835197/210654913-6635c6b0-891d-41cf-8aac-ab9c5450cb35.png "HR Analytics data")

![image](https://user-images.githubusercontent.com/120835197/210654988-15e052ad-9be5-46ce-ad1c-24439243389d.png "Employee data")

### Data Preparation
The data will now be imported into Power BI through the "Get Data" function in Home. It is worth attention to import data with a suitable mean of data sources. In this case, it would be "text/CSV"

![image](https://user-images.githubusercontent.com/120835197/210655349-97f553f6-753d-4fc1-949c-0926fe6f54c8.png "Get data")

![image](https://user-images.githubusercontent.com/120835197/210655420-7ed20349-bd08-4844-8ce8-2019b45563d3.png "View after importing")

The data in the first file seems to be a bit messy, but the cleanup will be done directly in Power Query Editor. Under the home section, use the button "split column" and split the column by delimiter.

![image](https://user-images.githubusercontent.com/120835197/210655527-4aead6e7-1189-473f-9201-abb757cf4a47.png "Split the column by delimiter")

![image](https://user-images.githubusercontent.com/120835197/210655588-c62d6be0-adb0-4278-a32b-eb484673cb51.png "The view after splitting the column")

The data now looks well-organized However, the headers do not make any sense. Moving the first row as headers will become the next step that can be realized with the click to "Use First Row as Headers" in the "transform" section.

![image](https://user-images.githubusercontent.com/120835197/210655650-ed84b4eb-c289-4e09-9de8-f82ea52ba742.png "Use First Row as Headers")

![image](https://user-images.githubusercontent.com/120835197/210655747-d139611a-3075-41f6-9d13-981880c00065.png "Data is now organized for analyzing")

After uploading the second file with a similar process, the dataset has been created and is ready for further analysis.

#### Data Analysis
Generating insights requires more than just generating PivotTables or PivotCharts from existing data. In this case, critical data needs to be analyzed across multiple data categories, and for different ranges. DAX is a collection of functions, operators, and constants that can be used in a formula or expression to calculate and return one or more values that will be used in this phrase. Some of the measures are created and will be shown below.
_Basic information calculation (COUNTROWS, CALCULATE, DIVIDE)_
```
# total employees, number of workers by gender, gender ratio, employee rating, current active workers 
Total Emp = COUNTROWS('HR Analytics Data')
Female = CALCULATE([Total Emp],'HR Analytics Data'[Gender]="female")
Female % = DIVIDE([Female],[Total Emp],0)
High emp rating = CALCULATE([Total Emp],'HR Analytics Data'[Performance Rating]="High ratings")
Active workers = CALCULATE([Total Emp],'HR Analytics Data'[Retirement]="on service")
```
_Advanced calculation(ISBLANK, IF)_
To operate the advanced calculation, it is important to first create a new column to show who should be promoted or not. Conditional formatting will be used here. Employees who have not been promoted last ten years or above can be potentially seen as"due promotion".

![image](https://user-images.githubusercontent.com/120835197/210656008-8bfb5616-7d31-44de-a518-b13841b9e947.png "Add conditional column")

```
# number of employees due for promotion
Due for promotion = IF(ISBLANK(CALCULATE([Total Emp],'HR Analytics Data'[Promotion status]="due for promotion")),0,CALCULATE([Total Emp],'HR Analytics Data'[Promotion status]="due for promotion"))
# Use the above conditional formatting again to see the number of employees due for retirement 
Due for retirement = IF(ISBLANK( CALCULATE([Total Emp],'HR Analytics Data'[Retirement]="due for retirement")),0, CALCULATE([Total Emp],'HR Analytics Data'[Retirement]="due for retirement"))
# number of employees on services
On service = IF(ISBLANK( CALCULATE([Total Emp],'HR Analytics Data'[Retirement status]="on service")),0, CALCULATE([Total Emp],'HR Analytics Data'[Retirement]="on service"))
```
_Quick Measure (star rating)_
To better visualize the data in the next step, the function of"quick measure" could be applied to use stars to show the rating of employees' job satisfaction.

![image](https://user-images.githubusercontent.com/120835197/210656134-0e54a6e9-153a-41c5-b440-723732c04148.png "Quick method: Star rating")

![image](https://user-images.githubusercontent.com/120835197/210656174-af315a9f-f7f2-44af-aae8-eac87cb391ae.png "Effects after use")

### Data visualization
Before generating charts and tables to visualize data, it is essential to create a canvas first to ensure the dashboard is structured further.

![image](https://user-images.githubusercontent.com/120835197/210656280-83990628-01db-412f-aa5d-e0ac0b415ced.png "Create a canvas as the dashboard background£")

Afterwards, work becomes more accessible with the help of"the "build visual" function. For example, Cards are used to highlight general information, Donut charts to show the percentage of different categories, stacked column charts to compare different levels, stacked bar charts to mark descending or ascending order, and combining tables and filters to manage employees' information and their KPIs.

![image](https://user-images.githubusercontent.com/120835197/210656350-6cc72415-3a6f-4160-9928-09768324e874.png "Card used to highlight general information")

![image](https://user-images.githubusercontent.com/120835197/210656397-5409c0b8-7d6e-4867-9b10-bf251f6d0950.png "Donout charts to show the percentage of different categories")

![image](https://user-images.githubusercontent.com/120835197/210656446-c7299a84-d0d9-4740-adc6-b39a38af5dbe.png "Stacked column chart to compare different levels")

![image](https://user-images.githubusercontent.com/120835197/210656602-c4a26c61-f0b3-4d99-a241-36cbafa8b6f5.png "Stacked bar chart to mark descending or ascending order")

![image](https://user-images.githubusercontent.com/120835197/210656701-1f5ffae1-9c9e-4428-bdd6-363977251922.png "Tables and filters to manage employees' information and their KPIs")

### A full review of the dashboard
![image](https://user-images.githubusercontent.com/120835197/210656854-67496198-eff9-48e6-8e4e-7eae78b2126a.png "Overview")

![image](https://user-images.githubusercontent.com/120835197/210656928-2cf1ef3a-7fa9-44c2-9aa9-052d3951f745.png "Database")

![image](https://user-images.githubusercontent.com/120835197/210656968-aca9cd3d-ae34-4043-8add-edd4288378b1.png "Detail")

### Key Insights
Overall, the company has a higher employee job satisfaction rating. However, there is still improvement in promoting gender equity in the company especially considering the gender ratio and the situation that masculinity takes over corporate leadership positions. Also, the arrangement of promotion should be reconsidered while viewing the corresponding unequal working years and different departments.

### Improvement approaches
- Establish a comprehensive promotion plan with the participation of employee representatives from all departments and ensure the transparency of the plan.
- Provide more room for promotion and more management seats for female employees. During the recruitment process, focus on finding more potential female candidates.
- Provide subsidies to employees who are far away from the company to further improve employee satisfaction.
- Set up the company’s talent training plan to coping with subsequent retirements of current job positions.

That’s the end of the case study, thank you for reading.

References

Data with decision. (n.d.). _Data with decision_. YouTube. https://www.youtube.com/@datalab365

Vulpen, E. van. (2021). _What is HR analytics?_ AIHR. from https://www.aihr.com/blog/what-is-hr-analytics/

Minewiskan. (n.d.). _Dax Function Reference_ — Dax. DAX | Microsoft Learn. https://learn.microsoft.com/en-us/dax/dax-function-reference














