# AWS-Youtube-Analytics  
Build an end-to-end Data Pipeline in AWS  

## Table of Contents  
1. [Project Overview](#project-overview)  
2. [Architecture](#architecture)  
3. [Services Used](#services-used)  
4. [Data Ingestion](#data-ingestion)  
5. [ETL System](#etl-system)  
6. [Data Lake](#data-lake)  
7. [Scalability Considerations](#scalability-considerations)  
8. [Cloud Usage](#cloud-usage)  
9. [Reporting](#reporting)  
10. [Dataset](#dataset)  
11. [Folder Structure](#folder-structure)  
12. [Final Visualization & Insights](#final-visualization--insights)  

---

## Project Overview

This repository houses a **Data Engineering YouTube Analysis Project** that aims to securely manage, streamline, and analyze YouTube videos data. The data is both structured and semi-structured and is enriched by pulling in video categories, trending metrics, and other metadata. By building a robust architecture on AWS, this project ensures that data ingestion, transformation, and analytics are all handled efficiently at scale.

### Key Objectives

- **Data Ingestion** — Build a mechanism to ingest data from diverse sources, specifically multiple CSV files containing YouTube trending video details.  
- **ETL System** — Transform raw CSV/JSON data to a more usable format, ensuring data cleaning and enhancement.  
- **Data Lake** — Store data from multiple sources in a centralized repository.  
- **Scalability** — Ensure that the system can handle growing data volumes efficiently.  
- **Cloud Deployment** — Utilize AWS services to handle large-scale data ingestion and analysis.  
- **Reporting** — Create an intuitive BI dashboard for data exploration and insights.  

---

## Architecture

The high-level architecture outlines how data flows through the system:

![architecture](https://github.com/user-attachments/assets/f81662d6-ae64-4673-9931-d56905026bf3)

### Components Overview

1. **Source Systems**: Raw CSV and JSON files containing trending YouTube data.  
2. **Data Platform**: An AWS S3 bucket designated for **bulk ingestion**.  
3. **Data Lake**: A structured, multi-tiered S3-based data lake (Landing, Cleansed, Enriched zones).  
4. **Data Processing**: AWS Glue jobs to crawl, catalog, and transform the data.  
5. **Analytical Data Access**: Leveraging AWS Lambda for small, on-demand data transformations and AWS Athena for serverless SQL queries.  
6. **Data Catalogue & Classification**: AWS Glue Data Catalog to store metadata about tables and data schema.  
7. **Reporting**: Amazon QuickSight for building interactive dashboards and visualizations.  

+------------------+
|  Source Systems  |
|------------------|
|  - CSV Files     |
|  - JSON Files    |
+--------+---------+
         |
         v
+--------------------------+
|      AWS S3 Bucket       |
|   (Bulk Ingestion Zone)  |
+--------------------------+
         |
         v
+--------------------------+
|     Data Lake - S3       |
|--------------------------|
| 1. Landing Zone          |
| 2. Cleansed Zone         |
| 3. Enriched Zone         |
+--------------------------+
         |
         v
+--------------------------+
|      AWS Glue Jobs       |
|--------------------------|
| - Crawling               |
| - Cataloging             |
| - Data Transformation    |
+-----------+--------------+
            |
            v
+-------------------------------+
|    AWS Glue Data Catalog      |
|-------------------------------|
| - Stores table metadata       |
| - Schema definitions          |
+-------------------------------+
            |
            v
+----------------------+      +-----------------------------+
|    AWS Lambda        | ---> | AWS Athena (SQL Queries)    |
|  (On-demand compute) |      +-----------------------------+
+----------------------+                  |
                                          v
                         +------------------------------+
                         |   Amazon QuickSight           |
                         |------------------------------|
                         | - Dashboards & Visuals        |
                         | - Reports for insights        |
                         +------------------------------+


---

## Services Used

- **Amazon S3**: Centralized object storage for raw and processed data.  
- **AWS IAM**: Secure identity and access management for services and users.  
- **AWS Glue**: Serverless ETL service used for transformation and metadata cataloging.  
- **AWS Lambda**: Event-driven compute service for lightweight data processing.  
- **AWS Athena**: Serverless query engine that queries data directly in S3 using SQL.  
- **Amazon QuickSight**: Business intelligence service for creating and publishing dashboards.  

---

## Data Ingestion

- **Bulk Data Load**: Upload Kaggle YouTube trending CSV/JSON files into S3 Landing zone.  
- **Access Control**: IAM roles/policies restrict unauthorized access and provide secure ingestion.  
- **Automation (Optional)**: AWS Step Functions or Lambda functions can automate fetching and uploading new data.

---

## ETL System

- **Crawling & Cataloging**: AWS Glue Crawler detects schema and populates the Glue Data Catalog.  
- **Transformation**: AWS Glue ETL jobs clean, transform, and move data to Cleansed/Enriched zones.  
  - Data cleaning includes:
    - Removing duplicates  
    - Filtering invalid entries  
    - Standardizing formats  
    - Type conversion  
- **Validation**: Queries in AWS Athena confirm correct transformations.

---

## Data Lake

- **Landing Zone**: Raw, unprocessed data directly from the source.  
- **Cleansed Zone**: Cleaned and validated data with minimal transformation.  
- **Enriched Zone**: Fully transformed and enriched data ready for analytics.

All zones are managed within **Amazon S3** for durability and scalability.

---

## Scalability Considerations

- **Serverless Architecture**: AWS Glue, Lambda, and Athena scale automatically with workload.  
- **Partitioning**: S3 data can be partitioned by region/date/category to optimize query performance.  
- **Cost Optimization**: Use S3 Lifecycle Policies to transition old data to cheaper storage classes (e.g., Glacier).

---

## Cloud Usage

AWS services are used for end-to-end processing due to the limitations of local machines in handling large data volumes:

- **Storage**: Amazon S3  
- **Compute & ETL**: AWS Glue, AWS Lambda  
- **Query Engine**: AWS Athena  
- **Visualization**: Amazon QuickSight  

---

## Reporting

**Amazon QuickSight** is used to build dashboards and visualize YouTube trending data.

### Dashboard Features

- **Trending Analysis**: Track top trending categories by region or date.  
- **Channel Insights**: Most viewed or engaged channels.  
- **Engagement Metrics**: Like-to-dislike ratio, comment count trends.  
- **Sentiment Analysis**: Evaluate public response to videos.  

🔗 **[View the Live Dashboard in Amazon QuickSight](https://us-east-1.quicksight.aws.amazon.com/sn/dashboards/84c7c91e-6bd6-4e42-9678-e8c3ff6fd92e)**

---

## Dataset

**Source**: [Kaggle - YouTube Trending Dataset](https://www.kaggle.com/datasets/datasnaek/youtube-new)

The dataset includes:
- Daily trending video data for multiple regions.
- Metadata such as title, channel, publish time, tags, views, likes, dislikes, and comments.
- Region-specific category mappings.

---

## Folder Structure

YouTube-Analysis-Project/
├── data/
│   ├── raw/              # Local raw files
│   └── sample/           # Sample datasets for quick testing
├── scripts/
│   ├── ingestion/        # Ingestion logic or Lambda scripts
│   ├── etl/              # Glue ETL scripts
│   └── queries/          # Athena validation queries
├── notebooks/
│   └── EDA.ipynb         # Exploratory analysis notebooks
├── dashboards/
│   └── QuickSight/       # Dashboard exports or notes
├── diagrams/
│   └── architecture.jpg  # Architecture diagram
├── README.md
└── requirements.txt      # Python dependencies (if any)

---

## Final Visualization & Insights
[Sheet_1_2025-04-16T09_43_07.pdf](https://github.com/user-attachments/files/19775015/Sheet_1_2025-04-16T09_43_07.pdf)


### 📊 Key Stats from Dashboard

- **Average Engagement Rate**: `3.38%`  
- **Likes to Dislike Ratio**: `41.9`  
- **Dislike Rate**: `12.68%`  
- **Average Views per Video**: `1.45M`  
- **Engagement Depth (Comments/1K Views)**: `4.49`  

### 🌍 Regional Viewership

- **Great Britain (GB)**: `5.36B views`  
- **United States (US)**: `2.97B views`  
- **Canada (CA)**: `1.92B views`  

### 🎬 Top Videos by Views

1. **[MV] 마마무(MAMAMOO)** — `1.07B`  
2. **Donald Trump Wins** — `1.03B`  
3. **Primitive Technology** — `0.45B`  
4. **LOGAN PAUL** — `0.31B`  
5. **Bruno Mars and Cardi B** — `0.27B`  

### 📈 Engagement Rate by Category

- Top performing categories:
  - **ID 26**: `7.3%`
  - **ID 20**: `5.05%`
  - **ID 10**: `4.03%`
- Lowest performing categories:
  - **ID 43**: `1.18%`
  - **ID 17**: `1.8%`

### 🧠 Observations

- Categories like music (26), entertainment (20), and news (10) are highly engaging.
- Regions like GB and US drive the majority of YouTube views.
- Dislike rate is notable at 12.68%, but the like-to-dislike ratio is still heavily skewed towards positive.
- Videos with higher engagement depth (comments/1K views) reflect more emotionally or socially impactful content.

---

## Conclusion

This project demonstrates the power of a cloud-native, serverless data pipeline to derive insights from semi-structured YouTube data. It leverages AWS services for ingestion, transformation, and visualization to deliver business intelligence that can help:

- Understand viewer behavior across regions and categories.
- Optimize content based on engagement metrics.
- Identify high-performing trends and viral videos.

With scalable infrastructure and automated workflows, this pipeline sets a foundation for real-time video analytics and more advanced ML-driven insights in the future.

