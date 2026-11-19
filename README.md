# What Do Entry-Level Software Jobs Actually Require?

**CSC 366: Principles of Data Science | Group 02**
**Group members:** Hyder Akbar, Faisal Aman, Andrew Chung, Maycol Medina, Damon Vickers


This project looks at real job postings to answer one question for computer science students looking for their first job:

> **What skills, experience, and pay do entry-level software jobs actually require?**

**Sub-questions:**
1. Which technical skills appear most often in entry-level postings?
2. Do "entry-level" postings really ask for 2 or more years of experience?
3. How does salary vary by location and remote status?
4. Are specific skills associated with higher pay?

---

## Data Sources

| Source | What it is | How it was collected | Size |
|---|---|---|---|
| **SerpAPI (Google Jobs)** | Job listings from Google Jobs | 8 entry-level job titles across 8 locations (New York, San Francisco, Seattle, Austin, Boston, Chicago, Denver, and Remote), collected on Oct 24, 2026 in 4 batches because of SerpAPI's search limits | 1,811 listings |
| **Hacker News "Who is hiring?"** | Monthly threads where companies (mostly startups) post jobs | 24 monthly threads (Nov 2024 to Oct 2026), collected through the Algolia Hacker News API | 7,627 posts |

SerpAPI is the **main source** for entry-level questions. Hacker News is used as a **comparison group**, since most of its posts come from startups hiring experienced engineers.

### Getting the data

The data files are too large to upload to GitHub through the website, so they are stored in Google Drive:

