# NYC Collision Analysis

## Table of Contents

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools Used](#tools-used)
- [Data Cleaning](#data-cleaning)
- [Explorartory Data Analysis](#explorartory-data-analysis)
- [Data Analysis](#data-analysis)
- [Data Visualization](#data-visualization)
- [Findings](#findings)
- [Recommendations](#recommendations)

### Project Overview

The NYC Collision Analysis is a data-driven project that investigates motor vehicle collisions reported across New York City between January and August 2020. Built as a capstone project during the Digitaley Drive Data Analyst Bootcamp, the aim of this analysis is to identify patterns in collision frequency, severity, contributing factors.
This end-to-end project leverages Excel for data cleaning and exploration, SQL for data transformation and querying, and Power BI for designing an interactive and insightful dashboard.

### Data Source

The dataset used for this project was provided as part of the Digitaley Drive Data Analyst Bootcamp capstone series. It contains records of all reported motor vehicle collisions in New York City from January to August 2020, based on official reports from the New York City Police Department.

### Tools Used

- Excel - Data Cleaning
- SQL Server - Data Analysis
- Power BI - Creating reports

### Data Cleaning

The dataset was carefully cleaned and preprocessed using Excel to ensure accuracy and consistency before proceeding to the analysis in SQL and visualization in Power BI. The following key steps summarize the data cleaning process:

1. Duplicate Check:
- No duplicate records were found in the dataset.
2. Missing Values Handling:
- Borough: 6,705 missing entries were replaced with "Unknown".
- Street Name: 346 missing entries were replaced with "Unknown".
- Contributing Factor: 1,179 blanks were replaced with "Unspecified".
- Persons Injured: This column contained binary values (0 or 1), with one missing value which was filled with 0.
3. Time Conversion and Additional Columns:
- The Time column was converted to 24-hour format for consistency.
- An Hour column was extracted from the formatted time.
- A new column named Time of Day was created based on the hour, categorizing collisions into Morning, Afternoon, Evening, and Night for better time-based analysis.
4. Data Structuring:
- From the Date column, two new columns — Day and Month were extracted to facilitate trend analysis over time.

### Exploratory Data Analysis

- Which Borough had the highest collision?
- What time of the day do most collisions occur?
- Are there patterns by day of week or month?
- What are the top contributing factors?
- What vehicle types are the most involved in collisions?
- Which streets have the most incidents?

### Data Analysis

The cleaned dataset was imported into MS SQL environment where exploratory analysis was performed to uncover trends and patterns in collision data. Below are the key questions explored and the SQL queries used to answer them:
1. % of total collision by month
```sql
SELECT 
    DATENAME(MONTH, date) AS month,
    COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Nyc_collisions WHERE date IS NOT NULL) AS percent_of_total_collision
FROM Nyc_collisions
WHERE date IS NOT NULL
GROUP BY DATENAME(MONTH, date), MONTH(date)
ORDER BY MONTH(date);
```
2. When do collisions occur mostly?
```sql
SELECT 
    DATENAME(DAY, date) AS day_of_week,
    DATEPART(HOUR, CAST(time AS TIME)) AS hour_of_day,
    COUNT(*) AS collision_count
FROM Nyc_collisions
WHERE date IS NOT NULL AND time IS NOT NULL
GROUP BY DATENAME(DAY, date), DATEPART(HOUR, CAST(time AS TIME))
ORDER BY collision_count DESC;
```
3. What street have the most collisions?
```sql
SELECT TOP 1
    street_name,
    COUNT(*) AS total_collisions,
    COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Nyc_collisions WHERE street_name IS NOT NULL) AS percent_of_all_collisions
FROM Nyc_collisions
WHERE street_name IS NOT NULL
GROUP BY street_name
ORDER BY total_collisions DESC;
```
4. Fatal collisions by borough
```sql
SELECT 
    borough,
    COUNT(*) AS fatal_collisions
FROM Nyc_collisions
WHERE persons_killed > 0
GROUP BY borough
ORDER BY fatal_collisions DESC;
```
5. Collisions with most injuries
```sql
SELECT TOP 5
    collision_id,
    persons_injured,
    street_name,
    date
FROM Nyc_collisions
ORDER BY persons_injured DESC;
```
6. Most common contributing factor for collision
```sql
SELECT TOP 5
    contributing_factor,
    COUNT(*) AS total_collisions
FROM nyc_collisions
WHERE contributing_factor IS NOT NULL 
  AND contributing_factor <> ''
GROUP BY contributing_factor
ORDER BY total_collisions DESC;
```
7. Collisions by vehicle type
```sql
SELECT TOP 5
    vehicle_type AS vehicle_type,
    COUNT(*) AS total_collisions
FROM Nyc_collisions
WHERE vehicle_type IS NOT NULL AND vehicle_type <> ''
GROUP BY vehicle_type
ORDER BY total_collisions DESC;
```

### Data Visualization

In this project, I created the NYC Collision Analysis Dashboard using Power BI to analyze traffic collision data in New York City. The dashboard offers a comprehensive overview of the dataset, highlighting key visualizations to uncover patterns, trends, and correlations in traffic collisions.


![Nyc collision](https://github.com/user-attachments/assets/77e59ec1-79ed-4c10-8744-79d1c0bd107c)

### Findings

- January recorded the highest percentage of total collisions, followed by June, October, and July, while February had the lowest, indicating potential seasonal or weather-related patterns influencing collision rates.
- Fridays accounted for the highest number of collisions, closely followed by Thursdays, suggesting increased traffic risk toward the end of the workweek.
- Belt Parkway experienced the highest number of total collisions among all streets, followed by Broadway and Atlantic Avenue, pointing to these as potential high-risk roadways.
- Brooklyn had the highest total collision count among all boroughs, followed by Queens, Bronx, Manhattan, Staten Island, and locations with unknown borough information.
- The street with the highest number of injury-related collisions was Hutchinson River Parkway Ramp (40 collisions), followed by Broadway (21), Broad Street (18), Bedford Avenue (17), and Belt Parkway (16), highlighting specific zones with elevated safety concerns.
- The top contributing factors to collisions were Unspecified, Driver Inattention/Distraction, Failure to Yield Right of Way, Following Too Closely, and Improper Lane Usage—underscoring a strong behavioral component in traffic incidents.
- Private vehicles were involved in the most collisions, followed by commercial vehicles, two-wheelers, utility vehicles, and emergency vehicles, indicating the need for targeted interventions across different vehicle categories.

### Recommendations

- Implement Targeted Safety Campaigns During High Risk Periods - Focus awareness and enforcement efforts on Fridays and during high-collision months like January and June, when incident rates are highest. These periods may benefit from increased traffic patrols and public education campaigns.
- Enhance Traffic Monitoring and Control on High-Risk Streets - Streets such as Belt Parkway, Broadway, and Atlantic Avenue should be prioritized for infrastructure reviews, speed control measures, and smart traffic monitoring systems to reduce accident frequency.
- Deploy Road Safety Interventions in High Injury Zones - Injury prone areas like Hutchinson River Parkway Ramp and Broadway require immediate attention. Installing better signage, improving lighting, and reconfiguring traffic flow could help mitigate injury severity.
- Address Driver Behavior Through Technology and Training - Since inattention and failure to yield are leading contributing factors, initiatives like mandatory driver re-education, in-vehicle alert systems, and mobile phone usage restrictions can reduce behavior-related collisions.
- Strengthen Regulations for Private and Commercial Vehicles - With private and commercial vehicles accounting for the majority of collisions, tailored safety inspections, telematics-based monitoring, and stricter licensing regulations should be introduced to improve accountability and compliance.



