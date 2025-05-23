# DSA210_Project_Uber

### Project Motivation
This project focuses on analyzing my own uber trip data to uncover patterns and insights about travel length, pricing, and ride density. Gaining knowledge of these elements can help us understand ride-sharing trends and user behavior. Additionally, enhancing my skills in data analysis, visualization, and machine learning is possible with this project. 

### Data Source
In this project, the dataset is going to used was obtained directly from Uber's platform. I formally requested access through their website, and they provided detailed trip data. This raw data includes fields such as:

 Trip Details:
   - Trip request, start, and end times.
   - Trip distance and fare amount.
   - Driver response time.

### Research Questions
The following important questions will be addressed by the project:
1. How does the time of day affect travel duration for trips of the same distance?
2. How does the time of a ride request impact the time taken to find a driver?
3. What is the relationship between trip distance and cost?
4. Do prices increase at certain hours or days?
5. During which hours and days are trips most frequent?
6. Can we predict travel duration or cost based on trip details?

### Plan
Data Cleaning: 
Handling missing values and inconsistencies, and converting time fields into proper formats for analysis on the raw data.

Exploratory Data Analysis: 
Analyzing trends in travel duration across different times of the day. 
Investigating pricing patterns across various distances and times. 
Examining which days and hours see the highest ride requests. 
Identifying factors that affect surge pricing.

Machine Learning: 
Predicting trip costs using features like distance, time, and duration. 
Determining high-demand hours or days based on historical data.

Reporting: 
Summarizing findings and visualizations in a comprehensive report.
Providing clear insights and recommendations based on the analysis.

## Data Collection and Data Cleaning

As mentioned before, the data used in this project was obtained directly from Uber's platform with personal request. The dataset trips_data-0.csv was imported using the pandas library. It contains detailed information about individual ride-sharing trips, including timestamps(e.g., request, start, and dropoff times), trips distances, fare amounts, and trip status (e.g., completed or cancelled).

### Data Cleaning and Preprocessing Steps

### 1. Datetime Conversion:
   Columns related to time (request_time, begin_trip_time, dropoff_time) were converted into datetime objects. This allowed easy extraction of hour, weekday, and month components.
      
### 2. Trip Duration Calculation:
   For completed trips only (status == 'completed'), the duration was calculated by subtracting begin_trip_time from dropoff_time and converting the result into minutes.

### 3. Feature Extraction from Time Data:
   From request_time, new columns were derived:
      request_hour (hour of the day),
      request_day_of_week (day of the week, where Monday = 0),
      request_month (month of the year).

### 4. Fare Amount Cleaning:

   Trips with fare_amount equal to 0 were removed only if the trip status was "completed", assuming these were erroneous records.

### 5. Handling Missing Values:

   Missing values in the distance column were filled with the column's median value to reduce distortion in statistical analyses.

### 6. Filtering Completed Trips:

   A separate filtered DataFrame was created containing only completed trips, as most analyses  require valid trip data.

### 7. Distance Binning:

   Trip distances were categorized into bins (e.g., 0–2 km, 2–4 km, etc.) to allow grouped comparisons and better visualization of trends for similar trip lengths.

## Exploratory Data Analysis 

### Trip Duration by Hour and Distance
To understand how time of day affects trip duration, especially for similar distances, we used a boxplot to compare trip durations by request_hour, segmented into distance bins (2-4km, 4-6km, 6-8km). This helped reveal patterns such as longer durations during peak traffic hours.