**[Project data folder (Google Drive)](https://drive.google.com/drive/folders/1HvIqlEDyIEtb_P6CljXlcfVkqqcCvZyX?usp=sharing)**

The Drive folder contains:
- `raw/`: the original JSON files from SerpAPI and Hacker News (never modified)
- `clean/`: `serp_clean.csv` and `hn_clean.csv`
- `final/`: `jobs_combined.csv` and `jobs_features.csv` (the final dataset used for analysis)

---

## Repository Structure

```
├── README.md                          ← you are here
├── notebooks/
│   ├── README.txt                     ← run order for the notebooks
│   ├── SerpAPI_Data_Cleaning.ipynb
│   ├── HN_Data_Cleaning.ipynb
│   ├── Combine_Data.ipynb
│   ├── Feature_Extraction.ipynb
│   └── Data_Visualization.ipynb
```

The data files are stored separately in [Google Drive](https://drive.google.com/drive/folders/1HvIqlEDyIEtb_P6CljXlcfVkqqcCvZyX?usp=sharing) (see **Getting the data** above).

Every notebook is fully annotated: each step has a short explanation of what was done and why, including notes on where AI was used.

---

## Process

### Step 1: Cleaning the SerpAPI data → [`SerpAPI_Data_Cleaning.ipynb`](notebooks/SerpAPI_Data_Cleaning.ipynb)

Google Jobs stores most of its useful information (salary, job type, remote status, posting age) as a list of tags. These tags were split into their own columns. Salary text such as "63.9K–97.8K a year" or "15–18 an hour" was converted into numbers and then into yearly pay. Locations were standardized to state, using each job's own location rather than the city that was searched, since Google returned about 30% of jobs from outside the searched city. Senior titles, duplicate listings, and salary outliers were flagged.

### Step 2: Cleaning the Hacker News data → [`HN_Data_Cleaning.ipynb`](notebooks/HN_Data_Cleaning.ipynb)

Each Hacker News post is a single block of HTML text with no structured fields, so it needed more work. HTML was removed, the first line of each post was used to find the company, and keyword searches were used to find job type, remote status, seniority, and whether the job is in the US. Salaries were only taken from dollar ranges, so that funding amounts like "\$20M raised" were not mistaken for pay. Comments that were not job posts and monthly reposts of the same ad were flagged.

### Step 3: Combining the datasets → [`Combine_Data.ipynb`](notebooks/Combine_Data.ipynb)

Both cleaned files were built with the same column names, so they could be stacked into one table of 9,437 rows. A `keep_for_analysis` column marks the 6,866 unique, real job postings used in the analysis.

### Step 4: Extracting skills and requirements → [`Feature_Extraction.ipynb`](notebooks/Feature_Extraction.ipynb)

Each job description was searched for 38 skills (Python, Java, SQL, AWS, and so on), years of experience required, degree requirements (required, preferred, or not mentioned), and seniority (entry, senior, mixed, or unspecified). Additional salaries written in the job descriptions were also found. The final dataset is `jobs_features.csv`.

### Step 5: Analysis and visualization → [`Data_Visualization.ipynb`](notebooks/Data_Visualization.ipynb)

Summary statistics and charts for each question: the most and least common skills, entry-level vs. startup skills, how skill demand changed over time, years of experience required, degree requirements, salary by location, remote vs. on-site pay, skills and salary, and a comparison of the two data sources.

### Rules followed throughout

- **The raw data was never modified.** Every notebook writes to a new file.
- **Flag, don't delete.** Duplicates, senior roles, outliers, and non-job posts are marked with True/False columns instead of being removed, so every decision can be reversed. The only row removed was one Hacker News post with no text.
- **Missing values were left missing** unless there was real evidence to fill them. Salaries were never filled in with an average.

---

## Key Findings

| | Finding |
|---|---|
| **Skills** | Python (50.5%) and Java (47.9%) were by far the most common skills, each appearing in about half of entry-level postings. AI-related skills appeared in 36% of postings when grouped together. |
| **Experience** | Only about 1 in 4 entry-level postings stated a number of years. Of those, **38% asked for 2+ years** and 18% asked for 3+. |
| **Degrees** | About two-thirds of entry-level postings mentioned a degree, but only about **one-third clearly required one**. 14% accepted equivalent experience. |
| **Pay** | The median entry-level salary was **\$90,000**, ranging from \$70,000 in Illinois to \$100,000 in California. Remote and on-site jobs paid about the same. |
| **Skills and pay** | Postings mentioning Python, machine learning, or Java had higher median salaries. SQL was the only skill linked to lower pay. |
| **Startups** | Startups on Hacker News leaned toward React, TypeScript, Postgres, and AI agents. Over two years, AI agent tools grew the fastest, while React, machine learning, and AWS made up a smaller share of skills mentioned. |

## What This Means

## What This Means
For CS students looking for their first job, our findings suggest that having strong programming skills is important, especially in **Python and Java**, since these were the two most common languages in entry-level job postings. Jobs that mentioned Python also had a higher median salary ($95,000 compared to $80,000 for jobs that didn't mention it). Students should also learn tools like **Git and APIs**, since employers often look for more than just one programming language. In fact, entry-level postings mentioned a median of 5 skills per job.

Another thing we found is that **entry-level doesn't always mean no experience**. Some jobs still ask for one or two years of experience, which is why internships and personal projects can be helpful. We even found a few entry-level postings asking for 8 to 10 years of experience. This shows that students should read the actual job description instead of assuming a job requires no experience just because it's labeled entry-level.

We also found that **not every job requires a college degree**. About two-thirds of entry-level postings mentioned a degree, but only about one-third actually required one. Some employers also accepted work experience instead of a degree. **Location made a big difference in salary**, with some areas paying tens of thousands of dollars more than others. However, remote and on-site jobs had similar salaries, so we didn't find much of a pay difference between the two.

The type of company also made a difference in what skills employers wanted. Startups on Hacker News mentioned **React, TypeScript, and Postgres** more often, while entry-level jobs from Google Jobs focused more on **Java, C++, and Agile**. This suggests that students should consider what kind of company they want to work for when deciding which skills to learn. We also noticed that some languages, like PHP, MATLAB, and Kotlin, were rarely mentioned in entry-level postings, even though they are used or taught elsewhere. Just because a language is popular doesn't necessarily mean a lot of entry-level jobs are asking for it.

Lastly, **AI tools are becoming more common**, especially in startup postings. Over the two years we studied, AI agent tools more than doubled their share of skill mentions on Hacker News. While these tools are still not common requirements for entry-level jobs, they could become more important in the future. For now, students should focus on building strong programming skills while also keeping up with newer technologies like AI.

---

## Limitations

- Only about half of entry-level postings listed a salary, so salary results are based on a few hundred postings, and some states have small samples.
- Skills, requirements, and seniority were found with keyword searches, so some postings may be missed or misclassified.
- The SerpAPI data is a single snapshot from October 2026, and the search titles did not include mobile-specific roles.
- Hacker News leans toward startups and senior roles, so it is not a representative sample of all jobs.
- The results show which skills are **associated** with higher pay, not which skills **cause** it.

---

## How to Run

The notebooks were written for **Google Colab**. First, download the data from the [Google Drive folder](https://drive.google.com/drive/folders/1HvIqlEDyIEtb_P6CljXlcfVkqqcCvZyX?usp=sharing). Then run the notebooks in order (see [`notebooks/README.txt`](notebooks/README.txt)), uploading the files listed below to Colab:

| Notebook | Upload to Colab | Output |
|---|---|---|
| 1. SerpAPI Data Cleaning | `raw/SerpAPI raw data/raw_serpapi_batch1.json` to `raw_serpapi_batch4.json` | `serp_clean.csv` |
| 2. HN Data Cleaning | `raw/raw_hn_24m.json` | `hn_clean.csv` |
| 3. Combine Data | `serp_clean.csv`, `hn_clean.csv` | `jobs_combined.csv` |
| 4. Feature Extraction | `jobs_combined.csv` | `jobs_features.csv` |
| 5. Data Visualization | `jobs_features.csv` | charts and summary statistics |

Notebooks 1 and 2 can be run in either order. Every output file is also included in the Google Drive folder, so any notebook can be run on its own.

---


## AI Use

AI (Claude, by Anthropic) was used throughout this project: to plan the data collection within SerpAPI's search limits, explain pandas and regex concepts,debug code, write the regex patterns used for extraction, check output files for errors, and help format the charts. All cleaning and analysis decisions were made and approved by the group.

For full transparency, a detailed AI disclosure is also included in each notebook. Every step where AI was used is marked with an **"AI assistance"** note explaining exactly what AI did in that step.
