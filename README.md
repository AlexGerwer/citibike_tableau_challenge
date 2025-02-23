# New York CitiBike Visualizations in Tableau

## Project Goal

Identify two unexpected phenomena in CitiBike usage specifically during February across multiple years, create visualizations and dashboards in Tableau to explain them, and provide actionable insights.

## Project Structure

This repository contains the data acquisition, cleaning, transformation, and visualization code and files for the CitiBike analysis project. The project is divided into two main phases:

1.  **Phase 1: Data Acquisition, Cleaning, and Preparation (Python with Pandas)** - This phase focuses on downloading, cleaning, and transforming the raw CitiBike data into a usable format for Tableau. All code for this phase is in Python.

2.  **Phase 2: Phenomenon Discovery and Visualization in Tableau** - This phase focuses on creating visualizations and dashboards in Tableau to explore the data, identify trends, and derive insights.

## Repository Contents

The repository contains the following files:

```
citibike_tableau_challenge/
├── citi_bike_data_column_map_1.pdf        <-  Column Mapping Information (PDF)
├── citi_bike_data_column_map_2.pdf        <-  Column Mapping Information (PDF)
├── citi_bike_data_extraction.ipynb         <- Data download, cleaning, and transformation (Jupyter Notebook)
├── citi_bike_data_schema_mapping_individual_years.ipynb  <- Schema mapping definition (Jupyter Notebook)
├── citi_bike_data_Visualization_Tableau.twb  <- Tableau workbook
├── DataFileSizes.pdf                       <- Report on data file sizes (PDF)
├── New York Citibike Visualizations in Tableau.pdf <- Project overview and instructions (PDF)
├── TableauVisualizations.pdf               <- Visualizations with the Tableau Project (PDF)
└── README.md                               <- This README file
```

*   **citi\_bike\_data\_column\_map\_1.pdf** and **citi\_bike\_data\_column\_map\_2.pdf:** These PDF files likely contain documentation or visualizations of the column mappings used to handle the schema changes in the CitiBike data over time. These are informational documents.
*   **citi\_bike\_data\_extraction.ipynb:** This Jupyter notebook is the core of Phase 1. It handles the automated download of CitiBike data, data cleaning, schema mapping, and transformation into a format suitable for Tableau.
*  **citi\_bike\_data\_schema\_mapping\_individual\_years.ipynb:** This Jupyter notebook defines the crucial schema mapping dictionary that's used by `citi_bike_data_extraction.ipynb` to standardize column names across different years of CitiBike data.
*   **citi\_bike\_data\_Visualization\_Tableau.twb:** This is the Tableau workbook file. It contains all the visualizations, dashboards, and stories created for Phase 2 of the project.
*   **DataFileSizes.pdf:** This PDF document provides a report on the sizes of the various data files involved in the project.  This could be useful for understanding the scale of the data.
*   **New York Citibike Visualizations in Tableau.pdf:** This PDF appears to be a document outlining the project's goals, steps, and instructions.  It acts as a high-level guide to the project.
*   **TableauVisualizations.pdf**: This pdf contains the final Tableau visualizations.
*   **README.md:**  This file (the one you are currently reading) provides a comprehensive overview of the project, its structure, dependencies, instructions, and key findings.

## Data Source

