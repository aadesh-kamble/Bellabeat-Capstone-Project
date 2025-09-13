# Bellabeat Fitness Data Analysis

**Name:** Aadesh Kamble  
**Date:** June 2025  
**Tools Used:** SQL, Python, pandas, matplotlib/seaborn  

---

## 📖 Introduction
Bellabeat is a high-tech wellness company that designs health-focused smart products, primarily for women. Their product ecosystem includes the Bellabeat app, wearable trackers like Leaf and Time, the Spring smart water bottle, and subscription-based wellness memberships.  

This case study simulates the role of a junior data analyst at Bellabeat, applying the **ask → prepare → process → analyze → share → act** framework to uncover insights and generate marketing recommendations.  

---

## 🎯 Scenario
As a junior data analyst on the Bellabeat marketing analytics team, my responsibility was to explore consumer usage of smart devices. By analyzing Fitbit fitness tracker data from 30 consenting users, I aimed to identify trends in daily activity and sleep behaviors that can inform Bellabeat’s marketing strategy.  

---

## 🛠 Business Task
The analysis answers three key questions:
1. What are the current trends in smart device usage?  
2. How can these trends be applied to Bellabeat’s customer base?  
3. How can these insights inform Bellabeat’s marketing strategy?  

---

## 📂 Prepare
- Dataset: [FitBit Fitness Tracker Dataset](https://www.kaggle.com/datasets/arashnic/fitbit) (public domain, Kaggle, Mobius)  
- Contains: minute-level activity, heart rate, sleep logs, daily activity, step counts  
- Limitations:
  - Possible sampling bias (volunteer participants may be more active than average)  
  - No gender data (important since Bellabeat targets women)  
  - Data is from 2016 (somewhat outdated)  

Tools used: **PostgreSQL, Python, pandas, matplotlib, seaborn**.  
Selected 4 datasets out of 18 as most relevant.  

---

## ⚙️ Process
Key steps:
- Cleaned and formatted data for clarity  
- Added columns, extracted useful information  
- Removed nulls, duplicates, and bad data  
- Consolidated into a **PostgreSQL relational database**  

**Sample SQL (duplicates + nulls check):**
```sql
-- 1.1 Total rows
SELECT COUNT(*) AS total_rows
FROM dailyactivitymerged_part1;

-- 1.2 Null counts for specific important columns

SELECT
  COUNT(*) FILTER (WHERE TotalSteps IS NULL)    AS null_totalsteps,
  COUNT(*) FILTER (WHERE TotalDistance IS NULL) AS null_totaldistance,
  COUNT(*) FILTER (WHERE Calories IS NULL)      AS null_calories
FROM dailyactivitymerged_part1;


-- Find duplicate records based on Id and ActivityDate
SELECT Id, ActivityDate, COUNT(*) AS duplicate_count
FROM dailyactivitymerged_part1
GROUP BY Id, ActivityDate
HAVING COUNT(*) > 1;


– View duplicate rows
SELECT *
FROM dailyactivitymerged_part1
WHERE (Id, ActivityDate) IN (
    SELECT Id, ActivityDate
    FROM dailyactivitymerged_part1
    GROUP BY Id, ActivityDate
    HAVING COUNT(*) > 1
);

-- Merge part1 and part2 into a single dataset
CREATE TABLE dailyactivityall AS
SELECT * FROM dailyactivity_merged_part1
UNION ALL
SELECT * FROM dailyactivity_merged_part2;

-- Optional: check row counts
SELECT
  (SELECT COUNT(*) FROM dailyactivity_merged_part1) AS part1_rows,
  (SELECT COUNT(*) FROM dailyactivity_merged_part2) AS part2_rows,
  (SELECT COUNT(*) FROM dailyactivity_merged) AS merged_rows;
```
The scatterplot shows a clear negative correlation between sedentary minutes and hours of sleep. This means that users who spend more time being sedentary tend to get fewer hours of sleep. The trend line also confirms this inverse relationship, suggesting that prolonged inactivity throughout the day may be linked with poor sleep duration.


The scatterplot of sedentary minutes versus calories burned shows no strong linear relationship. Most users burn between 2000–3000 calories per day, regardless of how much time they spend sedentary. This suggests that calorie burn is influenced more by other factors such as physical activity levels, exercise intensity, or individual metabolism, rather than sedentary time alone.


The distribution of bedtimes shows that most users tend to go to bed between 11:00 PM and 1:00 AM, with a sharp peak around midnight. A smaller number of users go to bed much earlier or much later, but the majority fall within this narrow time range. This indicates that users generally follow a late-night sleep schedule.


The distribution of activity minutes shows that users spend the vast majority of their time being sedentary (81.9%). Only a small fraction of time is spent being lightly active (15.4%), while fairly active (1.1%) and very active (1.6%) minutes are very limited. This highlights that most users have low levels of physical activity overall.


## 📌 Conclusion

The analysis of user activity and sleep patterns reveals that most users lead a **highly sedentary lifestyle**, with limited minutes spent in very active or fairly active states.  
Bedtime behavior also shows that the majority of users follow **late-night sleep schedules**, typically between **11:00 PM and 1:00 AM**.  

Additionally, increased sedentary time appears to correlate with **reduced hours of sleep**, which may negatively impact overall health.  
Calorie burn remains relatively stable around **2000–3000 calories**, suggesting that **short bursts of activity and individual metabolism** play a stronger role in daily energy expenditure than sedentary time alone.  

---

## 💡 Business Recommendations for Bellabeat

### 1. Promote Active Lifestyle
- Introduce app-based challenges and reminders that encourage users to reduce sedentary time by incorporating **short activity breaks** (e.g., stretching, quick walks).  
- Use **gamification features** (badges, streaks, leaderboards) to motivate users to increase “fairly active” and “very active” minutes.  

### 2. Sleep Improvement Guidance
- Provide personalized recommendations for **earlier bedtimes** and reminders to start winding down at night.  
- Offer **sleep coaching content** (articles, medi
