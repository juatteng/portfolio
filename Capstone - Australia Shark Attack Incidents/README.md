# Capstone - Australia Shark Attack Incidents

## Context and Scope

Australia is the only continent fully surrounded by water and has a booming tourist industry relating to beach and water activities, yet Australia is also the second-highest number of shark attacks in the world (the first being USA). There is an increasing trend in shark bite incidents over the years. **How can we make it safer for water activity lovers and the sharks as well?**

Objective of this study:
- To identify key factors of shark attack incidents
- To determine which model is the best choice for predicting injury risk based on key factors 

Impact of this study:
- To protect beach-goers and water activity lovers by raising awareness of the factors that may attract an shark attack incident and reduce its associated risk
- To protect sharks in their natural habitat by reducing unintended provocation by humans


## Methods/Techniques

1. Data cleaning on Excel - Remove unused columns, correct spelling mistakes, fill in blanks 
2. Data wrangling and EDA on Python - Univariate EDA, drop outlier rows, data reclassification
3. Visualization on Tableau - heat map, key trends, key insights
4. Classification predictive modelling on Python - Logistic Regression, Random Forest, XGBoost


## Files Directory

**1. Data**
- [Original Excel File from Australian Shark Incident Database](./data/Australian%20Shark-Incident%20Database%20Public%20Version%207%20(raw).xlsx)
- [Cleaned Excel File before saving to CSV file for Python Analysis](./data/Australian%20Shark-Incident%20Database%20Public%20Version%207%20(cleaned).xlsx)
- [Cleaned Data for Python Analysis](./data/aus_shark_v7.csv)
- [Cleaned Data after Python Analysis, for Tableau visualization](./data/aus_shark_v7_export.csv)

**2. Other Files**
- [Jupyter Notebook on EDA and predictive modelling](./aus_shark.ipynb)
- [Presentation Slides](./Presentation%20-%20Aus%20Shark%20Attack%20Incidents.pdf)
- [Technical Report](./Technical%20Report%20-%20Aus%20Shark%20Attack%20Incidents.pdf)
- [Tableau Visualization](https://public.tableau.com/views/AustraliaSharkAttackIncidents1900-2024/ByMonth?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link) This interactive visualization is published on Tableau Public. 


## Data Dictionary

| feature                 | type    | description                                                                            |
|-------------------------|---------|----------------------------------------------------------------------------------------|
| UIN                     | integer | unique identifier for each case                                                        |
| incident.month          | integer | month of year                                                                          |
| incident.year           | integer | year in full                                                                           |
| victim.injury           | text    | outcome of victim's health - fatal / injured / uninjured                               |
| state                   | text    | Australia state/territory; abbreviated                                                 |
| latitude                | numeric | latitude of incident                                                                   |
| longitude               | numeric | longitude of incident                                                                  |
| site.category           | text    | type of site where incident occurred                                                   |
| shark.common.name       | text    | Australian common name, by species or family group                                     |
| provoked/unprovoked     | text    | whether the person attracts or initiates physical contact with a shark                 |
| victim.activity         | text    | activity at time of incident                                                           |
| injury.location         | text    | areas on victim that are injured by shark                                              |
| injury.severity         | text    | severity of victim's injuries sustained from shark                                     |
| victim.gender           | text    | whether the victim is male or female                                                   |
| victim.age              | integer | age of victim in years                                                                 |
| hour.of.incident        | text    | time of day when incident occurred to the nearest hour in 24-hour time, or 'unknown'   |
| depth.of.incident.m     | numeric | estimated depth (metres) at which the incident took place, or '999' where unknown      |
| depth.category          | text    | depth of incident categorized by conventional scuba diving standards                   |


## Key Findings
1. Shark attack incidents peak during warmer summer months - coincides with summer activity trends. 
2. High-risk zones are New South Wales and Queensland (more than 60% of all incidents). 
3. Most shark attack incidents are unprovoked (66%).
4. Activities on water surface like swimming/surfing have significantly more incidents (>70%) than at depth like scuba diving (<10%).

For more info, check out the interactive Tableau visualization for this project [here](https://public.tableau.com/views/AustraliaSharkAttackIncidents1900-2024/ByMonth?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link). 

## Predictive Model
The dataset was put through 3 different predictive models to determine which one has the best scores - Logistic Regression (LR), Random Forest Classifier (RF) and XGBoost.

For all 3 models, additional weightage was given to the 'fatal' class type due to the small dataset available and critical nature of the class.
The ratio of the classes available in the dataset was 60-20-20 for injured, uninjured and fatal respectively, hence a class weight of 3 was allocated to 'fatal'.   

| Class Types  | Class Name  | Class Weight  |
|--------------|-------------|---------------|
| Class 0      | Uninjured   | 1             |
| Class 1      | Injured     | 1             |
| Class 2      | Fatal       | 3             |

#### Key Metrics
1. **Precision**: High precision = Few false alarms (e.g., 0.75 for "injured" means 75% of predicted "injured" cases were correct).
2. **Recall (Sensitivity)**: High recall = Few missed attacks (e.g., 0.83 for "injured" means 83% of actual "injured" cases were detected).
3. **F1-Score**: Balances precision and recall, useful for imbalanced data (e.g. 0.79 for "injured").

While accuracy score is a common approach for comparison, this study also focuses on achieving a high recall rate for Class 2 (fatality) as missing attacks has high consequences and also a decent precision rate to reduce false alarms. 

#### Choice of Predictive Model
Based on the results below, **Random Forest Classifier (Tuned)** emerged as the best choice for predicting shark attack outcomes. 
This model optimally trades off fatality detection (64% recall) with reasonable false alarms (48% precision) while maintaining high overall accuracy (67%).
Even though XGBoost has the highest Recall for Class 2, the Random Forest Classifier has a better all-round performance in all other metrics.

| Predictive Model              | Recall (Class 2) | Precision (Class 2) | F1-Score (Class 2) | F1-Score (Class 1) | Accuracy | Key Advantage             |
|-------------------------------|------------------|---------------------|--------------------|--------------------|----------|---------------------------|
| Random Forest (Tuned)         | 0.64             | **0.48**            | **0.55**           | **0.76**           | **0.67** | Best overall accuracy     |
| XGBoost (Tuned)               | **0.74**         | 0.37                | 0.49               | 0.69               | 0.60     | Best fatality detection   |
| Logistic Regression (Scaled)  | 0.32             | 0.39                | 0.35               | 0.74               | 0.63     | Worst fatality detection  |



## Conclusions and Future Steps

To conclude, the **Random Forest Classifier** performed relatively well overall to predict shark attack risks. This could potentially be deployed (with a confidence threshold) as a warning system to lifeguards or the public via social platforms like Telegram in a chatbot style i.e. to alert lifeguards if fatality probablity is more than 60%. 

This model can also be further improved by factoring environmental and human activity metrics or if possible, work with marine life experts to finetune based on shark behaviour patterns.

**Limitations**
1. Limited Dataset
    - Small dataset of ~1,200 records limits the usage of more powerful predictive models
    - Imbalanced classes where Class 2 (fatalities) is only 20% dataset - class weightage was used. 

2. Missing Factors and Data
A more comprehensive analysis can be done if the dataset can be cross-analysed with other data like:
    - Human activity metrics i.e. tourist numbers on Australia beaches, activity-specific participation.
    - Shark behavior data i.e. migratory patterns, breeding seasons. 
    - Environmental factors i.e. water temperature, weather. 


