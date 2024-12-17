# City of Calgary Crime Analysis

This project focuses on analyzing crime data for the city of Calgary, utilizing Power BI for visualizing crime patterns and trends across different time frames, communities, and sectors. The project aims to provide insights into crime occurrence, distribution, and changes over time to support decision-making and policy formulation.

## Project Overview

The goal of this project is to analyze and visualize crime data to identify trends, patterns, and insights that can aid city officials and policymakers in understanding crime trends, allocating resources, and formulating better crime prevention policies. Key aspects of the analysis include visualizing crime distribution by sector and community, tracking crime counts over time, and identifying temporal patterns in crime occurrence.

![image](https://github.com/user-attachments/assets/06e76003-7fe0-4bb9-9f07-5e0c7692b5a2)


### Key Features

1. **Crime Trends by Month & Year**  
   Visualizes crime occurrences by month and year to track overall trends over time.  
   - Used KPIs to highlight year-over-year growth/decline percentages.
   
2. **Crime Distribution by Sector & Community**  
   Analyzes crime distribution across different sectors (North, East, South, West) and communities within Calgary.  
   - Visualized using bar charts and maps for easy exploration.

3. **Dynamic Crime Metrics**  
   Created dynamic measures using DAX for:
   - **Crime rate**, **percentage change**, and **crime per capita**.
   - Key metrics such as **Year-over-Year (YoY) Growth**, **Crime Count by Sector**, and **Crime Count by Community**.
   
4. **Time-Based Analysis**  
   Analyzes the temporal patterns of crime occurrences by grouping data by weekdays and months to identify peak crime periods.
   
5. **Enhanced Visualizations**  
   - Includes line charts, donut charts, KPI indicators, and conditional formatting to display critical metrics and highlight key insights.

## Key Measures

The following key DAX measures were created and used throughout the analysis to calculate and visualize various crime metrics:

#### 1. Total Crime Count
A dynamic measure that calculates the total number of crimes:

      Total Crime Count = SUM('Crime'[Crime Count])

#### 2. YoY Growth in Crime
Calculates year-over-year (YoY) growth in crime counts:

      YoY Growth = 
      VAR CurrentYear = MAX('Crime'[Year])
      VAR PreviousYear = CurrentYear - 1
      VAR CurrentCrimeCount = CALCULATE([Total Crime Count], 'Crime'[Year] = CurrentYear)
      VAR PreviousCrimeCount = CALCULATE([Total Crime Count], 'Crime'[Year] = PreviousYear)
      RETURN 
      IF(PreviousCrimeCount <> 0, (CurrentCrimeCount - PreviousCrimeCount) / PreviousCrimeCount, BLANK())

#### 3. Crime Count by Sector
Calculates crime count by sector (North, South, East, West):

     Crime Count by Sector = CALCULATE([Total Crime Count], 'Crime'[Sector] = "North")

#### 4. Crime Rate per Capita
Calculates the crime rate per capita (using population data from another dataset):

     Crime Rate per Capita = DIVIDE([Total Crime Count], [Total Population], 0)

#### 5. Crime Count by Month
Measures the total crime count per month:

     Crime Count by Month = CALCULATE([Total Crime Count], 'Crime'[Month] = MONTH(TODAY()))

#### 6. Crime Percentage by Sector
Calculates the percentage of total crime occurring in each sector:

     Crime Percentage by Sector = 
     DIVIDE([Crime Count by Sector], [Total Crime Count], 0)

#### 7. Crime Change from Last Year
Measures the change in crime count from the previous year:

     Crime Change from Last Year = 
     VAR CurrentYearCrime = [Total Crime Count]
     VAR LastYearCrime = CALCULATE([Total Crime Count], SAMEPERIODLASTYEAR('Crime'[Date]))
     RETURN
     IF(LastYearCrime <> 0, CurrentYearCrime - LastYearCrime, BLANK())

#### 8. Crime by Community
Measures crime counts by individual communities in Calgary:

     Crime by Community = CALCULATE([Total Crime Count], 'Crime'[Community] = "Community Name")
#### 9. Crime Metrics Selection (Dynamic Slicer)
Allows dynamic selection of metrics for reporting:

     Crime Metrics = 
     SWITCH(
     TRUE(), 
     [Selected Metric] = "Total Crime", [Total Crime Count],
     [Selected Metric] = "Crime Rate", [Crime Rate per Capita],
     [Selected Metric] = "YoY Growth", [YoY Growth],
     BLANK()
     )

## Data Description
The dataset includes the following columns:

- Crime ID: A unique identifier for each crime incident.
- Crime Count: The number of incidents in each record.
- Sector: The sector of the city where the crime occurred (North, South, East, West).
- Community: The community name where the crime occurred.
- Date: The date when the crime occurred.
- Year: The year when the crime occurred.
- Month: The month when the crime occurred.
- Population: The population data for the corresponding year (used for crime rate per capita calculation).

## Setup
**Prerequisites
- Power BI (or another compatible data visualization platform).
- Crime Data (the dataset containing information about crime incidents, including sectors, communities, dates, and population data).

## Steps to Use
-  Import the Dataset: Import the crime dataset into Power BI.
-  Create Measures: Implement the DAX measures mentioned above for calculating crime metrics.
-  Design the Dashboard: Use charts, maps, KPIs, and slicers to build the interactive crime analysis dashboard.
-  Analyze: Use the interactive filters to explore different crime metrics, trends, and patterns across time, sectors, and communities.

## Conclusion
*** This project demonstrates how to use Power BI to perform detailed crime data analysis. The DAX formulas and visualizations help track crime trends over time, identify high-crime areas, and explore temporal patterns. The dynamic dashboard allows for easy interaction and provides actionable insights for decision-makers.
