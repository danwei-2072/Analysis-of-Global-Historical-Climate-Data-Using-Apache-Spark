# Analysis of Global Historical Climate Data Using Apache Spark (Based on Scalable Data Science Course Project)

## Project Overview

This project, based on a project for the Scalable Data Science course, aims to conduct an in-depth analysis of the Global Historical Climatology Network (GHCN) dataset using Apache Spark (via PySpark). The project focuses on exploring the active periods of global weather stations, daily climate variables, and trends in average temperature changes and climate variations across different regions. The analysis seeks to uncover long-term climate patterns, providing data support for climate change research and policy formulation.

Notably, this project achieved an A grade.

## Data Source

*   **Dataset**: Global Historical Climatology Network (GHCN) - Daily dataset (ghcnd_daily)
*   **Original Path (Example)**: `hdfs:///data/ghcnd/` on HDFS (read-only)
*   **Structure**:
    *   `daily/`: Contains `.csv.gz` compressed files named by year (e.g., `2023.csv.gz`), covering data from 1750 to 2024. Total size approx. 99.27 GB (compressed approx. 12.41 GB).
    *   `ghcnd-stations.txt`: Weather station metadata (ID, latitude/longitude, elevation, name, etc.)
    *   `ghcnd-countries.txt`: Country codes and names
    *   `ghcnd-states.txt`: US state codes and names
    *   `ghcnd-inventory.txt`: Element inventory for each station with data start/end years
*   **Key Variables**: Max/min temperature (TMAX, TMIN), total precipitation (PRCP), snowfall (SNOW), snow depth (SNWD), average temperature (TAVG), etc.

## Technology Stack

*   **Big Data Processing**: Apache Spark (PySpark)
*   **Distributed File System**: HDFS (for data storage)
*   **Programming Language**: Python
*   **Data Analysis/Visualization Libraries**: Pandas, Matplotlib (*Report mentioned attempting to use Geopandas*)

## Repository Files

*   `code_Analysis of Global Historical Climate Data Using Apache Spark.ipynb`: Jupyter Notebook source file containing all analysis code, comments, and visualization logic.
*   `Comprehensive Analysis of Global Historical Climate Data Using Apache Spark.pdf`: Detailed project report including background, methodology, results, and conclusions.
*   `README.md`: This readme file.

## Methodology and Steps

1.  **Environment Setup**: Start a PySpark session, configuring executors, cores, and memory.
2.  **Schema Definition**: Define a precise Spark SQL Schema (`StructType`) for the daily climate data.
3.  **Metadata Processing and Integration**:
    *   Read and parse fixed-width metadata text files using functions like `substring`.
    *   Merge station, country, and state information using `join` operations.
    *   Analyze `inventory` data using `groupBy` and `agg` (min, max, countDistinct) to determine station active periods and element types collected.
    *   Save the processed and enriched station information in Parquet format for optimized subsequent queries (Example path: `hdfs:///user/dwe55/outputs/stations`).
4.  **Data Exploration and Analysis (Spark DataFrame Operations)**:
    *   **Station Exploration**: Count total stations, active stations, stations by network (GSN, HCN, CRN), by country/state, by hemisphere, etc. Calculate geographical distances between stations (Example: New Zealand) using a Haversine formula UDF.
    *   **Daily Climate Summary Exploration**: Analyze HDFS block size vs. parallelism, count observation records for specific years or ranges, analyze core element distribution, check TMIN/TMAX data completeness, plot temperature time series (Example: New Zealand) after potentially caching the relevant DataFrame.
    *   **Precipitation Exploration**: Use Window functions to find the country with the highest average annual rainfall. Calculate and save average rainfall per country for a specific year (e.g., 2023), attempting local visualization.
5.  **Results Discussion**: Analyze findings and discuss insights.

## Key Findings and Impacts

*   Revealed the historical depth and spatial distribution characteristics of weather station observations.
*   Calculated distances between weather stations within New Zealand.
*   Observed a long-term upward trend in global temperatures (using New Zealand as an example).
*   Identified countries with the highest average rainfall in specific years.
*   Provided data support for understanding global climate change and formulating related policies, particularly concerning water resource distribution.

## Challenges and Limitations

*   Large data volume requires efficient processing.
*   Mixed data formats (compressed CSV, fixed-width text) add complexity.
*   Processing large compressed files (like gzip) can limit Spark's parallelism.
*   Data accuracy and completeness can be concerns due to diverse sources.
*   Installing specific Python libraries (e.g., `geopandas`) might be restricted in certain cluster environments.

## Future Work

*   Expand the data range (more years, more stations).
*   Apply more advanced analysis techniques (e.g., machine learning).

## How to Run

1.  **Environment**: Requires access to the data on HDFS (or a similar distributed file system) and an environment capable of running PySpark.
2.  **Clone Repository**:
    ```bash
    git clone https://github.com/danwei-2072/Analysis-of-Global-Historical-Climate-Data-Using-Apache-Spark
    cd Analysis-of-Global-Historical-Climate-Data-Using-Apache-Spark
    ```
3.  **Dependencies**: Ensure `pyspark`, `pandas`, and `matplotlib` are installed. `geopandas` (and its dependencies) is needed for local map plotting if desired.
4.  **Start Spark**: Initiate a PySpark session using the provided `start_spark()` function or a method suitable for your environment.
5.  **Execute Notebook**: Run the cells in the `.ipynb` file sequentially. **Note:** Input/output paths may need to be adjusted for your environment.
6.  **Output**: Results are typically printed within the notebook or saved to specified HDFS paths.
7.  **Stop Spark**: Close the Spark session using the `stop_spark()` function or the appropriate method.

## Acknowledgements

Thanks to Lecturer James, Tutors Hazel and Yiwen, and classmates from the Scalable Data Science course for their guidance and insights. Also, thanks to OpenAI for providing ChatGPT assistance.

## References

*   Intergovernmental Panel on Climate Change. (2021). Climate Change 2021: The Physical Science Basis.
*   National Research Council. (2010). Advancing the Science of Climate Change. Washington, DC: The National Academies Press.