The data used in this project comes from the official CitiBike system data, available at: [https://s3.amazonaws.com/tripdata/index.html](https://s3.amazonaws.com/tripdata/index.html)

The project specifically uses data from **February** of each year from 2014 to 2023.

## Phase 1: Data Acquisition, Cleaning, and Preparation (Python)

### Focus and Goals

*   **February Only:** Isolate data from February of each available year to simplify the analysis and address seasonality.
*   **Schema Mapping:** Handle the variations in column names across different years using a schema mapping dictionary.
*   **Automated Download and Extraction:** Download and extract data automatically using Python.
*   **Data Cleaning:** Clean and transform the data, including handling missing values, formatting dates, and creating unique trip IDs.
*   **Output File:** Generate a combined CSV file containing all the necessary fields and accounting for schema variations. This is handled within the `citi_bike_data_extraction.ipynb` notebook.

### Dependencies

The Python scripts require the following libraries:

*   pandas
*   requests
*   zipfile
*   io
*   os
*   re

You can install these libraries using pip:

```bash
pip install pandas requests
```

### Usage

1.  **Run `citi_bike_data_extraction.ipynb`:** Execute the cells in this notebook sequentially. The notebook will:
    *   Download the yearly zip files from the CitiBike website.
    *   Extract the February CSV files.
    *   Handle the nested zip structure for 2020 and later.
    *   Apply the schema mapping defined in `citi_bike_data_schema_mapping_individual_years.ipynb`.
    *   Perform data cleaning and transformation.
    *   Save the cleaned and combined data to a directory that it creates called `citibike_feb_data_cleaned`.  This directory is *not* included in the repository, but is generated by the notebook.

### Key Steps in `citi_bike_data_extraction.ipynb`

*   **Combined Logic with if/elif:** The code uses an `if`/`elif` structure within the main loop to correctly handle the different file structures:
    *   `if year >= 2020:`: Handles the nested zip structure specific to 2020-2023.
    *   `elif ...:`: Handles the 2014-2019 files, extracting February CSVs directly.
*   **Robust Regular Expression:** The code uses a robust regular expression (`r"(?:/|^)(?:JC-)?(\d{4})(\d{2})[\-_\.].*\.csv"`) to correctly identify and extract February files, handling variations in filenames and separators.
*   **Nested Zip Handling:** For 2020-2023, the code correctly handles the nested zip files, opening the inner zip file and extracting the CSVs from it.
*   **Correct File Path Handling:** The code extracts only the filename part and constructs the output path to ensure files are placed directly into the `year/month` directory.
*   **Schema Mapping:** The code utilizes a `schema_mapping` dictionary to standardize column names across different years. This ensures consistency for analysis in Tableau. The mapping is defined in `citi_bike_data_schema_mapping_individual_years.ipynb`.
*   **Datetime Formatting:** The code converts `starttime` and `stoptime` (or their equivalents) to datetime objects and then formats them consistently.
*   **Global `trip_id`:** A global `trip_id` is generated to ensure unique IDs across all files.
*   **File Grouping:** Files with the same naming prefix (before the `_1`, `_2`, etc., suffixes) are grouped and concatenated, resulting in one cleaned CSV file per original raw data file.
*   **Error Handling:** `try-except` blocks are used to handle potential errors during download, extraction, and file processing.

### Schema Mapping

The `citi_bike_data_schema_mapping_individual_years.ipynb` notebook defines the `schema_mapping` dictionary, which is crucial for handling variations in column names across different years.  This ensures data consistency for analysis.  The schema mapping is loaded and used in `citi_bike_data_extraction.ipynb`. An excerpt:

```python
# Example schema mapping (full mapping is in the notebook)
schema_mapping = {
    "2014": {
        "file_type": "2014",
        "tripduration": "tripduration",
        "starttime": "starttime",
        "stoptime": "stoptime",
        "start station id": "start station id",
        "start station name": "start station name",
      ...
    },
    "JC-2015":{
        "file_type": "JC-2015",
        "tripduration": "tripduration",
        "starttime": "starttime",
        "stoptime": "stoptime",
     ...
    }
  ...
}
```

## Phase 2: Phenomenon Discovery and Visualization in Tableau

### Data Import and Preparation in Tableau

1.  **Open Tableau Desktop and the `citi_bike_data_Visualization_Tableau.twb` file.**
2.  **Connect to Data:**  The workbook should already be connected. If not, you will need to run `citi_bike_data_extraction.ipynb` to generate the necessary cleaned CSV files. The workbook uses a *union* of all CSV files in the `citibike_feb_data_cleaned` directory.  You *may* need to refresh the data connection or re-establish the union if Tableau can't find the files:
      * Go to the Data Source pane.
      * Right-click on the "All Data" data source.
      * Choose "Edit Data Source..."
      * In the Union dialog, ensure the "Wildcard (automatic)" option is selected and that the "Include" pattern is set to `*.csv`.  Make sure it's pointing to the `citibike_feb_data_cleaned` directory *created by the notebook*.
3.  **Verify Date Fields:** Ensure that `start_time` and `end_time` are recognized as Date & Time fields.

### Calculated Fields

The Tableau workbook already contains the necessary calculated fields.  These are listed here for reference and in case you need to recreate them:

*   **Day of Week (from start\_time):**
    ```tableau
    DATENAME('weekday', [start_time])
    ```
*   **Hour of Day (from start\_time):**
    ```tableau
    DATEPART('hour', [start_time])
    ```
*   **Trip Duration (in minutes):**
    ```tableau
    DATEDIFF('second', [start_time], [end_time])/ 60
    ```
*   **E-Bike Trips:** (For counting e-bike trips)
    ```tableau
    IF [rideable_type] = "electric_bike" THEN 1 ELSE 0 END
    ```
*   **E-Bike Percentage:** (For calculating the percentage of e-bike trips)
    ```tableau
    SUM([E-Bike Trips]) / SUM([Number of Records])
    ```
*   **Trip Distance (meters):** (Using the Haversine formula)
    ```tableau
    // Convert degrees to radians
    FLOAT(
    3959 * ACOS(
    COS(RADIANS([start_station_latitude])) *
    COS(RADIANS([end_station_latitude])) *
    COS(RADIANS([end_station_longitude]) -
    RADIANS([start_station_longitude])) +
    SIN(RADIANS([start_station_latitude])) *
    SIN(RADIANS([end_station_latitude]))
    )
    )
    ```

### Visualizations and Dashboards

The `citi_bike_data_Visualization_Tableau.twb` workbook contains the following visualizations and dashboards:

**Visualizations:**

1.  **Total Trips per Year by Consumer Type (Stacked Bar Chart):** Shows the total number of trips per year, broken down by user type (Subscriber/Customer).
2.  **Consumer Type and Trip Duration (Stacked Bar Chart):** Shows the total trip duration per year, broken down by user type, with a "Percent of Total" table calculation.
3.  **Bicycle Trips from Specific Start Stations (Bar Chart):** Shows the number of trips originating from the top 10 starting stations each year.
4.  **Bicycle Trips to Specific End Stations (Bar Chart):** Shows the number of trips ending at the top 10 ending stations each year.
5.  **Total Number of Bicycle Rides by Time of Day by Year (Line Graph):** Shows the number of rides per hour of the day, colored by year.
6.  **Total Number of Bicycle Rides vs Hour of Day by Starting Station (Line Graph):** Shows the number of rides per hour, colored by starting station.
7.  **Annual EBike Usage (Bar Graph / Line Graph / Dual Axis):** Shows both the number of e-bike trips (bar) and the percentage of e-bike trips (line) per year, using a dual axis.
8.  **Top Bicycle Ride Endpoints in February, All Years (Map):** A map showing the top ending stations, with size representing the number of rides.
9.  **Growth in Individual Consumer vs Subscriber Riders (Pie Chart Grid):** A grid of pie charts showing the proportion of subscriber vs. customer rides for the top stations, across different years.
10. **Change in Bicycle Type Use (Pie Chart Grid):** A grid of pie charts showing the proportion of classic vs. electric bike rides for the top stations, across different years.
11. **Number of Bicycles Rides at Various Hours on Various Days (Heat Map):** A heat map showing the number of rides by hour of day and day of the week.
12. **Start Station, End Station, and the Ride in Between (Shape Grid):** Shows the combinations of the top start and end stations, colored by trip distance and sized by trip duration.

**Dashboards:**

1.  **Bicycle Ride Characteristics:** Combines visualizations 8 and 12. Focuses on characterizing trips between top starting and ending stations, including distance and duration.
2.  **Station Popularity and Usage Dashboard:** Combines visualizations 3 and 4. Shows how top station rankings change year-to-year and allows comparison of net bike flow.
3.  **Time of Day Dependency of Bicycle Rides:** Combines visualizations 5 and 6. Characterizes how ride volume varies by day of week and hour of day.
4.  **Growth in eBike Usage:** Combines visualizations 7 and 10.
5.  **Types of Consumers:** Combines visualizations 1, 2, and 9.

**Tableau Story Book Pages:**

The Tableau story book includes five pages that present the key findings:

1.  **Bicycle Ride Usage**
2.  **Time Dependency of Bicycle Usage**
3.  **Station Utilization**
4.  **Individual vs Subscription Rides**
5.  **Growth in Electric Bicycle Usage**

Each story book page includes visualizations and concise text summaries (added as captions or text objects on dashboards) explaining the key insights.  The summaries are provided in earlier responses of this conversation, and their incorporation into the Tableau file is also described.

The corresponding Tableau Packaged Workbook file can be found here:

https://drive.google.com/file/d/1TdhcW8BvKHSTIae4aQCIghUKLScTCU0b/view?usp=sharing 

## Key Findings and Insights

The analysis revealed several key insights, summarized below (and detailed in the earlier responses with bullet points for each story):

*   **Overall Growth:** Citi Bike usage in February has significantly increased over the years, particularly from 2019 onwards, indicating growing popularity. The introduction of e-bikes significantly boosted overall ridership.
*   **Subscriber Dominance:** Subscribers (annual members) consistently make up the vast majority of rides, highlighting the importance of this user base.
*   **Growing Casual Use:** There's a slow but steady increase in the proportion of casual (Customer) rides, particularly at certain stations, suggesting potential for targeted marketing.
*   **Strong Temporal Patterns:** Ridership is highly predictable based on time of day and day of week, with clear peaks during commuting hours (8-9 AM and 5-7 PM) and higher usage on weekdays.
*   **Top Stations and Net Flow:** A small number of stations consistently dominate as both starting and ending points. Comparing starting and ending station popularity reveals the net flow of bikes across the network, which is crucial for redistribution efforts.
*   **E-Bike Adoption:** E-bikes, introduced relatively recently, have seen rapid adoption, and their usage varies significantly between stations.
* **File Combination:** All downloaded files were corrected proccessed into a single output.

## Actionable Insights

*   **Optimize Bike Redistribution:** Use the insights on top stations and net bike flow to strategically redistribute bikes from "sink" stations to "source" stations, ensuring availability during peak hours.
*   **Targeted Marketing:** Focus marketing efforts on increasing casual usage, particularly at stations with a higher proportion of customer rides. Consider time-based pricing or promotions to incentivize usage during off-peak hours.
*   **E-Bike Placement:** Consider the spatial distribution of e-bikes based on station-specific usage patterns. Place more e-bikes at stations with higher e-bike adoption rates or in areas with challenging terrain.
*   **Station Capacity Planning:** Monitor the top stations and their growth trends to anticipate future demand and plan for potential station expansions or additions.
* **Weather Considerations:** Since all of the data comes from February, weather (especially temperature) will have a strong effect on the data.

## Installation and Usage

### Downloading, Extracting, and Cleaning the Data

Execute the download and extraction script first:

```sh
python citi_bike_data_extraction.py
```

Or run the Jupyter Notebook for a step-by-step analysis:

```sh
jupyter notebook citi_bike_data_extraction.ipynb
```
Then run the cleaning script:

```sh
python citi_bike_data_schema_mapping_individual_years.py
```

Or run the Jupyter Notebook for a step-by-step analysis:

```sh
jupyter notebook citi_bike_data_schema_mapping_individual_years.ipynb
```

The resulting cleaned data can be used in Tableau to construct visualizations, following the directions in the
New York Citibike Visualizations in Tableau.pdf project summary.

You can also see the webpage output of running the repository at https://public.tableau.com/app/profile/alex.gerwer/viz/citi_bike_data_Visualization_Tableau/BicycleRideUsage 

## License
The project is released under the MIT License, which is a permissive open-source license that allows users to freely use, modify, distribute, and sublicense the software with minimal restrictions. This means that anyone can incorporate this project into their own work, whether for personal, academic, or commercial purposes, as long as the original copyright notice and license terms are included. For full details, refer to the LICENSE file in the repository.
