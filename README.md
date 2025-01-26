# Decoding User Demographics and Patterns Behind Divvy’s Subscriber Conversions



### Abstract
Sustainable transportation options are becoming increasingly vital in today's urban environments. Among these options, bike-sharing programs like Chicago's Divvy offer an affordable and eco-friendly means of navigating the city, attracting both residents and tourists. This project aims to understand the dynamics between casual riders and annual subscribers within the Divvy user base, specifically identifying the factors that influence the transition from occasional use to annual membership. By utilizing the extensive Divvy bike-sharing dataset, which includes over 7 million trip records, this analysis highlights key demographic and behavioural elements that drive subscription conversions. Additionally, the report explores how machine learning models can help identify casual riders with a high likelihood of converting to subscribers, as well as strategies for retaining potential churners.

Through rigorous data exploration and predictive modelling, time-related features, including trip duration and timing are discovered to be pivotal in determining user conversion. The findings suggest that targeted marketing strategies could effectively engage casual riders, particularly males, during peak usage periods, thereby encouraging subscription uptake. 

This project not only strengthens Divvy's marketing and operational strategies but also serves as a valuable framework for other bike-sharing services seeking to improve user loyalty and financial performance in a competitive landscape. Future work could delve deeper into user engagement strategies, particularly addressing the factors contributing to the lag in female users' utilization of bike-sharing as a transportation option compared to their male counterparts.


 ---
### Chapter One: Introduction
As urban centres expand and evolve, the demand for sustainable and efficient transportation is becoming increasingly essential. In response, bike-sharing programs such as Chicago’s Divvy have gained popularity, offering an affordable, eco-friendly solution for navigating busy cityscapes. Both residents and tourists use Divvy bikes to access different parts of the city, contributing to a cleaner urban environment while reducing traffic congestion. Within Divvy’s user base, two primary groups have emerged: casual riders, who use the service occasionally, and annual subscribers, who rely on it regularly. Identifying and understanding the factors that might encourage casual riders to become annual subscribers could play a significant role in enhancing Divvy’s financial performance and fostering long-term loyalty.



#### Existing Research and Background on Divvy
The Divvy dataset is one of the most accessible public datasets for studying bike-sharing usage, and it has been extensively explored by researchers and data enthusiasts. Divvy’s publicly available transaction data, which removes user IDs and includes fabricated birth year and gender data for privacy, provides a rich foundation for insights on bike-sharing behaviour.

Most studies based on bike-sharing datasets focus on understanding usage patterns, demand forecasting, and predictive modelling. For instance, a recent study published in Transport Policy (Hossain et al., 2024) suggests that bike-share operators should prioritize low-income individuals, car-free households, and students, as these groups are more likely to rely on bike-sharing due to limited transportation options. This study also finds that women are less likely to use bike-sharing services than men, raising questions about potential barriers to female ridership that extend beyond general bicycling trends.

While prior studies have contributed valuable insights on general usage trends and demand across various demographics, this project takes a more focused approach. Rather than simply examining usage patterns or offering broad recommendations, this analysis aims to identify specific demographic and behavioural indicators that suggest a casual rider might convert to an annual subscriber. By focusing on attributes like ride duration, trip frequency, and user age, the project aims to deliver actionable strategies that could drive more meaningful engagement with casual users who exhibit a high propensity for conversion.

Furthermore, predictive modelling techniques are applied to identify casual riders whose behaviour closely resembles that of existing annual subscribers. This allows Divvy’s marketing team to effectively target high-potential users with tailored advertisements or incentives, encouraging subscription. Additionally, the analysis can detect patterns among annual subscribers who may exhibit occasional or low engagement, which could signal a risk of churn. This dual approach supports both acquisition and retention strategies, distinguishing this project’s impact from prior research that primarily offers generalized insights. 
 

