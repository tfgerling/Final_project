# Depression, Anxiety and Stress Prediction

The dataset we analyzed and built models for is results from an online DASS test which scales levels of depression, anxiety and stress.

This data was collected with an on-line version of the Depression Anxiety Stress Scales (DASS), see http://www2.psy.unsw.edu.au/dass/

The survey was open to anyone and people were motivated to take it to get personalized results. At the end of the test they also were given the option to complete a short research survey. This datatset comes from those who agreed to complete the research survey and answered yes to the question "Have you given accurate answers and may they be used for research?" at the end.

This data was collected 2017 - 2019.

### Preprocessing

1. Nulls were handled - nulls existed for 2 columns, major and country. Major was updated to "No Degree" while country was updated to the country code that appeared the most often.
2. Over 4500 majors were combined into under 100. 38 rows had fewer than 10 with the same major so those were dropped from the dataframe.
3. Outlier data for age and family size was discovered via boxplots. The ages over 95 were replaced with the mean of the ages. The family size over 15 was also replaced with the mean of the family size.
4. The expected values in each column were inspected and updated. Zeroes were not valid for most of the columns so those were changed to more appropriate values.
5. Columns were dropped that didn't add value, such as elapsed time on each question and position of the question in the survey.
6. The dataframe was written to a csv file to be used in the ETL process.

### ETL

1. Both LabelEncoder and pd.factorize were used to create unique id's (primary keys) for each of the 6 tables.
2. The data was written to 6 separate csv files to ease in loading the tables.
3. psycopg2 was used to create the database, tables, and then to load the tables.

![image](https://github.com/tfgerling/Final_project/blob/main/Data_Model_Final_Project.svg)

### Training the Model

1. Principal Component Analysis (PCA) was applied to the remaining features in the dataset to address multicollinearity.  There was, of course, quite a bit of multicollinearity among the first 42 questions in the survey, but not so much for the temperament questions and the demographic information.
2. We chose to focus on the temperament and demographic information to see if a tendency for severe depression, anxiety, or stress can be discerned from these 'other' features on their own without factoring in the answers to the first 42 questions.
3. The stress outcomes were underrepresented in the data, so we used Synthetic Minority Oversampling Technique (SMOTE) to resample the data for the Stress model only.
4. Next, we used a combination of a Random Forest Classifier and a Support Vector Classifier to fit the model to the data.  The results of these two classifiers were then incorporated with a Voting Classifier to determine the greatest probability among the options (soft voting) and choose the optimal model to use.
5. Random Forest Classifier was chosen because of its effectiveness we have seen in past modeling endeavors, and a Support Vector Classifier was chosen after the visualizations prompted a discussion about finding hyperplanes among the multivariate dataset.
6.  The Voting Classifier is a new technique we came across in our preparation.  It seemed to be a perfect fit to solidify the ensemble of techniques we had chosen.

### Cross-Validation
1. After fitting the initial model with default parameters, we used GridSearchCV to assess the performance of each model individually while tuning parameters and performing cross-validation.  
2. GridSearch helped us in two ways:  a) to determine the best parameters to use for the final model to fit to the test data, and b) to use cross-validation to help us avoid overfitting the model.

### Parameter Tuning
1.  The best combination of parameters we found for the Random Forest Classifier was to keep the default setting for max_depth but change n_estimators to 500 trees.
2.  The best combination of parameters for the Support Vector Classifier was to use     .

### Prediction
1. The predictive ability of the initial model was at 81%, with precision and recall scores both falling between 81% and 82%.
2. After tuning parameters, the predictive ability of the model increased to 





