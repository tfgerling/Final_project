# Depression, Anxiety and Stress Prediction

The dataset we analyzed and built models for is results from an online DASS test which scales levels of depression, anxiety and stress.

This data was collected with an on-line version of the Depression Anxiety Stress Scales (DASS), see http://www2.psy.unsw.edu.au/dass/

The survey was open to anyone and people were motivated to take it to get personalized results. At the end of the test they also were given the option to complete a short research survey. This datatset comes from those who agreed to complete the research survey and answered yes to the question "Have you given accurate answers and may they be used for research?" at the end.

This data was collected 2017 - 2019.

## Preprocessing

1. Nulls were handled - nulls existed for 2 columns, major and country. Major was updated to "No Degree" while country was updated to the country code that appeared the most often.
2. Over 4500 majors were combined into under 100. 38 rows had fewer than 10 with the same major so those were dropped from the dataframe.
3. Outlier data for age and family size was discovered via boxplots. The ages over 95 were replaced with the mean of the ages. The family size over 15 was also replaced with the mean of the family size.
4. The expected values in each column were inspected and updated. Zeroes were not valid for most of the columns so those were changed to more appropriate values.
5. Columns were dropped that didn't add value, such as elapsed time on each question and position of the question in the survey.
6. The dataframe was written to a csv file to be used in the ETL process.

## ETL

1. Both LabelEncoder and pd.factorize were used to create unique id's (primary keys) for each of the 4 tables.
2. The data was written to 4 separate csv files to ease in loading the tables.
3. psycopg2 was used to create the database, tables, and then to load the tables.

![image](https://github.com/tfgerling/Final_project/blob/main/Data_Model_Final_Project.svg)

## Models