---
### Chapter Two: Background
Launched in June 2013, Chicago’s Divvy Bike Sharing System quickly became one of the largest and most successful bike-sharing programs in the United States. Operated by Lyft Bikes and Scooters, LLC (“Bikeshare”) and owned by the Chicago Department of Transportation, Divvy offers a convenient and accessible way to travel throughout Chicago, Illinois. By 2022, Divvy expanded its coverage to all 77 neighbourhoods in Chicago, facilitating 6.3 million bike and e-scooter trips and drawing nearly 45,000 members. With a network of over 6,000 bikes and more than 570 docking stations across the city (Chicago Union Station, n.d.), Divvy makes it easy for users to rent bikes, scooters, and e-bikes. The program offers two pricing structures: a casual plan with single-ride and day-pass options and an annual membership.

Despite its success, Divvy faces strong competition from established operators like Lime, Spin, and Superpedestrian, as well as local companies like Big City Bikes and 3rd Coast Cycles. To remain competitive, Divvy must continue to innovate and adapt to the market’s demands.

In conclusion, the Divvy Bicycle Sharing Program has integrated itself as part of Chicago's transportation infrastructure, providing a sustainable and flexible solution for last-mile urban mobility. While facing intense competition from both large industry players and local businesses, Divvy’s focus on converting casual riders to annual members will be key to its sustained growth. By understanding the customer behaviours that drive this conversion, Divvy can strengthen its competitive position and support its continued success in the dynamic landscape of urban transportation. 


---
### Chapter Three: Data
The data for this project (Serge, n.d.) was sourced from Kaggle and includes key features necessary for conducting an in-depth analysis. These insights are essential for understanding user behaviours and demographic factors that influence the conversion of casual riders to annual pass subscribers.

This dataset contains 7,421,086 records of trips taken in Chicago, Illinois, from 2018 to 2019. However, due to the limitation of computing power, only 365,069 rows of data from the first quarter of 2019 were considered. The dataset headers were renamed to simplify column names, as shown in Table 2. Table 1 provides a snapshot of the raw data, where each row includes transaction details such as rental start and end times, origin and destination stations, and user information, including membership status, gender, and birth year. A new column, "age," was created by subtracting users’ birth years from the present year.

