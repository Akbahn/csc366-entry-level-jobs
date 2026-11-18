CSC 366: Principles of Data Science | Group 02
Project: What Do Entry-Level Software Jobs Actually Require?

DATA SOURCES
- SerpAPI (Google Jobs): collected Oct 24, 2026
- Hacker News "Who is hiring?": Nov 2024 to Oct 2026

NOTEBOOK ORDER
Upload the input files before running each one.

1. SerpAPI_Data_Cleaning.ipynb
   Input:  raw_serpapi_batch1.json to raw_serpapi_batch4.json
   Output: serp_clean.csv

2. HN_Data_Cleaning.ipynb
   Input:  raw_hn_24m.json
   Output: hn_clean.csv

3. Combine_Data.ipynb
   Input:  serp_clean.csv, hn_clean.csv
   Output: jobs_combined.csv

4. Feature_Extraction.ipynb
   Input:  jobs_combined.csv
   Output: jobs_features.csv (final dataset used for analysis)

Notebooks 1 and 2 can be run in either order.

AI USE
Each notebook includes "AI assistance" notes
describing where and how AI was used.