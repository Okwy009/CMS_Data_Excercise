# Introduction
This project aims to analyze the Payroll Based Journal (PBJ) Daily Nurse Staffing data from the Centers for Medicaid and Medicare Services (CMS) for the first quarter of 2024 [PBJ Dataset](https://data.cms.gov/quality-of-care/payroll-based-journal-daily-nurse-staffing/data), which provides detailed insights into daily staffing levels in U.S. nursing homes, including the roles of full-time employees and contractors. As Clipboard Health primarily employs contractors, this dataset is crucial for understanding competitor staffing practices and identifying growth opportunities. 

Additionally, we will incorporate the Skilled Nursing (SN) Facility Quality Reporting Program Provider Data [SN](https://data.cms.gov/provider-data/sites/default/files/resources/241dde688cb52b42eca009b9478205fa_1723752313/Skilled_Nursing_Facility_Quality_Reporting_Program_Provider_Data_Aug2024.csv) from August 2024 to enhance our analysis, allowing us to uncover staffing trends that can inform our sales strategies and improve service delivery to our clients.

# Questions I Answered
1. Which regions have a high dependency on contracted nursing staff
2. Which nursing homes have lower staffing levels and might be prime candidates for Clipboard Health’s contract staffing solutions?
3. Which regions have high contractor staffing usage, indicating competitor presence, and how can Clipboard Health differentiate itself in these markets?
4. Does a higher dependence on contractors lead to better or worse quality scores?

# Tools I Used 
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following **Python libraries:**
- **Pandas Library:** This was used to analyze the data.
- **Matplotlib Library:** I visualized the data.
- **Seaborn Library:** Helped me create more advanced visuals.
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

# Importing Libraries
```Python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt  
```

# Data Cleanup
## 1. Cleaning PBJ Dataset
``` Python
df = pd.read_csv('C:/Users/hp/Downloads/PBJ_Daily_Nurse_Staffing_Q1_2024.csv')
df['WorkDate'] = pd.to_datetime(df['WorkDate'], format='%Y%m%d', errors='coerce')
df['Year'] = df['WorkDate'].dt.year
df['Month'] = df['WorkDate'].dt.month
df.to_csv('C:/Users/hp/Downloads/Cleaned_PBJ_Daily_Nurse_Staffing_Q1_2024.csv', index=False)
```
## 2. Cleaning SN Dataset
```Python
df_sn = pd.read_csv('C:/Users/hp/Downloads/Skilled_Nursing_Facility_Quality_Reporting_Program_Provider_Data_Aug2024.csv')
df_sn['Start Date'] = pd.to_datetime(df_sn['Start Date'], errors='coerce')
df_sn['End Date'] = pd.to_datetime(df_sn['End Date'], errors='coerce')
df_sn['Score'] = pd.to_numeric(df_sn['Score'], errors='coerce')
df_sn['Score'].fillna(df_sn['Score'].mean())
df_sn.rename(columns={'CMS Certification Number (CCN)': 'PROVNUM'}, inplace=True)
df_sn.to_csv('C:/Users/hp/Downloads/Cleaned_Skilled_Nursing_Facility_Quality_Reporting_Program_Provider_Data_Aug2024.csv', index=False)
```
# Loading Data
```Python
# Load the PBJ Dataset
df = pd.read_csv('C:/Users/hp/Downloads/PBJ_Daily_Nurse_Staffing_Q1_2024.csv')

# Load the SN Dataset
df_sn = pd.read_csv('C:/Users/hp/Downloads/Skilled_Nursing_Facility_Quality_Reporting_Program_Provider_Data_Aug2024.csv')

# Load the cleaned PBJ dataset
df_cleaned = pd.read_csv('C:/Users/hp/Downloads/Cleaned_PBJ_Daily_Nurse_Staffing_Q1_2024.csv')

# Load the cleaned sn dataset
df_sn_cleaned = pd.read_csv(
    'C:/Users/hp/Downloads/Cleaned_Skilled_Nursing_Facility_Quality_Reporting_Program_Provider_Data_Aug2024.csv'
)
```

# The Analysis
Each Jupyter notebook for this project aimed at providing recommendations to the Clipboard Health sales leadership team. Here’s how I approached each question:

## 1. Which regions depend on contracted nursing staff?

To get the states with high dependency on contracted nursing nursing staff,I calculated the time spent by both contracted and employed staff for each facility, grouping them by the state they fall under.

View my notebook with detailed steps here: [2_Identifying_Markets](https://github.com/Okwy009/CMS_Data_Excercise/blob/main/2_Analysis/2_Identifying_Markets.ipynb).

### Visualization
```Python
plt.figure(figsize=(12, 8))
sns.barplot(x='Average_Contractor_Dependency', y='State', data=high_dependency_regions_sorted, errorbar=None)
plt.axvline(x=national_average, color='red', linestyle='--', label='National Average')
plt.title('High-Dependency Regions (Contractor Dependency)')
plt.xlabel('Average Contractor Dependency')
plt.ylabel('State')
plt.legend()
plt.show()
```
### Results
![High-Dependency Regions](https://github.com/Okwy009/CMS_Data_Excercise/blob/main/2_Analysis/images/Contractor_Dependency.png)
### Insights
- **High Dependency States:** States like Vermont and New Hampshire have a higher reliance on contractors, which might indicate a flexible labor market or a shortage of full-time employees.
- **National Average:** The national average for contractor dependency is around 0.18, serving as a benchmark for comparing individual states.
- **Economic Implications:** States with higher contractor dependency may face challenges in workforce stability and benefits provision.

### Recommendation
- **Policy Adjustments:** States with high dependency should consider policies to balance contractor and full-time employment to ensure workforce stability.
- **Resource Allocation:** Allocate resources for training and development to reduce dependency on contractors and build a more stable workforce.
- **Build Relationships:** Consider partnerships with local nursing homes or health facilities in high-dependency areas to build trust and establish Clipboard Health as a preferred staffing solution.

## 2. Which nursing homes have lower staffing levels and might be prime candidates for Clipboard Health’s contract staffing solutions?

To identify nursing homes with lower staffing levels that may be prime candidates for Clipboard Health’s contract staffing solutions, I conducted a comprehensive analysis of understaffed facilities. By merging this data with Skilled Nursing (SN) dataset, I was able to filter for facilities exhibiting quality concerns. This approach allowed me to pinpoint key regions that could benefit from Clipboard Health’s services and generate actionable insights for targeted sales efforts.

View my notebook with detailed steps here: [3_Identifying_Nursing_homes](https://github.com/Okwy009/CMS_Data_Excercise/blob/main/2_Analysis/3_Identifying_Nursing_homes.ipynb).

### Visualization
```Python

plt.figure(figsize=(12, 6))
sns.barplot(data=understaffed_by_state, x='STATE', y='Understaffed_Facilities')
plt.title('Number of Understaffed Facilities by State', fontsize=16)
plt.xlabel('State', fontsize=12)
plt.ylabel('Number of Understaffed Facilities', fontsize=12)
plt.xticks(rotation=90)
plt.tight_layout()
plt.show()

```

### Results
![Number of Understaffed Facilities by State](https://github.com/Okwy009/CMS_Data_Excercise/blob/main/2_Analysis/images/Number_of_Understaffed_Facilities_by_State.png)

### Insights
- **Texas (TX)** has the highest number of understaffed facilities by a significant margin, with over 300,000 hours of understaffing. This suggests a critical shortage of staffing resources in that state compared to others.

- **Florida (FL) and Ohio (OH)** follow Texas with high numbers of understaffed facilities. These states also exhibit a large disparity between their numbers and those of lower-ranked states, indicating concentrated staffing issues in specific regions.

- There is a steep decline in the number of understaffed facilities as we move from the top 5 states to the rest of the states. This suggests that a minority of states account for the majority of staffing shortages.

- Smaller states like **New Hampshire (NH), Kansas (KS), Illinois (IL), and Delaware (DE)** have fewer understaffed facilities, but they may still experience localized challenges despite the lower overall numbers. 

### Recommendation

- **Target Staffing Solutions in Texas and Florida:** Since Texas and Florida account for the largest share of understaffed facilities, Clipboard Health should focus on ramping up its recruitment efforts in these states. Partnering with local staffing agencies and incentivizing health professionals to work in these areas could help alleviate the shortage.

- **Focus on Regional Hiring Campaigns:** Given the large disparities between states, regional hiring strategies should be customized. For instance, offering competitive packages and flexible working conditions in Texas, Florida, and Ohio could be more beneficial than applying a blanket national strategy.

- **Collaborate with Local Governments:** High-need states like Ohio and California might benefit from collaboration between Clipboard Health and local governments to create policies that attract healthcare workers to those states, such as loan forgiveness programs, housing assistance, or other incentives.

- **Evaluate the Understaffing Causes:** For the top states, it may be beneficial to investigate the root causes of understaffing. Are the issues related to facility size, funding, location, or other factors? Understanding this will allow more targeted solutions to be deployed.

## 3. Which regions have high contractor staffing usage, indicating competitor presence, and how can Clipboard Health differentiate itself in these markets?

To identify regions with a high reliance on contractor staffing, indicating potential competitor presence, I analyzed facilities with elevated contractor staffing levels, categorizing them by region. Subsequently, I examined contractor staffing trends to assess competitor activity and market dynamics in these areas.

View my notebook with detailed steps here: [4_Evaluate_Competitor_Presence](https://github.com/Okwy009/CMS_Data_Excercise/blob/main/2_Analysis/4_Evaluate_Competitor_Presence.ipynb).

### Visualization
```Python
plt.figure(figsize=(8, 8)) 
plt.pie(contractor_by_state_sorted.head(10)['Total_Contractor_Hours'], 
        labels=contractor_by_state_sorted.head(10)['STATE'], 
        autopct='%1.1f%%',  
        startangle=90, 
        colors=sns.color_palette('viridis', 10), 
        wedgeprops={'edgecolor': 'white'})
plt.title('Top 10 States by Total Contractor Staffing Hours', fontsize=14)
plt.show()
```
### Results
![Top 10 States by Total Contractor Staffing Hours](https://github.com/Okwy009/CMS_Data_Excercise/blob/main/2_Analysis/images/Top_10_States_by_Total_Contractor_Staffing_Hours.png)

### Insights

- **New York (NY)** has the highest share of contractor staffing hours at 22.5%, indicating a heavy reliance on contractors in this state compared to others. This suggests that there is a significant need for temporary staffing solutions in NY, perhaps due to high patient volumes or ongoing workforce shortages.

- **Pennsylvania (PA)** follows closely with 19.9%, further highlighting a major reliance on contractor staffing. Like New York, this could indicate long-term staffing challenges in the healthcare facilities across the state.

- **New Jersey (NJ) and Illinois (IL)** also show considerable use of contractor staffing hours, accounting for 11.3% and 8.7% respectively, further emphasizing that these states are struggling to maintain permanent staffing levels.

- **Maryland (MD)**, with 4.8%, accounts for the smallest proportion among the top 10 states. This may suggest fewer staffing issues compared to other states, but still some reliance on temporary workers.


### Recommendation

- **Focus on Permanent Staffing Solutions in NY and PA:** Given that New York and Pennsylvania account for more than 40% of the total contractor staffing hours, there is a clear need for permanent staffing solutions. Clipboard Health could partner with healthcare facilities in these states to create more attractive offers for full-time workers, such as competitive pay, flexible hours, or career development opportunities, to reduce the reliance on contractors.

- **Evaluate the Factors Leading to High Contractor Usage in NY and PA:** Understanding the root causes of high contractor utilization in these two states could be key to developing long-term solutions. Whether the problem is driven by high patient volumes, an inability to retain staff, or geographic factors, identifying these challenges can inform the development of targeted programs to mitigate them.

- **Optimize Resource Allocation:** Since New York and Pennsylvania consume the largest share of contractor hours, focusing recruitment efforts, marketing, and workforce optimization in these states would yield the greatest returns for Clipboard Health in terms of addressing gaps in healthcare staffing.

- **Monitor States with Moderate Reliance on Contractors:** States like Ohio, California, and North Carolina, while not topping the chart, still require moderate levels of temporary staffing support. Clipboard Health could consider implementing workforce analytics to monitor trends in these states and anticipate changes in demand.

## 4. Does a higher dependence on contractors lead to better or worse quality scores?

To determine if higher contractor dependence impacts quality scores, a contractor usage metric is created by calculating the proportion of contractor hours relative to total staff hours. This metric is then correlated with CMS quality scores, which reflect the overall performance of nursing homes. By analyzing the correlation, we can assess whether greater contractor reliance leads to better or worse care quality. This analysis provides data-driven insights that can help Clipboard Health advise facilities on optimizing their staffing balance to improve care outcomes.

View my notebook with detailed steps here: [5_Contractor_Dependency](https://github.com/Okwy009/CMS_Data_Excercise/blob/main/2_Analysis/5_Contractor_Dependency.ipynb).

### Visualization
```Python
plt.figure(figsize=(10, 6))
sns.histplot(merged_df['Contractor_Usage_Percentage'], bins=30, kde=True, color='purple')
plt.title('Distribution of Contractor Usage Percentage')
plt.xlabel('Contractor Usage Percentage')
plt.ylabel('Frequency')
plt.tight_layout()
plt.show()
```

### Results
![Distribution of Contractor Usage Percentage](https://github.com/Okwy009/CMS_Data_Excercise/blob/main/2_Analysis/images/Distribution_of_Contractor_Usage_Percentage.png)

### Insights
- **High Concentration of Low Usage:** The histogram shows a significant number of contractors with very low usage percentages.
- **Sharp Decrease:** There is a steep drop-off in frequency as contractor usage percentage increases, indicating fewer contractors are highly utilized.
- **Underutilization:** The data suggests that many contractors are underutilized, which could imply inefficiencies in resource allocation.

### Recommendation

- **Investigate Underutilization:** Look into why many contractors have low usage percentages and identify opportunities for better resource allocation.
- **Increase Engagement:** Develop strategies to boost engagement and utilization among underutilized contractors.
-  **Analyze High Performers:** Study the factors contributing to high usage rates among the few heavily utilized contractors and apply these insights more broadly.

# Challenges
This project was not without its challenges, but it provided good learning opportunities:
- **Handling Memory Error:** Due to the size of the dataset, performing the merge was quite tricky as the memory required was more than available on my pc. 
To handle this error, I had to merge in chunks, merging only neccessary column which were relevant for my analysis.

- **Complex Data Visualization:** Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.

- **Balancing Breadth and Depth:** Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
Conclusion
In this analysis of the Payroll-Based Journal (PBJ) dataset, we explored various aspects of nurse staffing in nursing homes to provide actionable insights for Clipboard Health. These insights will help Clipboard Health capitalize on current staffing trends in the industry and strategically expand their contractor network to better meet the needs of nursing facilities.