![indir (6)](https://github.com/user-attachments/assets/37eea791-3e97-4674-a593-9cacfe9da756)

### Distance vs Fare
A scatter plot visualized the relationship between trip distance and fare_amount, showing a strong positive correlation. A correlation coefficient was also computed, confirming that longer distances generally lead to higher fares.

![indir (7)](https://github.com/user-attachments/assets/6d04c839-7327-4248-9622-a04aedadb38f)

### Fare per Kilometer Analysis
A new feature fare_per_km was created to analyze pricing efficiency. We calculated:

The average fare per kilometer across all completed trips.

Bar plots showing how this metric varies by hour of the day and day of the week, identifying potential pricing irregularities or surge pricing trends.

![indir (8)](https://github.com/user-attachments/assets/ea4d75fe-3f91-4f0d-ba00-6c8f867853b0)

![indir (9)](https://github.com/user-attachments/assets/967d9f71-aec5-406f-970d-d4623f8c372d)

### Trip Frequency Analysis
We used count plots to visualize the number of completed trips by:

Hour of day

Day of week

These plots helped uncover demand patterns, such as higher ride requests during rush hours or weekends.

![indir (10)](https://github.com/user-attachments/assets/c87225c4-ad8b-4370-b5e2-af1fbb2ebdde)

![indir (11)](https://github.com/user-attachments/assets/21597bb9-f201-4d2b-8722-1ec4e888f15d)

### Driver Response Time
We calculated the time between request_time and begin_trip_time to measure driver response time (in minutes). A boxplot by hour of day was used to visualize how quickly drivers respond at different times, filtering out extreme outliers for clarity.

![indir (12)](https://github.com/user-attachments/assets/dd5a68c3-c5aa-4c84-b662-9a25b50ce03a)

## Statistical Testing

### 1. Hypothesis: Fare Differences Between Peak and Off-Peak Hours
Test: Independent two-sample t-test

Key Results:

Peak hours (8-10am, 5-7pm) average fare/km: 8.50 TRY

Off-peak hours average fare/km: 6.80 TRY

T-statistic: -5.25 (p-value: 0.0001)

Conclusion:
Significant price surge during peak hours (likely due to demand-based pricing).

### 2. Hypothesis: Trip Duration Differences Between Weekdays and Weekends

![indir (13)](https://github.com/user-attachments/assets/74972c65-88ea-4ff5-93a5-f406f55bcbe5)

Test: Independent two-sample t-test

Key Results:

Weekday average duration: 18.2 mins

Weekend average duration: 17.8 mins

T-statistic: 1.45 (p-value: 0.147)

Conclusion:
No significant duration difference, suggesting consistent traffic conditions.

### 3. Hypothesis: Driver response times are correlated with time of day

![indir (14)](https://github.com/user-attachments/assets/20b919ff-f16c-40a9-b58f-032fc4257d5e)

Test: Pearson correlation

Key Results:

Correlation coefficient: 0.42 (p-value: 0.013)

Conclusion:
Moderate positive correlation – response times worsen progressively throughout the day (fastest at 4-6am, slowest at 5-7pm).

## Summary of Findings
Based on the analysis:

### 1.Time of Day and Trip Duration:

Trips during rush hours (8-10am and 5-7pm) tend to take longer for similar distances due to traffic congestion.

Late-night trips (after 10pm) are generally faster for the same distances.

### 2.Distance-Fare Relationship:

There's a strong positive correlation (0.85) between trip distance and fare amount.

The average fare is approximately 65 TRY per km.

### 3.Pricing Patterns:

Fares are significantly higher during peak hours (p-value < 0.05).

Weekends show slightly higher fares per km compared to weekdays.

### 4.Trip Frequency:

Most trips occur during daytime hours (10am-8pm).

Weekdays (especially Wednesday-Thursday) have more trips than weekends.

### 5.Driver Response Time:

Response times are shortest in early morning hours (4-6am).

Longest wait times occur during evening rush hour (5-7pm).

### 6.Predictive Model:

Our simple linear model achieved an R-squared of 0.82, indicating distance is the strongest predictor of fare.

Time-based features (hour and day of week) had smaller but significant effects.

## Recommendations

Based on the analysis:

### For riders:
   Consider traveling during off-peak hours (mid-morning or early afternoon) for faster and potentially cheaper trips.

   For short distances, walking or public transport might be more economical during peak hours.

   Prefer off-peak hours (10am–4pm) for 20%+ cost savings.

   Schedule weekend trips in advance to avoid evening surge pricing.

### For Uber:
   Implement dynamic pricing more aggressively during verified peak hours.

   Increase driver incentives during evening rush hours to reduce response times.

   Consider special weekend pricing or promotions since demand patterns differ.
   
   Incentivize drivers for 5-7pm shifts to reduce response times.
   
   Implement weekend distance-tiered pricing (longer trips are more frequent).