![image](https://github.com/user-attachments/assets/b31dad2f-29ef-4fc7-8eda-511e47882e84)

                                                Table 1: Raw Data

A duplication test was performed, confirming that no duplicate rows were found. Conversely, a null value analysis revealed that approximately 5% of the 'gender' and 'birth year' attributes were missing, and these records were subsequently dropped. The analysis was not impacted by this missing data, as these were synthetic values.

An outlier analysis was conducted using the 'describe' function on two continuous variables, 'age' and 'trip duration', which revealed some extreme values. For example, approximately 0.05%, or 171 records, showed ages exceeding 99 years, with the maximum recorded age being 124. Additionally, about 0.1%, or 335 records, had rental durations of 43,201 seconds or more, equivalent to renting a bike for 12 hours straight. These outliers were removed to preserve the dataset's integrity. 


 ![image](https://github.com/user-attachments/assets/0fe9b658-b81b-4093-8008-173e6a32c5d1)

                                  Table 2a: Latest Dataset with All Variables Added (Part 1)
       (Features marked with asterisks represent newly created, feature-engineered variables derived from the original data.)
       
 
 ![image](https://github.com/user-attachments/assets/5dade86d-8c03-4f80-84ad-a4b6fdb817c7)

                                    Table 2b: Latest Dataset with All Variables Added (Part 2)
              (Features marked with asterisks represent newly created, feature-engineered variables derived from the original data.) 

              
As part of the data cleaning process, attributes were converted to appropriate data types. For instance, the "start_time" field was converted to a date-time format to enable precise analysis in Python. From this cleaned "start_time" column, the month was extracted and saved in a new column, "month". Each month was then categorized by season: Winter (December, January, February), Spring (March, April, May), Summer (June, July, August), and Fall (September, October, November). This classification was stored in a "season" column, providing seasonal context for each entry. Additionally, the hour was extracted from the "start_time" column, with 1 added to each value to avoid zeros, and stored in a new column called "houroftheday" to support interactive analyses.

Additional date-related transformations are applied to the dataframe to further enrich its temporal analysis. The year, month, and day are extracted from the “start_time” column, resulting in the creation of three new columns: year, month, and day. To provide insight into the distribution of trips throughout the week, the code calculates the day of the week for each entry, represented as a numeric value where “1” corresponds to Monday and “7” corresponds to Sunday. This numeric representation is stored in the “day_of_week_num” column. 

To enhance the dataset's analytical capabilities, various new features were derived from existing columns in the “combined_data” dataframe. First, the interaction between trip duration and birth year was calculated, resulting in the new column “tripduration_birthyear”, capturing generational trends in trip pattern. Next, another feature, “tripduration_hourofday”, was created by multiplying the trip duration with the corresponding hour of the day, offering a time-of-day perspective on trip length. These features reveal potential differences in usage and patterns based on age and time.

Furthermore, the season was encoded numerically, with Winter represented as 1 and Fall as 4, leading to the creation of the “age_season” column, which captures the interaction between a user's age and the season of the trip. This feature reflects how different age groups may prefer certain seasons due to factors like weather tolerance or seasonal activities.

The analysis also examined station dynamics by generating the “from_to_station_combination” column, which represents the ID of the departing station multiplied by the ID of the ending station. The interaction between starting and ending stations reflects spatial dynamics in user behaviour, identifying popular routes, commuter corridors, or recreational loops.

Additionally, the relationship between trip duration and the day of the week was explored, resulting in the “tripduration_dayofweek” column, where trip duration is divided by the numeric representation of the day of the week. This interaction allows to normalize trip durations across different days of the week, possibly uncovering variations in ride lengths between weekends and weekdays.

Lastly, the “hourofday_dayofweek” column was calculated by multiplying the hour of the day by the numeric representation of the day of the week, with the aim to identify peak travel times more precisely.

These new interactive features were added to facilitate deeper insights into user behaviour and trip characteristics, especially for logistic regression, given that random forests and decision trees handle feature matching effectively.


 ![image](https://github.com/user-attachments/assets/9d54a323-408f-4f10-841c-31e73630f7ba)

              Diagram 1: Age Distribution for Overall, Subscriber, and Casual User Groups Across Eight Quarters in 2018-2019


 ![image](https://github.com/user-attachments/assets/182119d6-5710-4368-8502-8c1b2d1abf7b)

                     Diagram 2: Age Distribution of Overall, Subscriber, and Casual User Groups in Q1 2019
                     

Diagrams 1 and 2 provide insights into age distributions for Overall, Subscriber, and Casual User groups across different periods. Diagram 1 represents data from eight quarters in 2018–2019, while Diagram 2 focuses on Q1 2019. Both diagrams show a similar right-skew, indicating a younger user base with a notable presence of older users. Diagram 1 highlights a wider age range in age distribution, with high concentrations around ages 25–35, suggesting a diverse age spread. In contrast, Diagram 2 reveals a younger profile for both subscribers and casual users, potentially due to an influx of younger users or lower participation by older users.

Across both diagrams, the age distribution for subscribers aligns closely with the overall user base, signalling strong commitment among subscribers. Conversely, the casual rider histogram reveals many younger individuals who have yet to convert to subscriptions, representing an opportunity for targeted marketing to encourage subscriptions among this valuable demographic.



![image](https://github.com/user-attachments/assets/81e3b452-1e2e-43f0-9c7f-7dc96e336e85)
 
        Diagram 3: Gender Distribution for Overall, Subscriber, and Casual User Groups Across Eight Quarters in 2018-2019



 ![image](https://github.com/user-attachments/assets/d860cff7-fe89-4303-8181-6c4b37882583)

                Diagram 4: Gender Distribution Across Overall, Subscriber, and Casual User Groups in Q1 2019
                

The gender distribution analysis across the two diagrams reveals some valuable insights. In Diagram 3, representing eight-quarter data from 2018 to 2019, males constitute the majority across all user categories, with 74.3% overall, 75.4% among subscribers, and 61.4% among casual users. Notably, the casual user group displays a more balanced gender distribution, with females making up 38.6%, indicating a higher female representation in casual usage compared to the subscription model.

In contrast, Diagram 4 shows an even more pronounced male dominance. Here, males account for 80.6% of the overall user base, 80.8% of subscribers, and 68.4% of casual users, marking a shift towards higher male representation across both subscriber and casual categories. This suggests that, compared to the eight-quarter average in Diagram 3, Diagram 4 reflects a decreased female presence across all user types, potentially due to seasonal or trend-based factors.

Overall, while male users are the dominant demographic in both diagrams, casual usage consistently shows a more balanced gender distribution than subscriptions, especially in the extended data of Diagram 3. This trend suggests that females may be more inclined toward casual usage rather than subscriptions. Understanding this could be valuable for developing engagement strategies, particularly in attracting and retaining more female subscribers.


![image](https://github.com/user-attachments/assets/aa92dd3f-0910-4ce6-ac93-0a0763a74ef3)
 
                  Diagram 5: Average Trip Duration by Age, Gender and UserType Across Eight Quarters in 2018-2019

![image](https://github.com/user-attachments/assets/14a8044f-fc7e-49ae-9989-9546ac81eb33)


 
                         Diagram 6: Average Trip Duration by Age, Gender and UserType for Q1 2019


                         
Diagrams 5 and 6 illustrate average trip duration by age, gender, and user type across different time frame. Diagram 5 spans eight quarters from 2018 to 2019, showing long-term trends, while Diagram 6 zooms in on Q1 2019 for a closer look at quarterly variations. Both diagrams reveal that casual riders tend to take longer trips than subscribers, with female customers, particularly in the 40–70 age range, showing notably high trip durations. In contrast, subscribers typically have shorter, more consistent trip durations across all age groups, averaging around 1,000 seconds (16 minutes).

These longer trip durations among casual riders likely reflect differences in trip purpose. Casual riders, often tourists or occasional users, may use bike-sharing primarily for leisure or recreational activities, unlike subscribers, who might rely on it for daily commuting or routine, shorter trips. This distinction in trip purpose explains why casual riders, especially female customers, consistently have longer average durations than subscribers.



---
### Chapter Four: Modelling and Evaluation
The preprocessing begins with the conversion of the “usertype” column into a binary format, where “Subscriber” is encoded as 1 and “Casual” as 0. This transformation allows for easier handling of user type in the classification model. Next, the categorical feature, “gender” is label-encoded using the LabelEncoder. This process transforms the gender column into a numerical format stored in “gender_encoded”.

Furthermore, to enhance the model's input features, a new feature, “trip_duration_bins” is created by binning the trip duration into three categories: “1” (0 to 1800 seconds), “2” (1801 to 3600 seconds), and “3” (3601 seconds to 43201 seconds). This new column will assist in understanding the relationship between trip duration and user behaviour. Overall, these preprocessing steps set the foundation for effective model training and analysis.

  ![image](https://github.com/user-attachments/assets/80d7049f-222f-47d7-a787-04b414949a29)
  
                                   Table 3: List of Variables Passed to Machine Learning Models
                                   
                          (The 'usertype' variable is excluded from the list, as it serves as the target variable)
                          
Subsequently, non-essential columns were removed from the “combined_data” dataframe to streamline the dataset for model training. The columns dropped include 'trip_id,' 'from_station_id,' 'to_station_id,' 'birthyear,' 'gender,' 'age,' 'start_time,' 'end_time,' 'from_station_name,' 'to_station_name,' 'season,' 'year,' 'day_of_week_num,' 'month,' 'tripduration_birthyear,' and 'tripduration.' Many of these variables, such as 'gender' and 'age,' were excluded because their information is already captured by the interactive features, including them would only introduce multicollinearity. In other words, the reduction in the number of columns focuses the dataset on the most relevant features for predicting the “usertype”, which is the target variable. 

Additionally, the remaining feature names are extracted into a list called “features”, excluding the “usertype” column. This preparation step is crucial for ensuring that the model is trained on a clean and concise set of features, improving both the efficiency of the training process and the interpretability of the model results.


![image](https://github.com/user-attachments/assets/ebc74c13-e564-437a-a698-0ef6a4b8d406)

                                                    Table 4: The Output of VIF Score

Furthermore, the Variance Inflation Factor (VIF) was utilized on the variables in the “features” list. According to Minitab (n.d.), VIF quantifies how much the variance of an estimated regression coefficient increases when the predictors are correlated. Based on the rule of thumb, features with a VIF value greater than 5 may indicate multicollinearity issues, suggesting that they could potentially be removed or combined to improve the model's reliability and interpretability. However, the variables with high VIF values exist due to interactive features with similar bases, such as “tripduration_hourofday”, “tripduration_dayofweek”, and “trip_duration_bins”, where there is overlap in the information they provide. This overlap can inflate VIF values, even if these features are genuinely useful for the model. Thus, these interactive features were retained to examine the relationships between trip duration and the hour of the day, as well as the relationship between trip duration and the day of the week. They were believed to be critical for the analysis, as different usage patterns might occur on different days of the week or at different times of day. In general, high VIFs among interaction terms or derived features may not always be a concern if they are specifically intended to capture those interactions in the model.


 ![image](https://github.com/user-attachments/assets/95d4af6a-8f73-4b30-a6d7-0bad885ef7b6)

                                          Diagram 7: Heatmap Results with 14 Feature Pairs 
                                          

Heatmap analysis was conducted to check the correlation coefficients of the variables in the 'features' list. The heatmap provides a visual overview of the relationships between all feature pairs, making it particularly useful for spotting clusters of correlated features that may contribute to multicollinearity. Additionally, it highlights correlations that may not be as severe as indicated by VIF but could still impact model interpretability and performance. From Diagram 7, ‘tripduration_hourofday’ and ‘tripduration_birthyear’ were observed to have a correlation coefficient of 0.92. As a result, ‘tripduration_birthyear’ was removed to ensure the model's reliability and to mitigate any potential multicollinearity issues, allowing for clearer interpretation of the remaining variables.

The heatmap score for variables “from_station_encoded” and “to_station_encoded” is 0.72, showing a strong positive correlation, possibly indicating that certain start and end stations are often paired together.

The data is then split into training and testing sets, where X represents the “feature” columns and y is the target variable “usertype” (with ‘0’ ('casual') or ‘1’('subscriber')). A 70-30 split is performed using the “train_test_split” function, ensuring that the training set is used to fit the model while the testing set remains unseen. To address class imbalance in the dataset, the Synthetic Minority Over-sampling Technique (SMOTE) is applied solely to the training data, generating synthetic samples of the minority class to balance the class distribution.

![image](https://github.com/user-attachments/assets/413a8cb2-883c-4a19-90df-c4b212d033cd)

                             Table 5: The Output of the Top 10 Most Important Features from RFE
                             
When initializing the models, the “class_weight” parameter is set to “balanced” for both the Decision Tree classifier and the Random Forest model. This setting automatically adjusts the weights inversely proportional to class frequencies, while the “random_state” is set to 42 to ensure reproducibility.

Recursive Feature Elimination (RFE), a systematic approach to feature selection, is applied to three classifiers: Random Forest, Logistic Regression, and Decision Tree. RFE is used to iteratively select the top 10 features based on each model's performance, providing a nuanced understanding of feature importance. As shown in Table 5, it is unsurprising that the features selected by the Random Forest and Decision Tree models are similar, given their shared tree-based structure. Some features, like “season_encoded,” “gender_encoded,” and “trip_duration_bins,” are only selected by Logistic Regression, indicating that these features are more relevant to Logistic Regression or that they don’t contribute as strongly to the other models.

There are consistent features selected across models, such as “houroftheday,” “day,” “tripduration_dayofweek,” “hourofday_dayofweek,” “from_station_encoded,” and “to_station_encoded,” suggesting their importance across different types of models.

Following that, the three models were retrained using only the features identified through Recursive Feature Elimination (RFE) and fit to the resampled training data. Predictions were then generated on the test dataset, and model performance was evaluated through classification reports, detailing metrics such as precision, recall, F1-score, and overall accuracy.


![image](https://github.com/user-attachments/assets/6942d7c2-414b-4e9d-ac0e-28b2eb627582)

                                       Table 6: Classification Report for Random Forest


![image](https://github.com/user-attachments/assets/b9b82205-17d2-4597-b3e6-83c637272c9e)

                                      Table 7: Classification Report for Logistic Regression

![image](https://github.com/user-attachments/assets/2b193c7f-d372-4f9d-b473-83d2f6437da1)

                                        Table 8: Classification Report for Decision Tree
                                        

The classification reports reveal that the Random Forest Classifier emerging as the top performer in terms of overall accuracy and handling of the majority class (class 1), followed by Decision Tree and Logistic Regression. With a high overall accuracy of 97.5%, Random Forest demonstrates strong precision, recall, and F1-scores for class 1. However, it struggles with the minority class (class 0), showing relatively low precision (0.31) and recall (0.35). This discrepancy is reflected in the macro average metrics, which are notably lower than the weighted averages due to poor performance on class 0.

The Logistic Regression model, with an overall accuracy of 75.3%, performs significantly worse than the Random Forest, particularly with class 0, where it has a very low precision of 0.04, despite achieving a recall of 0.62. This high recall indicates that while the model captures most positive instances for class 0, it misclassifies many false positives, likely due to the imbalance favouring class 1. The overall accuracy for Logistic Regression is skewed by the performance of minority class, resulting in its poor score.

The Decision Tree Classifier, achieving a 94.1% accuracy, performs better than Logistic Regression but worse than the Random Forest. While it handles class 1 relatively well, the model’s precision for class 0 is low (0.11), although its recall (0.36) is higher than that of Logistic Regression. The macro average metrics indicate that the Decision Tree’s performance on the minority class is somewhat improved compared to Logistic Regression, but it still does not match the Random Forest’s overall balance between recall and precision.

In short, the Random Forest model performs best overall, especially for the majority class, although all three models struggle with the heavily imbalanced dataset, particularly in identifying the minority class. 


 ![image](https://github.com/user-attachments/assets/06a57f02-414b-48bc-99ef-4da25df87ce4)

Table 9: The Importance Score for Selected Features from Random Forest


 ![image](https://github.com/user-attachments/assets/2b36f23a-82ec-476f-9cb4-ae2c1f77cf61)

Table 10: The Importance Score for Selected Features from Decision Tree


 ![image](https://github.com/user-attachments/assets/aa29d682-5f1a-4900-b699-9256c9d52727)

Table 11: The Standardized Importance Score for Selected Features from Logistic Regression

The importance scores of the top 10 features selected by each model are displayed in Tables 9, 10, and 11. Notably, time-based features such as “tripduration_hourofday,” “hourofday_dayofweek,” and “houroftheday” emerged as the most influential factors in predicting whether a rider is a casual user or an annual subscriber in both the Random Forest and Decision Tree models. This finding suggests that trip timing and duration are central to distinguishing user types, with casual riders likely displaying different patterns than subscribers. Other features, including “from_to_station_combination” and “age_season,” demonstrated moderate importance, indicating that station combinations and seasonal age variations also play some role in shaping user behaviour. In contrast, station-related features (“from_station_encoded” and “to_station_encoded”) and “bikeid” ranked lower in importance, suggesting they have less impact on the prediction. Overall, both models consistently emphasize that when and how long a trip occurs are key differentiators, highlighting time-related patterns as primary indicators of user type.

Similarly, the logistic regression model’s standardized coefficients reinforce the significance of time-related features in predicting user type. The strongest negative coefficients, “tripduration_dayofweek” (-1.07) and “hourofday_dayofweek” (-0.73), indicate that longer trip durations on specific days and off-peak hours are strongly associated with casual riders, likely reflecting leisure-oriented usage. Conversely, positive coefficients for “gender_encoded” (0.53), “houroftheday” (0.31), and “age_season” (0.30) suggest that males, specific hours, and age-season combinations are more associated with annual subscribers. Specifically, the positive coefficient of 0.53 for gender_encoded indicates that males (represented by 1) are more likely to be annual subscribers compared to females, suggesting a tendency for male riders to convert more often to annual subscriptions. Lower-impact features, such as “trip_duration_bins,” “day,” “to_station_encoded,” and “from_station_encoded,” contribute less to predicting user type, though they still offer minor insights. Overall, these results emphasize that trip duration and timing are primary indicators, with demographic factors adding some predictive value in distinguishing between casual and annual riders.





 ![image](https://github.com/user-attachments/assets/085014ae-7d59-4a83-989c-b3833a8da9dd)

                           Table 12: Comparison of the 5-Fold CV Accuracies with Overall Accuracy

Cross-validation, specifically a 5-fold strategy, is employed to assess the models' robustness and generalization capabilities. This method not only evaluates performance but also ensures the results are reliable and not overly dependent on a specific train/test split.

From Table 12, the Random Forest model stands out with an overall accuracy of 0.9754 and a mean 5-fold cross-validation accuracy of 0.9869, indicating strong generalization across different data subsets. In contrast, the Logistic Regression model shows a significantly lower overall accuracy of 0.7532 and a mean CV accuracy of 0.7032, suggesting it struggles to effectively distinguish between classes, particularly in the context of class imbalance. The Decision Tree model performs decently with an overall accuracy of 0.9413 and a mean CV accuracy of 0.9521, but it still lags behind the Random Forest’s performance.

The poorer performance of Logistic Regression indicates it may not capture the complexities of the data, potentially due to its assumption of a linear relationship between features and the target variable. If the underlying relationships are non-linear or involve complex interactions, this model may struggle. Additionally, the selected features might not provide sufficient information for Logistic Regression compared to other classifiers. While often favoured for its interpretability and simplicity, its lower accuracy suggests it may be less suitable for this dataset.

 ![image](https://github.com/user-attachments/assets/8c209d6e-403a-43b9-8d32-a5ba28016d30)

                                           Table 13: Outcome of Hyperparameter Tuning

Hyperparameter tuning was conducted for each model to identify the most suitable parameters and enhance model performance. For the Random Forest model, a total of 15 fits were executed over 3 folds for 5 candidate configurations. The optimal hyperparameters identified were 'n_estimators': 500, 'min_samples_split': 2, 'min_samples_leaf': 1, and 'max_depth': None. This optimized model demonstrated robust performance, achieving a marginal improvement of 0.0004% in accuracy.

Similarly, the Logistic Regression model underwent tuning, resulting in optimal parameters of 'solver': 'lbfgs' and 'C': 100. The model's accuracy remained stable at 0.7532, indicating that the initial selected parameters provide the best achievable performance for this configuration.

For the Decision Tree model, the optimal hyperparameters were found to be 'min_samples_split': 2, 'min_samples_leaf': 5, and 'max_depth': None, resulting in an accuracy of 0.9324, which represents a slight decrease from the previous accuracy of 0.9413.

In conclusion, the hyperparameter tuning process demonstrated that the Random Forest classifier achieved the best performance among the models tested. With carefully optimized parameters, it offered the most favourable balance between accuracy and precision, outperforming both the Logistic Regression and Decision Tree models in classification effectiveness.


![image](https://github.com/user-attachments/assets/b805677c-acc1-48d3-8dda-1a2aadbe1343)

Table 14: A Total of 859 Transaction Records for Casual Riders were Identified as Having a 70% Probability of Converting to Subscribers


 ![image](https://github.com/user-attachments/assets/fefe637b-b26f-4ca7-a860-f4c680048f6a)

          Table 15: A Total of 167 Transaction Records for Subscriber were Identified as Having an 80% Probability of Churning

The trained Random Forest model was applied to predict the probability of casual riders converting to subscribers within the dataset. Using the “predict_proba” method, the model estimated each user’s likelihood of being a subscriber or casual rider, enabling a focused analysis of potential conversions and churn.

First, probability predictions were generated to assess the likelihood of casual riders converting to subscribers. A threshold of 0.7 was set, filtering for casual riders with a 70% or higher probability of subscribing. This subset of potential subscribers was highlighted, providing an opportunity for the marketing team to collaborate with cross-functional teams to develop targeted advertisements encouraging conversion.

Similarly, predictions were made to identify subscribers at high risk of churning to casual ridership. These probabilities were recorded, and a threshold of 0.8 was set to filter out the subscribers with an 80% or higher probability of churning. This high-risk group was filtered for retention-focused efforts. The resulting subsets of high-probability churners and potential subscribers offer a data-driven foundation for designing targeted marketing and retention strategies, strengthening user engagement and loyalty.



---
### Chapter Five: Recommendations/Conclusion                                                        
The analysis and modelling efforts in this study provide actionable insights that can drive Divvy’s marketing and operational strategies. By identifying key factors influencing the likelihood of converting casual riders into annual subscribers, Divvy can strengthen its competitive advantage in Chicago’s bike-sharing market, particularly as urbanization and demand for sustainable transport continue to grow.

The insights emphasize the significance of time-based features, such as trip duration and trip timing, suggesting that targeted marketing campaigns could be strategically timed for peak casual ridership periods to highlight the advantages of annual subscriptions. For instance, casual riders who frequently ride during specific hours or days could be enticed by highlighting the cost savings and convenience of an annual pass during these high-use periods. Additionally, demographic-specific incentives, such as promotions targeting male riders who show a higher conversion tendency, can improve campaign efficacy by focusing on demographics more inclined to subscribe.

The predictive models developed, particularly the Random Forest classifier, allow Divvy to segment and prioritize casual riders with a high probability of conversion. This allows Divvy’s marketing team to allocate resources effectively, focusing on high-potential casual riders for conversion campaigns, while also identifying subscribers at risk of churning. By proactively engaging these high-risk subscribers with retention strategies, Divvy can reduce churn rates, thereby maintaining and potentially growing its subscriber base.
From an industry perspective, the findings suggest that bike-sharing programs like Divvy can significantly enhance their profitability by focusing on rider conversion and retention through predictive analytics. The study’s methodology, which combines demographic and behavioural segmentation with targeted predictive modelling, provides a framework that other bike-sharing systems could adopt to improve user retention and loyalty.

#### Limitations and Potential Areas for Future Work
This study provides valuable insights into factors affecting the conversion of casual riders to annual subscribers, but several limitations suggest areas for improvement in future research. For instance, the dataset includes only basic demographic information, primarily age and gender, restricting the model’s ability to account for socioeconomic factors that may influence subscription decisions. Additional demographic variables, such as income level, education, and occupation, could provide deeper insights into user preferences, with surveys or external data sources enabling the creation of a more detailed user profile. Furthermore, although the dataset includes start and end station information, it lacks broader geospatial data like the proximity to public transit or the surrounding residential density. Integrating such data could reveal underlying factors influencing casual riding habits, particularly when comparing residents to tourists.



#### Potential Data Sources for Future Analysis
To enrich the analysis, additional data sources could be integrated. For instance, public transit data, including bus and subway schedules, could show how Divvy’s services interact with other transportation modes, potentially revealing opportunities for partnerships or service integration. Tourism and event data would also be beneficial, as casual ridership often rises during local festivals and tourist seasons, offering Divvy a chance to promote short-term subscription options.

In conclusion, while this study delivers substantial insights into user conversion and retention, future research integrating additional demographic, geospatial, and qualitative data could provide a more comprehensive understanding of rider motivations and behaviours. By exploring these avenues, Divvy could refine its strategies to enhance user engagement and profitability, contributing to the broader adoption of sustainable urban transport solutions.

---
### Reference
Minitab (n.d.) Multicollinearity in regression.
https://support.minitab.com/en-us/minitab/help-and-how-to/statistical-modeling/regression/supporting-topics/model-assumptions/multicollinearity-in-regression/#:~:text=The%20VIFs%20measure%20how%20much,1%2C%20the%20predictors%20are%20correlated.

Science Direct. (January 2024). Examining market segmentation to increase bike-share use and enhance equity: The case of the greater Sacramento region.
https://www.sciencedirect.com/science/article/pii/S0967070X23002950#sec5

Serge G. (n.d.). How Does a Bike-Share Navigate Speedy Success? https://www.kaggle.com/datasets/sergegeukjian/how-does-a-bikeshare-navigate-speedy-success




